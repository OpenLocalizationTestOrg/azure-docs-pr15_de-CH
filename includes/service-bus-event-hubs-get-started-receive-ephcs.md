## <a name="receive-messages-with-eventprocessorhost"></a>Nachrichten mit EventProcessorHost

[EventProcessorHost][] ist eine, die empfangen Ereignisse Ereignis Hubs durch Verwaltung von permanenten Prüfpunkte vereinfacht und Parallel empfängt diese Ereignis-Hubs. [EventProcessorHost][]verwenden, können Sie Ereignisse mehrere Empfänger verteilt selbst wenn in verschiedenen Knoten gehostet. Dieses Beispiel zeigt die Verwendung von [EventProcessorHost][] für einen einzelnen Empfänger. Das Beispiel [skaliert Event processing][] veranschaulicht [EventProcessorHost][] mit mehreren Empfängern verwenden.

Um [EventProcessorHost][]verwenden zu können, benötigen Sie ein [Speicherkonto Azure][]:

1. [Azure-Portal][]anmelden und auf **neu** oben links auf dem Bildschirm.

2. **Daten + Speicher**, klicken Sie auf **Konto**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Geben Sie einen Namen für das Speicherkonto Blatt **Speicherkonto erstellen** . Wählen Sie ein Azure-Abonnement Ressourcengruppe und Speicherort für die Ressource zu erstellen. Klicken Sie auf **Erstellen**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Klicken Sie in der Liste der Speicherkonten auf neu erstellte Speicherkonto.

5. Klicken Sie im Blatt Konto Speicher **Zugriffstasten**. Kopieren Sie den Wert von **key1** später in diesem Lernprogramm verwenden.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. Erstellen Sie in Visual Studio ein neues Visual C# Desktop-App-Projekt mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **Empfänger**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. Im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe und dann auf **NuGet-Pakete verwalten, Lösung**.

6. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Sicherstellen Sie, dass im Feld **Version** der Namen (**Empfänger**) angegeben ist. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio heruntergeladen, installiert und ein Verweis auf den [Azure Service Bus Event Hub - EventProcessorHost NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)mit allen abhängigen.

7. Mit der rechten Maustaste des **Empfänger** -Projekts, klicken Sie auf **Hinzufügen**und dann auf **Klasse**. Nennen Sie die neue Klasse **SimpleEventProcessor**, und klicken Sie dann auf **Hinzufügen** , um die Klasse zu erstellen.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. Fügen Sie die folgende Anweisung am Anfang der Datei SimpleEventProcessor.cs:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Dann ersetzen Sie folgenden Code für den Text der Klasse:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Diese Klasse wird von **EventProcessorHost** zur Verarbeitung von Ereignissen vom Event Hub empfangen aufgerufen werden. Beachten Sie, dass die `SimpleEventProcessor` Klasse verwendet eine Stoppuhr regelmäßig Checkpoint-Methode auf dem Kontext **EventProcessorHost** aufrufen. Dadurch wird sichergestellt, dass der Empfänger neu gestartet wird, nicht mehr als fünf Minuten Verarbeitung verloren geht.

9. Fügen Sie der **Program** -Klasse folgende `using` -Anweisung am Anfang der Datei:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Ersetzen der `Main` -Methode in der `Program` mit den folgenden Code ersetzen Event Hub Namen und Namespace-Ebene-Verbindungszeichenfolge, die Sie zuvor gespeichert und das Speicherkonto und Schlüssel, die in den vorherigen Abschnitten kopiert. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Diese praktische Einführung verwendet eine einzelne Instanz von [EventProcessorHost][]. Zur Erhöhung des Durchsatzes, wird empfohlen, dass mehrere Instanzen des [EventProcessorHost][]ausführen wie im Beispiel [skaliert, Verarbeitung][] . In diesen Fällen koordinieren die verschiedenen Instanzen automatisch mit Lastenausgleich empfangenen Ereignisse. Soll mehrere Empfänger jeder Prozess *Alle* Ereignisse, verwenden Sie das **ConsumerGroup** Konzept. Beim Empfangen von Ereignissen von verschiedenen Computern möglicherweise nützlich für [EventProcessorHost][] Instanzen auf dem Computer (oder Rollen) Namen angeben, in der sie bereitgestellt werden. Weitere Informationen zu diesen Themen finden Sie unter der [Übersicht über Hubs][] und [Ereignis Hubs Programming Guide][] .

<!-- Links -->
[Übersicht über Hubs]: ../articles/event-hubs/event-hubs-overview.md
[Ereignis-Hubs Programming Guide]: ../articles/event-hubs/event-hubs-programming-guide.md
[Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure Storage-Konto]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure-portal]: https://portal.azure.com