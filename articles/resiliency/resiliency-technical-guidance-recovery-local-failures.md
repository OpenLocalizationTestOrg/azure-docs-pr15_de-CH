<properties
   pageTitle="Technische Anleitung: Recovery bei lokalen in Azure | Microsoft Azure"
   description="Artikel verstehen und Entwerfen leistungsstarke, hochverfügbare, fehlertolerante Applikationen sowie Disaster Recovery konzentriert sich lokale Fehler in Azure planen."
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

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Technische Anleitung Azure Stabilität: Recovery bei lokalen in Azure

Sind zwei primäre Verfügbarkeit:

* Der Ausfall der Geräte wie Laufwerke und Server
* Erschöpfung der kritischen Ressourcen, wie Compute unter Maximale Belastung

Azure bietet eine Kombination von Ressourcen-Management, Elastizität Lastenausgleich und Partitionierung, um hohe Verfügbarkeit unter diesen Umständen aktivieren. Einige dieser Funktionen werden für alle Azure Services automatisch ausgeführt. Jedoch in einigen Fällen müssen Anwendungsentwickler zusätzliche Arbeit nutzen.

##<a name="cloud-services"></a>Cloud-Dienste

Azure Cloud Services besteht aus Sammlungen von mindestens Web- oder Worker-Rollen. Eine oder mehrere Instanzen einer Rolle können gleichzeitig ausgeführt werden. Die Konfiguration bestimmt die Anzahl der Instanzen. Instanzen werden überwacht und über eine Komponente namens den Fabric Controller verwaltet. Der Fabric Controller erkennt und automatisch auf Software und Hardware-Fehler reagiert.

Rolle in einem eigenen virtuellen Computer (VM) ausgeführt wird und der Fabric Controller durch einen Gast-Agent kommuniziert. Gast-Agent sammelt Ressource und Knoten Metriken einschließlich VM Verwendung, Status, Protokolle, Ressourcenverwendung, Ausnahmen und Fehler. Der Fabric Controller fragt der Gast-Agent in konfigurierbaren Intervallen und Neustart der VM Gast-Agent reagiert nicht. Bei Hardware-Ausfällen der zugeordneten Fabric Controller alle betroffenen Instanzen zu einem neuen Hardware-Knoten bewegt und neu im Netzwerk Datenverkehr vorhanden.

Um diese Funktionen nutzen, sollten Entwickler sicherstellen, dass alle Rollen speichern Sie Status auf die Instanzen. Stattdessen sollte alle permanente Daten aus dem dauerhaften Speicher Azure Storage oder Azure SQL-Datenbank zugegriffen werden. Dies ermöglicht alle Anfragen. Es bedeutet auch, dass Rolleninstanzen jederzeit ausfallen können, ohne Inkonsistenzen in den vorübergehenden oder permanenten Zustand des Dienstes.

Die Anforderung an extern speichern hat mehrere Implikationen. Dies bedeutet, z. B. alle zugehörige Änderungen einer Azure Storage-Tabelle in einer einzigen Transaktion Entität Gruppe ggf. geändert werden. Natürlich ist es nicht immer möglich, alle in einer einzigen Transaktion. Sie müssen besondere Vorsicht um sicherzustellen, dass die Rolle Instanz Fehler keine Probleme verursachen, wenn sie lang andauernde Vorgänge unterbrechen, die den Zustand des Dienstes zwei oder mehr Updates umfassen. Wenn eine andere Rolle versucht, einen Vorgang zu wiederholen, muss vorhersehen und der Fall, in dem die Arbeit teilweise abgeschlossen wurde.

Angenommen Sie, einen Dienst, der Daten über mehrere Informationsspeicher Partitionen. Wenn eine Worker-Rolle wird ausfällt, während es einen verlagern, könnte die Umsetzung der Splitter nicht abgeschlossen. Oder die Umsetzung kann von Anfang an eine andere Arbeitskraft Rolle Daten oder Datenverluste verursachen. Um Problemen vorzubeugen, muss lang andauernde Vorgänge eines oder beide der folgenden:

 * *Idempotent*: wiederholbare ohne Nebeneffekte. Um Idempotent sein müssen ein lang andauernder Vorgang dieselbe Wirkung egal wie oft es ausgeführt wird, selbst wenn er während der Ausführung unterbrochen wird.
 * *Inkrementell neu*: vom letzten Punkt aus fortsetzen. Inkrementell neu gestartet werden, sollte ein lang andauernder Vorgang eine Folge von kleineren atomarischen Vorgängen bestehen. Auch aufzeichnen den Fortschritt im dauerhaften Speicher, damit jeder nachfolgende Aufruf nimmt dem Vorgänger beendet.

Schließlich sollten alle lang andauernde Vorgänge wiederholt aufgerufen werden, bis sie erfolgreich. Beispielsweise kann ein Bereitstellungsvorgang in eine Azure-Warteschlange platziert; dann durch eine Worker-Rolle aus der Warteschlange entfernt nur bei Erfolg Garbagecollection möglicherweise zu bereinigen Daten, die Vorgänge unterbrochen.

###<a name="elasticity"></a>Elastizität

Die anfängliche Anzahl der Instanzen für jede Rolle wird in jede Rolle Konfiguration bestimmt. Administratoren sollten jede Rolle mit zwei oder mehr Instanzen auf Grundlage der erwarteten Auslastung konfigurieren. Jedoch können problemlos skalieren Instanzen oder Sie als Verwendung ändern. Sie können dies manuell tun, im Azure-Portal oder diesen Prozess automatisieren, mit Windows PowerShell, die Service Management-API oder Drittanbieter-Tools. Weitere Informationen finden Sie unter [Skalieren einer Anwendung](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Partitionierung

Azure Fabric Controller verwendet zwei Partitionsarten:

* *Domäne aktualisieren* wird verwendet, um Instanzen des Dienstes in Gruppen aktualisieren. Azure stellt Dienstinstanzen in mehreren aktualisierungsdomänen. Der Fabric Controller für ein direktes Update bringt alle Instanzen einer aktualisieren, aktualisiert und dann vor dem Verschieben in den nächsten Domäne aktualisieren neu. Dieser Ansatz verhindert, dass den gesamten Dienst wird während der Aktualisierung nicht verfügbar.
* Eine *Fehlerdomäne* definiert potenzielle Fehlerquellen Hardware- oder. Für jede Rolle, die über mehrere Instanzen verfügt, sorgt der Fabric Controller Instanzen über mehrere Fehlerdomänen isolierte Hardwarefehler Unterbrechung des Dienstes verhindern verteilt werden. Fehlerdomänen steuern alle gegenüber Server und Clusterfehler.

[Azure Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/) garantiert, dass zwei oder mehr Webrolleninstanzen verschiedene Fehler bereitgestellt und Domänen aktualisieren sie externe Konnektivität mindestens 99,95 % der Zeit haben. Im Gegensatz zu aktualisierungsdomänen ist keine Möglichkeit, die Anzahl der Fehlerdomänen steuern. Azure automatisch Fehlerdomänen reserviert und Instanzen auf diese verteilt. AT mindestens die ersten beiden Instanzen jeder Rolle werden verschiedene Fehler und Aktualisieren von Domänen, um sicherzustellen, dass jede Rolle mit mindestens zwei Instanzen die SLA erfüllen. Dies wird im folgenden Diagramm dargestellt.

![Vereinfachte Ansicht Fehler der Isolation von Domänen](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Lastenausgleich

Alle eingehender Datenverkehr zu einer Webrolle durchläuft eine statusfreie Lastenausgleich der Clientanforderungen unter den Rolleninstanzen verteilt. Einzelne Instanzen keinen öffentliche IP-Adressen und sind nicht direkt aus dem Internet adressierbar. Webrollen sind statusfrei, sodass jede Clientanforderung an jede Instanz der Rolle weitergeleitet werden kann. Ein [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) Ereignis wird alle 15 Sekunden ausgelöst. Sie können dies angeben, ob die Rolle empfangen kann, oder ob es ausgelastet ist und genommen Lastenausgleich Drehung.

##<a name="virtual-machines"></a>Virtuelle Computer

Azure Virtual Machines unterscheidet sich von Plattform Service (PaaS) berechnen Rollen in mehrfacher Hinsicht hohe Verfügbarkeit. In einigen Fällen müssen Sie zusätzliche Arbeit um hohe Verfügbarkeit zu gewährleisten.

###<a name="disk-durability"></a>Datenträger Dauerhaftigkeit

PaaS Rolleninstanzen unterscheidet Daten auf virtuellen Laufwerken persistent selbst, wenn der virtuelle Computer verschoben wird. Azure virtuelle Computer verwenden vorhanden VM-Datenträger als Blobs in Azure-Speicher. Aufgrund der Verfügbarkeit der Azure-Speicher ist die Daten auf einem virtuellen Computer auch hochverfügbar.

Beachten Sie, dass Laufwerk D (Systemressourcen) die Ausnahme von dieser Regel. Laufwerk D ist tatsächlich physischen Speicher der Rackserver, der die VM befindet, und seine Daten verloren VM wiederverwendet wird. Laufwerk D soll für die temporäre Speicherung. Unter Linux macht Azure "in der Regel" (aber nicht immer) der Festplatte temporäre als Block-Gerät/dev/sdb. Es ist oft von Azure Linux-Agent als /mnt/resource oder mnt Bereitstellungspunkte (konfigurierbar über /etc/waagent.conf) bereitgestellt.

###<a name="partitioning"></a>Partitionierung

Azure nativ versteht die Ebenen in einer PaaS-Anwendung (Webrolle und Worker-Rolle) und somit kann ordnungsgemäß verteilen Problem- und Update domänenübergreifend. Ebenen in einer Infrastruktur als Service (IaaS) Anwendung müssen dagegen manuell durch Verfügbarkeit legt definiert werden. Verfügbarkeit sind für eine SLA unter IaaS.

![Verfügbarkeit wird für Azure virtuelle Maschinen](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

In der obigen Abbildung sind Internet Information Services (IIS)-Ebene (die als Web app Ebene funktioniert) und SQL-Ebene (die als Datenebene funktioniert) zu anderen Verfügbarkeit zugewiesen. Dadurch wird sichergestellt, dass alle Instanzen der einzelnen Ebenen Hardwareredundanz von virtuellen Maschinen auf Fehlerdomänen verteilt, und ganze Ebenen während einer Aktualisierung nicht entfernt werden.

###<a name="load-balancing"></a>Lastenausgleich

Sollten die VMs Netzwerkverkehr über diese verteilt haben, müssen Sie über einen bestimmten TCP- oder UDP-Endpunkt VMs in eine Anwendung und Load Balance gruppieren. Weitere Informationen finden Sie unter [Load balancing virtuellen Maschinen](../virtual-machines/virtual-machines-linux-load-balance.md). Die VMs Eingaben aus einer anderen Quelle (z. B. Warteschlangenmechanismus) erhalten, ist ein System zum Lastenausgleich nicht erforderlich. Lastenausgleich mithilfe eine grundlegenden Gesundheitskontrolle, ob Datenverkehr an den Knoten gesendet werden soll. Es kann auch erstellen Prüfpunkte um anwendungsspezifische Zustandsmetriken implementieren, die bestimmen, ob die VM-Datenverkehr erhalten soll.

##<a name="storage"></a>Speicher

Azure-Speicher ist der geplanten dauerhaften Daten für Azure. Es bietet Blob, Tabelle, Warteschlange und VM-Speicherplatz. Hohe Verfügbarkeit in einem einzigen Rechenzentrum verwendet eine Kombination von Replikation und Ressourcenmanagement. Azure Storage Verfügbarkeits-SLA garantiert, dass mindestens 99,9 Prozent der Zeit:

* Ordnungsgemäß formatierten Anfragen zum Hinzufügen, aktualisieren, lesen und Löschen von Daten erfolgreich und korrekt verarbeitet werden.
* Speicherkonten haben Konnektivität zum Internet-Gateway.

###<a name="replication"></a>Replikation

Azure-Speicher ermöglicht Daten Haltbarkeit, da mehrfache Kopien aller Daten auf verschiedenen Laufwerken über unabhängige physische Speichersubsysteme im Bereich. Daten synchron repliziert und alle Kopien verpflichten, bevor der Schreibvorgang bestätigt wird. Azure-Speicher ist stark konsistente Lesevorgänge garantiert sind entsprechend der letzten schreibt. Darüber hinaus werden Kopien der Daten kontinuierlich um reparieren Bit Rot, eine oft übersehene Bedrohung der Integrität gespeicherter Daten gescannt.

Replikation mit Azure Storage Services nutzen. Der Entwickler muss zusätzliche Arbeit, lokaler Fehler wiederherzustellen.

###<a name="resource-management"></a>Ressourcenmanagement

Speicherkonten Mai 2014 erstellt kann (das vorherige Maximum war 200 TB) bis zu 500 TB anwachsen. Wenn zusätzlicher Speicherplatz erforderlich ist, müssen Programme soll mehrere Storage-Konten verwenden.

###<a name="virtual-machine-disks"></a>Festplatten mit virtuellen Maschinen

Festplatte des virtuellen Computers wird als Seitenblob in Azure-Speicher, damit dieselben Eigenschaften Haltbarkeit und skalierbar als BLOB-Speicher gespeichert. Dadurch werden die Daten auf der Festplatte des virtuellen Computers persistent, auch wenn der Server mit der VM nicht die VM auf einem anderen Server neu gestartet werden muss.

##<a name="database"></a>Datenbank

###<a name="sql-database"></a>SQL-Datenbank

Azure SQL-Datenbank bietet Datenbank als Dienst. Anwendung schnell bereitstellen, Daten und Abfrage relationaler Datenbanken einfügen kann. Es bietet viele der vertrauten SQL Server-Features und Funktionen, und abstrahiert die Beweislast Hardware, Konfiguration, Patches und Stabilität.

>[AZURE.NOTE] Azure SQL-Datenbank bietet keine 1: 1-Featureparität mit SQL Server. Es soll eine andere Gruppe von Vorschriften – eine zu decken, der eindeutig Cloudanwendungen (Flexible Skalierung, Datenbank als Dienst Wartungskosten usw.) geeignet ist. Weitere Informationen finden Sie unter [eine Wolke SQL Server Option auswählen: Azure SQL (PaaS) Datenbank oder SQL Server auf Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>Replikation

Azure SQL-Datenbank bietet integrierte Flexibilität auf Knotenebene Fehler. Alle Schreibvorgänge in eine Datenbank werden zwei oder mehr Hintergrund-Knoten über ein Quorum Commit-Methode automatisch repliziert. (Die primäre und sekundäre mindestens müssen bestätigen, dass die Aktivität im Transaktionsprotokoll geschrieben wird, bevor die Transaktion als Erfolg angesehen und gibt.) Bei Ausfall eines Knotens automatisch ein Failover die Datenbank auf einen sekundären Replikate. Dadurch wird für Clientanwendungen eine vorübergehende Verbindung unterbrochen. Aus diesem Grund müssen alle Azure SQL-Datenbank-Clients einige vorübergehende Verbindung Behandlung implementieren. Weitere Informationen finden Sie unter [Service-spezifische wiederholen](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Ressourcenmanagement

Jede Datenbank beim Erstellen, wird mit einer oberen Größenlimit konfiguriert. Die derzeit maximale Größe beträgt 1 TB (Größe variieren basierend auf der Dienstebene [Dienstebenen und Performance von Azure SQL-Datenbanken](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Bei eine Datenbank der obere Grenzwert erreicht, lehnt zusätzliche INSERT- oder UPDATE-Befehle. (Abfragen und Löschen von Daten ist immer noch möglich.)

In einer Datenbank verwendet Azure SQL-Datenbank eine Fabric zum Verwalten von Ressourcen. Verwendet jedoch statt einer Fabric Controller eine Ringtopologie um zu erkennen. Jedes Replikat in einem Cluster besitzt die beiden Nachbarn und erkennt, wenn sie gehen verantwortlich ist. Wenn ein Replikat ausfällt, lösen seiner Nachbarn einen Neukonfiguration Agent auf einem anderen Computer neu erstellen. Modul Drosselung wird ein logischer Server nicht zu viele Ressourcen auf einem Computer verwenden oder dem Computer physische Grenzen bereitgestellt.

###<a name="elasticity"></a>Elastizität

Wenn die Anwendung mehr als 1 TB Datenbankgröße erfordert, muss es Scale-Out-Ansatz implementieren. Sie skalieren mit Azure SQL-Datenbank von manuellen Partitionierung auch Sharding Daten in SQL-Datenbanken. Dadurch Dezentrales Skalieren bietet die Möglichkeit, mit nahezu linear Kosten wachsen. Elastische Wachstum oder bedarfsorientierte Kapazität kann inkrementelle Kosten entsprechend dem Bedarf wachsen da Datenbanken basierend auf der tatsächlichen Durchschnittsgröße pro Tag nicht basierend auf maximale Größe berechnet werden.

##<a name="sql-server-on-virtual-machines"></a>SQL Server auf virtuellen Computern

Installieren von SQL Server (Version 2014 oder später) auf Azure Virtual Machines können Sie herkömmliche Verfügbarkeit von SQL Server nutzen. Dazu gehören AlwaysOn Availability Gruppen und später. Beachten Sie, dass Azure VMs, Speicher und Netzwerke unterschiedliche Funktionseigenschaften als ein lokales, nicht virtualisierten IT-Infrastruktur. Eine erfolgreiche Implementierung von hoher Verfügbarkeit/Wiederherstellung (HA/DR) SQL Server-Lösung in Azure erfordert, dass diese Unterschiede und Ihrer Lösung entwerfen für ihre Unterbringung.

###<a name="high-availability-nodes-in-an-availability-set"></a>Knoten in einem Verfügbarkeit hohe Verfügbarkeit

Beim Implementieren einer Lösung mit hoher Verfügbarkeit in Azure können Sie die Verfügbarkeit in Azure festgelegt Hochverfügbarkeits-Knoten in separaten Fehlerdomänen und Domänen zu aktualisieren. Sind, ist der Verfügbarkeit einer Azure Konzept. Es ist eine bewährte Methode, die Sie befolgen sollten, um sicherzustellen, dass Ihre Datenbanken tatsächlich hochverfügbare, ob AlwaysOn Availability Gruppen später oder etwas anderes verwenden. Wenn Sie diese bewährte Methode nicht folgen, Sie unter der falschen Annahme möglicherweise hoch verfügbar ist. Aber in Wirklichkeit Knoten können alle gleichzeitig weil sie sich zufällig in derselben Fehlerdomäne in Azure Bereich platziert werden.

Diese Empfehlung ist nicht mit Protokollversand nach Bedarf. Als Disaster Recovery-Feature sollten die Server in separaten Bereichen Azure ausgeführt werden. Diese Bereiche sind separate Fehlerdomänen.

Für Azure Cloud Services VMs bereitgestellt durch das Verwaltungsportal Verfügbarkeit dieselben sein müssen Sie sie im selben Cloud-Dienst bereitstellen. Diese Einschränkung keinen VMs durch Azure Ressourcenmanager (aktuelle Portal) bereitgestellt. Für Verwaltungsportal VMs in Azure-Cloud-Dienst bereitgestellt, können dieselben Verfügbarkeit nur Knoten im selben Cloud-Dienst teilnehmen. Darüber hinaus sollte die Cloud Services VMs im gleichen virtuellen Netzwerk sicherstellen, dass sie ihre IP-Adressen auch nach der Reparatur Service verwalten. DNS-Update Störungen vermieden werden.

###<a name="azure-only-high-availability-solutions"></a>Nur Azure: High-Availability Solutions

Eine Lösung mit hoher Verfügbarkeit können für die SQL Server-Datenbanken in Azure AlwaysOn Availability Gruppen oder später.

Das folgende Diagramm veranschaulicht die Architektur von AlwaysOn Availability Gruppen auf Azure virtuelle Computer ausgeführt. Dieses Diagramm wurde den ausführlichen Artikel zu diesem Thema [hohe Verfügbarkeit und Disaster Recovery für SQL Server auf Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)entnommen.

![AlwaysOn Availability Gruppen in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

Sie können auch automatisch eine AlwaysOn Availability Gruppen Bereitstellung End-to-End Azure VMs mit AlwaysOn-Vorlage in Azure-Portal bereitstellen. Weitere Informationen finden Sie unter [SQL Server AlwaysOn bietet Microsoft Azure Portal Gallery](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Das folgende Diagramm veranschaulicht die Verwendung der datenbankspiegelung auf Azure Virtual Machines. Es wurden detaillierte Thema [hohe Verfügbarkeit und Disaster Recovery für SQL Server auf Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)entnommen.

![Spiegeln von Datenbanken in Microsoft Azure](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Beide Architekturen erfordern einen Domänencontroller. Jedoch kann durch datenbankspiegelung Serverzertifikate verwenden, um einen Domänencontroller erforderlich.

##<a name="other-azure-platform-services"></a>Andere Dienste Azure-Plattform

Programme, die auf Azure profitieren Plattformfunktionen zu lokalen. In einigen Fällen können Sie spezielle Maßnahmen zur Erhöhung der Verfügbarkeit für Ihr spezielles Szenario übernehmen.

###<a name="service-bus"></a>Servicebus

Um gegen eine zeitweilige Ausfall von Azure Service Bus zu verringern, sollten Sie in der dauerhafte clientseitige Warteschlange erstellen. Diese verwendet einen anderen lokalen Speichermechanismus vorübergehend Nachrichten gespeichert, die nicht auf Service Bus-Warteschlange hinzugefügt. Die Anwendung kann entscheiden, wie temporär gespeicherten Nachrichten zu verarbeiten, nachdem der Dienst wiederhergestellt ist. Weitere Informationen finden Sie unter [Best Practices für die Verwendung des Service Bus Leistungssteigerungen vermittelte messaging](../service-bus-messaging/service-bus-performance-improvements.md) und [Service Bus (Disaster Recovery)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>HDInsight

Azure HDInsight zugeordnet hat Daten werden standardmäßig in Azure BLOB-Speicher gespeichert. Azure-Speicher gibt hohe Verfügbarkeit und Haltbarkeit Eigenschaften für BLOB-Speicher. Die mehreren Knoten die Hadoop MapReduce Aufträge zugeordnet ist Verarbeitung auf eine vorübergehende Hadoop verteilt Datei System bietet, die bereitgestellt wird, wenn HDInsight benötigt. Ergebnisse aus einem MapReduce-Auftrag werden auch in Azure BLOB-Speicher gespeichert, damit die verarbeiteten Daten robust ist und hoch verfügbar bleibt, nachdem Hadoop Cluster hat. Weitere Informationen finden Sie unter [HDInsight (Disaster Recovery)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

##<a name="checklists-for-local-failures"></a>Prüflisten für lokale Fehler

###<a name="cloud-services"></a>Cloud-Dienste

  1. Überprüfen Sie die Clouddienste Abschnitt dieses Dokuments.
  2. Konfigurieren Sie mindestens zwei Instanzen für jede Rolle.
  3. Status im dauerhaften Speicher nicht auf Instanzen beibehalten.
  4. Behandeln Sie das StatusCheck-Ereignis ordnungsgemäß.
  5. Umschließen Sie Änderungen Transaktionen, wenn möglich.
  6. Überprüfen, ob die Rolle Arbeitskraftaufgaben Idempotent sind und neu gestartet.
  7. Vorgänge aufrufen, bis sie erfolgreich fortgesetzt.
  8. Berücksichtigen Sie automatische Skalierung Strategien.

###<a name="virtual-machines"></a>Virtuelle Computer

  1. Überprüfen des virtuellen Maschinen Abschnitts dieses Dokuments.
  2. Verwenden Sie Laufwerk D nicht für Speicherung.
  3. Gruppieren von Computern in einer Dienstebene zu Verfügbarkeit.
  4. Konfigurieren des Lastenausgleichs und optionale Prüfpunkte.

###<a name="storage"></a>Speicher

  1. Lesen Sie den Abschnitt Speichern dieses Dokuments.
  2. Verwenden Sie mehrere Storage-Konten, wenn Daten oder Bandbreite Kontingente überschreiten.

###<a name="sql-database"></a>SQL-Datenbank

  1. Überprüfen des SQL-Datenbank-Abschnitts dieses Dokuments.
  2. Implementieren Sie eine wiederholungsrichtlinie vorübergehender Fehler behandeln.
  3. Verwenden Sie partitionieren/Sharding Scale-Out-Strategie.

###<a name="sql-server-on-virtual-machines"></a>SQL Server auf virtuellen Computern

  1. Überprüfen Sie die SQL Server auf diesem virtuellen Computer.
  2. Führen Sie die vorherigen Vorschläge für virtuelle Computer.
  3. Verwenden Sie SQL Server hohe Verfügbarkeit wie AlwaysOn.

###<a name="service-bus"></a>Servicebus

  1. Überprüfen des Service Bus-Abschnitts dieses Dokuments.
  2. Sollten Sie eine dauerhafte clientseitige Warteschlange als Sicherung erstellen.

###<a name="hdinsight"></a>HDInsight

  1. Überprüfen Sie im HDInsight-Abschnitt dieses Dokuments.
  2. Es sind keine zusätzliche Verfügbarkeit Schritte für lokale Fehler.

##<a name="next-steps"></a>Nächste Schritte

Dieser Artikel ist Teil einer Reihe [Azure Resiliency technische](./resiliency-technical-guidance.md)Anleitung. Im nächste Artikel dieser Reihe ist [eine flächendeckende serviceunterbrechung](./resiliency-technical-guidance-recovery-loss-azure-region.md).
