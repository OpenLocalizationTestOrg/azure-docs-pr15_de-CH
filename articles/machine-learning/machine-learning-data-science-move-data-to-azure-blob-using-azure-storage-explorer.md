<properties 
    pageTitle="Verschieben von Daten zu und von Azure BLOB-Speicher mit Azure Storage Explorer | Microsoft Azure" 
    description="Verschieben von Daten zu und von Azure BLOB-Speicher mit Azure Storage Explorer" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Verschieben von Daten zu und von Azure BLOB-Speicher mit Azure Storage Explorer

Azure-Speicher-Explorer ist ein kostenloses Tool von Microsoft, die mit Azure-Speicher unter Windows, Mac OS und Linux ermöglicht. Dieses Thema beschreibt das hoch-und Herunterladen von Daten von Azure BLOB-Speicher verwenden. Das Tool kann [Microsoft Azure Storage Explorer](http://storageexplorer.com/)heruntergeladen werden.

Hinweise auf Daten und/oder von Azure BLOB-Speicher verschoben werden hier verknüpft:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Wenn Sie VM verwenden, die mit [Daten Wissenschaft virtuelle](machine-learning-data-science-virtual-machines.md)Maschinen in Azure bereitgestellten Skripts eingerichtet wurde, ist Azure Storage Explorer bereits auf dem virtuellen Computer installiert.
 
> [AZURE.NOTE] Eine vollständige Einführung in Azure BLOB-Speicher finden Sie in [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure BLOB-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird vorausgesetzt, dass Sie ein Azure-Abonnement ein Speicherkonto und entsprechende Speicherschlüssel für dieses Konto. Bevor Sie Daten hochladen/herunterladen, müssen Sie Ihre Azure-Konto und Speicherschlüssel kennen. 

- Zum Einrichten der Azure-Abonnement finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).
- Anleitung zum Erstellen ein Speicherkonto und Konto und wichtige Informationen anzeigen Sie [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md) Notieren Sie die Zugriffstaste für das Speicherkonto dieser Schlüssel Verbindung mit Azure Storage Explorer-Tools benötigen.
- [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/)kann Azure Storage Explorer-Tools heruntergeladen werden. Übernehmen Sie die Standardeinstellungen bei der Installation.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Azure-Speicher-Explorer verwenden 

Die folgenden Schritte dokumentieren, wie Daten mit Azure Storage Explorer Download oder Upload. 

1.  Starten Sie Microsoft Azure Storage Explorer.
2.  So **Melden Sie sich bei Ihrem Konto...** -Assistenten wählen Sie **Azure-Konto** Symbol, dann **ein Konto hinzufügen** , und geben Sie Anmeldeinformationen. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Um den **Azure-Speicher mit** Assistenten aufzurufen, wählen Sie verbinden **Azure-Speicher** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Geben Sie die Tastenkombination aus dem Azure-Speicher-Konto **mit Azure Storage** -Assistent und dann **Weiter**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Geben Sie im Feld **Kontoname** Storage-Kontonamen und anschließend auf **Weiter**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Das Speicherkonto hinzugefügt sollten nun angezeigt werden. Auf BLOB-Container in ein Speicherkonto erstellen, Maustaste Knoten **BLOB-Container** in das Konto wählen **Blob-Container erstellen**und benennen.
7. Zum Hochladen von Daten in einen Container wählen Sie den Zielcontainer aus und auf die Schaltfläche **Hochladen** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. **...** Rechts neben **dem Feld** auf, wählen Sie eine oder mehrere Dateien aus dem Dateisystem hochladen und klicken Sie auf **Hochladen** , zunächst die Dateien hochladen.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Herunterzuladende Daten Blob in den entsprechenden Container zu klicken Sie auf **Download**auswählen. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


