<properties
   pageTitle="DMZ-Beispiel: erstellen eine einfache DMZ mit NSGs | Microsoft Azure"
   description="Erstellen einer DMZ mit Netzwerk-Sicherheitsgruppen (NSG)"
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

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Beispiel 1: erstellen eine einfache DMZ mit NSGs

[Sicherheit Grenze Best Practices Seite zurück][HOME]

In diesem Beispiel wird eine einfache DMZ vier Windows-Server und Netzwerk-Sicherheitsgruppen erstellt. Es werden auch die entsprechenden Befehle ermöglichen ein besseres Verständnis der einzelnen Schritte durchlaufen. Ist auch ein Abschnitt Datenverkehr Szenario ermöglichen eine schrittweise wie Sicherheitsebenen in der DMZ Datenverkehr durchläuft. Schließlich wird die Verweise Abschnitt der vollständige Code und Anweisung zu dieser Umgebung testen und Experimentieren mit verschiedenen Szenarien 

![Eingehende DMZ mit NSG][1]

## <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel wird ein Abonnement, das Folgendes enthält:

- Zwei cloud-Services: "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit zwei Subnetzen; "FrontEnd" und "BackEnd"
- Eine Netzwerk-Sicherheitsgruppe, die auf beiden Subnetzen
- Ein Windows-Server, der einen Webserver für die Anwendung ("IIS01")
- Zwei Windows-Servern, die Anwendung wieder beenden Server ("AppVM01", "AppVM02")
- Ein Windows-Server, die DNS-Server ("DNS01")

Im Abschnitt Verweise ist eine PowerShell-Skript, die meisten der oben beschriebenen Umgebung aufzubauen. VMs und virtuelle Netzwerke erstellen zwar das Beispielskript erledigt werden nicht ausführlich in diesem Dokument. 

Um die Umgebung zu erstellen;

  1.    Die Netzwerk-Konfiguration XML-Datei im Abschnitt Informationsquellen enthalten (aktualisiert mit Namen, Speicherort und IP-Adressen, um bestimmten Szenarios zu entsprechen)
  2.    Aktualisieren Sie die Benutzervariablen im Skript entsprechend die Umgebung, die das Skript ausgeführt werden (Abonnements, Namen usw.)
  3.    Führen Sie das Skript in PowerShell

**Hinweis**: Region erhält, der in der PowerShell-Skript muss Bereich für die Netzwerk-Konfiguration XML-Datei angezeigt.

Nach der Ausführung des Skripts erfolgreich zusätzliche optionale Schritte ergriffen werden, im Abschnitt Informationsquellen zwei Skripts der Webserver und Anwendungsserver mit einer einfachen Web-Anwendung zum Testen dieser DMZ-Konfiguration eingerichtet.

Die folgenden Abschnitte enthalten eine ausführliche Beschreibung der Netzwerk-Sicherheitsgruppen und wie in diesem Beispiel funktionieren allein Positionen des PowerShell-Skripts.

## <a name="network-security-groups-nsg"></a>Netzwerk-Sicherheitsgruppen (NSG)
Für dieses Beispiel NSG Group erstellt und dann mit sechs Regeln geladen. 

>[AZURE.TIP] Im Allgemeinen erstellen Sie bestimmten "Zulassen" Regeln zuerst und dann die allgemeineren Regeln "Verweigern" Letzte. Zugewiesene Priorität bestimmt die Regeln ausgewertet zuerst. Datenverkehr für eine bestimmte Regel gelten gefunden, werden keine weiteren Regeln ausgewertet. NSG können entweder in der eingehenden oder ausgehenden Richtung (aus der Sicht des Subnetzes) gilt.

Deklarativ, die folgenden Regeln für eingehenden Datenverkehr entstehen:

1.  Interne DNS-Verkehr (Port 53)
2.  RDP-Datenverkehr (Port 3389) aus dem Internet VM ist zulässig.
3.  HTTP-Verkehr (Port 80) über das Internet auf Webserver (IIS01)
4.  Alle (alle Anschlüsse) von IIS01 zu AppVM1 zugelassen
5.  Datenverkehr (alle Ports) aus dem Internet die gesamte VNet (beide Subnetze) verweigert.
6.  Datenverkehr (alle Ports) aus dem Front-End-Subnetz mit dem Back-End-Subnetz verweigert

Mit diesen Regeln verpflichtet jedes Subnetz wurde eine HTTP-Anforderung an den Webserver sowohl Regeln 3 aus dem Internet eingehenden (zulassen) und 5 (verweigern) gilt, da Regel 3 eine höhere Priorität hat, es gilt und Artikel 5 nicht ins Spiel kommen würde. Damit die HTTP-Anforderung an den Webserver dürfen. Wenn diese denselben Datenverkehr wurde DNS01-Server erreichen, Regel 5 (verweigern) wäre der erste und der Datenverkehr würde dürfen nicht an den Server übergeben. Regel 6 (verweigern) blockiert des Front-End-Subnetzes mit Back-End-Subnetz (außer zulässiger Datenverkehr Regeln 1 und 4), dadurch wird das Back-End-Netzwerk bei ein Angreifer-Anwendung auf die Oberfläche der Angreifer Zugriff auf das Netzwerk Backend "protected" (nur auf die Ressourcen auf dem Server AppVM01 ausgesetzt) besitzen würden.

Es wird eine standardmäßige ausgehende Regel Datenverkehr aus dem Internet. In diesem Beispiel wir ausgehenden Datenverkehr zugelassen und ausgehenden Regeln nicht ändern. Zum Sperren von Datenverkehr in beiden Richtungen ist Benutzer definiert Routing erforderlich, dies in "Beispiel 3" untersucht wird unten.

Jede Regel wird ausführlicher wie folgt (**Hinweis**: ein Element in der Liste mit einem Dollarzeichen (z. B.: $NSGName) ist eine benutzerdefinierte Variable aus dem Skript im Referenzabschnitt dieses Dokuments):

1. Zuerst muss eine Netzwerk-Sicherheitsgruppe erstellt werden, um Regeln halten:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  Die erste Regel in diesem Beispiel können DNS-Datenverkehr zwischen alle internen DNS-Server auf dem Back-End-Subnetz. Die Regel hat einige wichtige Parameter:
  - "Typ" gibt an, in welche, die Richtung diese Regel wirksam wird; Dies ist aus der Perspektive der Subnetz oder virtuellen Computer (je nachdem, wo diese NSG gebunden ist). Daher ist "Eingang" und Datenverkehr tritt das Subnetz, gilt die Regel und Datenverkehr aus dem Subnetz sind von dieser Regel nicht betroffen.
  - "Priorität" legt die Reihenfolge, in der ein Verkehrswert ausgewertet werden. Je niedriger die Zahl desto höher die Priorität. Sobald eine Regel für einen bestimmten Verkehr gilt werden keine weiteren Regeln verarbeitet. Also wenn eine Regel mit Priorität 1 Datenverkehr lässt, und eine Regel mit Priorität 2 Datenverkehr verweigert sowohl Regeln für Datenverkehr gelten und der Datenverkehr übertragen darf (da Regel 1 höher eingestuften erst wirksam und keine weiteren Regeln angewendet wurden).
  - "Aktion" gibt an, wenn von dieser Regel betroffene Verkehr blockiert bzw. zugelassen sind.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Diese Regel ermöglicht RDP-Datenverkehr aus dem Internet an den RDP-Anschluss auf jedem Server entweder Subnetz in der VNET. Diese Regel verwendet zwei Arten von Präfixen. "VIRTUAL_NETWORK" und "INTERNET". Dies ist eine einfache Möglichkeit, eine größere Kategorie Adresspräfixe Adresse.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Diese Regel ermöglicht eingehende Internetverkehr an den Webserver erreicht. Dies ist das Routingverhalten ändern. Es kann nur Datenverkehr für IIS01 übergeben. Daher wäre Datenverkehr aus dem Internet Webserver als Ziel dieser Regel zulassen und keine weiteren Regeln. (In der Regel Priorität ist 140 alle anderen eingehende Internetverkehr blockiert). Wenn Sie nur HTTP-Verkehr verarbeiten, kann diese Regel weiter eingeschränkt werden, damit nur Ziel-Port 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Diese Regel ermöglicht Datenverkehr vom Server IIS01 mit dem AppVM01-Server später Regel blockiert alle anderen Front-End-Back-End-Datenverkehr. Diese Regel verbessern, wenn der Port bekannt, die hinzugefügt werden soll. Z. B. wenn der IIS-Server nur SQL Server auf AppVM01 treffen, Ziel-Portbereich sollte geändert werden von "*" (alle), 1433 (SQL-Anschluss) so dass eingehende Angriffsfläche verkleinern auf AppVM01 sollte die Anwendung nie gefährdet.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Diese Regel verweigert Datenverkehr aus dem Internet an alle Server im Netzwerk. In Kombination mit der Regel Priorität 110 und 120 können nur eingehende Internet-Verkehr die Firewall und RDP-Ports auf andere Server und blockiert alles. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  Die letzte Regel verweigert Datenverkehr vom Front-End-Subnetz im Back-End-Subnetz. Da dies eine nur eingehende Regel darf reverse Datenverkehr (aus dem Back-End auf dem Front-End).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Datenverkehr-Szenarien
#### <a name="allowed-web-to-web-server"></a>(*Zulässig*) Web Webserver
1.  Benutzer fordert HTTP Internetseite von FrontEnd001.CloudApp.Net (Internet Facing Cloud-Dienst)
2.  Cloud-Dienst übergibt Datenverkehr auf Port 80 zur IIS01 öffnen Endpunkt (Webserver)
3.  Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, Verschieben zum nächsten Regel
  3.    NSG Regel 3 (Internet IIS01) gilt, Datenverkehr erlaubt, Stop Verarbeitung
4.  Verkehr Treffer interne IP-Adresse des Webservers IIS01 (10.0.1.5)
5.  Iis01 Web-Datenverkehr überwacht, empfängt diese Anforderung und startet die Verarbeitung der Anforderung
6.  Iis01 fordert SQL Server auf AppVM01 Informationen
7.  Keine ausgehenden Regeln Frontend Subnetz wird Datenverkehr zugelassen.
8.  Das Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, Verschieben zum nächsten Regel
  3.    NSG Regel 3 (Internet Firewall) nicht anwenden, Verschieben zum nächsten Regel
  4.    NSG Regel 4 (IIS01, AppVM01) gilt, Verkehr, Verarbeitung beenden
9.  AppVM01 die SQL-Abfrage empfängt und beantwortet
10. Da keine ausgehenden Regeln auf Back-End-Subnetz ist die Antwort zulässig.
11. Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    Es gibt keine NSG-Regel, die auf eingehenden Datenverkehr aus dem Subnetz Backend-Front-End-Subnetz, damit keines der NSG Regeln anwenden
  2.    Die zulassen von Datenverkehr zwischen Subnetzen System Standardregel würde Datenverkehr zulassen, der Datenverkehr zulässig
12. Der IIS-Server empfängt die Antwort SQL und die HTTP-Antwort abgeschlossen ist und an den Requestor gesendet
13. Gibt keine ausgehenden Regeln im Front-End-Subnetz die Antwort zulässig und Internet-Benutzer erhält die angeforderte Web-Seite.

#### <a name="allowed-rdp-to-backend"></a>(*Zulässig*) RDP Backend
1.  Serveradmin Internet anfordert RDP-Sitzung mit AppVM01 auf BackEnd001.CloudApp.Net:xxxxx, wobei Xxxxx zufällig zugewiesene Portnummer für RDP, AppVM01 ist (zugewiesene Anschluss Azure-Portal oder über PowerShell finden)
2.  Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Regel 2 (RDP) gilt, Datenverkehr erlaubt, Stop Verarbeitung
3.  Keine ausgehenden Regeln Regeln anzuwenden und gegenläufige Datenverkehr zulässig
4.  RDP-Sitzung aktiviert
5.  AppVM01 aufgefordert, Benutzernamenkennwort

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Zulässig*) Web Server-DNS-Lookup auf den DNS-server
1.  Web-Server, IIS01, muss am www.data.gov einen feed, jedoch muss die Adresse aufgelöst.
2.  Die Konfiguration für VNet Listen DNS01 (10.0.2.4 im Back-End-Subnetz) als primärer DNS-Server, IIS01 sendet die DNS-Anforderung an DNS01
3.  Keine ausgehenden Regeln Frontend Subnetz wird Datenverkehr zugelassen.
4.  Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.     NSG Regel 1 (DNS) gilt, Datenverkehr erlaubt, Stop Verarbeitung
5.  DNS-Server empfängt die Anforderung
6.  DNS-Server muss die Adresse zwischengespeichert und fragt DNS-Stammserver im Internet
7.  Keine ausgehenden Regeln auf Back-End-Subnetz wird Datenverkehr zugelassen.
8.  Internet-DNS-Server antwortet, seit dieser Sitzung intern ist die Antwort zulässig.
9.  DNS-Server zwischengespeichert und entspricht der ersten Anforderung an IIS01
10. Keine ausgehenden Regeln auf Back-End-Subnetz wird Datenverkehr zugelassen.
11. Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    Es gibt keine NSG-Regel, die auf eingehenden Datenverkehr aus dem Subnetz Backend-Front-End-Subnetz, damit keines der NSG Regeln anwenden
  2.    Die zulassen von Datenverkehr zwischen Subnetzen System Standardregel würde dieser Datenverkehr, der Datenverkehr zulässig
12. Iis01 empfängt die Antwort vom DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Zulässig*) Webserver Zugriff auf Datei auf AppVM01
1.  Iis01 fordert eine Datei AppVM01
2.  Keine ausgehenden Regeln Frontend Subnetz wird Datenverkehr zugelassen.
3.  Das Back-End-Subnetz beginnt eingehende Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, Verschieben zum nächsten Regel
  3.    NSG Regel 3 (Internet IIS01) nicht anwenden, Verschieben zum nächsten Regel
  4.    NSG Regel 4 (IIS01, AppVM01) gilt, Verkehr, Verarbeitung beenden
4.  AppVM01 erhält die Anfrage und antwortet mit Datei (vorausgesetzt, die autorisierten Zugriff)
5.  Da keine ausgehenden Regeln auf Back-End-Subnetz ist die Antwort zulässig.
6.  Front-End-Subnetz beginnt eingehende Verarbeitung:
  1.    Es gibt keine NSG-Regel, die auf eingehenden Datenverkehr aus dem Subnetz Backend-Front-End-Subnetz, damit keines der NSG Regeln anwenden
  2.    Die zulassen von Datenverkehr zwischen Subnetzen System Standardregel würde Datenverkehr zulassen, der Datenverkehr zulässig
7.  Der IIS-Server empfängt die Datei

#### <a name="denied-web-to-backend-server"></a>(*Abgelehnt*) Web auf Back-End-Server
1.  Internet-Benutzer versucht, eine Datei AppVM01 über den BackEnd001.CloudApp.Net-Dienst zugreifen
2.  Da keine Endpunkte für Dateifreigabe geöffnet sind, das Cloud-Dienst nicht übergeben und den Server würde nicht erreichen
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet sind, würde NSG Regel 5 (Internet VNet) diesen Datenverkehr blockiert.

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Abgelehnt*) Web-DNS-Lookup auf den DNS-server
1.  Internet-Benutzer versucht, einen internen DNS-Eintrag für DNS01 durch den BackEnd001.CloudApp.Net Service suchen
2.  Da gibt es keine Endpunkte für DNS, das Cloud-Dienst nicht übergeben und den Server erreichen würde nicht
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet sind, würde NSG Regel 5 (Internet VNet) Datenverkehr blockieren (Hinweis: Diese Regel 1 (DNS) gilt nicht für zwei Gründe zunächst die Quelladresse ist die diese Regel gilt nur für lokale VNet als Quelle, auch dies deshalb eine Zulassungsregel nie Datenverkehr verweigern würde)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Abgelehnt*) Web SQL Zugriff durch Firewall
1.  Internetbenutzer fordert SQL Daten von FrontEnd001.CloudApp.Net (Internet Facing Cloud-Dienst)
2.  Da keine Endpunkte für SQL, das Cloud-Dienst nicht übergeben und würde nicht erreichen die firewall
3.  Endpunkte aus irgendeinem Grund geöffnet wurden, beginnt das Front-End-Subnetz eingehende Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, Verschieben zum nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, Verschieben zum nächsten Regel
  3.    NSG Regel 3 (Internet IIS01) gilt, Datenverkehr erlaubt, Stop Verarbeitung
4.  Verkehr Treffer interne IP-Adresse der IIS01 (10.0.1.5)
5.  Iis01 ist nicht an Port 1433, keine Antwort auf die Anforderung überwacht.

## <a name="conclusion"></a>Abschluss
Dies ist ein relativ einfach Weg der Back-End-Subnetz eingehenden Datenverkehr isoliert.

Weitere Beispiele und einen Überblick über Netzwerk-Sicherheitsgrenzen finden Sie [hier][HOME].

## <a name="references"></a>Referenzen
### <a name="main-script-and-network-config"></a>Haupt-Skript und Konfiguration
Speichern Sie das vollständige Skript in einem PowerShell-Skriptdatei. Speichern Sie die Konfiguration in eine Datei namens "NetworkConf1.xml".
Ändern Sie benutzerdefinierte Variablen nach Bedarf. Führen Sie das Skript, und folgen Sie dann den Firewall Regel Setup Anweisungen im Abschnitt Beispiel 1.

#### <a name="full-script"></a>Vollständiges Skript
Dieses Skript wird basierend auf dem benutzerdefinierten Variablen.

1.  Verbinden Sie mit Azure-Abonnement
2.  Erstellen eines neuen Speicherkontos
3.  Erstellen Sie eine neue VNet und zwei Subnetze im Netzwerk-Konfigurationsdatei
4.  4 Windows Server VMs erstellen
5.  Konfigurieren Sie NSG einschließlich:
  - Erstellen eine NSG
  - Auffüllen mit Regeln
  - Die entsprechenden Subnetze zuordnen die NSG

PowerShell-Skript sollte lokal auf einem PC oder Server verbunden ausgeführt werden.

>[AZURE.IMPORTANT] Wenn dieses Skript möglicherweise Warnungen oder andere PowerShell pop Informationsnachrichten. Nur Fehlermeldungen in Rot sind Anlass zur Sorge.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
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
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
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
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
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
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
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
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Eingehende DMZ mit NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

