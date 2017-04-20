## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Event Hubs

Die Java-Clientbibliothek für Event Hubs steht für Maven Projekte aus dem [Zentralen Repository Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)und Abhängigkeit Deklaration in der Projektdatei Maven verwenden:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Für verschiedene Arten von Buildumgebungen können Sie die neuesten JAR-Dateien aus dem [Zentralen Repository Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) oder [Release-Verteilungspunkt auf GitHub](https://github.com/Azure/azure-event-hubs/releases)explizit abrufen.  

Für einfache Ereignisherausgeber Importieren der *com.microsoft.azure.eventhubs* für die Clientklassen Ereignis Hubs und das Paket *com.microsoft.azure.servicebus* für Dienstprogrammklassen wie allgemeine Ausnahmen von Azure Service Bus messaging-Client genutzt werden. 

Im folgenden Beispiel erstellen Sie ein neues Maven-Projekt für eine Konsole-Shell-Anwendung zuerst in Ihrer bevorzugten Java Development-Umgebung. Die Klasse namens ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Ersetzen Sie Namespace und Event Hub Namen mit Werten verwendet, wenn der Hub Ereignis erstellt.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Erstellen Sie dann ein einzigartiges Ereignis durch eine Zeichenfolge in der UTF-8-Byte-Codierung aktivieren. Wir erstellen eine neue Ereignis Hubs Clientinstanz aus der Verbindungszeichenfolge und senden Sie die Nachricht.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
