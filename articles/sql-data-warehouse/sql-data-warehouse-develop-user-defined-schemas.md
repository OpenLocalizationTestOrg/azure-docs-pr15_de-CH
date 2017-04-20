<properties
   pageTitle="Benutzerdefinierte Schemas im SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Verwendung von Schemas Transact-SQL Azure SQL Data Warehouse Lösungen."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Benutzerdefinierte Schemas im SQL Data Warehouse

Traditionelle von Datawarehouses verwenden häufig mit separate Datenbanken Anwendungsgrenzen basierend auf Arbeitslast, Domäne oder Sicherheit erstellen. Einem herkömmlichen SQL Server Datawarehouse kann z. B. eine Stagingdatenbank, Data Warehouse-Datenbank und einige Datamart-Datenbanken enthalten. In dieser Topologie ist jeder Datenbank sowie Arbeitslast Sicherheitsgrenze in der Architektur.

Im Gegensatz dazu führt SQL Data Warehouse gesamte Data Warehouse Arbeitslast innerhalb einer Datenbank. Datenbankübergreifende Joins dürfen. SQL Data Warehouse erwartet daher alle Tabellen vom Lager in einer Datenbank gespeichert werden.

> [AZURE.NOTE] SQL Data Warehouse unterstützt keine Cross Datenbankabfragen jeglicher Art. Warehouse-Implementierung, die dieses Muster nutzen müssen daher überprüft werden.

## <a name="recommendations"></a>Empfehlung

Vorschläge für die Konsolidierung von Arbeitslasten, Sicherheit und Domänen hinweg mithilfe benutzerdefinierter Schemas sind

1. Verwenden Sie eine SQL Data Warehouse-Datenbank Ihre gesamte Data Warehouse Arbeitslast ausgeführt
2. Konsolidieren Sie Ihre vorhandenen Data Warehouse-Umgebung um eine SQL Data Warehouse-Datenbank verwenden
3. **Benutzerdefinierte Schemas** Grenze implementierten Datenbanken zu nutzen.

Wenn benutzerdefinierte Schemas nicht zuvor verwendet haben müssen vorn. Verwenden Sie einfach den alten Datenbanknamen als Grundlage für Ihre benutzerdefinierte Schemas im SQL Data Warehouse-Datenbank.

Wenn Schemas müssen einige Optionen bereits verwendet wurde:

1. Entfernen Sie ältere Schemanamen und starten
2. Beibehalten der älteren Schemanamen vorangestellt legacy Schemanamen Namen
3. Beibehalten der älteren Schemanamen Ansichten über die Tabelle in einem zusätzlichen Schema erneut zu alten Schemastruktur implementieren.

> [AZURE.NOTE] Option 3 mag auf den ersten Blick wie attraktivste Option. Allerdings ist der Teufel im Detail. Ansichten werden im SQL Data Warehouse gelesen. Daten oder Tabelle Änderung muss der Basistabelle ausgeführt werden. Option 3 führt auch eine Schicht von Ansichten in Ihrem System. Sie möchten diese zusätzliche nachdenken Wenn Sie Ansichten in Ihrer Architektur bereits verwenden.


### <a name="examples"></a>Beispiele:

Implementieren von benutzerdefinierten Schemas basierend auf Datenbank

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Binden Sie legacy Schemanamen von vorangestellt auf den Tabellennamen. Mithilfe von Schemas der Arbeitslast-Grenze.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behalten Sie ältere Schemanamen Ansichten bei

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Jede Änderung im Schema Strategie benötigt eine Überprüfung des Sicherheitsmodells für die Datenbank. In vielen Fällen können Sie vereinfachen das Sicherheitsmodell durch Zuweisen von Berechtigungen auf Schemaebene können. Wenn spezifischere Berechtigungen erforderlich sind, können Sie Datenbankrollen verwenden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Anwendungsentwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Anwendungsentwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
