<properties
   pageTitle="DMZ-Beispiel: erstellen eine DMZ Netzwerke mit Firewall, UDR und NSG | Microsoft Azure"
   description="Erstellen einer DMZ mit einer Firewall, benutzerdefinierte Routing (UDR) und Netzwerk-Sicherheitsgruppen (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Beispiel 3 – erstellen eine DMZ Netzwerke mit Firewall, UDR und NSG

[Sicherheit Grenze Best Practices Seite zurück][HOME]

Dieses Beispiel erstellt eine DMZ mit einer Firewall, vier Windows Server Benutzer definiert Routing, IP-Weiterleitung und Netzwerk-Sicherheitsgruppen. Es werden auch die entsprechenden Befehle ermöglichen ein besseres Verständnis der einzelnen Schritte durchlaufen. Ist auch ein Abschnitt Datenverkehr Szenario ermöglichen eine schrittweise wie Sicherheitsebenen in der DMZ Datenverkehr durchläuft. Schließlich wird die Verweise Abschnitt der vollständige Code und Anweisung zu dieser Umgebung testen und Experimentieren mit verschiedenen Szenarien 

![Bidirektionale DMZ NSG, NVA und UDR][1]

## <a name="environment-setup"></a>Setup der Umgebung
In diesem Beispiel wird ein Abonnement, das Folgendes enthält:

- Drei cloud-Services: "SecSvc001", "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit drei Subnetzen: "SecNet", "FrontEnd" und "BackEnd"
- Eine virtuelle Appliance, in diesem Beispiel eine Firewall mit dem Subnetz verbunden SecNet
- Ein Windows-Server, der einen Webserver für die Anwendung ("IIS01")
- Zwei Windows-Servern, die Anwendung wieder beenden Server ("AppVM01", "AppVM02")
- Ein Windows-Server, die DNS-Server ("DNS01")

Im Abschnitt Verweise ist eine PowerShell-Skript, die meisten der oben beschriebenen Umgebung aufzubauen. VMs und virtuelle Netzwerke erstellen zwar das Beispielskript erledigt werden nicht ausführlich in diesem Dokument.

So erstellen Sie die Umgebung:

  1.    Die Netzwerk-Konfiguration XML-Datei im Abschnitt Informationsquellen enthalten (aktualisiert mit Namen, Speicherort und IP-Adressen, um bestimmten Szenarios zu entsprechen)
  2.    Aktualisieren Sie die Benutzervariablen im Skript entsprechend die Umgebung, die das Skript ausgeführt werden (Abonnements, Namen usw.)
  3.    Führen Sie das Skript in PowerShell

**Hinweis**: Region erhält, der in der PowerShell-Skript muss Bereich für die Netzwerk-Konfiguration XML-Datei angezeigt.

Nach der erfolgreichen Ausführung des Skriptes können Skript folgendermaßen durchgeführt:

1.  Richten Sie die Firewall-Regeln, siehe weiter unten im Abschnitt: Beschreibung der Firewall.
2.  Optional sind im Abschnitt Informationsquellen zwei Skripts der Webserver und Anwendungsserver mit einer einfachen Web-Anwendung zum Testen dieser DMZ-Konfiguration eingerichtet.

Nach der erfolgreichen Ausführung des Skriptes Firewall Regeln müssen ausgefüllt werden, siehe Abschnitt: Firewall-Regeln.

## <a name="user-defined-routing-udr"></a>Benutzerdefinierte Routing (UDR)
Standardmäßig werden die folgenden systemrouten definiert als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Der VNETLocal ist immer prefix(es) definierten Adresse des VNet für das bestimmte Netzwerk (d.h. es ändert sich von VNet zu VNet je nach jedes bestimmte VNet Definition). Die restlichen systemrouten sind statische und wie oben beschrieben.

Priorität Routen über die längste Präfix Übereinstimmung (LPM) Methode verarbeitet, so gelten die konkreteste Route in der Tabelle für eine gegebene Zieladresse.

Daher Datenverkehr (z. B. in der DNS01 Server 10.0.2.4) für den lokale Netzwerk (10.0.0.0/16) über VNet zum Ziel durch die 10.0.0.0/16 Route weitergeleitet werden. In anderen Worten ist für 10.0.2.4, die 10.0.0.0/16 Route konkreteste Route aber 10.0.0.0/8 und 0.0.0.0/0 konnte gilt, sind weniger spezifischen sie wirken sich nicht auf diesen Datenverkehr. So würde der Datenverkehr 10.0.2.4 nächsten Hop des lokalen VNet haben und einfach an das Ziel weiterleiten.

Wenn Datenverkehr für 10.1.1.1 beispielsweise sollte 10.0.0.0/16 Route würde nicht angewendet, aber die 10.0.0.0/8 der genauesten wäre und Verkehr wäre gelöscht ("schwarzer Löcher"), da der nächste Hop Null ist. 

Das Ziel Null-Präfixe oder VNETLocal Präfixe gelten nicht, wenn folgten allgemeinsten route 0.0.0.0/0 und weitergeleitet mit dem Internet als Nächster Hop und somit, Azure Internet Rand.

Sind es zwei identische Präfixe in der Routentabelle, lautet die Reihenfolge basierend auf Attribut "Source" Routen:

1.  "VirtualAppliance" = Benutzer festgelegte Route der Tabelle manuell hinzugefügt
2.  "VPNGateway" = Dynamic Route (BGP mit Hybrid-Netzwerken) ein dynamisches Protokoll hinzugefügt, diese Routen möglicherweise ändern wie das dynamische Protokoll automatisch in hervorragendem Netzwerk widerspiegelt
3.  "Default" = Systemrouten, lokale VNet und statische Einträge wie in der obigen Routentabelle.

>[AZURE.NOTE] Jetzt können Benutzer definiert Routing (UDR) ExpressRoute mit VPN-Gateways Sie erzwingen, ausgehenden und eingehenden Cross lokale Datenverkehr an virtuelle Netzwerkgerät (NVA).

#### <a name="creating-the-local-routes"></a>Erstellen lokalen Routen

In diesem Beispiel werden zwei Routingtabellen benötigt, für die Front-End- und Back-End-Subnetze. Jede Tabelle wird mit statischen Routen für das angegebene Subnetz geladen. Für dieses Beispiel verfügt jede Tabelle über drei Routen:

1. Datenverkehr im lokalen Subnetz mit keine nächste Hop definiert lokalen Subnetz Datenverkehr die Firewall umgeht
2. Virtuelle Netzwerkverkehr mit einem Firewall als nächsten Hop überschreibt diese Standardregel, die lokale VNet Datenverkehr direkt weitergeleitet
3. Alle übrigen Datenverkehr (0/0) mit einer Firewall als nächsten Hop

Erstellte Routingtabellen sind sie Subnetze gebunden. Für Front-End-Subnetz sollte Routingtabelle einmal erstellt und an das Subnetz wie folgt aussehen:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Beispielsweise die folgenden Befehle werden zum Erstellen der Routentabelle, benutzerdefinierte Route hinzufügen und binden die Routentabelle Subnetz (Hinweis; Elemente unten mit einem Dollarzeichen (z. B.: $BESubnet) benutzerdefinierte Variablen des Skripts im Referenzabschnitt dieses Dokuments):

1.  Routing Basistabelle muss zunächst erstellt werden. Dieser Ausschnitt zeigt die Erstellung der Tabelle für die Back-End-Subnetz. Im Skript wird auch eine entsprechende Tabelle für das Front-End-Subnetz erstellt.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Nach dem Erstellen der Routentabelle können bestimmte benutzerdefinierte Routen hinzugefügt werden. In diesem Schnitt werden alle Datenverkehr (0.0.0.0/0) über die virtuelle Appliance weitergeleitet (eine Variable $VMIP [0] Dient zum Übergeben der IP-Adresse zugewiesen, wenn die virtuelle Appliance zuvor im Skript erstellt wurde). Im Skript wird auch eine entsprechende Regel in der Front-End-Tabelle erstellt.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Routeneintrag oben wird Datenverkehr innerhalb der VNet direkt zum Ziel, nicht die virtuelle Appliance Netzwerk weiterleiten könnte Standardroute auf "0.0.0.0/0", aber die noch bestehenden 10.0.0.0/16 Standardregel überschrieben. Muss korrekt dies folgende Regel hinzugefügt werden.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Zu diesem Zeitpunkt wird eine Auswahl. Oben zwei Routen werden alle Bewertung auch Datenverkehr innerhalb eines einzelnen Subnetzes Firewall Datenverkehr. Dies kann wünschenswert sein, aber Datenverkehr in einem Subnetz ohne Beteiligung der Firewall weiterleiten ein drittes lokal sehr spezielle Regel hinzugefügt werden kann. Diese Route Staaten jede Adresse bestimmen für das lokale Subnetz nur weiterleiten es direkt (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Schließlich muss mit die routing-Tabelle erstellt und mit einer benutzerdefinierten Tabelle nun mit einem Subnetz gebunden werden. Im Skript ist die front-End-Routentabelle auch Front-End-Subnetz gebunden. Hier ist das Skript Bindung für das Back-End-Subnetz.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-Weiterleitung
Companion-Funktion, UDR, ist IP-Weiterleitung. Dies ist eine Einstellung auf einer virtuellen Appliance, die Datenverkehr an die Appliance nicht gesondert behandelt und dieser Datenverkehr an das letztendliche Ziel weiterleiten kann.

Beispielsweise wenn Datenverkehr von AppVM01 Server DNS01 anfordert weitergeleitet UDR dies an den Firewall. Mit IP-Weiterleitung aktiviert werden Datenverkehr DNS01 Ziel (10.0.2.4) von der Appliance (10.0.0.4) akzeptiert und an das letztendliche Ziel (10.0.2.4) weitergeleitet. Ohne IP-Firewall aktiviert würde Datenverkehr nicht von der Appliance akzeptiert, obwohl die Routentabelle Firewall als Nächster Hop hat. 

>[AZURE.IMPORTANT] Es ist entscheidend für die IP-Weiterleitung in Verbindung mit Benutzer definiert Routing aktivieren.

IP-Weiterleitung ist ein Befehl und zur Erstellungszeit VM erfolgen. Für den Datenfluss in diesem Beispiel wird der Codeausschnitt gegen Ende des Skripts und mit den Befehlen UDR gruppiert:

1.  Rufen Sie die VM-Instanz, die in diesem Fall ist die virtuelle Appliance, die Firewall und IP-Forwarding aktivieren (Hinweis, jedes Element in Rot mit einem Dollarzeichen beginnt (z. B.: $VMName[0]) ist eine benutzerdefinierte Variable aus dem Skript im Referenzabschnitt dieses Dokuments. NULL in Klammern [0] stellt die erste VM im Array von VMs für das Beispielskript ohne Änderung, der erste virtuellen Computer (VM 0) muss die Firewall):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Netzwerk-Sicherheitsgruppen (NSG)
In diesem Beispiel eine NSG Gruppe erstellt und mit einer einzelnen Regel geladen. Diese Gruppe wird dann nur für die Front-End- und Back-End-Subnetze (nicht SecNet) gebunden. Die folgende Regel wird deklarativ erstellt:

1.  Datenverkehr (alle Ports) aus dem Internet auf das gesamte VNet (alle Subnetze) verweigert

Obwohl in diesem Beispiel NSGs verwendet werden, ist es als sekundäre Verteidigung gegen falsche manuelle Konfiguration. Wir alle eingehenden Datenverkehr aus dem Internet auf Frontend blockieren oder Teilnetze der Back-End-Datenverkehr nur über das Subnetz SecNet mit der Firewall fließen soll (und dann gegebenenfalls Frontend oder Backend-Subnetze). Darüber hinaus würde mit der UDR Regeln, Datenverkehr, der in den Subnetzen Frontend oder Backend machen, Firewall (durch UDR) weitergeleitet. Die Firewall sieht dies als eine asymmetrische Flow und ausgehenden Datenverkehr fallen würde. Daher sind drei Sicherheitsebenen schützen die Front-End- und Back-End-Subnetze. 1) keine geöffneten Endpunkte auf FrontEnd001 und BackEnd001-Clouddiensten, 2) NSGs Datenverkehr aus dem Internet (3) der Firewall ablegen asymmetrischen Datenverkehr verweigert.

Einen interessanter Punkt hinsichtlich der Netzwerk-Sicherheitsgruppe in diesem Beispiel ist, dass es nur eine Regel unten enthält Internetdatenverkehr an das gesamte virtuelle Netzwerk verweigern die Sicherheit Subnetz enthält. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Aber die NSG nur an die Subnetze Frontend und Backend gebunden ist, die Regel ist nicht verarbeitet Verkehr eingehende Sicherheit Subnetz. Daher auch wenn NSG Regel keine Internetverkehr zu einer Adresse auf der VNet die NSG nie mit Sicherheit Subnetz gebunden wurde, wird mit Sicherheit Subnetz Verkehrsfluss.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Firewall-Regeln
Die Firewall müssen Weiterleitungsregeln erstellt werden. Da die Firewall blockieren oder Weiterleitung aller eingehenden, ausgehenden und Intra-VNet Verkehr sind viele Firewall-Regeln erforderlich. Auch treffen alle eingehender Datenverkehr Sicherheitsdienst öffentliche IP-Adresse (auf verschiedene Ports) die Firewall verarbeitet werden. Empfiehlt die logische Abläufe vor dem Einrichten der Subnetze Diagramm und Firewall-Regeln zu später überarbeiten. Die folgende Abbildung zeigt eine logische Ansicht der Firewallregeln für dieses Beispiel:
 
![Logische Ansicht der Firewall-Regeln][2]

>[AZURE.NOTE] Basierend auf der Virtual Network Appliance verwendet, variiert die Management-Ports. In diesem Beispiel Barracuda NextGen Firewall verweist Häfen 22 801 807 verwendet. Finden Sie in der Herstellerdokumentation Gerät verwendeten Gerät verwendete Ports genau zu.

### <a name="logical-rule-description"></a>Logische Beschreibung
In das logische Diagramm wird Sicherheit Subnetz nicht angezeigt, da die Firewall die einzige Ressource in diesem Subnetz dieses Diagramm zeigt die Firewallregeln und wie sie logisch zulassen oder verweigern Verkehrswerte und nicht den eigentlichen Routingpfad. Externe Anschlüsse für RDP-Datenverkehr liegen gewählt reichte Ports (8014 – 8026) auch die letzten zwei Oktette der lokalen IP-Adresse Lesbarkeit etwas ausgerichtet wurden (z. B. lokale Adresse 10.0.1.4 ist mit externen Port 8014), jedoch keine höhere fehlerfreien Anschlüsse verwendet werden können.

Beispielsweise sollten die 7 Regeltypen, diese Regeltypen werden wie folgt beschrieben:

- Externe Regeln (für eingehenden Datenverkehr):
  1.    Firewallregel Management: Diese Regel App Umleitung ermöglicht Datenverkehr auf Management-Ports des virtuellen Netzwerkgerät.
  2.    RDP-Regeln (für jede Windows-Server): Diese vier Regeln (eine für jeden Server) ermöglichen Management einzelner Server über RDP. Auch kann in einer Regel je nach Netzwerk virtuelle Appliance verwendeten gebündelt werden.
  3.    Datenverkehr Regeln: Gibt es zwei Datenverkehr Anwendungsregeln, zuerst auf den front-End-Web-Verkehr und die zweite für den Back-End-Datenverkehr (z.B. Webserver auf Datenebene). Die Konfiguration dieser Regeln hängen von der Netzwerkarchitektur (wobei Server befinden) und Datenverkehr fließt (welche Richtung Verkehrswerte und welche ports verwendet werden).
      - Die erste Regel ermöglicht den Anwendungsdatenverkehr zu dem Anwendungsserver. Die Regeln können für Sicherheit, Verwaltung usw., sind Regeln externe Benutzer oder Dienste die Anwendung(en) zugreifen können. In diesem Beispiel gibt es ein einzelnen Webserver auf Port 80, damit einzelne Firewall-Anwendungsregel leiten eingehenden Datenverkehr zur externen IP Web Server internen IP-Adresse. Die Sitzung Verkehr umgeleitet hätte NAT an den internen Server sein.
      - Zweite Anwendung Datenverkehr Regel ist die Back-End Webserver mit AppVM01 Server (aber nicht AppVM02) über einen beliebigen Port zu.
- Geschäftsordnung (Intra-VNet-Datenverkehr)
  4.    Ausgehende Regel Internet: Diese Regel kann Datenverkehr von einem Netzwerk an ausgewählte Netzwerke übergeben. Diese Regel ist in der Regel eine Standardregel bereits auf die Firewall jedoch deaktiviert. Diese Regel sollte beispielsweise aktiviert werden.
  5.    DNS-Regel: Diese Regel ermöglicht nur DNS (Port 53) Datenverkehr an den DNS-Server. In dieser Umgebung, die meisten Datenverkehr von Frontend, Backend blockiert kann diese Regel speziell DNS aus lokalen Subnetz.
  6.    Subnetzen Regel: Diese Regel soll jeder Server auf dem Back-End-Subnetz Verbindung zu einem Server auf dem front-End-Subnetz (aber nicht umgekehrt).
- Ausfallsichere Regel (für Datenverkehr, der die oben entspricht):
  7.    Verweigerungsregel alle Datenverkehr: Dies sollte immer die letzte Regel (als Priorität) und so einen Datenfluss Fehler entsprechend der obigen Regeln, die von dieser Regel gelöscht wird. Dies ist eine Standardregel und normalerweise aktiviert, werden in der Regel keine Änderungen erforderlich.

>[AZURE.TIP] Für die zweite Anwendung Datenverkehr Regel alle Ports darf für einfach dieses Beispiels in einem realen Szenario spezifische Ports und Adressbereiche zum Reduzieren der Angriffsfläche von dieser Regel verwendet werden sollte.

<br />

>[AZURE.IMPORTANT] Nachdem alle oben genannten Regeln erstellt werden, ist es wichtig, überprüfen Sie die Prioritätsreihenfolge der einzelnen Regeln sichergestellt Datenverkehr zugelassen oder verweigert wie gewünscht. In diesem Beispiel sind die Regeln nach Priorität sortiert. Es ist einfach aufgrund falsch bestellten Firewall gesperrt. Mindestens sicherstellen Sie, dass die Verwaltung der Firewall selbst immer die absolute Regel mit höchster Priorität.

### <a name="rule-prerequisites"></a>Regel erforderliche Komponenten
Voraussetzung für den virtuellen Computer mit Firewall sind öffentliche Endpunkte. Die Firewall Datenverkehr verarbeiten müssen die entsprechende öffentliche Endpunkte geöffnet sein. Es gibt drei Typen von Datenverkehr in diesem Beispiel; 1) Management Datenverkehr die Firewall und Firewall-Regeln, 2) RDP-Datenverkehr auf Windows-Servern und 3) Anwendungsdatenverkehr steuern. Sind die drei Spalten Typen von Datenverkehr in der oberen Hälfte der logischen Ansicht über Firewall-Regeln.

>[AZURE.IMPORTANT] Eine wichtige emporter ist zu bedenken, dass **Alle** Datenverkehr durch den Firewall kommen. So mit Remotedesktop auf den Server IIS01, obwohl in Front-End-Cloud-Dienst die Front-End-Subnetz und Zugriff auf diese Server wir müssen die Firewall auf Port 8014 RDP, und kann die Firewall von der RDP intern IIS01 RDP-Port weitergeleitet. Schaltfläche "Verbinden" Azure-Portal wird nicht funktionieren, da keine direkten RDP Pfad IIS01 (soweit das Portal sehen kann). Dies bedeutet, dass alle Clientverbindungen aus dem Internet Security Service zu einem Port, z. B. secscv001.cloudapp.net:xxxx (Referenz der obigen Abbildung für die Zuordnung des externen Ports und interne IP-Adresse und Anschluss) werden.

Ein Endpunkt geöffnet werden bei der Erstellung von VM oder buchen Build das Beispielskript durchgeführt und in diesem Codeausschnitt unten (Hinweis; alle Artikel Anfang mit einem Dollarzeichen (z. B.: $VMName[$i]) ist eine benutzerdefinierte Variable vom Skript im Referenzabschnitt dieses Dokuments. "$i" in Klammern [$i] Array Anzahl eines bestimmten virtuellen in einem Array von VMs darstellt):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Obwohl nicht hier durch die Verwendung von Variablen jedoch Endpunkte sind **nur** auf Sicherheit Cloud-Dienst geöffnet. Dadurch wird sichergestellt, dass alle eingehender Datenverkehr verarbeitet (weitergeleitet, NAT, sank) von der Firewall.

Management-Client auf einem PC Firewall und erstellen die erforderlichen Konfigurationen installiert werden müssen. Anzeigen Sie Kreditor-Dokumentation Ihrer Firewall (oder andere NVA) zum Verwalten des Geräts Der Rest des in diesem und im nächsten Abschnitt, Firewall-Regeln erstellen, wird die Konfiguration der Firewall, durch die Kreditoren Management-Client (d. h. nicht Azure-Portal oder PowerShell) beschrieben.

Anleitung zum Client herunterladen und in diesem Beispiel verwendete Barracuda mit finden: [Barracuda NG Admin](https://techlib.barracuda.com/NG61/NGAdmin)

Nach der Anmeldung auf der Firewall jedoch vor dem Erstellen von Firewall-Regeln sind zwei erforderliche Objektklassen, die Erstellung der Regeln machen können. Netzwerk- und Service-Objekte.

Beispielsweise sollten drei benannte Objekte definierten (Subnetz Front-End-und Back-End-Subnetz auch ein Netzwerkobjekt die IP-Adresse des DNS-Servers) sein. Erstellen eines benannten; Barracuda NG Admin Client Dashboard ab, navigieren Sie zu der Konfigurationsregisterkarte, Abschnitt Betriebskonfiguration Regelsatz, dann klicken Sie auf "Netzwerke" Menü Firewall Objekte, klicken auf neu im Menü Bearbeiten Netzwerke. Das Netzwerkobjekt kann jetzt erstellt werden der Name und das Präfix:

![Erstellen Sie ein Objekt Front-End-Netzwerk][3]
 
Diese benannte Netzwerk für das Front-End-Subnetz erstellen, ein ähnliches Objekts sowie die Back-End-Subnetz erstellt werden soll. Jetzt können die Subnetze leichter nach Namen in der Firewall-Regeln verwiesen werden.

DNS-Server-Objekt:

![Ein DNS-Server-Objekt erstellen][4]

Diese IP-Adresse Verweis wird in einer DNS-Regel später verwendet.

Die zweite Voraussetzung Objekte sind Dienste. Diese stellt die RDP-Anschlüsse für jeden Server dar. Da vorhandene RDP-Dienstobjekt RDP-Standardanschluss gebunden ist, können 3389, neue Dienste geschaffen werden Datenverkehr von externen Ports (8014-8026). Die neuen Ports auch vorhandene RDP-Dienst hinzugefügt werden, aber für einfache Demonstration eine einzelne Regel für jeden Server erstellt werden. So erstellen Sie eine neue RDP-Regel für einen Server; Barracuda NG Admin Client Dashboard ab, navigieren Sie zu der Konfigurationsregisterkarte, klicken Sie im Bereich Betriebskonfiguration Regelsatz, und klicken Sie auf "Dienste" im Menü Firewall Objekte der Liste der Dienste einen Bildlauf und wählen Sie den Dienst "RDP". Mit der rechten Maustaste und wählen Sie kopieren, mit der rechten Maustaste und wählen Sie einfügen. Jetzt ist ein RDP-Copy1 Service-Objekt, das bearbeitet werden kann. RDP-Copy1 Maustaste, und wählen Sie bearbeiten, Objekt-Dienst bearbeiten Fenster wie hier gezeigt wird:

![Kopie der Standardregel RDP][5]

Die Werte können dann zum Darstellen des RDP-Dienstes für einen bestimmten Server bearbeitet werden. Für AppVM01 sollte über RDP Standardregel entsprechend einer neuen Dienstnamen, Beschreibung und externe RDP-Port verwendet das Diagramm in Abbildung 8 geändert (Hinweis: Ports sind standardmäßig RDP 3389 an den externen Anschluss für diesen speziellen Server geändert, bei AppVM01 des externen Ports 8025) der geänderte Dienst unten :

![AppVM01 Regel][6]
 
Dieser Prozess wiederholt RDP auf den restlichen Servern erstellen; AppVM02, DNS01 und IIS01. Die Erstellung dieser Dienste machen das Erstellen einfacher und klarer im nächsten Abschnitt.

>[AZURE.NOTE] RDP-Dienst für die Firewall ist aus zwei Gründen nicht erforderlich. (1) erste Firewall VM ein Linux-basierter ist SSH verwendet für die Verwaltung von virtuellen Computern anstatt RDP und 2) Anschluss 22 Port 22 und zwei Management-Ports dürfen die erste Management-Regel Management-Konnektivität zu beschrieben.

### <a name="firewall-rules-creation"></a>Firewall-Regeln erstellen
Es gibt drei Arten von Firewall-Regeln in diesem Beispiel verwendete haben unterschiedliche Symbole:

Die Regel Anwendung umleiten: ![Anwendungssymbol umleiten][7]

Ziel-NAT-Regel: ![Symbol für Ziel-NAT][8]

Die Regel Pass: ![Symbol übergeben][9]

Weitere Informationen zu diesen Regeln finden auf der Website von Barracuda.

Um die folgenden Regeln erstellen oder vorhandene Standardregeln überprüfen, ausgehend von Barracuda NG Admin Client Dashboard auf der Konfigurationsregisterkarte navigieren, in der Betriebskonfiguration Abschnitt Regelsatz auf. Ein Raster namens "Main Regeln" vorhandenen aktivierte und deaktivierten Regeln auf dieser Firewall zeigen. In der oberen rechten Ecke dieses Rasters ist ein kleines, Grün "+" Schaltfläche, klicken Sie hier, um eine neue Regel erstellen (Hinweis: Ihre Firewall möglicherweise "gesperrt" geändert, wenn eine Schaltfläche "Sperren" und nicht erstellen oder Bearbeiten von Regeln, klicken Sie auf diese Schaltfläche zu "Entsperren" des Regelsatzes bearbeiten). Wenn Sie eine vorhandene Regel ändern möchten, wählen Sie die Regel, mit der rechten Maustaste und Regel bearbeiten.

Regeln erstellt oder geändert, verschoben, der Firewall und aktiviert, wenn dies die Änderungen nicht wirksam. Die Beschreibung der Regel Details Push und Aktivierung Prozess beschrieben.

Die Einzelheiten der einzelnen Regeln erforderlich, um dieses Beispiel ausführen, sind wie folgt:

- **Firewallregel-Management**: dieser Anwendung umgeleitet Regel lässt Datenverkehr zu Management-Ports des virtuellen Netzwerk-Appliance dabei Barracuda NextGen Firewall. Die Management-Ports sind 801 807 und optional 22. Die externe und interne Ports entsprechen (d. h. keine portübersetzung). Diese Regel SETUP-Management-Zugriff ist eine Standardregel und standardmäßig aktiviert (Barracuda NextGen Firewall Version 6.1).

    ![Firewallregel-Management][10]

>[AZURE.TIP] Quelle-Adressraum in dieser Regel ist, Management IP-Adressbereiche bekannt, diesen Bereich reduzieren auch verringert die Angriffsfläche auf Management-Ports.

- **RDP-Regeln**: Diese Ziel-NAT-Regeln ermöglicht die Verwaltung der einzelnen Server über RDP.
Es gibt vier wichtige Felder musste diese Regel erstellen:
  1.    Quelle zu RDP überall den Verweis "Beliebig" wird im Feld verwendet.
  2.    Service – verwenden Sie das entsprechende Service-Objekt erstellt, in diesem Fall "AppVM01 RDP", externe Anschlüsse umleiten, die lokale IP-Adresse und Port 3386 (Standard-RDP-Port). Dieser speziellen Regel ist RDP-Zugriff auf AppVM01.
  3.    Ziel – sein *Lokaler Port in der Firewall*, "DHCP 1 lokalen IP" oder eth0 verwenden statische IP-Adressen. Die Ordinalzahl (eth0 eth1 usw.) abweichen hat Ihre Netzwerk-Appliance mehrere lokale Schnittstellen. Dies ist der Port die Firewall aus versenden (möglicherweise den Empfangsport identisch), der tatsächlichen routed werden im Feld Ziel.
  4.    Umleitung – teilt dieser Abschnitt der virtuellen Appliance, wo schließlich Datenverkehr umleiten. Die einfachste Umleitung ist IP und Port (optional) im Feld Ziel. Wird kein Port Zielport auf Anforderung werden (d.h. keine Übersetzung) verwendet, wenn ein Port den Port bezeichnet werden NAT wird zusammen mit der IP Adresse.

    ![RDP Firewallregel][11]

    Insgesamt vier RDP-Regeln müssen erstellt werden: 

  	|   Name der Regel    | Server  |   Dienst   |  Zielliste  |
  	|----------------|---------|-------------|---------------|
  	| RDP-IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP-DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP-AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP-AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Einschränken der Umfang des Quell-und Service Reduzieren der Angriffsfläche. Die meisten begrenzt, die Funktionen ermöglichen sollte verwendet werden.

- **Anwendungsregeln Datenverkehr**: Gibt es zwei Datenverkehr Anwendungsregeln, zuerst auf den front-End-Web-Verkehr und die zweite für den Back-End-Datenverkehr (z.B. Webserver auf Datenebene). Diese Regeln hängen von der Netzwerkarchitektur (wobei Server befinden) und Datenverkehr fließt (welche Richtung Verkehrswerte und welche ports verwendet werden).

    Zunächst wird die front-End-Regel für Webdatenverkehr:

    ![Web Firewallregel][12]
 
    Dieses Ziel-NAT-Regel kann den Anwendungsdatenverkehr Application Server erreichen. Die Regeln können für Sicherheit, Verwaltung usw., sind Regeln externe Benutzer oder Dienste die Anwendung(en) zugreifen können. In diesem Beispiel gibt es ein einzelnen Webserver auf Port 80, damit einzelne Firewall-Anwendungsregel leiten eingehenden Datenverkehr zur externen IP Web Server internen IP-Adresse.

    **Hinweis**: Es wurde keine Port im Feld Ziel zugewiesen, damit der eingehende Port 80 (oder 443 für den ausgewählten Dienst) verwendet die Umleitung des Webservers. Wenn der Webserver auf einem anderen Port abhört, beispielsweise port 8080, Feld Zielliste konnte 10.0.1.4:8080 Port-Umleitung zu aktualisiert werden.

    Nächste gilt Anwendung Datenverkehr Back-End-Regel zu AppVM01 Server (aber nicht AppVM02) mit dem Webserver über einen Dienst:

    ![Firewallregel-AppVM01][13]

    Diese Regel Pass ermöglicht IIS-Server Front-End-Subnetz zu den AppVM01 (IP-Adresse 10.0.2.5) an einem beliebigen Port mit einem beliebigen Protokoll von der Anwendung benötigten Daten.

    In diesem Screenshot einer "\<explizite Dest\>" wird im Feld Ziel verwendet, um 10.0.2.5 als Ziel anzugeben. Dies könnte eine explizite Namen wie dargestellt oder Netzwerkobjekt (wie die erforderlichen Komponenten für den DNS-Server). Dies ist der Administrator des Firewalls, welche Methode verwendet wird. Um eine Explict Desitnation 10.0.2.5 hinzufügen, doppelklicken Sie auf die erste leere Zeile unter \<explizite Dest\> und geben in das Fenster.

    Mit dieser Regel zu übergeben ist keine NAT erforderlich diese interne ist daher die Verbindungsmethode auf "No SNAT" festgelegt werden.

    **Hinweis**: das Quellnetzwerk in dieser Regel wird eine Ressource Subnetz FrontEnd bei nur einer oder eine bestimmte bekannte Anzahl von Webservern Netzwerkobjekt Ressource konnte erstellt werden, um genauen IP-Adressen anstelle der gesamten Frontend Subnetz.

>[AZURE.TIP] Diese Regel verwendet des Diensts "Alle" erleichtert die beispielanwendung zu nutzen, auch dadurch, dass ICMPv4 (ping) in einer einzelnen Regel. Dies ist jedoch nicht empfohlen. Die Ports und Protokolle ("Services") sollte auf ein Minimum eingeschränkt werden, die Anwendung zum Reduzieren der Angriffsfläche über diese Grenze ermöglicht.

<br />

>[AZURE.TIP] Obwohl diese Regel eine explizite Dest Referenz zeigt, sollte ein einheitliches Verfahren in der Firewallkonfiguration verwendet werden. Es wird empfohlen, benannte Netzwerkobjekt in für Lesbarkeit und-Unterstützung verwendet werden. Die explizite Dest ist hier nur verwendet, um eine alternative Referenz Methode und sollte nicht im Allgemeinen (insbesondere für komplexe Konfigurationen).

- **Ausgehende Internet Regel**: übergeben Sie diese Regel lässt Datenverkehr aus jeder Quellnetzwerk an das ausgewählte Zielnetzwerke übergeben. Diese Regel ist eine Standardregel bereits Barracuda NextGen Firewall aber deaktiviert ist. Mit der rechten Maustaste auf diese Regel möglich Befehls Regel aktivieren. Die Regel hier wurde geändert, um das Quellattribut der Regel zwei lokale Subnetze, die erstellt wurden als Verweise in den erforderlichen Abschnitt hinzufügen.

    ![Ausgehende Firewallregel][14]

- **DNS-Regel**: übergeben Sie diese Regel ermöglicht nur DNS (Port 53) Datenverkehr an den DNS-Server. In dieser Umgebung, die meisten Datenverkehr von FrontEnd, BackEnd blockiert kann diese Regel speziell DNS.

    ![DNS-Firewallregel][15]

    **Hinweis**: In diesem Bildschirm Aufnahme der enthalten ist. Da diese Regel für interne IP-Adresse, IP-Adresse Binnenverkehr ist keine NAT erforderlich ist, dies der Wert "No SNAT" für diese Regel übergeben.

- **Subnetzen Regel**: übergeben Sie diese Regel ist eine Standardregel, die aktiviert und um alle Server Back-End-Subnetz Verbindung mit jedem Server front-End-Subnetz geändert wurde. Diese Regel ist alle internen Verkehr die Verbindungsmethode auf No SNAT festgelegt werden kann.

    ![Firewallregel Intra-VNet][16]

    **Hinweis**: das Kontrollkästchen bidirektionale nicht aktiviert (noch ist es in den meisten Regeln) für diese Regel, macht dies die Regel "einseitig", eine Verbindung vom Back-End-Subnetz front-End-Netzwerk, aber nicht umgekehrt initiiert werden kann. Wenn dieses Kontrollkästchen aktiviert wurde, kann diese Regel bidirektionalen Datenverkehr die logische Diagramm nicht erwünscht ist.

- **Verweigert alle Datenverkehr Regel**: Dies sollte immer die letzte Regel (als Priorität) und so ergibt sich ein nicht überein vorangehenden Zugriffsregeln wird durch diese Regel gelöscht. Dies ist eine Standardregel und normalerweise aktiviert, werden in der Regel keine Änderungen erforderlich. 

    ![Firewall Verweigerungsregel][17]

>[AZURE.IMPORTANT] Nachdem alle oben genannten Regeln erstellt werden, ist es wichtig, überprüfen Sie die Prioritätsreihenfolge der einzelnen Regeln sichergestellt Datenverkehr zugelassen oder verweigert wie gewünscht. In diesem Beispiel sind die Regeln in der Reihenfolge im Raster Main Regeln Barracuda Management Client weitergeleitet werden soll.

## <a name="rule-activation"></a>Regel-Aktivierung
Mit der Spezifikation des Diagramms Logik geändert Regelsatz Regelsatz in der Firewall hochgeladen und aktiviert.

![Aktivierung der Firewall-Regel][18]
 
In der rechten oberen Ecke des Management-Clients sind eine Gruppe von Schaltflächen. Klicken Sie unter "Changes senden"-Schaltfläche, um die Regeln für die Firewall senden klicken Sie auf die Schaltfläche "Aktivieren".
 
Bei der Aktivierung des Firewall-Regelsatz Abschluss dieses Beispiel Umgebung erstellen.

## <a name="traffic-scenarios"></a>Datenverkehr-Szenarien
>[AZURE.IMPORTANT] Wichtige emporter ist zu bedenken, dass **Alle** Datenverkehr durch den Firewall kommen. So mit Remotedesktop auf den Server IIS01, obwohl in Front-End-Cloud-Dienst die Front-End-Subnetz und Zugriff auf diese Server wir müssen die Firewall auf Port 8014 RDP, und kann die Firewall von der RDP intern IIS01 RDP-Port weitergeleitet. Schaltfläche "Verbinden" Azure-Portal wird nicht funktionieren, da keine direkten RDP Pfad IIS01 (soweit das Portal sehen kann). Dies bedeutet, dass alle Clientverbindungen aus dem Internet Security Service zu einem Port, z. B. secscv001.cloudapp.net:xxxx werden.

Für diese Szenarien sollte die Firewall-Regeln an:

1.  Firewall-Management
2.  RDP zu IIS01
3.  RDP, DNS01
4.  RDP, AppVM01
5.  RDP, AppVM02
6.  Datenverkehr im Web App
7.  App-Datenverkehr zu AppVM01
8.  Ausgehende Internet
9.  Frontend DNS01
10. Innerhalb des Subnetzes Datenverkehr (Back-End front-End)
11. Alles ablehnen

Regelsatz eigentlichen Firewall haben wahrscheinlich viele Regeln dazu, die Regeln auf bestimmte Firewall auch andere Priorität Zahlen als die hier aufgeführten. Diese Liste und die zugehörigen Nummern sollen Relevanz zwischen nur diese elf Regeln und die relative Priorität darunter. Anders ausgedrückt; tatsächliche Firewall kann "RDP, IIS01" regelnummer 5, aber unterhalb der Regel "Firewall-Management" und über die Regel "RDP-DNS01" wäre es um dieser Liste auszurichten. Die Liste hilft auch dabei, die folgenden Szenarien weggelassen, sodass z.B. "WG Regel 9 (DNS)". Auch aus Platzgründen vier RDP-Regeln zusammen heißen, "RDP-Regeln" beim Datenverkehr Szenario RDP zusammenhängt.

Auch daran erinnern, dass Netzwerksicherheitsgruppen direkt für eingehende Internetverkehr auf den Front-End- und Back-End-Subnetzen.

#### <a name="allowed-internet-to-web-server"></a>(Zulässig) Internet-Webserver
1.  Benutzer fordert HTTP Internetseite von SecSvc001.CloudApp.Net (Internet Facing Cloud-Dienst)
2.  Cloud-Dienst übergibt Datenverkehr öffnen Endpunkt auf Port 80 auf Firewall-Schnittstelle 10.0.0.4:80
3.  Keine NSG Sicherheit Subnetz zugewiesen, so Systemregeln NSG Firewall Datenverkehr zulassen
4.  Verkehr Treffer interne IP-Adresse der Firewall (10.0.1.4)
5.  Firewall beginnt die Verarbeitung:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW Regeln 2-5 (RDP-Regeln) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regel 6 (Anwendung: Web) gilt Datenverkehr zugelassen, Firewall NAT es 10.0.1.4 (IIS01)
6.  Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) gilt nicht (Datenverkehr NAT wurde würde durch die Firewall, sodass die Quelladresse nun die Firewall Subnetz Sicherheit ist und Front-End-Subnetz NSG "local" Verkehr und ist daher zulässig), Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
7.  Iis01 Web-Datenverkehr überwacht, empfängt diese Anforderung und startet die Verarbeitung der Anforderung
8.  Iis01 versucht initiiert eine FTP-Sitzung AppVM01 Back-End-Subnetz
9.  UDR Route Frontend Subnetz sind der Firewall den nächsten hop
10. Keine ausgehenden Regeln Frontend Subnetz wird Datenverkehr zugelassen.
11. Firewall beginnt die Verarbeitung:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP-Regeln) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regel 6 (Anwendung: Web) nicht anwenden, Verschieben zum nächsten Regel
  4.    FW Regel 7 (Anwendung: Back-End) gilt, Verkehr, Firewall leitet Datenverkehr an 10.0.2.5 (AppVM01)
12. Das Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
13.  AppVM01 empfängt die Anforderung und leitet die Sitzung und reagiert
14. UDR Route Back-End-Subnetz sind der Firewall den nächsten hop
15. Da gibt es keine NSG Ausgangsregeln Subnetz Back-End-Antwort zulässig
16. Da dieser Datenverkehr auf eine bestehende Sitzung zurück übergibt die Firewall die Antwort zurück an den Webserver (IIS01)
17. Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
18. IIS-Server empfängt die Antwort, schliesst die Buchung in AppVM01 und führt anschließend die HTTP-Antwort erstellen, wird diese HTTP-Antwort an den Requestor gesendet.
19. Da gibt es keine NSG Ausgangsregeln Subnetz Frontend Antwort zulässig
20. Die HTTP-Antwort trifft die Firewall und ist dies die Antwort auf eine vorhandene NAT-Sitzung durch die Firewall angenommen
21. Die Firewall leitet die Antwort zurück an den Internet-Benutzer
22. Gibt keine ausgehenden erhält NSG Regeln UDR Hops im Front-End-Subnetz dürfen die Antwort oder die Internetbenutzer der angeforderten Webseite.

#### <a name="allowed-internet-rdp-to-backend"></a>(Zulässig) Internet RDP Backend
1.  Serveradmin Internet anfordert RDP-Sitzung zu AppVM01 über SecSvc001.CloudApp.Net:8025 8025 Benutzer Portnummer für die Firewall-Regel "RDP-AppVM01" ist
2.  Cloud-Dienst übergibt Datenverkehr öffnen Endpunkt Port 8025 Firewall-Schnittstelle 10.0.0.4:8025
3.  Keine NSG Sicherheit Subnetz zugewiesen, so Systemregeln NSG Firewall Datenverkehr zulassen
4.  Firewall beginnt die Verarbeitung:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW-Regel 2 (RDP IIS) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regel 3 (RDP DNS01) nicht anwenden, Verschieben zum nächsten Regel
  4.    FW-Regel 4 (RDP-AppVM01) gilt, Datenverkehr zugelassen, Firewall NAT es 10.0.2.5:3386 (RDP-Anschluss AppVM01)
5.  Das Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) gilt nicht (Datenverkehr wurde NAT würde durch die Firewall, sodass die Quelladresse nun die Firewall Subnetz Sicherheit ist und Back-End-Subnetz NSG "local" Verkehr und ist daher zulässig), Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
6.  AppVM01 für RDP-Datenverkehr überwacht und reagiert
7.  Keine ausgehenden Regeln NSG Standardregeln anwenden und gegenläufige Datenverkehr zulässig
8.  UDR Routen ausgehender Datenverkehr die Firewall als Nächster hop
9.  Da dieser Datenverkehr auf eine bestehende Sitzung zurück übergibt die Firewall die Antwort zurück an den Internet-Benutzer
10. RDP-Sitzung aktiviert
11. AppVM01 aufgefordert, Benutzernamenkennwort

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Zulässig) Web Server-DNS-Lookup auf den DNS-server
1.  Web-Server, IIS01, muss am www.data.gov einen feed, jedoch muss die Adresse aufgelöst.
2.  Die Konfiguration für VNet Listen DNS01 (10.0.2.4 im Back-End-Subnetz) als primärer DNS-Server, IIS01 sendet die DNS-Anforderung an DNS01
3.  UDR Routen ausgehender Datenverkehr die Firewall als Nächster hop
4.  Front-End-Subnetz keine NSG Ausgangsregeln verbunden sind, wird Datenverkehr zugelassen.
5.  Firewall beginnt die Verarbeitung:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP-Regeln) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regeln 6 & 7 (App Regeln) nicht anwenden, Verschieben zum nächsten Regel
  4.    FW Regel 8 (zum Internet) nicht anwenden, Verschieben zum nächsten Regel
  5.    FW Regel 9 (DNS) gilt, Verkehr, Firewall leitet Datenverkehr an 10.0.2.4 (DNS01)
6.  Das Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
7.  DNS-Server empfängt die Anforderung
8.  DNS-Server muss die Adresse zwischengespeichert und fragt DNS-Stammserver im Internet
9.  UDR Routen ausgehender Datenverkehr die Firewall als Nächster hop
10. Keine ausgehenden NSG-Regeln auf Back-End-Subnetz wird Datenverkehr zugelassen.
11. Firewall beginnt die Verarbeitung:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP-Regeln) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regeln 6 & 7 (App Regeln) nicht anwenden, Verschieben zum nächsten Regel
  4.     FW Regel 8 (zum Internet) gilt Datenverkehr zugelassen, Sitzung SNAT, um DNS-Stammserver im Internet
12. Internet-DNS-Server antwortet, seit dieser Sitzung von der Firewall wird die Antwort von der Firewall akzeptiert
13. Wie eine bestehende Sitzung sieht leitet die Firewall auf den ursprünglichen Server DNS01
14. Das Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
15. Der DNS-Server empfängt zwischengespeichert und reagiert auf die ursprüngliche Anforderung an IIS01
16. UDR Route Back-End-Subnetz sind der Firewall den nächsten hop
17. Keine ausgehenden NSG Regeln im Back-End-Subnetz vorhanden sind, wird Datenverkehr zugelassen.
18. Dies ist eine bestehende Sitzung auf der Firewall, die Antwort vom Firewall an den IIS-Server weitergeleitet
19. Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    Es gibt keine NSG-Regel, die auf eingehenden Datenverkehr aus dem Subnetz Backend-Front-End-Subnetz, damit keines der NSG Regeln anwenden
  2.    Die zulassen von Datenverkehr zwischen Subnetzen System Standardregel würde dieser Datenverkehr, der Datenverkehr zulässig
20. Iis01 empfängt die Antwort vom DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Zulässig) Back-End-Server Front-End-server
1.  Administrator angemeldet AppVM02 über RDP fordert eine Datei direkt vom Server IIS01 über Windows Datei-explorer
2.  UDR Route Back-End-Subnetz sind der Firewall den nächsten hop
3.  Da gibt es keine NSG Ausgangsregeln Subnetz Back-End-Antwort zulässig
4.  Firewall beginnt die Verarbeitung:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP-Regeln) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regeln 6 & 7 (App Regeln) nicht anwenden, Verschieben zum nächsten Regel
  4.    FW Regel 8 (zum Internet) nicht anwenden, Verschieben zum nächsten Regel
  5.    FW Regel 9 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  6.    FW Regel 10 (Intra-Subnetz) gilt, Verkehr, Firewall wird Datenverkehr zum 10.0.1.4 (IIS01)
5.  Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
6.  Ordnungsgemäße Authentifizierung und Autorisierung, IIS01 akzeptiert die Anfrage und antwortet
7.  UDR Route Frontend Subnetz sind der Firewall den nächsten hop
8.  Da gibt es keine NSG Ausgangsregeln Subnetz Frontend Antwort zulässig
9.  Eine bestehende Sitzung auf der Firewall ist diese Antwort zulässig und die Firewall gibt AppVM02
10. Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Standardregeln Subnetzen Datenverkehr, zugelassen, NSG Verarbeitung beenden
11. AppVM02 empfängt die Antwort

#### <a name="denied-internet-direct-to-web-server"></a>(Verweigert) Internet direkt zum Webserver
1.  Internet-Benutzer auf dem Webserver IIS01, über den FrontEnd001.CloudApp.Net-Dienst
2.  Da keine Endpunkte für HTTP-Datenverkehr geöffnet sind, würde dies nicht durch Cloud-Dienst und würde nicht erreichen server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet sind, würde NSG (Internet blockieren) im Front-End-Subnetz diesen Datenverkehr blockiert.
4.  Schließlich würde Frontend Subnetz UDR Route ausgehenden Datenverkehr die Firewall als Nächster Hop aus IIS01 senden, und die Firewall würde asymmetrischem Daten sehen und die ausgehende Antwort so gibt es mindestens drei unabhängige Sicherheitsebenen zwischen dem Internet und IIS01 über die Cloud-Dienst verhindert nicht autorisierte/Zugriff ablegen.

#### <a name="denied-internet-to-backend-server"></a>(Verweigert) Internet zum Back-End-Server
1.  Internet-Benutzer versucht, eine Datei AppVM01 über den BackEnd001.CloudApp.Net-Dienst zugreifen
2.  Da keine Endpunkte für Dateifreigabe geöffnet sind, das Cloud-Dienst nicht übergeben und den Server würde nicht erreichen
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet sind, würde NSG (Internet blockieren) diesen Datenverkehr blockiert.
4.  Schließlich würde UDR Route ausgehenden Datenverkehr die Firewall als Nächster Hop aus AppVM01 senden, und die Firewall würde asymmetrischem Daten sehen und die ausgehende Antwort so gibt es mindestens drei unabhängige Sicherheitsebenen zwischen dem Internet und AppVM01 über die Cloud-Dienst verhindert nicht autorisierte/Zugriff ablegen.

#### <a name="denied-frontend-server-to-backend-server"></a>(Verweigert) Front-End-Server auf Back-End-Server
1.  Angenommen Sie, IIS01 kompromittiert wurde und Backend-Subnetz Server scannen bösartigen Code ausgeführt wird.
2.  Frontend Subnetz UDR weiterleiten sendet ausgehenden Datenverkehr von IIS01 an die Firewall als Nächster Hop. Dies ist nicht ein beeinträchtigter virtueller Computer geändert werden können.
3.  Die Firewall würde den Datenverkehr Prozess ist eine AppVM01 oder der DNS-Server für DNS-Lookups, die Datenverkehr durch die Firewall (durch FW Regeln 7 und 9) möglicherweise dürfen konnte. FW Regel 11 (alle verweigern) würde jeder andere Datenverkehr blockiert.
4.  Wenn Erkennung auf dem Firewall aktiviert wurde (die nicht in diesem Dokument behandelt, finden Sie in der Herstellerdokumentation für Ihr Netzwerkgerät erweiterte Funktionen Bedrohung) auch Datenverkehr, der durch die in diesem Dokument beschriebenen grundlegenden Weiterleitungsregeln dürfen konnte verhindert werden, wenn der Datenverkehr enthalten bekannte Signaturen oder Muster, die eine Regel hochentwickelter kennzeichnen.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Verweigert) Internet-DNS-Lookup auf den DNS-server
1.  Internet-Benutzer versucht, einen internen DNS-Eintrag für DNS01 BackEnd001.CloudApp.Net Service suchen 
2.  Da keine Endpunkte für DNS-Datenverkehr geöffnet sind, würde dies nicht durch Cloud-Dienst und würde nicht erreichen server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet sind, würde NSG Regel (Internet blockieren) im Front-End-Subnetz diesen Datenverkehr blockiert.
4.  Schließlich die Back-End-Subnetz UDR Route sendet ausgehenden Datenverkehr von DNS01 Firewall als Nächster Hop und Firewall asymmetrischem Daten sehen und die ausgehende Antwort so gibt es mindestens drei unabhängige Sicherheitsebenen zwischen dem Internet und DNS01 über die Cloud-Dienst verhindert nicht autorisierte/Zugriff löschen würde.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Verweigert) Internet SQL Zugriff durch Firewall
1.  Internetbenutzer fordert SQL Daten von SecSvc001.CloudApp.Net (Internet Facing Cloud-Dienst)
2.  Da keine Endpunkte für SQL, das Cloud-Dienst nicht übergeben und würde nicht erreichen die firewall
3.  Wenn SQL-Endpunkte aus irgendeinem Grund geöffnet sind, würde die Firewall Verarbeitung beginnen:
  1.    FW Regel 1 (FW-Mgmt) nicht anwenden, Verschieben zum nächsten Regel
  2.    FW Regeln 2-5 (RDP-Regeln) nicht anwenden, Verschieben zum nächsten Regel
  3.    FW Regel 6 & 7 (Regeln) nicht anwenden, Verschieben zum nächsten Regel
  4.    FW Regel 8 (zum Internet) nicht anwenden, Verschieben zum nächsten Regel
  5.    FW Regel 9 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  6.    FW Regel 10 (Intra-Subnetz) nicht anwenden, Verschieben zum nächsten Regel
  7.    FW Regel 11 (alle verweigern) gilt, Datenverkehr blockiert, Stop Verarbeitung


## <a name="references"></a>Referenzen
### <a name="main-script-and-network-config"></a>Haupt-Skript und Konfiguration
Speichern Sie das vollständige Skript in einem PowerShell-Skriptdatei. Speichern Sie die Konfiguration in eine Datei namens "NetworkConf2.xml".
Ändern Sie benutzerdefinierte Variablen nach Bedarf. Führen Sie das Skript dann folgen Sie über Firewall-Regel Setup Anweisung.

#### <a name="full-script"></a>Vollständiges Skript
Dieses Skript wird basierend auf dem benutzerdefinierten Variablen:

1.  Verbinden Sie mit Azure-Abonnement
2.  Erstellen eines neuen Speicherkontos
3.  Erstellen Sie eine neue VNet und drei Subnetze im Netzwerk-Konfigurationsdatei
4.  Erstellen Sie fünf virtuelle Computer; 1 Firewall und 4 WindowsServer VMs
5.  Konfigurieren Sie UDR einschließlich:
  1.    Erstellen zwei neue Routetabellen
  2.    Routen zu den Tabellen hinzufügen
  3.    Tabellen mit entsprechenden Subnetzen binden
6.  IP-Weiterleitung auf NVA aktivieren
7.  Konfigurieren Sie NSG einschließlich:
  1.    Erstellen eine NSG
  2.    Hinzufügen einer Regel
  3.    Die entsprechenden Subnetze zuordnen die NSG

PowerShell-Skript sollte lokal auf einem PC oder Server verbunden ausgeführt werden.

>[AZURE.IMPORTANT] Wenn dieses Skript möglicherweise Warnungen oder andere PowerShell pop Informationsnachrichten. Nur Fehlermeldungen in Rot sind Anlass zur Sorge.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Netzwerk-Konfigurationsdatei
Diese XML-Datei mit aktualisierten und Datei $NetworkConfigFile Variablen im obigen Skript hinzufügen.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Beispielskripts für die Anwendung
Möchten Sie ein Beispiel für diese und andere Beispiele DMZ Installation, eine bereitgestellt wurden auf den folgenden Link: [Anwendung Beispielskript][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bidirektionale DMZ NSG, NVA und UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische Ansicht der Firewall-Regeln"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Erstellen Sie ein Objekt Front-End-Netzwerk"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Ein DNS-Server-Objekt erstellen"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopie der Standardregel RDP"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 Regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Anwendungssymbol umleiten"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ziel-NAT-Symbol"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Symbol für Pass"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewallregel-Management"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP Firewallregel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web Firewallregel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Firewallregel-AppVM01"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Ausgehende Firewallregel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS-Firewallregel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Firewallregel Intra-VNet"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Firewall Verweigerungsregel"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivierung der Firewall-Regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
