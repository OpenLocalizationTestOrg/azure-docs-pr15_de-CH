<properties
    pageTitle="Erste Schritte mit Azure Queue Storage mit .NET | Microsoft Azure"
    description="Azure Warteschlangen bieten zuverlässigen, asynchrones messaging zwischen Anwendungskomponenten. Cloud-messaging ermöglicht die Anwendungskomponenten unabhängig voneinander zu skalieren."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Erste Schritte mit Azure Queue Storage mit .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Azure Queue Storage bietet Cloud messaging zwischen Anwendungskomponenten. Entwerfen Sie Applikationen für Skalierung, Anwendungskomponenten oft entkoppelt, damit sie unabhängig voneinander skaliert werden können. Warteschlangenspeicher bietet asynchrones messaging für die Kommunikation zwischen Komponenten, ob sie in der Cloud, auf dem Desktop auf einem lokalen Server oder auf einem mobilen Gerät ausgeführt werden. Warteschlangenspeicher unterstützt asynchrone Aufgaben und Arbeitsabläufe erstellen.

### <a name="about-this-tutorial"></a>Zu diesem Lernprogramm

Dieses Lernprogramm zeigt für einige häufige Szenarien mit Azure Warteschlangenspeicher .NET programmieren. Fallen Szenarien erstellen und Löschen von Warteschlangen hinzufügen, lesen und Löschen von Nachrichten in Warteschlange.

**Geschätzte Dauer:** 45 Minuten

**Dabei:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure-Speicher-Clientbibliothek für .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Ein [Azure Storage-Konto](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Namespacedeklarationen hinzuzufügen

Fügen Sie den folgenden `using` Aussagen am oberen Rand der `program.cs` Datei:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Erstellen Sie den Warteschlange Webdienstclient

**CloudQueueClient** -Klasse können Sie Warteschlangen in Warteschlangenspeicher gespeicherten abzurufen. Hier ist eine Möglichkeit, den Webdienstclient erstellen:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Jetzt können Sie Code schreiben, die Daten liest und schreibt Daten in Warteschlangenspeicher.

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Dieses Beispiel zeigt, wie eine Warteschlange erstellen, falls es nicht bereits vorhanden ist:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Legen Sie eine Nachricht in einer Warteschlange

Zum Einfügen einer Nachricht in eine vorhandene Warteschlange zuerst erstellen Sie einen neuen **CloudQueueMessage**. Anschließend rufen Sie die **AddMessage** -Methode. Eine **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein **Byte** -Array erstellt werden. Hier steht die Warteschlange erstellt (falls nicht vorhanden) und die Meldung "Hello, World":

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Einsehen der nächsten Nachricht

Sie können die Nachricht in einer Warteschlange einsehen, ohne sie aus der Warteschlange von **PeekMessage** aufrufen.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Ändern Sie den Inhalt der Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht direkt in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie diese Funktion, zum Aktualisieren des Status der Arbeitsaufgabe. Der folgende Code Message Queue mit neuen Inhalten aktualisiert und sichtbarkeitstimeout erweitern Sie weitere 60 Sekunden festgelegt. Dies speichert den Zustand der Arbeit, die der Meldung zugeordnet und erhält der Client eine Minute weiterhin auf die Nachricht. Dieses Verfahren können Sie Warteschlangennachrichten, ohne am Anfang beginnen durch Hardware-oder Softwarefehler schlägt einen mehrstufigen Workflows verfolgen. Normalerweise würde eine Wiederholungsanzahl sowie halten, und wenn die Nachricht mehr als *n* Mal wiederholt wird, möchten Sie es löschen. Dies schützt vor einer Nachricht, die einen Anwendungsfehler jedem Trigger verarbeitet wird.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Die nächste Nachricht Warteschlange

Code Warteschlange eine Nachricht aus einer Warteschlange in zwei Schritten. Wenn Sie **GetMessage**aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Nachricht von **GetMessage** unsichtbar Lesen von Nachrichten aus dieser Warteschlange Code. Standardmäßig bleibt diese Meldung 30 Sekunden ausgeblendet. Um die Meldung aus der Warteschlange entfernt haben, müssen Sie auch **DeleteMessage**aufrufen. Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der Code ruft **DeleteMessage** rechts nach dem Verarbeiten der Meldung.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Asynchrone Await-Muster mit gemeinsamen Warteschlangenspeicher APIs verwenden

Dieses Beispiel zeigt, wie asynchrone Await-Muster mit gemeinsamen Warteschlangenspeicher APIs verwendet. Im Beispiel ruft die asynchrone Version der angegebenen Methoden durch das Suffix *Async* jeder Methode. Wenn eine asynchrone Methode verwendet wird, die Async-erwarten Muster lokale Ausführung angehalten, bis der Aufruf abgeschlossen ist. Dieses Verhalten kann den aktuellen Thread andere Arbeiten fortsetzen Leistungsengpässe zu vermeiden und verbessert die allgemeine Reaktionsfähigkeit der Anwendung. Weitere Informationen zur Verwendung des asynchrone Await-Musters in .NET finden Sie unter [Async und Await (C# und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Nutzen Sie zusätzliche Optionen für die Warteschlange Nachrichten

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen.
Zunächst erhalten Sie Nachrichten (bis zu 32). Zweitens können längere oder kürzere unsichtbarkeit Timeout festlegen Code mehr oder weniger jede Nachricht vollständig zu ermöglichen. Im folgenden Codebeispiel verwendet die Methode **GetMessages** zu 20 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer **Foreach** -Schleife. Außerdem wird das unsichtbarkeit Zeitlimit fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass 5 Minuten für alle Nachrichten zum gleichen Zeitpunkt nach 5 Minuten übergeben, da der Aufruf von **GetMessages**, nicht gelöschte Nachrichten wieder sichtbar.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Länge der Warteschlange abrufen

Sie erhalten eine Schätzung der Anzahl von Nachrichten in einer Warteschlange. **FetchAttributes** -Methode fordert der Warteschlangendienst Warteschlange Attribute, einschließlich der Anzahl der Nachrichten abzurufen. Die **ApproximateMessageCount** -Eigenschaft gibt den letzten Wert der **FetchAttributes** -Methode ohne Aufrufen des warteschlangendiensts abgerufen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Delete** -Methode für das Warteschlangenobjekt.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren.

- Anzeigen der Warteschlange servicedokumentation Weitere Informationen zu verfügbaren APIs:
    - [Speicher-Clientbibliothek für .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)
- Informationen Sie zur Vereinfachung des Codes schreiben [Azure Webaufträge SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)mit Azure-Speicher arbeiten.
- Zeigen Sie weitere Features Leitfäden zusätzliche Optionen zum Speichern von Daten in Azure an
    - [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md) strukturierten Datenspeicher.
    - [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) unstrukturierte Daten speichern.
    - Mit relationalen Datenspeicher [mit SQL-Datenbank mit .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) .

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
