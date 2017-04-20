<properties
    pageTitle="Über Clouddatenbanken skalierten | Microsoft Azure"
    description="elastische Abfragen über horizontale Partitionen einrichten"    
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
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Berichte über skalierte Clouddatenbanken (Vorschau)

![Splitter Abfragen][1]

Sharding Datenbanken Zeilen skalierten Daten verteilen Tier. Das Schema ist für alle teilnehmenden Datenbanken, auch bekannt als horizontale Partitionierung identisch. Elastische Abfrage können Sie Berichte erstellen, die alle Datenbanken in einer Sharding Datenbank umfassen.

Quick-Start finden Sie unter [Reporting über skalierte Cloud-Datenbanken](sql-database-elastic-query-getting-started.md).

Nicht Sharding Datenbanken finden Sie in der [Abfrage über Clouddatenbanken mit unterschiedlichen Schemas](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Erforderliche Komponenten

* Erstellen einer Splitter Karte elastische Datenbank-Clientbibliothek. Siehe [Splitter Management zuordnen](sql-database-elastic-scale-shard-map-management.md). Oder verwenden Sie die Beispiel-app in [Erste Schritte mit elastische Datenbanktools](sql-database-elastic-scale-get-started.md).
* Alternativ dazu können [Datenbanken skalierte Datenbanken migrieren](sql-database-elastic-convert-to-use-elastic-tools.md).
* Der Benutzer muss alle EXTERNEN Datenquelle ALTER-Berechtigung besitzen. Diese Berechtigung wird mit der ALTER DATABASE-Berechtigung.
* Alle EXTERNEN Datenquelle ändern Berechtigungen werden auf der zugrunde liegenden Datenquelle.

## <a name="overview"></a>Übersicht

Diese Aussagen erstellen Metadatendarstellung Sharding Datenebene elastische Abfragedatenbank 


1. [HAUPTSCHLÜSSEL ERSTELLEN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [ERSTELLEN DATENBANK BEGRENZT ANMELDEINFORMATIONEN](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE DATENQUELLE ERSTELLEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABELLE ERSTELLEN](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 Gültigkeitsbereich Datenbankhauptschlüssel und Anmeldeinformationen erstellen 

Die Anmeldeinformationen elastische Abfrage dient zum entfernten Datenbanken herstellen.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Stellen Sie sicher, dass die *"\<Benutzername\>"* ist *"@servername"* Suffix. 

## <a name="12-create-external-data-sources"></a>1.2 externe Datenquellen erstellen

Syntax:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Beispiel 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Die Liste der aktuellen externen Datenquellen abrufen: 

    select * from sys.external_data_sources; 

Die externe Datenquelle verweist auf die shardzuordnung. Elastische Abfrage mithilfe der externen Datenquelle und der zugrunde liegenden shardzuordnung Datenbanken auflisten, die auf der Datenebene teilnehmen. Die gleichen Anmeldeinformationen werden Splitter-Karte zu lesen und Zugriff auf die Daten auf den Splitter während der Verarbeitung einer elastischen Abfrage verwendet. 

## <a name="13-create-external-tables"></a>1.3 Erstellen von externen Tabellen 
 
Syntax:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Beispiel**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Die Liste der externen Tabellen aus der aktuellen Datenbank abgerufen: 

    SELECT * from sys.external_tables; 

Externe Tabellen zu löschen:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Beschreibung

Daten\_DATENQUELLEN-Klausel definiert die externe Datenquelle (shardzuordnung), die für die externe Tabelle verwendet wird.  

Das SCHEMA\_NAME und\_NAME-Klausel eine Tabelle in ein anderes Schema Definition der externen Tabelle zuordnen. Wenn nicht angegeben, wird davon ausgegangen, dass das Schema des Remoteobjekts "Dbo" und der Name wird der Name der externen Tabelle definiert werden. Dies ist nützlich, wenn der Name der entfernten Tabelle in der Datenbank bereits verwendet wird, in der Sie die externe Tabelle erstellen möchten. Z. B. eine externe Tabelle einen Gesamtüberblick über Katalogsichten zu definieren oder tier DMVs skalierten Daten. Da Katalogsichten und DMVs lokal vorhanden, nicht ihren Namen für die Definition der externen Tabelle zur Verfügung. Verwenden Sie stattdessen einen anderen Namen und verwenden die Katalogansicht oder der DMV Name im SCHEMA\_NAME und/oder\_NAME-Klausel. (Siehe Beispiel unten). 

Die Verteilung-Klausel gibt die Verteilung der Daten für diese Tabelle verwendet. Der Abfrageprozessor verwendet die Informationen in der DISTRIBUTION-Klausel die effizientesten Abfragepläne erstellen.  

1. **SHARDED** bedeutet, dass Daten über Datenbanken hinweg horizontal partitioniert werden. Der Partitionierungsschlüssel für die Verteilung der Daten ist der Parameter **< Sharding_column_name >** .
2. **REPLIZIERTE** bedeutet identische Kopien der Tabelle in jeder Datenbank vorhanden sind. Es liegt in Ihrer Verantwortung sicherzustellen, dass die Replikate über Datenbanken hinweg identisch sind.
3. **Runden\_ROBIN** bedeutet, dass die Tabelle horizontal partitioniert ist eine anwendungsabhängige Verteilungsmethode verwenden. 

**Datenebene Verweis**: externe Tabelle DDL auf einer externen Datenquelle. Die externe Datenquelle gibt eine Splitter-Karte bietet die externe Tabelle mit Angaben zu allen Datenbanken auf der Datenebene. 


### <a name="security-considerations"></a>Sicherheitsaspekte 

Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden remote Tabellen in externen Datenquellendefinition angegebenen Anmeldeinformationen. Vermeiden Sie unerwünschte Erhöhung von Berechtigungen durch die Anmeldeinformationen der externen Datenquelle. Verwenden Sie GRANT oder REVOKE für eine externe Tabelle nur als wäre es eine normale Tabelle.  

Nachdem Sie Ihre externe Datenquelle und externen Tabellen definiert haben, können Sie jetzt vollständige T-SQL über externen Tabellen.

## <a name="example-querying-horizontal-partitioned-databases"></a>Beispiel: Abfragen von horizontal partitionierten Datenbanken 

Die folgende Abfrage führt drei-Wege-Verknüpfung zwischen Lagerorten, Aufträgen und Auftragspositionen und mehrere Aggregate und benutzerdefinierter Filter. Es wird angenommen (1) horizontale Partitionierung (Sharding) und (2) sind Lagerorte, Aufträgen und Auftragspositionen nach Lagerort Id Spalte Sharding, elastische Abfrage Grenzschutz Joins auf der Splitter und teuren Teil der Abfrage auf die Splitter parallel verarbeiten kann. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Gespeicherte Prozedur für die Remoteausführung von T-SQL: sp\_Execute_remote

Elastische Abfrage führt auch eine gespeicherte Prozedur, die direkten auf die Splitter Zugriff. Die gespeicherte Prozedur aufgerufen [sp\_ausführen \_remote](https://msdn.microsoft.com/library/mt703714) und auszuführende remote gespeicherte Prozeduren oder T-SQL-Code aus den entfernten Datenbanken verwendet werden. Es verwendet die folgenden Parameter: 

* Name der Datenquelle (Nvarchar): der Name der externen Datenquelle vom Typ RDBMS. 
* Abfrage (Nvarchar): der T-SQL Abfrage für jede Splitter. 
* Parameterdeklaration (Nvarchar) - optional: Zeichenfolge mit Typdefinitionen für die Parameter in der Abfrage-Parameter (wie Sp_executesql) verwendet. 
* Wert Parameterliste - optional: durch Kommas getrennte Liste von Parameterwerten (wie Sp_executesql).

Der sp\_ausführen\_Remote verwendet die externe Datenquelle die Aufrufparameter gemäß bestimmte T-SQL-Anweisung in entfernten Datenbanken ausgeführt. Shardmap Manager-Datenbank und der entfernten Datenbank verwendet die Anmeldeinformationen der externen Datenquelle.  

Beispiel: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Konnektivität für tools  

Verwenden Sie reguläre SQL Server-Verbindungszeichenfolgen zur Verbindung der Anwendung der Integrationstools BI- und Daten mit externen Tabellendefinitionen. Stellen Sie sicher, dass SQL Server als Datenquelle für das Tool unterstützt wird. Dann verbunden Verweis elastische Abfragedatenbank wie SQL Server-Beispieldatenbank Tool und mit externen Tabellen aus einem Tool oder einer Anwendung als wären sie lokale Tabellen. 

## <a name="best-practices"></a>Bewährte Methoden 

* Sicherstellen Sie, dass elastische Abfrage Endpunkt Datenbank Zugriff auf die Datenbank Shardmap und alle Splitter über Firewalls hinweg SQL DB gewährt wurde.  

* Überprüfen Sie oder erzwingen Sie die Verteilung der Daten in der externen Tabelle definiert. Ist die Verteilung der Daten von der Verteilung in der Tabellendefinition können Abfragen zu unerwarteten Ergebnissen führen. 

* Elastische Abfrage ist derzeit nicht Splitter Eliminierung beim Prädikaten für die shardingschlüssel Verarbeitung bestimmte Splitter sicher ausschließen zulassen würde.

* Elastische Abfrage funktioniert am besten für Abfragen, die meisten der Berechnung auf der Splitter erfolgt. Sie erhalten normalerweise die abfrageleistung mit benutzerdefinierter Filterprädikate ausgewertet werden kann Splitter oder Joins die Partitionierung Schlüssel so ausgerichteter Partitionierung auf alle Splitter ausgeführt werden können. Andere Abfrage müssen große Datenmengen aus der Splitter auf dem Head-Knoten geladen und möglicherweise Leistungsverluste

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
