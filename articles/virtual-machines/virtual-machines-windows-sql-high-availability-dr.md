<properties
    pageTitle="Hohe Verfügbarkeit und Disaster Recovery für SQL Server | Microsoft Azure"
    description="Eine Erläuterung der verschiedenen HADR Strategien für SQL Server in Azure virtuellen Computer ausgeführt."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines

## <a name="overview"></a>Übersicht

Microsoft Azure virtuelle Maschinen (VMs) mit SQL Server können die Kosten für eine hohe Verfügbarkeit und Disaster Recovery (HADR) Datenbank-Lösung senken. Die meisten SQL Server HADR Solutions werden in Azure virtuellen Computer sowohl als nur Azure Hybrid-Lösung unterstützt. In einer Projektmappe nur Azure ausgeführt die gesamte HADR in Azure. In einer hybridkonfiguration führt der Projektmappe in Azure und der andere Teil ausgeführt lokal in Ihrer Organisation. Die Flexibilität der Azure-Umgebung können Sie teilweise oder vollständig in Azure die Budget und HADR Ihrer SQL Server-Datenbank-Systeme zu verschieben.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Grundlegendes zur Notwendigkeit einer Lösung HADR

Es liegt an Ihnen, sicherzustellen, dass Ihr Datenbanksystem HADR Funktionen verfügt, die der Vereinbarung zum Servicelevel (SLA) erfordert. Die Tatsache, dass Azure hohe Verfügbarkeit Mechanismen wie Reparatur für Cloud-Services und Erkennung von Fehlern Recovery für die virtuellen Computer nicht garantiert die gewünschte SLA erfüllen können. Diese Mechanismen schützen die hohe Verfügbarkeit der VMs, aber nicht die hohe Verfügbarkeit von SQL Server in den VMs. Es ist möglich für die SQL Server-Instanz fehlschlägt, während die VM online und nicht beschädigt ist. Auch die hohe Verfügbarkeit Mechanismen von Azure Ausfallzeiten der VMs durch Ereignisse wie Recovery Software oder Hardware-Ausfälle und Betriebssystem-Upgrades ermöglichen.

Darüber hinaus möglicherweise Geo redundanten Speicher (GRS) in Azure mit sogenannten Geo-Replikation implementiert, eine angemessene Disaster Recovery-Lösung für die Datenbanken nicht. Da Geo-Replikation Daten asynchron gesendet werden, können neue Updates Notfall verloren. Weitere Informationen zur Replikation geometrische Grenzen fallen im Abschnitt [Geo-Replikation für Daten und Protokolldateien auf separaten Datenträgern nicht unterstützt](#geo-replication-support) .

## <a name="hadr-deployment-architectures"></a>HADR Bereitstellungsarchitekturen

SQL Server HADR Technologies, die in Azure unterstützt gehören:

- [Immer auf Verfügbarkeitsgruppen](https://technet.microsoft.com/library/hh510230.aspx)
- [Spiegeln von Datenbanken](https://technet.microsoft.com/library/ms189852.aspx)
- [Protokollversand](https://technet.microsoft.com/library/ms187103.aspx)
- [Sicherung und Wiederherstellung mit Azure BLOB-Speicher](https://msdn.microsoft.com/library/jj919148.aspx)
- [Immer im Failovercluster Instanzen](https://technet.microsoft.com/library/ms189134.aspx)

Es ist möglich, die Technologien zu einer SQL Server-Lösung, die hohe Verfügbarkeit und Disaster Recovery-Funktionen. Abhängig von der verwendeten Technologie erfordern eine hybridbereitstellung einen VPN-Tunnel mit Azure virtual Network. Die folgenden Abschnitte zeigen Ihnen einige Beispiel Bereitstellungsarchitekturen.

## <a name="azure-only-high-availability-solutions"></a>Nur Azure: Lösungen für hohe Verfügbarkeit

Sie können eine für die SQL Server-Datenbanken in Azure immer auf Verfügbarkeit Gruppen oder Datenbank-Spiegelung.

| Technologie                               | Beispiel-Architekturen                    |
| ---------------------------------------- | ---------------------------------------- |
| **Immer auf Verfügbarkeitsgruppen**        | Alle verfügbarkeitsreplikate in Azure VMs für hohe Verfügbarkeit innerhalb desselben Bereichs ausgeführt. Sie müssen einen virtuellen Domänencontrollercomputer, da Windows Server Failover Clustering (WSFC) Active Directory-Domäne erforderlich sind.<br/> ![Immer auf Verfügbarkeitsgruppen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Weitere Informationen finden Sie unter [Konfigurieren immer auf Verfügbarkeitsgruppen in Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Immer im Failovercluster Instanzen** | Failover Cluster Instanzen (FCI) erfordern freigegebenen Speicher kann auf 2 Arten erstellt werden.<br/><br/>1. FCI auf einen Speicher von einer Drittanbieter-Clusterlösung unterstützt in Azure VMs mit zwei Knoten WSFC. Ein bestimmtes Beispiel SIOS DataKeeper finden Sie unter [hohe Verfügbarkeit für eine Dateifreigabe mit WSFC und 3rd Party Software SIOS Datakeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. FCI auf einem entfernten iSCSI-Ziel in Azure VMs mit zwei Knoten WSFC freigegeben Blockspeicher über ExpressRoute. NetApp Private Speicher (NPS) stellt beispielsweise ein iSCSI-Ziel über ExpressRoute equinix auf Azure VMs.<br/><br/>Für gemeinsam genutzten Speicher von Drittanbietern und Daten-Replikation wenden Sie den Kreditor für alle Probleme in Bezug auf Daten auf Failover.<br/><br/>Beachten Sie, dass mit FCI auf [Azure Dateispeicher](https://azure.microsoft.com/services/storage/files/) nicht noch unterstützt wird, da diese Lösung nicht Premium-Speicher nutzen. Wir arbeiten schnell unterstützt. |

## <a name="azure-only-disaster-recovery-solutions"></a>Nur Azure: Disaster Recovery-Lösung

Sie haben eine Disaster Recovery-Lösung für SQL Server-Datenbanken in Azure immer auf Verfügbarkeit Gruppen, Spiegeln von Datenbanken oder Backup und Wiederherstellen mit Speicher-Blobs.

| Technologie                               | Beispiel-Architekturen                    |
| ---------------------------------------- | ---------------------------------------- |
| **Immer auf Verfügbarkeitsgruppen**        | Verfügbarkeitsreplikate über mehrere Rechenzentren in Azure VMs für Disaster Recovery. Diese Cross-Region Lösung schützt vollständige Standort. <br/> ![Immer auf Verfügbarkeitsgruppen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Innerhalb eines Bereichs sollte alle Replikate in derselben Cloud-Dienst und die gleichen VNet. Da jede Region einen eigenen VNet haben, müssen diese Projektmappen VNet VNet Konnektivität. Weitere Informationen finden Sie unter [Konfigurieren einer Standort-zu-Standort-VPN im klassischen Azure-Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Spiegeln von Datenbanken**                   | Prinzipal und Spiegel und Servern in verschiedenen Rechenzentren für Disaster Recovery. Serverzertifikate verwenden, da active Directory-Domäne kann nicht mehrere Rechenzentren umfassen müssen bereitgestellt werden.<br/>![Spiegeln von Datenbanken](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Sicherung und Wiederherstellung mit Azure BLOB-Speicher** | Produktionsdatenbanken direkt auf BLOB-Speicher in einem anderen Rechenzentrum für die Wiederherstellung gesichert.<br/>![Sicherung und Wiederherstellung](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Weitere Informationen finden Sie unter [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybride IT: Disaster Recovery-Lösung

Sie haben eine Disaster Recovery-Lösung für die SQL Server-Datenbanken in einer Hybrid-IT-Umgebung immer auf Verfügbarkeitsgruppen, später Protokollversand und Backup und Wiederherstellen mit Azure-Blog.

| Technologie                               | Beispiel-Architekturen                    |
| ---------------------------------------- | ---------------------------------------- |
| **Immer auf Verfügbarkeitsgruppen**        | Einige verfügbarkeitsreplikate in Azure VMs und Replikaten lokal für standortübergreifende Disaster Recovery ausführen. Standort kann entweder lokal oder in Azure-Rechenzentrum.<br/>![Immer auf Verfügbarkeitsgruppen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Da alle verfügbarkeitsreplikate im selben WSFC Cluster sein müssen, muss der WSFC-Cluster beide Netzwerke (mehrere Subnetze WSFC Cluster) umfassen. Diese Konfiguration erfordert eine VPN-Verbindung zwischen Azure und dem lokalen Netzwerk.<br/><br/>Für die erfolgreiche Wiederherstellung der Datenbanken sollten Sie auch einen replizierten Domänencontroller am Disaster Recovery-Standort installieren.<br/><br/>Es ist möglich, den Assistenten zum Hinzufügen eines Replikats in SSMS Azure Replikat einer vorhandenen immer auf Verfügbarkeit Gruppe hinzufügen. Weitere Informationen finden Sie unter Tutorial: Erweitern der immer auf Availability Group in Azure. |
| **Spiegeln von Datenbanken**                   | Ein Partner in Azure-VM und anderen laufenden lokalen websiteübergreifenden Disaster Recovery mit Serverzertifikaten. Partner müssen nicht in derselben Active Directory-Domäne sein, und keine VPN-Verbindung erforderlich ist.<br/>![Spiegeln von Datenbanken](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Eine andere Datenbank Spiegelung Szenario umfasst ein Partner in Azure-VM und anderen laufenden lokalen in derselben Active Directory-Domäne für standortübergreifende Disaster Recovery. Eine [VPN-Verbindung zwischen Azure virtuelle Netzwerk und dem lokalen](../vpn-gateway/vpn-gateway-site-to-site-create.md) ist erforderlich.<br/><br/>Für die erfolgreiche Wiederherstellung der Datenbanken sollten Sie auch einen replizierten Domänencontroller am Disaster Recovery-Standort installieren. |
| **Protokollversand**                         | Ein Server in einer Azure VM und anderen laufenden lokalen websiteübergreifenden Disaster Recovery. Protokollversand hängt Windows-Dateifreigabe, damit eine VPN-Verbindung zwischen Azure virtual Network und dem lokalen Netzwerk erforderlich ist.<br/>![Protokollversand](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Für die erfolgreiche Wiederherstellung der Datenbanken sollten Sie auch einen replizierten Domänencontroller am Disaster Recovery-Standort installieren. |
| **Sicherung und Wiederherstellung mit Azure BLOB-Speicher** | Lokal-Produktionsdatenbanken direkt Azure BLOB-Speicher gesichert für Disaster Recovery.<br/>![Sicherung und Wiederherstellung](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Weitere Informationen finden Sie unter [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Wichtig für SQL Server HADR in Azure

Azure VMs, Speicher und Netzwerke haben unterschiedliche Funktionseigenschaften als ein lokales, nicht virtualisierten IT-Infrastruktur. Eine erfolgreiche Implementierung einer HADR SQL Server-Lösung in Azure muss diese Unterschiede und Entwerfen Ihrer Lösung für ihre Unterbringung.

### <a name="high-availability-nodes-in-an-availability-set"></a>Knoten in einem Verfügbarkeit hohe Verfügbarkeit

Verfügbarkeit legt in Azure können Sie hohe Verfügbarkeit Knoten in separaten Fehler (FDs) und Update-Domänen (UDs) platzieren. Für Azure VMs Verfügbarkeit dieselben platziert werden müssen Sie sie im selben Cloud-Dienst bereitstellen. Nur Knoten im selben Cloud-Dienst können dieselben Verfügbarkeit teilnehmen. Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>WSFC Cluster-Verhaltens in Azure-Netzwerken

Nicht RFC-kompatiblen DHCP-Dienst in Azure kann die Erstellung von bestimmten Clusterkonfigurationen WSFC Fehler auf der Cluster-Netzwerkname eine doppelte IP-Adresse wie die IP-Adresse als eine Cluster-Knoten zugewiesen wird. Dies ist ein Problem beim immer auf Verfügbarkeitsgruppen Implementieren der WSFC Funktion abhängt.

Das Szenario bei ein Cluster mit zwei Knoten erstellt und online geschaltet:

1. Cluster online geschaltet und NODE1 fordert eine dynamisch zugewiesene IP-Adresse für Cluster-Netzwerkname.

2. Keine IP-Adresse als NODE1 eigene IP-Adresse des DHCP-Dienstes gegeben, da der DHCP-Dienst erkennt, dass die Anforderung von NODE1 selbst.

3. Windows erkennt, dass eine doppelte NODE1 und Cluster-Netzwerkname Adresse und die Standard-Clustergruppe nicht online geschaltet.

4. Die Standard-Clustergruppe verschiebt NODE2 die behandelt NODE1s IP-Adresse als IP-Adresse und die Standard-Clustergruppe online.

5. NODE2 mit NODE1 Verbindung herzustellen versucht, verlasse Pakete NODE1 gegen NODE2 da NODE1s IP-Adresse sich auflöst. Node2 Konnektivität mit NODE1 kann nicht hergestellt werden, sein Quorum verliert und Cluster heruntergefahren.

6. In der Zwischenzeit NODE1 senden Pakete zu NODE2 jedoch NODE2 antwortet nicht. Node1 sein Quorum verliert und Cluster heruntergefahren.

Dieses Szenario kann vermieden werden, indem Clusternetzwerknamen Clusternetzwerknamen online bringen eine nicht verwendete statische IP-Adresse, wie eine Link-lokale Adresse wie 169.254.1.1, zuweisen. Zur Vereinfachung dieses Vorgangs finden Sie unter [Konfigurieren von Windows-Failovercluster in Azure für immer auf Verfügbarkeit](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Weitere Informationen finden Sie unter [Konfigurieren immer auf Verfügbarkeitsgruppen in Azure (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Verfügbarkeit Listener Unterstützung

Verfügbarkeit Gruppe Listener werden unter Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2, Windows Server 2016 Azure VMs unterstützt. Diese Unterstützung ermöglicht mit Lastenausgleich Endpunkte auf Azure-VMs, die Gruppenknoten Verfügbarkeit aktiviert. Befolgen Sie spezielle Konfigurationsschritte für die Listener für beide Clientanwendungen arbeiten, die in Azure als auch lokal ausführen ausgeführt werden.

Es gibt zwei Hauptoptionen für das Einrichten des Listeners: externe (öffentliche) oder. Externe (öffentliche) Listener ein Lastenausgleich mit Internetzugriff verwendet und ist mit einer öffentlichen VIP (Virtual IP), die über das Internet zugänglich ist. Ein interner Listener verwendet eine interne Lastenausgleich und unterstützt nur Clients im gleichen virtuellen Netzwerk. Entweder laden Balancer, direkte Server zurückgeben müssen. 

Der Availability Group umfasst mehrere Azure Subnetze (z. B. eine Bereitstellung, die Azure-Regionen überschreitet), sind die Clientverbindungszeichenfolge "**MultisubnetFailover = True**". Dies führt parallele Verbindungsversuche Replikate in verschiedenen Subnetzen. Informationen zum Einrichten der Listener finden Sie unter

- [Ein ILB-Listener für immer auf Verfügbarkeit in Azure konfigurieren](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Externe Listener für immer auf Verfügbarkeit in Azure konfigurieren](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Sie können weiterhin Replikats Verfügbarkeit VES direkt an die Dienstinstanz. Da immer auf Verfügbarkeitsgruppen abwärts kompatibel mit Clients zum Spiegeln von Datenbanken sind, können Sie auch Verfügbarkeit Replikate Datenbank Spiegelung Partner als Replikate Datenbankspiegeln ähnlich konfiguriert sind verbinden:

- Ein primäres Replikat und eine sekundäre Kopie

- Das sekundäre Replikat ist als nicht lesbar (**Lesbare sekundäre** Option auf **No**gesetzt) konfiguriert.

Unten ist eine Beispiel eines Client-Verbindungszeichenfolge, die Konfiguration dieses Spiegelung wie ADO.NET oder SQL Server Native Client entspricht:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Weitere Informationen zu Client-Verbindung finden Sie unter:

- [Verwenden von Schlüsselwörtern für Verbindungszeichenfolgen mit SQL Server Native Client](https://msdn.microsoft.com/library/ms130822.aspx)
- [Verbinden von Clients mit einer Datenbankspiegelungssitzung (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Herstellen einer Verbindung mit Availability Group Listener in Hybrid-IT](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Availability Group Listener Clientkonnektivität und Application Failover (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Mithilfe von Verbindungszeichenfolgen Database Mirroring mit Verfügbarkeit](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Netzwerklatenz hybride IT

Sie sollten Projektmappe HADR unter der Annahme möglicherweise Zeiträume mit Netzwerkwartezeiten zwischen dem lokalen Netzwerk und Azure bereitstellen. Wenn Sie Replikate in Azure bereitstellen, sollten Sie asynchronen Commit statt synchron Commit für Synchronisierungsmodus verwenden. Bereitstellen von Servern zum Spiegeln von Datenbanken sowohl vor Ort als in Azure anstelle den high Performance-Modus die hohe Sicherheitsstufe.

### <a name="geo-replication-support"></a>Geo-Unterstützung

Geo-Replikation in Azure Datenträger unterstützt nicht die Datendatei und die Protokolldatei der gleichen Datenbank auf separaten Datenträgern gespeichert werden. GRS repliziert Änderungen auf jeder Festplatte unabhängig und asynchron. Dieser Mechanismus gewährleistet die Schreibreihenfolge innerhalb einer einzelnen Festplatte Kopie Geo repliziert, nicht jedoch über geografisch repliziert Kopien von mehreren Datenträgern. Konfigurieren eine Datenbank zum Speichern der Datendatei und die Protokolldatei auf separaten Datenträgern enthalten die wiederhergestellten Laufwerke nach einem Notfall eine aktuellere Kopie der Datei als Protokolldatei Warteposition Write-ahead-Protokoll in SQL Server und die ACID-Eigenschaften von Transaktionen. Wenn Sie keinen Geo-Replikation auf das Speicherkonto deaktivieren sollten Sie alle Daten- und Protokolldateien Dateien für eine bestimmte Datenbank auf demselben Datenträger beibehalten. Wenn Sie mehrere Datenträger aufgrund der Größe der Datenbank verwenden möchten, müssen Sie eine der oben aufgeführten Daten Redundanz Disaster Recovery-Lösung bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

Azure virtuellen Computer mit SQL Server erstellen, finden Sie unter [Bereitstellen einer SQL Server-VM in Azure](virtual-machines-windows-portal-sql-server-provision.md).

Die beste Leistung von SQL Server auf eine Azure-VM finden Sie die Anleitung im [Best Practices zur Performance für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Weitere Themen zum Ausführen von SQL Server in Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Andere Ressourcen

- [Installieren einer neuen Active Directory-Gesamtstruktur in Azure](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Erstellen Sie WSFC-Cluster für immer auf Verfügbarkeit in Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
