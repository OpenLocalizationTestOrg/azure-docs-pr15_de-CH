<properties
    pageTitle="Wie BLOB-Speicher (Storage Objekt) von PHP mit | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Verwendung von PHP-BLOB-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Azure BLOB-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte-Blobs in der Cloud gespeichert. Beliebigen Text oder Binärdaten, Dokument, Datei oder Anwendung Installer kann BLOB-Speicher gespeichert werden. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

Dieses Handbuch veranschaulicht die Szenarien mit Azure Blob-Dienst ausführen. Die Beispiele in PHP geschrieben und [Azure SDK für PHP]verwenden [download]. Behandelten Beispiele **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs. Weitere Informationen zu Blobs finden Sie im Abschnitt [Weiter](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine PHP-Anwendung

Voraussetzung für das Erstellen einer PHP-Anwendung, die Azure Blob-Dienst zugreift, ist das Verweisen auf Klassen in Azure SDK für PHP aus im Code. Alle Entwicklungstools können zum Erstellen einer Anwendung, wie z. B. Editor.

In diesem Handbuch Verwenden Sie Service-Features in einer PHP-Anwendung lokal oder in einer Webrolle Azure Worker-Rolle oder Website Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen der Azure-Clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurieren der Anwendung Zugriff auf BLOB-Dienst

Um die Azure Blob Service-APIs verwenden, müssen Sie:

1. Mit der Anweisung [per] Autoloader-Datei verweisen und
2. Verweisen auf alle Klassen, die Sie verwenden können.

Im folgenden Beispiel wird veranschaulicht, wie Includedatei Autoloader und die **ServicesBuilder** -Klasse.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) angenommen, PHP-Clientbibliotheken für Azure über Composer installiert haben. Wenn die Bibliotheken manuell installiert wird, müssen Sie auf die `WindowsAzure.php` Autoloader-Datei.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In den folgenden Beispielen der `require_once` -Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel ausgeführt wird.

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindungs Azure-Speicher

Um ein Webdienstclient Azure Blob zu instanziieren, müssen Sie zunächst eine gültige Verbindungszeichenfolge. Das Format für den Blob-Verbindungszeichenfolge ist:

Für den Zugriff auf live-Dienstes:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Für den Zugriff auf Speicheremulator:

    UseDevelopmentStorage=true


Um Azure Webdienstclient erstellen, müssen Sie die **ServicesBuilder** -Klasse verwenden. Sie können:

* Die Verbindungszeichenfolge direkt übergeben oder
* Verwenden Sie **CloudConfigurationManager (CCM)** externe Quellen für die Verbindungszeichenfolge zu überprüfen:
    * Es ist standardmäßig mit Unterstützung für eine externe Quelle - Umgebungsvariablen.
    * Neue Datenquellen können durch Erweitern der **ConnectionStringSource** -Klasse.

Die hier aufgeführten Beispiele wird die Verbindungszeichenfolge direkt übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Ein **BlobRestProxy** -Objekt können Sie mit der Methode **CreateContainer** BLOB-Container erstellen. Beim Erstellen eines Containers können Sie Optionen für den Container festlegen, aber dies ist nicht erforderlich. (Das folgende Beispiel veranschaulicht die Zugriffssteuerungsliste (ACL) für Container und Container Metadaten.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Aufrufen von **SetPublicAccess (PublicAccessType::CONTAINER\_und\_BLOBS)** werden die Container und BLOB-Daten über anonyme Anfragen. **SetPublicAccess(PublicAccessType::BLOBS_ONLY)** aufrufen kann Blob-Daten über anonyme Anfragen. Weitere Informationen über Container ACLs finden Sie unter [Set Container-ACL-REST API-][container-acl].

Weitere Informationen zu Fehlercodes für BLOB-Dienst finden Sie unter [Fehlercodes für BLOB-Dienst][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Verwenden Sie zum Hochladen einer Datei als Blob **BlobRestProxy -> CreateBlockBlob** -Methode. Dieser Vorgang erstellt das Blob ist nicht vorhanden oder überschrieben wird. Das folgende Codebeispiel wird vorausgesetzt, dass der Container bereits erstellt wurde und [Fopen verwendet] [ fopen] zum Öffnen der Datei als Stream.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Beachten Sie, dass im vorherige Beispiel einen Blob als Stream hochgeladen. Jedoch ein Blob kann auch hochgeladen werden als eine Zeichenfolge verwenden, z. B. die [Datei\_abrufen\_Inhalt] [ file_get_contents] Funktion. Ändern Sie dazu das vorherige Beispiel mit `$content = fopen("c:\myfile.txt", "r");` , `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Blobs in einem Container aufzulisten, verwenden Sie die Methode **BlobRestProxy -> ListBlobs** mit **Foreach** Schleife über das Ergebnis. Der folgende Code zeigt den Namen jedes BLOB als Ausgabe in einem Container und den URI im Browser angezeigt.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Einen Blob herunterladen

Herunterladen ein BLOBs rufen Sie **BlobRestProxy -> GetBlob** -Methode auf und rufen Sie die **GetContentStream** -Methode des resultierenden Objekts **GetBlobResult** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Beachten Sie, dass im obigen Beispiel einen Blob als Streamressource (das Standardverhalten wird). Können jedoch die [Stream\_abrufen\_Inhalt] [ stream-get-contents] Funktion zurückgegebenen Stream in eine Zeichenfolge konvertieren.

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Übergeben Sie zum Löschen eines BLOBs Containername und Blob zu **BlobRestProxy DeleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Einen BLOB-Container löschen

Übergeben Sie **BlobRestProxy -> DeleteContainer**schließlich um einen BLOB-Container löschen, den Containernamen.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen von Azure Blob-Dienst bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren.

- Besuchen Sie den [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- Siehe [PHP Block BLOB-Beispiel](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Siehe [Beispiel PHP-Blob](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
 
Weitere Informationen finden Sie auch die [PHP-Entwicklercenter](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[per]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
