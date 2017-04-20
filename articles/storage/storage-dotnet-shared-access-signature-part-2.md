<properties
    pageTitle="Erstellen und Verwenden von SAS mit BLOB-Speicher | Microsoft Azure"
    description="Dieses Lernprogramm zeigt Ihnen, wie SAS für die Verwendung mit BLOB-Speicher erstellen und wie sie von Clientanwendungen verwendet."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Shared Access Signatures, Teil2: Erstellen und Verwenden von SAS mit BLOB-Speicher

## <a name="overview"></a>Übersicht

[Teil 1](storage-dotnet-shared-access-signature-part-1.md) dieses Lernprogramms untersucht shared Access Signaturen (SAS) und best Practices für deren Verwendung erläutert. Teil 2 wird das Generieren und dann SAS mit BLOB-Speicher veranschaulicht. Die Beispiele sind in C# geschrieben und Azure Storage-Clientbibliothek für .NET. Die Szenarios sind diese Aspekte der Arbeit mit SAS:

- Generieren von SAS für einen container
- Generieren von SAS auf ein blob
- Erstellen einer gespeicherten Zugriffsrichtlinie Signaturen des Containers Ressourcen verwalten
- Testen die SAS über eine Clientanwendung

## <a name="about-this-tutorial"></a>Zu diesem Lernprogramm

In diesem Lernprogramm konzentrieren wir uns auf SAS für Container und Blobs gegenseitige Konsole erstellen. Die erste Konsolenanwendung generiert SAS Container und Blob. Diese Anwendung kennt den speicherkontoschlüssel. Zweite Konsolenanwendung als Clientanwendung fungiert, greift auf Container und BLOB-Ressourcen mithilfe SAS mit der ersten Anwendung erstellt. Diese Anwendung verwendet die SAS nur für den Zugriff auf den Container zu authentifizieren und BLOB-Ressourcen – hat keine Kenntnisse über die kontoschlüssel.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Teil 1: Erstellen Sie eine SAS generieren

Zunächst sicherstellen Sie, dass die Clientbibliothek Azure Storage für .NET installiert haben. Sie können mit den aktuellsten Assemblys für die Clientbibliothek [NuGet-Paket](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-Paket") installieren. Dies ist die empfohlene Methode dafür, dass die neuesten Updates verfügen. Sie können auch die Clientbibliothek als Teil der neuesten Version von [Azure SDK für .NET](https://azure.microsoft.com/downloads/).

Erstellen Sie in Visual Studio eine neue Windows, und nennen Sie sie **GenerateSharedAccessSignatures**. Fügen Sie Verweise auf **Microsoft.WindowsAzure.Configuration.dll** und **Microsoft.WindowsAzure.Storage.dll**, mit einer der folgenden Methoden:

-   Wenn Sie das NuGet-Paket installieren möchten, zuerst installieren Sie [NuGet-Client](https://docs.nuget.org/consume/installing-nuget). Wählen Sie in Visual Studio **Projekt | NuGet-Pakete verwalten** **Azure Storage**online suchen und installieren Anleitung.
-   Alternativ suchen Sie die Assemblys in Azure SDK-Installation und fügen Sie Verweise hinzu.

Fügen Sie am Anfang der Datei Program.cs die folgende Anweisung **verwenden** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Bearbeiten Sie die App.config.Datei ist eine Einstellung mit einer Verbindungszeichenfolge, die auf das Speicherkonto. Die App.config.Datei sollte wie folgt aussehen:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Generieren einer SAS URI für einen Container

Zunächst fügen wir eine Methode zum Generieren der SAS in einen neuen Container. In diesem Fall ist die Signatur nicht gespeicherte Zugriffsrichtlinie zugeordnet, damit es den URI die Informationen enthält, die Ablaufzeit und die erteilten Berechtigungen angibt.

Zunächst fügen Sie Code **der Main()-Methode auf das Speicherkonto authentifizieren und einen neuen Container erstellen** :

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Fügen Sie eine neue Methode, die SAS für den Container und gibt die URI-Signatur an:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Fügen Sie die folgenden Zeilen am Ende der Methode **Main()** vor dem Aufruf von **Console.ReadLine()**, **GetContainerSasUri()** und Signatur URI im Konsolenfenster an:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Kompilieren Sie und führen Sie aus, um SAS URI für den neuen Container ausgeben. Der URI werden ähnlich dem folgenden URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Nach des Codes ausführen, die im Container erstellt SAS vierundzwanzig Stunden gelten. Die Signatur die Berechtigung einen Client Liste BLOBs im Container und dem Container ein neues Blob schreiben.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Generieren Sie SAS URI für ein Blob

Als Nächstes schreiben wir ähnlichen Code ein neues Blob im Container erstellen und generieren eine SAS dafür. Dieser SAS ist nicht gespeicherten Zugriffsrichtlinie zugeordnet, damit die Startzeit Ablaufzeit und Berechtigungsinformationen den URI enthält.

Fügen Sie eine neue Methode, die ein neues Blob erstellt und Schreiben Sie Text, generiert eine SAS und gibt die URI-Signatur:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Am Ende der Methode **Main()** fügen Sie folgende Zeilen zum Aufrufen von **GetBlobSasUri()**vor dem Aufruf von **Console.ReadLine()**und Schreiben Sie SAS URI im Konsolenfenster:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Kompilieren und SAS URI für neue Blob Ausgabe führen. Der URI werden ähnlich dem folgenden URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Erstellen einer gespeicherten Richtlinie für den Container

Jetzt im Container, die die Integritätsregeln für alle SAS definieren, die mit eine gespeicherten Richtlinie erstellen.

In den vorherigen Beispielen angegebenen wir die Startzeit (implizit oder explizit) die Ablaufzeit und die Berechtigungen auf SAS URI selbst. In den folgenden Beispielen werden wir die gespeicherte Zugriffsrichtlinie und nicht der SAS festgelegt. Dies ermöglicht, diese Sachzwänge ohne SAS erneut ausführen.

Es ist möglich, eine oder mehrere Beschränkungen SAS und der Rest der gespeicherten Richtlinie. Allerdings können Sie nur die Startzeit Ablaufzeit und Berechtigungen eine oder die andere angeben; Sie können nicht z. B. Geben Sie Berechtigungen für SAS und auch auf die gespeicherte Richtlinie angeben.

Beachten Sie beim Hinzufügen einer Richtlinie zu einem Container müssen Sie vorhandene Berechtigungen für den Container zu erhalten, fügen Sie die neue Richtlinie und legen Sie Berechtigungen für den Container.

Fügen Sie eine neue Methode, die eine neue gespeicherte Richtlinie für einen Container erstellt und gibt den Namen der Richtlinie:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

Am Ende der Methode **Main()** vor dem Aufruf von **Console.ReadLine()**fügen Sie folgende Zeilen zum ersten löschen alle vorhandenen Richtlinien und dann rufen Sie die Methode **CreateSharedAccessPolicy() auf** :    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Beachten Sie, dass beim Löschen der Richtlinien für einen Container müssen zuerst den Container vorhandenen Berechtigungen, dann deaktivieren Sie die Berechtigungen Sie legen Sie die Berechtigungen erneut.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Generieren einer SAS URI Container, der eine Zugriffsrichtlinie verwendet

Anschließend erstellen wir einen anderen SAS im Container, die wir zuvor, diesmal jedoch wir die Richtlinie die Signatur zugeordnet werden, die wir im vorherigen Beispiel erstellt erstellt.

Fügen Sie eine neue Methode zum Generieren von einem anderen SAS im Container:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Am Ende der Methode **Main()** vor dem Aufruf von **Console.ReadLine()**fügen Sie folgende Zeilen zum Aufrufen der **GetContainerSasUriWithPolicy** -Methode:

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Generiert eine URI Blob, das eine Zugriffsrichtlinie mithilfe SAS

Abschließend fügen wir eine ähnliche Methode zum Erstellen einer anderen Blob und eine SAS, die eine Richtlinie zugeordnet hat.

Fügen Sie eine neue Methode zum Erstellen eines BLOBs und eine SAS:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Am Ende der Methode **Main()** vor dem Aufruf von **Console.ReadLine()**fügen Sie folgende Zeilen zum Aufrufen der **GetBlobSasUriWithPolicy** -Methode:    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

Die Methode **Main()** sollte jetzt vollständig aussehen. Führen sie schreiben SAS URIs im Konsolenfenster kopieren und Einfügen einer Textdatei für die Verwendung im zweiten Teil dieses Lernprogramms.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Beim Ausführen der Anwendung GenerateSharedAccessSignatures Konsole sehen Sie eine Ausgabe ähnlich der folgenden im Konsolenfenster. Dies sind die SAS, die Sie in Teil2 des Lernprogramms verwenden.

![SAS-Konsole-Ausgabe-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Teil 2: Erstellen Sie eine zum Testen der SAS

Testen in den vorherigen Beispielen erstellt SAS, erstellen wir eine zweite Konsolenanwendung, die Signaturen im Container und Blob-Operationen verwendet.

> [AZURE.NOTE] Wenn mehr als 24 Stunden vergangen sind, seit Sie den ersten Teil des Lernprogramms erstellten Signaturen nicht mehr gelten. In diesem Fall führen Sie den Code in der ersten frischen SAS für die Verwendung im zweiten Teil des Tutoriums generiert.

Erstellen Sie in Visual Studio eine neue Windows, und nennen Sie sie **ConsumeSharedAccessSignatures**. Fügen Sie Verweise auf **Microsoft.WindowsAzure.Configuration.dll** und **Microsoft.WindowsAzure.Storage.dll**wie zuvor.

Fügen Sie am Anfang der Datei Program.cs die folgende Anweisung **verwenden** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Im Hauptteil der Methode **Main()** folgenden Konstanten hinzufügen und ihre Werte, die Sie in Teil 1 des Lernprogramms generiert SAS aktualisieren.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Hinzufügen einer Methode zum Container-Operationen mit einer SAS verwenden

Als Nächstes fügen wir eine Methode, die einige repräsentative Container Vorgänge mit einem SAS im Container testet. Beachten Sie, dass die SAS einen Verweis auf den Container im Container anhand der Signatur allein Netzwerkzugriffsauthentifizierung zurückzugeben.

"Program.cs" die folgende Methode hinzugefügt:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualisieren der Methode **Main()** **UseContainerSAS()** mit der SAS aufrufen, die im Container erstellt:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Hinzufügen einer Methode zum BLOB-Operationen mit einer SAS verwenden

Abschließend fügen wir eine Methode, die einige repräsentative Blob Vorgänge Blob ein SAS über testet. Verwenden Sie in diesem Fall den Konstruktor **CloudBlockBlob(String)**, SAS, übergeben einen Verweis auf das Blob zurückgegeben. Es ist keine Authentifizierung erforderlich; Es basiert auf der Signatur allein.

"Program.cs" die folgende Methode hinzugefügt:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualisieren der Methode **Main()** **UseBlobSAS()** mit der SAS aufrufen, der auf das Blob erstellt:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Führen Sie die Konsolenanwendung aus, und beobachten Sie die Ausgabe zu sehen, welche Vorgänge für die Signaturen zulässig sind. Die Ausgabe im Konsolenfenster wird wie folgt aussehen:

![SAS-Konsole-Ausgabe-2][sas-console-output-2]

## <a name="next-steps"></a>Nächste Schritte

[SAS, Teil 1: Grundlegendes zu SAS-Modell](storage-dotnet-shared-access-signature-part-1.md)

[Verwalten von anonymen Lesezugriff auf Container und blobs](storage-manage-access-to-resources.md)

[Delegieren des Zugriffs mit SAS (REST-API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Einführung in die Tabelle und SAS-Warteschlange](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
