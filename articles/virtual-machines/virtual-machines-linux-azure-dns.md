<properties 
   pageTitle="DNS-Auflösungsoptionen für Linux VMs in Azure"
   description="Name Resolution Szenarios für Linux VMs in Azure IaaS, einschließlich DNS-Dienste, Hybrid externen DNS und bringen Ihre eigenen DNS-Server."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>Optionen für DNS-Auflösung für Linux VMs in Azure

Azure bietet DNS-Auflösung standardmäßig für alle virtuellen Computer in einem virtuellen Netzwerk enthalten. Sie können Ihre eigenen DNS-Namen Auflösung Lösung implementieren, indem Sie eigene DNS-Dienste auf Ihre Azure gehosteten virtuellen Computer konfigurieren. Die folgenden Szenarien können Sie wählen, welcher für Ihre Situation besser funktioniert.

- [Azure bereitgestellte Auflösung](#azure-provided-name-resolution)

- [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server) 

Verwendete Auflösung hängt von der wie Ihre VMs und Instanzen miteinander müssen ab.

**Die folgende Tabelle veranschaulicht Szenarien und namens Auflösung Lösungen:**

| **Szenario** | **Lösung** | **Suffix** |
|--------------|--------------|----------|
| Auflösung zwischen Instanzen oder virtuelle Computer im gleichen virtuellen Netzwerk befindet | [Azure bereitgestellte Auflösung](#azure-provided-name-resolution)| Hostnamen oder FQDN |
| Auflösung zwischen Instanzen oder virtuelle Computer befindet sich im virtuellen Netzwerken | Vom Kunden gemanagte DNS-Weiterleiten von Abfragen zwischen Vnets für die Auflösung von Azure (DNS-Proxy).  Siehe [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server)| Nur FQDN |
| Auflösung von lokalen Computer- und Namen von Instanzen oder VMs in Azure | Vom Kunden gemanagte DNS-Server (lokale Domänencontroller, lokalen schreibgeschützten Domänencontroller oder einen sekundären DNS synchronisiert mit Zonentransfer).  Siehe [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server)|Nur FQDN |
| Auflösung von Azure Hostnamen von lokalen Computern | Abfragen an Kunden verwalteten DNS-Proxyserver in den entsprechenden Vnet weiterleiten, leitet der Proxyserver Abfragen in Azure Auflösung. Siehe [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server)| Nur FQDN |
| Reverse-DNS für interne IPs | [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server) | n/a |

## <a name="azure-provided-name-resolution"></a>Azure bereitgestellte Auflösung

Mit Auflösung von öffentlichen DNS-Namen bietet Azure interne Auflösung für VMs und Instanzen, die im gleichen virtuellen Netzwerk befinden.  ARM-basierten virtuellen Netzwerken das DNS-Suffix entspricht im virtuellen Netzwerk (also der FQDN nicht benötigt wird) und DNS-Namen können NICs und virtuellen Computern zugewiesen werden. Azure bereitgestellte Auflösung keine Konfiguration erforderlich, zwar es nicht die richtige Wahl für alle Bereitstellungsszenarien, wie in der obigen Tabelle.

### <a name="features-and-considerations"></a>Funktionen und Hinweise

**Funktionen:**

- Einfache Bedienung: Azure bereitgestellten Namen Auflösung ist keine Konfiguration erforderlich.

- Der Namensauflösungsdienst gelieferten Azure ist hochverfügbar, muss zum Erstellen und Verwalten von Clustern der DNS-Server.

- Zusammen mit Ihrem eigenen DNS-Server lässt sich auf lokale und Azure Hostnamen aufzulösen.

- Auflösung wird zwischen VMs in virtuelle Netzwerke ohne den FQDN bereitgestellt. 

- Hostnamen, der die Bereitstellung am besten beschreiben können anstatt mit automatisch generierten Namen.

**Hinweise:**

- Azure erstellt DNS-Suffix kann geändert werden.

- Eigene Einträge registrieren nicht manuell.

- WINS und NetBIOS-werden nicht unterstützt.

- Hostnamen müssen DNS-kompatiblen (verwenden sie nur 0-9, a-Z und '-', kann nicht beginnen und enden mit einer '-'. Abschnitt RFC 3696 2.)

- DNS-Datenverkehr ist für jede VM gedrosselt. Dies sollte nicht die meisten auswirken.  Sicherstellen Sie Anforderung-Drosselung festgestellt wird, dass clientseitiges Zwischenspeichern aktiviert ist.  Weitere Informationen finden Sie in der [optimalen Auflösung des Azure bereitgestellt](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Abrufen von Azure bereitgestellte Auflösung
**Clientseitiges Zwischenspeichern:**

Nicht alle DNS-Abfragen wird über das Netzwerk gesendet.  Clientseitiges Zwischenspeichern kann Latenz und Verbesserung gegenüber Netzwerk ahnen von wiederkehrenden DNS-Abfragen aus einem lokalen Cache auflösen.  DNS-Datensätze eine Time To Live (TTL) Cache Datensatz so lange zu speichern, ohne Beeinträchtigung der Datensatz frische ermöglicht.  Dadurch ist ein clientseitiges Zwischenspeichern für die meisten Situationen geeignet.

Einige Linux-Distributionen enthalten standardmäßig Zwischenspeichern nicht.  Es wird empfohlen, jede Linux VM (nach überprüfen, ob es nicht bereits ein lokaler Cache) hinzufügen.

Gibt es mehrere unterschiedliche DNS-caching Pakete, beispielsweise Dnsmasq, hier sind die Schritte zur Installation von Dnsmasq auf den gängigsten Distributionen:

- **Ubuntu (verwendet Resolvconf)**:
    - Installieren Sie das Paket Dnsmasq ("Sudo apt-Get Install Dnsmasq").
- **SUSE (mit Netconf)**:
    - Installieren Sie das Paket Dnsmasq ("Sudo Zypper Installation Dnsmasq") 
    - Aktivieren Sie den Dienst Dnsmasq ("Systemctl-dnsmasq.service aktivieren") 
    - Starten Sie den Dienst Dnsmasq ("Systemctl Start dnsmasq.service") 
    - Bearbeiten Sie "/ Etc/Sysconfig/Network/Config" und NETCONFIG_DNS_FORWARDER = "", "Dnsmasq"
    - der lokale DNS-Resolver aktualisieren Sie resolv.conf ("Netconfig Update") zum Festlegen des cache
- **OpenLogic (NetworkManager verwendet)**:
    - Installieren Sie das Paket Dnsmasq ("Sudo Yum installieren Dnsmasq")
    - Aktivieren Sie den Dienst Dnsmasq ("Systemctl-dnsmasq.service aktivieren")
    - Starten Sie den Dienst Dnsmasq ("Systemctl Start dnsmasq.service")
    - "Domain Name Server 127.0.0.1; voran" hinzufügen zu "/etc/dhclient-eth0.conf"
    - Starten Sie den Netzwerkdienst ("Service Network Restart") zum Cache als die lokale DNS-Auflösung festlegen

> [AZURE.NOTE]: Das Paket 'Dnsmasq' ist nur eine der vielen DNS-Caches für Linux.  Überprüfen Sie vor der Verwendung, seine Eignung für Ihren speziellen Bedürfnissen und kein Cache installiert.

**Clientseitige Versuche:**

DNS ist hauptsächlich ein UDP-Protokoll.  Wie das UDP-Protokoll Nachrichtenübermittlung nicht, erfolgt Wiederholungslogik in das DNS-Protokoll selbst.  Jeder DNS-Client (Betriebssystem) kann andere Wiederholungslogik je nach Vorliebe Ersteller aufweisen:

 - Windows-Betriebssysteme Wiederholung nach einem und dann wieder nach anderen zwei, vier und anderen vier Sekunden. 
 - Standardmäßige Linux Setup Wiederholungsversuche nach fünf Sekunden.  Sie sollten diese fünf Mal in Intervallen von einer Sekunde erneut ändern.  

Die Standardeinstellungen auf einer Linux-VM "Cat /etc/resolv.conf und betrachten die Zeile 'Optionen' beispielsweise überprüfen:

    options timeout:1 attempts:5

Resolv.conf-Datei wird automatisch generiert und sollten nicht bearbeitet werden.  Die entsprechenden Schritte für die 'Optionen' Zeile variieren je nach Distribution:

- **Ubuntu** (verwendet Resolvconf):
    - Fügen Sie die Optionszeile ' / etc/resolveconf/resolv.conf.d/head' 
    - Führen Sie 'Resolvconf -u' aktualisieren
- **SUSE** (verwendet Netconf):
    - die NETCONFIG_DNS_RESOLVER_OPTIONS 'Timeout:1 Versuche: 5' hinzufügen = "" in "/ Etc/Sysconfig/Network/Config"-Parameter 
    - Führen Sie 'Netconfig Update aktualisieren
- **OpenLogic** (NetworkManager verwendet):
    - 'Echo "Optionen Timeout:1 Versuche: 5" ' hinzufügen ' / etc/NetworkManager/dispatcher.d/11-dhclient' 
    - Führen Sie "Dienst Netzwerk neu starten" Aktualisieren

## <a name="name-resolution-using-your-own-dns-server"></a>Auflösung über eigene DNS-server
Es gibt mehrere Situationen, wo Name Resolution Bedarf über Features hinausgehen bereitgestellt von Azure, beispielsweise wenn Sie DNS-Auflösung zwischen virtuellen Netzwerken (Vnets) benötigen.  Um dieses Szenario zu behandeln, ermöglicht Azure Sie eigene DNS-Server verwenden.  

DNS-Server in einem virtuellen Netzwerk können DNS-Abfragen an Azure rekursive Konfliktlöser zum Auflösen von Hostnamen in diesem virtuellen Netzwerk weiterleiten.  Z. B. kann andere Abfragen in Azure ein DNS-Server in Azure DNS-Abfragen für einen eigenen DNS-Zonendateien beantworten und weiterleiten.  Dadurch können virtuelle Computer Ihre Einträge in Zonendateien und Azure bereitgestellten Hostnamen (per Weiterleitung) anzuzeigen.  Zugriff auf Azure rekursive Konfliktlöser erfolgt über die virtuelle IP-Adresse 168.63.129.16.

DNS-Weiterleitung auch ermöglicht u. Vnet DNS-Auflösung und der lokale Computer zum Azure bereitgestellten Hostnamen aufzulösen.  Um eine VM-Hostnamen aufzulösen, der DNS-Server VM muss im gleichen virtuellen Netzwerk befinden und forward Hostname Abfragen in Azure konfiguriert werden.  Wie das DNS-Suffix jedes Vnet unterscheidet, können Regeln der bedingten Weiterleitung Sie DNS-Abfragen an die richtige Vnet für Auflösung senden.  Die folgende Abbildung zeigt zwei Vnets und einem lokalen Netzwerk inter Vnet DNS-Auflösung mit dieser Methode ausführen:

![Inter-Vnet DNS](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Auflösung von Azure bereitgestellten Namen verwendet interne DNS-Suffix für jeden VM erfolgt über DHCP.  Bei Ihrer eigenen Lösung namens Auflösung dieses Suffix nicht auf VMs angegeben da mit anderen DNS-beeinträchtigt.  Auf Maschinen FQDN oder das Suffix der virtuellen Computer konfigurieren, kann das Suffix mit PowerShell oder der API ermittelt:

-  Ressourcenmanagement Azure verwaltet Vnets, das Suffix über die [Netzwerkschnittstellenkarte](https://msdn.microsoft.com/library/azure/mt163668.aspx) Ressource oder führen Sie den Befehl `azure network public-ip show <resource group> <pip name>` die öffentliche IP-Adresse den FQDN des NIC einschließlich Details anzuzeigen    


Weiterleiten von Abfragen in Azure Ihren Bedürfnissen entspricht, müssen Sie eigene DNS-Lösung.  Die DNS-Lösung muss:

-  Eine entsprechende Hostname-Auflösung beispielsweise über [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md).  Hinweis Wenn DDNS möglicherweise deaktivieren DNS-Datensatz Aufräumvorgang Azure DHCP-Leases sind sehr lang und Aufräumen kann DNS-Einträge vorzeitig entfernen. 
-  Entsprechende rekursive Auflösung Auflösung externer Domänennamen zu bieten.
-  Zugänglich (TCP und UDP-Port 53) von Clients dient und auf das Internet zugreifen.
-  Vor Zugriff geschützt werden, aus dem Internet Gefahren durch äußere Einflüsse zu verringern.

> [AZURE.NOTE] Für eine optimale Leistung bei Azure VMs als DNS-Server IPv6 sollte deaktiviert und [Auf Instanzebene öffentliche IP-Adresse](../virtual-network/virtual-networks-instance-level-public-ip.md) sollte jeder DNS-Server virtueller Computer zugewiesen werden.  

