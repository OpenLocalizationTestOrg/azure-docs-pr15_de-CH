<properties
    pageTitle="Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung) | Microsoft Azure"   
    description="Verwendung von elastische Abfrage mit vertikal partitionierten Datenbanken"
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
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung) (Vorschau)

Elastische Abfrage (Vorschau) für Azure SQL-Datenbank können Sie T-SQL-Abfragen, die eine einzelne Verbindung mit Datenbanken umfassen. Dieses Thema gilt für [Datenbanken vertikal partitioniert](sql-database-elastic-query-vertical-partitioning.md).  

Wenn abgeschlossen, wird: Informationen zum Konfigurieren und einer Azure SQL-Datenbank Abfragen ausführen, die zugehörige Datenbanken umfassen. 

Weitere Informationen über die Abfragefunktion elastische Datenbank finden Sie unter [Azure SQL-Datenbank elastische Datenbank Abfrage (Übersicht)](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Erstellen von Beispieldatenbanken

Zunächst müssen wir zwei Datenbanken, **Customers** und **Orders**, entweder im gleichen oder in verschiedenen logischen Servern erstellen.   

Führen Sie die folgenden Abfragen für die Datenbank **Aufträge** erstellen Sie die Tabelle **"OrderInformation"** und geben Sie die Beispieldaten. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Jetzt, nach Abfrage **der Kundendatenbank **CustomerInformation** Tabelle erstellen und geben Sie die Beispieldaten** ausführen. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Erstellen von Datenbankobjekten
### <a name="database-scoped-master-key-and-credentials"></a>Bewertete Hauptschlüssel und Anmeldeinformationen-Datenbank

1. Öffnen Sie SQL Server Management Studio oder SQL Server Data Tools in Visual Studio.
2. Verbinden mit der Datenbank Aufträge, und führen Sie folgende T-SQL-Befehle:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Username" und "Password" sollte den Benutzernamen und das Kennwort für die Anmeldung in der Datenbank des Kunden.
    Authentifizierung mit Active Directory Azure elastische Abfragen wird derzeit nicht unterstützt.

### <a name="external-data-sources"></a>Externe Datenquellen
Erstellen Sie eine externe Datenquelle in der Orders-Datenbank führen Sie den folgenden Befehl aus: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externe Tabellen
Erstellen einer externen Tabelle Orders Datenbank die Definition der Tabelle CustomerInformation entspricht:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Ausführen einer Abfrage elastische T-SQL-Beispiel

Nachdem Sie Ihre externe Datenquelle und externen Tabellen definiert haben jetzt können T-SQL Sie externen Tabellen abzufragen. Führen Sie diese Abfrage für die Datenbank Aufträge: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kosten

Derzeit ist die Abfragefunktion elastische Datenbank in die Kosten der SQL Azure-Datenbank enthalten.  

Preisinformationen finden Sie unter [SQL Datenbank Preise](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
