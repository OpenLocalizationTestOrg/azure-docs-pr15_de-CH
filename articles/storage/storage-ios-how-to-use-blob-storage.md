<properties
    pageTitle="Wie Azure BLOB-Speicher von iOS | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud Azure BLOB-Speicher (Storage Objekt)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Verwendung von iOS-BLOB-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

In diesem Artikel zeigen Sie Szenarien mit Microsoft Azure BLOB-Speicher durchführen. Die Proben sind in Objective C geschrieben und der [Azure-Speicher-Clientbibliothek für iOS](https://github.com/Azure/azure-storage-ios). Behandelten Beispiele **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs. Weitere Informationen zu Blobs finden Sie im Abschnitt [Weiter](#next-steps) . Sie können auch die [Beispiel-app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) in iOS-Anwendung die Verwendung der Azure-Speicher schnell anzeigen.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Die Anwendung importieren Sie die Azure-Speicher iOS-Bibliothek

Sie können Azure Storage iOS-Bibliothek in der Anwendung mithilfe von [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) oder **Framework** -Datei importieren.

## <a name="cocoapod"></a>CocoaPod

1. Wenn Sie dies noch [CocoaPods installiert](https://guides.cocoapods.org/using/getting-started.html#toc_3) auf Ihrem Computer ein Terminalfenster öffnen und Ausführen des folgenden Befehls getan haben

        sudo gem install cocoapods

2. Im Projektverzeichnis (das Verzeichnis mit der `.xcodeproj` Datei), eine neue Datei namens `Podfile`(ohne Erweiterung). Fügen Sie den folgenden `Podfile` und speichern

        pod 'AZSClient'

3. Im terminal-Fenster zum Projektverzeichnis zu navigieren Sie, und führen Sie folgenden Befehl

        pod install

4. Wenn Ihre `.xcodeproj` in Xcode geöffnet ist, schließen Sie es. Die neu erstellte Projektdatei die im Projektverzeichnis Öffnen der `.xcworkspace` Erweiterung. Dies ist die Datei von für jetzt auf arbeiten Sie.

## <a name="framework"></a>Rahmen
Um den Azure-Speicher iOS-Bibliothek verwenden, müssen Sie zunächst die Frameworkdatei zu erstellen.

1. Zuerst herunterladen Sie oder Klonen Sie der [Azure-Speicher-Ios Repo](https://github.com/azure/azure-storage-ios).

2. In *Azure Storage Ios* -> *Lib* -> *Azure Storage-Clientbibliothek*und öffnen `AZSClient.xcodeproj` in Xcode.

3. Ändern Sie oben links Xcode aktive Schema von "Azure Storage Client Library", "Framework".

4. Erstellen Sie das Projekt (⌘ + B). Dadurch entsteht ein `AZSClient.framework` Datei auf Ihrem Desktop.

Framework-Datei können in der Anwendung Sie folgendermaßen:

1. Erstellen Sie ein neues Projekt oder öffnen Sie eines vorhandenen Projekts in Xcode.

2. Klicken Sie auf das Projekt in der linken Navigationsleiste und *Allgemein am oberen Rand der projekteditor* .

3. Klicken Sie auf die Schaltfläche hinzufügen (+) Abschnitt *verknüpfte Frameworks und Bibliotheken* .

4. Klicken Sie auf *hinzufügen...*. Navigieren zu und fügen die `AZSClient.framework` Datei, die Sie gerade erstellt haben.

5. Klicken Sie im Abschnitt *verknüpfter Frameworks und Bibliotheken* hinzufügen (+) erneut.

6. Suchen Sie in der Liste der Bibliotheken bereits `libxml2.2.dylib` und dem Projekt hinzugefügt.

7. Klicken Sie auf die *Buildeinstellungen* Registerkarte am oberen Rand der projekteditor.

8. Abschnitt *Suchpfade* *Framework Suchpfade* Doppelklicken Sie und fügen Sie den Pfad zu Ihrem `AZSClient.framework` Datei.

## <a name="import-statement"></a>Import-Anweisung
Sie müssen die folgenden Import-Anweisung in der Datei enthalten die Azure-API aufgerufen werden soll.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynchrone Vorgänge
> [AZURE.NOTE] Alle Methoden, die eine für den Dienst Anforderung sind asynchrone Vorgänge. In den Codebeispielen finden diese Methoden einen Handler abgeschlossen haben. Code im Handler Abschluss läuft **nach** , wenn die Anforderung abgeschlossen ist. Code nach Abschlusshandler **während** die Anforderung läuft erfolgt.

## <a name="create-a-container"></a>Erstellen eines Containers
Jedes Blob in Azure-Speicher muss in einem Container befinden. Im folgenden Codebeispiel wird einen Container namens *Newcontainer*in das Speicherkonto vorhanden nicht erstellen. Wenn Sie einen Namen für den Container auswählen, achten Sie auf genannten Benennungskonventionen.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Sie können bestätigen, dass dies [Microsoft Azure Storage Explorer funktioniert](http://storageexplorer.com) und überprüfen, ob in der Liste der Container für das Speicherkonto, *Newcontainer* ist.

## <a name="set-container-permissions"></a>Festlegen der Berechtigungen für Container
Ein Container Berechtigungen sind standardmäßig für **Private** -Zugriff konfiguriert. Container bieten jedoch eine Reihe von Optionen für den Containerzugriff:

- **Privat**: Container und BLOB-Daten vom Kontoinhaber nur gelesen werden.

- **BLOB**: BLOB-Daten in diesem Container lesen Sie über anonyme Anforderung jedoch Containerdaten sind nicht verfügbar. Clients können nicht Blobs im Container über anonyme Anforderung aufgelistet werden.

- **Container**: Container und BLOB-Daten über anonyme Anforderung gelesen werden können. Clients können Blobs im Container über anonyme Anforderung aufzählen, aber das Speicherkonto Container können nicht aufgelistet werden.

Im folgende Beispiel veranschaulicht das Erstellen eines Containers mit Zugriffsberechtigungen **Container** öffentlichen schreibgeschützten Zugriff für alle Benutzer im Internet ermöglicht:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen
Wie in Abschnitt [BLOB-Konzepte](#blob-service-concepts) , BLOB-Speicher bietet drei verschiedene Blobs: blockieren Blobs, Anhängen Blobs und Seite Blobs. Derzeit unterstützt die Azure-Speicher iOS-Bibliothek nur Block-Blobs. Block-Blob ist in den meisten Fällen den empfohlenen Typ.

Im folgenden Beispiel wird veranschaulicht, wie ein Blockblob aus einer NSString hochladen. Wenn ein Blob mit demselben Namen bereits im Container vorhanden ist, wird der Inhalt dieses BLOB überschrieben.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Sie können bestätigen, dass dies [Microsoft Azure Storage Explorer funktioniert](http://storageexplorer.com) und ob der Container *Containerpublic*BLOBs, die *Sampleblob*enthält. In diesem Beispiel verwendet haben wir einen öffentlichen Container um auch zu überprüfen, dass dies funktioniert auf Blobs URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Neben Hochladen einer Block-Blob aus einem NSString bestehen ähnliche Methoden für NSData, NSInputStream oder einer lokalen Datei.

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container
Im folgenden Beispiel wird veranschaulicht, wie alle Blobs in einem Container. Bei diesem Vorgang, achten Sie auf die folgenden Parameter:     

- **ContinuationToken** - stellt das Fortsetzungstoken beginnt der Vorgang Angebot. Sofern kein Token listet es Blobs ab. Eine beliebige Anzahl von Blobs kann aufgeführt, von null bis maximum festgelegt. Auch wenn diese Methode keine Ergebnisse zurückgibt wenn `results.continuationToken` ist nicht NULL, möglicherweise weitere Blobs für den Dienst nicht aufgeführt.
- **Präfix** - Präfix für BLOB-Angebot verwenden soll. Nur mit diesem Präfix beginnen Blobs werden aufgelistet.
- **UseFlatBlobListing** - wie im Abschnitt [Naming verweisenden Container und Blobs](#naming-and-referencing-containers-and-blobs) BLOB-Dienst eine flache Lagerhaltung ist Sie virtuelle Hierarchie können durch Benennung Blobs Pfadinformationen. Keine flache Liste ist jedoch derzeit nicht unterstützt. Dies kommt bald. Jetzt sollte dieser Wert sein.`YES`
- **BlobListingDetails** - welche Elemente beim Blobs enthalten soll
    - `AZSBlobListingDetailsNone`: Liste Commit Blobs und BLOB-Metadaten nicht zurück.
    - `AZSBlobListingDetailsSnapshots`: Liste Commit Blobs und BLOB-Snapshots.
    - `AZSBlobListingDetailsMetadata`: Abrufen Blob Metadaten für jedes Blob in der Auflistung zurückgegeben.
    - `AZSBlobListingDetailsUncommittedBlobs`: Engagiert und nicht festgeschriebene Blobs auflisten
    - `AZSBlobListingDetailsCopy`: Enthalten Sie Eigenschaften kopieren in der Liste.
    - `AZSBlobListingDetailsAll`: Liste aller verfügbaren Commit Blobs, nicht festgeschriebene Blobs und Snapshots und alle Metadaten und Kopie-Status für die Blobs zurück.
- **MaxResults** - die maximale Anzahl der Ergebnisse für diese Operation zurückgegeben. Mit ­1 können Sie keinen Grenzwert festgelegt.
- **CompletionHandler** - Codeblock mit Angebot Vorgang ausführen.

In diesem Beispiel wird eine Hilfsmethode mit rekursiv Aufruf der Liste blobs Methode jedes Mal ein Fortsetzungstoken zurückgegeben wird.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Einen Blob herunterladen

Im folgenden Beispiel wird veranschaulicht, wie einen Blob ein Objekt NSString herunterladen.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Im folgende Beispiel veranschaulicht, wie einen Blob löschen.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Einen BLOB-Container löschen

Das folgende Beispiel veranschaulicht die Container löschen.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Nächste Schritte

Nun, wie BLOB-Speicher von iOS beherrschen, folgen Sie diesen Links erfahren Sie mehr über die iOS-Bibliothek und der Speicherdienst.

- [Azure Storage-Clientbibliothek für iOS](https://github.com/azure/azure-storage-ios)
- [Azure Storage iOS Dokumentation](http://azure.github.io/azure-storage-ios/)
- [Azure-Speicherdienste REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage)

Haben Sie Fragen zu dieser Bibliothek gerne [MSDN Azure Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) oder [Stapelüberlauf](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files)buchen.
Haben Sie Feature-Vorschläge für Azure Storage, Buchen Sie Feedback der [Azure](https://feedback.azure.com/forums/217298-storage/)-Speicher.
