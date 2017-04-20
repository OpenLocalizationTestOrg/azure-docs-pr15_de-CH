<properties
    pageTitle="Abfrage über Clouddatenbanken mit anderen Schema | Microsoft Azure"
    description="datenbankübergreifende Abfragen über vertikale Partitionen einrichten"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Clouddatenbanken mit unterschiedlichen Schemas (Vorschau) Abfragen

![Tabellen in unterschiedlichen Datenbanken Abfragen][1]

Vertikal partitioniert Datenbanken verwenden unterschiedliche Tabellen in verschiedenen Datenbanken. Das bedeutet, dass das Schema auf andere Datenbanken. Beispielsweise sind alle Tabellen für den Bestand in einer Datenbank alle Accounting-bezogene Tabellen in einer zweiten Datenbank. 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Der Benutzer muss alle EXTERNEN Datenquelle ALTER-Berechtigung besitzen. Diese Berechtigung wird mit der ALTER DATABASE-Berechtigung.
* Alle EXTERNEN Datenquelle ändern Berechtigungen werden auf der zugrunde liegenden Datenquelle.

## <a name="overview"></a>Übersicht

**Hinweis**: anders als mit der horizontalen Partitionierung DDL-Anweisungen hängen keine Datenebene mit einem Splitter durch die Clientbibliothek elastische Datenbank definieren.

1. [HAUPTSCHLÜSSEL ERSTELLEN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [ERSTELLEN DATENBANK BEGRENZT ANMELDEINFORMATIONEN](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE DATENQUELLE ERSTELLEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABELLE ERSTELLEN](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Der Gültigkeitsbereich Datenbankhauptschlüssel und Anmeldeinformationen erstellen 

Die Anmeldeinformationen elastische Abfrage dient zum entfernten Datenbanken herstellen.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Hinweis**    Sicherstellen, dass die *<username>* ist *"@servername"* Suffix. 

## <a name="create-external-data-sources"></a>Erstellen von externen Datenquellen

Syntax:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Wichtig**   Der Typparameter muss **RDBMS**festgelegt werden. 

### <a name="example"></a>Beispiel 

Das folgende Beispiel veranschaulicht die Verwendung der CREATE-Anweisung für externe Datenquellen. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Die Liste der aktuellen externen Datenquellen abrufen: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externe Tabellen 

Syntax:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Beispiel  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Im folgenden Beispiel wird veranschaulicht, wie die Liste der externen Tabellen aus der aktuellen Datenbank abrufen: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Beschreibung

Elastische Abfrage erweitert vorhandene externe Tabelle Syntax zum externe Tabellen definieren, die externe Datenquellen vom Typ RDBMS zu verwenden. Eine externe Tabellendefinition für vertikale Partitionierung werden folgende Aspekte behandelt: 

* **Schema**: die externe Tabelle DDL definiert ein Schema, das Ihre Abfragen verwenden können. Das Schema in der Definition der externen Tabelle muss das Schema der Tabellen in der entfernten Datenbank übereinstimmen, in dem die tatsächlichen Daten gespeichert. 

* **Entfernte Datenbankverweis**: externe Tabelle DDL auf einer externen Datenquelle. Die externe Datenquelle gibt den logischen Servernamen und Namen der entfernten Datenbank die eigentlichen Tabellendaten Speicherort. 

Verwenden eine externe Datenquelle wie im vorherigen Abschnitt beschrieben, lautet die Syntax zum Erstellen von externer Tabellen 

DATA_SOURCE-Klausel definiert die externe Datenquelle (d. h. der entfernten Datenbank bei vertikale Partitionierung), die für die externe Tabelle verwendet wird.  

Die Klauseln SCHEMA_NAME und OBJECT_NAME bieten die Möglichkeit, die Definition der externen Tabelle in eine Tabelle in einem anderen Schema in der entfernten Datenbank oder einer Tabelle mit einem anderen Namen zuordnen. Dies ist nützlich, wenn Sie eine externe Tabelle in der entfernten Datenbank – oder eine andere Situation, Namen der entfernten Tabelle bereits lokal verwendet wird, eine Katalogansicht oder DMV definieren.  

Die folgende DDL-Anweisung löscht eine externen Tabellendefinition aus dem lokalen Katalog. Sie beeinflusst nicht die entfernte Datenbank. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Berechtigungen für CREATE/DROP externe Tabelle**: alle EXTERNEN Datenquelle ALTER-Berechtigungen für externe Tabelle DDL die auch auf der zugrunde liegenden Datenquelle verweisen benötigt werden.  

## <a name="security-considerations"></a>Sicherheitsaspekte
Benutzer mit Zugriff auf die externe Tabelle erhalten automatisch Zugriff auf die zugrunde liegenden remote Tabellen in externen Datenquellendefinition angegebenen Anmeldeinformationen. Sie sollten Zugriff auf die externe Tabelle sorgfältig verwalten, um unerwünschte Erhöhung von Berechtigungen durch die Anmeldeinformationen der externen Datenquelle zu vermeiden. Reguläre SQL-Berechtigungen können gewähren oder Aufheben des Zugriffs auf externe Tabelle nur als wäre es eine normale Tabelle verwendet werden.  


## <a name="example-querying-vertically-partitioned-databases"></a>Beispiel: Abfragen vertikal partitioniert Datenbanken 

Die folgende Abfrage führt drei-Wege-Verknüpfung zwischen zwei lokalen Tabellen für Aufträge und Auftragspositionen und die entfernte Tabelle für Kunden. Dies ist ein Beispiel des Anwendungsfalls Verweis Daten für elastische Abfrage: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Reguläre SQL Server-Verbindungszeichenfolgen können die Integrationstools BI- und Daten auf Datenbanken auf SQL-Datenbankserver herstellen, elastische Abfrage aktiviert und externen Tabellen definiert ist. Stellen Sie sicher, dass SQL Server als Datenquelle für das Tool unterstützt wird. Auf Abfragedatenbank elastische und ihre externen Tabellen wie jede andere SQL Server-Datenbank, die Sie mit dem Tool Verbindung würde verweisen. 

## <a name="best-practices"></a>Bewährte Methoden 
 
* Stellen Sie sicher, daß die elastische Abfrage Endpunkt Datenbank Zugriff in die entfernte Datenbank ermöglicht den Zugriff in der Firewallkonfiguration DB SQL Azure-Dienste. Außerdem sicherstellen Sie, dass in der externen Datenquellendefinition bereitgestellten Anmeldeinformationen erfolgreich in der entfernten Datenbank anmelden kann und die Zugriffsberechtigungen für die entfernte Tabelle hat.  

* Elastische Abfrage funktioniert am besten für Abfragen, die meisten der Berechnung auf den entfernten Datenbanken möglich. Sie erhalten in der Regel die abfrageleistung mit benutzerdefinierter Filterprädikate ausgewertet werden kann, auf entfernte Datenbanken oder Joins, die vollständig in der entfernten Datenbank ausgeführt werden können. Andere Abfragemuster müssen große Mengen von Daten aus der entfernten Datenbank laden und schlecht durchführen können. 


## <a name="next-steps"></a>Nächste Schritte

Zum Abfragen von horizontal partitionierter Datenbanken (auch bekannt als Sharding) finden Sie unter [Abfragen über Sharding Clouddatenbanken (horizontal partitioniert)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
