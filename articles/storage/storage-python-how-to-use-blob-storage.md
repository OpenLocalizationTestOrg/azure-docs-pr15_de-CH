<properties
    pageTitle="Wie Azure BLOB-Speicher (Objektspeicher) aus Python | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-azure-blob-storage-from-python"></a>Verwendung von Python Azure Blob-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Azure BLOB-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte-Blobs in der Cloud gespeichert. Beliebigen Text oder Binärdaten, Dokument, Datei oder Anwendung Installer kann BLOB-Speicher gespeichert werden. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

Dieser Artikel zeigt Ihnen Szenarien mit BLOB-Speicher durchführen. Die Proben sind in Python geschrieben und verwenden [Microsoft Azure Storage SDK für Python]. Behandelten Beispiele hochladen, auflisten, herunterladen und Löschen von Blobs.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Erstellen eines Containers

Basierend auf dem Typ des BLOBs verwenden, **BlockBlobService**, **AppendBlobService**oder **PageBlobService** Objekt erstellen möchten. Der folgende Code verwendet ein **BlockBlobService** -Objekt. Fügen Sie folgenden am oberen Rand jeder Python-Datei in der Azure Blob Blockspeicher programmgesteuert zugreifen möchten.

    from azure.storage.blob import BlockBlobService

Der folgende Code erstellt ein **BlockBlobService** Objekt Konto und Speicherschlüssel verwenden.  Ersetzen Sie "Mein Konto" und "Mykey" mit Ihren Benutzernamen und Schlüssel.

    block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Im folgenden Codebeispiel können Sie ein **BlockBlobService** -Objekt Container erstellen, falls er vorhanden ist.

    block_blob_service.create_container('mycontainer')

Neue Container ist standardmäßig private, daher den speicherzugriffsschlüssel müssen Sie (wie zuvor) herunterladen Blobs in diesem Container. Wenn Sie Blobs im Container für alle Benutzer verfügbar machen möchten, können Sie Container erstellen und die öffentliche Zugriffsebene mithilfe des folgenden Codes übergeben.

    from azure.storage.blob import PublicAccess
    block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)

Alternativ können Sie einen Container nach mithilfe des folgenden Codes erstellen ändern.

    block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)

Nach dieser Änderung jeder im Internet kann Blobs in einem öffentlichen Container sehen, aber nur Sie ändern oder löschen können.

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Erstellt ein upload von Daten und verwenden die **Erstellen\_Blob\_von\_Pfad**, **Erstellen\_Blob\_aus\_Stream**, **Erstellen\_Blob\_von\_Bytes** oder **Erstellen\_Blob\_von\_Text** Methoden. Sie sind allgemeine Methoden, die erforderlichen chunking überschreitet die Datenmenge 64 MB.

**Erstellen\_Blob\_von\_Pfad** lädt den Inhalt einer Datei aus dem angegebenen Pfad und **Erstellen\_Blob\_von\_Stream** lädt den Inhalt einer bereits geöffneten Datei-Stream. **Erstellen\_Blob\_von\_Bytes** lädt ein Bytearray und **Erstellen\_Blob\_von\_Text** lädt den angegebenen Textwert mit der angegebenen Codierung (standardmäßig UTF-8).

Im folgende Beispiel wird der Inhalt der Datei **sunset.png** in **Myblob** Blob hochgeladen.

    from azure.storage.blob import ContentSettings
    block_blob_service.create_blob_from_path(
        'mycontainer',
        'myblockblob',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png')
                )

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Zum Auflisten von Blobs in einem Container verwenden die **Liste\_Blobs** Methode. Diese Methode gibt einen Generator. Im folgende Code wird der **Name** jedes BLOB in einem Container auf der Konsole.

    generator = block_blob_service.list_blobs('mycontainer')
    for blob in generator:
        print(blob.name)

## <a name="download-blobs"></a>Herunterladen von blobs

Verwenden, um Daten von einem Blob herunterzuladen **erhalten\_Blob\_,\_Pfad**, **erhalten\_Blob\_,\_Stream**, **abrufen\_Blob\_,\_Bytes**, oder **erhalten\_Blob\_,\_Text**. Sie sind allgemeine Methoden, die erforderlichen chunking überschreitet die Datenmenge 64 MB.

Das folgende Beispiel veranschaulicht die Verwendung **abrufen\_Blob\_,\_Pfad** Inhalt der Blob **Myblob** heruntergeladen und in die Datei **sunset.png aus** .

    block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Rufen Sie anschließend zum Löschen eines BLOBs **Delete_blob**.

    block_blob_service.delete_blob('mycontainer', 'myblockblob')

## <a name="writing-to-an-append-blob"></a>Schreiben in ein Blob anhängen

Ein Blob Append ist für Append Operationen wie Protokollierung optimiert. Wie ein Blockblob ein Blob Append besteht aus Blöcken, aber wenn ein Blob Anhängen einen neuen Block hinzufügen, es ist immer am Ende angefügt des Blob. Nicht aktualisieren oder Löschen einen vorhandenen Block in ein Blob anhängen. Die Block-IDs für ein Blob Anhängen sind nicht verfügbar, wie für ein.

Jeder Block in ein Blob anhängen kann eine andere Größe maximal 4 MB und ein Append-Blob kann maximal 50.000 Blöcke enthalten. Die maximale Größe eines Append-BLOBs ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Im folgenden Beispiel erstellt ein neues Anhängen Blob und fügt einige Daten, simuliert eine einfache Protokollierung.

    from azure.storage.blob import AppendBlobService
    append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

    # The same containers can hold all types of blobs
    append_blob_service.create_container('mycontainer')

    # Append blobs must be created before they are appended to
    append_blob_service.create_blob('mycontainer', 'myappendblob')
    append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

    append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der BLOB-Speicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherdienste REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure Storage SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK für Python]: https://github.com/Azure/azure-storage-python
