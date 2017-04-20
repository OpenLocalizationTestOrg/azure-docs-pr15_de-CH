<properties 
   pageTitle="Lösung für virtuelle Computer und Instanzen"
   description="Name Resolution Szenarien Azure IaaS Hybrid-Lösung zwischen verschiedenen Cloud-Dienste, Active Directory und über eigene DNS-server "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Auflösung für VMs und Instanzen

Je nachdem, wie Sie Azure IaaS, PaaS und Hybrid-Lösungen hosten müssen Sie virtuelle Computer und Instanzen, die Sie erstellen, um miteinander kommunizieren können. Obwohl diese Kommunikation über IP-Adressen tun kann, ist viel einfacher Namen verwenden, die leicht gespeichert und nicht ändern. 

Wenn Instanzen und VMs in Azure gehostet Domänennamen internen IP-Adressen auflösen, können sie zwei Methoden verwenden:

- [Azure bereitgestellte Auflösung](#azure-provided-name-resolution)

- [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server) (die möglicherweise Azure bereitgestellten DNS-Server Abfragen weiterleiten) 

Verwendete Auflösung hängt von der wie Ihre VMs und Instanzen miteinander müssen ab.

**Die folgende Tabelle veranschaulicht Szenarien und namens Auflösung Lösungen:**

| **Szenario** | **Lösung** | **Suffix** |
|--------------|--------------|----------|
| Auflösung zwischen Instanzen oder virtuelle Computer befindet sich im selben Cloud-Dienst oder virtuelles Netzwerk | [Azure bereitgestellte Auflösung](#azure-provided-name-resolution)| Hostnamen oder FQDN |
| Auflösung zwischen Instanzen oder virtuelle Computer befindet sich im virtuellen Netzwerken | Vom Kunden gemanagte DNS-Weiterleiten von Abfragen zwischen Vnets für die Auflösung von Azure (DNS-Proxy).  Siehe [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server)| Nur FQDN |
| Auflösung von lokalen Computer- und Namen von Instanzen oder VMs in Azure | Vom Kunden gemanagte DNS-Server (z. B. lokale Domänencontroller oder lokalen schreibgeschützten Domänencontroller synchronisiert mit Zonentransfer sekundären DNS).  Siehe [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server)|Nur FQDN |
| Auflösung von Azure Hostnamen von lokalen Computern | Abfragen an Kunden verwalteten DNS-Proxyserver in den entsprechenden Vnet weiterleiten, leitet der Proxyserver Abfragen in Azure Auflösung. Siehe [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server)| Nur FQDN |
| Reverse-DNS für interne IPs | [Auflösung über eigene DNS-server](#name-resolution-using-your-own-dns-server) | n/a |
| Auflösung von VMs oder Instanzen andere Cloud-Dienste nicht in einem virtuellen Netzwerk befindet| Nicht anwendbar. Konnektivität zwischen VMs und Instanzen in andere Cloud-Dienste wird außerhalb eines virtuellen Netzwerks nicht unterstützt.| n/a |



## <a name="azure-provided-name-resolution"></a>Azure bereitgestellte Auflösung

Mit Auflösung von öffentlichen DNS-Namen bietet Azure interne Auflösung für VMs und Instanzen, die innerhalb des gleichen virtuellen Netzwerk oder Cloud-Dienst befinden.  VMs oder Instanzen in einem Clouddienst Teilen das gleiche DNS-Suffix (daher allein Hostname ausreichend ist), aber im klassischen virtuelle Netzwerke andere Cloud Services haben verschiedene DNS-Suffixe der FQDN erforderlich ist, zwischen verschiedenen Cloud-Services zu.  Virtuelle Netzwerke in der Ressourcen-Manager-Bereitstellungsmodell das DNS-Suffix entspricht im virtuellen Netzwerk (also der FQDN nicht benötigt wird) und DNS-Namen können NICs und virtuellen Computern zugewiesen werden. Azure bereitgestellte Auflösung keine Konfiguration erforderlich, zwar es nicht die richtige Wahl für alle Bereitstellungsszenarien wie in der Tabelle oben.

> [AZURE.NOTE] Bei Web- und Workerrollen Rollen können Sie auch die internen IP-Adressen basierend auf den Namen und die Instanz Rollennummer Azure Service Management REST API mit Instanzen zugreifen. Weitere Informationen finden Sie unter [Service Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Funktionen und Hinweise

**Funktionen:**

- Einfache Bedienung: um Azure bereitgestellten Namen Auflösung ist keine Konfiguration erforderlich.

- Der Namensauflösungsdienst gelieferten Azure ist hochverfügbar, muss zum Erstellen und Verwalten von Clustern der DNS-Server.

- Kann in Verbindung mit DNS-Servern verwendet werden, an lokale und Azure Hostnamen aufzulösen.

- Auflösung wird zwischen Rolle Instanzen/VMs innerhalb derselben Cloud-Dienst ohne einen FQDN bereitgestellt.

- Auflösung wird zwischen VMs in virtuelle Netzwerke bereitgestellt, mit dem Ressourcen-Manager-Bereitstellungsmodell ohne den FQDN. Virtuelle Netzwerke im klassischen Bereitstellungsmodell erfordern den FQDN beim Auflösen von Namen in anderen Cloud-Services. 

- Hostnamen, der die Bereitstellung am besten beschreiben können anstatt mit automatisch generierten Namen.

**Hinweise:**

- Azure erstellt DNS-Suffix kann geändert werden.

- Eigene Einträge registrieren nicht manuell.

- WINS und NetBIOS-werden nicht unterstützt. (Ihre VMs in Windows Explorer nicht angezeigt werden.)

- Hostnamen müssen DNS-kompatiblen (verwenden sie nur 0-9, a-Z und '-', kann nicht beginnen und enden mit einer '-'. Abschnitt RFC 3696 2.)

- DNS-Datenverkehr ist für jede VM gedrosselt. Dies sollte nicht die meisten auswirken.  Sicherstellen Sie Anforderung-Drosselung festgestellt wird, dass clientseitiges Zwischenspeichern aktiviert ist.  Weitere Informationen finden Sie in der [optimalen Auflösung des Azure bereitgestellt](#Getting-the-most-from-Azure-provided-name-resolution).

- Nur VMs im ersten 180 Cloud-Services sind für jedes virtuelle Netzwerk im klassischen Bereitstellungsmodell registriert. Dies gilt nicht für virtuelle Netzwerke im Ressourcenmanager Bereitstellungsmodelle.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Abrufen von Azure bereitgestellte Auflösung
**Clientseitiges Zwischenspeichern:**

Nicht alle DNS-Abfragen muss über das Netzwerk gesendet werden.  Clientseitiges Zwischenspeichern kann Latenz und Verbesserung gegenüber Netzwerk ahnen von wiederkehrenden DNS-Abfragen aus einem lokalen Cache auflösen.  DNS-Datensätze eine Time To Live (TTL) ermöglicht den Cache zum Speichern des Datensatzes so lange ohne Beeinträchtigung des Datensatzes frische clientseitiges Zwischenspeichern ist für die meisten Situationen geeignet.

Standard Windows DNS-Client hat einen integrierten DNS-Cache.  Einige Linux Distributionen nicht enthalten standardmäßig Zwischenspeichern wird empfohlen einen für jede Linux VM (nach überprüfen, ob es nicht bereits ein lokaler Cache) hinzugefügt werden.

Gibt eine Anzahl von verschiedenen DNS-caching Paketen verfügbar, z. B. Dnsmasq sind hier die Schritte zur Installation von Dnsmasq auf den gängigsten Distributionen:

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

> [AZURE.NOTE] Paket "Dnsmasq" ist nur eine der vielen DNS-Caches für Linux.  Vor, überprüfen Sie die Eignung für Ihre Bedürfnisse und kein Cache installiert.

**Clientseitige Versuche:**

DNS ist hauptsächlich ein UDP-Protokoll.  Wie das UDP-Protokoll Nachrichtenübermittlung nicht, erfolgt Wiederholungslogik in das DNS-Protokoll selbst.  Jeder DNS-Client (Betriebssystem) kann andere Wiederholungslogik je nach Vorliebe Ersteller aufweisen:

 - Windows-Betriebssysteme Wiederholung nach 1 und dann wieder nach einem anderen 2, 4 und anderen 4 Sekunden. 
 - Die standardmäßige Linux Setup Versuche nach 5 Sekunden.  Es wird empfohlen, dazu 5 Mal in 1-Sekunden-Intervallen wiederholen.  

Die Standardeinstellungen auf einer Linux-VM "Cat /etc/resolv.conf und betrachten die Zeile 'Optionen' z.B. überprüfen:

    options timeout:1 attempts:5

Resolv.conf-Datei ist normalerweise automatisch generiert und sollten nicht bearbeitet werden.  Die entsprechenden Schritte für die 'Optionen' Zeile variieren je nach Distribution:

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
Gibt es unterschiedliche Situationen Bedarf Namen Auflösung über die Funktionen von Azure, beispielsweise beim Active Directory-Domänen gehen möglicherweise verwenden oder DNS-Auflösung zwischen virtuellen Netzwerken (Vnets) erforderlich.  Um diese Szenarios abzudecken, ermöglicht Azure Sie eigene DNS-Server verwenden.  

DNS-Server in einem virtuellen Netzwerk können DNS-Abfragen an Azure rekursive Konfliktlöser zum Auflösen von Hostnamen in diesem virtuellen Netzwerk weiterleiten.  Angenommen, kann andere Abfragen in Azure ein Domänencontroller (DC) in Azure ausgeführt Antworten auf DNS-Abfragen für die Domänen und weiterleiten.  Dadurch VMs Ihre lokale Ressourcen (DC) und Azure bereitgestellten Hostnamen (per Weiterleitung).  Zugriff auf Azure rekursive Konfliktlöser erfolgt über die virtuelle IP-Adresse 168.63.129.16.

DNS-Weiterleitung auch ermöglicht u. Vnet DNS-Auflösung und der lokale Computer zum Azure bereitgestellten Hostnamen aufzulösen.  Um eine VM-Hostnamen aufzulösen, der DNS-Server VM muss im gleichen virtuellen Netzwerk befinden und forward Hostname Abfragen in Azure konfiguriert werden.  Wie das DNS-Suffix jedes Vnet unterscheidet, können Regeln der bedingten Weiterleitung Sie DNS-Abfragen an die richtige Vnet für Auflösung senden.  Die folgende Abbildung zeigt zwei Vnets und einem lokalen Netzwerk inter Vnet DNS-Auflösung mit dieser Methode ausführen.  Eine Beispiel-DNS-Weiterleitung ist in [Azure Schnellstart Vorlagensammlung](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) und [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder)verfügbar.

![Inter-Vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Bei Azure bereitgestellten namensauflösung, ein interner DNS-Suffix (*. internal.cloudapp.net) wird bereitgestellt, um jede VM mit DHCP.  Auflösung von Hostnamen wird dadurch, wie der Hostnamen-Datensätze im Bereich internal.cloudapp.net.  Bei Ihrer eigenen Lösung namens Auflösung IDNS Suffix nicht auf VMs angegeben da mit anderen DNS-Architekturen (wie Domäne Szenarien) beeinträchtigt.  Stattdessen bieten wir einen Platzhalter nicht funktioniert (reddog.microsoft.com).  

Gegebenenfalls kann das interne DNS-Suffix mit PowerShell oder der API bestimmt:

-  Für virtuelle Netzwerke im Ressourcenmanager Bereitstellungsmodelle steht das Suffix über die [Netzwerkschnittstellenkarte](https://msdn.microsoft.com/library/azure/mt163668.aspx) Ressource oder das Cmdlet " [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) ".    
-  In klassischen Bereitstellungsmodelle das Suffix steht über [Erhalten Deployment API](https://msdn.microsoft.com/library/azure/ee460804.aspx) -Aufruf oder über die [Get-AzureVM-Debuggen](https://msdn.microsoft.com/library/azure/dn495236.aspx) Cmdlet.


Wenn die Weiterleitung von Abfragen in Azure Ihren Bedürfnissen entspricht, müssen Sie eigene DNS-Lösung.  Die DNS-Lösung müssen:

-  Eine entsprechende Hostname-Auflösung z.B. [DDNS](virtual-networks-name-resolution-ddns.md).  Hinweis Wenn DDNS möglicherweise deaktivieren DNS-Datensatz Aufräumvorgang Azure DHCP-Leases sind sehr lang und Aufräumen kann DNS-Einträge vorzeitig entfernen. 
-  Entsprechende rekursive Auflösung Auflösung externer Domänennamen zu bieten.
-  Zugänglich (TCP und UDP-Port 53) von Clients dient und auf das Internet zugreifen.
-  Vor Zugriff geschützt werden, aus dem Internet Gefahren durch äußere Einflüsse zu verringern.

> [AZURE.NOTE] Für eine optimale Leistung bei Azure VMs als DNS-Server IPv6 sollte deaktiviert und [Auf Instanzebene öffentliche IP-Adresse](virtual-networks-instance-level-public-ip.md) sollte jeder DNS-Server virtueller Computer zugewiesen werden.  Wenn Sie Windows Server als DNS-Server verwenden, bietet [Dieser Artikel](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) zusätzliche Performance-Analyse und Optimierung.


### <a name="specifying-dns-servers"></a>Festlegen von DNS-Servern

Wenn Sie eigene DNS-Server verwenden, ermöglicht Azure geben mehrere DNS-Server pro virtuelles Netzwerk oder Netzwerkschnittstelle (Ressourcen-Manager) oder cloud-Dienst (klassisch).  DNS-Server für eine Cloud Service-Netzwerk-Schnittstelle angegeben erhalten Vorrang über für das virtuelle Netzwerk.

> [AZURE.NOTE] Netzwerk-Eigenschaften, wie DNS-Server IP-Adressen, dürfen nicht direkt in Windows VMs bearbeitet werden, wie sie bei erhalten möglicherweise heilen virtuelle Netzwerkadapter ersetzt wird. 


Verwendung das Ressourcen-Manager-Bereitstellungsmodell können DNS-Server in der Portal-API-Vorlagen ([Vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [Nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) oder PowerShell ([Vnet](https://msdn.microsoft.com/library/mt603657.aspx), [Nic](https://msdn.microsoft.com/library/mt619370.aspx)) angegeben werden.

Bei Verwendung das klassischen Bereitstellungsmodell können DNS-Server für das virtuelle Netzwerk im Portal oder [ *Netzwerk* -Konfigurationsdatei](https://msdn.microsoft.com/library/azure/jj157100)angegeben.  Für Cloud-Services werden die DNS-Server über [die *Service* -Konfigurationsdatei](https://msdn.microsoft.com/library/azure/ee758710) oder in PowerShell ([New AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)) angegeben.

> [AZURE.NOTE] Wenn Sie die DNS-Einstellungen für einen virtuellen Netzwerk virtuellen Computer, die bereits bereitgestellt ist ändern, müssen Sie jede betroffene VM Änderungen wirksam neu starten.


## <a name="next-steps"></a>Nächste Schritte

Ressourcen-Manager-Bereitstellungsmodell:

- [Ein virtuelles Netzwerk erstellen oder aktualisieren](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Erstellen oder Aktualisieren einer Netzwerkkarte](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Neue AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Neue AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klassische Bereitstellungsmodell:

- [Konfigurationsschema Azure Service](https://msdn.microsoft.com/library/azure/ee758710)
- [Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100)
- [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerk-Konfigurationsdatei](virtual-networks-using-network-configuration-file.md) 

