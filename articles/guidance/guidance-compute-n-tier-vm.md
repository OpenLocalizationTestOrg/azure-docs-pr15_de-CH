<properties
   pageTitle="Windows-VMs für N-Tier-Architektur mit | Referenzarchitektur | Microsoft Azure"
   description="Wie Sie eine mehrstufige Architektur auf Azure besondere Aufmerksamkeit auf Verfügbarkeit, Sicherheit, Skalierung und Verwaltung Sicherheit implementieren."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Auf Windows Azure ausgeführte Windows-VMs für N-Tier-Architektur

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [N-Tier-Architektur auf Azure Linux VMs ausführen](guidance-compute-n-tier-vm-linux.md)
- [Auf Windows Azure ausgeführte Windows-VMs für N-Tier-Architektur](guidance-compute-n-tier-vm.md)

Dieser Artikel beschreibt eine Reihe von bewährten Methoden zum Ausführen von Windows virtuelle Maschinen (VMs) für eine Anwendung mit N-Tier-Architektur. Baut auf Artikel [mehrere VMs auf Azure ausführen][multi-vm].

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. In diesem Artikel verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

## <a name="architecture-diagram"></a>Architekturdiagramm

Es gibt Variationen des N-Ebenen-Architekturen. Zum größten Teil sollte nicht die Unterschiede im Sinne dieser Empfehlung wichtig. Regel 3-Tier-Web app vorausgesetzt:

- **Die Webebene.** HTTP-Anfragen behandelt. Antworten werden durch diese Ebene zurückgegeben.

- **Geschäftsebene.** Implementierung von Geschäftsprozessen und andere funktionale Logik für das System.

- **Die Datenbankebene.** Bietet permanente Datenspeicher [SQL Server immer auf Verfügbarkeitsgruppen] mit[ sql-alwayson] für hohe Verfügbarkeit.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Compute - Multi-Tier (Windows) Seite.

![[0]][0]

- **Verfügbarkeit legt.** Erstellen Sie eine [Verfügbarkeit] [ azure-availability-sets] für jedes tier und mindestens zwei VMs pro Ebene bereitstellen. Dies ist erforderlich, damit Verfügbarkeits- [SLA] [ vm-sla] für virtuelle Computer.

- **Subnetze.** Erstellen Sie ein separates Subnetz für jede Ebene. Geben Sie die Adressbereich und Subnetzmaske Adressmaske [CIDR] -Notation verwenden. 

- **Lastenausgleich.** Ein [System zum Lastenausgleich Internetzugriff] verwenden[ load-balancer-external] Internetverkehr die Webebene und eine [interne Lastenausgleich] verteilt[ load-balancer-internal] Netzwerkverkehr von der Webebene auf Geschäftsebene verteilen.

- **Jumpbox**. Eine _Jumpbox_, auch als [Bastion-Host]ist ein virtueller Computer auf dem Netzwerk, mit dem Administratoren die VMs herstellen. Die Jumpbox hat eine NSG, die remote Verkehr nur von zugelassenen öffentliche IP-Adressen zulässt. Die NSG sollte Remotedesktop (RDP) Datenverkehr zuzulassen.

- **Überwachen**. Überwachungssoftware wie [Nagios], [Zabbix]oder [Icinga] erhalten Sie Einblick in die Reaktionszeit, VM Betriebszeit und den Gesamtzustand des Systems. Installieren Sie die Überwachungssoftware auf einem virtuellen Computer in einem separaten Management-Subnetz befindet.

- **NSGs**. Verwenden Sie [netzwerksicherheitsgruppen] [ nsg] (NSGs) Beschränkung des Netzwerkverkehrs in der VNet. Beispielsweise in der hier gezeigten 3-Tier-Architektur akzeptiert die Datenbankebene Datenverkehr vom Web-front-End von Geschäftsebene und Management-Subnetz keine.

- **SQL Server immer auf Availability Group.** Bietet hohe Verfügbarkeit auf der Datenebene durch Replikation und Failover.

- **Active Directory Domain Services (AD DS)-Server**. Active Directory-Domänendienste (AD DS) speichert Verzeichnisdaten und verwaltet die Kommunikation zwischen Benutzern und Domänen, einschließlich Benutzeranmeldeprozesse, Authentifizierung und Verzeichnissuchen. Active Directory-Domänencontroller ist ein Server mit AD DS. Vor Windows Server 2016 müssen immer auf Verfügbarkeitsgruppen einer Domäne angehören. Ist Windows Server Failover Cluster (WSFC) Technologie Verfügbarkeitsgruppen abhängen. Windows Server 2016 stellt die Möglichkeit zum Erstellen eines Failoverclusters ohne Active Directory. Weitere Informationen finden Sie unter [Neuigkeiten bei Failoverclustering in Windows Server 2016][wsfc-whats-new]

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie erstellen eigene Referenzarchitektur folgen diese Empfehlung nur eine Anforderung, die Empfehlung nicht passen

### <a name="vnet--subnets"></a>VNet / Subnetze

Beim Erstellen des Vnets reservieren Sie genügend Adressraum für die Subnetze, die Sie benötigen. Geben Sie die VNet Adressbereich und Subnetzmaske Adressmaske [CIDR] -Notation verwenden. Verwenden Sie einen Adressraum der standardmäßigen [private IP-Adresse blockiert innerhalb][private-ip-space], 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 sind.

Wählen Sie einen Adressbereich, der nicht mit dem lokalen Netzwerk überschneidet bei Gateway zwischen der VNet und das lokale Netzwerk später einrichten möchten. Nach Erstellung das VNet können Sie den Adressbereich nicht ändern.

Entwerfen Sie Subnetze Funktionalität und Sicherheit Anforderungen. Alle VMs innerhalb derselben Ebene oder Rolle gehen in demselben Subnetz eine Sicherheitsgrenze erfolgen kann. Weitere Informationen zum Entwerfen von VNets und Subnetzen finden Sie unter [Planen und Entwerfen Azure Virtual Networks][plan-network].

Geben Sie für jedes Subnetz den Adressbereich für das Subnetz in CIDR-Notation. '10.0.0.0/24' erstellt z. B. einen Bereich von 256 IP-Adressen. (VMs können diese 251, fünf reserviert. [Virtuelles Netzwerk FAQ]finden Sie unter[vnet faq].) Sicherstellen Sie, dass die Adressbereiche über Subnetze hinweg nicht überlappen.

### <a name="network-security-groups"></a>Netzwerk-Sicherheitsgruppen

Verwenden Sie NSG Regeln Verkehr zwischen eingeschränkt. Z. B. in der obigen 3-Tier-Architektur kommuniziert die Webebene nicht direkt mit der Datenbank. Um dies zu erzwingen, sollte die Datenbankebene eingehenden Datenverkehr aus dem Subnetz Web-Tier blockieren.  

  1. Erstellen einer NSG und Subnetz Tier Datenbank zuordnen.

  2. Hinzufügen einer Regel, die den gesamten eingehenden Datenverkehr aus der VNet verweigert. (Mit dem `VIRTUAL_NETWORK` Tag in der Regel.) 

  3. Hinzufügen einer Regel mit höherer Priorität, der eingehenden Datenverkehr aus dem Subnetz Business Ebene ermöglicht. Diese Regel überschreibt die vorherige Regel und ermöglicht die Geschäftsebene mit der Datenbankebene.

  4. Hinzufügen einer Regel, die Datenverkehr aus der Datenbank Tier im Subnetz ermöglicht. Diese Regel ermöglicht die Kommunikation zwischen virtuellen Computern auf der Datenbankebene für Replikation und Failover.

  5. Fügen Sie eine Regel RDP-Datenverkehr aus dem Subnetz Jumpbox. Diese Regel ermöglicht Administratoren die Datenbankebene aus der Jumpbox herstellen.

  > [AZURE.NOTE] Eine NSG [Standardregeln] hat[ nsg-rules] eingehenden Datenverkehr von innerhalb der VNet zulassen. Diese Regeln können nicht gelöscht werden, jedoch mit höherer Priorität Regeln erstellen überschreiben können.

### <a name="load-balancers"></a>Lastenausgleich

Externer Lastenausgleich verteilt die Webebene Internetverkehr. Erstellen Sie eine öffentliche IP-Adresse für dieses System zum Lastenausgleich. Siehe [Erstellen einer Internetschnittstelle Lastenausgleich][lb-external-create].

Interne Lastenausgleich verteilt Netzwerkverkehr von der Webebene für die Geschäftsebene. Geben diese Lastenausgleich eine private IP-Adresse, eine Front-End-IP-Konfiguration und die Subnetzmaske für die Geschäftsebene zugeordnet. Finden Sie unter [Erste Schritte beim Erstellen einer internen Lastenausgleich][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server immer auf Verfügbarkeitsgruppen

Wir empfehlen [Immer auf Verfügbarkeitsgruppen] [ sql-alwayson] für hohe Verfügbarkeit von SQL Server. Immer erforderlich auf Verfügbarkeit einen Domänencontroller. Alle Knoten in der Availability Group muss sich in derselben Active Directory-Domäne.

Andere Ebenen Verbinden mit der Datenbank über einen [verfügbarkeitsgruppenlistener][sql-alwayson-listeners]. Der Listener kann ein SQL Client ohne den Namen der physischen Instanz von SQL Server herstellen. VMs, Zugriff auf die Datenbank mit der Domäne verknüpft werden muss. Der Client (in diesem Fall einer anderen Ebene) verwendet DNS den Listener virtuelles Netzwerknamen in IP-Adressen auflösen.

Konfigurieren Sie SQL Server immer auf ein:

- Erstellen eines Clusters Windows Server Failover Clustering (WSFC) und eine Gruppe SQL Server immer auf Verfügbarkeit. Weitere Informationen finden Sie unter [Erste Schritte mit immer auf Verfügbarkeit][sql-alwayson-getting-started].

- Erstellen einer internen Lastenausgleich statische private IP-Adresse.

- Erstellen Sie ein verfügbarkeitsgruppenlistener, und ordnen Sie den Listener DNS-Namen der IP-Adresse des internen Lastenausgleich. 

- Erstellen Sie eine Regel Load Balancer empfangsbereiten SQL Server-Port (standardmäßig TCP-Port 1433). Load Balancer Regel müssen _schwebenden IP_, auch als direkte Server zurück. Dadurch wird die VM Antworten direkt an den Client eine direkte Verbindung mit dem primären Replikat ermöglicht.

    > [AZURE.NOTE] Umlaufende IP-aktiviert ist, muss die Front-End-Port-Nummer die Backend-Portnummer in der Regel Load Balancer identisch sein.

Wenn ein SQL-Client versucht, verbinden, leitet Lastenausgleich die Verbindungsanfrage auf das Replikat ist die aktuelle. Ist ein Failover zu einem anderen Replikat, leitet der Lastenausgleich nachfolgende Anforderungen an neue primäres Replikat weiter. Weitere Informationen finden Sie unter [Konfigurieren des Systems zum Lastenausgleich für SQL stets][sql-alwayson-ilb].

Bei einem Failover werden vorhandene Clientverbindungen geschlossen. Nach Abschluss des Failovers werden neue Verbindungen mit dem neuen primären Replikat weitergeleitet.

Stellt Ihre Anwendung wesentlich liest als schreibt, können einige schreibgeschützte Abfragen auf einem sekundären Replikat verschieben. Siehe [Listener eine schreibgeschützte sekundäre Verbindung mit Replikat (schreibgeschützte Routing)][sql-alwayson-read-only-routing].

Die Bereitstellung testen, indem Sie [ein manuelles Failover][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Lassen Sie nicht RDP-Zugriff vom öffentlichen Internet auf VMs, die Arbeitslast der Anwendung ausgeführt. Stattdessen muss alle RDP-SSH-Zugriff auf die VMs durch die Jumpbox kommen. Ein Administrator anmeldet, die Jumpbox und meldet sich die VM aus den Jumpbox. Die Jumpbox kann RDP-Datenverkehr aus dem Internet jedoch nur von bekannten, zugelassenen IP-Adressen.

Platzieren Sie die Jumpbox in demselben VNet wie die VMs, jedoch in einem Subnet eine getrennte Verwaltung.

Erstellen Sie eine [öffentliche IP-Adresse] für die Jumpbox.

Verwenden Sie eine VM-klein für Jumpbox wie Standard A1.

Konfigurieren Sie die NSGs für die Webebene Geschäftsebene und Datenbank Tier Subnetze um administrative (RDP) Datenverkehr aus dem Subnetz Management passieren.

Um die Jumpbox zu sichern, erstellen Sie eine NSG und Jumpbox Subnetz zuweisen. Hinzufügen einer NSG Regel RDP-Verbindungen nur aus einer weißen Liste öffentlicher IP-Adressen.

Die NSG kann das Subnetz oder Jumpbox NIC angefügt: In diesem Fall wird empfohlen, die NIC anfügen, damit nur für die Jumpbox RDP-Datenverkehr zulässig ist, auch wenn andere virtuelle Computer im gleichen Subnetz hinzufügen.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Versetzen Sie jedes Tier oder Rolle zu einer separaten Verfügbarkeit Setzen VMs aus unterschiedlichen Ebenen in dieselben Verfügbarkeit. 

Auf der Datenbankebene übersetzt mit mehreren VMs nicht automatisch in einer hochverfügbaren. Bei einer relationalen Datenbank müssen Sie i. d. r. von Replikation und Failover für hohe Verfügbarkeit zu erreichen. Für SQL Server empfehlen wir [Immer auf Verfügbarkeitsgruppen][sql-alwayson]. 

Höheren Verfügbarkeit als der [Azure-SLA für VMs] benötigen[ vm-sla] bereitstellt, replizieren Sie die Anwendung in beiden Regionen und Azure Traffic Manager für Failover. Weitere Informationen finden Sie unter [Ausführen von Windows-VMs in mehreren Regionen für hohe Verfügbarkeit][multi-dc].   

## <a name="security-considerations"></a>Sicherheitsaspekte

Verschlüsseln von Daten. Verwenden von [Azure Key Vault] [ azure-key-vault] die Datenbank-Verschlüsselungsschlüssel verwalten. Key Vault können Hardwaresicherheitsmodule (HSMs) Schlüssel speichern. Weitere Informationen finden Sie unter [Konfigurieren Azure Key Vault Integration für SQL Server auf Azure VMs] [ sql-keyvault] wurde empfohlen, Anwendung geheime Daten wie Verbindungszeichenfolgen im Schlüssel Depot gespeichert.

Erwägen Sie eine virtuelle Appliance (NVA) eine DMZ zwischen dem öffentlichen Internet und Azure virtuelles Netzwerk erstellen. NVA ist ein allgemeiner Begriff für virtuelle Appliance, die Netzwerk-Aufgaben wie Firewall, PaketprŸfung, Überwachung, benutzerdefinierte routing oder eine Vielzahl anderer Operationen ausführen können. Weitere Informationen finden Sie unter [Implementieren einer DMZ zwischen Azure und dem Internet][dmz].

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Der Lastenausgleich verteilt auf Web und Geschäftsebenen. Skalieren Sie horizontal neue VM-Instanzen hinzufügen. Beachten Sie, dass Sie Web- und Ebenen unabhängig voneinander skalieren können basierend auf. Um mögliche Komplikationen muss Clientzugehörigkeit reduzieren, sollte die VMs in der Webebene zustandslos sein. Die Geschäftslogik hosten VMs sollte statusfreie.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Vereinfachen Sie das Management des gesamten Systems mit zentralisierten Verwaltungstools wie [Azure Automation][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [Koch][chef], oder [Marionette][puppet]. Diese Tools können Diagnose- und Gesundheit Informationen aus mehreren VMs einen allgemeinen Überblick über das System zu konsolidieren.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Bereitstellung für eine Referenzarchitektur, die diese Empfehlung zur [Github][github-folder]. Diese Referenzarchitektur enthält eine Webebene Geschäftsebene, eine Datenebene, sowie eine Jumpbox VM und Active Directory-Domänencontroller.

Die Referenzarchitektur kann anhand der Anweisungen bereitgestellt werden: 

1. Klicken Sie auf die Schaltfläche unten.  
[! ["Deploy in Azure"] [1]][2]

2. Nach die Verknüpfung in Azure-Portal geöffnet, geben Sie die folgenden Werte: 
  - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-ntier-sql-network-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Überprüfen Sie Azure Portal Benachrichtigung für eine Nachricht die Bereitstellung abgeschlossen ist.

4. Klicken Sie auf die Schaltfläche unten.  
[! ["Deploy in Azure"] [1]][3]

5. Nach die Verknüpfung in Azure-Portal geöffnet, geben Sie die folgenden Werte: 
  - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Vorhandene verwenden** und geben `ra-ntier-sql-workload-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

6. Überprüfen Sie Azure Portal Benachrichtigung für eine Nachricht die Bereitstellung abgeschlossen ist.

7. Klicken Sie auf die Schaltfläche unten.  
[! ["Deploy in Azure"] [1]][4]

8. Nach die Verknüpfung in Azure-Portal geöffnet, geben Sie die folgenden Werte: 
  - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-ntier-sql-network-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

9. Überprüfen Sie Azure Portal Benachrichtigung für eine Nachricht die Bereitstellung abgeschlossen ist.

10. Parameterdateien enthalten eine hartcodierte Administrator-Benutzernamen und Kennwörter und wird empfohlen, dass Sie unmittelbar auf die VMs ändern. Jeder virtuelle Computer in Azure-Portal klicken auf **Passwort** **Support + Problembehandlung** Blatt. Wählen Sie **Kennwort zurücksetzen** im Feld Dropdown- **Modus** und anschließend einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort beibehalten. 

Informationen über weitere Methoden zum Bereitstellen dieser Referenzarchitektur, lesen Sie die Infodatei in der [Anleitung einzelnen Vm] [ github-folder] Github Ordner. 

## <a name="next-steps"></a>Nächste Schritte

Zu hohe Verfügbarkeit für diese Referenzarchitektur wir empfehlen die [Bereitstellung in mehreren Regionen][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[Bastion-Hosts]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[öffentliche IP-Adresse]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "N-Tier-Architektur mit Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json