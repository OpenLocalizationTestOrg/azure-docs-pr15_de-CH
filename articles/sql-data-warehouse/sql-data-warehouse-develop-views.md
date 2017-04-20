<properties
   pageTitle="Ansichten im SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Transact-SQL-Ansichten in Azure SQL Data Warehouse Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Ansichten im SQL Datawarehouse

Ansichten sind besonders im SQL Data Warehouse. Sie können auf vielfältige Arten zur Verbesserung der Qualität der Lösung verwendet werden.  Dieser Artikel erläutert einige Beispiele zum Gestalten Sie Ihre Lösung mit Ansichten, die Grenzen, die berücksichtigt werden müssen.

> [AZURE.NOTE] Syntax für `CREATE VIEW` in diesem Artikel nicht behandelt. Finden Sie die [CREATE VIEW][] MSDN-Artikel für diese Informationen.

## <a name="architectural-abstraction"></a>Architektonische Abstraktion
Eine sehr allgemeine Anwendung wird Tabellen erstellen Tabelle als auswählen (CTAS) gefolgt von einem Objekt Muster umbenennen, während Daten laden neu erstellen.

Im folgenden Beispiel wird eine neue Datensätze hinzugefügt. Beachten Sie, wie neue Tabble, DimDate_New, zunächst erstellt und dann umbenannt, um die ursprüngliche Version der Tabelle ersetzen.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Dieser Ansatz kann jedoch in Tabellen erscheinen und verschwinden des Benutzers sowie Fehlermeldungen "Tabelle existiert nicht" führen. Sichten können verwendet werden, um Benutzern eine konsistente Darstellungsschicht und die zugrunde liegenden Objekte umbenannt werden. Indem Benutzer Zugriff auf Daten über Sichten, bedeutet, dass Benutzer nicht den zugrunde liegenden Tabellen haben müssen. Dies bietet ein einheitliches Benutzererlebnis während der Data-Designer Warehouse kann das Datenmodell entwickeln und Maximierung der Leistung beim Laden von Daten mithilfe von CTAS.    

## <a name="performance-optimization"></a>Performance-Optimierung
Ansichten können auch verwendet werden, um Leistung optimiert Joins zwischen Tabellen zu erzwingen. Beispielsweise kann eine Ansicht redundanten Verteilungsschlüssel als Bestandteil der beitretenden Kriterien Daten zu integrieren.  Ein weiterer Vorteil der Ansicht möglicherweise eine oder verknüpfen Hinweis erzwingen. Mithilfe von Ansichten auf diese Weise wird sichergestellt, dass Verknüpfungen auf optimale Weise Benutzer an das richtige Konstrukt für ihre Joins überflüssig immer ausgeführt werden.

## <a name="limitations"></a>Grenzen
Ansichten im SQL Data Warehouse sind nur Metadaten.  Daher sind die folgenden Optionen nicht verfügbar:

-   Es gibt keine Option Schema binden
-   Tabellen können nicht in der Ansicht aktualisiert werden
-   Ansichten können nicht über temporäre Tabellen erstellt werden
-   Es gibt keine Unterstützung für das Erweitern / NOEXPAND-Hinweise
-   Es sind keine indizierten Ansichten im SQL Data Warehouse


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].
Für `CREATE VIEW` Syntax finden Sie in der [Ansicht erstellen][].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse-Anwendungsentwicklung, Überblick]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[ANSICHT ERSTELLEN]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
