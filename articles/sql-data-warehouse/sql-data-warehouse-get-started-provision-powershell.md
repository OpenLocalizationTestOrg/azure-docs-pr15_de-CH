<properties
   pageTitle="SQL Data Warehouse mithilfe von PowerShell erstellen | Microsoft Azure"
   description="Erstellen Sie mithilfe von PowerShell SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Erstellen Sie SQL Data Warehouse mit PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Dieser Artikel zeigt ein mit PowerShell SQL Data Warehouse erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zunächst müssen wie folgt vor:

- **Ein Azure-Konto**: Besuchen Sie [Azure-Testversion][] oder [MSDN Azure][] ein Konto erstellen.
- **SQL Azure-Server**: Näheres [erstellen einen logischen Server Azure SQL-Datenbank mit dem Azure-Portal][] oder [Erstellen einer Azure SQL-Datenbank logischen Server mit PowerShell][] .
- **Ressourcengruppe**: Verwenden derselben Ressourcengruppe als SQL Azure-Server oder finden Sie unter [erstellen eine Ressourcengruppe][].
- **PowerShell Version 1.0.3 oder höher**: Überprüfen Sie die Version mit **Get-Modul - ListAvailable-Namen Azure**.  Die neueste Version kann vom [Microsoft-Webplattform-Installer][]installiert werden.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

> [AZURE.NOTE] Erstellen ein SQL Data Warehouse möglicherweise neue fakturierbaren Service.  Weitere Informationen zu Preisen finden Sie unter [SQL Data Warehouse Preise][] .

## <a name="create-a-sql-data-warehouse"></a>Erstellen Sie ein SQL Datawarehouse

1. Öffnen Sie Windows PowerShell.
2. Dieses Cmdlet Anmeldung an Azure-Ressourcen-Manager ausführen.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Wählen Sie das Abonnement für die aktuelle Sitzung verwenden möchten.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Erstellen Sie Datenbank. Dieses Beispiel erstellt eine Datenbank namens "Mynewsqldw" mit Service Ziel "DW400" auf dem Server mit dem Namen "sqldwserver1", die in der Ressourcengruppe mit dem Namen "mywesteuroperesgp1".

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Erforderliche Parameter sind:

- **RequestedServiceObjectiveName**: der Betrag der [DWU][] werden.  Unterstützte Werte sind: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 und DW6000.
- **Datenbankname**: den Namen des SQL Data Warehouse, die Sie erstellen.
- **ServerName**: der Name des Servers, den Sie für die Erstellung verwenden (V12 muss sein).
- **ResourceGroupName**: Ressourcengruppe, die Sie verwenden.  Verfügbare Ressource zu verwenden Sie Gruppen in Ihrem Abonnement Get-AzureResource.
- **Ausgabe**: "Data Warehouse" muss ein SQL Data Warehouse erstellen.

Optionale Parameter sind:

- **CollationName**: die Standardsortierreihenfolge nicht angegeben ist, SQL_Latin1_General_CP1_CI_AS.  Sortierung kann in einer Datenbank geändert werden.
- **MaxSizeBytes**: die maximale Standardgröße einer Datenbank ist 10 GB.


Weitere Informationen zu verfügbaren Parameter finden Sie unter [Neue AzureRmSqlDatabase][] und [Create Database (Azure SQL Data Warehouse)][].

## <a name="next-steps"></a>Nächste Schritte

Nach SQL Data Warehouse-Bereitstellung sollten Sie versuchen, [Daten zu laden][] oder [zu entwickeln][], [Laden][]oder [Migrieren][].

Wenn Sie weitere Informationen zu SQL Data Warehouse programmatisch managen unsere Artikel zur Verwendung von [PowerShell-Cmdlets und REST-APIs][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Migrieren]: ./sql-data-warehouse-overview-migrate.md
[Entwickeln]: ./sql-data-warehouse-overview-develop.md
[Laden]: ./sql-data-warehouse-load-with-bcp.md
[Laden von Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell-Cmdlets und REST-APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Zum Installieren und Konfigurieren von Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Erstellen Sie einen logischen Server Azure SQL-Datenbank mit Azure-Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Erstellen Sie einen logischen Server Azure SQL-Datenbank mit PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Erstellen eine Ressourcengruppe]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Neue AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Erstellen der Datenbank (SQL Azure Datawarehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft-Webplattform-Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure-Testversion]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure-Gutschriften]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
