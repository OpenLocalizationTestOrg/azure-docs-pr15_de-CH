<properties
   pageTitle="Azure Architektur Referenz - IaaS: Erstellen einer Active Directory-Ressourcengesamtstruktur in Azure | Microsoft Azure"
   description="Erstellen eine vertrauenswürdige Active Directory-Domäne in Azure"
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt, wie Active Directory-Domäne in Azure erstellen, trennen, aber von vertrauenswürdigen Domänen in der lokalen Gesamtstruktur.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Diese Referenzarchitektur verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Eine Organisation, die lokale Active Directory (AD) führt möglicherweise eine Gesamtstruktur mit zahlreichen Domänen. Beispielsweise können Sie einzelne Domänen für verschiedene Kostenstellen oder Unterorganisationen erstellen oder neue Domänen können durch Erwerb oder Verschmelzung anderer Organisationen hinzugefügt wurden. Können Sie Domänen zwischen Funktionsbereiche isolieren, die möglicherweise aus Sicherheitsgründen getrennt werden müssen, aber Informationen zwischen Domänen durch Einrichten von Vertrauensstellungen teilen.

Eine Organisation, die separate Domänen nutzt profitieren Azure durch eine oder mehrere dieser Domänen in die Cloud verschieben. Alternativ möchte eine Organisation alle Cloudressourcen logisch abgetrennter diese statt lokal halten und speichern Informationen über Cloud-Ressourcen im eigenen Verzeichnis in einer Domäne auch in der Cloud gespeichert.

Sie können Active Directory in Azure in implementieren verschiedene Arten gemäß den Artikeln [Erweitern von Active Directory in Azure] [ extending-ad-to-azure] und [Implementieren von Azure Active Directory][implementing-aad]. Dieses Dokument konzentriert sich auf ein bestimmtes Szenario: Erstellen einer Domäne in der Cloud von Domänen lokal gehalten wird, aber deren lokalen Domänen eine Vertrauensstellung. 

Normale Anwendungsfälle für diese Architektur gehören:

- Verwalten von Sicherheit Trennung für Objekte und Identitäten in der Cloud gespeichert.

- Migrieren von einzelnen Domänen lokal in der Cloud.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur (*zum Vergrößern klicken*). Weitere Informationen über die Elemente abgeblendet finden Sie unter [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure] [ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Lokalen Netzwerk.** Das lokale Netzwerk enthält einen eigenen AD-Gesamtstruktur und Domänen.

- **AD-Server.** Diese sind Domänencontroller Directory Services (AD DS) als VMs in der Cloud zu implementieren. Dieser Server einer Gesamtstruktur mit einer oder mehreren Domänen, getrennt von den sich lokal.

- **Unidirektionale Vertrauensstellung.** Im Diagramm Beispiel eine unidirektionale Vertrauensstellung aus der Domäne in der Cloud mit der lokalen Domäne. Diese Beziehung ermöglicht lokalen Zugriff auf Ressourcen in der Domäne in der Cloud, aber nicht umgekehrt. Dies ist häufig. Jedoch können Sie eine bidirektionale Vertrauensstellung erstellen, wenn Cloud Benutzer auch Zugriff auf lokale Ressourcen benötigen.

- **Active Directory-Subnetz.** Die AD DS-Server befinden sich in einem separaten Subnetz. NSG Regeln schützen die AD DS-Server und eine Firewall vor Datenverkehr von unerwarteten Quellen erhalten.

- **Web-Tier-Subnetz** **Business Tier Subnetz**und **Datenebene Subnetz**. Diese Subnets host Server und Komponenten, die Anwendung in der Cloud ausgeführt. Weitere Informationen finden Sie unter [VMs für N-Tier-Architektur in Azure ausgeführt][running-VMs-for-an-N-tier-architecture-on-Azure]. Ressourcen und VMs in diesem Subnetz sind in der Cloud-Domäne enthalten.

- **Azure-Gateways**. Azure-Gateway stellt eine Verbindung zwischen dem lokalen Netzwerk und Azure-VNet. Dies ist eine [VPN-Verbindung] [ azure-vpn-gateway] oder [Azure ExpressRoute][azure-expressroute]. Weitere Informationen finden Sie unter [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Empfehlung

Dieser Abschnitt enthält eine Liste der Grundlage die wesentlichen Komponenten erforderlich, die grundlegende Architektur implementiert. Diese Empfehlung behandelt:

- Fügt Einstellungen und

- Vertrauen Sie Beziehung Konfiguration.

Sie müssen möglicherweise zusätzliche oder abweichende Vorschriften von den hier beschriebenen. Die Elemente können in diesem Abschnitt als Ausgangspunkt für wie die Architektur für das eigene System anpassen.

### <a name="adds-recommendations"></a>Fügt recommendations

Spezifische Vorschläge zur Implementierung von Active Directory in der Cloud finden Sie im Dokument [Active Directory erweitern, um Azure][extending-ad-to-azure]. [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines] Artikel[ ad-azure-guidelines] enthält weitere Informationen.

### <a name="trust-recommendations"></a>Empfohlene vertrauen

Die lokalen Domänen befinden sich in einer anderen Gesamtstruktur Domänen in der Cloud. Um die Authentifizierung von lokalen Benutzern in der Cloud müssen die Domänen in der Cloud Anmeldedomäne in der lokalen Gesamtstruktur vertrauen. Ebenso stellt die Cloud eine Anmeldedomäne für externe Benutzer, kann für die lokale Gesamtstruktur die Cloud Domäne erforderlich.

Einrichten von Vertrauensstellungen auf Gesamtstrukturebene [Erstellen von Gesamtstrukturvertrauensstellungen][creating-forest-trusts], oder durch [externe Vertrauensstellungen]auf Domänenebene[creating-external-trusts]. Eine Vertrauensstellung auf Gesamtstrukturebene erstellt eine Beziehung zwischen allen Domänen in beiden Gesamtstrukturen. Eine externe Ebene Vertrauensstellung erstellt nur eine Beziehung zwischen zwei angegebenen Domänen. Erstellen Sie nur externe Ebene von Vertrauensstellungen zwischen Domänen in verschiedenen Gesamtstrukturen.

Vertrauensstellungen können unidirektional sein (unidirektional) oder bidirektionale (bidirektional):

- Eine unidirektionale Vertrauensstellung kann Benutzer in einer Domäne oder Gesamtstruktur (bekannt als *eingehende* Domäne oder Gesamtstruktur) auf Ressourcen in einer anderen ( *ausgehenden* Domäne oder Gesamtstruktur). 

- Eine bidirektionale Vertrauensstellung kann Benutzer in der Domäne oder Gesamtstruktur Zugriff auf Ressourcen in einer anderen.

In der folgenden Tabelle zusammengefasst vertrauen Konfigurationen für einige einfache Szenarios:

| Szenario | Lokale vertrauen | Cloud-vertrauen |
|----------|-------------------|-------------|
| Lokalen Benutzer benötigen Zugriff auf Ressourcen in der Cloud, aber nicht umgekehrt. | Unidirektionale, eingehende | Unidirektionale, ausgehende |
| Benutzer in der Cloud benötigen Zugriff auf lokale Ressourcen, aber nicht umgekehrt | Unidirektionale, ausgehende | Unidirektionale, eingehende |
| Benutzer in der Cloud und lokalen benötigt Zugriff auf Ressourcen in der Cloud und lokal | Bidirektionale, eingehende und ausgehende | Bidirektionale, eingehende und ausgehende |

## <a name="security-considerations"></a>Sicherheitsaspekte

Ebene Gesamtstruktur-Vertrauensstellungen sind transitiv. Einrichten eine Vertrauensstellung auf Gesamtstrukturebene zwischen einer lokalen und einer Gesamtstruktur in der Cloud wird dieses Vertrauen in andere neuen Domänen in einer Gesamtstruktur erstellte erweitert. Wenn Sie Domänen aus Sicherheitsgründen Trennung zu verwenden, sollten Sie auf der Domänenebene erstellen. Auf Vertrauensstellungen sind nicht transitiv.

AD-spezifische Sicherheitsaspekte finden Sie im Abschnitt *Security Considerations* [Erweitert Active Directory Azure][extending-ad-to-azure].

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Mindestens zwei Domänencontroller für jede Domäne zu implementieren. Dies ermöglicht die automatische Replikation zwischen Servern. Erstellen Sie eine Verfügbarkeit für VMs als AD Server jeder Domäne festgelegt. Stellen Sie sicher, dass mindestens zwei Server in der Gruppe. 

Außerdem sollten Sie einen oder mehrere Server in jeder Domäne als [Standby-Betriebsmaster] festlegen[ standby-operations-masters] bei Verbindung zu einem Server als FSMO-Funktion fehlschlägt.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

AD ist automatisch skalierbar für Domänencontroller, die zu derselben Domäne gehören. Anfragen werden auf allen Domänencontrollern in einer Domäne verteilt. Sie können einen anderen Domänencontroller und synchronisiert automatisch mit der Domäne. Konfigurieren Sie ein separates Lastenausgleich direkter Datenverkehr auf Domänencontroller in der Domäne. Sicherstellen Sie, dass alle Domänencontroller genügend Speicher und Ressourcen die Domänendatenbank behandeln. Stellen Sie alle Domänencontroller VMs dieselbe Größe.

## <a name="management-and-monitoring-considerations"></a>Management und Überwachung Aspekte

Informationen zur Verwaltung und Überwachung Aspekte finden Sie die entsprechende Abschnitte [Erweitern von Active Directory in Azure][extending-ad-to-azure]. 

Weitere Informationen finden Sie unter [Monitoring Active Directory][monitoring_ad]. Sie können Tools wie [Microsoft Systems Center] [ microsoft_systems_center] auf monitoring Server im Subnetz Management können Sie diese Aufgaben ausführen. 

## <a name="solution-components"></a>Lösungskomponenten

Eine Lösung Beispielskript [Bereitstellen ReferenceArchitecture.ps1][solution-script], steht zur Verfügung, mit die Architektur implementiert, die Vorschläge in diesem Artikel beschriebenen folgt. Dieses Skript nutzt Azure Resource Manager Vorlagen. Die Vorlagen werden als eine Reihe von grundlegender Bausteine, von die jede eine bestimmte Aktion wie ein VNet erstellen oder Konfigurieren einer NSG führt. Das Skript soll Bereitstellung koordinieren.

Die Vorlagen werden mit den Parametern, die als separate JSON-Dateien parametrisiert. Sie können die Parameter in diesen Dateien die Bereitstellung Ihren Bedürfnissen konfigurieren. Sie müssen nicht die Vorlagen selbst zu ändern. Sie müssen die Schemas der Objekte in Parameterdateien nicht ändern.

Wenn Sie die Vorlagen bearbeiten, erstellen Sie Objekte, die [Namenskonventionen für Azure Ressourcen sollten]gemäß Namenskonventionen[naming-conventions].

Die Beispielprojektmappe erstellt und konfiguriert eine Umgebung in der Cloud, die eine Domäne namens *treyresearch.com*implementiert. Diese Umgebung umfasst ADDS Subnetz und Servern, DMZ Webebene, Geschäftsebene und Datenzugriffskomponenten Tier VPN-Gateway und Verwaltungsebene. Die Beispielprojektmappe enthält auch eine optionale Konfiguration zum Erstellen einer lokalen simulierten Umgebung mit seiner eigenen Domäne *contoso.com*. Die Lösung umfasst Skripts, die in diesen Domänen, die lokalen Zugriff auf Objekte in der Domäne *treyresearch.com* in die Cloud ermöglicht eine Vertrauensstellung hergestellt.

Die folgenden Abschnitte beschreiben die Elemente der lokalen und cloud-Konfigurationen.

### <a name="on-premises-components"></a>Lokalen Komponenten

>[AZURE.NOTE] Diese Komponenten stehen nicht in diesem Dokument beschriebenen Architektur und dienen lediglich die Cloud-Umgebung testen, anstatt einer echten produktiven Umgebung geben. Aus diesem Grund werden in diesem Abschnitt nur die Schlüsselparameter Dateien zusammengefasst. Wie die IP-Adressen oder die Größe der VMs können es jedoch viele weitere Parameter unverändert lassen.

Diese Umgebung besteht aus einer AD-Gesamtstruktur der Domäne *contoso.com* . Die Domäne enthält zwei fügt Server mit IP-Adressen 192.168.0.4 und 192.168.0.5. Diese zwei Server auch den DNS-Dienst. Das lokale Administratorkonto auf beiden virtuellen Computern heißt `testuser` mit Kennwort `AweS0me@PW`. Darüber hinaus richtet die Konfiguration ein VPN-Gateway zur Verbindung mit den VNet in der Cloud. Ändern Sie die Konfiguration bearbeiten die folgenden JSON Dateien im [**Parameter-Onpremise**] [ on-premises-folder] Ordner:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Diese Datei definiert den Netzwerkadressbereich lokalen Umgebung.

- ** [VirtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Diese Datei enthält die Konfiguration der lokalen VMs fügt Hostingdienste. Standardmäßig werden zwei *Standard-DS3-v2* -VMs erstellt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** und ** [connection.parameters.json][on-premises-connection-parameters]**. Diese Dateien enthalten die Einstellungen für die VPN-Verbindung Azure VPN-Gateway in der Cloud, einschließlich der freigegebenen Schlüssel zum Schutz des Datenverkehrs durch das Gateway.

Die verbleibenden Dateien im Ordner enthalten die Konfigurationsinformationen zum Erstellen der lokalen Domäne (*contoso.com*) dieser Infrastruktur. Sie fügt Setup DNS installieren verwenden und die lokalen Gesamtstruktur erstellen.

Die Lösung verwendet außerdem das folgende Skript namens [eingehenden trust.ps1][incoming-trust], das auf einem Computer in der lokalen Domäne ausgeführt wird:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dieses Skript fügt die IP-Adressen von AD DS-Servern in der Cloud (siehe nächsten Abschnitt) an den lokalen DNS-Dienst verwendet [Netdom] [ netdom] Befehl, um eine unidirektionale eingehende Vertrauensstellung aus der Domäne in der Cloud (*treyresearch.com*) erstellen.

### <a name="cloud-components"></a>Cloud-Komponenten

Diese Komponenten bilden den Kern dieser Architektur. Sie die Infrastruktur für die *treyresearch.com* -Domäne einrichten und Vertrauensstellungen mit *der lokalen Domäne* erstellen. [**Parameter-Azure**] [ azure-folder] Ordner enthält die folgenden Parameterdateien für diese Komponenten konfigurieren:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Diese Datei definiert die Struktur der VNet für die VMs und andere Komponenten in der Cloud. Es enthält Eigenschaften wie Name, Adressraum, Subnetze und die Adressen der DNS-Server erforderlich. Die DNS-Adressen in dieser Referenz wird die IP-Adressen des lokalen DNS-Server und Azure DNS-Standardserver. Ändern Sie diese Adressen Ihrer eigenen DNS-Konfiguration verweisen, wenn Sie Beispiel lokalen Umgebung nicht verwenden:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [VirtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Diese Datei konfiguriert die VMs fügt in der Cloud ausgeführt. Die Konfiguration umfasst zwei VMs. Ändern Sie den Benutzernamen Administrator und das Kennwort in der `virtualMachineSettings` Abschnitt, und Sie können optional die VM Größe an Bereich anpassen:

    Weitere Informationen finden Sie unter [Erweitern von Active Directory in Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [hinzufügen-Fügt-Domain-controller.parameters.json][add-adds-domain-controller-parameters]**. Diese Datei enthält die Einstellungen für das Erstellen von Server fügt *treyresearch.com* -Domäne. Es verwendet benutzerdefinierte Erweiterung, die die Domäne eingerichtet und fügt Server hinzufügen. Sofern weitere fügt Server erstellen (in diesem Fall sie hinzufügen sollten die `vms` Array), ihren Namen von der Standardeinstellung ändern und eine Domäne unter einem anderen Namen erstellen Sie diese Datei ändern müssen möchten.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Diese Datei enthält die zum Erstellen von Azure VPN-Gateway in der Cloud zum lokalen Netzwerk herstellen. Sollten Sie ändern die `sharedKey` Wert der `connectionsSettings` Abschnitt mit lokalen VPN-Gerät übereinstimmt. Weitere Informationen finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** und ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Diese Dateien Konfigurieren eingehender (öffentlich) und ausgehenden (privat) VMs, die DMZ Schutz von Servern in der Cloud umfassen. Weitere Informationen über diese Elemente und deren Konfiguration finden Sie unter [Implementieren einer DMZ zwischen Azure und dem Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [LoadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [LoadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, und ** [LoadBalancer data.parameters.json][loadBalancer-data-parameters]**. Diese Parameter-Dateien enthalten die VM-Spezifikationen für Ebenen Access Web, Geschäfts- und und Lastenausgleich für jede Ebene konfigurieren. Dies sind die VMs, die Web-apps und Datenbanken hosten und die Arbeitslasten in Ihrem Unternehmen für die Organisation ausführen. VMs in der Webebene der Domäne in der Cloud mithilfe dieser Einstellung hinzugefügt werden die ** [von Vm-Domäne join.parameters.json] [ web-vm-domain-join-parameters] ** Datei.

    Jede Datei enthält zwei Gruppen von Konfigurationsparameter. Die `virtualMachineSettings` Abschnitt definiert die VMs, die ADFS-Dienst in der Cloud zu hosten. Standardmäßig erstellt das Skript zwei VMs in der gleichen Verfügbarkeit. Die folgenden Fragmente zeigen die relevanten Teile der Datei *LoadBalancer web.parameters.json* :

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Sie können Größe und Anzahl der VMs pro Ebene entsprechend ändern.

    Die `loadBalancerSettings` Abschnitt beschreibt den Lastenausgleich für diese VMs. Lastenausgleich übergibt wird Datenverkehr auf Port 80 (HTTP) und 443 (HTTPS) oder andere VMs. 

    >[AZURE.NOTE] Die Regel für Port 80 verwendet eine TCP-Verbindung anstelle von HTTP. Ist die Installation von IIS auf der Webebene unterstützt nur Windows-Authentifizierung konfiguriert ist. Anonyme Authentifizierung ist deaktiviert. *Ping* Versuch schlägt fehl Port 80 über eine HTTP-Verbindung eine 401 (nicht autorisiert) Fehler während einer TCP-Verbindung nur mit erkennt, ob der Anschluss aktiv ist:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [VirtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Diese Datei enthält die Konfiguration für Sprung Feld/Verwaltung virtueller Computer. Es kann nur zu Anmeldung und Administratorzugriff auf die VMs im Web, Geschäfts- und Datenebenen aus springen. Standardmäßig erstellt das Skript einen einzelnen *Standard_DS1_v2* VM, aber diese Datei größer oder weitere virtuelle Computer erstellen, wird der Verwaltungsaufwand erheblich zu ändern.

Die Konfiguration verwendet auch [ausgehende trust.ps1] [ outgoing-trust] Skript unten eine unidirektionale ausgehende Vertrauensstellung mit *der Domäne* zu erstellen:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Dieses Skript ist *eingehende trust.ps1* Skript beschriebenen ähnelt. Fügt die IP-Adressen der lokalen AD DS Server an den lokalen DNS-Dienst verwendet [Netdom] [ netdom] Befehl, um die ausgehende Vertrauensstellung erstellen.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Die Lösung setzt die folgenden Komponenten:

- Sie haben ein Azure-Abonnement Sie Ressourcengruppen erstellen können.

- Sie haben heruntergeladen und den neuesten Build von Azure Powershell installiert. Finden Sie [hier] [ azure-powershell-download] Informationen.

Um das Skript auszuführen, das die Lösung bereitgestellt wird:

1. Verschieben in einen geeigneten Ordner auf Ihrem lokalen Computer, und erstellen Sie die folgenden Unterordner:

    - Skripts

    - Skripts-Parameter

    - Parameter/Scripts/Onpremise

    - Parameter/Scripts/Azure

2. Download [Bereitstellen ReferenceArchitecture.ps1] [ solution-script] Datei in den Ordner Scripts

3. Downloaden Sie den Inhalt des [Parameter-Onpremise] [ on-premises-folder] Ordner "Parameter/Scripts/Onpremise":

4. Inhalt der [Parameter-Azure] heruntergeladen[ azure-folder] Ordner "Parameter/Scripts/Azure".

5. Bearbeiten Sie Deploy-ReferenceArchitecture.ps1-Datei im Ordner "Scripts", und ändern Sie folgenden Zeilen Ressourcengruppen angeben, die erstellt oder verwendet, um die Ressourcen von dem Skript erstellt werden soll:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Bearbeiten der Parameter Parameter/Scripts/Onpremise und Ordner Skripts, Parameter/Azure. Aktualisieren der Gruppenverweise in diesen Dateien entsprechend die Namen von Ressourcengruppen zugewiesenen Variablen in der Datei ReferenceArchitecture.ps1 bereitstellen. Die folgende Tabelle zeigt, welche Parameterdateien der Ressourcengruppe verwiesen. Ressourcengruppen *Ra Adfs-Arbeitslast Rg*, *Ra Adfs-Sicherheit Rg* *Ra Adfs fügt Rg*, *Ra-Adfs-Adfs-Rg*und *Ra Adfs-Proxy Rg* nur in der PowerShell-Skript verwendet und nicht in Parameterdateien auftreten.

  	|Ressourcengruppe|Parameterdateien|
  	|--------------|--------------|
  	|RA-Adtrust-Onpremise-rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|RA-Adtrust-Netzwerk-rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-private.Parameters.JSON<br />parameters\azure\dmz-public.Parameters.JSON<br />parameters\azure\loadBalancer-biz.Parameters.JSON<br />parameters\azure\loadBalancer-Data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*zweimal*)

    Außerdem legen Sie die Konfiguration für die lokale und cloud-Komponenten unter [Komponenten] [ solution-components] Abschnitt.

7. Öffnen Sie ein Azure PowerShell-Fenster, wechseln Sie zum Ordner Skripts und führen Sie folgenden Befehl:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Ersetzen Sie `<subscription id>` mit Ihrer Azure-Abonnement-ID

    Für `<location>`, geben einen Bereich Azure `eastus` oder `westus`.

    Die `<mode>` Parameter kann die folgenden Werte aufweisen:

    - `Onpremise`, lokalen simulierten Umgebung erstellen.

    - `Infrastructure`, zu schaffen VNet Feld in der Cloud zu wechseln.

    - `CreateVpn`, Azure virtuelle Netzwerk-Gateway erstellen und mit dem lokalen Netzwerk verbunden.

    - `AzureADDS`, um als Server fügt VMs zu erstellen, Bereitstellen von Active Directory auf die VMs und die Domäne in der Cloud erstellen.

    - `WebTier`, erstellt Web tier VMs und Lastenausgleich.

    - `Prepare`, das alle vorhergehenden Aufgaben ausführt. **Dies ist empfohlen, wenn Sie eine neue Bereitstellung erstellen, und Sie eine vor-Ort-Infrastruktur müssen.**

    - `Workload`Geschäfts- und Tier VMs und Lastenausgleich. Diese virtuellen Computer sind nicht Bestandteil der `Prepare` Option.

    >[AZURE.NOTE] Verwenden Sie die `Prepare` Option des Skripts dauert mehrere Stunden.

8.  Bei Verwendung die lokalen Beispielkonfiguration:

    1. Verbinden Sie mit im Feld Gehe zu (*Ra-Adtrust-Management-vm1* Ressourcengruppe *Ra-Adtrust-Sicherheit-Rg* ). Melden Sie sich als *Testbenutzer* mit Kennwort *AweS0me@PW*.

    2.  Öffnen Sie im Feld Gehe zu auf eine RDP-Sitzung auf dem ersten virtuellen Computer in *der Domäne (die lokale Domäne)* . Diese virtuellen Computer ist die IP-Adresse 192.168.0.4. Der Benutzername ist *Contoso\testuser* mit Kennwort *AweS0me@PW*.

    3. Herunterladen der [eingehenden trust.ps1] [ incoming-trust] Skript und führen sie eingehende Vertrauensstellung aus der Domäne *treyresearch.com* .

9. Wenn Sie eigene lokale Infrastruktur verwenden:

    1. Herunterladen der [eingehenden trust.ps1] [ incoming-trust] Skript.

    2. Bearbeiten Sie das Skript und Ersetzen Sie den Wert der `$TrustedDomainName` Variable mit dem Namen Ihrer Domäne.

    3. Führen Sie das Skript.

10. Jump-Feld Verbinden Sie mit der ersten VM in der *treyresearch.com* -Domäne (der Domäne in der Cloud). Diese virtuellen Computer ist die IP-Adresse 10.0.4.4. Der Benutzername ist *Treyresearch\testuser* mit Kennwort *AweS0me@PW*.

11. Herunterladen der [ausgehenden trust.ps1] [ outgoing-trust] Skript und führen sie eingehende Vertrauensstellung aus der Domäne *treyresearch.com* . Wenn Sie Ihre eigenen lokalen Computer verwenden, bearbeiten Sie das Skript zunächst. Festlegen der `$TrustedDomainName` auf den Namen der lokalen Domäne und die IP-Adressen der AD DS-Server für diese Domäne in die `$TrustedDomainDnsIpAddresses` Variable.

12. Auf einem lokalen Computer führen Sie die Schritte in Artikel [Prüfen einer Vertrauensstellung] [ verify-a-trust] bestimmt, ob die Vertrauensstellung zwischen den Domänen *contoso.com* und *treyresearch.com* ordnungsgemäß konfiguriert wurde. Sie müssen warten Sie wenige Minuten nach Abschluss der vorangegangenen Schritte, bevor die Vertrauensstellung überprüft werden kann.

Optionale Schritte zeigen ermitteln, ob die Vertrauensstellung funktioniert wie erwartet. Diese Schritte müssen auf einem Entwicklungscomputer mit Visual Studio installiert haben.

1.  Führen Sie den folgenden Befehl auf die Webebene erstellen, wenn Sie ihn nicht zuvor festgelegt haben von Azure PowerShell-Fenster (mit der `Prepare` Option):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Dieser Befehl erstellt die Webebene und *treyresearch.com* Domäne VMs hinzugefügt.

2. Erstellen Sie mit Visual Studio auf dem Entwicklungscomputer eine ASP.NET Web-Anwendung mit dem Namen *TreyResearchWebApp*. Verwenden Sie die.NET Framework-4.5.2.

3. Die *MVC* -Vorlage auswählen und die Authentifizierung auf *Windows-Authentifizierung*ändern. Erstellen Sie ein App-Dienst nicht in der Cloud.

3. Erstellen Sie und führen Sie die Anwendung so testen Sie die Authentifizierung. Stellen Sie sicher, dass Ihre Windows-Benutzernamen in der Menüleiste oben auf der Seite rechts angezeigt wird.

4. Schließen Sie InternetExplorer.

5. Im Fenster *Projektmappen-Explorer* mit der rechten Maustaste des TreyResearchWebApp-Projekts, klicken Sie auf *Veröffentlichen*.

6. Klicken Sie auf *Benutzerdefiniert*, klicken Sie im *Web veröffentlichen* . Erstellen Sie ein benutzerdefiniertes Profil mit dem Namen *TreyResearchWebApp*.

7. Auf der Seite *Verbindung* *Veröffentlichungsmethode* auf *Dateisystem* festlegen Sie und einen Ordner namens *TreyResearchWebApp*, an einem geeigneten Ort auf dem Entwicklungscomputer befindet.

8. Legen Sie auf *der Einstellungsseite* der *Konfiguration* *auf*.

9. Klicken Sie auf *der Vorschauseite* auf *Veröffentlichen*.

10. An jede VM in der Webebene wiederum (über das Feld springen) und führen Sie die folgenden Aufgaben. Die IP-Adressen im Web-Tier-VMs sind 10.0.1.4 und 10.0.1.5. Der Benutzername für beide VMs *Treyresearch\testuser* Kennwort ist *AweS0me@PW*:

    1. Kopieren von *TreyResearchWebApp* Ordner und seinen Inhalt vom Entwicklungscomputer *C:\inetpub* Ordner.

    2. Navigieren Sie die Internet Information Services (IIS)-Manager-Konsole *Sites\Default* Website auf dem Computer.

    3. Klicken Sie im *Aktionsbereich* auf *Standardeinstellungen*und ändern Sie den physischen Pfad der Website in *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. Doppelklicken Sie im Ansichtsfenster *Features* auf *Authentifizierung*. Überprüfen Sie, ob *Windows-Authentifizierung* aktiviert und *Die anonyme Authentifizierung* deaktiviert ist.

11. Öffnen Sie von einem lokalen Computer Internet Explorer, und navigieren Sie zu der Website unter http://10.0.1.254 (Dies ist die Adresse des Web-Tier-Lastenausgleich).

12. Geben Sie im Dialogfeld *Windows-Sicherheit* die Anmeldeinformationen eines Benutzers in der lokalen Domäne. Geben Sie einen Benutzernamen in der Domäne *Treyresearch* nicht vorhanden ist. Verwenden die lokalen simulierten Umgebung zunächst einen Benutzer in der Domäne *Contoso* und geben Sie die Anmeldeinformationen des Benutzers.

13. Wenn die Startseite angezeigt wird, überprüfen Sie, ob der richtigen Domäne und Benutzername in der Menüleiste oben auf der Seite rechts angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- Lernen Sie die best Practices für die [Erweiterung Ihrer Domäne lokale fügt Azure][adds-extend-domain]

- Lernen Sie die best Practices für [ADFS Infrastruktur] [ adfs] in Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Sichere Hybrid-Netzwerkarchitektur mit separaten Active Directory-Domänen"