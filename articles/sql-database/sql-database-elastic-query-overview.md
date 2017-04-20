<properties
    pageTitle="Azure SQL-Datenbank elastische Datenbank Abfrage Übersicht | Microsoft Azure"
    description="Übersicht über die elastische Abfragefunktion"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL-Datenbank elastische Datenbank Abfrage Übersicht (Vorschau)

Die elastische Datenbank Abfrage (in der Vorschau) ermöglicht eine Transact-SQL-Abfrage ausführen, die mehrere Datenbanken in Azure SQL Datenbank (SQLDB) umfasst. Dadurch werden datenbankübergreifenden Abfragen auf Remotetabellen zugreifen und Verbindung von Microsoft und Drittanbietern Tools (Excel PowerBI Tableaus, etc.) auf Datenebene mit mehreren Datenbanken abgefragt. Dieses Feature können Sie Abfragen auf große Datenebenen in SQL-Datenbank skalieren und Ergebnisse in Business Intelligence (BI) Berichten darstellen.

## <a name="documentation"></a>Dokumentation

* [Erste Schritte mit datenbankübergreifenden Abfragen](sql-database-elastic-query-getting-started-vertical.md)
* [Bericht über skalierte Cloud-Datenbanken](sql-database-elastic-query-getting-started.md)
* [Sharding Clouddatenbanken (horizontal partitioniert) Abfragen](sql-database-elastic-query-horizontal-partitioning.md)
* [Cloud-Datenbanken mit unterschiedlichen Schemas (vertikal partitioniert) Abfragen](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Warum verwendet elastische Abfragen?

**SQL Azure-Datenbank**

SQL Azure-Datenbanken in T-SQL-Abfragen. Dadurch wird für schreibgeschützte Abfragen von entfernten Datenbanken. Dies ermöglicht das lokale SQL Server-Kunden drei und vierteiligen Namen oder Verbindungsserver SQL DB Anwendung migrieren.

**Auf standard-Stufe** Elastische Abfrage wird auf der Ebene der Standard Performance zusätzlich zu Premium-Performance Tier unterstützt. Abschnitt Einschränkungen unter Vorschau auf Leistungseinbrüche für niedrigere Performance-Stufen.

**Drücken Sie auf entfernte Datenbanken**

Elastische Abfragen können SQL-Parameter an die entfernten Datenbanken zur Ausführung "pushen".

**Gespeicherte Prozedur**

Aufrufe remote gespeicherter Prozeduren oder remote mit Funktionen ausführen [sp\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714).

**Flexibilität**

Externe Tabellen mit elastische Abfrage können jetzt entfernten Tabellen mit einem anderen Schema oder Tabellennamen verweisen.

## <a name="elastic-database-query-scenarios"></a>Elastische Abfrageszenarien

Das Ziel ist zu Abfragen Szenarien, wo Datenbanken Zeilen in einer einzigen Gesamtergebnis beitragen. Die Abfrage kann entweder vom Benutzer oder der Anwendung direkt oder indirekt über Tools, die mit der Datenbank verbunden bestehen. Dies ist besonders beim Erstellen von Berichten mit professionellen BI oder Data Integration – oder einer Anwendung geändert werden kann. Sie können mit einer elastischen Abfrage über mehrere Datenbanken in der SQL Server-Konnektivität vertraut in Tools wie Excel, PowerBI, Tableaus oder Cognos Abfragen.
Elastische Abfrage ermöglicht einfachen Zugriff auf eine ganze Auflistung von Datenbanken durch Abfragen von SQL Server Management Studio oder Visual Studio und datenbankübergreifenden Abfragen von Entity Framework oder anderen ORM-Umgebung erleichtert. Abbildung 1 zeigt ein Szenario, in dem eine vorhandenen Cloudanwendung (der [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md)verwendet) basiert auf der Datenebene skalierte und elastische Abfrage für datenbankübergreifende reporting verwendet.

**Abbildung 1** Elastische Abfrage auf Datenebene skalierte verwendet

![Elastische Abfrage auf Datenebene skalierte verwendet][1]

Kundenszenarien für elastische Abfrage zeichnen sich durch die folgenden Topologien:

* **Vertikale Partitionierung – datenbankübergreifenden Abfragen** (Topologie 1): Datenpartitionierung vertikal zwischen mehreren Datenbanken in eine Datenebene. Normalerweise befinden sich verschiedene Tabellen in verschiedenen Datenbanken. Das bedeutet, dass das Schema auf andere Datenbanken. Beispielsweise sind alle Tabellen für den Bestand in einer Datenbank alle Accounting-bezogene Tabellen in einer zweiten Datenbank. Einsatzbereiche mit dieser Topologie erforderlich für Abfragen oder Berichte über Tabellen in Datenbanken erstellen.
* **Horizontale Partitionierung – Sharding** (Topologie 2): Datenpartitionierung horizontalen Zeilen auf einer skalierten Daten verteilt Tier. Dadurch wird das Schema auf alle teilnehmenden Datenbanken identisch. Dieser Ansatz wird auch "Sharding" bezeichnet. Sharding erfolgen und verwalteten (1) die elastische Datenbank mit tools, Bibliotheken oder (2) selbst-Sharding. Elastische Abfrage zum Abfragen oder Berichte über viele Splitter erstellen.

> [AZURE.NOTE] Elastische Abfrage funktioniert am besten für gelegentliche reporting-Szenarien, in denen die Verarbeitung zum größten Teil auf der Datenebene ausgeführt werden können. Für hohe Arbeitslasten reporting oder Lagerung Szenarien mit komplexeren Abfragen auch erwägen Sie [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Elastische Datenbank Abfrage Topologien

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topologie 1: Vertikale Partitionierung – datenbankübergreifenden Abfragen

Zunächst Codierung finden Sie unter [Erste Schritte mit Cross - Abfrage (vertikale Partitionierung)](sql-database-elastic-query-getting-started-vertical.md).

Elastische Abfrage kann zu Daten in einer Datenbank SQLDB andere SQLDB Datenbanken verwendet werden. Dadurch können Abfragen aus einer Datenbank zu Tabellen in anderen remote SQLDB Datenbank verweisen. Der erste Schritt ist eine externe Datenquelle für jede entfernte Datenbank definieren. Die externe Datenquelle wird in der lokalen Datenbank definiert auf Tabellen in der entfernten Datenbank zugreifen soll. Es sind keine Änderungen in der entfernten Datenbank erforderlich. Für normale vertikale Partitionierung Szenarios, in denen verschiedene Datenbanken andere Schemas, elastische Abfragen dienen Einsatzbereiche wie Daten implementieren und datenbankübergreifenden Abfragen.

**Referenzdaten**: Topologie 1 Datenmanagements Verweis verwendet. In der folgenden Abbildung werden zwei Tabellen (T1 und T2) mit Daten in einer dedizierten Datenbank gespeichert. Eine elastische Abfrage können Sie jetzt Tabellen T1 und T2 aus anderen Datenbanken Remotezugriff wie in der Abbildung dargestellt. Mit Topologie Tabellen sind kleine oder entfernte Abfragen in Tabelle 1 sind selektive Prädikaten.

**Abbildung 2** Vertikale Partitionierung - elastische Abfrage Abfrage Daten verwenden

![Vertikale Partitionierung - elastische Abfrage Abfrage Daten verwenden][3]

**Datenbankübergreifende Abfragen**: elastische Abfragen aktivieren Anwendungsfälle, die Abfragen über SQLDB Datenbanken benötigen. Abbildung 3 zeigt vier verschiedene Datenbanken: CRM, Lager, HR und Produkte. Abfragen der Datenbanken durchgeführt benötigen außerdem Zugriff auf eine oder alle anderen Datenbanken. Eine elastische Abfrage können Sie die Datenbank für diesen Fall mit ein paar einfachen DDL-Anweisungen auf jede der vier Datenbanken konfigurieren. Nach diesem einmaligen Konfiguration ist auf eine Remotetabelle so einfach wie auf eine lokale Tabelle T-SQL-Abfragen oder BI-Tools. Dieser Ansatz wird empfohlen, wenn die Remoteabfragen große Ergebnisse zurückgegeben werden.

**Abbildung 3** Vertikale Partitionierung - elastischen Abfrage Abfrage über verschiedene Datenbanken verwenden

![Vertikale Partitionierung - elastischen Abfrage Abfrage über verschiedene Datenbanken verwenden][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologie 2: Horizontale Partitionierung – Sharding

Elastische Abfrage muss reporting über Sharding, d. h., horizontal partitioniert Aufgaben Datenebene eine [elastische Datenbank shardzuordnung](sql-database-elastic-scale-shard-map-management.md) Datenbanken von der Datenebene darstellen. In der Regel nur einen einzigen shardzuordnung in diesem Szenario verwendet und eine dedizierte Datenbank elastische Abfragefunktionen dient als Einstiegspunkt für Berichtsabfragen. Dieser dedizierte Datenbank benötigt Zugriff auf die shardzuordnung. Abbildung 4 veranschaulicht diese Topologie und die Konfiguration mit der Datenbank und Splitter elastische Abfrage. Azure SQL-Datenbank Version oder Edition können Datenbanken auf der Datenebene sein. Weitere Informationen über die Clientbibliothek elastische Datenbank und Splitter Karten erstellen finden Sie unter [Splitter Management zuordnen](sql-database-elastic-scale-shard-map-management.md).

**Abbildung 4** Horizontale Partitionierung - elastische Abfrage für Berichte über Sharding Datenebenen verwenden

![Horizontale Partitionierung - elastische Abfrage für Berichte über Sharding Datenebenen verwenden][5]

> [AZURE.NOTE] Die dedizierte elastische Abfragedatenbank muss einer v12 SQL DB. Gibt es keine Einschränkung für die Splitter selbst.

Zunächst Codierung finden Sie unter [Erste Schritte mit elastische Abfrage für horizontale Partitionierung (Sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Implementieren flexible Abfragen

Die Schritte zum Implementieren elastische Abfrage für die vertikalen und horizontalen Partitionierung Szenarien werden in den folgenden Abschnitten erläutert. Sie finden Sie ausführliche Dokumentation Partitionierung Szenarien.

### <a name="vertical-partitioning---cross-database-queries"></a>Vertikale Partitionierung - datenbankübergreifenden Abfragen

Die folgenden Schritte konfigurieren elastische Abfragen für vertikale Partitionierung Szenarien, die auf eine Tabelle auf Remotedatenbanken SQLDB mit demselben Schema zugreifen:

*    [HAUPTSCHLÜSSEL erstellen](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Erstellen Datenbank begrenzt Anmeldeinformationen](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [CREATE/DROP externe Datenquelle](https://msdn.microsoft.com/library/dn935022.aspx) "MyDatasource" vom Typ **RDBMS**
*    [Externe CREATE/DROP TABLE](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Nach dem Ausführen von DDL-Anweisungen, können Sie die entfernte Tabelle "Mytable" zugreifen, als handele es sich um eine lokale Tabelle. Azure SQL-Datenbank automatisch öffnet eine Verbindung zur entfernten Datenbank bearbeitet Ihre Anfrage in der entfernten Datenbank und gibt die Ergebnisse zurück.
Weitere Informationen über die Schritte für die vertikale Partitionierung Szenario finden im [elastischen Abfrage für die vertikale Partitionierung](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Horizontale Partitionierung - Sharding

Die folgenden Schritte konfigurieren elastische Abfragen für horizontale Partitionierung Szenarien, die Zugriff auf eine Tabelle, die sich auf (normalerweise) remote SQLDB Datenbanken:

*    [HAUPTSCHLÜSSEL erstellen](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Erstellen Datenbank begrenzt Anmeldeinformationen](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Erstellen einer [shardzuordnung](sql-database-elastic-scale-shard-map-management.md) die Datenebene mit die Clientbibliothek elastische Datenbank darstellt.   
*    [CREATE/DROP externe Datenquelle](https://msdn.microsoft.com/library/dn935022.aspx) "MyDatasource" vom Typ **SHARD_MAP_MANAGER**
*    [Externe CREATE/DROP TABLE](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Nachdem Sie diese Schritte ausgeführt haben, können Sie horizontal partitionierte Tabelle "Mytable" zugreifen, als handele es sich um eine lokale Tabelle. Azure SQL-Datenbank automatisch mehrere parallele Verbindungen zu entfernten Datenbanken, Tabellen physisch gespeichert sind, verarbeitet die Clientanforderungen auf die entfernten Datenbanken und gibt die Ergebnisse zurück.
Weitere Informationen über die Schritte zur horizontalen Partitionierung Szenario finden im [elastischen Abfrage für horizontale Partitionierung](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL-Abfragen
Wenn Sie externe Datenquellen und externen Tabellen definiert haben, können reguläre SQL Server-Verbindungszeichenfolgen Datenbanken herstellen, externen Tabellen definiert. Sie können dann T-SQL-Anweisungen über externen Tabellen über diese Verbindung mit der unten beschriebenen ausführen. Weitere Informationen und Beispiele für T-SQL-Abfragen finden in den Themen für [horizontale Partitionierung](sql-database-elastic-query-horizontal-partitioning.md) und [vertikale Partitionierung](sql-database-elastic-query-vertical-partitioning.md)Sie.

## <a name="connectivity-for-tools"></a>Konnektivität für tools
Reguläre SQL Server-Verbindungszeichenfolgen können Sie Ihre Applikationen und BI oder Daten Integrationstools Datenbanken herstellen, die externe Tabellen. Stellen Sie sicher, dass SQL Server als Datenquelle für das Tool unterstützt wird. Sobald verbunden, finden Sie die elastische Abfragedatenbank und die externen Tabellen in der Datenbank genau wie mit jeder anderen SQL Server-Datenbank, die mit der Verbindung.

> [AZURE.IMPORTANT] Authentifizierung mit Active Directory Azure elastische Abfragen wird derzeit nicht unterstützt.

## <a name="cost"></a>Kosten

Elastische Abfrage ist in die Kosten von Azure SQL Datenbank Datenbanken enthalten. Beachten Sie, dass Topologien die entfernten Datenbanken in einem anderen als den Endpunkt elastische Abfrage sind werden Daten Ausgang aus Remotedatenbanken regulären [Azure Preise](https://azure.microsoft.com/pricing/details/data-transfers/)berechnet werden.

## <a name="preview-limitations"></a>Vorschau-Grenzen
* Die erste elastische Abfrage ausführen kann einige Minuten dauern auf den Standard Performance. Dieses Mal ist die elastische Abfragefunktion laden. verbessert die Leistung beim Laden von mit höherer Leistung.
* Scripting von externen Datenquellen oder externen Tabellen SSMS oder SSDT wird nicht unterstützt.
* Import/Export für SQL DB unterstützt noch externe Datenquellen und externen Tabellen nicht. Benötigen Sie Import/Export verwenden, löschen Sie diese Objekte vor dem Exportieren und dann nach dem Import neu erstellen.
* Elastische Abfrage unterstützt derzeit nur schreibgeschützten Zugriff auf externe Tabellen. Allerdings können Sie alle T-SQL-Funktionen in der Datenbank, in die externe Tabelle definiert. Dies ist nützlich, z. B. temporäre Ergebnisse verwenden, z. B. beizubehalten, wählen < Column_list > in < Local_table > oder gespeicherte Prozeduren elastische Abfrage-Datenbank, die mit externen Tabellen beziehen.
* LOB-Typen sind in Definitionen für externe außer nvarchar(max) nicht unterstützt. Als Workaround Sie erstellen eine Ansicht in der entfernten Datenbank, die den Typ LOB nvarchar(max) umwandelt, definieren die externe Tabelle in die Ansicht anstelle der Basistabelle und wandeln Sie ihn wieder in den ursprünglichen Typ LOB, in Ihren Abfragen.
* Spaltenstatistiken externen Tabellen werden nicht unterstützt. Statistiken für Tabellen werden unterstützt, aber manuell erstellt werden.

## <a name="feedback"></a>Feedback
Teilen Sie Feedback zu Ihrer Erfahrung mit elastische Abfragen mit uns Disqus folgenden MSDN-Foren oder Stackoverflow. Wir sind alle Arten von Feedback zu Service (Fehler, Kanten, Funktion Lücken) interessiert.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu datenbankübergreifenden Abfragen und vertikale Partitionierung Szenarios finden Sie in den folgenden Dokumenten:

* [Datenbankübergreifende Abfragen und vertikale Partitionierung (Übersicht)](sql-database-elastic-query-vertical-partitioning.md)
* Versuchen Sie unsere schrittweise Lernprogramm zu einer vollständigen Arbeitsbeispiel Minuten: [Erste Schritte mit Cross - Abfrage (vertikale Partitionierung)](sql-database-elastic-query-getting-started-vertical.md).


Weitere Informationen zum horizontalen Partitionierung und Sharding Szenarien ist hier verfügbar:

* [Horizontale Partitionierung und Sharding (Übersicht)](sql-database-elastic-query-horizontal-partitioning.md)
* Versuchen Sie unsere schrittweise Lernprogramm zu einer vollständigen Arbeitsbeispiel Minuten: [Erste Schritte mit elastische Abfrage für horizontale Partitionierung (Sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
