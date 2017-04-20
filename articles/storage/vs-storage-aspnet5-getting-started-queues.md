<properties
    pageTitle="Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (ASP.NET 5) | Microsoft Azure"
    description="Erste Schritte mit Azure Warteschlangenspeicher in einem ASP.NET 5-Projekt in Visual Studio"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-5"></a>Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

##<a name="overview"></a>Übersicht

Dieser Artikel beschreibt den Einstieg Azure Queue Storage in Visual Studio nach erstellt oder referenziert Azure Speicherkonto in einem ASP.NET 5-Projekt über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** . Operation **Verbundenen Dienste hinzufügen** installiert die entsprechenden NuGet-Pakete zum Azure-Speicher in Ihrem Projekt zugreifen und die Verbindungszeichenfolge für das Speicherkonto Projektdateien Konfiguration hinzugefügt.

Azure Warteschlangenspeicher ist ein Dienst zum Speichern von großen Anzahl von Nachrichten, die von überall auf der Welt über authentifizierte Aufrufe mit HTTP oder HTTPS zugegriffen werden können. Eine einzelne warteschlangennachricht kann bis zu 64 Kilobyte (KB) Groß und eine Warteschlange enthalten Millionen Nachrichten bis zu insgesamt maximal ein Speicherkonto.

Zunächst müssen Sie eine Azure-Warteschlange in das Speicherkonto erstellen. Wir zeigen Ihnen eine Warteschlange im Code erstellen. Wir werden auch wie grundlegende Warteschlangenoperationen, wie hinzufügen, ändern, lesen und Nachrichten in Warteschlange entfernen angezeigt. Die Beispiele sind in C# geschrieben\# code und Azure Storage-Clientbibliothek für .NET. Weitere Informationen zu ASP.NET finden Sie unter [ASP.NET](http://www.asp.net).

**Hinweis:** Einige der APIs, die Aufrufe von Azure-Speicher in ASP.NET 5 ausführen sind asynchron. Weitere Informationen finden Sie unter [asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Im folgenden Code wird angenommen, dass asynchrone Programmierung Methoden verwendet werden.

- Weitere Informationen zum programmgesteuerten Bearbeiten von Warteschlangen finden Sie unter [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md) .
- Allgemeine Informationen zum Azure-Speicher finden Sie unter [Dokumentation](https://azure.microsoft.com/documentation/services/storage/) .
- Allgemeine Informationen zu Azure Cloud Services finden Sie unter [Cloud Services-Dokumentation](https://azure.microsoft.com/documentation/services/cloud-services/) .
- Weitere Informationen über ASP.NET Applications programming finden Sie [ASP.NET](http://www.asp.net) .





##<a name="access-queues-in-code"></a>Zugriff auf Warteschlangen im code

Zugriff auf Warteschlangen in ASP.NET 5-Projekten müssen Sie eine C#-Quelldatei Folgendes, der Azure-Warteschlangenspeicher zugreift.

1. Sicherstellen Sie, dass die Namespacedeklarationen am oberen Rand der C#-Datei diese **Verwendung** enthalten.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Abzurufen Sie ein **CloudStorageAccount** -Objekt, Ihre speicherkontoinformationen darstellt. Verwenden Sie den folgenden Code zu dem Ihre Verbindungszeichenfolge Speicher und Speicher-Kontoinformationen von Azure-Dienstkonfiguration.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient** -Objekt auf Warteschlangenobjekte in das Speicherkonto.  

        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Ein **CloudQueue** -Objekt auf eine bestimmte Warteschlange zu erhalten.

        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Hinweis:** Verwenden Sie den obigen Code vor dem Code in den folgenden Beispielen.

###<a name="create-a-queue-in-code"></a>Erstellen einer Warteschlange im code

Zum Erstellen der Azure-Warteschlange im Code einfach fügen Sie einen Aufruf **CreateIfNotExistsAsync hinzu**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

##<a name="add-a-message-to-a-queue"></a>Hinzufügen einer Nachricht zu einer Warteschlange

Zum Einfügen einer Nachricht in einer vorhandenen Warteschlange ein neues **CloudQueueMessage** -Objekt erstellen und die **AddMessageAsync** -Methode aufrufen.

Ein **CloudQueueMessage** -Objekt kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Hier zeigt die Meldung "Hello, World" eingefügt.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

##<a name="read-a-message-in-a-queue"></a>Lesen von Nachrichten in einer Warteschlange

Sie können die Nachricht in einer Warteschlange einsehen, ohne durch Aufruf der **PeekMessageAsync** -Methode aus der Warteschlange entfernt.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


##<a name="read-and-remove-a-message-in-a-queue"></a>Lesen und eine Meldung in einer Warteschlange entfernen

Code kann entfernen (entfernen) einer Nachricht aus einer Warteschlange in zwei Schritten.
1. Rufen Sie **GetMessageAsync** die nächste Nachricht in einer Warteschlange zu. Eine Nachricht von **GetMessageAsync** wird zum Lesen von Nachrichten aus dieser Warteschlange Code unsichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden ausgeblendet.
2.  Rufen Sie abschließend die Nachricht aus der Warteschlange entfernt **DeleteMessageAsync**.

Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der folgende Code ruft **DeleteMessageAsync** rechts nach dem Verarbeiten der Meldung.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Nutzen Sie zusätzliche Optionen zum Abarbeiten der Warteschlange Nachrichten

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen.
Zunächst erhalten Sie Nachrichten (bis zu 32). Zweitens können längere oder kürzere unsichtbarkeit Timeout festlegen Code mehr oder weniger jede Nachricht vollständig zu ermöglichen. Im folgenden Codebeispiel verwendet die Methode **GetMessages** zu 20 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer **Foreach** -Schleife. Außerdem wird das unsichtbarkeit-Timeout 5 Minuten für jede Nachricht festgelegt. Hinweis 5 Minuten für alle Nachrichten zum gleichen Zeitpunkt beginnen nach 5 Minuten nach dem Aufruf von **GetMessages**, nicht gelöschte Nachrichten wieder sichtbar übergeben.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Länge der Warteschlange abrufen

Sie erhalten eine Schätzung der Anzahl von Nachrichten in einer Warteschlange. **FetchAttributes** -Methode fordert der Warteschlangendienst Warteschlange Attribute, einschließlich der Anzahl der Nachrichten abzurufen. Die **ApproximateMethodCount** -Eigenschaft gibt den letzten Wert der **FetchAttributes** -Methode ohne Aufrufen des warteschlangendiensts abgerufen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Verwenden Sie asynchrone Await-Muster mit gemeinsame Warteschlange APIs

Dieses Beispiel veranschaulicht das asynchrone Await-Muster mit gemeinsame Warteschlange APIs verwenden. Im Beispiel ruft die asynchrone Version der angegebenen Methoden. Das werden Async nach der Korrektur der jede Methode sehen. Wenn eine asynchrone Methode unterbricht asynchrone Await-Muster lokale Ausführung, bis der Aufruf abgeschlossen ist. Dieses Verhalten kann den aktuellen Thread andere Performance-Engpässe zu vermeiden und verbessert die allgemeine Reaktionsfähigkeit der Anwendung arbeiten. Weitere Informationen zur Verwendung des asynchrone Await-Musters in .NET finden Sie unter [Async und Await (C# und Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Delete** -Methode für das Warteschlangenobjekt.

    // Delete the queue.
    messageQueue.Delete();


##<a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
