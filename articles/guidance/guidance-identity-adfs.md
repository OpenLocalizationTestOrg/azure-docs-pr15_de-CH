<properties
   pageTitle="Implementieren von ADFS in Azure | Microsoft Azure"
   description="Zum Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Active Directory-Verbunddienst Autorisierung in Azure."
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
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Implementieren von Active Directory Federation Services (ADFS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt die optimale Methoden zum Implementieren eines sicheren Hybrid, Ihrem lokalen Netzwerk in Azure erweitert und mit [Active Directory Federation Services (ADFS)] [ active-directory-federation-services] für Verbundauthentifizierung und Autorisierung für Komponenten in der Cloud. Diese Architektur erweitert die Struktur von [Active Directory erweitern, um Azure]beschrieben[implementing-active-directory].

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Diese Referenzarchitektur verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

ADFS können lokal, jedoch in einer Anwendung in Azure befinden sich Hybrid-Szenario kann sein, diese Funktion in der Cloud zu implementieren. Normale Anwendungsfälle für diese Architektur gehören:

- Hybrid Applications, Arbeitslasten Ausführen teilweise lokalen, und teilweise in Azure.

- Projektmappen, die verbundenen Autorisierung gemacht ASP.NET-Webanwendungen zu Partnerorganisationen nutzen.

- Systeme, die von außerhalb der Organisation Firewalls mit Webbrowsern unterstützt.

- Systeme, die Benutzern Zugriff auf Webapplikationen aus autorisierten externen Geräte wie remote-Computern, Notebooks und andere mobile Geräte ermöglichen. 

Weitere Informationen zur Funktionsweise von ADFS finden Sie unter [Active Directory Federation Services Overview][active-directory-federation-services-overview]. Außerdem Artikel [ADFS-Bereitstellung in Azure] [ adfs-intro] enthält eine ausführliche schrittweise Einführung zum Implementieren von ADFS in Azure.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur (*zum Vergrößern klicken*). Weitere Informationen über ein Element nicht im Zusammenhang mit ADFS finden Sie unter [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture], [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], und [Implementieren Sie eine sichere Hybrid-Netzwerkarchitektur mit Active Directory in Azure][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Dieses Diagramm zeigt die folgenden Fällen:
>
>- Anwendungscode in einer Partnerorganisation greift eine Anwendung in Ihrem VNet Azure gehostet.
>
>- Eine externe, registrierte Benutzer (mit ADDS gespeicherten Anmeldeinformationen) den Zugriff auf eine Anwendung in Ihrem VNet Azure gehostet.
>
>- Ein Benutzer mit Ihr VNet mit autorisierten Geräte und eine Web-Anwendung in Ihrem VNet Azure gehostet.
>
>Diese Architektur konzentriert sich außerdem auf passiven Verbund der Verbundserver,, wie und wann entscheiden einen Benutzer authentifizieren. Der Benutzer muss beim Start einer Anwendung Anmeldeinformationen bereitstellen. Dies ist der Mechanismus, der am häufigsten verwendeten Webbrowser und umfasst eine Protokoll, die den Browser zu einer Website leitet, in dem Benutzer ihre Anmeldeinformationen eingeben kann. ADFS unterstützt auch aktiven Verbund, wobei eine Anwendung übernimmt ohne weiteren Benutzereingriff Anmeldeinformationen bereitstellen, aber dieser Fall Anwendungsbereich dieser Architektur ist.

- **Fügt Subnetz.** Der Server fügt sind im eigenen Subnetz enthalten. NSG Regeln fügt Server schützen und eine Firewall vor Datenverkehr von unerwarteten Quellen erhalten.

- **Server hinzugefügt.** Dies sind Domänencontroller als VMs in der Cloud. Diese Server können Authentifizierung von lokalen Identitäten in der Domäne bereitstellen.

- **ADFS-Subnetz.** ADFS-Server können in einem eigenen Subnetz NSG Regeln als Firewall fungieren.

- **ADFS-Server.** ADFS-Server bieten verbundenen Autorisierung und Authentifizierung. In dieser Architektur führen sie die folgenden Aufgaben:

    - Sie erhalten Sicherheitstoken mit Ansprüchen Partnerverbundserver für Partnerbenutzer. ADFS überprüfen, ob diese Token vor Claims to in Azure ausgeführte Webanwendung gültig sind. Corporate Webanwendung (in Azure) können diese Ansprüche Anfragen autorisieren. In diesem Szenario corporate Web-Anwendung ist die *relying Party,*und ist die Verantwortung des Verbundservers Partner ausstellen behauptet, von corporate Web-Anwendung verstanden werden. Da sie Zugriff für authentifizierte Konten in der Partnerorganisation einzureichen Verbundserver Partner als *Kontopartner* bezeichnet. ADFS-Server sind *Ressourcenpartner* bezeichnet, da sie auf Ressourcen (in diesem Fall Web Applications Zugriff).

    - Authentifizieren sie (fügt und [Active Directory Gerät Registrierungsdienst][ADDRS]) und Anfragen von externen Benutzern ein Web-Browser oder Geräte, die Zugriff auf Ihre corporate Web Applications. 

    ADFS-Server sind als Farm Zugriff über Azure Lastenausgleich konfiguriert. Diese Struktur hilft zu Verfügbarkeit und Skalierung. Beachten Sie, dass ADFS-Server sind nicht direkt mit dem Internet, sondern alle Internetverkehr über ADFS Web Application Proxyserver und einer DMZ gefiltert.

- **ADFS Proxy befindet.** NSG Regeln Schutz können ADFS-Proxyserver im eigenen Subnetz enthalten sein. Server in diesem Subnetz sind im Internet über eine Reihe von virtuellen Netzwerkgeräte ausgesetzt, die einen Firewall zwischen Azure virtuelle Netzwerk und dem Internet ermöglichen.

- **ADFS Web Proxy (WAP) Anwendungsserver.** Diese Computer fungieren als ADFS-Server für die Anfragen von Organisationen und externen Geräte. WAP-Server fungieren als Filter ADFS-Server vor direktem Zugriff vom öffentlichen Internet abschirmen. Mit ADFS-Server bereitstellen das WAP-Server in einer Farm mit Lastenausgleich ermöglicht höhere Verfügbarkeit und skalierbar als eine Sammlung von eigenständigen Servern bereitstellen.

    >[AZURE.NOTE] Ausführliche Informationen zum Installieren von WAP-Servern finden Sie unter [Web Application Proxy Server installieren und konfigurieren][install_and_configure_the_web_application_proxy_server]

- **Die Organisation.** Dies ist ein Beispiel Partnerorganisation, der eine Anwendung ausgeführt wird, der den Zugriff anfordert, die Webanwendung in Azure ausgeführt. Der Verbundserver in der Partnerorganisation authentifiziert Anfragen lokal und Sicherheitstoken mit Ansprüche in Azure mit ADFS sendet. ADFS in Azure überprüft die Sicherheitstoken und Gültigkeit der Ansprüche auf die Web-Anwendung in Azure ermächtigen sie übergeben.

    >[AZURE.NOTE] Sie können auch einen VPN-Tunnel über Azure Gateway den direkten Zugriff auf ADFS für vertrauenswürdige Partner. Diese Partner Anfragen durchlaufen WAP-Server.

## <a name="recommendations"></a>Empfehlung

Dieser Abschnitt enthält eine Zusammenfassung für die Implementierung von ADFS in Azure für:

- VM Recommendations.

- Netzwerk empfohlen.

- Verfügbarkeit Recommendations.

- Sicherheitsaspekte.

- ADFS mit der Installation beginnen.

- ADFS vertrauen Recommendations.

### <a name="vm-recommendations"></a>VM-Empfehlung

VMs mit ausreichenden Ressourcen, um die erwartete Menge Datenverkehr erstellen. Verwenden Sie die Größe der vorhandenen Computer hosten ADFS lokal als Ausgangspunkt. Überwachen der Ressourcenverwendung. Die VMs Größe können und skalieren, sind zu groß.

Befolgen Sie die Ratschläge in [einer VM Windows Azure ausgeführt]aufgeführt[vm-recommendations].

### <a name="networking-recommendations"></a>Netzwerk-Empfehlung

Konfigurieren der Netzwerkschnittstelle für jede VMs mit ADFS und WAP Servern statische private IP-Adressen.

Geben Sie ADFS VMs öffentlichen IP-Adressen. Weitere Informationen finden Sie unter [Security Considerations][security-considerations].

Legen Sie die IP-Adresse der bevorzugte und sekundäre DNS-Server für die Netzwerkschnittstellen für jede ADFS und WAP VM VMs fügt verweisen (DNS ausgeführt werden soll). Dieser Schritt ist notwendig jede VM, der Domäne beizutreten.

### <a name="availability-recommendations"></a>Verfügbarkeit recommendations

Erstellen einer Serverfarm ADFS mindestens zwei Servern zur Erhöhung der Verfügbarkeit des ADFS-Diensts.

Verwenden Sie verschiedene Konten für jede ADFS VM in der Farm. Dadurch wird sichergestellt, dass ein Fehler in ein einzelnes Speicherkonto nicht die gesamte Farm nicht verfügbar.

Erstellen Sie separate Azure verfügbarkeitssätze für ADFS und WAP VMs. Sicherstellen Sie, dass es mindestens zwei VMs in jedem Satz. Jeder Satz Verfügbarkeit müssen mindestens zwei aktualisierungsdomänen und zwei Fehlerdomänen.

Konfigurieren Sie den Lastenausgleich für ADFS VMs und WAP VMs wie folgt:

- Verwenden Sie Azure Lastenausgleich den externen Zugriff auf WAP-VMs und internen Lastenausgleich Verteilung die Last über ADFS-Server in der Farm ADFS.

- Nur übergeben Sie Datenverkehr auf Port 443 (HTTPS) an die ADFS-WAP-Server.

- Geben Sie das System zum Lastenausgleich eine statische IP-Adresse.

- Erstellen Sie einen Integritätstest anstelle der TCP-Protokoll HTTPS. Pingen Sie Port 443 zu überprüfen, ob ein ADFS-Server funktioniert.

    >[AZURE.NOTE] ADFS-Server verwenden Server Name Angabe (SNI)-Protokoll, so Prüfpunkt mit HTTPS-Endpunkt aus der Load Balancer nicht versucht.

- Die Domäne des Lastenausgleichs ADFS einen DNS- *A* -Datensatz hinzufügen. 

    Die IP-Adresse des Lastenausgleich, und geben sie einen Namen in der Domäne (z. B. adfs.contoso.com). Dies ist der Name, über den Clients und Servern WAP ADFS-Serverfarm zugreifen.

### <a name="security-recommendations"></a>Sicherheitsaspekte

Vermeiden Sie direkte ADFS-Server im Internet. ADFS-Server sind Domänencomputern, die vollständige Genehmigung Sicherheitstoken. Wenn ein ADFS-Server gefährdet ist, kann ein böswilliger Benutzer alle ASP.NET-Webanwendungen und Verbundserver ADFS geschützt sind vollständige Zugriffstoken ausstellen. Behandlung von Anfragen von externen Benutzer nicht vertrauenswürdigen Partner-Websites muss Ihr System verwenden Sie WAP-Server diese Anfragen zu behandeln. Weitere Informationen finden Sie unter [wo eines Verbundserverproxys][where-to-place-an-fs-proxy].

Direkte ADFS-Server und Servern in getrennten Subnetzen mit eigenen Firewalls WAP. NSG Regeln können Sie Firewall-Regeln. Umfassenderen Schutz benötigen Sie können implementieren zusätzliche Sicherheitszaun Servern mit einem Subnets und NVAs, wie das [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure]Dokument[implementing-a-secure-hybrid-network-architecture-with-internet-access]. Beachten Sie, dass alle Firewalls auf Port 443 (HTTPS Datenverkehr).

Direkte Anmeldung Zugriff auf die ADFS und WAP DevOps-Mitarbeiter sollte eine Verbindung herstellen.

WAP-Server der Domäne nicht beitreten.

### <a name="adfs-installation-recommendations"></a>ADFS Installation beginnen

[Bereitstellen einer Verbundserverfarm] Artikel[ Deploying_a_federation_server_farm] bietet detaillierte Anleitung zum Installieren und Konfigurieren von ADFS. Führen Sie die folgenden Schritte vor dem ersten ADFS-Server in der Farm konfigurieren:

1. Beziehen Sie öffentlich vertrauenswürdige Zertifikat für die Authentifizierung durchführen. Der *Antragstellername* muss den Namen enthalten, den Verbunddienst Clientzugriff. Dies ist der DNS-Name für den Lastenausgleich, z. B. *adfs.contoso.com* registriert (verwenden Sie Platzhalter wie **. contoso.com*, aus Sicherheitsgründen). Verwenden Sie dasselbe Zertifikat auf VMs ADFS-Server. Ein Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle erwerben, aber wenn Ihre Organisation Active Directory-Zertifikatdienste können eigene. 

    Der *alternative Antragstellername* DRS dient zum Aktivieren des Zugriffs von externen Geräten. Dies sollte der Form *enterpriseregistration.contoso.com*.

    Weitere Informationen finden Sie unter [erwerben und konfigurieren ein SSL-Zertifikat für ADFS][adfs_certificates].

2. Auf dem Domänencontroller neuen Schlüssel Root für Key Distribution Service. Festgelegt effektiv immer die aktuelle Uhrzeit minus 10 Stunden (diese Konfiguration reduziert die Verzögerung, die auftreten können, und synchronisieren Schlüssel in der Domäne). Dieser Schritt ist erforderlich zur Unterstützung beim Erstellen des Dienstkontos Gruppe, die zum Ausführen des ADFS-Diensts verwendet werden. Der folgende Powershell-Befehl zeigt ein Beispiel dazu:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Jede ADFS-Server virtueller Computer der Domäne hinzufügen.

>[AZURE.NOTE] Um ADFS zu installieren, muss der Domänencontroller mit der PDC-Emulator-FSMO-Rolle für die Domäne ausgeführt werden und über die ADFS-VMs.

### <a name="adfs-trust-recommendations"></a>Empfohlene ADFS vertrauen

Föderation Vertrauensstellungen zwischen der ADFS-Installation und der Verbundserver Partnerorganisationen einrichten. Konfigurieren Sie alle Ansprüche filtern und Zuordnung erforderlich. Dieser Prozess erfordert:

- DevOps Mitarbeiter **in jeder Organisation** eine relying Party Vertrauensstellung für ASP.NET-Webanwendungen über die ADFS-Server hinzufügen.

- DevOps-Mitarbeitern **in Ihrer Organisation** konfigurieren Anspruchsanbieter Vertrauensstellung ADFS Server Ansprüche vertrauen, die Partnerorganisationen zu ermöglichen.

- DevOps-Mitarbeitern **in Ihrer Organisation** konfigurieren ADFS Ansprüche auf Ihrer Organisation ASP.NET-Webanwendungen übergeben.

    Weitere Informationen finden Sie unter [Einrichten Föderation vertrauen][establishing-federation-trust].

Veröffentlichen Sie Ihrer Organisation ASP.NET-Webanwendungen und zur stellen Sie Verfügung externe Partner mit Vorauthentifizierung WAP-Server. Weitere Informationen finden Sie unter [Veröffentlichen Anwendung ADFS Vorauthentifizierung][publish_applications_using_AD_FS_preauthentication]

Beachten Sie, dass ADFS token Transformation und Erweiterung unterstützt. Azure Active Directory bietet dieses Feature nicht. Mit ADFS beim Einrichten der Vertrauensstellungen können Sie:

- Erstellt für Autorisierungsregeln zu konfigurieren. Beispielsweise können Sie Sicherheit von einer Darstellung einer Organisation nicht Microsoft Partner etwas in Ihrer Organisation, fügt autorisieren kann zuordnen.

- Ansprüche von einem Format in ein anderes zu transformieren. Beispielsweise können Sie von SAML 2.0 SAML 1.1 zuordnen, wenn Ihre Anwendung nur SAML 1.1 Ansprüche unterstützt. 

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

SQL Server oder die interne Windows-Datenbank (AID) können Sie ADFS Konfigurationsinformationen. Aid bietet einfache Redundanz. Während andere Server Pull-Replikation verwenden, um ihre Datenbanken Stand werden direkt an einen der ADFS-Datenbanken im Cluster ADFS Änderungen geschrieben. Mit SQL Server können vollständige Redundanz und Failover clustering oder Spiegelung mit hoher Verfügbarkeit bereitstellen.

## <a name="security-considerations"></a>Sicherheitsaspekte

ADFS verwendet das HTTPS-Protokoll, um sicherzustellen, dass NSG Regeln für das Subnetz mit Web VMs zulassen HTTPS-Anfragen tier. Diese Anfragen können vom lokalen Netzwerk Subnetze mit der Webebene, Geschäftsebene Datenebene, private DMZ, öffentliche DMZ und das Subnetz mit ADFS-Server stammen.

Verwenden Sie einen Satz von virtuellen Netzwerkgeräte, der Informationen zum Verkehr durch die Kante des virtuellen Netzwerks Überwachungszwecken protokolliert.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Aus dem Dokument [Planen der Bereitstellung von ADFS]zusammengefasst Folgendes[plan-your-adfs-deployment], geben Sie einen Ausgangspunkt für ADFS Farmen anpassen:

- Haben Sie weniger als 1000 Benutzer dedizierte ADFS-Server nicht erstellen, aber auf jedem Server fügt in der Cloud ADFS installieren (Stellen Sie sicher, dass mindestens zwei fügt Server auf Verfügbarkeit). Erstellen eines einzelnen WAP-Servers.

- Haben Sie zwischen 1000 und 15000 Benutzer, erstellen Sie zwei dedizierte ADFS-Server und zwei dedizierte WAP-Server.

- Haben Sie zwischen 15000 und 60.000 Benutzer zwischen drei und fünf dedizierte ADFS-Server und mindestens zwei dedizierte WAP-Server erstellen.

Diese Zahlen gehen, Sie verwenden zwei Quad-Core-VMs (Standard D4_v2 oder besser) Servern in Azure.

Beachten Sie, dass wenn Sie die Windows Internal Database ADFS-Konfigurationsdaten verwenden, auf acht ADFS-Server in der Farm. Wenn Sie brauchen mehr erwarten, verwenden Sie SQL Server. Weitere Informationen finden Sie in [Der Rolle der ADFS-Konfigurationsdatenbank][adfs-configuration-database].

## <a name="management-considerations"></a>Gesichtspunkte

DevOps Mitarbeiter sollten bereit sein, die folgenden Aufgaben ausführen:

- Verwalten von Verbundservern (Verwalten der ADFS-Farm, Vertrauensrichtlinie Verbundservern, und die Zertifikate von Federation Services).

- (Verwalten der WAP-Farm Verwalten von Zertifikaten WAP) WAP-Server verwalten.

- Verwalten von Webapplikationen (Konfigurieren von vertrauenden Seiten, Authentifizierungsmethoden und Ansprüche Zuordnung).

- Sichern von ADFS-Komponenten.

## <a name="monitoring-considerations"></a>Aspekte der Überwachung

[Microsoft System Center Management Pack für Active Directory Federation Services 2012 R2] [ oms-adfs-pack] bietet proaktive und reaktive Überwachung der ADFS-Bereitstellung für den Verbundserver. Dieses Management Pack überwacht:

- Ereignisse, die ADFS-Einträge in den Ereignisprotokollen ADFS service.

- Die Leistungsdaten, die ADFS-Leistungsindikatoren erfassen. 

- Der Gesamtzustand ADFS System- und Anwendung (relying Parties) und Alarme für kritische Probleme und Warnungen.

## <a name="solution-components"></a>Lösungskomponenten

Eine Lösung Beispielskript [Bereitstellen ReferenceArchitecture.ps1][solution-script], steht zur Verfügung, mit die Architektur implementiert, die Vorschläge in diesem Artikel beschriebenen folgt. Dieses Skript nutzt Azure Resource Manager Vorlagen. Die Vorlagen werden als eine Reihe von grundlegender Bausteine, von die jede eine bestimmte Aktion wie ein VNet erstellen oder Konfigurieren einer NSG führt. Das Skript soll Bereitstellung koordinieren.

Die Vorlagen werden mit den Parametern, die als separate JSON-Dateien parametrisiert. Sie können die Parameter in diesen Dateien die Bereitstellung Ihren Bedürfnissen konfigurieren. Sie müssen nicht die Vorlagen selbst zu ändern. Beachten Sie, dass Sie die Schemas der Objekte in Parameterdateien nicht ändern müssen.

Wenn Sie die Vorlagen bearbeiten, erstellen Sie Objekte, die [Namenskonventionen für Azure Ressourcen sollten]gemäß Namenskonventionen[naming-conventions].

Die Beispielprojektmappe erstellt und konfiguriert die Umgebung in der Cloud aus fügt Subnetz und Server ADFS Subnetz und Server, ADFS Proxy Subnetz und Server, DMZ Webebene, Business-Tier und Datenzugriffskomponenten Tier, VPN-Gateway und Verwaltungsebene. Die Beispielprojektmappe enthält auch eine optionale Konfiguration zum Erstellen einer lokalen simulierten Umgebung.

Die folgenden Abschnitte beschreiben die Elemente der lokalen und cloud-Konfigurationen.

### <a name="on-premises-components"></a>Lokalen Komponenten

>[AZURE.NOTE] Diese Komponenten stehen nicht in diesem Dokument beschriebenen Architektur und dienen lediglich die Cloud-Umgebung testen, anstatt einer echten produktiven Umgebung geben. Aus diesem Grund werden in diesem Abschnitt nur die Schlüsselparameter Dateien zusammengefasst. Wie die IP-Adressen oder die Größe der VMs können es jedoch viele weitere Parameter unverändert lassen.

Diese Umgebung besteht aus einer AD-Gesamtstruktur für eine Domäne mit dem Namen contoso.com. Die Domäne enthält zwei fügt Server mit IP-Adressen 192.168.0.4 und 192.168.0.5. Diese zwei Server auch den DNS-Dienst. Das lokale Administratorkonto auf beiden virtuellen Computern heißt `testuser` mit Kennwort `AweS0me@PW`. Darüber hinaus richtet die Konfiguration ein VPN-Gateway zur Verbindung mit den VNet in der Cloud. Ändern Sie die Konfiguration bearbeiten die folgenden JSON Dateien im [**Parameter-Onpremise**] [ on-premises-folder] Ordner:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Diese Datei definiert den Netzwerkadressbereich lokalen Umgebung.

- ** [VirtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Diese Datei enthält die Konfiguration der lokalen VMs fügt Hostingdienste. Standardmäßig werden zwei *Standard-DS3-v2* -VMs erstellt.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** und ** [connection.parameters.json][on-premises-connection-parameters]**. Diese Dateien enthalten die Einstellungen für die VPN-Verbindung Azure VPN-Gateway in der Cloud, einschließlich der freigegebenen Schlüssel zum Schutz des Datenverkehrs durch das Gateway.

Die verbleibenden Dateien im Ordner enthalten die Konfigurationsinformationen zum Erstellen der lokalen Domäne dieser Infrastruktur. Sie verwenden sie fügt installieren DNS einrichten, erstellen Sie eine Gesamtstruktur und Konfigurieren der Replikation Sites für die Gesamtstruktur.

### <a name="cloud-components"></a>Cloud-Komponenten

Diese Komponenten bilden den Kern dieser Architektur. [**Parameter-Azure**] [ azure-folder] Ordner enthält die folgenden Parameterdateien für diese Komponenten konfigurieren:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Diese Datei definiert die Struktur der VNet für die VMs und andere Komponenten in der Cloud. Es enthält Eigenschaften wie Name, Adressraum, Subnetze und die Adressen der DNS-Server erforderlich. Beachten Sie, dass die DNS-Adressen in diesem Beispiel die IP-Adressen des lokalen DNS-Server sowie Standard Azure DNS-Server verweisen. Ändern Sie diese Adressen Ihrer eigenen DNS-Konfiguration verweisen, wenn Sie Beispiel lokalen Umgebung nicht verwenden:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
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
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [VirtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Diese Datei konfiguriert die VMs fügt in der Cloud ausgeführt. Die Konfiguration umfasst zwei VMs. Ändern Sie den Benutzernamen Administrator und das Kennwort in der `virtualMachineSettings` Abschnitt, und Sie können optional die VM Größe an Bereich anpassen:

    Weitere Informationen finden Sie unter [Erweitern von Active Directory in Azure][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
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
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [hinzufügen-Fügt-Domain-controller.parameters.json][add-adds-domain-controller-parameters]**. Diese Datei enthält die Einstellungen für das Erstellen der Domäne CONTOSO über Server fügt. Es verwendet benutzerdefinierte Erweiterung, die die Domäne eingerichtet und fügt Server hinzufügen. Sofern weitere fügt Server erstellen (in diesem Fall sie hinzufügen sollten die `vms` Array), ihren Namen von der Standardeinstellung ändern und eine Domäne unter einem anderen Namen erstellen Sie diese Datei ändern müssen möchten.

- ** [LoadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Die Datei enthält zwei Gruppen von Konfigurationsparameter. Die `virtualMachineSettings` Abschnitt definiert die VMs, die ADFS-Dienst in der Cloud zu hosten. Erstellt das Skript zwei VMs in der gleichen Verfügbarkeit:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
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
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    Die `loadBalancerSettings` Abschnitt beschreibt den Lastenausgleich für diese VMs. Lastenausgleich übergibt von VMs Datenverkehr auf Port 443 (HTTPS):

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [Adfs-Farm-Domain-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Diese Datei enthält die Einstellungen der Domäne CONTOSO ADFS-Server hinzufügen. Müssen nur diese Datei ändern, wenn Sie zusätzliche ADFS-Server erstellt haben (Aktualisieren der `vms` in diesem Fall array), oder der Domänenname geändert haben.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [Adfs-Farm-first.parameters.json][adfs-farm-first-parameters]**, und ** [Adfs-Farm-rest.parameters.json][adfs-farm-rest-parameters]**. Das Skript verwendet die Einstellungen in diesen Dateien die ADFS-Serverfarm erstellen. 

    Die Datei *gmsa.parameters.json* enthält die Einstellungen für ADFS-Dienst verwendete Dienstkonto Gruppe verwaltet. Sie können diese Datei den Namen des Kontos oder der Domäne ändern möchten.

    *Adfs-Farm-first.parameters.json* -Datei enthält die ADFS-Serverfarm erstellen und Hinzufügen des ersten Servers erforderlichen Informationen. Wenn Sie die Domäne oder den Namen des Dienstkontos Gruppe verwaltet geändert haben, sollten Sie diese Datei aktualisieren.

    *Adfs-Farm-rest.parameters.json* Datei dient der Farm verbleibenden ADFS-Server hinzufügen. Wenn Sie die Domäne oder den Namen des Dienstkontos Gruppe verwaltet geändert haben, sollten Sie auch diese Datei aktualisieren. Aktualisierung der `vms` array, wenn Sie zusätzliche ADFS-Server erstellt haben.

- ** [LoadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Diese Datei ähnelt in Struktur und Inhalt der *LoadBalancer adfs.parameters.json* Datei. Sie enthält Daten ADFS-Proxyserver und Lastenausgleich.

- ** [Adfsproxy-Farm-first.parameters.json][adfsproxy-farm-first-parameters]**, und ** [Adfsproxy Farm rest.parameters.json][adfsproxy-farm-rest-parameters]**. Das Skript verwendet die Einstellungen in diesen Dateien ADFS Proxy Serverfarm erstellen. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Diese Datei enthält die zum Erstellen von Azure VPN-Gateway in der Cloud zum lokalen Netzwerk herstellen. Sollten Sie ändern die `sharedKey` Wert der `connectionsSettings` Abschnitt mit lokalen VPN-Gerät übereinstimmt. Weitere Informationen finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** und ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Diese Dateien Konfigurieren eingehender (öffentlich) und ausgehenden (privat) VMs, die DMZ Schutz von Servern in der Cloud umfassen. Weitere Informationen über diese Elemente und deren Konfiguration finden Sie unter [Implementieren einer DMZ zwischen Azure und dem Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [LoadBalancer web.parameters Json][loadBalancer-web-parameters]**, ** [LoadBalancer biz.parameters Json][loadBalancer-biz-parameters]**, und ** [LoadBalancer data.parameters Json][loadBalancer-data-parameters]**. Diese Parameter-Dateien enthalten die VM-Spezifikationen für Ebenen Access Web, Geschäfts- und und Lastenausgleich für jede Ebene konfigurieren. Dies sind die VMs, die Web-apps und Datenbanken hosten und die Arbeitslasten in Ihrem Unternehmen für die Organisation ausführen. Sie können Größe und Anzahl der VMs pro Ebene entsprechend ändern.

- ** [VirtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Diese Datei enthält die Konfiguration für Sprung Feld/Verwaltung virtueller Computer. Es kann nur zu Anmeldung und Administratorzugriff auf die VMs im Web, Geschäfts- und Datenebenen aus springen. Standardmäßig erstellt das Skript einen einzelnen *Standard_DS1_v2* VM, aber diese Datei größer oder weitere virtuelle Computer erstellen, wird der Verwaltungsaufwand erheblich zu ändern.

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
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Bearbeiten der Parameter Parameter/Scripts/Onpremise und Ordner Skripts, Parameter/Azure. Aktualisieren der Gruppenverweise in diesen Dateien entsprechend die Namen von Ressourcengruppen zugewiesenen Variablen in der Datei ReferenceArchitecture.ps1 bereitstellen. Die folgende Tabelle zeigt, welche Parameterdateien der Ressourcengruppe verwiesen. Beachten Sie, dass *Ra Adfs-Arbeitslast Rg*, *Ra Adfs-Sicherheit Rg* *Ra Adfs fügt Rg*, *Ra-Adfs-Adfs-Rg*und *Ra Adfs-Proxy Rg* -Gruppen nur in der PowerShell-Skript verwendet und nicht in Parameterdateien treten.

  	|Ressourcengruppe|Parameter-Dateien|
  	|--------------|--------------|
  	|RA-Adfs-Onpremise-rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|RA-Adfs-Netzwerk-rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-private.Parameters.JSON<br />parameters\azure\dmz-public.Parameters.JSON<br />parameters\azure\loadBalancer-ADFS.Parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.Parameters.JSON<br />parameters\azure\loadBalancer-biz.Parameters.JSON<br />parameters\azure\loadBalancer-Data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*zweimal*)

    Außerdem legen Sie die Konfiguration für die lokale und cloud-Komponenten, wie im Abschnitt Lösungskomponenten [Lösungskomponenten].

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

    - `AdfsVm`ADFS-VMs und fügen sie die Domäne in der Cloud.

    - `ProxyVm`ADFS Proxy VMs erstellen, und fügen sie die Domäne in der Cloud.

    - `Prepare`, das die oben genannten Aufgaben ausführt. **Dies ist empfohlen, wenn Sie eine neue Bereitstellung erstellen, und Sie eine vor-Ort-Infrastruktur müssen.**

    >[AZURE.NOTE] Sie können auch das Skript mit einer `<mode>` Parameter `Workload` Web, Unternehmen und Datenebene VMs und Netzwerk erstellen. Dieses Setup ist nicht Bestandteil der `Prepare` Modus.

    Verwenden Sie die `Prepare` Option des Skripts dauert mehrere Stunden und endet mit der Nachricht *Vorbereitung abgeschlossen. Zertifikat für alle ADFS und Proxy VMs installieren.*

8.  Neu (*Ra Adfs-Mgmt vm1* *Ra Adfs-Sicherheit Rg* -Gruppe) im Feld Gehe zu starten, um die DNS-Einstellungen zu ermöglichen.

9.  [Rufen Sie ein SSL-Zertifikat für ADFS] [ adfs_certificates] und dieses Zertifikat auf VMs ADFS installieren. Beachten Sie, dass ADFS VMs mithilfe von Jump hergestellt werden kann. Die IP-Adressen sind *10.0.5.4* und *10.0.5.5*. Der Standardbenutzername ist *Contoso\testuser* mit Kennwort *AweSome@PW*.

    >[AZURE.NOTE] Kommentare im Skript bereitstellen ReferenceArchitecture.ps1 zu diesem Zeitpunkt erhalten Sie detaillierte Informationen für ein selbstsigniertes Testzertifikat und Autorität mit der `makecert` Befehl. Verwenden Sie die Zertifikate Makecert in einer produktiven Umgebung jedoch nicht.

10. Führen Sie den folgenden Powershell-Befehl ADFS-Serverfarm konfigurieren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Suchen Sie im Feld Gehe zu auf *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* zum Testen der ADFS-Installation (Sie erhalten möglicherweise eine zertifikatswarnung für diesen Test zu ignorieren). Überprüfen Sie, ob die Contoso Corporation-Seite angezeigt wird. Melden Sie sich als *Contoso\testuser* mit Kennwort *AweS0me@PW*.

12. Installieren Sie das SSL-Zertifikat für ADFS Proxy VMs. Die IP-Adressen sind *10.0.6.4* und *10.0.6.5*.

13. Führen Sie den folgenden Powershell-Befehl den ersten ADFS Proxyserver konfigurieren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Befolgen Sie die Anweisungen vom Skript zum Testen der Installation des ersten ADFS-Proxyservers.

15. Führen Sie den folgenden Powershell-Befehl zum zweiten ersten ADFS-Proxyserver konfigurieren:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Anweisungen Sie angezeigt, indem das Skript die gesamte ADFS Proxy-Konfiguration zu testen.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Active Directory][aad].

- [Azure Active Directory B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Sichere Hybrid-Netzwerkarchitektur mit Active Directory"
