<properties
    pageTitle="Verarbeiten IoT Hub (.Net) Gerät Cloud-Nachrichten | Microsoft Azure"
    description="Dieses Lernprogramm erfahren Sie nützliche Muster IoT Hub Gerät Cloud-Nachrichten verarbeiten."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Exemplarische Vorgehensweise: IoT Hub Gerät Cloud-Nachrichten mit .net verarbeiten

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Einführung

Azure IoT Hub einen komplett verwalteten Service, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und eine Anwendung beenden. Andere Lernprogramme ([mit IoT Hub] und [Cloud-zu-Gerät-Nachrichten mit IoT Hub][lnk-c2d]) veranschaulichen die grundlegende Gerät Cloud und Cloud Gerät Messagingfunktionen IoT Hub verwenden.

Dieses Lernprogramm baut auf den Code im Lernprogramm [Erste Schritte mit IoT Hub] und zeigt zwei skalierbare Muster mit Gerät Cloud-Nachrichten verarbeiten:

- Zuverlässige Speicherung Gerät Cloud-Nachrichten in [Azure BLOB-Speicher]. Häufig wird *kalte Pfad* Analytics speichern Telemetriedaten Blobs als Eingabe in Analytics Prozesse verwenden. Diese Prozesse können mit Tools wie [Azure Data Factory] oder Stapel [HDInsight (Hadoop)] gesteuert werden.

- Zuverlässige *interaktive* Gerät Cloud-Nachrichten. Gerät-zu-Cloud-Nachrichten sind interaktiv, wird sofort Trigger für eine Reihe von Aktionen in der Anwendung Back-End. Beispielsweise kann ein Gerät eine Alarm senden, die ein Ticket in einem CRM-System eingefügt wird. Dagegen feed Nachrichten *Daten* einfach in einem Analyse-Engine. Temperatur Telemetriedaten aus ein Gerät, das für die spätere Analyse gespeichert ist z. B. eine Nachricht zeigen.

Da IoT Hub ein [Ereignis Hub]macht[lnk-event-hubs]-kompatiblen Endpunkt Gerät Cloud Nachrichten dieses Lernprogramms verwendet eine [EventProcessorHost] -Instanz erhalten. Diese Instanz:

* Zuverlässige speichert *Daten* Nachrichten in Azure BLOB-Speicher.
* Leitet *interaktive* Gerät Cloud-Nachrichten mit einer Azure [Service Bus-Warteschlange] zur unmittelbaren Verarbeitung.

Service Bus trägt zuverlässige interaktive Nachrichten pro Nachricht und das Mal Fenster-basierte Deduplizierung bietet.

> [AZURE.NOTE] Eine **EventProcessorHost** -Instanz ist nur eine Möglichkeit, interaktive Nachrichten verarbeiten. [Azure Service Fabric] sind[ lnk-service-fabric] und [Azure Stream Analytics][lnk-stream-analytics].

Am Ende dieser praktischen Einführung führen Sie drei Windows-Konsole apps.

* **SimulatedDevice**, eine geänderte Version der Anwendung erstellt im [Einstieg IoT Hub] Lernprogramm sendet Daten zeigen Gerät Cloud Nachrichten jeder zweite und interaktive Gerät Cloud-Nachrichten alle 10 Sekunden. Diese Anwendung verwendet das Protokoll AMQP Kommunikation mit IoT Hub.
* **ProcessDeviceToCloudMessages** verwendet die [EventProcessorHost] -Klasse zum Abrufen von Nachrichten vom Ereignis Hub-kompatiblen Endpunkt. Dann zuverlässig speichert Daten zeigen Sie Nachrichten in Azure BLOB-Speicher, und interaktive Nachrichten an eine Warteschlange Service Bus weiterleitet.
* **ProcessD2CInteractiveMessages** Warteschlange interaktiven Nachrichten aus der Warteschlange Service Bus.

> [AZURE.NOTE] IoT Hub unterstützt SDK für zahlreiche Plattformen und Sprachen, einschließlich C#, Java und JavaScript. Wie ein physisches Gerät das simulierte Gerät in diesem Lernprogramm ersetzen und IoT Hub Geräten finden Sie unter [Azure IoT Developer Center].

Dieses Lernprogramm ist auf andere Weise Ereignis Hub-kompatiblen Nachrichten, wie [HDInsight (Hadoop)] Projekte. Weitere Informationen finden Sie im [Entwicklerhandbuch Azure IoT Hub - Gerät in die cloud].

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Ein aktives Azure-Konto. <br/>Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) in wenigen Minuten erstellen.

Sie müssen einige grundlegende Kenntnisse der [Azure-Speicher] und [Azure Service Bus].


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktive Nachrichten von einem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierten geräteanwendung erstellten im Lernprogramm [Erste Schritte mit IoT Hub] IoT Hub interaktive Gerät Cloud-Nachrichten an.

1. Fügen Sie die folgende Methode in Visual Studio im Projekt **SimulatedDevice** an der **Program** -Klasse.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Diese Methode ist vergleichbar mit der **SendDeviceToCloudMessagesAsync** -Methode im **SimulatedDevice** -Projekt. Die einzigen Unterschiede sind, dass Sie die Systemeigenschaft **MessageId** festlegen und eine Benutzereigenschaft **MessageType**.
    Der Code der **MessageId** -Eigenschaft einen global eindeutigen Bezeichner (GUID) zugewiesen. Dieser Bezeichner können Service Bus Deduplizierung die Nachrichten, die er erhält. Der Beispielcode verwendet die **MessageType** -Eigenschaft interaktive Daten zeigen Nachrichten unterscheiden. Die Anwendung wird hierzu Nachrichteneigenschaften statt in den Nachrichtentext, sodass der Ereignisprozessor nicht deserialisiert die Meldung Nachrichtenrouting durchführen muss.

    > [AZURE.NOTE] Es ist wichtig zum interaktive Nachrichten in der Gerätecode deduplizieren **MessageId** erstellen. Zeitweise Netzwerkkommunikation oder andere Fehler führen mehrere Retransmissionen dieselbe Nachricht dieses Geräts. Sie können auch eine semantische Nachrichten-ID wie ein Hash der Datenfelder entsprechende Meldung anstelle einer GUID verwenden.

2. Fügen Sie die folgende Methode in die **Main** -Methode vor der `Console.ReadLine()` Zeile:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Der Einfachheit halber implementiert dieses Lernprogramm keine Richtlinien wiederholen. Implementieren Sie im Produktionscode eine wiederholungsrichtlinie wie exponentielle Backoff wie im MSDN-Artikel [Vorübergehende Fehler behandeln].

## <a name="process-device-to-cloud-messages"></a>Prozess Gerät Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Windows-Konsole app, die Gerät zu Cloud IoT Hub Nachrichten verarbeitet. IOT Hub macht [Ereignis-Hub]-kompatiblen Endpunkt um eine Anwendung Gerät Cloud-Nachrichten lesen. Diese praktische Einführung verwendet die [EventProcessorHost] -Klasse verarbeitet diese Nachrichten in einer Konsole app. Weitere Informationen über das Verarbeiten von Nachrichten Ereignis Hubs finden Sie im Lernprogramm [Beginnen mit der Ereignis-Hubs] .

Die Herausforderung beim Implementieren zuverlässigen Speicher Datenpunkt Nachrichten oder Weiterleiten von interaktiven Nachrichten diese Verarbeitung wird basiert auf Meldungsconsumer zu Prüfpunkte für den Fortschritt. Darüber hinaus zu einem hohen Durchsatz beim Lesen von Ereignis soll Prüfpunkte in großen Mengen. Dieser Ansatz schafft die Möglichkeit, doppelte Verarbeitung für eine große Anzahl von Nachrichten, wenn ein Fehler vorliegt und Sie vorherigen Prüfpunkt zur. In diesem Lernprogramm erfahren Sie, wie Azure-Speicher schreibt und Service Bus Deduplizierung Windows mit **EventProcessorHost** Prüfpunkte synchronisieren.

Um Nachrichten zuverlässig in Azure-Speicher schreiben, verwendet das Beispiel einzelner Block Commit Feature [Block]BLOBs[Azure Block Blobs]. Der Ereignisprozessor sammelt Nachrichten im Arbeitsspeicher, bis einen Prüfpunkt bereitzustellen ist. Nachdem der kumulierte Puffer Nachrichten die maximale Größe von 4 MB erreicht oder Service Bus Deduplizierung läuft beispielsweise Zeitfenster. Dann verpflichtet vor dem Prüfpunkt der Code einen neuen Block Blob.

Der Ereignisprozessor wird Ereignis Hubs Nachricht offsets als Block-IDs verwendet. Dieser Mechanismus ermöglicht das Ereignis eine Prüfung Deduplizierung durchzuführen, bevor neuen Speicher eines eventuellen Absturzes Commit zwischen Commit einen Block und der Prüfpunkt kümmern.

> [AZURE.NOTE] Diese praktische Einführung verwendet ein Einzelkonto Azure Storage schreiben alle Nachrichten IoT Hub entnommen. Entscheidung, mehrere Azure-Speicherkonten in einer Projektmappe verwenden möchten, finden Sie unter [Azure-Speicher skalierbar Richtlinien].

Die Anwendung verwendet die Funktion Service Bus Deduplizierung um Duplikate zu vermeiden, wenn sie interaktive Nachrichten verarbeitet. Das simulierte Gerät versieht jede interaktive Meldung mit eindeutigen **MessageId**. Diese IDs können Service-Bus, um sicherzustellen, dass keine zwei Nachrichten mit der gleichen **MessageId** im Zeitfenster angegebenen Deduplizierung an Empfänger übermittelt werden. Diese Deduplizierung mit der bereitgestellten Service Bus-Warteschlangen Nachrichten Abschluss Semantik erleichtert die zuverlässige interaktive Nachrichten implementieren.

Um sicherzustellen, dass keine Nachricht außerhalb der Deduplizierung erneut synchronisiert der Code **EventProcessorHost** Checkpoint-Mechanismus mit Service Bus Warteschlange Deduplizierung Fenster. Diese Synchronisation erfolgt einmal erzwingen einen Checkpoint jedem Fenster Deduplizierung abläuft (in diesem Lernprogramm ist das Fenster eine Stunde).

> [AZURE.NOTE] Diese praktische Einführung verwendet eine einzelne partitionierte Service Bus-Warteschlange Prozess IoT Hub interaktiven Nachrichten entnommen. Weitere Informationen zur Verwendung von Servicebuswarteschlangen Skalierbarkeitsanforderungen Ihrer Lösung zu Dokumentation der [Azure Service Bus] .

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Bereitstellen einer Azure Storage-Konto und Service Bus-Warteschlange
Um die [EventProcessorHost] verwenden, müssen Sie ein Konto Azure Storage **EventProcessorHost** Prüfpunktinformationen aufzeichnen aktivieren. Sie verwenden ein Azure Storage-Konto oder führen Sie die Schritte zum Erstellen einer neuen [Azure-Speicher] . Notieren Sie sich die Verbindungszeichenfolge Azure Storage-Konto.

> [AZURE.NOTE] Kopieren und Einfügen die Verbindungszeichenfolge Azure Storage-Konto Achten Sie dürfen keine Leerzeichen enthalten.

Sie benötigen außerdem eine Service Bus-Warteschlange zuverlässige interaktive Nachrichten aktivieren. Eine Warteschlange können programmgesteuert in einem Fenster eine Stunde Deduplizierung wie in [Verwendung Service Bus Warteschlangen][Service Bus-Warteschlange]. Alternativ können Sie das [klassische Azure-Portal][lnk-classic-portal], wie folgt:

1. Klicken Sie in der unteren linken Ecke auf **neu** . Klicken Sie auf **Anwendungsdienste** > **Service Bus** > **Warteschlange** > **Benutzerdefinierte**. Geben Sie den Namen **d2ctutorial**, wählen Sie eine Region verwenden Sie einen vorhandenen Namespace und erstellen Sie eine neue. Auf der nächsten Seite **Duplikaterkennungsregel**aktivieren und **doppelte Erkennung bisherige Zeit** eine Stunde festgelegt. Klicken Sie auf das Häkchen in der unteren rechten Ecke die Warteschlangenkonfiguration speichern.

    ![Erstellen einer Warteschlange in Azure-portal][30]

2. In der Liste der Servicebuswarteschlangen auf **d2ctutorial**und klicken Sie dann auf **Konfigurieren**. Erstellen Sie zwei freigegebene Richtlinien eine aufgerufene **Senden** **Senden** Berechtigungen und eine aufgerufene **hören** mit Berechtigungen **hören** . **Wenn Sie fertig sind, klicken Sie unten.**

    ![Konfigurieren einer Warteschlange in Azure-portal][31]

3. Klicken Sie auf **Dashboard** oben und dann **Informationen** unten. Notieren Sie sich die zwei Verbindungszeichenfolgen.

    ![Warteschlange Dashboard in Azure-portal][32]

### <a name="create-the-event-processor"></a>Der Ereignisprozessor erstellen

1. Klicken Sie in der aktuellen Visual Studio-Projektmappe erstellen einer Visual C# Windows-Projekt mit der Projektvorlage **Konsolenanwendungsprojekt** auf **Datei** > **Hinzufügen** > **Neues Projekt**. Stellen Sie sicher, dass die.NET Framework-Version 4.5.1 oder höher. Nennen Sie das Projekt **ProcessDeviceToCloudMessages**, und klicken Sie auf **OK**.

    ![Neues Projekt in Visual Studio][10]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **ProcessDeviceToCloudMessages** Projekts und dann auf **NuGet-Pakete verwalten**. Das Dialogfenster **NuGet Paket-Manager** .

3. **WindowsAzure.ServiceBus**suchen, klicken Sie auf **Installieren**und die Vereinbarung akzeptieren. Dieser Vorgang heruntergeladen, installiert und ein Verweis auf das [Azure Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus)mit allen abhängigen.

4. **Microsoft.Azure.ServiceBus.EventProcessorHost**suchen, klicken Sie auf **Installieren**und die Vereinbarung akzeptieren. Dieser Vorgang heruntergeladen, installiert und ein Verweis auf den [Azure Service Bus Event Hub - EventProcessorHost NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)mit allen abhängigen.

5. Mit der rechten Maustaste des **ProcessDeviceToCloudMessages** -Projekts, klicken Sie auf **Hinzufügen**und dann auf **Klasse**. Nennen Sie die neue Klasse **StoreEventProcessor**, und klicken Sie dann auf **OK** , um die Klasse zu erstellen.

6. Fügen Sie die folgende Anweisung am Anfang der Datei StoreEventProcessor.cs:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Ersetzen Sie den folgenden Code für den Text der Klasse:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    **EventProcessorHost** -Klasse ruft diese Klasse zum Gerät Cloud-Nachrichten IoT Hub zu verarbeiten. Der Code in dieser Klasse implementiert die Logik Nachrichten zuverlässig in einem BLOB-Container speichern und interaktive Nachrichten Service Bus-Warteschlange weiterleiten.

    **OpenAsync** -Methode initialisiert die **CurrentBlockInitOffset** Variablen den aktuellen Offset der ersten Nachricht von diesem Ereignisprozessor gelesen. Beachten Sie, dass jeder Prozessor für eine einzelne Partition verantwortlich ist.

    Die **ProcessEventsAsync** -Methode IoT Hub Nachrichten empfängt und wie folgt verarbeitet: interaktive Nachrichten an die Warteschlange Service Bus gesendet und fügt Daten zeigen Nachrichten an den Speicherpuffer **ToAppend**aufgerufen. Wenn Puffer Grenzwert von 4 MB erreicht Zeitfenster Deduplizierung abläuft (eine Stunde nach einem Prüfpunkt in diesem Lernprogramm), löst die Anwendung einen Checkpoint.

    Die **AppendAndCheckpoint** -Methode generiert zunächst eine Block-ID für den Block angefügt. Azure-Speicher erfordert alle IDs dieselbe Länge haben, damit die Methode den Offset mit führenden Nullen - pads blockieren `currentBlockInitOffset.ToString("0000000000000000000000000")`. Und wenn ein Block mit dieser ID bereits im Blob ist, die Methode mit dem aktuellen Inhalt des Puffers überschreibt.

    > [AZURE.NOTE] Um den Code zu vereinfachen, wird in diesem Lernprogramm ein einzelnes Blob pro Partition zum Speichern von Nachrichten verwendet. Lösung implementieren Sie Datei durch weitere Dateien nach einer bestimmten Zeit oder bei Erreichen eine bestimmten Größe erstellen. Beachten Sie, dass ein Azure Blockblob höchstens 195 GB Daten enthalten kann.

8. Fügen Sie in der **Program** -Klasse die folgende **using** -Anweisung am Anfang:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Ändern Sie die **Main** -Methode in **der Programmklasse** wie folgt. Ersetzen Sie **{Iot Hub Verbindungszeichenfolge}** mit der Verbindungszeichenfolge **Iothubowner** [Erste Schritte mit IoT Hub] Tutorial. Die Verbindungszeichenfolge, die zu Beginn dieses Abschnitts erwähnt ersetzen Sie Verbindungszeichenfolge Speicher. Ersetzen der Verbindungszeichenfolge Service Bus mit Berechtigungen für die Warteschlange **d2ctutorial** am Anfang dieses Abschnitts notierten **Senden** :

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Der Einfachheit halber wird in diesem Lernprogramm eine Instanz der [EventProcessorHost] -Klasse verwendet. Weitere Informationen finden Sie unter [Event Hubs Programming Guide].

## <a name="receive-interactive-messages"></a>Interaktive Nachrichten
In diesem Abschnitt schreiben Sie eine Windows-Konsole app, interaktiven Nachrichten aus der Warteschlange Service Bus. Weitere Informationen zum Aufbau einer Lösung mit Service Bus finden Sie in der [Multi-Tier-Anwendungsentwicklung mit Service Bus][].

1. In der aktuellen Visual Studio-Projektmappe mithilfe eines Visual C# Windows Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **ProcessD2CInteractiveMessages**.

2. Im Projektmappen-Explorer mit der rechten Maustaste des **ProcessD2CInteractiveMessages** Projekts und dann auf **NuGet-Pakete verwalten**. Dieser Vorgang zeigt das Fenster **NuGet Paket-Manager** .

3. **WindowsAzure.ServiceBus**suchen, klicken Sie auf **Installieren**und die Vereinbarung akzeptieren. Dieser Vorgang heruntergeladen, installiert und ein Verweis auf den [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus)mit sämtlichen abhängigen.

4. Fügen Sie am Anfang der Datei **Program.cs** Folgendes **verwenden** :

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Schließlich fügen Sie die folgenden Zeilen der **Main** -Methode. Ersetzen Sie die Verbindungszeichenfolge mit Berechtigungen für die Warteschlange **d2ctutorial** **hören** .

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt können Sie die Anwendung ausführen.

1.  Wählen Sie in Visual Studio im Projektmappen-Explorer **Startprojekte festlegen aus**und klicken Sie die Projektmappe. Wählen Sie **mehrere Startprojekte**, dann **Starten Sie** als Aktion für Projekte **ProcessDeviceToCloudMessages**, **SimulatedDevice**und **ProcessD2CInteractiveMessages** .

2.  Drücken Sie **F5** , um die Konsole drei Programme starten. Die **ProcessD2CInteractiveMessages** -Anwendung sollte jede interaktive **SimulatedDevice** Anwendung gesendete Nachricht verarbeiten.

  ![Konsole Applikationen][50]

> [AZURE.NOTE] Updates in das Blob finden müssen Sie die Konstante **MAX_BLOCK_SIZE** in der **StoreEventProcessor** auf einen kleineren Wert wie **1024**reduzieren. Diese Änderung ist nützlich, weil einige Zeit, maximale Größe blockieren mit dem simulierten Gerät gesendeten Daten. Mit einer kleineren Blockgröße müssen Sie nicht so lange warten auf das Blob erstellt und aktualisiert. Allerdings vereinfacht die Verwendung einer größeren Blockgröße Anwendung skalierbarer.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie zuverlässig Datenpunkt und interaktive Gerät Cloud-Nachrichten mithilfe der [EventProcessorHost] -Klasse.

[Wie Cloud-Gerät mit IoT Hub senden] [ lnk-c2d] zeigt, wie Sie Ihre Geräte vom Back-End schicken.

Beispiele für umfassende End-to-End-Lösung, IoT Hub verwenden, finden Sie unter [Azure IoT Suite][lnk-suite].

Weitere Informationen zur Entwicklung mit IoT Hub finden Sie unter [IoT Hub-Entwicklerhandbuch].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure BLOB-Speicher]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Service Bus-Warteschlange]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT Hub-Entwicklerhandbuch - Gerät in die cloud]: iot-hub-devguide-messaging.md

[Azure-Speicher]: https://azure.microsoft.com/documentation/services/storage/
[Azure Servicebus]: https://azure.microsoft.com/documentation/services/service-bus/

[Entwicklerhandbuch IoT Hub]: iot-hub-devguide.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Vorübergehender Fehler behandeln]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Zum Azure-Speicher]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Erste Schritte mit Event Hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure Storage Skalierbarkeit Richtlinien]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Ereignis-Hubs Programming Guide]: ../event-hubs/event-hubs-programming-guide.md
[Vorübergehender Fehler behandeln]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Multi-Tier-Anwendungsentwicklung mit Service Bus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
