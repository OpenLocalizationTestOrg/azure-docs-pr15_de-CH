<properties
    pageTitle="Verschieben von Daten zu und von Azure BLOB-Speicher mit AzCopy | Microsoft Azure"
    description="Verschieben von Daten zu und von Azure BLOB-Speicher mit AzCopy"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Verschieben von Daten zu und von Azure BLOB-Speicher mit AzCopy

AzCopy ist ein Befehlszeilenprogramm zum Hochladen, herunterladen und Kopieren von Daten zu und von Microsoft Azure BLOB-Datei und Tabellenspeicher entwickelt.

Anleitung zur Installation von AzCopy und Weitere Informationen auf der Azure-Plattform mit finden Sie unter [Erste Schritte mit AzCopy Befehlszeilenprogramm](../storage/storage-use-azcopy.md).

Hinweise auf Daten und/oder von Azure BLOB-Speicher verschoben werden hier verknüpft:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Wenn Sie VM verwenden, die mit [Daten Wissenschaft virtuelle](machine-learning-data-science-virtual-machines.md)Maschinen in Azure bereitgestellten Skripts eingerichtet wurde, ist AzCopy bereits auf dem virtuellen Computer installiert.

> [AZURE.NOTE] Eine vollständige Einführung in Azure BLOB-Speicher finden Sie in [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure BLOB-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird vorausgesetzt, dass Sie Azure-Abonnement ein Speicherkonto und entsprechende Speicherschlüssel für dieses Konto. Bevor Sie Daten hochladen/herunterladen, müssen Sie Ihre Azure-Konto und Speicherschlüssel kennen.

- Zum Einrichten der Azure-Abonnement finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).

- Anleitung zum Erstellen ein Speicherkonto und Konto und wichtige Informationen anzeigen Sie [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)


## <a name="run-azcopy-commands"></a>AzCopy Befehle ausführen

Um AzCopy Befehle auszuführen, öffnen Sie ein Befehlsfenster und navigieren Sie zum Installationsverzeichnis auf Ihrem Computer befindet sich die ausführbare AzCopy.exe AzCopy. 

Die grundlegende Syntax für AzCopy Befehle ist:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Sie können Ihrem Installationsordner AzCopy hinzufügen und führen Sie die Befehle aus dem Verzeichnis. Standardmäßig wird AzCopy *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* oder *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*installiert.

## <a name="upload-files-to-an-azure-blob"></a>Ein Azure Blob Dateien hochladen

Verwenden Sie den folgenden Befehl, um eine Datei hochzuladen:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Dateien von Azure blob

Zum Herunterladen einer Datei von Azure Blob Befehl:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Übertragen von Blobs zwischen Azure Container

Zum Übertragen von Blobs zwischen Azure Container Befehl:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tipps zur Verwendung von AzCopy

> [AZURE.TIP]   
> 1. Beim **Hochladen von** *Dateien/s* lädt Dateien rekursiv. Ohne diesen Parameter werden Dateien in Unterverzeichnissen nicht hochgeladen.  
> 2. **Downloaden von** *Datei/s* durchsucht rekursiv Container alle Dateien im angegebenen Verzeichnis und dessen Unterverzeichnissen, bis alle Dateien, die dem angegebenen im angegebenen Verzeichnis und dessen Unterverzeichnissen Muster, werden geladen.  
> 3.  Sie können keine **bestimmte BLOB-Datei** herunterladen, verwenden *den Parameter* angeben. Um eine bestimmte Datei geben Sie den Dateinamen Blob herunterladen mit dem */Pattern* -Parameter an. **Parameter/s** lässt sich eine Datei namens Muster rekursiv suchen AzCopy haben. Ohne den Musterparameter downloads AzCopy alle Dateien in diesem Verzeichnis.
