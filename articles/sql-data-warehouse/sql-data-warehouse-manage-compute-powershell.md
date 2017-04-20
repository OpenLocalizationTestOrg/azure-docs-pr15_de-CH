<properties
   pageTitle="Verwaltung Compute Azure SQL Data Warehouse (PowerShell) | Microsoft Azure"
   description="PowerShell Aufgaben berechnen macht. Skalierung compute-Ressourcen durch DWUs anpassen. Anhalten oder Fortsetzen von computeressourcen, um Kosten zu sparen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Verwaltung Compute Azure SQL Data Warehouse (PowerShell)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Leistung durch dezentrales Skalieren berechnen Ressourcen und des Speichers ändern Ihre Arbeitslast gerecht. Kosten Sie durch Skalierung zurück Ressourcen Nebenzeiten oder Anhalten Compute insgesamt. 

Diese Auflistung von Tasks verwendet im Azure-Portal:

- Skalierung compute
- Pause compute
- Resume compute

Informationen hierzu finden Sie unter [Verwalten berechnen Overview][].


## <a name="before-you-begin"></a>Bevor Sie beginnen

### <a name="install-the-latest-version-of-azure-powershell"></a>Installieren Sie die neueste Version von Azure PowerShell

> [AZURE.NOTE]  Verwendung von Azure PowerShell mit SQL Data Warehouse benötigen Sie Azure PowerShell Version 1.0.3 oder höher.  Überprüfen Sie die aktuelle Version der Befehl **Get-Modul - ListAvailable-Namen Azure**. Sie können die neueste Version von [Microsoft-Webplattform-Installer][].  Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Erste Schritte mit Azure PowerShell-cmdlets

Erste Schritte:

1. Azure PowerShell zu öffnen. 
2. PowerShell dazu aufgefordert werden, führen Sie diese Befehle zum Azure-Ressourcen-Manager anzumelden und Ihr Abonnement aktivieren.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Rechenleistung skalieren

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Verwenden Sie zum Ändern der DWUs [Set AzureRmSqlDatabase][] PowerShell-Cmdlet. Im folgende Beispiel wird das SLO auf DW1000 für die Datenbank MySQLDW, die auf Server MyServer befindet. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pause compute

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Verwenden Sie zum Anhalten einer [Standby-AzureRmSqlDatabase][] -Cmdlet. Im folgende Beispiel wird eine Datenbank namens Database02 mit dem Namen Server01 gehostet angehalten. Der Server ist ein Azure Ressourcengruppe mit dem Namen ResourceGroup1. 

> [AZURE.NOTE] Beachten Sie, dass Ihr Server ist foo.database.windows.net, "Foo" als ServerName - PowerShell-Cmdlets.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Eine Variante das nächste Beispiel ruft die Datenbank in das $database-Objekt. Es leitet dann das Objekt [AzureRmSqlDatabase unterbrechen][]. Die Ergebnisse werden im ResultDatabase-Objekt gespeichert. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume compute

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Um eine Datenbank zu starten, verwenden Sie [Resume-AzureRmSqlDatabase][] -Cmdlet. Im folgende Beispiel wird eine Datenbank namens Database02 mit dem Namen Server01 gehostet. Der Server ist ein Azure Ressourcengruppe mit dem Namen ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Eine Variante das nächste Beispiel ruft die Datenbank in das $database-Objekt. Er leitet das Objekt [AzureRmSqlDatabase fortsetzen][] und speichert die Ergebnisse in $resultDatabase. Der letzte Befehl zeigt die Ergebnisse an.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte

Andere Verwaltungsaufgaben finden Sie unter [Überblick über][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Überblick]: ./sql-data-warehouse-overview-manage.md
[Zum Installieren und Konfigurieren von Azure PowerShell]: ./powershell-install-configure.md
[Verwalten von Compute-Übersicht]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Anhalten AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[AzureRmSqlDatabase festlegen]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft-Webplattform-Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
