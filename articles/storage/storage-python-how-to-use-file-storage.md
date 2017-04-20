<properties
    pageTitle="Wie Azure Dateispeicher Python | Microsoft Azure"
    description="Informationen Sie zum Verwenden der Azure Dateispeicher Python hochladen, Liste, herunterladen und löschen."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Wie Sie aus Python Azure Dateispeicher

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Übersicht

Dieser Artikel zeigt Ihnen Szenarien mit Dateispeicher durchführen. Die Proben sind in Python geschrieben und verwenden [Microsoft Azure Storage SDK für Python]. Behandelten Beispiele hochladen, auflisten, herunterladen und Löschen von Dateien.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Erstellen einer Freigabe

Das **FileService** -Objekt können Sie mit Freigaben, Verzeichnisse und Dateien arbeiten. Der folgende Code erstellt ein **FileService** -Objekt. Fügen Sie folgenden oben eine Python-Datei in der Azure-Speicher programmgesteuert zugreifen möchten.

    from azure.storage.file import FileService

Der folgende Code erstellt ein **FileService** Objekt Konto und Speicherschlüssel verwenden.  Ersetzen Sie "Mein Konto" und "Mykey" mit Ihren Benutzernamen und Schlüssel.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Im folgenden Codebeispiel können Sie ein **FileService** -Objekt die Freigabe erstellt, falls er vorhanden ist.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Hochladen einer Datei in einen freigegebenen Ordner

Ein Azure-Speicher Dateifreigabe enthält mindestens, ein Stammverzeichnis, befinden. In diesem Abschnitt lernen Sie das Hochladen einer Datei vom lokalen Speicher auf das Stammverzeichnis der Freigabe.

Erstellen Sie eine Datei Hochladen von Daten und Verwenden der **Erstellen\_Datei\_von\_Pfad**, **Erstellen\_Datei\_aus\_Stream**, **Erstellen\_Datei\_aus\_Bytes** oder **Erstellen\_Datei\_von\_Text** Methoden. Sie sind allgemeine Methoden, die erforderlichen chunking überschreitet die Datenmenge 64 MB.

**Erstellen\_Datei\_von\_Pfad** lädt den Inhalt einer Datei aus dem angegebenen Pfad und **Erstellen\_Datei\_von\_Stream** lädt den Inhalt einer bereits geöffneten Datei-Stream. **Erstellen\_Datei\_von\_Bytes** lädt ein Bytearray und **Erstellen\_Datei\_von\_Text** lädt den angegebenen Textwert mit der angegebenen Codierung (standardmäßig UTF-8).

Im folgende Beispiel wird der Inhalt der Datei **sunset.png** in **Myfile** Datei hochgeladen.

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Gewusst wie: Erstellen eines Verzeichnisses

Speicher Organisieren von Dateien in Unterverzeichnissen anstatt alle im Stammverzeichnis. Azure Dateispeicherdienst können Sie Ihr Konto kann Verzeichnisse erstellen. Der folgende Code erstellt ein Unterverzeichnis mit dem Namen **Sampledir** im Stammverzeichnis.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Gewusst wie: Auflisten von Dateien und Verzeichnissen in einer Freigabe

Verwenden Sie zum Auflisten von Dateien und Verzeichnissen in einer Freigabe der **Liste\_Verzeichnisse\_und\_Dateien** Methode. Diese Methode gibt einen Generator. Der folgende Code gibt den **Namen** jeder Datei und Verzeichnis eine Freigabe der Konsole.

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Dateien herunterladen

Verwenden, um Daten aus einer Datei herunterzuladen **erhalten\_Datei\_,\_Pfad**, **abrufen\_Datei\_,\_Stream**, **erhalten\_Datei\_,\_Bytes**, oder **erhalten\_Datei\_,\_Text**. Sie sind allgemeine Methoden, die erforderlichen chunking überschreitet die Datenmenge 64 MB.

Das folgende Beispiel veranschaulicht die Verwendung **abrufen\_Datei\_,\_Pfad** Inhalt der **Datei** Datei heruntergeladen und in die Datei **sunset.png aus** .

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Löschen einer Datei

Rufen Sie anschließend zum Löschen einer Datei **Delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Dateispeicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherdienste REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure Storage SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK für Python]: https://github.com/Azure/azure-storage-python
