<properties
    pageTitle="Verschieben von Daten zu und von Azure BLOB-Speicher mit Python | Microsoft Azure"
    description="Verschieben von Daten zu und von Azure BLOB-Speicher mit Python"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Verschieben von Daten zu und von Azure BLOB-Speicher mit Python

Dieses Thema beschreibt die Liste hochladen und Herunterladen von Blobs mit der Python-API. Mit der Python-API in Azure SDK bereitgestellten können Sie:

- Erstellen eines Containers
- Ein Container einen Blob hochladen
- Herunterladen von blobs
- Liste der Blobs in einem container
- Löschen eines BLOBs

Weitere Informationen über die Python-API finden Sie unter [Verwendung der Blob-Speicherdienst Python](../storage/storage-python-how-to-use-blob-storage.md).

Hinweise auf Daten und/oder von Azure BLOB-Speicher verschoben werden hier verknüpft:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Wenn Sie VM verwenden, die mit [Daten Wissenschaft virtuelle](machine-learning-data-science-virtual-machines.md)Maschinen in Azure bereitgestellten Skripts eingerichtet wurde, ist AzCopy bereits auf dem virtuellen Computer installiert.

> [AZURE.NOTE] Eine vollständige Einführung in Azure BLOB-Speicher finden Sie in [Azure Blob Grundlagen](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure BLOB-Dienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Erforderliche Komponenten

Dieses Dokument wird vorausgesetzt, dass Sie ein Azure-Abonnement ein Speicherkonto und entsprechende Speicherschlüssel für dieses Konto. Bevor Sie Daten hochladen/herunterladen, müssen Sie Ihre Azure-Konto und Speicherschlüssel kennen.

- Zum Einrichten der Azure-Abonnement finden Sie [kostenlose Testversion für einen Monat](https://azure.microsoft.com/pricing/free-trial/).

- Anleitung zum Erstellen ein Speicherkonto und Konto und wichtige Informationen anzeigen Sie [zur Azure-Speicherkonten](../storage/storage-create-storage-account.md)


## <a name="upload-data-to-blob"></a>Blob Daten hochladen

Fügen Sie den folgenden Codeausschnitt oben Python-Code in der Azure-Speicher programmgesteuert zugreifen möchten:

    from azure.storage.blob import BlobService

Das **BlobService** -Objekt können Sie Behälter mit Blobs arbeiten. Der folgende Code erstellt ein BlobService Objekt Konto und Speicherschlüssel verwenden. Ersetzen Sie Kontoname und kontoschlüssel mit Sachkonto und Schlüssel.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Verwenden Sie die folgenden Methoden zum Hochladen von Daten in ein Blob:

1. Einfügen\_Block\_Blob\_von\_Pfad (lädt den Inhalt einer Datei im angegebenen Pfad)
2. Einfügen\_Block_blob\_von\_Datei (lädt den Inhalt einer bereits geöffneten Datei-Stream)
3. Einfügen\_Block\_Blob\_von\_Bytes (Uploads ein Array von Bytes)
4. Einfügen\_Block\_Blob\_von\_(uploads mit der angegebenen Codierung angegebenen Textwert) Text

Der folgende Beispielcode Uploadet eine lokale Datei in einen Container:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Der folgende Code lädt alle Dateien (ausgenommen Verzeichnisse) in einem lokalen Verzeichnis BLOB-Speicher:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Blob Daten herunterladen

Verwenden Sie die folgenden Methoden, Daten von einem Blob zu downloaden:
1. Abrufen\_Blob\_,\_Pfad
2. Abrufen\_Blob\_,\_Datei
3. Abrufen\_Blob\_,\_Bytes
4. Abrufen\_Blob\_,\_Text

Diese Methoden, die erforderlichen chunking ausführen überschreitet die Datenmenge 64 MB.

Der folgende Beispielcode downloads Inhalt ein Blob in einem Container zu einer lokalen Datei:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Der folgende Code lädt alle Blobs aus einem Container. Liste verwendet\_zu der Liste der verfügbaren Blobs im Container blobs und in einem lokalen Verzeichnis heruntergeladen.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
