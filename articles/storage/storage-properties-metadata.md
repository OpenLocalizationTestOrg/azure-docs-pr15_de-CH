<properties
    pageTitle="Festlegen und Abrufen von Eigenschaften und Metadaten für Objekte in Azure Storage | Microsoft Azure"
    description="Speichern Sie benutzerdefinierten Metadaten auf Azure Storage und festlegen und Abrufen von Systemeigenschaften."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Festlegen und Abrufen von Eigenschaften und Metadaten #

## <a name="overview"></a>Übersicht

Objekte in Azure-Speicher unterstützt Eigenschaften und benutzerdefinierte Metadaten neben den Daten:

*   **Systemeigenschaften.** Systemeigenschaften vorhanden auf jeder Speicherressourcen. Einige können lesen oder festlegen, während andere schreibgeschützt. Bestimmte standard-HTTP-Header im Hintergrund entsprechen einige Systemeigenschaften. Die Azure-Speicher-Clientbibliothek verwaltet diese.  

*   **Benutzerdefinierte Metadaten.** Benutzerdefinierte Metadaten sind Metadaten, die auf eine bestimmte Ressource in Form von Name-Wert-Paar angeben. Metadaten können Sie zusätzliche Werte eine Speicherressource. Diese Werte sind für Ihre eigenen Zwecke und haben keinen Einfluss auf das Verhalten der Ressource.  

Eigenschaft und Metadaten Werte für eine Speicherressource ist ein zweistufiger Prozess. Bevor Sie diese Werte lesen können, müssen Sie explizit abrufen, durch Aufrufen der **FetchAttributes** -Methode.

> [AZURE.IMPORTANT] Werte-Eigenschaft und Metadaten für eine Speicherressource werden nicht aufgefüllt, wenn einer der **FetchAttributes** -Methoden aufrufen. 

## <a name="setting-and-retrieving-properties"></a>Festlegen und Abrufen von Eigenschaften

Zum Abrufen von Eigenschaftswerten **FetchAttributes** -Methode auf dem Blob oder Container Eigenschaften Auffüllen und dann die Werte.

Zum Festlegen von Eigenschaften für ein Objekt den Eigenschaftswert anzugeben und **SetProperties** -Methode aufrufen.

Im folgenden Codebeispiel wird einen Container erstellt und schreibt einige Eigenschaftswerte in einem Konsolenfenster:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Festlegen und Abrufen von Metadaten

Sie können Metadaten als ein oder mehrere Name-Wert-Paare Blob oder Container Ressource angeben. Legen Sie Metadaten **Metadatensammlung für die Ressource** Name-Wert-Paare hinzugefügt und die **SetMetadata** Methode um die Werte für den Dienst gespeichert.

> [AZURE.NOTE] Der Name der Metadaten muss die Namenskonventionen für C#-Bezeichner entsprechen.
 
Im folgenden Codebeispiel wird die Metadaten in einem Container. **Add** -Methode der Auflistung mit einem Wert festlegen. Der Wert wird mit impliziten Schlüsselwert Syntax festgelegt. Beide sind gültig.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Um Metadaten abzurufen, rufen Sie die **FetchAttributes** -Methode auf dem Blob oder Container zum Auffüllen der Auflistung **Metadaten** und Lesen Sie, wie im folgenden Beispiel gezeigt.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Siehe auch  

- [Azure-Speicher-Clientbibliothek for .NET Reference](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure Storage-Clientbibliothek für .NET Paket](https://www.nuget.org/packages/WindowsAzure.Storage/) 
