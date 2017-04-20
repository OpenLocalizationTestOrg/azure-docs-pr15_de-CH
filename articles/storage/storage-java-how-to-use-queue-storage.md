<properties
    pageTitle="Verwendung von Java Warteschlangenspeicher | Microsoft Azure"
    description="Informationen Sie zum Verwenden des Azure-warteschlangendiensts erstellen und Löschen von Warteschlangen fügen Sie ein, abrufen Sie und löschen. Beispiele in Java geschrieben."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Verwendung von Java Warteschlangenspeicher

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Dabei wird mithilfe den Azure-Warteschlange Speicherdienst Szenarien durchführen angezeigt. Die Proben sind in Java geschrieben und [Azure Storage SDK für Java][]verwenden. Die Szenarios enthalten auch **Erstellen** und **Löschen von** Warteschlangen Nachrichten in Warteschlange **Einfügen**, **einsehen**, **Abrufen**und **Löschen** . Weitere Informationen zu Warteschlangen finden Sie im Abschnitt [Weiter](#Next-Steps) .

Hinweis: Eine SDK ist für Entwickler, Azure-Speicher auf Android-Geräte verfügbar. Weitere Informationen finden Sie in [Azure Storage SDK für Android][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen Sie eine Java-Anwendung

Verwenden Sie in diesem Handbuch Speicherfunktionen in einer Java-Anwendung lokal oder Code in einer Webrolle oder Worker-Rolle in Azure ausgeführt werden können.

Hierzu müssen Sie Java Development Kit (JDK) installiert und Azure-Speicher in der Azure-Abonnement registrieren. Nachdem Sie dies getan haben, müssen Sie überprüfen, ob Entwicklungssystem erfüllt die Mindestanforderungen und Abhängigkeiten in [Azure Storage SDK für Java][] Repository auf GitHub aufgeführt sind. Wenn Ihr System die Anforderungen können Sie herunterladen und Installieren von Azure Storage Bibliotheken für Java auf Ihrem System aus Repository befolgen. Nach Abschluss dieser Aufgaben werden Sie in den Beispielen in diesem Artikel verwendet eine Java-Anwendung erstellen.

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurieren Sie Ihre Anwendung auf Warteschlangenspeicher

Fügen Sie die folgende Import-Anweisung oben der Java-Datei mit Azure-Speicher-APIs auf Warteschlangen zugegriffen werden soll:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und Anmeldeinformationen für den Zugriff auf Daten-Management-Services. Beim Ausführen in einer Clientanwendung müssen Sie Speicher Verbindungszeichenfolge im folgenden Format angeben, mit dem Namen Ihres Speicherkontos und der primäre Schlüssel für das Speicherkonto in [Azure-Portal](https://portal.azure.com) für *Kontoname* und *AccountKey* aufgeführt. Dieses Beispiel zeigt, wie Sie ein statisches Feld der Verbindungszeichenfolge zu deklarieren:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In einer Anwendung eine Rolle in Microsoft Azure diese Zeichenfolge in die Dienstkonfigurationsdatei *ServiceConfiguration.cscfg*gespeichert werden und mit einem Aufruf der Methode **RoleEnvironment.getConfigurationSettings** zugegriffen werden. Hier ist ein Beispiel für das Abrufen der Verbindungszeichenfolge aus einer **Einstellung** Element mit dem Namen *StorageConnectionString* in der Service-Konfigurationsdatei:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

In den folgenden Beispielen davon ausgehen, dass Sie eine dieser beiden Methoden zu Speicher-Verbindungszeichenfolge verwendet haben.

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen einer Warteschlange

Ein **CloudQueueClient** -Objekt können Sie Referenzobjekte für Warteschlangen. Der folgende Code erstellt ein **CloudQueueClient** -Objekt. (Hinweis: Gibt es weitere Methoden zum Erstellen von **CloudStorageAccount** -Objekten; Weitere Informationen finden Sie unter **CloudStorageAccount** in [Azure Storage Client SDK-Referenz].)

Verwenden Sie das **CloudQueueClient** -Objekt zu einem Verweis auf die Warteschlange, die Sie verwenden möchten. Wenn vorhanden ist, können Sie die Warteschlange erstellen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Gewusst wie: hinzufügen eine Nachricht zu einer Warteschlange

Zum Einfügen einer Nachricht in eine vorhandene Warteschlange zuerst erstellen Sie einen neuen **CloudQueueMessage**. Anschließend rufen Sie die **AddMessage** -Methode. Eine **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden. Hier steht die Warteschlange erstellt (falls nicht vorhanden) und die Meldung "Hello, World".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Sie können die Nachricht in einer Warteschlange einsehen, ohne sie aus der Warteschlange von **PeekMessage**aufrufen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht direkt in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie diese Funktion, zum Aktualisieren des Status der Arbeitsaufgabe. Der folgende Code Message Queue mit neuen Inhalten aktualisiert und sichtbarkeitstimeout erweitern Sie weitere 60 Sekunden festgelegt. Dies speichert den Zustand der Arbeit, die der Meldung zugeordnet und erhält der Client eine Minute weiterhin auf die Nachricht. Dieses Verfahren können Sie Warteschlangennachrichten, ohne am Anfang beginnen durch Hardware-oder Softwarefehler schlägt einen mehrstufigen Workflows verfolgen. Normalerweise würde eine Wiederholungsanzahl sowie halten, und wenn die Nachricht mehr als *n* Mal wiederholt wird, möchten Sie es löschen. Dies schützt vor einer Nachricht, die einen Anwendungsfehler jedem Trigger verarbeitet wird.

Der folgende Code-Beispiel durchsucht die Warteschlange der Nachrichten, sucht nach der ersten Meldung, die mit "Hello, World" für den Inhalt und ändert die Nachrichten und beendet.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Im folgenden Beispiel aktualisiert auch nur die erste sichtbare Meldung in der Warteschlange.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen die Warteschlangenlänge

Sie erhalten eine Schätzung der Anzahl von Nachrichten in einer Warteschlange. Die **DownloadAttributes** -Methode fragt der Warteschlangendienst für mehrere aktuelle Werte, einschließlich der Anzahl der Nachrichten in der Warteschlange befinden. Die Anzahl ist nur ungefähre Nachrichten können hinzugefügt oder entfernt, nachdem der Warteschlangendienst auf die Anforderung reagiert. Die **GetApproximateMessageCount** -Methode gibt den letzten Wert durch den Aufruf von **DownloadAttributes**ohne des warteschlangendiensts abgerufen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Gewusst wie: Abrufen die nächste Nachricht

Der Code übergibt eine Nachricht aus einer Warteschlange in zwei Schritten. Wenn Sie **RetrieveMessage**aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Nachricht von **RetrieveMessage** wird zum Lesen von Nachrichten aus dieser Warteschlange Code unsichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden ausgeblendet. Um die Meldung aus der Warteschlange entfernt haben, müssen Sie auch **DeleteMessage**aufrufen. Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der Code ruft **DeleteMessage** rechts nach dem Verarbeiten der Meldung.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Zusätzliche Optionen zum Abarbeiten der Warteschlange Nachrichten

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen. Zunächst erhalten Sie Nachrichten (bis zu 32). Zweitens können längere oder kürzere unsichtbarkeit Timeout festlegen Code mehr oder weniger jede Nachricht vollständig zu ermöglichen.

Das folgende Codebeispiel verwendet die **RetrieveMessages** -Methode zu 20 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer **for** -Schleife. Außerdem wird das unsichtbarkeit-Timeout 5 Minuten (300 Sekunden) für jede Nachricht festgelegt. Beachten Sie, dass fünf Minuten für alle Nachrichten gleichzeitig, so als fünf Minuten vergangen seit dem Aufruf von **RetrieveMessages**, nicht gelöschte Nachrichten werden wieder sichtbar.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Gewusst wie: Auflisten von Warteschlangen

Um eine Liste der aktuellen Warteschlangen abzurufen, rufen Sie die **CloudQueueClient.listQueues()** -Methode, die eine Auflistung von **CloudQueue** -Objekten zurück.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen einer Warteschlange

Rufen Sie die **DeleteIfExists** -Methode um eine Warteschlange und alle darin enthaltenen Nachrichten löschen auf, auf das **CloudQueue** -Objekt.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren.

- [Azure-Speicher SDK für Java][]
- [Azure-Speicher Client SDK-Referenz][]
- [Azure-Speicherdienste REST-API][]
- [Azure-Speicher-Teamblog][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure-Speicher SDK für Java]: https://github.com/azure/azure-storage-java
[Azure-Speicher SDK für Android]: https://github.com/azure/azure-storage-android
[Azure-Speicher Client SDK-Referenz]: http://dl.windowsazure.com/storage/javadoc/
[Azure-Speicherdienste REST-API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
