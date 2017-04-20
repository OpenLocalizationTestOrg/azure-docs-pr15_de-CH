<properties
   pageTitle="Data Warehouse-Workloads"
   description="Elastizität SQL Data Warehouse können Sie vergrößern, verkleinern oder Compute macht eine gleitende Skalierung des Data Warehouse-Einheiten (DWUs) mit anhalten. Dieser Artikel beschreibt Data Warehouse Metriken und wie sie auf DWUs. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Data Warehouse-Workloads
Data Warehouse-Workloads bezieht sich auf alle Vorgänge, die für ein Datawarehouse ablaufen müssen. Data Warehouse-Workloads umfasst den gesamten Prozess der Daten in das Warehouse geladen, Analyse und Berichterstattung auf das Datawarehouse verwalten Daten in das Datawarehouse und Exportieren von Daten aus dem Datawarehouse. Die Tiefe und Breite dieser Komponenten entsprechen häufig den Reifegrad des Datawarehouse.


## <a name="new-to-data-warehousing"></a>Neue Datawarehousing?
Ein Datawarehouse ist eine Sammlung von Daten, die aus einer oder mehreren Datenquellen geladen werden und Business Intelligence Aufgaben wie reporting und Analyse ausführen.

Datawarehouses Zeichnen von Abfragen, die größere Anzahl von Zeilen, große Datenbereiche Scannen und möglicherweise relativ große Ergebnisse zwecks Analyse und Berichterstattung. Datawarehouses zeichnen auch relativ große Datenmengen und kleine Transaktionsebene fügt/Updates/löscht.

- Ein Datawarehouse führt am besten, wenn die Daten gespeichert werden, die beim Optimieren von Abfragen, die viele Zeilen oder große Datenbereiche scannen. Dies funktioniert am besten, wenn die Daten gespeichert und nach Spalten anstelle von Zeilen durchsucht.

>[AZURE.NOTE] Index im Speicher Columnstore Speicherung verwendet, bietet bis zu 10 x Komprimierung Gewinne und 100 x Leistungssteigerungen Abfrage über herkömmliche binären Strukturen reporting und Analysen Abfragen. Wir betrachten Columnstore Indizes als Standard zum Speichern und Scannen von großen Datenmengen in einem Datawarehouse.

- Ein Datawarehouse hat andere Anforderungen als ein System online Transaction processing (OLTP) optimiert. OLTP-System ist einfügen, aktualisieren und löschen. Diese Suchvorgänge auf bestimmte Zeilen in der Tabelle. Tabelle soll am besten durchführen, wenn die Daten auf den Zeilen gespeichert werden. Daten können sortiert werden schnell mit einer Division gesucht und erobern Ansatz eine binäre Struktur oder Btree Suche.


## <a name="data-loading"></a>Laden von Daten
Laden von Daten ist ein wichtiger Teil des Data Warehouse-Workloads. Unternehmen haben normalerweise ein beschäftigt OLTP-System, das ändert sich im Laufe des Tages verfolgt beim Kunden Geschäftstransaktionen generieren. Oft nachts ein Wartungsfenster, Transaktionen regelmäßig verschoben oder in das Datawarehouse kopiert. Sobald in das Datawarehouse Daten können Analysten analysieren und Entscheidungen für die Daten.

- Traditionell wird das Laden von ETL für extrahieren, Transformieren und Laden bezeichnet. Daten in der Regel mit anderen Daten in das Datawarehouse wird transformiert werden. Zuvor, zum Unternehmen dedizierte ETL-Server die Transformationen durchführen. Jetzt mit fast parallele Verarbeitung können Daten zunächst in SQL Data Warehouse geladen, und führen Sie dann die Transformationen. Dabei heißt extrahieren, laden und Transformieren (ELT) und wird einen neuen Standard für die Data Warehouse-Workloads.

> [AZURE.NOTE] SQL Server 2016 jetzt möglich, Analysen in Echtzeit auf einer OLTP-Tabelle. Dies ersetzt nicht die Notwendigkeit für ein Datawarehouse zum Speichern und Analysieren von Daten, aber es bietet die Möglichkeit zur Analyse in Echtzeit.

### <a name="reporting-and-analysis-queries"></a>Berichts- und Abfragen
Berichts- und Abfragen häufig in kleinen, mittleren kategorisiert und große basierend auf einer Reihe von Kriterien, aber normalerweise basierend auf Uhrzeit. In den meisten Datawarehouses ist eine gemischte Arbeitslast schnell ausgeführt und Abfragen. In jedem Fall ist wichtig, diese Mischung zu ermitteln und die Häufigkeit (stündlich, täglich, Monatsende Quartalsende usw.). Es ist wichtig zu verstehen, dass die Arbeitslast gemischte Abfrage gekoppelt mit der Parallelität zu richtigen Kapazität für ein Datawarehouse planen.

- Planung kann eine komplexe Aufgabe für eine gemischte Abfrage Arbeitslast besonders wenn eine lange Lieferzeit Datawarehouse Kapazität hinzufügen. SQL Data Warehouse entfernt die Dringlichkeit der Planung wachsen und schrumpfen Rechenkapazität jederzeit und Speicher und Rechenkapazität unabhängig angepasst.

### <a name="data-management"></a>Datenmanagement
Verwalten von Daten ist wichtig, besonders wenn Sie wissen, Speicherplatz in naher Zukunft ausführen kann. Normalerweise teilen Datawarehouses Daten in aussagekräftige Bereiche, die als Partitionen in einer Tabelle gespeichert werden. Alle SQL Server-basierte Produkte können Sie Partitionen in der Tabelle verschieben. Diese Partition wechseln können ältere Daten auf kostengünstigeren Speicher verschieben und die neuesten Daten im Speicher beibehalten.

- Columnstore Indizes unterstützen partitionierte Tabellen. Columnstore Indizes werden partitionierte Tabellen für Daten-Management und Archivierung verwendet. Für Tabellen Zeile für Zeile müssen Partitionen eine größere Rolle Abfrage-Leistung.  

- PolyBase spielt eine wichtige Rolle bei der Verwaltung von Daten. PolyBase verwenden, können Sie ältere Daten Hadoop oder Azure Blob-Speicher.  Dies bietet viele Optionen, da die Daten noch online ist.  Dauern länger aus Hadoop, aber Kompromiss Abruf kann die Speicherkosten überwiegen.

### <a name="exporting-data"></a>Exportieren von Daten
Eine Möglichkeit, Daten für Berichte und Analysen zur Verfügung stellen wird Daten aus dem Datawarehouse Server für das Ausführen von Berichten und Analysen an. Diese Server werden Datamarts bezeichnet. Sie können z. B. Daten vorverarbeitet und dann aus dem Datawarehouse viele Server weltweit für Kunden und Analysten verfügbar machen exportieren.

- Berichte zu erstellen, können Sie jede Nacht schreibgeschützte Berichtsserver mit einer Momentaufnahme der täglichen Daten auffüllen. Dadurch höheren Bandbreite für Kunden und zugleich Compute Ressourcenbedarfs für das Datawarehouse. Ein Aspekt Sicherheit können Datamarts verringern die Anzahl der Benutzer mit Zugriff auf das Datawarehouse.
- Für Analysen können entweder Analysis-Cubes im Data Warehouse zu erstellen und das Datawarehouse Analyse ausführen oder Daten vorverarbeitet und zum Analysis-Server für weitere Analysen exportieren.

## <a name="next-steps"></a>Nächste Schritte
Nachdem ein wenig wissen über SQL Data Warehouse erfahren wie schnell [ein SQL Data Warehouse erstellen][] und [Laden von Beispieldaten][].

<!--Image references-->

<!--Article references-->
[Laden von Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[Erstellen Sie ein SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
