<properties
    pageTitle="Azure IoT Hub für Java Einstieg | Microsoft Azure"
    description="Azure IoT Hub mit Java Schritte. Verwenden Sie Azure IoT Hub und Java mit Microsoft Azure IoT SDKs Internet der Dinge-Lösung implementieren."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Erste Schritte mit Azure IoT Hub für Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Lernprogramms haben Sie Java-Konsole Applikationen:

* **Erstellen Sie Device-Identität**ein geräteidentität zugeordneten Sicherheitsschlüssel simulierte Gerät verbunden werden.
* **d2c Nachrichten lesen**, dem simulierten Gerät per Telemetrie angezeigt.
* **simulierte Gerät**die Verbindung IoT Hub mit der zuvor erstellten Gerät und sendet eine Nachricht Telemetrie jedes zweite mit AMQP-Protokoll.

> [AZURE.NOTE] Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen zu den verschiedenen SDKs, mit denen Sie sowohl Anwendung auf Geräte und Ihre Lösung Back-End.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Java SE 8. <br/> [Vorbereiten der Umgebung] [ lnk-dev-setup] Java für dieses Lernprogramm unter Windows oder Linux Installation beschrieben.

+ Maven 3.  <br/> [Vorbereiten der Umgebung] [ lnk-dev-setup] beschreibt das Maven für dieses Lernprogramm unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Als letzten Schritt Notieren Sie sich den **Primärschlüssel** Wert und dann auf **Nachrichten**. -Blade **Messaging** Notieren Sie **Ereignis Hub-kompatible Namen** und dem **Ereignis Hub-kompatiblen Endpunkt**. Sie benötigen diese drei Werte beim Erstellen der Anwendung **d2c Nachrichten lesen** .

![Azure Portal IoT Hub Messaging-blade][6]

Sie haben jetzt Ihre IoT Hub und Sie haben IoT Hub Hostname, Verbindungszeichenfolge IoT Hub, IoT Hub Primärschlüssel, Ereignis Hub-kompatible Namen und Ereignis Hub-kompatiblen Endpunkt Sie dieses Lernprogramm abgeschlossen müssen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine geräteidentität

In diesem Abschnitt erstellen Sie eine Java-Konsole app, die eine neue Identität des Geräts in der identitätsregistrierung IoT Hub erstellt. Ein Gerät Verbindung keine IoT Hub, wenn einen Eintrag in die Registrierung des Geräts Identität verfügt. Siehe Abschnitt **Geräteidentitätsregistrierung** [IoT Hub-Entwicklerhandbuch] [ lnk-devguide-identity] Weitere Informationen. Beim Ausführen dieser Konsolenanwendung generiert er einen eindeutigen Geräte-ID und Schlüssel, mit dem das Gerät selbst ermitteln, wenn Gerät Cloud-Nachrichten IoT Hub sendet.

1. Erstellen Sie einen neuen Ordner namens Iot-Java-erste-Schritte. Erstellen Sie im Ordner Iot-Java-erste-Schritte ein neues Maven Projekt **Erstellen-Device-Identität** mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass eine lange Befehl:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner erstellen Gerät-Identität.

3. Mit einem Texteditor, öffnen Sie die Datei pom.xml im Ordner erstellen Gerät Identität und der **abhängigen** Knoten fügen Sie die folgende Abhängigkeit hinzu. So können Sie das Paket Iothub Service Sdk in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Texteditor die Datei create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Die folgende Anweisung **Importieren** der Datei hinzufügen:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Der **App** -Klasse zu **{Yourhubconnectionstring}** mit dem Wert der genannten zuvor fügen Sie die folgenden Variablen hinzu:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Ändern Sie die Signatur der **main** -Methode Ausnahmen wie folgt:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Fügen Sie den folgenden Code als Text der **main** -Methode. Dieser Code erstellt ein *Javadevice* in der Registrierung IoT Hub Identität aufgerufen, wenn nicht bereits vorhanden ist. Anschließend werden die Geräte-Id und Schlüssel später angezeigt:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Speichern Sie und schließen Sie die Datei App.java.

11. Um Maven Anwendung durch **Erstellen Gerät Identität** zu erstellen, führen Sie den folgenden Befehl an der Befehlszeile im Ordner erstellen Gerät Identität:

    ```
    mvn clean package -DskipTests
    ```

12. Um Maven Anwendung durch **Erstellen Gerät Identität** auszuführen, führen Sie den folgenden Befehl an der Befehlszeile im Ordner erstellen Gerät Identität:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Notieren Sie sich die **Geräte-Id** und **Gerät**. Später benötigen Sie beim Erstellen einer Anwendung, die Verbindung IoT Hub ein.

> [AZURE.NOTE] IoT Hub identitätsregistrierung speichert nur Identitäten Gerät an den Hub sicheren Zugriff zu ermöglichen. Es speichert Geräte-IDs und Schlüssel als Anmeldeinformationen und ein Flag aktiviert/deaktiviert, mit denen Sie Zugriff für ein einzelnes Gerät deaktivieren. Wenn die Anwendung andere gerätespezifische Metadaten speichern muss, sollte einen anwendungsspezifische Speicher verwendet werden. [IoT Hub Developer Guide] finden Sie unter[ lnk-devguide-identity] Weitere Informationen.

## <a name="receive-device-to-cloud-messages"></a>Gerät-zu-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Java-Konsole app, die IoT Hub Gerät Cloud-Nachrichten liest. IoT Hub macht ein [Ereignis Hub][lnk-event-hubs-overview]-kompatiblen Endpunkt Gerät Cloud-Nachrichten lesen können. In diesem Lernprogramm erstellt zur Vereinfachung, einen grundlegenden Leser, der nicht für einen hohen Durchsatz geeignet ist. [Gerät-zu-Cloud-Nachrichten verarbeiten] [ lnk-process-d2c-tutorial] Lernprogramm veranschaulicht das Gerät Cloud-Nachrichten Ebene verarbeiten. [Erste Schritte mit Event Hubs] [ lnk-eventhubs-tutorial] Tutorial enthält weitere Informationen zum Verarbeiten von Nachrichten von Ereignis und IoT Hub Ereignis Hub-kompatible Endpunkte für.

> [AZURE.NOTE] Ereignis Hub-kompatiblen Endpunkt zum Lesen von Gerät Cloud-Nachrichten immer verwendet das AMQP-Protokoll.

1. Iot-Java-erste-Schritte Ordner im Abschnitt *Erstellen einer geräteidentität* erstellten erstellen Sie ein neues Maven mit dem Namen **d2c Nachrichten lesen** , die mit dem folgenden Befehl in der Befehlszeile. Beachten Sie, dass eine lange Befehl:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner d2c Nachrichten lesen.

3. Mit einem Texteditor, öffnen Sie die Datei pom.xml im Ordner d2c Nachrichten lesen und **abhängigen** Knoten fügen Sie die folgende Abhängigkeit hinzu. Dadurch können Sie eventhub-Client-Paket vom Ereignis Hub-kompatiblen Endpunkt Lesen in Ihrer Anwendung verwenden:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Texteditor die Datei read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Die folgende Anweisung **Importieren** der Datei hinzufügen:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Der **App** -Klasse die folgenden Variablen hinzufügen. Ersetzen Sie **{Youriothubkey}**, **{Youreventhubcompatibleendpoint}**und **{Youreventhubcompatiblename}** durch die zuvor notierten Werte:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Fügen Sie die folgende **ReceiveMessages** -Methode der **App** -Klasse. Diese Methode erstellt eine **EventHubClient** -Instanz für die Verbindung zum Ereignis Hub-kompatiblen Endpunkt und erstellt dann eine **PartitionReceiver** -Instanz aus einer Event Hub Partition lesen asynchron. Dabei ständig durchläuft und die Nachrichtendetails bis die Anwendung beendet wird.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Diese Methode verwendet einen Filter, wenn Empfänger erstellt, damit der Empfänger liest nur Nachrichten IoT Hub nach Empfänger gestartet. Dies ist nützlich in einer Umgebung die aktuelle Gruppe von Nachrichten zu sehen. , Dass der Code sicherstellen sollten in einer so verarbeitet alle Nachrichten - [wie IoT Hub Gerät Cloud-Nachrichten] finden Sie unter[ lnk-process-d2c-tutorial] Tutorial für Weitere Informationen.

9. Ändern Sie die Signatur der **main** -Methode die Ausnahme wie folgt einfügen:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. **Die Hauptmethode der **App** -Klasse** den folgenden Code hinzufügen. Dieser Code erstellt zwei Instanzen **EventHubClient** und **PartitionReceiver** und ermöglicht Ihnen die Anwendung schließen nach Nachrichten verarbeiten:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Dieser Code wird vorausgesetzt, dass Ihr IoT Hub F1 (kostenlos) Ebene erstellt. Kostenlose IoT Hub verfügt über zwei Partitionen mit dem Namen "0" und "1".

11. Speichern Sie und schließen Sie die Datei App.java.

12. Um mit Maven **d2c Nachrichten lesen** -Anwendung zu erstellen, führen Sie den folgenden Befehl an der Befehlszeile im Ordner d2c Nachrichten lesen:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt erstellen Sie eine Java-Konsole app, die ein Gerät simuliert, das Gerät zu Cloud an IoT Hub sendet.

1. Iot-Java-erste-Schritte Ordner im Abschnitt *Erstellen einer geräteidentität* erstellten erstellen Sie ein neues Maven-Projekt **simulierten Gerät** mit dem folgenden Befehl an der Befehlszeile aufgerufen. Beachten Sie, dass eine lange Befehl:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile den simulierten Gerät Ordner.

3. Mit einem Texteditor, öffnen Sie die Datei pom.xml im Ordner simulierten Gerät und **abhängigen** Knoten fügen Sie die folgenden Abhängigkeiten hinzu. Dadurch werden Iothub-Java-Client-Paket mit IoT Hub kommunizieren und Java-Objekte zu JSON serialisiert in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Texteditor die Datei simulated-device\src\main\java\com\mycompany\app\App.java.

6. Die folgende Anweisung **Importieren** der Datei hinzufügen:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Der **App** -Klasse ersetzen Ihrer IoT Hub und den mit dem Gerät Schlüsselwert im Abschnitt *erstellen eine geräteidentität* erstellten **{Yourdevicekey}** **{Youriothubname}** fügen Sie die folgenden Variablen hinzu:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Diese Anwendung verwendet die **Protokoll** -Variable, wenn ein **DeviceClient** -Objekt instanziiert. Die HTTP oder AMQP können mit IoT Hub kommunizieren.

8. Fügen Sie folgenden geschachtelte Klasse innerhalb der **App** -Klasse die Telemetriedaten an Ihr Gerät IoT Hub sendet **TelemetryDataPoint** hinzu:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Fügen Sie die folgende verschachtelte **EventCallback** Klasse innerhalb der **App** -Klasse den Bestätigung Status angezeigt, den beim Verarbeiten einer Nachricht aus dem simulierten Gerät IoT Hub zurückgibt. Diese Methode benachrichtigt auch den Hauptthread der Anwendung, wenn die Meldung verarbeitet wurde:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Fügen Sie die folgende verschachtelte **MessageSender** Klasse innerhalb der **App** -Klasse. **Führen Sie** Methode in dieser Klasse generiert Beispiel Telemetriedaten IoT Hub an und wartet auf eine Bestätigung, bevor die nächste Nachricht gesendet:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Diese Methode sendet eine Nachricht Gerät Cloud eine Sekunde nach IoT Hub die Nachricht bestätigt. Die Nachricht enthält ein Objekt JSON serialisiert DeviceId und Zufallsgenerierte Nummer ein Geschwindigkeitssensor Wind simulieren.

11. Ersetzen Sie die **main** -Methode mit dem folgenden Code, der einen Thread zum Gerät Cloud IoT Hub schicken erstellt:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Speichern Sie und schließen Sie die Datei App.java.

13. Um die **simulierten Gerät** Anwendung Maven zu erstellen, führen Sie den folgenden Befehl an der Befehlszeile im Ordner simulierten Gerät:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. eine exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Eine Befehlszeile im Ordner lesen d2c führen Sie den folgenden Befehl zum Überwachen der ersten Partition IoT Hub:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-Clientanwendung Gerät Cloud-Nachrichten überwachen][7]

2. Eine Befehlszeile im Ordner simulierten Gerät führen Sie den folgenden Befehl Senden von Daten an IoT Hub beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub Gerät Clientanwendung Gerät Cloud-Nachrichten][8]

3. Die **Verwendung** Kachel in [Azure-Portal] [ lnk-portal] zeigt die Anzahl der Nachrichten an den Hub gesendet:

    ![Azure Portal Verwendung nebeneinander anzeigen der Anzahl der Nachrichten an IoT Hub][43]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm einen neuen IoT Hub in Azure-Portal konfiguriert und eine geräteidentität in den Hub identitätsregistrierung erstellt. Mithilfe dieses geräteidentität der simulierten Gerät App Gerät Cloud-Nachrichten an den Hub senden. Sie erstellt auch eine Anwendung, die Nachrichten vom Hub angezeigt. 

Erste Schritte mit IoT Hub und zu finden Sie unter anderen IoT Szenarien:

- [Verbinden Ihr Gerät][lnk-connect-device]
- [Erste Schritte mit Device-management][lnk-device-management]
- [Erste Schritte mit IoT Gateway SDK][lnk-gateway-SDK]

Die Projektmappe IoT Erweiterung Gerät Cloud-Nachrichten Ebene verarbeiten finden Sie unter [Device Cloud-Nachrichten verarbeiten] [ lnk-process-d2c-tutorial] Tutorial.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/