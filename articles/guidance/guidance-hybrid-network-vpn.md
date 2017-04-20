<properties
   pageTitle="Implementieren einer Hybrid-Netzwerkarchitektur Azure und lokalen VPN | Microsoft Azure"
   description="Zum Implementieren einer sicheren Netzwerkarchitektur zwischen Standorten, die einen virtuellen Azure-Netzwerk und einem lokalen Netzwerk über ein VPN verbunden."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Eine Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-Implementierung

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt Methoden zum Erweitern von einem lokalen Netzwerk auf Azure eine Standort-zu-Standort-virtuelles privates Netzwerk (VPN). Der Datenfluss zwischen dem lokalen Netzwerk und ein Azure Virtual Network (VNet) über IPSec VPN-Tunnel. Diese Architektur ist geeignet für Hybrid mit folgenden Merkmalen:

- Teile der Anwendung ausführen lokalen, während andere in Azure ausgeführt.

- Datenverkehr zwischen lokalen Hardware und die Cloud ist zu hell oder etwas längere Wartezeit für Flexibilität und Leistung der Cloud Handel von Vorteil ist.

- Erweiterte Netzwerk stellt ein geschlossenes System. Es ist keine direkten Pfad zum Azure-VNet aus dem Internet.

- Benutzer Verbinden mit dem lokalen Netzwerk in Azure gehostete Dienste verwenden. Die Brücke zwischen dem lokalen Netzwerk und in Azure ausgeführten Dienste ist für Benutzer transparent.

Beispiele für Szenarien, die diesem Profil:

- LOB Anwendung innerhalb einer Organisation verwendet, wurde Teil der Funktionalität zur Cloud migriert.

- Entwicklung/Testlabor Arbeitslasten.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Azure Resource Manager] [ resource-manager-overview] und classic. Dieser Plan verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt die Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Hybrid Netzwerk - VPN" Seite.

![[0]][0]

- **Lokalen Netzwerk.** Dies ist ein Netzwerk von Computer und Geräte über ein privates LAN-Netzwerk in einer Organisation.

- **VPN-Anwendung.** Dies ist ein Gerät oder einen Dienst, die externe Konnektivität mit dem lokalen Netzwerk. Die VPN-Anwendung möglicherweise ein Hardwaregerät oder eine Software-Lösung wie der Routing- und RAS-Dienst (RRAS) in Windows Server 2012 sein.

> [AZURE.NOTE] Eine Liste der unterstützten VPN-Geräte und Informationen zum ausgewählten VPN-Geräte für die Verbindung mit einer Azure VPN-Gateway konfigurieren, finden Sie in der Anleitung für das entsprechende Gerät in der [Liste der von Azure unterstützten VPN-Geräte][vpn-appliance].

- **Mehrstufige Cloudanwendung.** Dies ist die Anwendung in Azure. Es kann mehrere Ebenen mit mehreren Subnetzen über Azure Lastenausgleich verbundenen enthalten. Der Datenverkehr in jedem Subnet unterliegen Regeln [Azure Netzwerk]Sicherheitsgruppen (NSGs)[azure-network-security-group]. Weitere Informationen finden Sie unter [Erste Schritte mit Microsoft Azure Security][getting-started-with-azure-security].

> [AZURE.NOTE] Dieser Artikel beschreibt Cloudanwendung als eine Einheit. Finden Sie unter [Ausführen von N-Tier-Architektur auf Azure] [ implementing-a-multi-tier-architecture-on-Azure] Informationen.

- **[Virtuelles Netzwerk (VNet)][azure-virtual-network].** Cloudanwendung und die Komponenten für Azure VPN-Gateway befinden sich in der gleichen VNet.

- **[Azure VPN-Gateway][azure-vpn-gateway].** VPN-Gateway-Dienst können Sie das VNet mit dem lokalen Netzwerk über eine VPN-Anwendung verbinden. Weitere Informationen finden Sie in [einem lokalen Netzwerk mit einem virtuellen Microsoft Azure-Netzwerk verbinden][connect-to-an-Azure-vnet]. VPN-Gateway umfasst Folgendes:

  - **Virtuelle Netzwerk-Gateway.** Dies ist eine Ressource eine virtuelle VPN-Anwendung für das VNet. Er ist verantwortlich Datenverkehr aus dem lokalen Netzwerk auf die VNet.

  - **Lokales Netzwerk-Gateway.** Dies ist eine Abstraktion des lokalen VPN-Anwendung. Netzwerkverkehr von Cloudanwendung mit dem lokalen Netzwerk wird über dieses Gateway geleitet.

  - **Verbindung.** Die Verbindung hat Eigenschaften, mit denen die Verbindung (IPSec) und der gemeinsam mit lokalen VPN-Anwendung zur Verschlüsselung des Verkehrs.

  - **Gatewaysubnetz.** Virtuelle Netzwerk-Gateway findet in seinem eigenen Subnetz die verschiedenen Auflagen gemäß Abschnitt Recommendations.

- **Interne Lastenausgleich.** Cloud-Anwendung über eine interne Lastenausgleich wird Netzwerkverkehr von VPN-Gateway weitergeleitet. Lastenausgleich befindet sich im Subnetz Front-End-Anwendung.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie eigene Referenzarchitektur erstellen diese Empfehlung Sie befolgen nur eine bestimmte Anforderung, die Empfehlung nicht passen.

### <a name="vnet-and-gateway-subnet"></a>VNet und Gateway Subnetz

Erstellen Sie ein Azure-VNet für die Aufnahme der Komponenten in der Cloud. Adressraum Azure VNet muss groß genug für die Adressen von VMs und Subnetze in der VNet verwendet. Sicher, dass der Adressraum VNet ausreichend Platz für Wachstum sind zusätzliche VMs wahrscheinlich in der Zukunft. Der Adressraum der VNet muss nicht mit dem lokalen Netzwerk überlappen. Diagramm verwendet z. B. Adresse Speicherplatz 10.20.0.0/16 für die VNet.

Erstellen Sie ein Subnetz mit dem Namen _Subnetzname_mit einem Adressbereich des /27. Virtuelle Netzwerk-Gateway dieses Subnetz erforderlich ist und Zuweisen von 32 Adressen zu diesem Subnetz hilft, um Sie gegen mögliche Gateway Größenlimits zukünftig zu verhindern. Stellen Sie dieses Subnetz in den Adressraum. Eine gute Vorgehensweise soll den Adressbereich für das gatewaysubnetz am oberen Ende des Adressbereichs VNet. In der Abbildung dargestellten Beispiel: 10.20.255.224/27.  Ein schnelles Verfahren zum Berechnen der CIDR lautet wie folgt:

1. Legen Sie Variablen Bits im Adressraum des VNet 1 bis Bits wird vom gatewaysubnetz verwendet und die restlichen Bits auf 0 festgelegt.

2. Die resultierenden Bits in eine Dezimalzahl konvertieren und als Adressraum mit dem Präfix Länge die Größe des Subnetzes Gateway express.

Beispielsweise wird ein VNet mit einer IP-Adresse 10.20.0.0/16 Anwendung Schritt #1 oben 10.20.0b11111111.0b11100000.  Die in eine Dezimalzahl konvertieren und als Adressraum 10.20.255.224/27 ergibt Ausdrücken

> [AZURE.WARNING] Gateway-Subnetz anderen virtuellen Computern oder Instanzen nicht bereitgestellt werden. Außerdem weisen Sie eine NSG zu diesem Subnetz als Gateway zum Absturz führt.

### <a name="virtual-network-gateway"></a>Virtuelle Netzwerk-gateway

Reservieren Sie eine öffentliche IP-Adresse für das virtuelle Netzwerkgateway.

Virtuelles Netzwerk-Gateways in Subnetz Gateway erstellen und neu zugewiesene öffentliche IP-Adresse zuweisen. Verwenden Sie den Gateway, die Ihren Anforderungen entspricht, und durch die VPN-Anwendung aktiviert ist:

Erstellen Sie eine [Policy-basierte Gateway] [ policy-based-routing] genau steuern können, wie Anfragen weitergeleitet werden soll. basierend auf Kriterien wie Adresspräfixe. Policy-basierte Gateways statisches routing verwenden und nur mit zwischen Standorten.

Erstellen einer [Route-basierte Gateway] [ route-based-routing] Verbindung im lokalen Netzwerk mit RRAS unterstützt mehrere Standorte oder Cross-Region Anschlüsse oder VNet VNet-Verbindungen (einschließlich Routen, die mehrere VNets durchlaufen) implementieren. Route-basierte Gateways verwenden Sie dynamisches routing für direkte Datenverkehr zwischen Netzwerken. Sie können Fehler im Netzwerkpfad besser als statische Routen tolerieren, da sie alternative Routen versuchen. Route-basierte Gateways können auch den Verwaltungsaufwand reduziert, da Routen möglicherweise nicht manuell aktualisiert werden, wenn Adressen ändern.

Eine Liste der unterstützten VPN-Geräte finden Sie [über VPN-Geräte für Standort-zu-Standort-VPN-Gateway Verbindungen][vpn-appliances].

> [AZURE.NOTE] Nach dem Erstellen das Gateway können Sie zwischen Gateway löschen und Neuerstellen des Gateways nicht ändern.

Wählen Sie Azure VPN-Gateway SKU, die Ihren durchsatzanforderungen entspricht. Azure VPN-Gateway ist in drei in der folgenden Tabelle aufgeführten SKUs verfügbar. Bietet unterschiedliche Durchsatz [Gebühren erhoben] , jede SKU[ azure-gateway-charges] basiert auf der Zeitspanne Gateway bereitgestellt und verfügbar.

| SKU | VPN-Durchsatz | Max. IPSec-Tunnel|
|-----|----------------|------------------|
| Grundlegende | 100 Mbit/s | 10 |
| Standard | 100 Mbit/s | 10 |
| Hohe Leistung | 200 Mbit/s | 30 |

> [AZURE.NOTE] Grundlegende SKU ist nicht kompatibel mit Azure ExpressRoute. [Ändern die SKU] kann[ changing-SKUs] nachdem das Gateway erstellt wurde.

Erstellen Sie Routingregeln für das gatewaysubnetz,, direkte Anwendungsdatenverkehr vom Gateway zu internen Lastenausgleich anstatt Anfragen direkt an die VMs übergeben, die die Anwendung implementieren.

### <a name="on-premises-network-connection"></a>Vor-Ort-Verbindung

Erstellen Sie ein lokales Netzwerk-Gateway. Geben Sie die öffentliche IP-Adresse des lokalen VPN-Anwendung und der Adressbereich des lokalen Netzwerks. Beachten Sie, dass die lokalen VPN-Anwendung eine öffentliche IP-Adresse muss, die vom lokalen Netzwerk-Gateway in Azure VPN-Gateway zugegriffen werden kann. Das VPN-Gerät kann nicht hinter einem NAT-Gerät befinden.

Erstellen Sie eine Standort-zu-Standort-Verbindung virtuelles Netzwerk-Gateways und lokale Netzwerk-Gateway. Wählen Sie den Verbindungstyp Standorten (IPSec) und den freigegebenen Schlüssel. Standort-zu-Standort-Verschlüsselung mit Azure VPN-Gateway basiert auf das IPSec-Protokoll zur Authentifizierung mit vorinstallierten Schlüsseln. Den Schlüssel wird beim Erstellen von Azure VPN-Gateway angeben. Sie müssen denselben Schlüssel mit lokalen VPN-Anwendung konfigurieren. Weitere Authentifizierungsverfahren werden derzeit nicht unterstützt.

Stellen Sie sicher, dass die lokalen Routinginfrastruktur Anfragen weiterleiten zur Adressen in Azure VNet VPN-Gerät konfiguriert ist.

Öffnen Sie alle Anschlüsse, die Cloud-Anwendung im lokalen Netzwerk.

Um sicherzustellen, dass die Verbindung zu testen:

- Die lokalen VPN-Anwendung leitet den Datenverkehr ordnungsgemäß Cloud-Anwendung über Azure VPN-Gateway.

- Das VNet leitet den Datenverkehr ordnungsgemäß an das lokale Netzwerk.

- Unzulässige Datenverkehr in beiden Richtungen richtig blockiert.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Begrenzte vertikale Skalierung erreichen durch Verschieben von Basic oder Standard-VPN-Gateway-SKUs hohe Leistung VPN-SKU.

VNets, die eine große Anzahl von VPN-Datenverkehr erwarten, sollten Sie verschiedenen Arbeitslasten in separaten kleineren VNets verteilen und für jeden ein VPN-Gateway konfigurieren.

Sie können das VNet horizontal oder vertikal partitionieren. Um horizontal zu partitionieren, bewegen Sie manchmal VM von jedes Tier in Subnetzen eines neuen VNet Das Ergebnis hat jedes VNet dieselbe Struktur und dieselben Funktionen. Um vertikal partitionieren, Entwerfen Sie jede Ebene die Funktionen in logische Bereiche (z. B. Aufträge, Fakturierung, Kundenbetreuung und usw.) zu unterteilen. Jeder Funktionsbereich kann dann in eine eigene VNet platziert werden.

Replizieren eines lokalen Active Directory-Domänencontrollers in die VNet und Implementieren von DNS in der VNet können um einige der Sicherheit und Verwaltung Datenverkehr lokal in die Cloud zu reduzieren.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Möchten Sie das lokale Netzwerk Azure VPN-Gateway verfügbar bleibt, Implementieren eines Failoverclusters lokalen VPN-Gateways.

Wenn Ihre Organisation mehrere lokale Standorte verfügt, [standortübergreifende Verbindungen] erstellen[ vpn-gateway-multi-site] auf eine oder mehrere Azure-VNets. Dieser Ansatz erfordert dynamisches routing (Route-basiert), so sicher, dass der lokale VPN-Gateway dieses Feature unterstützt.

[SLA für VPN-Gateway] finden Sie unter[ sla-for-vpn-gateway] Einzelheiten VPN-Gateway SLA.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Überwachen Sie Diagnoseinformationen vom lokalen VPN-Geräte. Dabei hängt die Features der VPN-Anwendung. Z. B. Wenn Sie den Routing- und RAS-Dienst in Windows Server 2012 verwenden, Sie sollten Protokollierung [RRAS][rras-logging].

Verwenden Sie [Azure VPN-Gateway-Diagnose] [ gateway-diagnostic-logs] Informationen über Probleme mit der Netzwerkkonnektivität. Diese Protokolle können verwendet werden, um Informationen wie die Quelle und das Ziel der Verbindung fordert, welches Protokoll verwendet wurde, und wie die Verbindung (oder warum fehlgeschlagen).

Überwachen Sie Betriebsprotokolle Azure VPN-Gateway die Überwachungsprotokolle in Azure-Portal verfügbar. Separate Protokolle sind für das lokale Netzwerk-Gateway, Azure Gateways und die Verbindung. Diese Informationen kann zum Nachverfolgen von Änderungen an das Gateway verwendet werden und ist nützlich, wenn eine zuvor funktionierende Gateway aus irgendeinem Grund ausfällt.

![[2]][2]

Verbindung überwachen und Konnektivität Fehlerereignisse nachverfolgt. Überwachungspaket wie [Nagios] können[ nagios] aufzeichnen und diese Informationen.

## <a name="security-considerations"></a>Sicherheitsaspekte

Erstellen Sie einen anderen freigegebenen Schlüssel für jede VPN-Gateway. Mithilfe einer starken Schlüsseln Brute-Force-Angriffen.

> [AZURE.NOTE] Sie können nicht derzeit Azure Key Vault vorinstallierte Schlüssel Azure VPN-Gateway verwenden.

Sicherstellen, dass die lokalen VPN-Anwendung eine Verschlüsselungsmethode verwendet [Azure VPN-Gateway mit][vpn-appliance-ipsec]. Policy-basierte routing unterstützt Azure VPN-Gateway AES128, AES256 und 3DES Verschlüsselungsalgorithmen. Route-basierte Gateways unterstützen AES256 und 3DES.

Wenn Ihre lokalen VPN-Anwendung in einem Umkreisnetzwerk mit einer Firewall zwischen dem Umkreisnetzwerk und dem Internet, müssen Sie möglicherweise [zusätzliche Firewall-Regeln] konfigurieren[ additional-firewall-rules] um die Standort-zu-Standort-VPN-Verbindung zu ermöglichen.

Die Anwendung in das VNet Daten über das Internet sendet, sollten [erzwungenen Tunneln implementieren] [ forced-tunneling] für alle Internet gerichtete Datenverkehr im lokalen Netzwerk. Dadurch können Sie ausgehende Anfragen von der Anwendung der lokalen Infrastruktur überwachen.

> [AZURE.NOTE] Erzwungene Tunnel Konnektivität Azure Services (z. B. der Speicherdienst) auswirken kann und die Windows-Lizenz-Manager.

## <a name="troubleshooting-considerations"></a>Aspekte zur Problembehandlung

> [AZURE.NOTE] Besuchen Sie Routing und Remote Access Blog allgemeine Informationen zur [Behebung von allgemeinen VPN - Fehler][troubleshooting-vpn-errors].

Wenn Datenverkehr die VPN-Verbindung durchlaufen kann, überprüfen Sie Folgendes:

### <a name="on-premises-vpn-appliance"></a>Lokalen VPN-Anwendung:

**Ist die lokale VPN-Anwendung fehlerfrei?**

Überprüfen Sie alle Protokolldateien, die VPN-Anwendung für Fehler generiert. Der Speicherort dieser Informationen variiert je nach Ihrer Appliance. Beispielsweise verwenden Sie RRAS auf Windows Server 2012, können den folgenden PowerShell-Befehl Sie die Fehler Ereignisinformationen für den RRAS-Dienst:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

_Die Nachrichteneigenschaft für jeden Eintrag_ enthält eine Beschreibung des Fehlers. Einige Beispiele sind:

- Unfähigkeit, möglicherweise durch eine falsche IP-Adresse für Azure VPN-Gateway in die Konfiguration der Netzwerkschnittstelle RRAS VPN eine Verbindung herzustellen:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Im RRAS VPN Netzwerkschnittstellenkonfiguration falschen Schlüsseln:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Ereignisprotokollinformationen über Verbindungsversuche über den RRAS-Dienst mit dem folgenden PowerShell-Befehl zur Verfügung:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Bei Ausfall einer Verbindung wird dieses Protokoll Fehler enthalten, die etwa wie folgt aussehen:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Ist der VPN-Gerät ordnungsgemäß routing über Azure VPN-Gateway?**

Verwenden Sie ein Tool wie [PsPing] [ psping] Konnektivität und routing über die VPN-Gateways überprüfen. Z. B. zum Testen der Konnektivität von einem lokalen Computer auf einen Webserver auf die VNet führen den folgenden Befehl (ersetzen Sie `<<web-server-address>>` mit der Adresse des Webservers):

```
PsPing -t <<web-server-address>>:80
```

Wenn der lokale Computer Datenverkehr an den Webserver weiterleiten kann, sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Wenn der lokale Computer mit dem angegebenen Ziel kommunizieren kann, sehen Sie Nachrichten wie diese:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Lässt die lokale Firewall VPN-Datenverkehr zum passieren? Sind die richtigen Ports geöffnet?**

Überprüfen, ob die lokalen VPN-Anwendung eine Verschlüsselungsmethode verwendet [Azure VPN-Gateway mit][vpn-appliance].

Policy-basierte routing unterstützt Azure VPN-Gateway AES128, AES256 und 3DES Verschlüsselungsalgorithmen. Route-basierte Gateways unterstützen AES256 und 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet Gateway:

Untersuchen Sie [Diagnoseprotokolle Azure VPN-Gateway] [ gateway-diagnostic-logs] auf potenzielle Probleme.

**Sind Azure VPN-Gateway und lokalen VPN-Anwendung mit den gleichen gemeinsamen Authentifizierungsschlüssel konfiguriert**

Sie können den freigegebenen Schlüssel gespeichert von Azure VPN-Gateway mit folgenden Azure CLI-Befehl anzeigen:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Befehl für die lokale VPN-Anwendung an den gemeinsamen Schlüssel für dieses Gerät konfiguriert.

Überprüfen Sie, ob _Subnetzname_ Subnetz Azure VPN-Gateway keine NSG zugeordnet ist.

Mit folgenden Azure CLI-Befehl können Sie die Subnetdetails anzeigen:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Sicher kein Datenfeld _Network Security Group Id_benannt. Das folgende Beispiel zeigt die Ergebnisse von _Subnetzname_ , die eine zugewiesene NSG (_VPN-Gateway-Gruppe_). Dadurch kann das Gateway ordnungsgemäß funktioniert, gibt es keine Regeln für diese NSG verhindern:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Werden die virtuellen Computer in Azure VNet konfiguriert geleitete Datenverkehr des Vnets zulassen? Überprüfen Sie alle Regeln NSG Subnetzen mit diesen virtuellen Computern.**

Mit dem folgenden Azure CLI-Befehl können Sie alle NSG Regeln anzeigen:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Wurde der Azure VPN-Gateway getrennt?**

Folgenden Azure PowerShell-Befehl können Sie den aktuellen Status der Azure-VPN-Verbindung überprüfen. Die `<<connection-name>>` Parameter ist der Name der Azure-VPN-Verbindung, das virtuelle Netzwerk-Gateway und das lokale Gateway.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Die folgenden Ausschnitte markieren die Ausgabe generiert, wenn das Gateway (das erste Beispiel) verbunden und getrennt (zweites Beispiel):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>VM-Hostkonfiguration Nutzung der Netzwerk-Bandbreite und Leistung der Anwendung:

Überprüfen, ob die Firewall im Gastbetriebssystem auf Azure VMs im Subnetz sind konfiguriert ordnungsgemäß zugelassenen Datenverkehr von lokalen IP-Bereiche.

**Steht der Umfang des Datenverkehrs nähern Bandbreite Azure VPN-Gateway?**

Werkzeuge hängt Einrichtungen der VPN-Anwendung lokal ausführen. Beispielsweise bei Verwendung von RRAS in Windows Server 2012 können-Systemmonitor Sie Datenmengen empfangen und übertragen die VPN-Verbindung zu verfolgen; Wählen Sie _RAS insgesamt_ Objekt die Leistungsindikatoren _Empfangene Bytes/s_ und _Gesendet_ :

![[3]][3]

Vergleichen Sie die Ergebnisse mit VPN-Gateway (100 Mbit/s für Basic und Standard-SKUs und 200 Mbit/s für hohe Leistung SKU) verfügbare Bandbreite:

![[4]][4]

**Laufen alle virtuellen Computer in Azure VNet langsam? Sie überladen sind, sind es genug davon das handhaben, alle Lastenausgleich richtig konfiguriert?**

[Erfassen und analysieren Sie Diagnoseinformationen]hierfür[azure-vm-diagnostics]. Überprüfen Sie die Ergebnisse mithilfe des Azure-Portals, aber viele Drittanbieter-Tools sind verfügbar, die bieten detaillierte Einblicke in die Performance-Daten.

**Ist die Anwendung effizienter Cloudressourcen?**

Instrument Anwendungscode auf jede VM ermitteln, ob die Anwendung die beste Ressourcen nutzen. Verwenden Sie Tools wie [Application Insights] [ application-insights] können Sie diese Aufgaben ausführen.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Wenn Sie eine vorhandene lokale Infrastruktur bereits ein VPN-Gerät konfiguriert haben, können Sie Referenzarchitektur folgendermaßen bereitstellen:

1. Rechts klicken und entweder "Link öffnen in neuem Tab" oder "Link in neuem Fenster öffnen":  
[![In Azure bereitstellen](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Warten auf die Verknüpfung zum Azure-Portal öffnen, und gehen Sie folgendermaßen vor: 
    - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-hybrid-vpn-rg` im Textfeld.
    - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie, bis die Bereitstellung abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

- [Installieren zusätzliche Domänencontroller für lokale Active Directory-Domäne mit VMs in einer Azure-VNet][installing-ad].

- [Richtlinien für die Bereitstellung von Windows Server Active Directory Azure VMs][deploying-ad].

- [Erstellen einen DNS-Server in einem VNet][creating-dns].

- [Konfigurieren und Verwalten von DNS in einem VNet][configuring-dns].

- [Ein VPN über lokale Stormshield Netzwerksicherheitsgeräte][stormshield].

- [Implementieren einer Hybrid-Netzwerkarchitektur mit Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Eine Mischung aus dem lokalen Netzwerk und cloud-Infrastrukturen"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "VNet zur Verbesserung der Skalierbarkeit Partitionierung"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Überwachungsprotokolle in Azure-portal"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Leistungsindikatoren zum Überwachen des Netzwerkverkehrs VPN"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Beispiel-VPN-Netzwerk Leistungsdiagramm"