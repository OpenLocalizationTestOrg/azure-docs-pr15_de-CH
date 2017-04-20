<properties
   pageTitle="Erstellen Sie ein SQL Datawarehouse mit TSQL | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer Azure SQL Data Warehouse mit TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Erstellen Sie SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Dieser Artikel beschreibt, wie eine T-SQL mit SQL Data Warehouse erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst müssen wie folgt vor: 

- **Ein Azure-Konto**: Besuchen Sie [Azure-Testversion][] oder [MSDN Azure][] ein Konto erstellen.
- **SQL Azure-Server**: Näheres [erstellen einen logischen Server Azure SQL-Datenbank mit dem Azure-Portal][] oder [Erstellen einer Azure SQL-Datenbank logischen Server mit PowerShell][] .
- **Ressourcengruppe**: Verwenden derselben Ressourcengruppe als SQL Azure-Server oder finden Sie unter [erstellen eine Ressourcengruppe][].
- **Zur Ausführung von T-SQL-Umgebung**: Verwenden Sie [Visual Studio][Installing Visual Studio and SSDT], [Sqlcmd][]oder [SSMS][] auszuführende T-SQL.

> [AZURE.NOTE] Erstellen ein SQL Data Warehouse möglicherweise neue fakturierbaren Service.  Weitere Informationen zu Preisen finden Sie unter [SQL Data Warehouse Preise][] .

## <a name="create-a-database-with-visual-studio"></a>Erstellen einer Datenbank mit Visual Studio

Wenn Sie mit Visual Studio vertraut sind, finden Sie im Artikel [Abfrage Azure SQL Data Warehouse (Visual Studio)][].  Starten, SQL Server-Objekt-Explorer in Visual Studio öffnen und Verbinden mit dem Server, der SQL Data Warehouse-Datenbank hostet.  Nachdem die Verbindung hergestellt ist, können Sie durch Ausführen des folgenden SQL-Befehls für die **master** -Datenbank ein SQL Data Warehouse erstellen.  Dieser Befehl erstellt die Datenbank MySqlDwDb mit einem Dienst Ziel DW400 und die Datenbank auf maximal 10 TB anwachsen.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Erstellen einer Datenbank mit sqlcmd

Alternativ können Sie den gleichen Befehl mit Sqlcmd ausführen, indem Sie folgende Befehlszeile.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Die Standard-Sortierreihenfolge, wenn keine angegeben ist SQL_Latin1_General_CP1_CI_AS sortieren.  Die `MAXSIZE` zwischen 250 GB und 240 TB.  Die `SERVICE_OBJECTIVE` zwischen DW100 und DW2000 [DWU][].  Eine Liste aller gültigen Werte finden Sie in der MSDN-Dokumentation für [CREATE DATABASE][].  Die MAXSIZE und SERVICE_OBJECTIVE können mit einer T-SQL-Befehl [ALTER DATABASE][] geändert werden.  Die Sortierung einer Datenbank kann nach der Erstellung geändert werden.   Ändern SERVICE_OBJECTIVE ändern DWU verursacht einen Neustart der Dienste, der bricht alle Abfragen in Flight Vorsicht.  MAXSIZE ändern werden keine Dienste neu gestartet ist nur eine einfache Metadatenvorgang.

## <a name="next-steps"></a>Nächste Schritte

Nach SQL Data Warehouse Bereitstellung Sie [Beispieldaten laden][] kann oder wie [entwickeln][], [Laden][]oder [Migrieren][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Abfrage SQL Azure Datawarehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[Migrieren]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Laden]: sql-data-warehouse-overview-load.md
[Laden von Beispieldaten]: sql-data-warehouse-load-sample-databases.md
[Erstellen Sie einen logischen Server Azure SQL-Datenbank mit Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen Sie einen logischen Server Azure SQL-Datenbank mit PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Erstellen eine Ressourcengruppe]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[DATENBANK ERSTELLEN]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure-Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure-Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
