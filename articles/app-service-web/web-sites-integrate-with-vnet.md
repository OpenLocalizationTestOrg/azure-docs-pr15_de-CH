<properties 
    pageTitle="Azure Virtual Network app integrieren" 
    description="Zeigt, wie Sie eine Anwendung in Azure App Service mit einem neuen oder vorhandenen Azure virtuellen Netzwerk verbinden" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Azure Virtual Network app integrieren #

Dieses Dokument beschreibt die Integration von Azure App Service virtuelles Netzwerk und gezeigt, wie Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)eingerichtet.  Wenn Sie Azure Virtual Networks (VNETs) vertraut sind, ist dies eine Funktion, die können Sie viele der Azure-Ressourcen in einem Netzwerk nicht Internet geroutet, die Zugang zu steuern.  Diese Netzwerke können dann mit einer Reihe von VPN-Technologie in lokalen Netzwerken verbunden werden.  Lernen über virtuelle Netzwerke Azure beginnen mit den Informationen: [Azure Virtual Network (Übersicht)][VNETOverview].  

Azure App Service verfügt über zwei Formen.  

1. Multi-Tenant-Systeme, die sämtliche Preisübersicht unterstützen
1. Die App Service-Umgebung (ASE) premiumfunktion, die in der VNET bereitgestellt.  

Dieses Dokument durchläuft VNET-Integration und nicht App Service-Umgebung.  Möchten Sie erfahren mehr über die Funktion ASE und starten Sie dann die Informationen: [App Service-Umgebung Einführung][ASEintro].

VNET-Integration ermöglicht die Web app Zugriff auf Ressourcen im virtuellen Netzwerk jedoch gewährt keinen privaten Zugriff zu Ihrer Anwendung aus dem virtuellen Netzwerk.  Private Websitezugriff ist nur mit einer ASE konfiguriert mit einer internen Load Balancer (ILB).  Informationen zur Verwendung einer ILB ASE beginnen mit dem Artikel: [Erstellen und Verwenden einer ILB ASE][ILBASE]. 

Ein häufiges Szenario, in dem Sie VNET-Integration verwenden, ermöglicht Ihrer Anwendung an eine Datenbank oder einen Webdienst auf einem virtuellen Computer in Azure virtuelle Netzwerk zugreifen.  VNET Integration müssen Sie einen öffentlichen Endpunkt verfügbar machen, auf die VM kann jedoch stattdessen privaten routingfähigen nicht Internet-Adressen verwenden.  

Die VNET-Integration:

- erfordert einen Standard oder Premium Preise plan 
- Arbeiten mit Classic(V1) oder Manager(V2)-VNET Ressource 
- TCP und UDP unterstützt
- mit Web, Mobile und API-apps
- ermöglicht eine Anwendung gleichzeitig eine Verbindung mit nur 1 VNET
- bis zu 5 VNETs in einem App Plan integriert werden können 
- das gleiche VNET von mehreren apps in einer App Service-Plan verwendet werden können
- eine SLA 99,9 % aufgrund der Abhängigkeit VNET Gateway unterstützt

Es gibt einige Dinge, die VNET-Integration einschließlich nicht unterstützt:

- Bereitstellen eines Laufwerks
- AD-integration 
- NetBios
- Zugriff auf die privaten Website

### <a name="getting-started"></a>Erste Schritte ###
Hier sind einige Dinge bedenken, bevor Sie ein virtuelles Netzwerk mit Ihrer Anwendung:

- VNET-Integration funktioniert nur mit apps in **Standard** oder **Premium** Preise Plan.  Aktivieren Sie die Funktion und skalieren Ihre App Service-Plan für eine nicht unterstützte Preisgestaltung apps die Verbindung der VNETs verlieren sie verwenden.  
- Das virtuelle Zielnetzwerk bereits vorhanden, müssen Punkt-zu-Standort-VPN mit dynamischen routing-Gateway aktiviert werden, bevor mit einer Anwendung verbunden werden können.  Sie können Punkt-zu-Standort-Virtual Private Network (VPN) aktivieren, wenn Ihr Gateway mit statischen routing konfiguriert ist.
- Das VNET muss das Abonnement Ihrer App Service Plan(ASP).  
- Apps, die mit einem VNET integriert werden DNS verwenden, für diese VNET angegeben.
- Standardmäßig werden die Integration von apps nur Datenverkehr in Ihrem VNET anhand der Routen, die in Ihrem VNET definiert sind.  


## <a name="enabling-vnet-integration"></a>VNET-Integration aktivieren ##

Dieses Dokument konzentriert sich hauptsächlich mit der Azure-Portal für VNET-Integration.  Führen Sie zum Aktivieren der VNET-Integration mit PowerShell mithilfe der Richtung: [Verbinden Ihrer Anwendung mit dem virtuellen Netzwerk mithilfe von PowerShell][IntPowershell].

Sie können Ihre Anwendung mit einem neuen oder vorhandenen virtuellen Netzwerk verbinden.  Wenn Sie ein neues Netzwerk als Teil der Integration dann neben nur das VNET erstellen, ist dynamisches routing-Gateway vorkonfiguriert und auf Standort VPN aktiviert.  

>[AZURE.NOTE] Konfigurieren eine neues virtuelles Netzwerkintegration kann mehrere Minuten dauern.  

VNET-Integration Öffnen Ihrer app Einstellungen aktivieren und wählen Sie Netzwerk.  Die Benutzeroberfläche öffnet Möglichkeiten drei Netzwerke.  Dieses Handbuch wird VNET Integration jedoch Hybrid-Verbindungen und App Service-Umgebungen werden weiter unten in diesem Dokument erläutert.  

Wenn Ihre Anwendung nicht in der richtigen Plan Preise ermöglicht die Benutzeroberfläche hilfreich Ihren Plan zu höheren Preisen Plan Ihrer Wahl skalieren.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Integration in vorhandene VNET VNET aktivieren###
VNET Integration Benutzeroberfläche ermöglicht Ihnen die Auswahl aus einer Liste der vnets.  Klassische VNETs zeigen, daß sie das Wort "Klassische" in Klammern neben dem Namen VNET.  Die Liste wird sortiert, sodass VNETs Ressourcenmanager zuerst aufgeführt werden.  In der Abbildung unten sehen Sie, dass nur ein VNET ausgewählt werden kann.  Es gibt mehrere Gründe, ein VNET, einschließlich Grau:

- Das VNET ist ein weiteres Abonnement auf Ihr Konto zugreifen
- Das VNET ist nicht Website aktiviert haben
- Das VNET hat keine dynamische routing-gateway


![][2]

Zum Aktivieren Integration klicken Sie einfach auf die VNET integrieren möchten.  Nach Auswahl das VNET wird Ihre app für wirksam wird automatisch gestartet.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Aktivieren Sie auf der Website in einem klassischen VNET #####
Wenn das VNET keinen Gateway Website muss haben, zuerst festgelegt werden.  Dazu für klassische VNET zum [Azure-Portal] [ AzurePortal] und die Liste der virtuellen Networks(classic).  Hier klicken Sie auf das Netzwerk soll integriert und auf großen Feld unter Essentials VPN-Verbindungen bezeichnet.  Hier erstellen Sie Ihren Zugriffspunkt Standort VPN und sogar ein Gateway erstellen.  Nach Website mit Gateway erstellen den Punkt durchlaufen werden ca. 30 Minuten vor.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Aktivieren auf Site ein Ressourcen-Manager VNET #####

Ein Ressourcen-Manager-VNET mit einem Gateway konfigurieren und Website Sie PowerShell verwenden, wie müssen [eine Punkt-zu-Standort-Verbindung zu einem virtuellen Netzwerk mit PowerShell konfigurieren][V2VNETP2S].  Die Benutzeroberfläche zum Ausführen dieser Funktion ist noch nicht verfügbar. 

### <a name="creating-a-pre-configured-vnet"></a>Erstellen einer vorkonfigurierten VNET ###
Sie gegebenenfalls eine neue VNET mit einem Gateway konfiguriert und Punkt-zu-Standort hat networking UI App-Dienst die Möglichkeit, dass nur ein Ressourcen-Manager VNET.  Klassische VNET mit einem Gateway und Punkt-zu-Standort erstellen möchten, müssen Sie manuell über die Netzwerk-Benutzeroberfläche dazu. 

Um ein Ressourcen-Manager VNET über die VNET-Integration-Benutzeroberfläche zu erstellen, wählen Sie **Neues virtuelles Netzwerk erstellen** , und geben die:

- Name des virtuellen Netzwerks
- Virtuelles Netzwerkadressblock
- Subnet-Name
- Subnetz Adressblock
- Gateway-Adresse blockieren
- Punkt-zu-Standort-Adresse

Soll dieses VNET für die Verbindung zu Ihrem Netzwerk sollten Sie vermeiden, Kommissionierung IP-Adressraum mit diesen Netzwerken.  

>[AZURE.NOTE] Ressourcenmanager VNET erstellen mit einem Gateway dauert etwa 30 Minuten und derzeit werden nicht integrieren das VNET Ihrer Anwendung.  Nach dem Erstellen das VNET mit dem Gateway müssen Sie für Ihre Anwendung VNET Integration Benutzeroberfläche und wählen Sie das neue VNET.

![][3]

Azure VNETs werden normalerweise in privaten Netzwerkadressen erstellt.  Standardmäßig die VNET-Integration leitet Feature sämtlichen Datenverkehr für die IP-Adressbereiche in Ihr VNET.  Die private IP-Adressbereiche sind:

- 10.0.0.0/8 - entspricht 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 - entspricht 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 - entspricht 192.168.0.0 - 192.168.255.255
 
VNET Adressraum muss in CIDR-Notation angegeben werden.  Wenn Sie mit CIDR-Notation nicht vertraut sind, ist eine Methode zur Angabe einer IP-Adresse und eine Ganzzahl die Netzwerkmaske Adressblöcke. Sollten Sie eine Kurzübersicht, 10.1.0.0/24 würde 256 Adressen und 10.1.0.0/25 128 Adressen.  Eine IPv4-Adresse mit einer /32 wäre nur 1 Adresse.  

Wenn die DNS-Serverinformationen hier festgelegt werden, die für das VNET festgelegt werden.  Nach der Erstellung der VNET können Sie diese Informationen aus der Benutzerfunktionalität VNET bearbeiten.

Beim Erstellen einer klassischen VNET mit VNET Integration Benutzeroberfläche erstellt er eine VNET in derselben Ressourcengruppe wie Ihre app. 

## <a name="how-the-system-works"></a>Funktionsweise des ##
Hinter den Kulissen erstellt dieses Feature auf Punkt-zu-Standort-VPN-Technologie für das VNET Ihrer app herstellen.   Apps in Azure App Service sind Multi-Tenant-System die Bereitstellung einer Anwendung direkt in ein VNET wie mit virtuellen Computern ausschließt.  Auf Punkt-zu-Standort-Technologie beschränken wir Netzwerkzugriff nur virtuelle Computer hosten der Anwendung.  Zugriff auf das Netzwerk ist außerdem auf diesen Hosts app eingeschränkt, damit Ihre apps nur Netzwerke zugreifen können, die Zugang zu konfigurieren.  

![][4]
 
Wenn Sie einen DNS-Server, mit dem virtuellen Netzwerk konfiguriert haben müssen Sie IP-Adressen verwenden.  Beachten Sie bei IP-Adressen verwenden, dass der größte Vorteil dieses Features privaten Adressen innerhalb des privaten Netzwerks verwendet werden können.  Festlegen Ihrer app Verwendung öffentlicher IP-für eines Ihrer virtuellen Computer Adressen nicht die VNET-Integration verwenden und über das Internet kommunizieren.


##<a name="managing-the-vnet-integrations"></a>VNET-Integrationen verwalten##

Das Verbinden und Trennen einer VNET ist Ebene Anwendung.  Vorgänge, die die VNET-Integration über mehrere apps beeinflussen können sind ein ASP.  In der Benutzeroberfläche, die Ebene der app angezeigt erfahren auf Ihrem VNET Sie.  Die gleichen Informationen wird auch angezeigt, auf der ASP.  

![][5]

Auf der Netzwerkstatus-Funktion finden Sie unter Wenn Ihre Anwendung mit der VNET verbunden ist.  Ist Ihr Gateway VNET Sie zeige was dann deshalb als nicht verbunden.  

Die Informationen, die Sie jetzt in der Anwendung verfügbar ist auf VNET Integration UI detaillierte Informationen erhalten Sie von ASP.  Hier sind die Artikel:

- VNET Name - dieser Link öffnet die Netzwerk-Benutzeroberfläche
- Ort – spiegelt die Position der VNET.  Es ist möglich, vnet an einem anderen Ort zu integrieren.
- Zertifikatstatus - Zertifikate sichern die VPN-Verbindung zwischen der Anwendung und der VNET gibt.  Dieser enthält einen Test, um sicherzustellen, dass sie synchron sind.
- Status des Gateway - die Gateways aus irgendeinem Grund ausfallen sollte kann Ihre app Ressourcen in das VNET nicht zugreifen.  
- VNET Adressraum - Dies ist die IP-Adressraum für das VNET.  
- Zeigen Sie Website-Adressraum - Dies ist auf Seite IP-Adressbereich für das VNET.  Ihre app zeigt Kommunikation eines IP-Adressen in diesem Adressraum aus.  
- Website-Adressraum - Website können Standort-zu-Standort-VPNs Ihr VNET auf lokale Ressourcen oder andere VNETs herzustellen.  Haben Sie, konfiguriert dann die IP-Bereiche mit VPN-Verbindung wird hier angezeigt.
- DNS-Server - haben Sie Ihr vnet dann hier aufgeführten konfigurierten DNS-Server.
- IPs weitergeleitet VNET - dort sind eine Liste von IP-Adressen hat das VNET routing definiert.  Diese Adressen werden hier angezeigt.  

Der einzige Vorgang können Sie in der app-Ansicht Ihrer VNET-Integration ist das VNET Ihrer Anwendung trennen, derzeit verbunden ist.  Klicken Sie hierzu einfach auf Trennen oben.  Diese Aktion ändert nicht das VNET.  Die VNET und die Konfiguration einschließlich Gateways bleibt unverändert.  Wenn Sie dann Ihr VNET löschen müssen Sie zunächst die Ressourcen einschließlich Gateways löschen.  

Die Ansicht App Service-Plan hat eine Reihe von zusätzlichen Operationen.  Es ist auch anders als die Anwendung zugreifen.  Zu öffnen ASP Networking-Benutzeroberfläche Sie einfach Ihre ASP-Benutzeroberfläche und Bildlauf nach unten.  Es ist ein Benutzeroberflächenelement Netzwerkstatus-Funktion aufgerufen.  Geben sie einige kleinere Details der VNET-Integration ein.  Diese Benutzeroberfläche auf öffnet Netzwerk Feature Status Benutzeroberfläche.  Wenn klicken Sie auf "Click here to verwalten" Öffnen Sie Benutzeroberfläche, in der die VNET Integrationen in ASP aufgeführt.

![][6]

Der Speicherort der ASP ist denken, wenn an der VNETs, die Integration mit.  Das VNET wird an einer anderen Stelle sind Sie eher Wartezeitprobleme finden Sie unter.  

Integriert mit VNETs erinnert auf wie viele VNETs Ihre apps mit ASP und wie viele Sie haben integriert.  

Um zusätzliche Details jedes VNET anzuzeigen, klicken Sie einfach auf VNET interessiert sind.  Neben den Waren, erwähnt sehen Sie auch eine Liste der ASP-apps, die dieses VNET verwenden.  

Maßnahmen sind zwei primäre Aktionen.  Die erste ist Routen hinzufügen, die Datenverkehr Ihre app in Ihrem VNET steuern.  Die zweite Aktion ist die Möglichkeit, Zertifikate und Informationen zu synchronisieren.

![][7]

**Routing** Wie bereits erwähnt sind Routen, die in Ihrem VNET definiert wie für den Verkehr in das VNET von Ihrer Anwendung verwendet wird.  Es gibt einige Verwendungen aber an Kunden zusätzliche ausgehenden Datenverkehr von einer Anwendung in der VNET und diese Funktion senden möchten ist.  Was geschieht mit den Datenverkehr bis wie Kunden ihre VNET konfiguriert ist.  

**Zertifikate** Status der spiegelt einer Überprüfung vom App-Dienst überprüft sind Zertifikate, die für die VPN-Verbindung wir verwenden dennoch durchgeführt werden.  VNET-Integration aktiviert, dann gibt ist dies die erste Integration, dass VNET von apps, die in dieser ASP ein erforderlichen Austausch von Zertifikaten für die Sicherheit der Verbindung.  Mit den Zertifikaten erhalten wir die DNS-Konfiguration, Routen und andere ähnliche Dinge, die das Netzwerk beschreiben.
Wenn diese Zertifikate oder Netzwerkinformationen geändert müssen Sie "Netzwerk synchronisieren" klicken.  **Hinweis**: Wenn Sie "Sync Netzwerk" klicken Sie eine kurze Ausfallzeit in Verbindung zwischen Ihrer Anwendung und Ihrem VNET verursacht.  Während Ihre Anwendung nicht gestartet werden konnte Konnektivität Ihre Website nicht richtig verloren.  

##<a name="accessing-on-premise-resources"></a>Zugriff auf lokale Ressourcen##

Einer der Vorteile der Integration von VNET ist, die das VNET, die auf lokalen Netzwerk mit einem Standort zu Standort VPN verbunden ist, dann Ihre apps Zugriff auf lokale Ressourcen in Ihrer Anwendung können.  Dies funktioniert zwar möglicherweise das auf lokale VPN-Gateway mit Routen für Ihren Zugriffspunkt IP-Website aktualisieren liegen.  Bei das Standort zu Standort VPN zunächst sollten Skripts konfigurieren auf Routen der auf Standort VPN einrichten.  Wenn Standort VPN nach dem Punkt hinzufügen der Standort zu Standort VPN erstellen, müssen Sie die Routen manuell aktualisieren.  Ausführliche Informationen dazu variieren Gateway und werden hier nicht beschrieben.  

>[AZURE.NOTE] Während die VNET-Integration mit einem Standort zu Standort VPN Zugriff auf lokale Ressourcen funktioniert funktioniert derzeit nicht mit einem ExpressRoute VPN dazu es.  Dies gilt bei der Integration von Classic oder Ressourcen-Manager VNET.  Benötigen Sie Zugriff auf Ressourcen über ein ExpressRoute VPN können Sie eine ASE verwenden, die in Ihrem VNET ausführen können. 

##<a name="pricing-details"></a>Preisdetails##
Es gibt ein paar Preise Nuancen, die Ihnen bei die VNET-Integration verwenden.  Es gibt 3 Abgaben für die Verwendung dieses Features:

- ASP Preise Tier Vorschriften
- Kosten für die Datenübertragung
- VPN-Gateway Kosten.

Für apps, um dieses Feature verwenden können müssen sie in einem Standard oder Premium App Service-Plan.  Finden Sie Details über diese Kosten: [App Servicepreise][ASPricing]. 

Aufgrund der Punkt zum Standort-VPNs werden behandelt, Sie immer Gebühren für ausgehende Daten über die Verbindung VNET Integration ist das VNET im gleichen Rechenzentrum.  Auf den Kosten sind finden Sie hier: [Transfer Preisdetails][DataPricing].  

Das letzte Element ist die Kosten für die VNET-Gateways.  Gateways etwas wie Standort, Standort-VPNs benötigen werden für Gateways unterstützen die Integration VNET bezahlt werden.  Gibt Details dieser Kosten: [VPN-Gateway Preise][VNETPricing].  

##<a name="troubleshooting"></a>Problembehandlung##

Während die Funktion einfach heißt das nicht, dass Ihre Erfahrung problemlos.  Stoßen Sie sind Probleme beim Zugriff auf den gewünschten Endpunkt vorhanden einige Dienstprogramme zum Testen der Verbindung in der app-Konsole verwendeten.  Es gibt zwei Konsole Erfahrungen verwenden können.  Ist von der Kudu-Konsole und der andere die Konsole von Azure-Portal innerhalb.  Kudu-Konsole aus Ihrer app zu Extras -> Kudu.  Dies entspricht dem zu [Sitename]. scm.azurewebsites.net.  Sobald, die einfach öffnet gehen Sie zur Registerkarte Debug-Konsole.  Um Azure Portal gehosteten Konsole dann Ihre App Extras-Konsole.  


####<a name="tools"></a>Tools####

Die Tools Ping und Nslookup Tracert funktioniert nicht über die Konsole aus Gründen der Sicherheit.  Zum Füllen die void dort wurden zwei separate Tools hinzugefügt.  Um DNS-Funktionalität testen haben wir ein Tool namens nameresolver.exe hinzugefügt.  Die Syntax lautet:

    nameresolver.exe hostname [optional: DNS Server]

Nameresolver können Sie die Hostnamen überprüfen, von denen die Anwendung abhängt.  Auf diese Weise können Sie testen, wenn etwas nicht mit DNS konfiguriert oder möglicherweise keinen Zugriff auf den DNS-Server haben.

Nächste Tool können Sie die TCP-Verbindung mit einer Kombination aus Host und Port testen.  Dieses Programm heißt tcpping.exe und die Syntax ist:

    tcpping.exe hostname [optional: port]

Dieses Tool informiert, wenn Sie einen bestimmten Host und Port erreichen führt die gleiche Aufgabe mit dem ICMP Ping-Dienstprogramm basiert.  ICMP-Ping-Dienstprogramm informiert Sie, wenn Ihr Host ist.  Mit Tcpping finden Sie Wenn Sie einen bestimmten Port auf einem Server zugreifen können.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Debuggen auf VNET gehosteten Ressourcen####

Es gibt eine Reihe von Dingen, die Ihre Anwendung verhindern können, dass ein bestimmter Host und Port.  In den meisten Fällen ist es drei Dinge:

- **Eine Firewall wie**  Wenn Sie eine Firewall wie dann TCP-Timeout erreicht wird.  Das ist in diesem Fall 21 Sekunden.  Verwenden Sie das Tool Tcpping zum Testen der Konnektivität.  TCP-Timeouts kann durch viele Dinge über Firewalls jedoch beginnen.  
- **DNS ist nicht verfügbar**  DNS-Zeitlimit beträgt 3 Sekunden pro DNS-Server.  Haben Sie 2 Sekunden wird DNS-Server.  Verwenden Sie Nameresolver, um festzustellen, ob DNS funktioniert.  Beachten Sie, dass Sie Nslookup verwenden können, wie die nicht DNS verwendet das VNET mit konfiguriert ist.
- **Ungültiger P2S IP-Bereich** Auf Seite IP Adressbereich muss in RFC 1918 privaten IP-Bereiche (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) Bereich verwendet IP-Adressen außerhalb, wenn Dinge funktionieren nicht.  

Wenn dieser Artikel Ihr Problem nicht, zunächst für einfache Dinge wie:  

- Zeigt das Gateway als oben im Portal?
- Werden Zertifikate werden als Synchronisierung angezeigt?
- Hat jemand die Netzwerkkonfiguration ohne "Sync Netzwerk" in der betroffenen ASPs geändert? 

Wenn Ihr Gateway ist bringen Sie es wieder.  Wenn Ihre Zertifikate synchron ASP Ansicht Ihrer VNET-Integration, und drücken Sie "Sync-Netzwerk".  Wenn Sie vermuten, dass eine Änderung an der Konfiguration VNET und es nicht synchronisieren möchten, mit der ASPs ASP Ansicht Ihrer VNET-Integration und drücken "Netzwerk synchronisieren" nur als eine Erinnerung, dadurch wird eine kurze Ausfallzeit apps mit der VNET-Verbindung.  

Wenn alles in Ordnung ist, müssen Sie etwas tiefer:

- Gibt es andere apps mit VNET-Integration in demselben VNET erreichen? 
- Sie gehen Sie zur Konsole app und verwenden Tcpping zu anderen Ressourcen in Ihrem VNET?  

Wenn eines der oben genannten true dann Ihre VNET Integration ist gut und das Problem an anderer Stelle.  Dadurch wird es schwieriger, da gibt es keine einfache Möglichkeit, warum Host: Port erreichen kann.  Die Ursachen gehören:

- Sie haben eine Firewall auf Ihrem Host verhindern des Zugriffs auf die Anwendungsport von Ihrem Standort IP-Bereich.  Subnetze häufig überschreiten erfordert öffentlichen Zugriff.
- der Zielhost ist ausgefallen
- die Anwendung ist
- Sie hatten falsche IP oder hostname
- die Anwendung überwacht nicht erwartet.  Sie können dies überprüfen auf diesem Host und "Netstat-aon" Cmd Prompt aus.  Diese zeigen was Ihnen ID was port abhört.  
- die Netzwerk-Sicherheitsgruppen sind so konfiguriert, dass sie Zugriff auf Ihre Anwendungshost und Anschluss von Ihrem Standort IP-Bereich verhindern

Beachten Sie, dass Sie welche IP des Zugriffspunkts Website IP-Bereich nicht, die Ihre app kennen müssen Zugriff aus dem gesamten Bereich verwenden.  

Zusätzliche Debug Schritte umfassen:

- eine andere VM in Ihrem VNET melden Sie an und versuchen Sie, die Ressource Host: Port aus.  Gibt es einige TCP-Ping-Dienstprogramme können für diesen Zweck oder sogar können Telnet werden.  Hier soll nur bestimmen, ob Konnektivität von diesem VM. 
- eine Anwendung auf einer anderen VM und Test Zugriff, Host und Port von der Konsole aus Ihrer Anwendung bringen  
####<a name="on-premise-resources"></a>Lokale Ressourcen####
Wenn Ihre Ressourcen lokal nicht erreichen, dann prüfen Sie ist Wenn Sie eine Ressource in Ihrem VNET erreichen können.  Wenn das funktioniert, sind die nächsten Schritte einfach  VM in Ihrem VNET müssen Sie versuchen, die auf lokale Anwendung.  Sie können Telnet oder ein TCP-Ping-Dienstprogramm verwenden.  Wenn der VM kann nicht zugreifen auf lokale Ressource zuerst sicherstellen arbeitet die VPN-Verbindung von Standort zu Standort.  Wenn es funktioniert überprüfen Sie dasselbe erwähnt sowie das lokale Gateway-Konfiguration und Status.  

Jetzt Ihr VNET gehostet VM erreichen Ihre lokalen System jedoch Ihre app darf nicht der Grund wahrscheinlich eine der folgenden ist:
- Ihre Routen sind nicht mit Ihrer Website IP-Bereiche in Ihr auf lokalen Gateway konfiguriert.
- die netzwerksicherheitsgruppen blockieren Zugriff für Ihren Zugriffspunkt Website IP-Bereich
- auf lokale Firewalls blockieren Datenverkehr von Ihrem Standort IP-Bereich
- ein Benutzer definierte Route(UDR) haben in Ihrem VNET, das Ihren Website-Datenverkehr verhindert auf lokale Netzwerk erreicht

## <a name="hybrid-connections-and-app-service-environments"></a>Hybridverbindungen und App Service-Umgebung##
Es gibt 3 Merkmale, die Zugriff auf VNET gehosteten Ressourcen ermöglichen.  Sie sind:

- VNET-Integration
- Hybridverbindungen
- App Service-Umgebung

Hybridverbindungen müssen Sie einen Relay-Agent namens Hybrid Verbindung Manager(HCM) in Ihrem Netzwerk installieren.  Der HCM muss in Azure und Ihre Anwendung herstellen.  Diese Lösung eignet sich insbesondere aus einem Remotenetzwerk wie auf lokale Netzwerk oder sogar mit einem weiteren Cloud gehostete Netzwerk da anliegenden Internet Zugriff nicht erforderlich ist.  Der HCM läuft nur unter Windows und Sie können bis zu 5 Instanzen zur Gewährleistung hoher Verfügbarkeit.  Hybridverbindungen unterstützt nur TCP jedoch und jeder Endpunkt HC entsprechend einer bestimmten Host: Port-Kombination.  

App Service-Umgebung-Funktion können Sie eine Instanz von Azure App Service in Ihrem VNET ausgeführt.  Dadurch werden Ihre apps Zugriff auf Ressourcen in Ihrem VNET ohne zusätzliche Schritte.  Die Vorteile von App Service-Umgebung ist, 8 Core dedizierte Arbeitskräfte mit 14 GB RAM zu verwenden.  Ein weiterer Vorteil ist, dass das System Ihren Bedürfnissen skaliert werden kann.  Im Gegensatz zu den Multi-Tenant-Umgebungen, ASP Größe begrenzt ist, steuern in einem ASE wie viele Ressourcen dem System erhalten soll.  Hinsichtlich der Fokus dieses Dokuments gehört, die Dinge mit einer ASE, die nicht mit VNET-Integration mit einem ExpressRoute VPN arbeiten können.  

Zwar es einige Fall überlappen verwenden gibt, können keines dieser Funktion andere ersetzen.  Wissen, welche Funktion für Ihre Bedürfnisse verbunden ist und wie möchten sie verwenden.  Zum Beispiel:

- Wenn Sie Entwickler sind und einfach eine Website in Azure ausgeführt und den Datenbankzugriff auf der Arbeitsstation unter Ihrem Schreibtisch ist am einfachsten mit Hybrid-Verbindungen.  
- Sie eine große Organisation möchte eine große Anzahl von Webeigenschaften in der öffentlichen cloud und in Ihrem eigenen Netzwerk verwalten, sollten Sie sich die App Service-Umgebung.  
- Haben Sie eine Anzahl von App Service apps gehostet und Zugriff auf Ressourcen in Ihrem VNET VNET Integration ist der Weg zu gehen wollen.  

Über die Anwendungsfälle gibt es einige Einfachheit Aspekte.  Wenn Ihr VNET ist bereits am lokalen Netzwerk mit VNET-Integration oder App Service-Umgebung leicht auf lokale Ressourcen verbrauchen.  Andererseits, wenn das VNET nicht mit auf lokalen Netzwerk verbunden ist, wird viel Aufwand zum Einrichten einer Standort-zu-Standort-VPN mit gegenüber das VNET Installation der HCM.  

Funktionale Unterschiede gibt auch Preise Unterschiede.  Das App Service-Umgebung ist eine Prämie Serviceangebot aber bietet die Optionen zur Konfiguration der neben anderen tollen Funktionen.  VNET-Integration mit Standard oder Premium ASPs verwendet werden und ist ideal für Ressourcen in Ihrem VNET von Multi-Tenant App Service sicher zu verbrauchen.  Hybrid-Verbindung hängt derzeit ein Konto mit Preisen, die kostenlos und dann schrittweise teurer Grundlage benötigten BizTalk.  Kommt es jedoch in vielen Netzwerken arbeiten, ist keine Funktion wie Hybridverbindungen, die Sie in über 100 separate Netzwerke zugreifen können.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
