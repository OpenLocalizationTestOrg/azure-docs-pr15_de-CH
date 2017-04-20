<properties
    pageTitle="Daten aus dem BLOB-Speicher mit SQL Datenbank | Microsoft Azure"
    description="In diesem Lernprogramm wird veranschaulicht, wie Kopieraktivität in Azure Data Factory-Pipeline mit Daten aus dem BLOB-Speicher mit SQL-Datenbank kopieren."
    Keywords="BLOB-Sql, BLOB-Speicher Daten kopieren"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Daten Sie aus dem BLOB-Speicher mit SQL-Datenbank mithilfe von Data Factory 
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure-Ressourcen-Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm erstellen Sie eine Factory Daten mit einer Pipeline Daten aus dem BLOB-Speicher in SQL-Datenbank kopiert.

Der Kopie Aktivität Datenübertragungen in Azure Data Factory. Es versorgt von einem global zur Verfügung, die Daten zwischen verschiedenen Datenspeichern eine sichere, zuverlässige und skalierbare Weise kopieren können. Siehe [Datenaktivitäten](data-factory-data-movement-activities.md) Weitere Details zur Aktivität kopieren.  

> [AZURE.NOTE] Eine ausführliche Übersicht über Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md) -Artikel.

##<a name="prerequisites-for-the-tutorial"></a>Voraussetzung für das Lernprogramm
Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Azure-Abonnement**.  Wenn Sie ein Abonnement haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Finden Sie [Kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/) Artikel Weitere Informationen.
- **Azure-Speicher berücksichtigt**. In diesem Lernprogramm verwenden Sie den BLOB-Speicher als **Quelle** Datenspeicher. Wenn ein Azure Storage-Konto haben, finden Sie im Artikel [erstellen ein Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) Schritte erstellen.
- **Azure SQL-Datenbank**. In diesem Lernprogramm verwenden Sie eine SQL Azure-Datenbank als **Ziel** Datenspeicher. Haben Sie eine Azure SQL-Datenbank, die Sie im Lernprogramm verwenden können, finden Sie [das Erstellen und Konfigurieren einer Azure SQL-Datenbank](../sql-database/sql-database-get-started.md) zu erstellen.
- **SQL Server 2012/2014 oder Visual Studio 2013**. Erstellen einer Beispieldatenbank und die Daten in der Datenbank anzuzeigen, verwenden Sie SQL Server Management Studio oder Visual Studio.  

## <a name="collect-blob-storage-account-name-and-key"></a>BLOB-speicherkontonamen und Schlüssel sammeln 
Sie benötigen den Namen und das kontoschlüssel Ihres Kontos Azure-Speicher dieses Lernprogramm soll. Notieren Sie **Namen** und **kontoschlüssel** für Ihr Konto Azure-Speicher.

1. Auf der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie im linken Menü auf **Weitere Dienste** und wählen Sie **Speicherkonten**.

    ![Suchen - Speicherkonten](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Wählen Sie Blatt **Speicherkonten** **Azure Storage-Konto** , das Sie in diesem Lernprogramm verwenden möchten.
4. Klicken Sie **Zugriffstasten** unter **SETTINGS**.
5.  Klicken Sie auf **Kopieren** (Bild) neben Textfeld **Kontoname Speicher** und speichern und Einfügen es irgendwo (zum Beispiel: in einer Textdatei).
6. Wiederholen Sie den vorherigen Schritt kopieren oder notieren **key1**.
    
    ![Speicher-Tastenkombination](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Schließen Sie alle Blades **X**.

## <a name="collect-sql-server-database-user-names"></a>Sammeln von SQL Server, Datenbank oder Benutzernamen
Die Namen der SQL Azure-Server, Datenbank und Benutzer dieses Lernprogramm erforderlich. Notieren Sie die Namen der ** **Server**, **Datenbank**und** für SQL Azure-Datenbank.

1. **Azure-Portal**auf **Weitere Dienste** auf der linken Seite und wählen Sie **SQL-Datenbanken**.
2. Wählen Sie in **SQL-Datenbanken Blade**die **Datenbank** , die Sie in diesem Lernprogramm verwenden möchten. Notieren Sie sich den **Datenbanknamen**.  
3. **SQL Datenbank** -Blatt klicken Sie auf **Eigenschaften** unter **SETTINGS**.
4. Notieren Sie die Werte für **Servername** und **SERVER ADMINISTRATORANMELDUNG**.
5. Schließen Sie alle Blades **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Können Sie Azure Services auf SQL Server zugreifen 
Sicherstellen Sie, dass die **Einstellung **Zugriff auf Azure Services** für SQL Azure-Server aktiviert, damit Daten Factorydienst SQL Azure-Server zugreifen kann** . Überprüfen diese Einstellung aktivieren und führen Sie die folgenden Schritte aus:

1. Klicken Sie auf **Weitere Dienste** Hub auf der linken Seite und auf **SQL Server**.
2. Wählen Sie den Server, und klicken Sie unter **Einstellung**auf **Firewall** . 
4. Klicken Sie **auf** Blatt **Firewall Settings** für **Zugriff auf Azure Services**.
5. Schließen Sie alle Blades **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>BLOB-Speicher und SQL Datenbank vorbereiten 
Jetzt bereiten Sie Ihrer Azure BLOB-Speicher und SQL Azure-Datenbank das Lernprogramm mit den folgenden Schritten vor:  

1. Starten Sie den Editor, fügen Sie den folgenden Text und als **emp.txt** **C:\ADFGetStarted** Ordner auf Ihrer Festplatte speichern.

        John, Doe
        Jane, Doe

2. Verwenden Sie Tools wie [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/) **Adftutorial** Container erstellen und Hochladen der Datei **emp.txt** auf den Container.

    ![Azure-Speicher-Explorer. Daten Sie aus dem BLOB-Speicher mit SQL-Datenbank](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Verwenden Sie das folgende SQL-Skript zum Erstellen der Tabelle **emp** in der Azure SQL-Datenbank.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **SQL Server 2012/2014 auf Ihrem Computer installiert haben:** Anweisungen aus [Schritt2: Verbinden mit SQL-Datenbank der Verwaltung von Azure SQL-Datenbank mit SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md#Step2) Artikel zu Ihrem SQL Azure-Server das SQL-Skript ausführen. In diesem Artikel verwendet das [klassische Azure-Portal](http://manage.windowsazure.com)nicht das [neue Azure-Portal](https://portal.azure.com)Firewall eine SQL Azure-Server konfigurieren.

    Wenn der Client nicht den SQL Azure-Server zugreifen, müssen Sie Firewall für den SQL Azure-Server Zugriff von Ihrem Computer (IP-Adresse). [In diesem Artikel](../sql-database/sql-database-configure-firewall-settings.md) Schritte zum Konfigurieren der Firewall für den SQL Azure-Server anzeigen

Sie haben die erforderlichen Komponenten. Sie können eine Data Factory mithilfe einer der folgenden Arten erstellen. Klicken Sie auf die Registerkarten oben oder folgende Links, das Lernprogramm auszuführen.     

- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
