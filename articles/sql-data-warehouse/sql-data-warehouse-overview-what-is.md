<properties
   pageTitle="Was ist Azure SQL Data Warehouse? | Microsoft Azure"
   description="Unternehmen verteilte Verarbeitung Petabyte von relationalen und nicht relationalen Datenbank. Ist die branchenweit erste Cloud Datawarehouse mit vergrößern, verkleinern und in Sekunden angehalten."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# <a name="what-is-azure-sql-data-warehouse"></a>Was ist Azure SQL Data Warehouse?

Azure SQL Data Warehouse ist eine cloudbasierte, Scale-Out Datenbank verarbeiten große Mengen an relationalen und nicht relationalen Daten. Grundlage unserer parallele (MPP) Verarbeitungsarchitektur kann SQL Data Warehouse Enterprise-Arbeitslast zu bewältigen.

SQL Datawarehouse:

- Kombiniert die relationale SQL Server-Datenbank mit Azure Cloud skalieren. Sie vergrößern, verkleinern, Anhalten oder fortsetzen Compute in Sekunden. Sie sparen CPU bei Bedarf skalieren und Verwendung in nicht-Spitzenzeiten kürzen.
- Die Azure-Plattform nutzt. Einfache Bereitstellung, nahtlose verwaltet und vollständig fehlertolerant durch automatische Backups.
- Ergänzt das SQL Server-Ökosystem. Sie können mit vertrauten SQL Server Transact-SQL (T-SQL) und Tools entwickeln.

Dieser Artikel beschreibt die wichtigsten Features von SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>Parallele Verarbeitungsarchitektur

SQL Data Warehouse ist eine parallele Verarbeitung (MPP) Datenbank verteilt. Daten und Verarbeitungskapazität auf mehreren Knoten bieten SQL Data Warehouse große skalierbar - weit über jedes einzelne System.  Hinter den Kulissen verteilt SQL Data Warehouse Daten solch Speicher und Einheiten. Daten im Premium lokal redundanter Speicher gespeichert und verknüpft, um Knoten für die Ausführung der Abfrage berechnet. Mit dieser Architektur akzeptiert SQL Data Warehouse "Teile und herrsche" Ansatz lädt und komplexe Abfragen. Anfragen der Knoten erhalten, optimiert und dann ihre Arbeit parallel Computeknoten übergeben.

SQL Data Warehouse können MPP-Architektur und Azure Storage-Funktionen kombinieren:

- Vergrößert oder verkleinert Speicher unabhängig von Compute.
- Vergrößert oder verkleinert Compute ohne Verschieben von Daten.
- Pause Rechenkapazität dabei Daten intakt.
- Resume Rechenkapazität in einem Augenblick.

Die folgende Abbildung zeigt die Architektur genauer.

![SQL Data Warehouse-Architektur][1]


**Knoten:** Der Knoten verwaltet und optimiert die Abfragen. Es ist mit allen Applikationen und Verbindungen front-End. SQL Data Warehouse ist der Knoten SQL-Datenbank, und anschließen sieht identisch ist. Unter der Oberfläche koordiniert der Knoten alle Daten und Berechnung parallele Abfragen verteilte Daten erforderlich. Beim Senden einer T-SQL-Abfrage in SQL Data Warehouse transformiert der Knoten in separate Abfragen, die auf jeder Compute-Knoten parallel ausgeführt.

**Compute-Knoten:** Compute-Knoten dienen als Leistungsstärke SQL Data Warehouse. SQL-Datenbanken, die Daten speichern und verarbeiten die Abfrage sind. Beim Hinzufügen von Daten stellt SQL Data Warehouse Zeilen die Compute-Knoten. Compute-Knoten sind die parallele auf die Daten Abfragen. Nach der Verarbeitung übergeben der Ergebnisse an den Knoten. Um die Abfrage zu beenden, der Knoten aggregiert die Ergebnisse und gibt das Endergebnis zurück.

**Speicher:** Die Daten werden in Azure BLOB-Speicher gespeichert. Wenn Compute-Knoten mit den Daten interagieren sie schreiben und Lesen direkt auf BLOB-Speicher. Da Azure-Speicher transparent und deutlich erweitert, kann SQL Data Warehouse identisch. Da Computing- und unabhängig sind, kann SQL Data Warehouse Speicher getrennt von Skalierung Compute und umgekehrt automatisch skaliert. Azure BLOB-Speicher ist fehlertolerant und rationalisiert die Sicherung und Wiederherstellung.

**Datendienst Bewegung:** Daten verschieben Service (DMS) verschoben Daten zwischen den Knoten. DMS ermöglicht den Compute-Knoten Zugriff auf Daten für Joins und Aggregationen. DMS ist kein Azure Service. Es ist ein Windows-Dienst, der auf den Knoten neben SQL-Datenbank ausgeführt wird. Da DMS im Hintergrund ausgeführt wird, interagieren nicht Sie direkt mit ihm. Jedoch beim Betrachten von Abfrageplänen sehen Sie, dass sie einige DMS-Vorgänge enthalten, da Daten jede Abfrage parallel ausgeführt werden.


## <a name="optimized-for-data-warehouse-workloads"></a>Optimiert für Data Warehouse Arbeitslasten

MPP-Ansatz wird eine Anzahl von Data warehousing-spezifische Performance-Optimierungen, einschließlich unterstützt:

- Eine verteilte Abfrage Optimizer und komplexe Statistiken über alle Daten. Informationen über Größe und Verteilung verwenden, kann der Dienst Abfragen optimieren, indem Sie die Kosten für bestimmte verteilte Abfrageoperationen.

- Algorithmen und Techniken integriert Datentransfers effizient zwischen Computerressourcen für die Abfrage ggf. verschieben. Diese Bewegung Datenoperationen erstellt und automatisch alle Optimierungen an den Datendienst Bewegung.

- Gruppierte **Columnstore** Indizes standardmäßig. Mithilfe der Spalte-basierte ruft SQL Data Warehouse durchschnittlich 5 X Komprimierung Gewinne über herkömmliche zeilenorientierten Speicher und bis zu 10 X oder weitere Abfrage Leistungssteigerungen. Analytics-Abfragen, die eine große Anzahl von Zeilen arbeiten auf Columnstore Indizes überprüfen müssen.


## <a name="predictable-and-scalable-performance"></a>Prognostizierbare und skalierbare performance

SQL Data Warehouse trennt Speicher- und Compute, die jeweils unabhängig voneinander skalieren kann. SQL Data Warehouse können schnell und einfach skalieren, um zusätzliche Datenverarbeitungsressourcen zur Zeit hinzuzufügen. Diese Ergänzung ist die Verwendung von Azure BLOB-Speicher. BLOBs bieten nicht nur stabile replizierten Speicher, sondern auch die Infrastruktur für mühelose Erweiterung kostengünstig. Mit dieser Kombination Cloud skalieren und Azure Compute können SQL Data Warehouse Sie bezahlen für Leistung und Speicher bei Bedarf. Ändern von Compute ist so einfach wie eine Schiebereglers im Azure-Portal nach links oder rechts, oder es kann auch geplant werden mit T-SQL und PowerShell.

Sowie die Compute unabhängig Speicher vollständig steuern können mit SQL Data Warehouse das Datawarehouse bedeutet, dass Sie Compute bezahlen sollen nicht vollständig anzuhalten. Beibehaltung des Speichers an alle Compute in main Pool von Azure, sparen Sie freigegeben. Bei Bedarf einfach fortsetzen Sie Compute Daten Sie und berechnen Sie für Ihre Arbeitslast zu.

## <a name="data-warehouse-units"></a>Data Warehouse-Einheiten

Zuordnung von Ressourcen zu SQL Data Warehouse ist in Data Warehouse-Einheiten (DWUs) gemessen. DWUs sind ein Maß für die zugrunde liegenden Ressourcen wie CPU, Arbeitsspeicher, IOPS, die SQL Data Warehouse zugeordnet sind. Die Anzahl der DWUs erhöhen, Ressourcen und der Leistung. Insbesondere dazu DWUs:

- Sie können das Datawarehouse skalieren, ohne die zugrunde liegende Hardware oder Software.

- Sie können Leistungssteigerung für eine DWU-Ebene, bevor Sie die Größe des Datawarehouse ändern.

- Die zugrunde liegende Hardware und Software der Instanz ändern oder verschieben ohne die Arbeitslast.

- Microsoft kann die zugrunde liegende Architektur des Dienstes anpassen ohne die Leistung Ihrer Arbeit.

- Microsoft kann schnell im SQL Data Warehouse auf eine Weise verbessern, die skalierbar und gleichmäßig System.

Data Warehouse-Einheiten bieten eine drei präzise Metriken Datawarehousing-Arbeitslast Korrelation mit sind. Ziel ist es, dass die folgenden Schlüssel Arbeitslast Metriken linear mit der DWUs skaliert werden, die für das Datawarehouse gewählt haben.

**Scan-Aggregation:** Diese Metrik Arbeitslast hat einen standard Data warehousing-Abfrage, die eine große Anzahl von Zeilen durchsucht und führt dann eine komplexe Aggregation. Dies ist ein e/a und CPU-intensiver Vorgang.

**Laden:** Diese Metrik misst die Möglichkeit, Daten in den Dienst aufnehmen. Lasten werden mit PolyBase Laden eines repräsentativen Daten von Azure BLOB-Speicher abgeschlossen. Diese Metrik soll Stress Netzwerk- und CPU-Aspekte des Dienstes.

**Tabelle erstellen als auswählen (CTAS):** CTAS misst die Möglichkeit, eine Tabelle zu kopieren. Dies umfasst Lesen von Daten aus dem Speicher, auf die Knoten der Appliance verteilt und wieder in den Speicher zu schreiben. Es ist eine CPU, e/a und zeitaufwendig.

## <a name="pause-and-scale-on-demand"></a>Anhalten und Skalierung bei Bedarf

Wenn Sie schneller, erhöhen die DWUs und bezahlen für mehr Leistung. Beim benötigen weniger Strom zu berechnen, die DWUs verringern und nur bezahlen, was Sie brauchen. Möglicherweise denken Ihre DWUs in diesen Szenarien ändern:

- Wenn Du musst, vielleicht in Abfragen abends oder am Wochenende, stoppen die Abfragen ausführen. Halten Sie dann die Datenverarbeitungsressourcen zur Vermeidung von DWUs bezahlen, wenn Sie sie benötigen.

- Wenn Ihr System wenig verfügt, sollten Sie DWU auf eine kleine Größe verringern. Sie können weiterhin auf die Daten zugreifen aber erhebliche Kostensenkungen.

- Bei einem Vorgang großer Datenmengen laden oder Transformation möchten Sie skalieren, damit die Daten schneller verfügbar ist.

Wissen, was der ideale DWU-Wert ist, versuchen Skalierung nach oben und unten und einige Abfragen nach dem Laden der Daten ausführen. Da Skalierung schnell, probieren Sie eine Anzahl verschiedener Leistungsstufen innerhalb einer Stunde oder weniger.  Bedenken, dass SQL Data Warehouse große Datenmengen verarbeiten und echten-Funktionen für die Skalierung konzipiert ist, insbesondere bei größeren Maßstäben bieten wir wollen mit einer großen Datenmenge erreicht oder überschreitet 1 TB.


## <a name="built-on-sql-server"></a>In SQL Server erstellt

SQL Data Warehouse basiert auf das relationale SQL Server-Datenbankmodul und umfasst viele der Funktionen, die ein Enterprise Data Warehouse erwarten. Wenn schon T-SQL lässt Ihre Kenntnisse in SQL Data Warehouse übertragen. Ob Sie erweiterte oder gerade erst hilft die Beispiele in der Dokumentation zu beginnen. Insgesamt können über Ihre Meinung, dass wir wie folgt die Sprachelemente SQL Data Warehouse erstellt haben:

- SQL Data Warehouse verwendet T-SQL-Syntax für viele Vorgänge. Es unterstützt auch eine Reihe von herkömmlichen SQL-Konstrukte wie gespeicherte Prozeduren, benutzerdefinierte Funktionen Tabellenpartitionierung, Indizes und Sortierreihenfolgen.

- SQL Data Warehouse enthält auch einige neue SQL Server-Funktionen, einschließlich: gruppierte **Columnstore** Indizes, PolyBase Integration und Daten (komplett mit Bewertung) überwachen.

- Bestimmte T-SQL-Sprachelemente, die weniger häufig für Data warehousing-Arbeitslasten oder zu SQL Server möglicherweise nicht verfügbar. Weitere Informationen finden Sie in der [Migrationsdokumentation][].

Mit der Transact-SQL und Funktion Gemeinsamkeit zwischen SQL Server, SQL Data Warehouse SQL-Datenbank und Analytics Plattform-System können Sie eine Lösung entwickeln, die Ihren Bedürfnissen entspricht. Sie können entscheiden, wo Ihre Daten basierend auf Leistung, Sicherheit und Skalierung Vorschriften und Daten nach Bedarf zwischen verschiedenen Systemen übertragen.

## <a name="data-protection"></a>Datenschutz

SQL Data Warehouse speichert alle Daten in Azure Premium lokal redundanter Speicher. Synchrone Kopien der Daten werden im lokalen Rechenzentrum Transparenz Schutz bei lokalisierten Fehler verwaltet. Darüber hinaus sichert SQL Data Warehouse automatisch die aktiven (nicht angehaltenen) Datenbanken in regelmäßigen Abständen mit Azure Storage Snapshots. Mehr über das Sichern und Wiederherstellen von Works finden Sie unter [Übersicht über die Sicherung und Wiederherstellung][].

## <a name="integrated-with-microsoft-tools"></a>Integriert mit Microsoft-tools

SQL Data Warehouse lässt sich auf viele Tools, SQL Server-Benutzer mit vertraut sein. Dazu gehören:

**Herkömmlichen SQL Server-Tools:** SQL Data Warehouse ist in SQL Server Analysis Services, Integration Services und Reporting Services vollständig integriert.

**Cloud-basierte Tools:** SQL Data Warehouse können mit neuen Tools in Azure Data Factory, Stream Analytics maschinelles lernen und Power BI einschließlich verwendet werden. Eine vollständige Liste finden Sie unter [Übersicht über die integrierten Tools][].

**Von Drittanbietertools:** Eine große Anzahl von Drittanbieter-Tool-Anbieter haben Integration Tools mit SQL Data Warehouse zertifiziert. Eine vollständige Liste finden Sie unter [SQL Data Warehouse Solution Partner][].

## <a name="hybrid-data-sources-scenarios"></a>Hybrid-Datenquellen Szenarien

PolyBase mit SQL Data Warehouse kann Benutzer eine enorme Verschieben von Daten zwischen ihrem Ökosystem entsperren können erweiterte Hybrid-Szenarios mit nicht-relationalen einrichten und lokalen Datenquellen.

Polybase können Sie Ihre Daten aus verschiedenen Quellen nutzen bekannte T-SQL-Befehle. Polybase können Sie Abfrage nicht relationalen Daten in Azure BLOB-Speicher als wäre es eine normale Tabelle. Verwenden Sie Polybase nicht relationale Daten und nicht-relationalen Daten in SQL Data Warehouse importieren.

- PolyBase verwendet nicht-relationalen Daten Zugriff auf externe Tabellen. Die Tabellendefinitionen im SQL Data Warehouse gespeichert werden und Sie sie mithilfe von SQL und wie Sie würde Zugriff auf normalen relationalen Daten.

- Polybase ist die Integration unabhängig. Macht die gleichen Merkmale und Funktionen für alle unterstützten Datenquellen. Polybase gelesenen Daten können eine Vielzahl von Formaten, einschließlich getrennte Dateien oder ORK.

- PolyBase kann verwendet werden, um BLOB-Speicher zugreifen, die auch als Speicher für HDInsight-Cluster verwendet wird. Dadurch erhalten Sie Zugriff auf die gleichen Daten mit relationalen und nicht relationalen.

## <a name="sla"></a>SLA

SQL Data Warehouse bietet eine Produkt auf Vereinbarung zum Servicelevel (SLA) als Teil des Microsoft Online Services-SLA. Weitere Informationen finden Sie in der [SLA für SQL Data Warehouse][]. SLA-Informationen zu anderen Produkten finden Sie [Service Level Agreements] Azure Seite oder auf der [Volume Licensing][] -Seite herunterladen. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem ein wenig wissen über SQL Data Warehouse erfahren wie schnell [ein SQL Data Warehouse erstellen][] und [Laden von Beispieldaten][]. Wenn Sie in Azure sind, kann hilfreich [Azure Glossar][] auf neue Begriffe. Oder sehen Sie sich einige dieser anderen SQL Data Warehouse-Ressourcen.  

- [Erfahrungsberichte von Kunden]
- [Blogs]
- [Wünsche]
- [Videos]
- [Customer Advisory-Teamblogs]
- [Support-Ticket erstellen]
- [MSDN-forum]
- [Stack-Überlauf-forum]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Support-Ticket erstellen]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Laden von Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[Erstellen Sie ein SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migrationsdokumentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse Solution Partner]: ./sql-data-warehouse-partner-business-intelligence.md
[Übersicht über die integrierten tools]: ./sql-data-warehouse-overview-integrate.md
[Sicherung und Wiederherstellung (Übersicht)]: ./sql-data-warehouse-restore-database-overview.md
[Azure-Glossar]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Erfahrungsberichte von Kunden]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory-Teamblogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Wünsche]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack-Überlauf-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA für SQL Datawarehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volumenlizenzierung]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/
