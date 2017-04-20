<properties
    pageTitle="Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (ASP.NET) | Microsoft Azure"
    description="Erste Schritte mit Azure Queue Storage in ein ASP.NET Projekt in Visual Studio nach dem Anschließen an ein Speicherkonto mit Visual Studio verbunden services"
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
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Erste Schritte mit Azure-Warteschlange speichern und Visual Studio verbunden Services

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie Einstieg Azure Queue Storage in Visual Studio verwenden, nachdem Sie erstellt oder referenziert ein Azure-Speicher-Konto in ein ASP.NET Projekt über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** .

Wir zeigen Ihnen erstellen und Zugriff auf eine Warteschlange Azure Ihres Speicherkontos. Wir werden auch wie grundlegende Warteschlangenoperationen, wie hinzufügen, ändern, lesen und Nachrichten in Warteschlange entfernen angezeigt. Die Beispiele sind in C# geschrieben und [Microsoft Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)verwenden. Weitere Informationen zu ASP.NET finden Sie unter [ASP.NET](http://www.asp.net).

Azure Queue Storage ist ein Dienst zum Speichern von großen Anzahl von Nachrichten, die von überall auf der Welt über authentifizierte Aufrufe mit HTTP oder HTTPS zugegriffen werden können. Eine einzelne warteschlangennachricht kann bis zu 64 KB und eine Warteschlange enthalten Millionen Nachrichten bis zu insgesamt maximal ein Speicherkonto.

## <a name="access-queues-in-code"></a>Zugriff auf Warteschlangen im code

Zugriff auf Warteschlangen in ASP.NET Projekte müssen Sie eine C#-Quelldatei Folgendes, der Azure-Warteschlange Speicher zuzugreifen.

1. Sicherstellen Sie, dass die Namespacedeklarationen am oberen Rand der C#-Datei diese **Verwendung** enthalten.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Abzurufen Sie ein **CloudStorageAccount** -Objekt, Ihre speicherkontoinformationen darstellt. Verwenden Sie den folgenden Code zu dem Ihre Verbindungszeichenfolge Speicher und Speicher-Kontoinformationen von Azure-Dienstkonfiguration.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudQueueClient** -Objekt auf Warteschlangenobjekte in das Speicherkonto.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Ein **CloudQueue** -Objekt auf eine bestimmte Warteschlange zu erhalten.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Hinweis** Verwenden Sie den obigen Code vor dem Code in den folgenden Beispielen.

## <a name="create-a-queue-in-code"></a>Erstellen einer Warteschlange im code

Zum Erstellen einer Azure-Warteschlange im Code nur fügen Sie einen Aufruf **CreateIfNotExists** Code oben hinzu

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Hinzufügen einer Nachricht zu einer Warteschlange

Zum Einfügen einer Nachricht in einer vorhandenen Warteschlange ein neues **CloudQueueMessage** -Objekt erstellen und die **AddMessage** -Methode aufrufen.

Ein **CloudQueueMessage** -Objekt kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Hier zeigt die Meldung "Hello, World" eingefügt.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Lesen von Nachrichten in einer Warteschlange

Sie können die Nachricht in einer Warteschlange einsehen, ohne durch Aufruf der PeekMessage()-Methode aus der Warteschlange entfernt.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lesen und eine Meldung in einer Warteschlange entfernen

Code entfernen kann (Warteschlange) eine Nachricht von einer Warteschlange in zwei Schritten.
1. Rufen Sie GetMessage() die nächste Nachricht in einer Warteschlange zu. Eine Nachricht von GetMessage() wird zum Lesen von Nachrichten aus dieser Warteschlange Code unsichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden ausgeblendet.
2.  Rufen Sie abschließend die Nachricht aus der Warteschlange entfernt **DeleteMessage**.

Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der folgende Code ruft **DeleteMessage** rechts nach dem Verarbeiten der Meldung.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Zusätzliche Optionen für Warteschlange Nachrichten verwenden

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen.
Zunächst erhalten Sie Nachrichten (bis zu 32). Zweitens können längere oder kürzere unsichtbarkeit Timeout festlegen Code mehr oder weniger jede Nachricht vollständig zu ermöglichen. Im folgenden Codebeispiel verwendet die Methode **GetMessages** zu 20 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer **Foreach** -Schleife. Außerdem wird das unsichtbarkeit Zeitlimit fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass 5 Minuten für alle Nachrichten zum gleichen Zeitpunkt nach 5 Minuten übergeben, da der Aufruf von **GetMessages**, nicht gelöschte Nachrichten wieder sichtbar.

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

Sie erhalten eine Schätzung der Anzahl von Nachrichten in einer Warteschlange. **FetchAttributes** -Methode fordert Queueservice Warteschlange Attribute, einschließlich Anzahl der Nachrichten abrufen. Die **ApproximateMethodCount** -Eigenschaft gibt den letzten Wert der **FetchAttributes** -Methode ohne die Queueservice abgerufen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Warten auf asynchrone Muster mit gemeinsamen queueAPIs

Dieses Beispiel zeigt, wie asynchrone Await-Muster mit gemeinsamen QueueAPIs. Im Beispiel ruft die asynchrone Version der angegebenen Methoden, dies sichtbar Async nach der Korrektur der einzelnen Methoden. Verwendung eine asynchronen Methode Async-erwarten Muster lokale Ausführung angehalten, bis der Aufruf abgeschlossen ist. Dieses Verhalten kann den aktuellen Thread andere Performance-Engpässe zu vermeiden und verbessert die allgemeine Reaktionsfähigkeit der Anwendung arbeiten. Weitere Informationen zur Verwendung des asynchrone Await-Musters in .NET finden Sie in [Async und Await (C# und Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Delete** -Methode für das Warteschlangenobjekt.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]