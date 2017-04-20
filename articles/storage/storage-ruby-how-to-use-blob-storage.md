<properties
    pageTitle="Wie BLOB-Speicher (Storage Objekt) von Ruby | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Verwendung von Ruby-BLOB-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Azure BLOB-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte-Blobs in der Cloud gespeichert. Beliebigen Text oder Binärdaten, Dokument, Datei oder Anwendung Installer kann BLOB-Speicher gespeichert werden. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

Dieses Handbuch wird wie allgemeine Szenarien mit BLOB-Speicher angezeigt. Die Beispiele wurden mithilfe der Ruby-API. Die Szenarios umfassen **Hochladen auflisten, herunterladen** und **Löschen von** Blobs.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-Anwendung erstellen

Ruby-Anwendung zu erstellen. Eine Anleitung finden Sie in [Ruby on Rails-Anwendung auf eine Azure-VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren Sie Ihre Anwendung auf Speicher zugreifen

Um Azure-Speicher zu verwenden, müssen Sie downloaden und Ruby Azure Paket Komfort Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems des Pakets zu verwenden

1. Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster Gem und Abhängigkeiten installieren "Gem installieren Azure".

### <a name="import-the-package"></a>Paket importieren

Verwenden einen Texteditor, fügen Sie Folgendes zu der Dateianfang Ruby Speicher verwendet werden soll:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Ein Azure-Speicher Verbindung

Azure-Modul liest die Umgebungsvariablen **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS_KEY** für die Verbindung zu Ihrem Konto Azure-Speicher erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen vor **Azure::Blob::BlobService** mit dem folgenden Code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Diese Werte aus einer klassischen oder Ressourcenmanager Speicherkonto in Azure-Portal zu erhalten:

1. Auf der [Azure-Portal](https://portal.azure.com)anmelden.
2. Navigieren Sie zu Speicher-Konto zu verwenden.
3. Klicken Sie im Blatt Einstellungen auf der rechten Seite auf **Zugriffstasten**.
4. In Access Keys Blade, das angezeigt wird, sehen Sie die Taste 1 und Taste 2. Sie können diese verwenden. 
5. Klicken Sie auf Kopieren, um den Schlüssel in die Zwischenablage kopieren. 

Diese Werte aus einem klassischen Speicherkonto im klassischen Azure-Portal zu erhalten:

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Navigieren Sie zu Speicher-Konto zu verwenden.
3. Klicken Sie am unteren Rand des Navigationsbereichs **ZUGRIFFSTASTEN verwalten** .
4. Im Popup-Dialogfeld sehen Sie Storage-Kontoname, primäre Schlüssel und sekundäre Taste. Zugriffstaste können Sie entweder der primären oder der sekundären. 
5. Klicken Sie auf Kopieren, um den Schlüssel in die Zwischenablage kopieren.

## <a name="create-a-container"></a>Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Das **Azure::Blob::BlobService** -Objekt können Sie Behälter mit Blobs arbeiten. Um einen Container zu erstellen, verwenden Sie die **Erstellen\_container()** Methode.

Das folgende Codebeispiel erstellt einen Container oder drucken Sie den Fehler ggf..

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Wenn Sie die Dateien im Container öffentlich machen möchten, können Sie Berechtigungen für den Container festlegen.

Sie können nur die <strong>Erstellen\_container()</strong> Aufruf übergeben der **: öffentliche\_Zugriff\_auf** Option:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Gültige Werte für die **: öffentliche\_Zugang\_auf** sind:

* **Blob:** Gibt öffentliche Lesezugriff für Container und BLOB-Daten. Clients können Blobs im Container über anonyme Anforderung aufzählen, aber das Speicherkonto Container können nicht aufgelistet werden.

* **Container:** Gibt öffentliche Lesezugriff für Blobs. BLOB-Daten in diesem Container können über anonyme Anforderung gelesen werden, aber Containerdaten sind nicht verfügbar. Clients können nicht Blobs im Container über anonyme Anforderung aufgelistet werden.

Oder Sie können die öffentliche Zugriffsebene eines Containers mit **festgelegt\_Container\_acl()** -Methode öffentliche Zugriffsebene an.

Im folgenden Codebeispiel wird die öffentliche Zugriffsebene **Container**:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Verwenden Sie zum Hochladen von Inhalten in ein Blob die **Erstellen\_Block\_blobbuilder** das Blob erstellen Methode eine Datei oder eine Zeichenfolge als Inhalt des Blob.

Im folgende Code lädt die Datei **test.png** als neue Blob mit dem Namen "Image-Blob" im Container.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Verwenden Sie zum Auflisten der Container **list_containers()** Methode.
Zum Auflisten von Blobs in einem Container verwenden **Liste\_blobs()** Methode.

Dies gibt die Urls aller Blobs in allen Containern für das Konto.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Herunterladen von blobs

Blobs herunterladen, verwenden die **abrufen\_blobbuilder** Methode, um den Inhalt abzurufen.

Das folgende Codebeispiel veranschaulicht die Verwendung **abrufen\_blobbuilder** zu Inhalt "Bild-Blob" in eine lokale Datei schreiben.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Löschen eines BLOBs
Schließlich um ein Blob zu löschen, verwenden Sie die **Löschen\_blobbuilder** Methode. Im folgenden Codebeispiel wird veranschaulicht, wie einen Blob zu löschen.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu komplexeren Speicheraufgaben folgen Sie diesen Links:

- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) Repository auf GitHub
- [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
