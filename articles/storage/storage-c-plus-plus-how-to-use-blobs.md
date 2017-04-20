<properties
    pageTitle="Wie BLOB-Speicher (Storage Objekt) von C++ | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-blob-storage-from-c"></a>Verwendung von C++-BLOB-Speicher  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Azure BLOB-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte-Blobs in der Cloud gespeichert. Beliebigen Text oder Binärdaten, Dokument, Datei oder Anwendung Installer kann BLOB-Speicher gespeichert werden. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

Dabei werden allgemeine Szenarien mithilfe des Diensts für Azure BLOB-Speicher veranschaulicht. Die Beispiele sind in C++ geschrieben und der [Azure-Speicher-Clientbibliothek für C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Behandelten Beispiele **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs.  

>[AZURE.NOTE] Dabei zielt Azure Storage-Clientbibliothek für C++ Version 1.0.0 und höher. Die empfohlene Version ist Speicher-Clientbibliothek 2.2.0, über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendung
In diesem Handbuch Verwenden Sie Storage-Funktionen in eine C++-Anwendung ausgeführt werden kann.  

Hierzu müssen Sie installieren Azure Storage-Clientbibliothek für C++ und Azure-Speicher in der Azure-Abonnement registrieren.   

Installation von Azure Storage-Clientbibliothek für C++ können Sie die folgenden Methoden:

-   **Linux:** Anweisungen Sie auf [Azure Storage-Clientbibliothek C++-Infodatei](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** Klicken Sie in Visual Studio auf **Tools > NuGet Paket-Manager > Paket-Manager Konsole**. Geben Sie folgenden Befehl in die [Konsole NuGet Paket-Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) und drücken Sie die **EINGABETASTE**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Konfigurieren Sie Ihre Anwendung auf BLOB-Speicher  
Fügen Sie folgende Anweisung am Anfang der Datei C++ enthalten soll den Azure-Speicher-APIs verwenden, um Blobs zuzugreifen:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher
Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und Anmeldeinformationen für den Zugriff auf Daten-Management-Services. Beim Ausführen in einer Clientanwendung müssen Sie Speicher Verbindungszeichenfolge im folgenden Format angeben, mit dem Namen Ihres Speicherkontos und Zugriffstaste Speicher für das Speicherkonto in [Azure-Portal](https://portal.azure.com) für *Kontoname* und *AccountKey* aufgeführt. Informationen zu Speicherkonten und Zugriffstasten anzeigen Sie [Zu Azure-Speicherkonten](storage-create-storage-account.md) Dieses Beispiel zeigt, wie Sie ein statisches Feld der Verbindungszeichenfolge zu deklarieren:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Zum Testen einer Anwendung in Ihren lokalen Computer können Sie Microsoft Azure [Speicheremulator](storage-use-emulator.md) verwenden, die mit dem [Azure SDK](https://azure.microsoft.com/downloads/)installiert ist. Der Speicheremulator ist ein Dienstprogramm, das die BLOB-Warteschlange und Tabelle Angebote in Azure auf Ihrem lokalen Entwicklungscomputer simuliert. Das folgende Beispiel zeigt, wie Sie ein statisches Feld Verbindungszeichenfolge die lokalen Speicheremulator zu deklarieren:

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Um den Azure-Speicher-Emulator zu starten, wählen Sie die Schaltfläche **Start** oder drücken Sie **Windows** . Geben Sie **Azure Speicheremulator**, und wählen Sie aus der Liste der Programme **Microsoft Azure Speicheremulator** .  

In den folgenden Beispielen davon ausgehen, dass Sie eine dieser beiden Methoden zu Speicher-Verbindungszeichenfolge verwendet haben.  

## <a name="retrieve-your-connection-string"></a>Abrufen der Verbindungszeichenfolge
Die **Cloud_storage_account** -Klasse können Sie Kontoinformationen Speicher darstellen. Rufen Sie Ihre speicherkontoinformationen aus der Verbindungszeichenfolge Storage können Sie die **parse** -Methode.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Als Nächstes rufen Sie einen Verweis auf eine **Cloud_blob_client** Klasse als können Sie Objekte abgerufen, die Container und Blobs innerhalb der BLOB-Speicher gespeichert. Der folgende Code erstellt ein **Cloud_blob_client** Objekt das Kontoobjekt wir über abgerufen:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Gewusst wie: Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Dieses Beispiel zeigt, wie Sie einen Container erstellen, wenn es nicht bereits vorhanden ist:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Standardmäßig der neue Container ist privat, und geben Sie den speicherzugriffsschlüssel herunterladen Blobs in diesem Container. Wenn Sie Dateien (Blobs) im Container für alle Benutzer verfügbar machen möchten, können Sie den Container öffentliche mithilfe des folgenden Codes festlegen:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Jeder im Internet sehen Blobs in einem öffentlichen Container jedoch ändern oder löschen, nur, wenn die entsprechende Taste.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Gewusst wie: Hochladen ein BLOBs in einem Container
Block-Blobs und Seitenblobs unterstützt Azure BLOB-Speicher. Block-Blob ist in den meisten Fällen den empfohlenen Typ.  

Zum Hochladen einer Datei auf ein Container bringen und einen Block BLOB-Verweis verwenden. Haben Sie eine BLOB-laden Stream von Daten, Sie durch Aufrufen der **Upload_from_stream** -Methode. Dieser Vorgang erstellt das Blob nicht bereits vorhanden, oder überschreiben sie vorhanden ist. Im folgenden Beispiel wird veranschaulicht, wie einen Blob in einem Container hochladen und geht davon aus, dass der Container bereits erstellt wurde.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

**Upload_from_file** -Methode können Sie auch Hochladen einer Datei auf ein.

## <a name="how-to-list-the-blobs-in-a-container"></a>Gewusst wie: Auflisten von Blobs in einem Container
Um die Blobs in einem Container sind, rufen Sie zunächst einen Containerverweis. **List_blobs** -Methode des Containers können Sie Blobs bzw. Verzeichnisse innerhalb abzurufen. Zugriff auf umfassende Eigenschaften und Methoden für das zurückgegebene **List_blob_item**müssen Sie die **list_blob_item.as_blob** Methode, um ein **Cloud_blob** -Objekt oder ein Cloud_blob_directory Objekt die **list_blob.as_directory** -Methode aufrufen. Der folgende Code veranschaulicht das Abrufen und ausgeben den URI der einzelnen Elemente im Container **Eigene Probenbehälter** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Weitere Informationen zum Auflisten von Vorgängen finden Sie unter [Liste Azure Speicherressourcen in C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Gewusst wie: Herunterladen von Blobs
Herunterladen von Blobs zunächst rufen Sie Blob Referenz ab und dann rufen Sie die Methode **Download_to_stream auf** . Das folgende Beispiel verwendet die Methode **Download_to_stream** BLOB-Inhalt in einem Stream-Objekt übertragen, die dann in eine lokale Datei beibehalten werden.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

**Download_to_file** -Methode können Sie auch den Inhalt eines BLOBs in eine Datei herunterladen.
Darüber hinaus auch können die **Download_text** -Methode Sie den Inhalt eines Blob als Textzeichenfolge herunterladen.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Gewusst wie: Löschen von Blobs
Zum Löschen eines BLOBs zunächst eine Blob Referenz, und rufen die Methode **Delete_blob** auf.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen der BLOB-Speicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr über Azure-Speicher.  

-   [Verwendung von C++ Warteschlangenspeicher](storage-c-plus-plus-how-to-use-queues.md)
-   [Verwendung von C++ Tabellenspeicher](storage-c-plus-plus-how-to-use-tables.md)
-   [Liste Azure Speicherressourcen in C++](storage-c-plus-plus-enumeration.md)
-   [Speicher-Clientbibliothek für C++-Referenz](http://azure.github.io/azure-storage-cpp)
-   [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
- [Datenübertragung mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)
