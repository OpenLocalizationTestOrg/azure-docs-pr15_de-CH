<properties
    pageTitle="Verwendung von C++ Dateispeicher | Microsoft Azure"
    description="Speichern Sie Daten in der Cloud mit Azure-Datei."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Verwendung von C++ Dateispeicher

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Zu diesem Lernprogramm

In diesem Lernprogramm erfahren Sie, wie grundlegende Operationen auf der Microsoft Azure Speicher hat. Beispiele in C++ geschrieben erfahren Sie, wie Erstellen von Freigaben und Verzeichnisse hochladen, auflisten und löschen. Wenn Sie mit Microsoft Azure Dateispeicher Service sind, werden durchlaufen die Konzepte in den folgenden Abschnitten sehr Verständnis der Beispiele.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendung

Um die Beispiele zu erstellen, müssen Sie installieren Azure Storage-Clientbibliothek 2.4.0 für C++. Sie sollten auch ein Azure Storage-Konto erstellt haben.

Um Azure Storage Client 2.4.0 für C++ installieren, können Sie eine der folgenden Methoden:

-   **Linux:** Anweisungen Sie auf [Azure Storage-Clientbibliothek C++-Infodatei](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .

-   **Windows:** Klicken Sie in Visual Studio auf **Tools &gt; NuGet Paket-Manager &gt; Paket-Manager Konsole**. Geben Sie folgenden Befehl in die [Konsole NuGet Paket-Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) und drücken Sie die **EINGABETASTE**.

    Install-Package wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Richten Sie die Anwendung für Dateispeicher

Fügen Sie folgende Anweisung am Anfang der Datei C++ enthalten soll den Azure-Speicher-APIs verwenden, um Dateien zugreifen:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Richten Sie eine Verbindungszeichenfolge Azure-Speicher

Um Dateispeicher verwenden, müssen Sie Ihre Azure Storage-Konto herstellen. Der erste Schritt wäre eine Verbindungszeichenfolge konfigurieren wir das Speicherkonto Verbindung verwenden. Eine statische Variable dafür definieren.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einer Azure Storage-Konto

Die **Cloud_storage_account** -Klasse können Sie Kontoinformationen Speicher darstellen. Rufen Sie Ihre speicherkontoinformationen aus der Verbindungszeichenfolge Storage können Sie die **parse** -Methode.

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Gewusst wie: Erstellen einer Freigabe

Alle Dateien und Verzeichnisse im Dateispeicher befinden sich in einem Container **Freigeben**. Das Speicherkonto haben Ihre Kapazität ermöglicht Aktie. Um Zugriff auf eine Freigabe erhalten, müssen Sie einen Datei Speicher-Client verwenden.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Mithilfe der Datei Speicher Clients dann erhalten einen Verweis auf eine Freigabe Sie.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Um die Freigabe zu erstellen, verwenden Sie die **Create_if_not_exists** Methode des **Cloud_file_share** -Objekts.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Zu diesem Zeitpunkt enthält **gemeinsam** auf einer Freigabe namens **Mein Beispiel freigeben**.

## <a name="how-to-upload-a-file"></a>Gewusst wie: Hochladen einer Datei

Zumindest enthält eine Azure Storage Dateifreigabe ein Stammverzeichnis, in dem Dateien gespeichert werden. In diesem Abschnitt lernen Sie das Hochladen einer Datei vom lokalen Speicher auf das Stammverzeichnis der Freigabe.

Der erste Schritt beim Hochladen einer Datei ist einen Verweis auf das Verzeichnis erhalten, sollten sie sich befindet. Dazu Aufrufen der **Get_root_directory_reference** -Methode von Objekt freigeben.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Jetzt haben Sie einen Verweis auf das Stammverzeichnis der Freigabe können Sie eine Datei hochladen. In diesem Beispiel lädt eine Datei, Text und einen Stream.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Gewusst wie: Erstellen eines Verzeichnisses

Speicher Organisieren von Dateien in Unterverzeichnissen anstatt alle im Stammverzeichnis. Azure Dateispeicherdienst können Sie Ihr Konto kann Verzeichnisse erstellen. Im folgenden Code wird ein Verzeichnis **Eigene Beispielverzeichnis** das Stammverzeichnis sowie ein Unterverzeichnis namens **Mein Beispiel Unterverzeichnis**erstellen.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Gewusst wie: Auflisten von Dateien und Verzeichnissen in einer Freigabe

Eine Liste der Dateien und Verzeichnisse in einer Freigabe erfolgt einfach durch Aufrufen von **List_files_and_directories** auf **Cloud_file_directory** . Zugriff auf umfassende Eigenschaften und Methoden für das zurückgegebene **List_file_and_directory_item**müssen Sie die **list_file_and_directory_item.as_file** Methode, um ein **Cloud_file** -Objekt oder ein **Cloud_file_directory** Objekt die **list_file_and_directory_item.as_directory** -Methode aufrufen.

Der folgende Code veranschaulicht die abrufen und ausgeben den URI der einzelnen Elemente im Stammverzeichnis der Freigabe.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Gewusst wie: Downloaden einer Datei

Zum Herunterladen von Dateien zunächst abgerufen Sie einen Dateiverweis und dann die **Download_to_stream** -Methode, um den Inhalt der Datei auf ein Datenstromobjekt übertragen die dann zu einer lokalen Datei beibehalten werden. **Download_to_file** -Methode können Sie auch den Inhalt einer Datei in eine lokale Datei herunterladen. **Download_text** -Methode können Sie den Inhalt einer Datei als Textzeichenfolge herunterladen.

Das folgende Beispiel verwendet die Methoden **Download_to_stream** und **Download_text** zeigen die Dateien in früheren Abschnitten erstellt wurden.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Gewusst wie: Löschen einer Datei

Eine andere wird häufige Datei Speicher Datei löschen. Der folgende Code Löscht eine Datei namens my-Sample-Datei-3 unter dem Stammverzeichnis gespeichert.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Gewusst wie: Löschen eines Verzeichnisses

Löschen eines Verzeichnisses ist eine einfache Aufgabe, obwohl anzumerken ist, dass ein Verzeichnis mit noch Dateien oder andere Verzeichnisse löschen können.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Gewusst wie: löschen eine Freigabe

Löschen einer Freigabe erfolgt durch Aufrufen der **Delete_if_exists** -Methode für ein Cloud_file_share-Objekt. Hier ist Beispielcode, der ausführt.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Die maximale Größe für eine Dateifreigabe

Kontingent (oder maximale Größe) können für eine Dateifreigabe in Gigabyte festgelegt werden. Sie können auch überprüfen, auf die gespeicherten Datenmenge.

Kontingent für eine Freigabe festlegen, können Sie die Gesamtgröße der Dateien in der Freigabe einschränken. Wenn die Gesamtgröße der Dateien in der Freigabe auf die festgelegte Kontingent überschreitet, dann können Clients Dateien vergrößern oder neue Dateien erstellen, wenn diese Dateien leer sind.

Das folgende Beispiel zeigt, wie die aktuelle Verwendung für eine Freigabe überprüfen und das Kontingent für die Freigabe.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generieren Sie SAS für eine Datei oder eine Dateifreigabe

Sie können eine SAS (SAS) für eine Dateifreigabe oder eine einzelne Datei. Eine freigegebene Richtlinie erstellen Sie auf einer Dateifreigabe zu SAS. Erstellen einer Richtlinie für gemeinsamen Zugriff wird empfohlen, wie eine bestimmte SAS widerrufen, wenn es manipuliert.

Das folgende Beispiel erstellt eine Richtlinie für gemeinsamen Zugriff auf eine Freigabe und dieser Richtlinie zu den Nebenbedingungen für SAS für eine Datei in der Freigabe verwendet.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Weitere Informationen zum Erstellen und Verwenden von SAS finden Sie unter [Verwendung von gemeinsamen Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie erfahren Sie mehr über Azure-Speicher:

-   [Speicher-Clientbibliothek für C++](https://github.com/Azure/azure-storage-cpp)

-   [Azure-Speicher-Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
