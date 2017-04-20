<properties
   pageTitle="PowerShell-Cmdlets für Azure SQL Data Warehouse"
   description="Finden Sie die oberen PowerShell-Cmdlets für Azure SQL Data Warehouse zum Anhalten und Fortsetzen einer Datenbank."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse

Viele SQL Data Warehouse Verwaltungsaufgaben können mithilfe von Azure PowerShell-Cmdlets oder anderen APIs verwaltet werden.  Unten sind einige Beispiele von PowerShell-Befehlen zum Automatisieren von Aufgaben im SQL Data Warehouse verwenden.  Einige Beispiele von REST finden Sie unter [Skalierung mit verwalten][].

> [AZURE.NOTE]  Für die Nutzung der Azure PowerShell mit SQL Data Warehouse benötigen Azure PowerShell Version 1.0.3 oder höher.  Überprüfen Sie die Version mit **Get-Modul - ListAvailable-Namen Azure**.  Die neueste Version kann vom [Microsoft-Webplattform-Installer][]installiert werden.  Weitere Informationen zum Installieren der neuesten Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-cmdlets

1. Öffnen Sie Windows PowerShell. 
2. PowerShell dazu aufgefordert werden, führen Sie diese Befehle zum Azure-Ressourcen-Manager anzumelden und Ihr Abonnement aktivieren.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Pause SQL Data Warehouse-Beispiel

Anhalten einer Datenbank namens "Database02" gehostet auf einem Server namens "Server01."  Der Server ist ein Azure Ressourcengruppe mit dem Namen "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Eine Variante dieses Beispiel leitet das abgerufene Objekt die [Suspend-AzureRmSqlDatabase][].  Dadurch wird die Datenbank angehalten. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Starten Sie SQL Data Warehouse-Beispiel

Wiederaufnahme des Betriebs einer Datenbank namens "Database02" gehostet auf einem Server namens "Server01." Der Server enthält eine Ressourcengruppe mit dem Namen "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Eine Variante dieses Beispiel ruft eine Datenbank namens "Database02" von einem Server namens "Server01", die in einer Ressourcengruppe mit dem Namen "ResourceGroup1" enthalten Er leitet das abgerufene Objekt [AzureRmSqlDatabase fortsetzen][].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Beachten Sie, dass Ihr Server ist foo.database.windows.net, "Foo" als ServerName - PowerShell-Cmdlets.

## <a name="frequently-used-powershell-cmdlets"></a>Häufig verwendete PowerShell-cmdlets

PowerShell-Cmdlets werden häufig Azure SQL Data Warehouse.

- [AzureRmSqlDatabase abrufen][]
- [AzureRmSqlDeletedDatabaseBackup abrufen][]
- [AzureRmSqlDatabaseRestorePoints abrufen][]
- [Neue AzureRmSqlDatabase][]
- [AzureRmSqlDatabase entfernen][]
- [Wiederherstellung AzureRmSqlDatabase][] 
- [Resume-AzureRmSqlDatabase][]
- [AzureRmSubscription auswählen][]
- [AzureRmSqlDatabase festlegen][]
- [Anhalten AzureRmSqlDatabase][]

## <a name="next-steps"></a>Nächste Schritte
Weitere PowerShell-Beispiele finden Sie unter:

- [Erstellen Sie ein SQL Data Warehouse mit PowerShell][]
- [Datenbank wiederherstellen][]

Eine Liste aller Aufgaben mit PowerShell automatisiert werden können, finden Sie unter [Azure SQL-Datenbank-Cmdlets][].  Eine Liste der Vorgänge mit automatisiert werden können, finden Sie unter [für Azure SQL-Datenbanken][].

<!--Image references-->

<!--Article references-->
[Zum Installieren und Konfigurieren von Azure PowerShell]: ./powershell-install-configure.md
[Erstellen Sie ein SQL Data Warehouse mit PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Datenbank wiederherstellen]: ./sql-data-warehouse-restore-database-powershell.md
[Skalierung mit verwalten]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[SQL Azure-Datenbank Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Für SQL Azure-Datenbanken]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[AzureRmSqlDatabase abrufen]: https://msdn.microsoft.com/library/mt603648.aspx
[AzureRmSqlDeletedDatabaseBackup abrufen]: https://msdn.microsoft.com/library/mt693387.aspx
[AzureRmSqlDatabaseRestorePoints abrufen]: https://msdn.microsoft.com/library/mt603642.aspx
[Neue AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[AzureRmSqlDatabase entfernen]: https://msdn.microsoft.com/library/mt619368.aspx
[Wiederherstellung AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[AzureRmSubscription auswählen]: https://msdn.microsoft.com/library/dn722499.aspx
[AzureRmSqlDatabase festlegen]: https://msdn.microsoft.com/library/mt619433.aspx
[Anhalten AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft-Webplattform-Installer]: https://aka.ms/webpi-azps
