<properties 
   pageTitle="Hybridverbindung mit 2-Tier-Anwendung | Microsoft Azure"
   description="Erfahren Sie, wie virtuelle Appliances und UDR zum Erstellen einer Anwendung mit mehreren Ebenen Umgebung in Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtuelle Appliance-Szenario

Häufig unter mehr Azure Kunden muss eine zweistufige Anwendung beim Zugriff auf die hintere Ebene aus einem lokalen Rechenzentrum über das Internet verfügbar. Dieses Dokument führt Sie durch ein Szenario Benutzer definierten Routen (UDR), ein VPN-Gateway und virtuelle Netzwerkgeräte eine 2-Tier-Umgebung bereitstellen, die der folgenden Mindestanforderungen erfüllt:

- Anwendung muss vom öffentlichen Internet zugänglich sein.
- Webserver der Anwendungdes muss einen Back-End-Anwendungsserver zugreifen können.
- Firewall virtuelle Appliance muss alle Datenverkehr aus dem Internet auf die Anwendung durchlaufen. Das virtuelle Gerät wird für Internet-Datenverkehr verwendet werden.
- Firewall virtuelle Appliance muss alle Datenverkehr zum Application Server durchlaufen. Das virtuelle Gerät wird für den Back-End-End-Server, und Zugriff aus dem lokalen Netzwerk über ein VPN-Gateway verwendet.
- Administratoren müssen Firewall virtuelle Appliances auf ihren lokalen Computern zu verwalten, mit firewall eine dritte virtuelle Appliance ausschließlich für Verwaltungszwecke verwendet.

Dies ist ein standard DMZ-Szenario einer DMZ mit einem geschützten Netzwerk. Szenario kann mit NSGs Firewall virtuelle Appliances oder eine Kombination beider in Azure erstellt werden. In der folgenden Tabelle werden einige der vor- und Nachteile zwischen NSGs und Firewall virtuelle appliances

||Profis|Nachteile|
|---|---|---|
|NSG|Keine Kosten. <br/>Azure RBAC integriert. <br/>Regeln können in ARM-Vorlagen erstellt werden.|Komplexität konnte in einer größeren Umgebung variieren. |
|Firewall|Vollzugriff auf Datenebene. <br/>Zentrales Management über Firewall-Konsole.|Kosten der Firewall-Appliance. <br/>Nicht integriert in Azure RBAC.|

Die Lösung wird mit Firewall virtuelle Appliances eine DMZ/geschützte Szenario implementiert.

## <a name="considerations"></a>Hinweise

Sie können in Azure verwenden unterschiedliche Funktionen heute, erläuterte Umgebung bereitstellen.

- **Virtual Network (VNet)**. Ein Azure-VNet ähnlich wie mit einem lokalen Netzwerk fungiert und kann in einem oder mehreren Subnetzen Datenverkehr isoliert und Trennung von Bereichen aufgeteilt.
- **Virtuelle Appliance**. Verschiedene Partner bieten virtuelle Appliances in Azure-Markt, die für die drei oben beschriebenen Firewalls verwendet werden kann. 
- **Benutzerdefinierte Routen (UDR)**. Routetabellen können Decision zur Azure-Netzwerken Steuerung von Paketen in einem VNet enthalten. Subnetze können diese Routetabellen zugewiesen. Die neuesten Funktionen in Azure gehört die Fähigkeit, eine Routentabelle Subnetzname die Möglichkeit, den Datenverkehr in Azure VNet Hybrid-Verbindung zu einem virtuellen Gerät weiterleiten.
- **IP-Weiterleitung**. Standardmäßig Azure networking Engine weiterleiten Pakete virtuelle Netzwerkkarten (NICs) nur, wenn die Paket IP-Zieladresse NIC IP-Adresse übereinstimmt Daher ein UDR definiert, dass ein Paket an bestimmte virtuelle Appliance gesendet werden muss, würden Azure networking Engine, verworfen. Um sicherzustellen, dass das Paket einer VM (in diesem Fall virtuelle Appliance) übermittelt, das nicht das eigentliche Ziel des Pakets ist, müssen Sie die IP-Weiterleitung für die virtuelle Appliance aktivieren.
- **Netzwerk-Sicherheitsgruppen (NSGs)**. Im folgenden Beispiel wird nicht nutzen NSGs, jedoch können Sie in dieser Lösung Subnets oder Netzwerkkarten angewendet NSGs um Datenverkehr die Subnetze und Netzwerkkarten, weiter zu filtern.


![IPv6-Konnektivität](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

In diesem Beispiel wird ein Abonnement, das Folgendes enthält:

- 2 Ressourcengruppen nicht im Diagramm angezeigt. 
    - **ONPREMRG**. Enthält alle Ressourcen für ein lokales Netzwerk simulieren.
    - **AZURERG**. Enthält alle Ressourcen für Azure virtuelle Netzwerk. 
- Ein VNet namens **Onpremvnet** verwendet, um einem lokalen Rechenzentrum segmentiert wie nachstehend imitieren.
    - **onpremsn1**. Subnetz mit einem virtuellen Computer (VM) unter Ubuntu auf einen lokalen Server zu simulieren.
    - **onpremsn2**. Subnetz mit einer VM unter Ubuntu imitieren einen lokalen Computer verwendet werden.
- Es ist ein virtuelles Firewallgerät namens **OPFW** auf **Onpremvnet** verwendet, um einen Tunnel zum **Azurevnet**verwalten.
- Ein VNet mit dem Namen **Azurevnet** segmentiert wie unten aufgeführt.
    - **azsn1**. Externen Firewall Subnetz ausschließlich für die externe Firewall verwendet. Alle Internet-Datenverkehr wird durch dieses Subnetz kommen. Diesem Subnetz enthält nur eine Netzwerkkarte, die mit dem externen Firewall verbunden.
    - **azsn2**. Front-End-Subnetz hostet eine VM als Webserver, der über das Internet zugegriffen werden.
    - **azsn3**. Back-End-Subnetz hostet einen virtuellen Computer mit einem Back-End-Anwendungsserver, der vom front-End-Webserver zugegriffen wird.
    - **azsn4**. Management-Subnetz ausschließlich auf Management-Zugriff auf alle virtuellen Firewall-Geräte verwendet. Diesem Subnetz enthält nur eine NIC für jede virtuelle Firewall-Appliance Lösung verwendet.
    - **Subnetzname**. Azure Hybrid Verbindung Subnetz Konnektivität zwischen Azure VNets und anderen Netzwerken für die ExpressRoute und VPN-Gateway erforderlich. 
- Es gibt 3 Firewall virtuellen Geräte im Netzwerk **Azurevnet** . 
    - **AZF1**. Externe Firewall im öffentlichen Internet mit einer öffentlichen IP-Adressenressource in Azure ausgesetzt. Sie müssen sicherstellen, dass Sie eine Vorlage aus dem Markt oder direkt von der Appliance Lieferanten dieser Vorschriften virtuelle Appliance 3-NIC haben.
    - **AZF2**. Internen Firewall Datenverkehr zwischen **azsn2** und **azsn3**. Dies ist auch eine virtuelle Appliance 3-NIC.
    - **AZF3**. Management-Firewall für Administratoren von lokalen Datencenter und verwendet, um alle Firewall-Geräte verwalten Verwaltung Subnetz verbunden. 2 NIC virtuelle Appliance Vorlagen auf dem Markt finden oder direkt vom Hersteller Geräts anfordern.

## <a name="user-defined-routing-udr"></a>Benutzerdefinierte Routing (UDR)

Jedes Subnetz in Azure kann verknüpft werden, zu einer UDR Tabelle definiert, wie Datenverkehr initiiert, Subnetz weitergeleitet wird. Wenn keine Decision definiert sind, verwendet Azure Standardrouten-Datenverkehr von einem Teilnetz zu einem anderen. Zum besseren Verständnis Decision besuchen Sie [was Benutzer definiert und IP-Weiterleitung](./virtual-networks-udr-overview.md#ip-forwarding).

Um sicherzustellen, dass die Kommunikation erfolgt über die gute Firewall-Appliance basierend auf der letzten Anforderung oben müssen Sie die folgende Route mit Decision **Azurevnet**erstellen.

### <a name="azgwudr"></a>azgwudr

In diesem Szenario muss der nur Datenverkehr von lokalen Azure verwaltet die Firewalls mit **AZF3**verwendet werden, und Datenverkehr durch die interne Firewall und **AZF2**. Also nur einen **Subnetzname** wie folgt erforderlich.

|Ziel|Nächster hop|Erklärung|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Lokalen Datenverkehr zu Management Firewall **AZF3**|

### <a name="azsn2udr"></a>azsn2udr

|Ziel|Nächster hop|Erklärung|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Datenverkehr an das Back-End-Subnetz hosting Anwendungsserver durch **AZF2**|
|0.0.0.0/0|10.0.2.10|Lässt den gesamten Datenverkehr durch **AZF1** weitergeleitet werden|

### <a name="azsn3udr"></a>azsn3udr

|Ziel|Nächster hop|Erklärung|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Kann Datenverkehr **azsn2** vom Anwendungsserver zu Webserver durch **AZF2** übertragen|

Sie möchten Routetabellen für Subnetze in **Onpremvnet** lokalen Datencenter imitieren erstellen.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Ziel|Nächster hop|Erklärung|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Datenverkehr, **onpremsn2** bis **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Ziel|Nächster hop|Erklärung|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Ermöglicht gesicherte Subnetz-Datenverkehr in Azure durch **OPFW**|
|192.168.1.0/24|192.168.2.4|Datenverkehr, **onpremsn1** bis **OPFW**|

## <a name="ip-forwarding"></a>IP-Weiterleitung 

UDR und IP-Weiterleitung sind Funktionen, mit denen Sie, in Kombination mit virtuellen Geräte verwendet werden, um Datenverkehr in einem Azure-VNet steuern können.  Virtuelle Appliance ist nichts anderes als ein virtueller Computer, die eine Anwendung zur Behandlung von Netzwerkverkehr gewissermaßen eine Firewall oder ein NAT-Gerät verwendet.

Diese virtuelle Appliance VM muss eingehenden Datenverkehr empfangen, der sich nicht behandelt. Damit eine VM an andere Ziele gerichtete Datenverkehr empfangen können, müssen Sie die IP-Weiterleitung für den virtuellen Computer aktivieren. Dies ist ein Azure, keine Einstellung im Gastbetriebssystem. Die virtuelle Appliance muss irgendeine Anwendung eingehenden Datenverkehr und entsprechend weiterleiten ausführen.

Weitere Informationen zu IP-Weiterleitung finden Sie auf [welche Benutzer definiert und IP-Weiterleitung](./virtual-networks-udr-overview.md#ip-forwarding).

Beispielsweise genommen Sie an, das folgende Setup in Azure Vnet haben:

- Subnetz **onpremsn1** enthält einen virtuellen Computer mit dem Namen **onpremvm1**.
- Subnetz **onpremsn2** enthält einen virtuellen Computer mit dem Namen **onpremvm2**.
- **Onpremsn1** und **onpremsn2**mit dem Namen **OPFW** virtuelle Appliance verbunden.
- Eine benutzerdefinierte Route verknüpft **onpremsn1** gibt an, dass alle Datenverkehr **onpremsn2** an **OPFW**gesendet werden muss.

An diesem Punkt **onpremvm1** versucht, eine Verbindung mit **onpremvm2**herzustellen, die UDR verwendet und Datenverkehr zu **OPFW** als Nächster Hop. Denken Sie daran, dass das eigentliche Ziel nicht geändert wird, sagt er noch **onpremvm2** Ziel. 

Ohne IP- **OPFW**aktiviert wird Azure virtual Netzwerklogik die Pakete verwirft, da es nur Pakete an einer VM die VM-IP-Adresse ist das Ziel für das Paket.

Beim Weiterleiten von IP-leiten Logik Azure virtuelles Netzwerk Pakete, OPFW, ohne seine ursprüngliche Zieladresse. **OPFW** müssen die Pakete behandeln und welche mit.

Für dieses Szenario arbeiten, Sie müssen IP-Weiterleitung auf den NICs für **OPFW**, **AZF1**, **AZF2**und **AZF3** , die für das routing verwendet werden (alle NICs mit Ausnahme der Management-Subnetz verbunden). 

## <a name="firewall-rules"></a>Firewall-Regeln

Wie oben beschrieben sicher IP-Weiterleitung nur Pakete an die virtuellen Geräte gesendet werden. Ihre Anwendung muss entscheiden, was mit diesen Paketen. Im oben beschriebenen Szenario müssen Sie die folgenden Regeln in Ihre Appliances erstellen:

### <a name="opfw"></a>OPFW

OPFW stellt einen lokalen Gerät mit den folgenden Regeln:

- **Route**: gesamten Datenverkehr an 10.0.0.0/16 (**Azurevnet**) **ONPREMAZURE**Tunnel gesendet werden muss.
- **Richtlinie**: alle bidirektionalen Datenverkehr zwischen **Anschluss2** und **ONPREMAZURE**zulassen.
 
### <a name="azf1"></a>AZF1

AZF1 stellt eine Azure virtual Appliance enthält Folgendes:

- **Richtlinie**: alle bidirektionalen Datenverkehr zwischen **port1** und **Anschluss2**zulassen.

### <a name="azf2"></a>AZF2

AZF2 stellt eine Azure virtual Appliance enthält Folgendes:

- **Route**: alle Datenverkehr an 10.0.0.0/16 (**Onpremvnet**) muss der Azure-Gateway IP-Adresse (z. B. 10.0.0.1) durch **port1**gesendet.
- **Richtlinie**: alle bidirektionalen Datenverkehr zwischen **port1** und **Anschluss2**zulassen.

## <a name="network-security-groups-nsgs"></a>Netzwerk-Sicherheitsgruppen (NSGs)

In diesem Szenario werden NSGs nicht verwendet wird. Sie können jedoch NSGs für jedes Subnetz beschränkt eingehenden und ausgehenden Datenverkehr anwenden. Beispielsweise können Sie externe FW-Teilnetz NSG Folgendes zuweisen.

**Eingehende**

- Alle TCP-Datenverkehr aus dem Internet auf Port 80 auf alle virtuellen Computer im Subnetz zulassen
- Verweigern Sie anderer Datenverkehr aus dem Internet.

**Ausgehende**
- Verweigern Sie allen Datenverkehr mit dem Internet.

## <a name="high-level-steps"></a>High-Level-Schritte

Um dieses Szenario bereitstellen, gehen Sie auf hoher Ebene.

1.  Melden Sie Ihre Azure-Abonnement.
2.  Bereitstellen Sie VNet zum lokalen Netzwerk imitieren bereitstellen möchten, die Ressourcen, die Teil des **ONPREMRG**.
3.  Bereitstellungsressourcen Sie **AZURERG**gehören.
4.  Bestimmung der Tunnel vom **Onpremvnet** zum **Azurevnet**.
5.  Sobald alle Ressourcen bereitgestellt werden, melden Sie sich bei **onpremvm2** und ping 10.0.3.101 zum Testen der Konnektivität zwischen **onpremsn2** und **azsn3**.
