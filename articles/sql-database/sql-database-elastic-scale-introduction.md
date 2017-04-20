<properties
    pageTitle="Skalieren mit Azure SQL-Datenbank | Microsoft Azure"
    description="Software als Service (SaaS)-Entwickler kann leicht flexible, skalierbare Datenbanken in der Cloud mit diesen Tools erstellen."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Dezentrales Skalieren mit Azure SQL-Datenbank

SQL Azure-Datenbanken mit **Elastische** Datenbanktools können problemlos skalieren. Diese Tools und Funktionen können Sie praktisch unbegrenzte Datenbankressourcen **Azure SQL** -Datenbank Lösungen für transaktionale Workloads und insbesondere Software als Service (SaaS) Anwendung. Elastische Datenbankfunktionen bestehen aus den folgenden:

* [Elastische Datenbank-Clientbibliothek](sql-database-elastic-database-client-library.md): die Clientbibliothek ist ein Feature, mit dem Sie erstellen und Verwalten von Datenbanken Sharding.  [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md)anzeigen
* [Elastische Datenbank Split - Dienstprogramm](sql-database-elastic-scale-overview-split-and-merge.md): Verschiebt Daten zwischen Datenbanken Sharding. Dies ist nützlich zum Verschieben von Daten aus einer Datenbank Multi-Tenant Single-Tenant-Datenbank (oder umgekehrt). [Elastische Datenbank Split-Merge Tool Lernprogramm](sql-database-elastic-scale-configure-deploy-split-and-merge.md)anzeigen
* [Elastische Datenbank Aufträge](sql-database-elastic-jobs-overview.md) (Vorschau): Aufträge zu große Anzahl von Azure SQL-Datenbanken verwenden. Durchführen Sie administrative Vorgänge oder geändert, Verwaltung von Anmeldeinformationen, Verweis Datenupdates, Leistungsdaten Mieter (Kunde) Telemetrie Auflistung mit leicht.
* [Elastische Abfrage](sql-database-elastic-query-overview.md) (Vorschau): ermöglicht es Ihnen, eine Transact-SQL-Abfrage ausführen, die mehrere Datenbanken umfasst. Dies ermöglicht die Verbindung zum reporting-Tools wie Excel, PowerBI, Tableaus.
* [Elastische Transaktionen](sql-database-elastic-transactions-overview.md): Dadurch Transaktionen ausführen, die mehrere Datenbanken in Azure SQL-Datenbank umfassen. Elastische Datenbanktransaktionen stehen für .NET Applications mit ADO .NET und Programmierung vertraut mit [System.Transaction-Klassen](https://msdn.microsoft.com/library/system.transactions.aspx)integriert.

Die folgende Grafik zeigt eine Architektur mit **elastischen Datenbankfunktionen** in Bezug auf mehrere Datenbanken.

In dieser Abbildung stellen die Farben der Datenbank Schemas. Datenbanken mit derselben Farbe nutzen dasselbe Schema.

1. Eine Reihe von **SQL Azure-Datenbanken** auf Sharding Architektur Azure gehostet.
2. Die **Clientbibliothek elastische Datenbank** zum Splitter Gruppe verwalten.
3. Eine Teilmenge der Datenbanken werden in einem **Pool für elastische Datenbanken**eingefügt. (Siehe [Was ist ein Pool?](sql-database-elastic-pool.md)).
4. Ein **Auftrag elastische Datenbank** führt geplante oder Ad-hoc-T-SQL-Skripts für alle Datenbanken.
5. **Split - Dienstprogramm** wird verwendet, um Daten zwischen einem Splitter verschieben.
6. **Elastische Abfrage** können Sie eine Abfrage schreiben, die alle Datenbanken in den Splitter umfasst.
7. **Elastische Transaktionen** können Sie Transaktionen ausführen, die mehrere Datenbanken umfassen. 


![Elastische Datenbanktools][1]


## <a name="why-use-the-tools"></a>Warum verwenden die?

Erreichen Elastizität und Skalierung Cloudanwendungen wurde für VMs und BLOB-Speicher einfach hinzufügen oder Einheiten entfernen oder macht. Es ist jedoch eine Herausforderung für statusbehaftete Datenverarbeitung in relationalen Datenbanken. Probleme entwickelt in diesen Szenarien:

* Vergrößern und verkleinern Kapazität für relationale Teil Ihrer Arbeit.
* Verwalten von Hotspots, die auf einer bestimmten Teilmenge von Daten wie eine besonders Endkunden (Mandant) auftreten können.

Bisher wurden Szenarien wie diese Investitionen in größere Datenbankserver zur Unterstützung der Anwendung behandelt. Diese Option ist in der Cloud jedoch, die gesamte Verarbeitung auf vordefinierte Standardhardware geschieht. Stattdessen stellt (ein dezentrales Skalieren Muster "Sharding" genannt) Verteilen von Daten und Verarbeitung über viele Datenbanken identisch strukturierte Alternative zu herkömmlichen skalieren Kosten und Elastizität.

## <a name="horizontal-and-vertical-scaling"></a>Horizontale und vertikale Skalierung

Folgende Abbildung zeigt die horizontale und vertikale Skalierung, die grundlegenden Arten, die elastischen Datenbanken skaliert werden können.

![Horizontale und vertikale Scaleout][2]

Horizontale Skalierung verweist auf Hinzufügen oder Entfernen von Datenbanken um Speicherkapazität oder allgemeine Leistung anpassen. Dies steht "Skalierung". Sharding, in der Daten über mehrere identisch strukturierte Datenbanken partitioniert ist ist ein gängiges Verfahren zum horizontalen Skalierung zu implementieren.  

Vertikale Skalierung erhöhen oder verringern die Leistungsfähigkeit einer einzelnen Datenbank bezeichnet – dies wird auch bezeichnet als "Aufwärtsskalieren."

Meisten Cloud angelegte Datenbank verwendet eine Kombination aus diesen beiden Strategien. Beispielsweise können eine Software als Service-Anwendung horizontale Skalierung neue Kunden bereitstellen und vertikale Skalierung jede Endkunden Datenbank vergrößert oder verkleinert die Arbeitslast benötigt Ressourcen.

* Horizontale Skalierung wird die [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md)verwaltet.

* Vertikale Skalierung erfolgt mithilfe von Azure PowerShell-Cmdlets die Dienstebene ändern oder Datenbanken im elastischen Pool.

## <a name="sharding"></a>Sharding

*Sharding* ist eine Technik, um große Datenmengen identisch strukturierte Anzahl unabhängiger Datenbanken verteilt. Es ist besonders mit Cloud-Entwickler Erstellen von Software als Service (SAAS) Angebote für Kunden und Unternehmen. Diese Kunden werden häufig als "Mieter" bezeichnet. Sharding kann für verschiedene Gründe erforderlich sein:  

* Die Gesamtmenge der Daten ist zu groß für im Rahmen einer einzelnen Datenbank
* Der Transaktionsdurchsatz der gesamten Arbeitslast übertrifft die Funktionalität einer einzelnen Datenbank
* Mieter können erfordern physische Isolation voneinander getrennte Datenbanken für jeden Mandanten erforderlich sind
* Verschiedene Abschnitte einer Datenbank müssen in verschiedenen Regionen Compliance, Leistung oder geopolitischen Gründen befinden.

In anderen Szenarien, z. B. Erfassung von Daten aus verteilten Geräte kann Sharding müssen zeitlich organisiert sind eine Reihe von Datenbanken verwendet werden. Beispielsweise kann eine separate Datenbank pro Tag oder Woche gewidmet. In diesem Fall die shardingschlüssel kann eine ganze Zahl, die das Datum (alle Zeilen Sharding Tabellen vorhanden) und Abfragen Abrufen von Informationen für einen Zeitraum von der Anwendung für die Teilmenge der Datenbanken des betreffenden Bereichs weitergeleitet werden müssen.

Sharding funktioniert am besten bei jeder Transaktion in einer Anwendung auf einen einzelnen Wert ein shardingschlüssel beschränkt werden kann. Das gewährleistet, dass alle Transaktionen für eine bestimmte Datenbank lokal werden.

## <a name="multi-tenant-and-single-tenant"></a>Multi-Tenant und einzelne Mieter

Einige Programme verwenden Sie am einfachsten erstellen Sie eine separate Datenbank für jeden Mandanten. Dies ist die **einzelne Sharding Muster** , die Isolierung, Backup-und Recovery-Fähigkeit und Ressourcen bei der Granularität des Mandanten. Mit einzelne Sharding jede Datenbank ist mit bestimmten Mandanten-ID-Wert (oder Schlüsselwert Kunden), aber diesen Schlüssel muss immer nicht in den Daten selbst. Die Anwendung muss jede Anforderung an die entsprechende Datenbank-weiterleiten und Clientbibliothek kann diesen Vorgang vereinfachen.

![Einzelne Mieter und Multi-tenant][4]

Andere Szenarien pack mehrere Mandanten zusammen in Datenbanken, sondern in separaten Datenbanken isolieren. Dies ist ein Standard **Multi-Tenant-Sharding Muster** – und kann durch die Tatsache, dass eine Anwendung viele kleine Mieter verwaltet gesteuert werden. Im Multi-Tenant-Sharding Zeilen in Datenbanktabellen sollen einen Schlüssel, der die Mandanten-ID identifiziert oder shardingschlüssel. Wiederum die Anwendungsebene ist verantwortlich für das routing des Mieters Anforderung zur entsprechenden Datenbank und kann diese von der Clientbibliothek elastische Datenbank unterstützt. Darüber hinaus dienen zeilenbasierter Sicherheit Filter die Zeilen jeder – Weitere Informationen zugreifen kann finden Sie unter [Multi-Tenant-Applikationen elastische Datenbanktools mit Sicherheit auf Zeilenebene](sql-database-elastic-tools-multi-tenant-row-level-security.md). Verteilen von Daten zwischen Datenbanken ggf. mit der Multi-Tenant-Sharding, und dies von der geteilten Zusammenführungstool elastische Datenbank. Erfahren Sie mehr über Entwurfsmuster für SaaS-Applikationen mit elastischen Pools finden Sie unter [Entwurfsmuster für Multi-Tenant SaaS mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Verschieben von Daten aus mehreren einzelnen Mandanten Datenbanken

Beim Erstellen einer SaaS-Anwendung ist es üblich, potenziellen Kunden eine Testversion der Software. In diesem Fall ist die kostengünstige Multi-Tenant-Datenbank für die Daten verwenden. Allerdings wird ein Interessent einen Kunden, ist eine Datenbank mit einem Mandanten besser da bessere Leistung. Verwenden Sie der Kunde während des Testzeitraums Daten erstellt haben, [Teilen Zusammenführungstool](sql-database-elastic-scale-overview-split-and-merge.md) Daten aus mehreren Mandanten in die neue Single-Tenant-Datenbank verschieben.

## <a name="next-steps"></a>Nächste Schritte

Eine Beispiel-app, die Clientbibliothek veranschaulicht, finden Sie unter [Erste Schritte mit elastischen Datababase](sql-database-elastic-scale-get-started.md).

Konvertieren von Datenbanken, verwenden Sie die Tools finden Sie unter [Migrieren bestehender Datenbanken nach Dezentrales Skalieren](sql-database-elastic-convert-to-use-elastic-tools.md).

Die Einzelheiten des Pools elastische Datenbank finden finden Sie [Preis- und Hinweise für einen Datenbankpool elastische](sql-database-elastic-pool-guidance.md)oder erstellen Sie einen neuen Pool mit dem [Lernprogramm](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

