## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Nachrichten Sie mit EventProcessorHost in Java

EventProcessorHost ist eine Java-Klasse, die Empfang von Ereignissen von Ereignis durch Verwaltung von permanenten Prüfpunkte vereinfacht und Parallel empfängt diese Ereignis-Hubs. Mit EventProcessorHost können Sie Ereignisse mehrere Empfänger verteilt selbst wenn in verschiedenen Knoten gehostet. Dieses Beispiel zeigt die Verwendung von EventProcessorHost für einen einzelnen Empfänger.

###<a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

Um EventProcessorHost verwenden zu können, benötigen Sie ein [Speicherkonto Azure][]:

1. Melden Sie das [klassische Azure-Portal an][]und auf **neu** am unteren Bildschirmrand.

2. Klicken Sie auf **Data Services**, **Speicher**und dann **Schnell erstellen**und geben Sie einen Namen für das Speicherkonto. Wählen Sie die gewünschte Region und dann auf **Storage-Konto erstellen**.

    ![][11]

3. Klicken Sie auf das neu erstellte Speicherkonto, und klicken Sie **Zugriffstasten verwalten**:

    ![][12]

    Kopieren Sie den primäre Schlüssel später in diesem Lernprogramm

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Erstellen Sie ein Java-Projekt mit dem EventProcessor-Host

Die Java-Clientbibliothek für Event Hubs steht für Projekte aus dem [Zentralen Repository Maven]Maven[Maven Package], und die Abhängigkeit Deklaration in der Projektdatei Maven verwenden:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Für verschiedene Arten von Buildumgebungen explizit erhalten Sie die neuesten JAR-Dateien aus dem [Zentralen Repository Maven] [ Maven Package] oder [Release-Verteilungspunkt auf GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Im folgenden Beispiel erstellen Sie ein neues Maven-Projekt für eine Konsole-Shell-Anwendung zuerst in Ihrer bevorzugten Java Development-Umgebung. Die Klasse namens ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Verwenden Sie den folgenden Code erstellt eine neue Klasse namens ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Erstellen Sie eine finale Klasse aufgerufen ```EventProcessorSample```, mit dem folgenden Code.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Ersetzen Sie die folgenden Felder mit den Werten verwendet, wenn das Ereignis Hub und Speicherkonto erstellt.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Diese praktische Einführung verwendet eine einzelne Instanz von EventProcessorHost. Zur Erhöhung des Durchsatzes, wird empfohlen, dass mehrere Instanzen des EventProcessorHost ausgeführt. In diesen Fällen koordinieren die verschiedenen Instanzen automatisch mit Lastenausgleich empfangenen Ereignisse. Soll mehrere Empfänger jeder Prozess *Alle* Ereignisse, verwenden Sie das **ConsumerGroup** Konzept. Beim Empfangen von Ereignissen von verschiedenen Computern möglicherweise nützlich für EventProcessorHost Instanzen auf dem Computer (oder Rollen) Namen angeben, in der sie bereitgestellt werden.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure Storage-Konto]: ../articles/storage/storage-create-storage-account.md
[Azure-Verwaltungsportal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

