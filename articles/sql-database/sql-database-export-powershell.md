<properties
    pageTitle="Archivieren einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe von PowerShell"
    description="Archivieren einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe von PowerShell"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Archivieren einer Azure SQL-Datenbank in eine Datei BACPAC mithilfe von PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


Dieser Artikel beschreibt, wie Sie für die Archivierung der SQL Azure-Datenbank in einer [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) Datei (gespeichert in Azure BLOB-Speicher) mit PowerShell.

Wenn Sie ein Archiv einer Azure SQL-Datenbank erstellen möchten, können Sie das Datenbankschema und die Daten in eine BACPAC-Datei exportieren. Eine BACPAC-Datei ist einfach eine ZIP-Datei mit der Erweiterung .bacpac. BACPAC Datei kann später in Azure BLOB-Speicher oder im lokalen Speicher in einem lokalen Verzeichnis gespeichert werden. Außerdem importiert werden in Azure SQL-Datenbank oder in einer SQL Server-Installation vor Ort.

**Hinweise**

- Ein Archiv konsistent sein, Sie müssen sicherstellen, dass kein Schreibzugriff Aktivität erfolgt während des Exports oder aus einer [transaktionell konsistenten Kopie](sql-database-copy.md) der SQL Azure-Datenbank exportieren.
- Die maximale Größe einer BACPAC-Datei in Azure BLOB-Speicher archiviert beträgt 200 GB. Um eine größere BACPAC-Datei in den lokalen Speicher zu archivieren, verwenden Sie das Befehlszeilendienstprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Dieses Dienstprogramm wird mit Visual Studio und SQL Server. Sie können auch die neueste Version von SQL Server Data Tools zu diesem Dienstprogramm [herunterladen](https://msdn.microsoft.com/library/mt204009.aspx) .
- Archivierung in Azure Premium-Speicher mit einer BACPAC-Datei wird nicht unterstützt.
- Wenn der Exportvorgang 20 Stunden überschreitet, kann es abgebrochen. Zum Steigern der Leistung beim Export können Sie folgende Aktionen ausführen:
 - Erhöhen Sie der Dienst vorübergehend.
 - Beenden Sie alle Lese- und Schreibvorgänge während des Exports.
 - Verwenden Sie einen [gruppierten Index](https://msdn.microsoft.com/library/ms190457.aspx) mit Werten ungleich Null für alle großen Tabellen. Ohne gruppierte Indizes möglicherweise ein Export mehr als 6 bis 12 Stunden dauert. Ist der exportdienst muss einen Tabellenscan um ganze Tabelle exportieren abgeschlossen. **DBCC SHOW_STATISTICS** und stellen Sie sicher, dass *RANGE_HI_KEY* nicht null und sein Wert gute ist eine gute Möglichkeit, zu bestimmen, ob die Tabellen für den Export optimiert sind. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs werden nicht für die Sicherung und Wiederherstellung. Azure SQL-Datenbank erstellt automatisch Backups für jede Benutzerdatenbank. Details finden Sie in der [SQL-Datenbank automatisierte Backups](sql-database-automated-backups.md).

Um dieses Artikels abzuschließen, benötigen Sie Folgendes:

- Ein Azure-Abonnement.
- Eine SQL Azure-Datenbank.
- Ein [Azure standardspeicherkonto](../storage/storage-create-storage-account.md)mit einem BLOB-Container der BACPAC im Standardspeicher speichern.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Ihre Datenbank exportieren

[New-AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)-Cmdlet sendet eine Export Datenbank-Anforderung an den Dienst. Die Größe der Datenbank kann der Exportvorgang einige Zeit dauern.

> [AZURE.IMPORTANT] Gewährleistung eine transaktionell konsistente BACPAC-Datei sollte zunächst [eine Kopie der Datenbank erstellen](sql-database-copy-powershell.md)und exportieren die Datenbankkopie.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Überwachen Sie den Fortschritt des Exportvorgangs

Nach dem Ausführen von [New AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), Sie können den Status Überprüfen der Anforderung mit [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Ausführen dieser unmittelbar nach der Anforderung in der Regel gibt **Status: in Bearbeitung**. Wenn Sie sehen **Status: erfolgreich** der Export ist abgeschlossen.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>SQL-Datenbank wird exportiert

Im folgenden Beispiel wird eine vorhandene SQL-Datenbank in eine BACPAC exportiert und wie den Status des Exportvorgangs zu überprüfen.

Zum Ausführen des Beispiels werden einige Variablen müssen Sie bestimmte Werte für Ihre Datenbank und Speicher ersetzt. Suchen Sie in [Azure-Portal](https://portal.azure.com)Ihres Speicherkontos zu Speicher-Kontoname, BLOB-Containername und Wert. Den Schlüssel finden Sie **Zugriffstasten** auf Ihrem Konto Speicher auf.

Ersetzen Sie Folgendes `VARIABLE-VALUES` Werte für bestimmten Azure Ressourcen. Name der Datenbank wird die vorhandene Datenbank zu exportieren.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Importieren einer Azure SQL-Datenbank mithilfe von Powershell finden Sie unter [Importieren einer BACPAC mithilfe von PowerShell](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Neue AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)