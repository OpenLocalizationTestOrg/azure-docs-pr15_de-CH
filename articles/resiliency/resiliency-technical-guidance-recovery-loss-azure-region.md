<properties
   pageTitle="Flexibilität für die Wiederherstellung nach Datenverlust eine technische Anleitung Azure-Region | Microsoft Azure"
   description="Artikel verstehen und leistungsstarke, hochverfügbare, Fault tolerant Applications entwerfen sowie die Planung für die Wiederherstellung"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-a-region-wide-service-disruption"></a>Technische Anleitung Azure Stabilität: Wiederherstellung von regionalen serviceunterbrechung

Azure ist physisch und logisch in Einheiten bezeichneten Bereiche unterteilt. Ein Bereich besteht aus mindestens Rechenzentren in der Nähe. Zum Zeitpunkt der Erstellung dieses Dokuments hat Azure 24 weltweit.

In seltenen Fällen kann es im gesamten Raum beispielsweise aufgrund von Netzwerkfehlern zugegriffen werden können. Oder Funktionen verloren, beispielsweise aufgrund einer Naturkatastrophe. Dieser Abschnitt erläutert die Funktionen von Azure zum Erstellen von Clientanwendungen Regionen verteilt sind. Verteilung ermöglicht die Möglichkeit minimieren, dass ein Fehler in einer Region andere Bereiche auswirken.

##<a name="cloud-services"></a>Cloud-Dienste

###<a name="resource-management"></a>Ressourcenmanagement

Bereiche können in jeder Zielregion einen separate Cloud-Dienst erstellen und veröffentlichen das Bereitstellungspaket zum Cloud-Dienst Serverinstanzen verteilt werden. Beachten Sie jedoch, dass Verteilen des Datenverkehrs über Cloud-Services in unterschiedlichen Regionen der Anwendungsentwickler oder Datenverkehr Management Service implementiert werden muss.

Bestimmen der Anzahl von spare Rolleninstanzen für Disaster Recovery im Voraus bereitstellen ist ein wichtiger Aspekt der Planung. Eine umfassende sekundäre Bereitstellung wird sichergestellt, dass Kapazität bereits bei Bedarf zur Verfügung; jedoch kann dies effektiv Kosten. Ein gemeinsames Muster ist eine kleine, sekundäre Bereitstellung nur groß genug ist, um kritische Services ausführen. Diese kleine sekundäre Bereitstellung empfiehlt sich, beide Kapazitäten und für die Konfiguration der sekundären Umgebung testen.

>[AZURE.NOTE]Das abonnementkontingent ist keine kapazitätsgarantie für. Die Quote ist einfach ein Kreditlimit. Um zu gewährleisten, muss die erforderliche Anzahl von Rollen im Dienstmodell definiert werden und die Rollen müssen bereitgestellt werden.

###<a name="load-balancing"></a>Lastenausgleich

Lastenausgleich erfordert Datenverkehr Regionen eine Lösung Datenverkehr. Azure bietet [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/). Sie können auch Dienste von Drittanbietern nutzen ähnlichen Datenverkehr Verwaltungsfunktionen.

###<a name="strategies"></a>Strategien

Viele alternative Strategien sind für das Implementieren verteilter Compute Regionen verfügbar. Diese müssen bestimmte Unternehmen und Umstände der Anwendung angepasst werden. Auf einer hohen Ebene können die Ansätze in die folgenden Kategorien unterteilt:

  * __Erneut auf__: dabei die Anwendung zum Zeitpunkt des Absturzes neu bereitgestellt. Dies eignet sich für nicht kritische Anwendung, die eine garantierte Wiederherstellungszeit benötigen.

  * __Warm Spare (aktiv/passiv)__: sekundärer gehosteter Dienst in einer anderen Region erstellt und Rollen bereitgestellten minimal; gewährleisten die Rollen erhalten keine Produktionsverkehr. Dieser Ansatz eignet sich für Programme, die nicht zum Verteilen von Datenverkehr auf Regionen entwickelt wurden.

  * __Hot-Spare (aktiv/aktiv)__: die Anwendung Arbeitslast in mehreren Regionen erhalten soll. Die Clouddienste in jeder Region können für höhere Kapazität als für Disaster Recovery konfiguriert werden. Alternativ können die Clouddienste skalieren out ggf. bei einem Notfall und Failover. Dieser Ansatz erfordert erheblichen Investitionen in Anwendung, aber es hat erhebliche Vorteile. Hierzu gehören niedrig und garantierte Wiederherstellungszeit, kontinuierliche Recovery-Standorten und effiziente Nutzung von Kapazität testen.

Eine vollständige Erläuterung der verteilte Design ist außerhalb des Bereichs dieses Dokuments. Weitere Informationen finden Sie unter [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](https://aka.ms/drtechguide).

##<a name="virtual-machines"></a>Virtuelle Computer

Wiederherstellung der Infrastruktur als ein Service (IaaS) virtuelle Maschinen (VMs) ähnelt Plattform als Service (PaaS) berechnen Recovery in vielerlei Hinsicht. Gibt es wichtige Unterschiede jedoch, die VM und die VM-Datenträger eine VM IaaS besteht.

  * __Azure Backup-Cross Region Backups erstellen, die Anwendung konsistent sind__.
  [Azure Backup](https://azure.microsoft.com/services/backup/) können Kunden Sicherungskopien Anwendung konsistent über mehrere VM-Festplatten und Replikation von Backups Regionen unterstützt. Hierzu wählen Geo-Replikation backup Depot zum Zeitpunkt der Erstellung. Beachten Sie, dass die Replikation der Sicherungskopie Depot muss zum Zeitpunkt der Erstellung konfiguriert werden. Es kann später festgelegt werden. Wenn ein Bereich verloren geht, wird Microsoft Sicherungskopien die Kunden zur Verfügung. Kunden werden zu ihren konfigurierten Wiederherstellungspunkte wiederherstellen.

  * __Trennen Sie den Datenträger aus der Betriebssystem-CD__. Ein wichtiger Aspekt für IaaS VMs ist, dass Sie die Betriebssystem-CD ändern können, ohne die VM neu zu erstellen. Dies ist kein Problem Ihre Wiederherstellungsstrategie nach dem Notfall erneut bereitstellen. Es könnte ein Problem jedoch verwenden Sie Warm Spare Ansatz Kapazität reservieren. Ordnungsgemäße Implementierung muss den Betriebssystem-Datenträger auf primären und sekundären Standorten bereitgestellt und müssen die Daten auf einem separaten Laufwerk gespeichert. Verwenden Sie möglichst eine Betriebssystem-Konfiguration, die an beiden Standorten bereitgestellt werden kann. Nach einem Failover müssen Sie dann Ihre vorhandenen IaaS VMs in sekundären DC das Datenlaufwerk zuordnen. Verwenden Sie AzCopy, um Snapshots der Datenträger Daten an remote-Standorten kopieren.

  * __Beachten Sie mögliche Konsistenzprobleme nach einem Geo-Failover mehrere VM-Datenträger__. VM-Datenträger als Azure Storage Blobs implementiert und haben die gleichen Geo-Replikation. Sofern [Azure Backup](https://azure.microsoft.com/services/backup/) verwendet wird, gibt es keine Garantie der Konsistenz auf Festplatten, Geo-Replikation ist asynchron und separat repliziert. Einzelne VM-Datenträger sind garantiert bei einer konsistenten Zustand nach Geo-Failover, aber nicht auf Festplatten. Dies kann in einigen Fällen (z. B. bei Festplatten-Striping) beeinträchtigen.

##<a name="storage"></a>Speicher

###<a name="recovery-by-using-geo-redundant-storage-of-blob-table-queue-and-vm-disk-storage"></a>Wiederherstellung mit Geo redundanter Speicherung von Blob, Tabelle Warteschlange und VM-Festplattenspeicher

In Azure-Blobs, Tabellen, Warteschlangen und VM sind Festplatten Geo-repliziert standardmäßig. Dies wird als Geo-Redundant Storage (GRS) bezeichnet. GRS repliziert Daten in gepaarten Datacenter Hunderte von Teilen in einer bestimmten geografischen Region Meilen. G bietet zusätzliche Haltbarkeit bei einem großen Rechenzentrum Notfall. Microsoft-Steuerelemente bei einem Failover und Failover ist auf Katastrophen in der ursprünglichen primären Standort nicht behebbarer in einem angemessenen Zeitraum gilt. In einigen Szenarien kann dies mehrere Tage sein. Daten werden normalerweise innerhalb weniger Minuten repliziert, obwohl Synchronisierungsintervall noch nicht durch eine Vereinbarung zum Servicelevel abgedeckt ist.

Bei einem Failover Geo werden unverändert, wie das Konto zugegriffen wird (die Taste URL und Konto wird nicht geändert). Das Speicherkonto, jedoch in einer anderen Region nach einem Failover werden. Dies konnte Programme auswirken, die regionale Affinität mit dem Storage-Konto erforderlich. Auch für Dienste und Programme, die nicht im gleichen Datencenter ein Speicherkonto erfordern möglicherweise Cross Datacenter Latenz und Bandbreite Zuschläge Gründe Datenverkehr Failover-Bereich vorübergehend zwingende. Dies könnte in Disaster Recovery Gesamtstrategie Faktor.

Neben Automatische Failover von g bereitgestellt hat Azure Dienst, der Lesezugriff auf die Kopie der Daten in der sekundären Speicherort gibt. Lesezugriff Geo redundanten Speicher (RA-GRS) wird aufgerufen.

Weitere Informationen zum GRS und RA-GRS Speicher finden Sie unter [Azure Storage Replication](../storage/storage-redundancy.md).

###<a name="geo-replication-region-mappings"></a>Geo-Replikation Region Zuordnungen:

Es ist wichtig zu wissen, wo Ihre Daten Geo repliziert, um zu wissen, wo die anderen Instanzen von Daten bereitzustellen, die regionale Affinität mit dem Speicher erforderlich. Die folgende Tabelle zeigt die primären und sekundären Standort Paare:

[AZURE.INCLUDE [paired-region-list](../../includes/paired-region-list.md)]

###<a name="geo-replication-pricing"></a>Geo-Replikation Preisgestaltung:

Geo-Replikation ist im aktuellen Preisstruktur für Azure-Speicher enthalten. Geo-Redundant Storage (GRS) wird aufgerufen. Möchten Sie nicht Ihre Daten geografisch repliziert können Sie Geo-Replikation für Ihr Konto deaktivieren. Dies heißt lokal redundanter Speicher und wird zu einem reduzierten Preis im Vergleich zu GRS belastet.

###<a name="determining-if-a-geo-failover-has-occurred"></a>Bestimmen, ob ein Geo-Failover stattgefunden hat

Geo-Failover stattfindet, werden diese [Azure Service Health Dashboard](https://azure.microsoft.com/status/)gebucht. Programme können erkennen, jedoch durch die Überwachung des geografischen Region für ihre Storage-Konto eine automatisierte Methode implementieren. Hiermit können andere Recovery-Vorgänge wie Aktivierung von Datenverarbeitungsressourcen in geografischen Region auslösen an Speicher verschoben werden. Eine Abfrage möglich für diesen Service Management-API mit [Speichereigenschaften abrufen](https://msdn.microsoft.com/library/ee460802.aspx). Die entsprechenden Eigenschaften sind:

    <GeoPrimaryRegion>primary-region</GeoPrimaryRegion>
    <StatusOfPrimary>[Available|Unavailable]</StatusOfPrimary>
    <LastGeoFailoverTime>DateTime</LastGeoFailoverTime>
    <GeoSecondaryRegion>secondary-region</GeoSecondaryRegion>
    <StatusOfSecondary>[Available|Unavailable]</StatusOfSecondary>

###<a name="vm-disks-and-geo-failover"></a>VM-Festplatten und Geo-failover

Wie im Abschnitt auf VM-Festplatten, gibt es keine Garantien Datenkonsistenz über VM Festplatten nach einem Failover. Um die Richtigkeit der Backups sicherzustellen, sollte backup-Produkten wie Data Protection Manager zum Sichern und Wiederherstellen von Anwendungsdaten verwendet werden.

##<a name="database"></a>Datenbank

###<a name="sql-database"></a>SQL-Datenbank

Azure SQL-Datenbank bietet zwei Typen von Recovery: Geo wiederherstellen und aktive Geo-Replikation.

####<a name="geo-restore"></a>Geo-Wiederherstellung

[Geo-Wiederherstellung](../sql-database/sql-database-recovery-using-backups.md#geo-restore) steht außerdem Basic, Standard und Premium-Datenbanken. Es bietet die Standardwiederherstellungsoption, wenn die Datenbank wegen eines Vorfalls in der Region nicht verfügbar ist, die Datenbank gehostet wird. Ähnlich wie Point-In-Time-Wiederherstellung, Geo-Wiederherstellung Datenbanksicherungskopien Geo redundante Azure-Speicher verwendet. Der geometrische repliziert Sicherungskopie wiederhergestellt, und deshalb anfällig für Ausfälle Speicher im Bereich für primäre. Weitere Informationen finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank oder Failover zu einem sekundären](../sql-database/sql-database-disaster-recovery.md).

####<a name="active-geo-replication"></a>Aktive Geo-Replikation

[Aktive Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md) ist für alle Datenbank-Ebenen verfügbar. Es dient zur Anwendung, die kürzere Recovery benötigt als geografische Wiederherstellung bieten. Aktive Geo-Replikation können Sie bis zu vier lesbare sekundäre auf Servern in unterschiedlichen Regionen erstellen. Sie können Failover auf die sekundäre initiieren. Außerdem kann aktive Geo-Replikation unterstützt Anwendungsszenarios Aktualisierung oder Umsetzung sowie Lastenausgleich für schreibgeschützte Arbeitslasten verwendet werden. Weitere Informationen finden Sie unter [Geo - Replikation](../sql-database/sql-database-geo-replication-portal.md) und [ein Failover auf den sekundären Datenbank](../sql-database/sql-database-geo-replication-failover-portal.md). Einzelheiten zum Entwerfen und implementieren Applikationen und Applikationen Upgrades ohne Ausfallzeiten finden Sie unter [Entwerfen einer Anwendung für die Cloud Disaster Recovery mit aktive Geo-Replikation in SQL-Datenbank](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Verwalten von parallelen Updates von Cloudanwendungen mit SQL Datenbank aktive Geo-Replikation](../sql-database/sql-database-manage-application-rolling-upgrade.md) .

###<a name="sql-server-on-virtual-machines"></a>SQL Server auf virtuellen Computern

Eine Vielzahl von Optionen stehen für Recovery und hohe Verfügbarkeit für SQL Server 2012 (und höher) in Azure virtuellen Computer ausgeführt. Weitere Informationen finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

##<a name="other-azure-platform-services"></a>Andere Dienste Azure-Plattform

Wenn in mehreren Regionen Azure Cloud-Dienst ausführen, müssen Sie auf jeder Ihrer Abhängigkeiten berücksichtigen. In den folgenden Abschnitten vorausgesetzt dienstspezifische Anleitung gleichen Azure Service in anderen Azure-Rechenzentrum verwenden müssen. Dies umfasst Konfiguration und Daten-Replikationsaufgaben.

>[AZURE.NOTE]In einigen Fällen können folgendermaßen um dienstspezifische Ausfall als ein gesamtes Rechenzentrum Ereignis zu verringern. Aus der Anwendungsperspektive dienstspezifische Ausfall möglicherweise nur eingeschränkt und erfordert den Dienst vorübergehend alternative Azure Region migrieren.

###<a name="service-bus"></a>Servicebus

Azure Service Bus verwendet einen eindeutigen Namespace, der nicht Azure Bereiche umfassen. So ist die erste Anforderung erforderlichen Service Bus Namespaces in anderen Regionen einrichten. Es gibt auch Hinweise für die Dauerhaftigkeit von Nachrichten in der Warteschlange. Es gibt mehrere Strategien für die Replikation von Nachrichten in Azure Regionen. Einzelheiten zu diesen Replikationsstrategien und andere Strategien zur Wiederherstellung finden Sie unter [Best Practices für isolierende Anwendung Service Bus Ausfällen und Katastrophen](../service-bus-messaging/service-bus-outages-disasters.md). Andere Aspekte Verfügbarkeit finden Sie unter [Service Bus (Verfügbarkeit)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="app-service"></a>App Service

Zum Migrieren einer Anwendung Azure App Service Web Apps oder Mobile Apps sekundären Azure Region müssen Sie eine Sicherung der Website veröffentlicht. Wenn der Ausfall nicht das gesamte Datencenter Azure, kann FTP verwenden, um eine Sicherungskopie des Inhalts der Website downloaden möglich. Erstellen Sie neue app dann Alternativer Bereich, wenn Sie zuvor damit Kapazitäten reserviert haben. Veröffentlichen Sie die Site in die neue Region und ändern Sie erforderlichen Konfiguration. Hierzu zählen Datenbankverbindungszeichenfolgen oder andere regionale Einstellungen. Gegebenenfalls fügen Sie die Website SSL-Zertifikat hinzu und ändern Sie den DNS CNAME-Eintrag, sodass der benutzerdefinierten Domänennamen erneut bereitgestellten Azure Web App-URL zeigt.

###<a name="hdinsight"></a>HDInsight

HDInsight zugeordneten Daten werden standardmäßig in Azure BLOB-Speicher gespeichert. HDInsight erfordert ein Hadoop Cluster MapReduce Aufträge im Bereich Speicher-Konto befinden muss, die die analysierten Daten enthält. Verwenden die Azure-Speicher zur Geo-Replikationsfunktion möglich die Daten in der sekundären Region, wo die Daten repliziert wurden, wenn aus irgendeinem Grund die primäre Region nicht verfügbar ist. Einen neuen Hadoop Cluster können in der Region die Daten repliziert wurden und weiter verarbeitet. Andere Aspekte Verfügbarkeit finden Sie unter [HDInsight (Verfügbarkeit)](./resiliency-technical-guidance-recovery-local-failures.md#other-azure-platform-services).

###<a name="sql-reporting"></a>SQL Reporting

Zu diesem Zeitpunkt erfordert das Wiederherstellen aus einer Azure-Region SQL Reporting mehrmals in verschiedenen Azure-Regionen. Diese SQL Reporting-Instanzen sollten dieselben Daten zugreifen und diese Daten müssen eigene Wiederherstellung im Katastrophenfall planen. Sie können auch externe Sicherungskopien der RDL-Datei für jeden Bericht verwalten.

###<a name="media-services"></a>Media Services

Azure Media Services hat unterschiedliche Recovery-Ansatz für das Codieren und streaming. Normalerweise ist streaming in einen regionalen Ausfall. Zur Vorbereitung dieser benötigen Sie ein Media Services-Konto in zwei verschiedenen Azure-Regionen. Der codierte Inhalt sollte in beiden Bereichen befinden. Bei einem Ausfall können Sie alternative Region streaming Datenverkehr umleiten. Verschlüsselung in Azure Regionen möglich. Ist Codierung zeitkritisch, z.B. bei live-Ereignis verarbeiten, müssen Sie zu einem anderen Rechenzentrum Aufträge bei Fehler senden vorbereitet.

###<a name="virtual-network"></a>Virtuelles Netzwerk

Konfigurationsdateien beinhalten am schnellsten ein virtuelles Netzwerk in einer alternativen Azure Region einrichten. Nach dem Konfigurieren des virtuellen Netzwerks in der primären Azure Region für das aktuelle Netzwerk zu einer Netzwerk-Konfigurationsdatei [Exportieren virtual Network Settings](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Bei Ausfall der primären Region aus der gespeicherten Konfigurationsdatei [das virtuelle Netzwerk wiederherstellen](../virtual-network/virtual-networks-create-vnet-classic-portal.md) . Andere Cloud-Dienste, virtuelle Computer bzw. standortübergreifend neues virtuelles Netzwerk zu konfigurieren.

##<a name="checklists-for-disaster-recovery"></a>Prüflisten für Disaster recovery

##<a name="cloud-services-checklist"></a>Checkliste für Cloud-Services

  1. Überprüfen Sie die Clouddienste Abschnitt dieses Dokuments.
  2. Erstellen einer Wiederherstellungsstrategie Cross-Region.
  3. Verstehen Sie Reservierung von Kapazität in anderen Regionen Kompromisse.
  4. Verwenden Sie Datenverkehr routing-Tools wie Azure Traffic Manager.

##<a name="virtual-machines-checklist"></a>Checkliste für virtuelle Computer

  1. Überprüfen des virtuellen Maschinen Abschnitts dieses Dokuments.
  2. Sicherungsprogramm [Azure](https://azure.microsoft.com/services/backup/) Anwendung konsistente Backups Regionen erstellen.

##<a name="storage-checklist"></a>Speicher-Checkliste

  1. Lesen Sie den Abschnitt Speichern dieses Dokuments.
  2. Deaktivieren Sie Geo-Replikation von Speicherressourcen nicht.
  3. Verstehen Sie Alternativer Bereich Geo-Replikation bei Failover.
  4. Erstellen Sie benutzerdefinierte Strategien für benutzergesteuerten Failoverstrategien.

##<a name="sql-database-checklist"></a>SQL Datenbank-Checkliste

  1. Überprüfen des SQL-Datenbank-Abschnitts dieses Dokuments.
  2. Verwenden Sie [Geografische Wiederherstellung](../sql-database/sql-database-recovery-using-backups.md#geo-restore) oder [Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md) .

##<a name="sql-server-on-virtual-machines-checklist"></a>SQL Server auf virtuellen Maschinen-Checkliste

  1. Überprüfen Sie die SQL Server auf diesem virtuellen Computer.
  2. Verwenden Sie Cross-AlwaysOn Availability Gruppen oder später.
  3. Alternativ sollten Sicherung und Wiederherstellung BLOB-Speicher.

##<a name="service-bus-checklist"></a>Service Bus-Checkliste

  1. Überprüfen des Service Bus-Abschnitts dieses Dokuments.
  2. Konfigurieren Sie einen Service Bus-Namespace in einen anderen Bereich.
  3. Berücksichtigen Sie benutzerdefinierte Replikationsstrategien Nachrichten Regionen.

##<a name="app-service-checklist"></a>App-Checkliste

  1. Überprüfen Sie die App Service-Abschnitt dieses Dokuments.
  2. Verwalten Sie Backups außerhalb des primären Standorten.
  3. Wenn Ausfall partiell ist, Abrufen der aktuellen Website mit FTP.
  4. Die Website für neue oder vorhandene Website in einer anderen Region bereitstellen möchten.
  5. Planen Sie Neukonfiguration Anwendung und DNS CNAME-Einträge.

##<a name="hdinsight-checklist"></a>HDInsight-Checkliste

  1. Überprüfen Sie im HDInsight-Abschnitt dieses Dokuments.
  2. Erstellen eines neuen Clusters Hadoop mit replizierten Daten.

##<a name="sql-reporting-checklist"></a>Prüfliste SQL Reporting

  1. Lesen Sie den Abschnitt SQL Reporting.
  2. Verwalten einer alternativen SQL Reporting-Instanz in einen anderen Bereich.
  3. Verwalten Sie einen eigenen Plan das Ziel für diese Region zu replizieren.

##<a name="media-services-checklist"></a>Media Services-Prüfliste

  1. Überprüfen der Media Services-Abschnitt dieses Dokuments.
  2. Erstellen eines Kontos Media Services in einem anderen Bereich.
  3. Codieren Sie den Inhalt beider Bereiche streaming Failover unterstützen.
  4. Senden Sie Codierung Aufträge auf einen anderen Bereich im Störungsfall Service.

##<a name="virtual-network-checklist"></a>Virtuelles Netzwerk Checkliste

  1. Überprüfen Sie virtuelles Netzwerk-Abschnitt dieses Dokuments.
  2. Verwendung exportiert virtual Network Settings in einen anderen Bereich neu erstellt.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe [Azure Resiliency technische](./resiliency-technical-guidance.md)Anleitung. Im nächste Artikel dieser Reihe konzentriert sich auf die [Wiederherstellung aus einem lokalen Rechenzentrum in Azure](./resiliency-technical-guidance-recovery-on-premises-azure.md).
