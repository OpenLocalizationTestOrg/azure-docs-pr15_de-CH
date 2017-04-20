<properties
   pageTitle="Unter Linux VMs für N-Tier-Architektur auf Azure | Microsoft Azure"
   description="Linux VMs für eine N-Ebenen-Architektur in Microsoft Azure ausführen"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
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

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>N-Tier-Architektur auf Azure Linux VMs ausführen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [N-Tier-Architektur auf Azure Linux VMs ausführen](guidance-compute-n-tier-vm-linux.md)
- [Auf Windows Azure ausgeführte Windows-VMs für N-Tier-Architektur](guidance-compute-n-tier-vm.md)

Dieser Artikel beschreibt eine Reihe von bewährten Methoden zum Ausführen von Linux virtuellen Maschinen (VMs) für eine Anwendung mit N-Tier-Architektur. Baut auf Artikel [mehrere VMs auf Azure ausführen][multi-vm].

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle: [Ressourcen-Manager] [ resource-manager-overview] und classic. In diesem Artikel verwendet Ressourcen-Manager für die Bereitstellung neuer Microsoft empfiehlt.

## <a name="architecture-diagram"></a>Architekturdiagramm

Es gibt Variationen des N-Ebenen-Architekturen. Zum größten Teil sollte nicht die Unterschiede im Sinne dieser Empfehlung wichtig. Regel 3-Tier-Web app vorausgesetzt:

- **Die Webebene.** HTTP-Anfragen behandelt. Antworten werden durch diese Ebene zurückgegeben.

- **Geschäftsebene.** Implementierung von Geschäftsprozessen und andere funktionale Logik für das System.

- **Die Datenbankebene.** Bietet permanente Datenspeicher mit Apache Cassandra für hohe Verfügbarkeit.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält steht auf der [Microsoft download Center]zum Download[visio-download]. Dieses Diagramm ist "Compute - Multi-Tier (Linux) Seite.

![[0]][0]

- **Verfügbarkeit legt.** Erstellen Sie eine [Verfügbarkeit] [ azure-availability-sets] für jedes tier und mindestens zwei VMs pro Ebene bereitstellen. Dies ist erforderlich, damit Verfügbarkeits- [SLA] [ vm-sla] für virtuelle Computer.

- **Subnetze.** Erstellen Sie ein separates Subnetz für jede Ebene. Geben Sie die Adressbereich und Subnetzmaske Adressmaske [CIDR] -Notation verwenden. 

- **Lastenausgleich.** Ein [System zum Lastenausgleich Internetzugriff] verwenden[ load-balancer-external] Internetverkehr die Webebene und eine [interne Lastenausgleich] verteilt[ load-balancer-internal] Netzwerkverkehr von der Webebene auf Geschäftsebene verteilen.

- **Jumpbox**. Eine _Jumpbox_, auch als [Bastion-Host]ist ein virtueller Computer auf dem Netzwerk, mit dem Administratoren die VMs herstellen. Die Jumpbox hat eine NSG, die remote Verkehr nur von zugelassenen öffentliche IP-Adressen zulässt. Die NSG sollten secure Shell (SSH) Datenverkehr zuzulassen.

- **Überwachen**. Überwachungssoftware wie [Nagios], [Zabbix]oder [Icinga] erhalten Sie Einblick in die Reaktionszeit, VM Betriebszeit und den Gesamtzustand des Systems. Installieren Sie die Überwachungssoftware auf einem virtuellen Computer in einem separaten Management-Subnetz befindet.

- **NSGs**. Verwenden Sie [netzwerksicherheitsgruppen] [ nsg] (NSGs) Beschränkung des Netzwerkverkehrs in der VNet. Beispielsweise in der hier gezeigten 3-Tier-Architektur akzeptiert die Datenbankebene Datenverkehr vom Web-front-End von Geschäftsebene und Management-Subnetz keine.

- **Apache Cassandra Datenbank**. Bietet hohe Verfügbarkeit auf der Datenebene durch Replikation und Failover.

## <a name="recommendations"></a>Empfehlung

Azure bietet viele verschiedene Ressourcen und Ressourcentypen, sodass diese Referenzarchitektur kann viele verschiedene Arten bereitgestellt. Wir haben eine Vorlage Azure-Ressourcen-Manager um die Referenzarchitektur installieren, die diese Empfehlung folgt bereitgestellt. Wenn Sie erstellen eigene Referenzarchitektur folgen diese Empfehlung nur eine Anforderung, die Empfehlung nicht passen

### <a name="vnet--subnets"></a>VNet / Subnetze

Beim Erstellen des Vnets reservieren Sie genügend Adressraum für die Subnetze, die Sie benötigen. Geben Sie die VNet Adressbereich und Subnetzmaske Adressmaske [CIDR] -Notation verwenden. Verwenden Sie einen Adressraum der standardmäßigen [private IP-Adresse blockiert innerhalb][private-ip-space], 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 sind.

Wählen Sie einen Adressbereich, der nicht mit dem lokalen Netzwerk überschneidet bei Gateway zwischen der VNet und das lokale Netzwerk später einrichten möchten. Nach Erstellung das VNet können Sie den Adressbereich nicht ändern.

Entwerfen Sie Subnetze Funktionalität und Sicherheit Anforderungen. Alle VMs innerhalb derselben Ebene oder Rolle gehen in demselben Subnetz eine Sicherheitsgrenze erfolgen kann. Weitere Informationen zum Entwerfen von VNets und Subnetzen finden Sie unter [Planen und Entwerfen Azure Virtual Networks][plan-network].

Geben Sie für jedes Subnetz den Adressbereich für das Subnetz in CIDR-Notation. '10.0.0.0/24' erstellt z. B. einen Bereich von 256 IP-Adressen. (VMs können diese 251, fünf reserviert. [Virtuelles Netzwerk FAQ]finden Sie unter[vnet faq].) Sicherstellen Sie, dass die Adressbereiche über Subnetze hinweg nicht überlappen.

### <a name="load-balancers"></a>Lastenausgleich

Externer Lastenausgleich verteilt die Webebene Internetverkehr. Erstellen Sie eine öffentliche IP-Adresse für dieses System zum Lastenausgleich. Siehe [Erstellen einer Internetschnittstelle Lastenausgleich][lb-external-create].

Interne Lastenausgleich verteilt Netzwerkverkehr von der Webebene für die Geschäftsebene. Geben diese Lastenausgleich eine private IP-Adresse, eine Front-End-IP-Konfiguration und die Subnetzmaske für die Geschäftsebene zugeordnet. Finden Sie unter [Erste Schritte beim Erstellen einer internen Lastenausgleich][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Wir empfehlen [DataStax Enterprise] [ datastax] Produktion verwenden, aber diese Vorschläge gelten für alle Cassandra Edition. Weitere Informationen zum Ausführen von DataStax in Azure finden Sie [DataStax Enterprise Deployment Guide für Azure][cassandra-in-azure]. 

Die VMs Cassandra Cluster in einem Satz Verfügbarkeit Cassandra Replikate über mehrere Fehlerdomänen verteilt und Aktualisieren von Domänen versetzen Weitere Informationen über Fehler und Upgrade-Domänen finden Sie unter [Verwalten der Verfügbarkeit virtueller Maschinen][availability-sets-manage]. 

3 Fehlerdomänen (maximal) pro Verfügbarkeit zu konfigurieren. 

Konfigurieren von 18 Upgrade Domänen pro Verfügbarkeit. Dadurch werden maximal Upgrade Domänen als domänenübergreifende Fehler noch gleichmäßig verteilt werden kann.   

Konfigurieren Sie Knoten im Rack-fähige Modus. Ordnen Sie Fehlerdomänen Regale in der `cassandra-rackdc.properties` Datei.

Ein Lastenausgleich vor dem Cluster erforderlich nicht. Der Client verbindet direkt auf einen Knoten im Cluster.

### <a name="jumpbox"></a>Jumpbox

Platzieren Sie die Jumpbox in demselben VNet wie die VMs, jedoch in einem Subnet eine getrennte Verwaltung.

Erstellen Sie eine [öffentliche IP-Adresse] für die Jumpbox.

Verwenden Sie eine VM-klein für Jumpbox wie Standard A1.

Konfigurieren Sie die NSGs für die Webebene Geschäftsebene und Datenbank Tier Subnetze um administrative (SSH) Datenverkehr aus dem Subnetz Management passieren.

Um die Jumpbox zu sichern, erstellen Sie eine NSG und Jumpbox Subnetz zuweisen. Hinzufügen einer NSG Regel SSH-Verbindungen nur aus einer weißen Liste öffentlicher IP-Adressen.

Die NSG kann das Subnetz oder Jumpbox NIC angefügt: In diesem Fall wird empfohlen, die NIC anfügen, damit nur auf die Jumpbox SSH Datenverkehr zulässig ist, auch wenn andere virtuelle Computer im gleichen Subnetz hinzufügen.

Ermöglichen Sie SSH-Zugriff nicht vom öffentlichen Internet auf VMs, die Arbeitslast der Anwendung ausgeführt. Stattdessen muss alle SSH-Zugriff auf die VMs durch die Jumpbox kommen. Ein Administrator anmeldet, die Jumpbox und meldet sich die VM aus den Jumpbox. Die Jumpbox ermöglicht SSH-Verkehr aus dem Internet jedoch nur von bekannten, zugelassenen IP-Adressen.

## <a name="availability-considerations"></a>Aspekte der Verfügbarkeit

Versetzen Sie jedes Tier oder Rolle zu einer separaten Verfügbarkeit Setzen VMs aus unterschiedlichen Ebenen in dieselben Verfügbarkeit. 

Auf der Datenbankebene übersetzt mit mehreren VMs nicht automatisch in einer hochverfügbaren. Bei einer relationalen Datenbank müssen Sie i. d. r. von Replikation und Failover für hohe Verfügbarkeit zu erreichen.  

Höheren Verfügbarkeit als der [Azure-SLA für VMs] benötigen[ vm-sla] bereitstellt, replizieren Sie die Anwendung in beiden Regionen und Azure Traffic Manager für Failover. Weitere Informationen finden Sie unter [Ausführen von Linux VMs in mehreren Regionen für hohe Verfügbarkeit][multi-dc].  

## <a name="security-considerations"></a>Sicherheitsaspekte

Verwenden Sie NSG Regeln Verkehr zwischen eingeschränkt. Z. B. in der obigen 3-Tier-Architektur kommuniziert die Webebene nicht direkt mit der Datenbank. Um dies zu erzwingen, sollte die Datenbankebene eingehenden Datenverkehr aus dem Subnetz Web-Tier blockieren.  

  1. Erstellen einer NSG und Subnetz Tier Datenbank zuordnen.

  2. Hinzufügen einer Regel, die den gesamten eingehenden Datenverkehr aus der VNet verweigert. (Mit dem `VIRTUAL_NETWORK` Tag in der Regel.) 

  3. Hinzufügen einer Regel mit höherer Priorität, der eingehenden Datenverkehr aus dem Subnetz Business Ebene ermöglicht. Diese Regel überschreibt die vorherige Regel und ermöglicht die Geschäftsebene mit der Datenbankebene.

  4. Hinzufügen einer Regel, die Datenverkehr aus der Datenbank Tier im Subnetz ermöglicht. Diese Regel ermöglicht die Kommunikation zwischen virtuellen Computern auf der Datenbankebene für Replikation und Failover.

  5. Fügen Sie eine Regel SSH-Datenverkehr aus dem Subnetz Jumpbox. Diese Regel ermöglicht Administratoren die Datenbankebene aus der Jumpbox herstellen.

  > [AZURE.NOTE] Eine NSG [Standardregeln] hat[ nsg-rules] eingehenden Datenverkehr von innerhalb der VNet zulassen. Diese Regeln können nicht gelöscht werden, jedoch mit höherer Priorität Regeln erstellen überschreiben können.

Erwägen Sie eine virtuelle Appliance (NVA) eine DMZ zwischen dem öffentlichen Internet und Azure virtuelles Netzwerk erstellen. NVA ist ein allgemeiner Begriff für virtuelle Appliance, die Netzwerk-Aufgaben wie Firewall, PaketprŸfung, Überwachung, benutzerdefinierte routing oder eine Vielzahl anderer Operationen ausführen können. Weitere Informationen finden Sie unter [Implementieren einer DMZ zwischen Azure und dem Internet][dmz].

## <a name="scalability-considerations"></a>Aspekte der Skalierung

Der Lastenausgleich verteilt auf Web und Geschäftsebenen. Skalieren Sie horizontal neue VM-Instanzen hinzufügen. Beachten Sie, dass Sie Web- und Ebenen unabhängig voneinander skalieren können basierend auf. Um mögliche Komplikationen muss Clientzugehörigkeit reduzieren, sollte die VMs in der Webebene zustandslos sein. Die Geschäftslogik hosten VMs sollte statusfreie.

## <a name="manageability-considerations"></a>Aspekte der Verwaltung

Vereinfachen Sie das Management des gesamten Systems mit zentralisierten Verwaltungstools wie [Azure Automation][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [Koch][chef], oder [Marionette][puppet]. Diese Tools können Diagnose- und Gesundheit Informationen aus mehreren VMs einen allgemeinen Überblick über das System zu konsolidieren.

## <a name="solution-deployment"></a>Bereitstellung der Lösung

Bereitstellung für eine Referenzarchitektur, die diese Empfehlung zur [Github][github-folder]. Diese Referenzarchitektur enthält eine Webebene Geschäftsebene und eine Datenebene.

1. Klicken Sie auf die Schaltfläche unten.  
[! ["Deploy in Azure"] [1]][2]

2. Nach die Verknüpfung in Azure-Portal geöffnet, geben Sie die folgenden Werte: 
  - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert, wählen Sie **Neu erstellen** und geben Sie `ra-ntier-sql-network-rg` im Textfeld.
  - Wählen Sie die Region aus der Dropdownliste **Speicherort** .
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder Textfelder **Parameter Stamm-Uri** .
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen und **akzeptiere die allgemeinen Geschäftsbedingungen genannten** Kontrollkästchen.
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Überprüfen Sie Azure Portal Benachrichtigung für eine Nachricht die Bereitstellung abgeschlossen ist.

4. Parameterdateien enthalten eine hartcodierte Administrator-Benutzernamen und Kennwörter und wird empfohlen, dass Sie unmittelbar auf die VMs ändern. Jeder virtuelle Computer in Azure-Portal klicken auf **Passwort** **Support + Problembehandlung** Blatt. Wählen Sie **Kennwort zurücksetzen** im Feld Dropdown- **Modus** und anschließend einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort beibehalten.

## <a name="next-steps"></a>Nächste Schritte

Zu hohe Verfügbarkeit für diese Referenzarchitektur wir empfehlen die [Bereitstellung in mehreren Regionen][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[Bastion-Hosts]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[öffentliche IP-Adresse]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "N-Tier-Architektur mit Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


