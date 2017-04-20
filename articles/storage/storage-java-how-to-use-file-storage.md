<properties
    pageTitle="Verwendung von Java Dateispeicher | Microsoft Azure"
    description="Informationen Sie zum Verwenden des Azure-Dateidienst hochladen, herunterladen, Liste und löschen. Beispiele in Java geschrieben."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Verwendung von Java Dateispeicher

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Übersicht

In diesem Handbuch erfahren Sie, wie grundlegende Operationen auf der Microsoft Azure Speicher hat. In Java geschriebene Beispiele erfahren Sie, wie Erstellen von Freigaben und Verzeichnisse hochladen, auflisten und löschen. Wenn Sie mit Microsoft Azure Dateispeicher Service sind, werden durchlaufen die Konzepte in den folgenden Abschnitten sehr Verständnis der Beispiele.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen Sie eine Java-Anwendung

Um die Beispiele zu erstellen, benötigen Sie Java Development Kit (JDK) und [Azure Storage-SDK für Java] []. Sie sollten auch ein Azure Storage-Konto erstellt haben.

## <a name="setup-your-application-to-use-file-storage"></a>Einrichten der Anwendung Dateispeicher

Um den Azure-Speicher-APIs zu verwenden, fügen Sie die folgende Anweisung am Anfang der Datei Java Speicherdienst aus zugreifen möchten.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Um Dateispeicher verwenden, müssen Sie Ihre Azure Storage-Konto herstellen. Der erste Schritt wäre eine Verbindungszeichenfolge konfigurieren wir das Speicherkonto Verbindung verwenden. Eine statische Variable dafür definieren.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Ersetzen Sie Your_storage_account_name und Your_storage_account_key mit den tatsächlichen Werten für das Speicherkonto.

## <a name="connecting-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einer Azure Storage-Konto

Verbindung Ihres Speicherkontos müssen Sie das Objekt **CloudStorageAccount** verwenden und die **parse** -Methode eine Verbindungszeichenfolge übergeben.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** löst eine InvalidKeyException, so Sie in einem Try-Catch-Block eingefügt müssen.

## <a name="how-to-create-a-share"></a>Gewusst wie: Erstellen einer Freigabe

Alle Dateien und Verzeichnisse im Dateispeicher befinden sich in einem Container **Freigeben**. Das Speicherkonto haben Ihre Kapazität ermöglicht Aktie. Um Zugriff auf eine Freigabe erhalten, müssen Sie einen Datei Speicher-Client verwenden.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Mithilfe der Datei Speicher Clients dann erhalten einen Verweis auf eine Freigabe Sie.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Um die Freigabe zu erstellen, verwenden Sie die **CreateIfNotExists** Methode des CloudFileShare-Objekts.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

**Freigeben** enthält nun einen Verweis auf eine Freigabe mit dem Namen **Sampleshare**.

## <a name="how-to-upload-a-file"></a>Gewusst wie: Hochladen einer Datei

Ein Azure-Speicher Dateifreigabe enthält mindestens, ein Stammverzeichnis, befinden. In diesem Abschnitt lernen Sie das Hochladen einer Datei vom lokalen Speicher auf das Stammverzeichnis der Freigabe.

Der erste Schritt beim Hochladen einer Datei ist einen Verweis auf das Verzeichnis erhalten, sollten sie sich befindet. Dazu Aufrufen der **GetRootDirectoryReference** -Methode von Objekt freigeben.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Jetzt haben Sie einen Verweis auf das Stammverzeichnis der Freigabe, laden Sie eine Datei mit dem folgenden Code.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Gewusst wie: Erstellen eines Verzeichnisses

Speicher Organisieren von Dateien in Unterverzeichnissen anstatt alle im Stammverzeichnis. Azure Dateispeicherdienst können Sie Ihr Konto kann Verzeichnisse erstellen. Der folgende Code erstellt ein Unterverzeichnis mit dem Namen **Sampledir** im Stammverzeichnis.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Gewusst wie: Auflisten von Dateien und Verzeichnissen in einer Freigabe

Eine Liste der Dateien und Verzeichnisse in einer Freigabe erfolgt einfach durch Aufrufen von **ListFilesAndDirectories** auf CloudFileDirectory. Die Methode gibt eine Liste der ListFileItem Objekte auf durchlaufen werden können. Im folgende Code werden beispielsweise Dateien und Verzeichnisse im Stammverzeichnis aufgelistet.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Gewusst wie: Downloaden einer Datei

Häufiger Operationen gegen Datei ausführen werden heruntergeladen. Der Code im folgenden Beispiel SampleFile.txt downloads und sein Inhalt angezeigt.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Gewusst wie: Löschen einer Datei

Eine andere wird häufige Datei Speicher Datei löschen. Der folgende Code Löscht eine Datei namens SampleFile.txt im Verzeichnis **Sampledir**gespeichert.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Gewusst wie: Löschen eines Verzeichnisses

Löschen eines Verzeichnisses ist eine ziemlich einfache Aufgabe jedoch anzumerken ist, dass ein Verzeichnis mit noch Dateien oder andere Verzeichnisse löschen können.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Gewusst wie: löschen eine Freigabe

Löschen einer Freigabe erfolgt durch Aufrufen der **DeleteIfExists** -Methode für ein CloudFileShare-Objekt. Hier ist Beispielcode, der ausführt.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie weitere Informationen zu anderen Azure-Speicher-APIs, folgen Sie diesen Links.

- [Java Developer Center](http://azure.microsoft.com/develop/java/)
- [Azure-Speicher SDK für Java](https://github.com/azure/azure-storage-java)
- [Azure-Speicher SDK für Android](https://github.com/azure/azure-storage-android)
- [Azure-Speicher Client SDK-Referenz](http://dl.windowsazure.com/storage/javadoc/)
- [Azure-Speicherdienste REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
