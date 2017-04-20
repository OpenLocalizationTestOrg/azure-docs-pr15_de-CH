## <a name="virtual-network-basics"></a>Virtuelles Netzwerk-Grundlagen

### <a name="what-is-an-azure-virtual-network-vnet"></a>Was ist ein Azure Virtual Network (VNet)?

Können verwenden VNets bereitstellen und Verwalten von virtuellen privaten Netzwerken (VPNs) in Azure und optional verknüpfen VNets mit anderen VNets in Azure oder Ihre lokalen IT-Infrastruktur, Hybrid oder standortübergreifend Lösungen. Jeder erstellten VNet hat eine eigene CIDR-Block und kann mit anderen VNets und lokalen Netzwerken verknüpft, als CIDR-Blocks nicht kollidieren. Sie können auch Steuerelemente DNS-Einstellungen für VNets und Segmentierung der VNet in Subnetzen.

VNets zu verwenden:

- Erstellen Sie ein dediziertes privates Cloud nur virtuelles Netzwerk

    Manchmal benötigen nicht Sie standortübergreifenden Konfiguration für Ihre Lösung. Wenn Sie ein VNet erstellen, können Ihre Dienste und VMs in Ihrem VNet direkt und sicher in der Cloud kommunizieren. Dies hält Datenverkehr sicher innerhalb der VNet jedoch noch kann Endpunkt Verbindungen für VMs und Dienste mit Internet-Kommunikation im Rahmen der Lösung konfiguriert.

- Sichere Erweiterung Ihres Rechenzentrums

    Erstellen Sie mit VNets traditionelle zwischen Standorten (S2S) VPNs Ihr Datencenter Kapazität sicher skalieren. S2S VPNs mithilfe von IPSEC eine sichere Verbindung zwischen Ihrem Unternehmen VPN-Gateway und Azure bereitstellen.

- Aktivieren der Hybriden Cloud-Szenarien

    VNets flexibel Hybriden Cloud-Szenarien zu unterstützen. Sie können sichere Cloud-basierte Anwendung für alle lokalen System wie Mainframes und Unix-Systemen.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Wie erkenne ich ein virtuelles Netzwerk erforderlich?

Besuchen Sie die [Übersicht über Virtual Network](../articles/virtual-network/virtual-networks-overview.md) um eine Entscheidungstabelle anzuzeigen, mit denen das Netzwerk Design beste für Sie entscheiden.

### <a name="how-do-i-get-started"></a>Wie fange ich an?

Besuchen Sie [virtuelle Netzwerkdokumentation](https://azure.microsoft.com/documentation/services/virtual-network/) Einstieg. Diese Seite enthält Links zu allgemeinen Konfigurationsschritte sowie Informationen, mit denen Sie die Dinge zu verstehen, die Sie berücksichtigen Ihr virtuelles Netzwerk entwerfen müssen.

### <a name="what-services-can-i-use-with-vnets"></a>Verwenden Was kann ich VNets?

VNets kann mit einer Vielzahl von verschiedenen Diensten Azure Cloud Service (PaaS), virtuelle Computer und Web Apps verwendet werden. Es gibt jedoch einige Dienste, die auf einem VNet nicht unterstützt. Überprüfen Sie den bestimmten Dienst zu verwenden und sicherzustellen, dass sie kompatibel ist.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kann ich VNets ohne standortübergreifende Verbindung verwenden?

Ja. Ein VNet können ohne Verbindung zwischen Standorten. Dies ist besonders nützlich, wenn Domänencontroller und SharePoint-Farmen in Azure ausgeführt werden soll.

## <a name="virtual-network-configuration"></a>Virtuelles Netzwerk konfigurieren

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welche Tools werden verwendet, um ein VNet zu erstellen?

Sie können die folgenden Tools erstellen oder Konfigurieren eines virtuellen Netzwerks:

- Azure-Portal (Classic und VNets Ressourcenmanager).

- Eine Netzwerk-Konfigurationsdatei (Netcfg - für klassische VNets). Finden Sie unter [Konfigurieren eines virtuellen Netzwerks mit einer Netzwerk-Konfigurationsdatei](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (Classic und VNets Ressourcenmanager).

- Azure CLI (Classic und VNets Ressourcenmanager).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welche Adressbereiche kann ich in meinen VNets verwenden?

Verwenden Sie öffentliche IP-Adressbereiche und alle IP-Adressbereich in [RFC 1918](http://tools.ietf.org/html/rfc1918)definiert.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kann ich in meinem VNets öffentliche IP-Adressen haben?

Ja. Weitere Informationen über öffentliche IP-Adressbereiche finden Sie [öffentliche IP-Adressraum in einem virtuellen Netzwerk (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Denken Sie daran, dass die öffentliche IP-Adressen nicht direkt aus dem Internet.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Gibt es eine Beschränkung für die Anzahl von Teilnetzen virtuellen Netzwerk?

Es gibt keine Beschränkung für die Anzahl der Subnetze in einem VNet verwenden. Alle Subnetze müssen vollständig in den Adressraum des virtuellen Netzwerks enthalten und sollte nicht überschneiden.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Gibt es Einschränkungen auf IP-Adressen in diesen Subnetzen?

Azure reserviert einige IP-Adressen innerhalb jedes Subnets. Die ersten und die letzten IP-Adressen der Subnetze sind Protokoll Konformität mit 3 weitere Adressen für Azure Services vorbehalten.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Wie kleine und große wie VNets und Subnetzen.

Das kleinste Subnetz unterstützt ist ein /29 und ist ein /8 (mit CIDR Subnetzdefinitionen).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Kann ich meine VLANs in Azure bringen VNets verwenden?

Nein. VNets sind Layer-3-Overlays. Azure unterstützt keine Layer-2-Semantik.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kann ich benutzerdefinierte Routingrichtlinien VNets und Subnetze angeben?

Ja. Sie können Benutzer definiert Routing (UDR). Weitere Informationen zu UDR finden Sie auf [Benutzer definiert und IP-Weiterleitung](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Unterstützen VNets Multicast- oder broadcast?

Nein. Wir unterstützen keine Multicast- oder broadcast.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welche Protokolle können in VNets werden verwendet?

Sie können standardmäßige IP-basierte Protokolle in VNets. Multicast, broadcast, IP-IP gekapselte Pakete und Paketen (GRE = Generic Routing Encapsulation) sind jedoch in VNets blockiert. Arbeiten Standardprotokollen gehören:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kann mein Standard-Router in einem VNet ping?

Nein.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Kann ich Tracert Konnektivität Diagnose verwenden?

Nein.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Kann ich nach der Erstellung des Vnets Subnetze hinzufügen?

Ja. Subnetze können VNets jederzeit hinzugefügt werden, solange die Subnetzadresse nicht zu einem anderen Subnetz in der VNet gehört.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kann ich ändern die Größe meiner Subnetz nachdem ich erstellen?

Hinzufügen können, entfernen, erweitern oder verkleinern ein Subnetzes, wenn keine VMs Dienste oder mithilfe von PowerShell-Cmdlets oder die Datei NETCFG darin bereitgestellt. Sie können hinzufügen, entfernen, erweitern oder verkleinern keine Präfixe als Subnetze, die VMs oder Dienstleistungen umfassen nicht von dieser Änderung betroffen sind.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kann ich Subnetze ändern, nachdem ich sie erstellt?

Ja. Sie können hinzufügen, entfernen und Ändern von einem VNet verwendeten CIDR-Blocks.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Kann ich mit dem Internet anschließen, wenn ich meine Dienste in einem VNet verwende?

Ja. Alle Dienste innerhalb einer VNet können mit dem Internet verbinden. Jeder Cloud-Dienst in Azure bereitgestellt hat öffentlich adressierbare VIP zugewiesen. Sie müssen input Endpunkte PaaS-Rollen und Endpunkten für virtuelle Maschinen zum Aktivieren dieser Dienste Clientverbindungen aus dem Internet akzeptieren.

### <a name="do-vnets-support-ipv6"></a>Unterstützen VNets IPv6?

Nein. Sie können nicht zu diesem Zeitpunkt IPv6 mit VNets verwenden.

### <a name="can-a-vnet-span-regions"></a>Kann ein VNet Bereiche umfassen?

Nein. Ein VNet ist auf einen einzelnen Bereich.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kann ein VNet mit anderen VNet in Azure werden verbinden?

Ja. Sie können VNet VNet Kommunikation mithilfe von REST-APIs oder Windows PowerShell erstellen. Sie können auch VNets über VNet Peering. Weitere Informationen über peering [hier.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Auflösung (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Was sind DNS-Optionen für VNets?

Entscheiden Sie anhand der Tabelle auf [Auflösung für VMs und Instanzen](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) , durch alle verfügbaren DNS-Optionen führt.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kann ich für ein VNet DNS-Server angeben?

Ja. Sie können IP-Adressen für DNS-Server den VNet angegeben. Dies wird als die Standard-DNS-Server für alle virtuellen Computer in die VNet angewendet werden.

### <a name="how-many-dns-servers-can-i-specify"></a>Wie viele DNS-Server kann ich angeben?

Sie können bis zu 12 DNS-Server angeben.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Kann ich meinen DNS-Server ändern, nachdem ich das Netzwerk erstellt haben?

Ja. Sie können die Liste der DNS-Server für das VNet jederzeit ändern. Ändern Sie die Liste der DNS-Server müssen Sie alle VMs in Ihrem VNet Reihenfolge für diese neuen DNS-Server übernehmen neu starten.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Was ist DNS Azure bereitgestellt und mit VNets funktioniert?

Azure bereitgestellte DNS ist ein Multi-Tenant DNS-Dienst von Microsoft angeboten wird. Azure registriert alle VMs und Instanzen in diesem Dienst. Dieser Service bietet Auflösung des Hostnamens für VMs und Instanzen in derselben Cloud-Dienst enthalten und FQDN VMs und Instanzen in der gleichen VNet.

> [AZURE.NOTE] Es ist eine Einschränkung zu diesem Zeitpunkt auf die ersten 100 Clouddienste im virtuellen Netzwerk zwischen Mieter Auflösung mit DNS Azure bereitgestellt. Wenn Sie eigene DNS-Server verwenden, gilt diese Einschränkung nicht.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Kann ich überschreiben Meine DNS-Clienteinstellungen auf pro VM oder service Basis?

Ja. DNS-Server lassen pro Service-Cloud Netzwerk Standardeinstellungen überschreiben. Wir empfehlen jedoch Netzwerk DNS so weit wie möglich verwenden.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kann ich meinen eigenen DNS-Suffix bringen?

Nein. Sie können keine benutzerdefinierte DNS-Suffix für die VNets angeben.

## <a name="vnets-and-vms"></a>VNets und VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Kann ich ein VNet VMs bereitstellen?

Ja.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Kann ich ein VNet Linux VMs bereitstellen?

Ja. Sie können jede von Azure unterstützten Linux-Distribution bereitstellen.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Was ist der Unterschied zwischen einer öffentlichen VIP und eine interne IP-Adresse?

- Eine interne IP-Adresse ist eine Adresse, die jede VM innerhalb einer VNet durch DHCP zugewiesen ist. Es ist nicht öffentlich. Wenn ein VNet erstellt haben, wird die interne IP-Adresse aus dem Bereich zugewiesen, die im Subnetz Einstellungen von der VNet angegeben. Wenn Sie nicht über ein VNet verfügen, wird dennoch eine interne IP-Adresse zugewiesen. Die interne IP-Adresse bleibt der VM während ihrer gesamten Lebensdauer, wenn, die VM freigegeben wird.

- Öffentliche VIP ist die öffentliche IP-Adresse, die der Cloud Service oder Load Balancer zugewiesen ist. Es wird nicht direkt die VM NIC. zugewiesen Die VIP-Adresse bleibt mit Cloud-Dienst erst alle VMs zugeordnet ist, Cloud-Dienst freigegeben oder gelöscht. Nun erscheint.

### <a name="what-ip-address-will-my-vm-receive"></a>Erhalten IP-Adresse Meine VM?

- **Interne IP-Adresse:** Wenn Sie einem VNet einen virtuellen Computer bereitstellen, erhält die VM eine interne IP-Adresse aus einer internen IP-Adressen. VMs kommunizieren mit internen IP-Adressen innerhalb der VNets. Obwohl Azure dynamische interne IP-Adresse zuweist, können Sie eine statische Adresse für die VM anfordern. Weitere Informationen zu internen Adressen finden Sie auf [wie interne Adresse](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** VM ist auch mit einer VIP, obwohl eine VIP nie direkt der VM zugewiesen wird. Ein VIP ist eine öffentliche IP-Adresse, die dem Cloud-Dienst zugewiesen werden kann. Sie können optional eine VIP-Adresse für den Clouddienst reservieren.

- **ILPIP-** Sie können auch eine auf Instanzebene öffentliche IP-Adresse (ILPIP). ILPIPs sind die VM als Cloud-Dienst zugeordnet. Weitere Informationen zu ILPIPs finden Sie auf [Instanzebene öffentlichen IP-Übersicht](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kann ich eine interne IP-Adresse für eine VM reservieren, die ich später erstellen?

Nein. Eine interne IP-Adresse kann nicht reserviert werden. Ist eine interne IP-Adresse wird es eine VM oder Rolle Instanz der DHCP-Server zugewiesen werden. Die VM kann oder möglicherweise nicht die interne IP-Adresse zugewiesen werden soll. Sie können jedoch die interne IP-Adresse der erstellten VM verfügbaren internen IP-Adresse ändern.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Interne IP-Adressen ändern für virtuelle Computer in einem VNet?

Ja. Interne IP-Adressen bleiben mit der VM Lebensdauer, wenn die VM freigegeben wird. Wenn ein virtueller Computer freigegeben wird, wird die interne IP-Adresse definiert eine interne statische IP-Adresse für die VM freigegeben. Wenn die VM ist einfach beendet (und nicht den Status **beendet (Deallocated)**) bleibt die IP-Adresse der VM zugewiesenen.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Können IP-Adressen manuell zu Netzwerkkarten in VMs werden zugewiesen?

Nein. Sie müssen keine Schnittstelleneigenschaften von VMs ändern. Änderungen führen zur VM Konnektivität verlieren.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Was geschieht mit Meine IP-Adressen, wenn ich einen virtuellen Computer herunterfahren?

Nichts. Die IP-Adressen (VIP öffentliche und interne IP-Adresse) bleibt mit dem Cloud-Dienst oder VM.

> [AZURE.NOTE] Wenn Sie den virtuellen Computer einfach herunterfahren möchten, verwenden Sie nicht Verwaltungsportal dazu. Derzeit wird die Schaltfläche Herunterfahren der virtuellen Computer freigeben.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Kann ich VMs von einem Subnetz in ein anderes Subnetz in ein VNet ohne erneut verschieben?

Ja. Weitere Informationen finden Sie [hier](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kann ich für meine VM eine statische MAC-Adresse konfigurieren?

Nein. Eine MAC-Adresse kann nicht statisch konfiguriert.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Bleibt die MAC-Adresse für meine VM nach dem Erstellen?

Ja, die MAC-Adresse bleibt für eine VM, obwohl die VM (freigegeben) beendet wurde und neu.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kann ich mit dem Internet in ein VNet eines virtuellen Computers anschließen?

Ja. Alle Dienste innerhalb einer VNet können mit dem Internet verbinden. Außerdem hat jeder Cloud-Dienst in Azure bereitgestellt öffentlich adressierbare VIP zugewiesen. Input Endpunkte PaaS-Rollen und Endpunkten für VMs zum Aktivieren dieser Dienste Clientverbindungen aus dem Internet akzeptieren muss.

## <a name="vnets-and-services"></a>VNets und Dienste

### <a name="what-services-can-i-use-with-vnets"></a>Verwenden Was kann ich VNets?

Sie können nur Compute-Dienste innerhalb von VNets. Compute-Dienste sind auf Clouddienste (Web- und Worker-Rollen) und VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Kann Web Apps mit virtuellen Netzwerks werden verwendet?

Ja. Sie können Web-Apps in einem VNet mit ASE (App Service-Umgebung) bereitstellen. Hinzufügen, können Web Apps sicher herstellen und Zugriff auf Ressourcen in der Azure-VNet Punkt-zu-Standort-für das VNet konfiguriert haben. Weitere Informationen finden Sie unter:


- [Erstellen von Web-Apps in einer App Service-Umgebung](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Web Apps virtuelle Vernetzung](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Webapps mithilfe VNet-Integration und Hybridverbindungen](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Ein virtuelles Netzwerk Azure integrieren Sie eine Web app](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kann Cloud-Diensten mit Web- und Workerrollen (PaaS) in einem VNet bereitstellen?

Ja. Sie können in VNets PaaS-Dienste bereitstellen.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Wie wird ein VNet Bereitstellung PaaS Rollen?

Sie erreichen dies, indem den Namen VNet und Zuordnung /subnet Rolle im Konfigurationsabschnitt Network Service-Konfiguration. Sie müssen nicht die Binärdateien aktualisiert.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kann ich meine Dienste und VNets verschieben?

Nein. Services und VNets kann nicht verschoben werden. Sie müssen löschen und erneut bereitstellen, den Dienst auf einem anderen VNet.

## <a name="vnets-and-security"></a>VNets und Sicherheit

### <a name="what-is-the-security-model-for-vnets"></a>Was ist das Sicherheitsmodell für VNets?

VNets sind vollständig isoliert, und andere Dienste in der Azure-Infrastruktur gehostet. Ein VNet ist eine Vertrauensgrenze.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Können ACLs oder NSGs für meine VNets werden definiert?

Nein. Sie können keine ACLs oder NSGs zu VNets zuordnen. Allerdings ACLs definiert werden, auf input Endpunkte VMs auf einem VNets bereitgestellt und NSGs können Subnetze oder Netzwerkkarten verbunden werden.

### <a name="is-there-a-vnet-security-whitepaper"></a>Gibt es ein Whitepaper VNet Sicherheit?

Ja. Sie können [hier](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>APIs, Schemas und Tools

### <a name="can-i-manage-vnets-from-code"></a>Kann ich VNets Code verwalten?

Ja. REST-APIs können Sie um VNets und standortübergreifende Konnektivität zu verwalten. Weitere Informationen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Gibt es an Unterstützung für VNets?

Ja. Sie können PowerShell und Befehlszeile für verschiedene Plattformen verwenden. Weitere Informationen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=317721).
