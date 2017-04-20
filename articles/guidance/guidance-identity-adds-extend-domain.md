<properties
   pageTitle="Azure-Architektur Referenz - IaaS: Erweitern von Active Directory in Azure | Microsoft Azure"
   description="Zum Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Active Directory-Autorisierung in Azure."
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
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Erweitern Sie Active Directory-Verzeichnisdienst (ADDS) in Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel beschreibt Empfohlene Verfahrensweisen zur Erweiterung Ihrer Umgebung Active Directory (AD) Azure verteilte Authentifizierung mit [Active Directory-Domänendienste (AD DS)]bereitstellen[active-directory-domain-services]. Diese Architektur erweitert die [Implementierung einer sicheren Hybrid-Netzwerkarchitektur in Azure] Artikel beschrieben[ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. Diese Referenzarchitektur verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

Verwenden Sie AD DS Identitäten zu authentifizieren. Diese Identitäten können Benutzer, Computer, Programme oder andere Ressourcen, die Teil einer Sicherheitsdomäne angehören. AD DS lokalen host, aber im Hybrid-Szenario Elemente einer Anwendung in Azure befinden möglich effizienter, diese Funktionalität und AD-Repository in die Cloud zu replizieren. Dieser Ansatz kann verringern die Verzögerung durch das Senden von Authentifizierung und lokale Autorisierungsanfragen aus der Cloud zurück in AD DS lokal ausgeführt. 

Es gibt zwei Arten der Verzeichnisdienste in Azure gehostet:

1. [Azure Active Directory (AAD)] können[ azure-active-directory] eine neue Active Directory-Domäne in der Cloud erstellen und verknüpfen Sie es mit einem lokalen Active Directory-Domäne. Setup wird [Azure AD Connect] [ azure-ad-connect] Versprechen Identitäten in der Replikation der lokalen Repository in die Cloud. Beachten Sie, dass das Verzeichnis in der Cloud **nicht** Teil des lokalen Systems, sondern ist eine, die die gleiche Identität enthält. Auf diese Identitäten Cloud jedoch Änderungen in der Cloud **nicht** lokal kopiert werden werden, die lokale Domäne repliziert. Beispielsweise müssen Kennwörtern lokal ausgeführt und Azure AD verbinden die Änderung in die Cloud zu kopieren. Beachten Sie, dass dieselbe Instanz des AAD mehrere Instanzen von AD DS verknüpft werden kann; AAD enthält die Identität jedes Repository AD, verknüpft ist.

    AAD eignet sich für Situationen, in dem lokalen Netzwerk und Azure virtuelle Netzwerk Cloudressourcen nicht direkt verknüpft sind mit einem VPN-Tunnel oder ExpressRoute-Verbindung. Obwohl diese Lösung einfach ist es möglicherweise nicht für Systeme, konnte Komponenten lokal und Cloud-Grenze migrieren, wie diese Neukonfiguration AAD erfordern. Außerdem behandelt AAD nur Authentifizierung als Authentifizierung. Einige Programme und Dienste wie SQL Server-Authentifizierung erfordern die Lösung nicht geeignet ist.

2. Sie können einen virtuellen Computer als Domänencontroller in Azure AD DS ausgeführt, Erweiterung der vorhandenen Active Directory-Infrastruktur von lokalen Datencenter bereitstellen. Dieser Ansatz ist für Szenarios, in dem lokalen Netzwerk und Azure virtual Network VPN- oder ExpressRoute-Verbindung verbunden sind. Diese Lösung unterstützt auch die bidirektionale Replikation aktivieren Sie Änderungen in der Cloud und lokal am besten geeignet ist. Je nach Ihrer Sicherheitserfordernisse kann die AD-Installation in der Cloud:
    - Teil derselben Domäne wie dieser statt lokal
    - eine neue Domäne in eine gemeinsame Gesamtstruktur
    - eine separate Gesamtstruktur

<!-- we might want to add info on how to choose from the three options above -->

Diese Architektur Schwerpunkt Lösung 2 gleiche AD DS-Domäne in Azure und lokalen.

Normale Anwendungsfälle für diese Architektur gehören:

- Hybrid Applications, Arbeitslasten Ausführen teilweise lokalen, und teilweise in Azure.

- Programme und Dienste, die Authentifizierung mithilfe von Active Directory ausführen.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm zeigt wichtigen Komponenten in dieser Architektur (*zum Vergrößern klicken*). Weitere Informationen über die Elemente abgeblendet finden Sie unter [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure] [ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Lokalen Netzwerk.** Das lokale Netzwerk enthält lokale AD-Servern, die Authentifizierung und Autorisierung für Komponenten lokal ausführen können.

- **AD-Server.** Diese sind Domänencontroller Directory Services (AD DS) als VMs in der Cloud zu implementieren. Diese Server können Komponenten in Azure virtuelle Netzwerk-Authentifizierung bereitstellen.

- **Active Directory-Subnetz.** Die AD DS-Server befinden sich in einem separaten Subnetz. NSG Regeln schützen die AD DS-Server und eine Firewall vor Datenverkehr von unerwarteten Quellen erhalten.

- **Synchronisierung Azure-Gateways und AD.**. Azure-Gateway stellt eine Verbindung zwischen dem lokalen Netzwerk und Azure-VNet. Dies ist eine [VPN-Verbindung] [ azure-vpn-gateway] oder [Azure ExpressRoute][azure-expressroute]. Alle Synchronisierungsanfragen zwischen den AD-Servern in der Cloud und lokal übergeben über das Gateway. Benutzerdefinierte Routen (Decision) behandeln routing für Synchronisierung Datenverkehr direkt an den AD Server in der Cloud und nicht über das vorhandene Netzwerk virtuelle Appliances (NVAs) in diesem Szenario verwendeten übergeben.

    Weitere Informationen zum Konfigurieren von Decision und der NVAs finden Sie unter [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Empfehlung

Dieser Abschnitt enthält eine Zusammenfassung für die Implementierung von AD DS in Azure für:

- VM Recommendations.

- Netzwerk empfohlen.

- Sicherheitsaspekte. 

- Active Directory-Standort empfohlen.

- Active Directory FSMO-Platzierung Recommendations.

>[AZURE.NOTE] [Richtlinien für die Bereitstellung von Windows Server Active Directory in Azure Virtual Machines] Dokument[ ad-azure-guidelines] enthält ausführlichere Informationen über viele dieser Punkte.

### <a name="vm-recommendations"></a>VM-Empfehlung

VMs mit ausreichenden Ressourcen, um die erwartete Menge Datenverkehr erstellen. Verwenden Sie die Anzahl der Computer, auf denen AD DS lokal als Ausgangspunkt. Überwachen der Ressourcenverwendung. die VMs Größe können und skalieren, sind zu groß. Weitere Informationen zum Anpassen von AD DS-Domänencontrollern finden Sie unter [Planung für Active Directory-Domänendienste][capacity-planning-for-adds].

Erstellen Sie einen separaten virtuellen Datenträger zum Speichern von Datenbank, Protokollen und SYSVOL für Active Directory. Speichern Sie diese Elemente nicht auf derselben Festplatte wie das Betriebssystem. Beachten Sie, dass standardmäßig einer VM hängen Datenträger Durchschreibcache. Diese Art des Zwischenspeicherns kann jedoch an AD DS Konflikt. Aus diesem Grund wird *Host Cache* -Einstellung auf dem Datenträger *keine*fest. Weitere Informationen finden Sie unter [Platzierung von Windows Server AD DS-Datenbank und SYSVOL][adds-data-disks].

Bereitstellen von Azure virtuellen Netzwerks [Verfügbarkeit](#Availability-considerations) aus AD DS-Domänencontrollern mit mindestens zwei VMs.

### <a name="networking-recommendations"></a>Netzwerk-Empfehlung

Konfigurieren der Netzwerkschnittstelle für jede VMs hosten AD DS statische private IP-Adressen. Diese Konfiguration unterstützt DNS besser auf die AD-VMs. Weitere Informationen finden Sie unter [eine statische private IP-Adresse im Azure-Portal][set-a-static-ip-address].

> [AZURE.NOTE] Geben Sie die öffentlichen IP-Adressen von AD DS VMs. [Sicherheit] Siehe[ security-considerations] Weitere Informationen.

Sie müssen sicherstellen, dass Datenverkehr zwischen den AD-Servern in der Cloud und lokalen frei fließen kann:

- AD-Subnetz, das lokal eingehenden Datenverkehr zulassen fügen Sie NSG Regeln hinzu. Detaillierte Informationen über die Ports, die AD DS verwenden, finden Sie unter [Active Directory und Active Directory Domain Services Port erforderlich][ad-ds-ports].

- Stellen Sie sicher, UDR Tabellen nicht AD DS in diesem Szenario verwendeten NVAs Datenverkehr weiterleiten. Eigene Bereitstellung, je nachdem, welche NVAs Sie verwenden, sollten Sie Datenverkehr umleiten.

### <a name="security-recommendations"></a>Sicherheitsaspekte

AD DS-Server Authentifizierung behandelt und sind daher sehr empfindlich Elemente. Vermeiden Sie direkte AD DS-Server im Internet. Platzieren Sie AD DS-Server in einem separaten Subnetz mit eigenen Firewall. Sicherzustellen Sie, dass AD DS für Authentifizierung und Autorisierung und Synchronisation Server verwenden Ports geöffnet bleiben, schließen Sie alle anderen Ports. Weitere Informationen finden Sie unter [Active Directory und Active Directory Domain Services Port erforderlich][ad-ds-ports]. Die [Komponenten der Lösung] [ solution-components] weiter unten in diesem Dokument wird die NSG Regeln die Beispielprojektmappe verwendet, um die Ports zu öffnen.

NSG Regeln können Sie eine einfache Firewall erstellen. Umfassenderen Schutz benötigen Sie können implementieren zusätzliche Sicherheitszaun Servern mit einem Subnets und NVAs, wie das [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure]Dokument[implementing-a-secure-hybrid-network-architecture-with-internet-access].

Verwenden Sie BitLocker oder Azure datenträgerverschlüsselung zum Verschlüsseln des Datenträger mit der AD DS-Datenbank.

### <a name="active-directory-site-recommendations"></a>Active Directory-Standort empfohlen

Standorte in AD DS Gruppe zusammen Domänencontroller können, die eine schnelle Verbindung verbunden sind. Domänencontroller am gleichen AD DS-Standort replizieren ihre Verzeichniswechsel automatisch und wenig Konfiguration muss die Replikation behandeln.

Replikationsverkehr zwischen Azure und lokalen Datencenter zu steuern, wird empfohlen, separaten AD DS-Standort zum Darstellen von Azure verwendet Adressraum hinzufügen. Konfigurieren Sie eine standortverknüpfung zwischen Ihrem lokalen AD DS sites und standortübergreifende Replikation besser steuern.

Site Trennung können Sie verschiedene Gruppenrichtlinienobjekte (GPOs) zu Computern in Azure und Programme nutzen, die "wissen", wie System Center Configuration Manager sind anzuwenden.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO-Platzierung recommendations

Flexible Single Master Operation (FSMO) Server sind spezielle Domänencontroller Datenkonsistenz über unterschiedliche Einstellungen in AD DS aufgeführten Reposnsible.

- **Schemamaster**. Dies ist eine gesamtstrukturweite Rolle, die die Struktur des Schemas von AD DS verwendet verwaltet.

- **Der Domänennamenmaster**. Dies ist eine gesamtstrukturweite Rolle, die Informationen über den Domänennamen in der Gesamtstruktur verwaltet.

- **Primäre Domänencontroller (PDC)**. Dies ist eine domänenweite Rolle. Der PDC behandelt Kennwort Updates und Kontensperrungen. Er wird von anderen DCs gehört bei Serviceanfragen Authentifizierung Kennwörter stimmen nicht überein. Der PDC Gruppenrichtlinien Updates behandelt auch das Ziel DC für Legacyanwendungen und einige Verwaltungstools, die einige beschreibbaren Operationen durchführen.

- **Relativen ID (RID) Master**. RIDs wird Hilfe Objekte innerhalb eines Verzeichnisses eindeutig zu identifizieren. Dieser Server ist einen Pool domänenweiten RID für die Verwendung von AD-Servern in der Domäne generiert.

- **Funktion der Infrastruktur**. Ein Objekt in einer Domäne kann ein Objekt in einer anderen Domäne verweisen. Diese Rolle domänenweite ist verantwortlich für die Aktualisierung des Objekts SID und DN in einer domänenübergreifenden Referenz. Beachten Sie, dass ein Server implementieren diese Rolle auch als globalen Katalog (GC) Server handeln kann.

Weitere Informationen finden Sie unter [Active Directory FSMO-Funktionen in Windows][AD-FSMO-roles-in-windows].

Für dieses Szenario sollten Sie vermeiden FSMO-Funktionen auf die Domänencontroller in Azure bereitstellen. 

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Erstellen Sie eine Verfügbarkeit für den AD-Servern festgelegt. Stellen Sie sicher, dass mindestens zwei Server in der Gruppe. AD-Servern in der Cloud sollten Domänencontroller innerhalb derselben Domäne sein. Damit wird die automatische Replikation zwischen Servern.

Bedenken Sie auch, Festlegen von Servern als [Standby-Betriebsmaster] [ standby-operations-masters] bei Verbindung zu einem Server als FSMO-Funktion fehlschlägt.

## <a name="security-considerations"></a>Sicherheitsaspekte

Wenn alle Domäne Admin wahrscheinlich mit den lokalen Domänencontrollern, erwägen Sie DCs in der Cloud schreibgeschützt. Schreibgeschützter DC nur eine Teilmenge der Benutzeranmeldeinformationen (ausreichend für Authentifizierung lokal) verwaltet und Cache-Informationen nur für bestimmte Benutzer konfiguriert werden. Daher ist ein schreibgeschützter DC gefährdet, die Informationen für zwischengespeicherte Benutzer, anstatt die Details jedes Konto in der Domäne wirkt. Weitere Informationen finden Sie unter [Schreibgeschützten Domänencontroller][read-only-dc].

Schwachstelle einzelner Benutzerkonten zu minimieren und versucht Einbruch abzuschrecken folgen Sie empfohlen und Verwalten von Benutzern in Active Directory Weitere Informationen finden Sie unter [Best Practices für die Durchsetzung von Kennwortrichtlinien][best_practices_ad_password_policy]. Auch darauf achten, welche Gruppen Benutzer zuweisen. Stellen Sie z. B. nicht normale Benutzer Mitglied der Gruppe Organisations-Admins Gruppe Schema-Admins und Domänen-Admins.

## <a name="scalability-considerations"></a>Aspekte der Skalierung

AD ist automatisch skalierbar für Domänencontroller, die zu derselben Domäne gehören. Anfragen werden auf allen Domänencontrollern in einer Domäne verteilt. Sie können einen anderen Domänencontroller und automatisch mit der Domäne synchronisiert wird. Konfigurieren Sie ein separates Lastenausgleich direkter Datenverkehr auf Domänencontroller in der Domäne. Sicherzustellen Sie, dass alle Domänencontroller über genügend Arbeitsspeicher und Speicherressourcen Domänendatenbank behandelt; Idealerweise stellen Sie alle Domänencontroller VMs dieselbe Größe.

## <a name="management-considerations"></a>Gesichtspunkte

Kopieren Sie die VHD Dateien Domänencontroller anstatt regelmäßige Backups, da Wiederherstellung zwischen Domänencontrollern zu Inkonsistenzen führen kann.

Beenden Sie und starten Sie einen virtuellen Computer, die Domänencontroller im Gastbetriebssystem anstatt die Option Herunterfahren im Azure-Portal in Azure ausgeführt wird. Azure-Portal einen virtuellen Computer herunterfahren, wird die VM freigegeben werden. Diese Aktion setzt VM GenerationID ist ein Domänencontroller nicht erwünscht. Beim Zurücksetzen der VM-GenerationID Aufrufkennung AD-Repository auch Zurücksetzen der RID-Pool verworfen und SYSVOL als nicht autorisierend gekennzeichnet.

## <a name="monitoring-considerations"></a>Aspekte der Überwachung

Zu überwachen und unterhalten Netzwerk AD-Server kann Probleme wie auftreten:

- **Anmeldung fehlgeschlagen**. Anmeldefehler kann in der gesamten Domäne oder Gesamtstruktur auftreten, fällt eine vertrauenswürdige Beziehung oder Namen Auflösung oder ein globalen Katalogserver Mitgliedschaft bestimmen kann.

- **Konto gesperrt**. Benutzer- und Dienstkonten können gesperrt werden, wenn der primäre Domänencontroller nicht verfügbar ist oder die Replikation zwischen mehreren Domänencontrollern schlägt fehl.

- **Domänencontroller-Fehler**. Wenn das Laufwerk mit der Datei Ntds.dit Speicherplatz ausgeführt wird, kann der Domänencontroller funktionieren nicht mehr.

- **Anwendungsfehler**. Programme, die für Ihr Unternehmen wie Microsoft Exchange oder eine andere e-Mail-Anwendung können fehlschlagen, wenn Adresse Adressbuch in das Verzeichnis Abfragen.

- **Inkonsistente Daten**. Wenn längere Zeit Replikation fehlschlägt, doppelte Objekte im Verzeichnis erstellt werden und erfordern umfangreiche Diagnose- und Zeit zu.

- **Fehler beim Erstellen des Kontos**. Ein Domänencontroller kann Benutzer- oder Computerkonten erstellen, wenn seine Versorgung mit relativen IDs erschöpft und RID-Master nicht verfügbar ist.

- **Sicherheit-Richtlinienfehler**. Wenn der freigegebene Ordner SYSVOL nicht ordnungsgemäß repliziert wird, werden Gruppenrichtlinienobjekte und Sicherheitsrichtlinien nicht ordnungsgemäß Clients angewendet.

AD-Server sorgfältig überwachen und schnell korrigierend bereit. Erstellen Sie eine Prüfliste der Überwachungsaufgaben zu einem geeigneten Zeitpunkt ausgeführt werden. Beispielsweise können Sie die folgenden wichtigen Aufgaben täglich planen:

- Prüfen und Alarme gemeldet von Domänencontrollern 

- Überprüfen Sie alle Domänencontroller kommunizieren und die Replikation funktioniert.

- Stellen Sie sicher, dass SYSVOL freigegebene.

- Beheben Sie Synchronisierung Probleme.

- Überprüfen Sie Speicherplatz auf physischen Laufwerken AD verwendet.

Sie können andere weniger häufig Routineaufgaben. Angenommen, Sie Vertrauensstellungen überprüfen und veraltete oder fehlerhafte Vertrauensstellungen wöchentlich überprüfen und überprüfen, ob alle Domänencontroller mit denselben Servicepacks und Hotfixes Patches monatlich ausgeführt werden.

Weitere Informationen finden Sie unter [Monitoring Active Directory][monitoring_ad]. Sie können Tools wie [Microsoft Systems Center] [ microsoft_systems_center] auf dem überwachenden Server ( [Architekturdiagramm][architecture]) können Sie diese Aufgaben ausführen. 

## <a name="solution-components"></a>Lösungskomponenten

Die Lösung für diese Architektur eines sicheren Hybrid erstellt durch [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure] Dokumente wie[ implementing-a-secure-hybrid-network-architecture] und [Implementieren einer sicheren Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], mit folgenden Elementen:

- Eine Azure-Ressourcengruppe mit dem Namen *Basename*- Dns-Rg, wobei *Basisname* ein Präfix ist angeben, wenn die Lösung bereitstellen.

- Zwei Azure VMs aufgerufen *Basename*-ad1-Vm und *Basename*-ad2-Vm *Basename*Dns - Rg-Ressourcengruppe erstellt. Die VMs werden als AD Server DNS installiert und konfiguriert mit Verzeichnisdiensten konfiguriert.

- Eine NSG namens *Basename*-Ad-Nsg *Basename*- Ntwk Rg Azure-Ressourcengruppe. Diese Ressourcengruppe ist Bestandteil der Infrastruktur, die sichere Hybrid-Netzwerk bilden, aber neue NSG ist eine Ergänzung, die eingehende Sicherheitsregeln für den AD-Servern wie in der folgenden Tabelle gezeigt definiert:


    Priorität|Name|Quelle|Ziel|Dienst|Aktion|
    --------|----|------|-----------|-------|------|
    170|Vnet port53|10.0.0.0/16|Alle|Custom(Any/53)|Zulassen|
    180|Vnet port88|10.0.0.0/16|Alle|Custom(Any/88)|Zulassen|
    190|Vnet port135|10.0.0.0/16|Alle|Custom(Any/135)|Zulassen|
    200|Vnet bis port137 9|10.0.0.0/16|Alle|Custom(Any/137-139)|Zulassen|
    210|Vnet port389|10.0.0.0/16|Alle|Custom(Any/389)|Zulassen|
    220|Vnet port464|10.0.0.0/16|Alle|Custom(Any/464)|Zulassen|
    230|Vnet Rpc-Dynamik|10.0.0.0/16|Alle|Custom(Any/49152-65535)|Zulassen|
    240|Onprem Ad, port53|192.168.0.0/24|Alle|Custom(Any/53)|Zulassen|
    250|Onprem Ad, port88|192.168.0.0/24|Alle|Custom(Any/88)|Zulassen|
    260|Onprem Ad, port135|192.168.0.0/24|Alle|Custom(Any/135)|Zulassen|
    270|Onprem Ad, port389|192.168.0.0/24|Alle|Custom(Any/389)|Zulassen|
    280|Onprem Ad, port464|192.168.0.0/24|Alle|Custom(Any/464)|Zulassen|
    290|Management-Rdp-zulassen|10.0.0.128/25|Alle|Custom(Any/3389)|Zulassen|
    300|Gateway zulassen|10.0.255.224/27|Alle|Custom(Any/Any)|Zulassen|
    310|selbst zulassen|10.0.255.192/27|Alle|Custom(Any/Any)|Zulassen|
    320|Vnet-verweigern|Alle|Alle|Custom(Any/Any)|Zulassen|

    AD DS verwendet Port 53, 89, 135, 389 und 464 eingehende Replikation und Authentifizierungsdatenverkehr. In dieser Tabelle ist der lokale Domänencontroller Adresse Speicherplatz 192.168.0.0/24 (der Adressraum variieren - diese Angaben als Parameter zu den Vorlagen bereitgestellte Lösung.

    Die NSG definiert auch ausgehende Sicherheit Folgendes die Synchronisierung und Genehmigung Datenverkehr an das lokale Netzwerk ermöglichen:

    Priorität|Name|Quelle|Ziel|Dienst|Aktion|
    --------|----|------|-----------|-------|------|
    100|aus port53|Alle|192.168.0.0/24|Custom(Any/53)|Zulassen|
    110|aus port88|Alle|192.168.0.0/24|Custom(Any/88)|Zulassen|
    120|aus port135|Alle|192.168.0.0/24|Custom(Any/135)|Zulassen|
    130|aus port389|Alle|192.168.0.0/24|Custom(Any/389)|Zulassen|
    140|Out-Port 445|Alle|192.168.0.0/24|Custom(Any/445)|Zulassen|
    150|aus port464|Alle|192.168.0.0/24|Custom(Any/464)|Zulassen|
    160|Out-Rpc-Dynamik|Alle|192.168.0.0/24|Custom(Any/49152-65535)|Zulassen|

Mit der Lösung bereitgestellte Skript auch folgende Aufgaben:

- Fügt die *Basename*-ad1-Vm und *Basename*-ad2-Vm-Server als Domänencontroller in der Domäne. Sie können diese Server in der Konsole *Active Directory-Benutzer und-Computer* auf dem lokalen Domänencontroller anzeigen:

![[1]][1]

- Erstellt ein neues Subnetz (10.0.0.0/16) für eine AD-Standort namens Azure-VNet-Ad-Standort, der Domäne. Diese Website enthält *Basename*-ad1-Vm und *Basename*-ad2-Vm-Servern. 

- Es fügt IP-standortübergreifenden transporteinstellungen, die das Intervall zwischen der lokalen und der Domänencontroller in der Cloud zu konfigurieren. Sie können für das Subnetz, Websites und transporteinstellungen in der Konsole *Active Directory-Standorte und Server* auf dem lokalen Domänencontroller anzeigen:

![[2]][2]

## <a name="deployment"></a>Bereitstellung

Die Beispielprojektmappe enthält die folgenden Prerequsites:

- Die lokale Domäne bereits konfiguriert haben und DNS konfiguriert haben, und installierte Routing- und RAS-Dienste zur Unterstützung eines VPN verbinden Azure VPN-Gateway.


- Die neueste Version der Azure-CLI installiert. [Gehen Sie ausführliche][cli-install].

- Wenn Sie die Lösung von Windows bereitstellen, müssen ein Tool eine Bash Shell wie [GitHub Desktop]installieren[github-desktop].

>[AZURE.NOTE] Haben Sie Zugriff auf eine vorhandene lokale Domäne, können eine Umgebung mit [onpremdeploy.sh] [ onpremdeploy] bash-Skript. Dieses Skript erstellt ein Netzwerk und VM in der Cloud, die eine sehr einfachen lokalen Setup simuliert. Bearbeiten Sie dieses Skript vor dem Ausführen, und legen Sie die folgenden Variablen am Anfang der Datei definiert:
>
> - **BASE_NAME**. Ein benutzerdefiniertes Präfix für die Ressourcengruppe und die VM erstellt das Skript. Dieses Element sollte **nicht mehr als 5 Zeichen** andernfalls das Skript versucht, einen virtuellen Computer mit einem ungültigen Namen generieren und nicht.
> 
> - **Abonnement**. Ihre Azure-Abonnement-ID. Die Ressourcengruppe wird in diesem Abonnement erstellt.
> 
> - **Speicherort**. Der Azure Speicherort in der Ressourcengruppe wie *Eastus* oder *Westus*erstellt.
> 
> - **ADMIN_USER_NAME**. Der Name für das Administratorkonto auf dem virtuellen Computer verwenden.
> 
> - **ADMIN_PASSWORD**. Das Kennwort für das Administratorkonto.

Führen Sie die folgenden Schritte aus, um die Beispielprojektmappe erstellen:

1. Herunterladen und Bearbeiten der [azuredeploy.sh] [ azuredeploy] Skript und am Anfang der Datei die folgenden Parameter festlegen:

    - **BASE_NAME**. Ein benutzerdefiniertes Präfix für Ressourcengruppen und VMs von dem Skript erstellt. Wie zuvor sollte dieses Element **nicht mehr als 5 Zeichen**.

    - **Abonnement**. Ihre Azure-Abonnement-ID.

    - **CPU-Typ**. Das Betriebssystem (*Windows* oder *Linux*) der Zugriffsschicht Web, Geschäfts- und VMs mit. Beachten Sie, dass alle vom Skript erstellten AD-Server Windows Server 2012 unabhängig von dieser Einstellung.

    - **Domänenname**. Der Name der lokalen Domäne. Beachten Sie, dass bei Verwendung die Umgebung durch das onpremdeploy.sh-Skript erstellt diese *contoso.com*muss.

    - **Speicherort**. Der Azure Speicherort, Ressourcengruppen erstellen.

    - **ADMIN_USER_NAME**. Der Name der Administratorkonten in verschiedenen VMs mit.

    - **ADMIN_PASSWORD**. Das Kennwort für das Administratorkonto.

    - **ON_PREMISES_PUBLIC_IP**. Die öffentliche IP-Adresse des lokalen VPN-Computers.

    - **ON_PREMISES_ADDRESS_SPACE**. Den internen Adressbereich des lokalen Netzwerks. Bei Verwendung die Umgebung erstellt das Skript onpremdeploy.sh ist 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. Die gemeinsamen IPSec-Schlüssel für die VPN-Verbindung zwischen dem lokalen Netzwerk und Azure VPN-Gateway verwendet.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Die IP-Adresse des lokalen DNS-Server. Bei Verwendung die Umgebung erstellt das Skript onpremdeploy.sh ist 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Das Adresspräfix des lokalen Subnetzes. Bei Verwendung die Umgebung erstellt das Skript onpremdeploy.sh ist 192.168.0.0/24.

    >[AZURE.NOTE] UM Ressourcen und Zeit zu sparen, erstellt das Skript keine Geschäfts- oder Access-Ebenen. Benötigen diese Elemente können im folgenden Abschnitt azuredeploy.sh Skript entfernen:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Öffnen einer Shell nachlesen und verschieben in den Ordner mit dem Skript azuredeploy.sh.

3. Auf der Azure-Konto anmelden. Geben Sie in der Bash Shell den folgenden Befehl ein:

    ```
    azure login
    ```

    Führen Sie die Schritte zum Herstellen von Azure.

4. Führen Sie den Befehl `./azuredeploy.sh`, und warten Sie, während das Skript die Netzwerkinfrastruktur erstellt.

5. Aufgefordert werden *Überprüfen Sie das VNet erstellt wurde*verwenden Sie Azure-Portal eine Ressourcengruppe mit dem Namen *Basename*- Ntwk-Rg erstellt wurde, und enthält Elemente wie in der folgenden Abbildung dargestellt:

    ![[3]][3]

    >[AZURE.NOTE] In den Beispielen wurde *Basename* *Cloud* festgelegt, beim Ausführen des Skripts.

    *Basename*- Vnet VNet auf, klicken Sie auf *Subnets*und überprüfen Sie, ob die Subnetze unten erstellt wurde:

    ![[4]][4]

6. Drücken Sie bei der Aufforderung in der Bash Shell-Fenster und warten Sie die Webebene und Lastenausgleich erstellt werden.

7. Aufgefordert werden *Überprüfen Sie die Webebene korrekt erstellt wurde*verwenden Sie Azure-Portal namens *Basename*Web-Tier-Rg Ressourcengruppe erstellt wurde, und enthält Elemente wie unten:

    ![[5]][5]

8. Bei der Aufforderung in der Bash Shell-Fenster drücken und die NVAs erstellt werden.

9. Verwenden Sie an die *Stellen Sie sicher, dass die NVA ordnungsgemäß erstellt wurde*Azure-Portal zu um überprüfen, ob eine Ressourcengruppe namens *Basename*- Management-Rg mit folgendem Inhalt erstellt wurde:

    ![[6]][6]

10. Bei Aufforderung im Bash Shell-Fenster drücken und der Jumpbox erstellt wird.

11. Verwenden Sie an die *Stellen Sie sicher, dass die Jumpbox ordnungsgemäß erstellt wurde*Azure-Portal zu um überprüfen, ob Folgendes *Basename*- Mgmt Rg-Ressourcengruppe hinzugefügt wurden:

    - Bezeichnet ein Satz Verfügbarkeit *Basename*- Jb-als.

    - Ein virtueller Computer mit dem Namen *Basename*- Jb-Vm.

    - Eine Schnittstelle namens *Basename*- Jb-Netzwerkkarte

12. Bei der Aufforderung in der Bash Shell-Fenster drücken und Azure VPN-Gateway und Verbindung erstellt werden. Beachten Sie, dass dieser Schritt bis zu 30 Minuten dauern kann.

13. Verwenden Sie bei der Aufforderung *Bitte überprüfen Sie das VPN-Gateway korrekt erstellt wurde*Azure-Portal zu um überprüfen, ob der Ressourcengruppe *Basename*Rg - Ntwk die folgenden Elemente hinzugefügt wurden:

    - Ein lokales Netzwerk-Gateway bezeichnet lokal-Lgw.
    
    - Ein virtuelles Netzwerk-Gateway *Basename*- Vpngw aufgerufen.

    - Gateway-Verbindung namens *Basename*- Vnet-Vpnconn. Beachten Sie der Status dieser Verbindung möglicherweise *nicht verbunden* , wenn der lokale Endpunkt der Verbindung noch nicht konfiguriert haben. Diese werden später behandelt werden.

14. Bei der Aufforderung in der Bash Shell-Fenster drücken und VMs und andere Ressourcen für die DMZ erstellt werden.

15. Notizlisten *überprüfen die DMZ korrekt erstellt wurde*verwenden Sie Azure-Portal zu um überprüfen, ob eine Ressourcengruppe namens *Basename*-dmz-Rg mit folgendem Inhalt erstellt wurde:

    ![[7]][7]

16. Drücken Sie ein bei Aufforderung im Bash Shell-Fenster. Folgenden Ansagen sollte angezeigt werden:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Melden Sie sich beim lokalen Computers Azure Gateway, und konfigurieren Sie die Verbindung entsprechend. Das lokale Gatewaygerät, das Requestsfor Bereich 10.0.0.0/16 über das Gateway auf das VNet leitet fügen Sie statische Routen hinzu. Die Schritte hierzu hängt davon ab, wie Sie eine Verbindung herstellen. Finden Sie unter [Implementieren einer Hybrid-Netzwerkarchitektur Azure mit lokalen VPN-] [ implementing-a-hybrid-network-architecture-with-vpn] Weitere Informationen.

    Beachten Sie, dass die öffentliche IP-Adresse des Azure VPN-Gateways können mithilfe des Azure-Portals zu *Basename*- Vpngw Gateway in der Ressourcengruppe *Basename*Rg - Ntwk:

    ![[8]][8]

    Sie können bestimmen, ob die Verbindung ordnungsgemäß anhand des Status *Basename*- Vnet-Vpnconn-Verbindung hergestellt wurde. Es sollte fest *verbunden*.

    Öffnen Sie eine Remotedesktopverbindung zum Testen der Verbindung zu Jumpbox (10.0.0.254) von einem Computer im lokalen Netzwerk befindet.

17. Drücken Sie ein bei Aufforderung im Bash Shell-Fenster. Aufgefordert, *jede Taste VNet Einstellung für die VNet auf lokalen DNS update*beim nächsten drücken und warten Sie, während die DNS-Clienteinstellungen für die VNet auf den Wert als **ON_PREMISES_DNS_SERVER_ADDRESS** im azuredeploy.sh-Skript angegeben aktualisiert werden.

18. Verwenden Sie dazu aufgefordert werden, *Stellen Sie sicher, dass die DNS-Einstellung für die VNet aktualisiert wurden*, Azure-Portal eingestellten *DNS-Server* die *Basename*- Vnet-VNet in der Ressourcengruppe *Basename*Rg - Ntwk zu. Sollte auf *DNS benutzerdefiniert*festgelegt werden, und dem *primären DNS-Server* sollte die Adresse des lokalen DNS-Servers:

    ![[9]][9]

19. *Drücken die Ressourcengruppe für den AD-Servern erstellen* dazu aufgefordert in der Bash Shell-Fenster drücken Sie eine Taste und Ressourcengruppe für den AD-Servern in der Cloud halten erstellt wird.

20. *Drücken der VMs für den AD-Servern erstellen* aufgefordert werden in der Bash Shell-Fenster eine Taste und warten VMs erstellt und der Ressourcengruppe hinzugefügt werden.

21. *Drücken die VMs zur lokalen Domäne beitreten* zum Azure-Portal, und stellen Sie sicher, dass eine Gruppe namens *Basename*- Dns-Rg erstellt wurde und enthält zwei VMS (*Basename*-ad1-Vm und *Basename*-ad2-Vm):

    ![[10]][10]

22. In der Ressourcengruppe *Basename*Rg - Ntwk sicher, dass eine NSG erstellt wurde aufgerufen *Basename*-Ad-Nsg. Prüfen Sie eingehende und ausgehende Sicherheitsregeln für diese NSG Sie übereinstimmen aufgeführten Tabellen in die [Komponenten] [ solution-components] Abschnitt.

23. In der Bash Shell-Fenster dazu aufgefordert eine Taste und warten VMs mit der lokalen Domäne hinzugefügt werden.

24. Bei der *Gehen Sie zu der lokalen AD Server überprüfen, ob der Computer zu Domänen hinzugefügt wurden* aufgefordert, an den lokalen Computer anschließen und verwenden die Konsole *Active Directory-Benutzer und-Computer* überprüfen, ob beide virtuelle Computer der Domäne hinzugefügt wurden:

    ![[11]][11]

25. Bei der Aufforderung in der Bash Shell-Fenster drücken und AD Replication-Standort in der Domäne erstellt wird.

26. Bei der *Gehen Sie zu der lokalen AD Server überprüfen, ob die Replikation Website* aufgefordert, verwenden die Konsole *Active Directory-Standorte und-Dienste* zu um überprüfen, ob eine Replikation Website namens *Azure-Vnet-Ad-Standort* erfolgreich erstellt wurde, zusammen mit einer IP-standortübergreifende Verbindung namens *AzureToOnpremLink*und eine Subnetzmaske, die das VNet verweist:

    ![[12]][12]

27. Drücken Sie bei der Aufforderung in der Bash Shell-Fenster und warten Sie das Skript auf jedem AD-VMs Verzeichnisdiensten und DNS installiert.

28. *Bitte melden Sie sich an jede Azure AD Server erfolgreich konfiguriert Verzeichnisdienste* aufgefordert werden, öffnen Sie eine Remotedesktop-Verbindung von einem lokalen Computer auf Jumpbox (*Basename*- Jb-Vm) und anderen Remotedesktopverbindung aus der Jumpbox zum ersten AD Server öffnen (*Basename*-ad1-Vm). Melden Sie sich mit **Domänenname**, **ADMIN_USER_NAME**und **ADMIN_PASSWORD** in das azuredeploy.sh-Skript angegeben. Mit Server-Manager überprüfen Sie, ob Active Directory und DNS Rollen sowohl hinzugefügt wurden. Wiederholen Sie diesen Vorgang für den zweiten AD Server (*Basename*-ad2-Vm).

29. Drücken Sie ein bei Aufforderung im Bash Shell-Fenster. *Beliebige Taste Azure VNet DNS-Clienteinstellungen auf DNS in Azure festlegen* angezeigt wird, drücken Sie eine Taste und lassen Sie das Skript die DNS-Einstellungen für das VNet aktualisieren.

30. Wenn die Aufforderung *Überprüfen VNet DNS Einstellung wurde Azure VM DNS aktualisiert Server* wird mit dem Azure Portal eingestellten *DNS-Server* die *Basename*- Vnet-VNet in der Ressourcengruppe *Basename*Rg - Ntwk. Die primären und sekundären DNS-Server sollte jetzt zwei AD-VMs verweisen:

    ![[13]][13]

31. Neustart jeder AD-VMs bevor Sie fortfahren. Dieser Schritt ist notwendig, damit sie die DNS-Einstellungen von Azure abholen. Warten Sie, bis beide VMs zuerst ausgeführt werden.

32. Drücken Sie ein bei Aufforderung im Bash Shell-Fenster. Die nächste Aufforderung *Gateway UDR Gateway Subnetz anwenden (möglicherweise wurde es entfernt) drücken*, drücken Sie eine Taste und Skrip Gateway UDR aktualisieren.

33. Stellen Sie sicher, dass das Skript erfolgreich abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

- Lernen Sie die optimalen Methoden zum [Erstellen einer Ressourcengesamtstruktur fügt] [ adds-resource-forest] in Azure.

- Lernen Sie die best Practices für [ADFS Infrastruktur] [ adfs] in Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Sichere Hybrid-Netzwerkarchitektur mit Active Directory"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "Die Konsole Active Directory-Benutzer und-Computer mit zwei Azure VMs Server"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Die Konsole Active Directory-Standorte und-Dienste mit Replikationseigenschaften für die Site in der cloud"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "Der Inhalt der Basisname Ntwk Rg Ressourcengruppe"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "Die Subnetze Basename-Vnet VNet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "Die Elemente in der Webebene"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "NVAs in der Ressourcengruppe Basename Mgmt rg"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "Die Ressourcen in der Ressourcengruppe Basename dmz rg"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Suchen die öffentliche IP-Adresse des Azure VPN-Gateways"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "Der DNS-Server-Einstellungen für die * Basename *-Vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "Die * Basename *-Dns-Rg-Ressourcengruppe, die AD-Server VMs"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "Die Konsole Active Directory-Benutzer und-Computer AD Server virtueller Computer als Mitglied der Domäne auflisten"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Die Konsole Active Directory-Standorte und-Dienste mit der Replikation Website Azure AD-Server"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "DNS-Server-Einstellungen für Basename Vnet VNet verweisen auf den AD-Servern in der cloud"