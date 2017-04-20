<properties
    pageTitle="Cloud-zu-Gerät-Nachrichten mit IoT Hub | Microsoft Azure"
    description="Dieses Lernprogramm lernen Java Azure IoT Hub mit Cloud-zu-Gerät-Nachrichten senden."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Exemplarische Vorgehensweise: Cloud Gerät Nachrichten IoT Hub und Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten IoT aktivieren und eine Anwendung zurück. Das Lernprogramm [Erste Schritte mit IoT Hub] veranschaulicht IoT Hub erstellen und Bereitstellen einer geräteidentität code eines simulierten Geräts, das Gerät zu Cloud-Nachrichten sendet.

Dieses Lernprogramm baut auf [Erste Schritte mit IoT Hub]. Es veranschaulicht an:

- Nachrichten Sie von der Anwendung Cloud Back-End Cloud-Gerät an einem einzigen Gerät IoT Hub.
- Nachrichten Sie Cloud-zu-Gerät-auf einem Gerät.
- Ihre Anwendung Cloud back-End, Nachrichten an ein Gerät von IoT Hub Übermittlung Bestätigung (*Feedback*) anfordern.

Finden Sie mehr Cloud-zu-Gerät-Nachrichten [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

Am Ende dieser praktischen Einführung führen Sie gegenseitige Java-Konsole:

* **simulierte Gerät**eine geänderte Version der app in [Schritten mit IoT Hub], Verbindung IoT Hub und empfängt Nachrichten, Cloud-Gerät erstellt.
* **c2d senden**, sendet eine Nachricht Cloud-Gerät an dem simulierten Gerät IoT Hub und empfängt dann die Lieferung Bestätigung.

> [AZURE.NOTE] IoT Hub unterstützt SDK für zahlreiche Plattformen und Sprachen (z. B. C, Java und Javascript) durch Azure IoT Geräte-SDKs. Eine ausführliche Anleitung zum Verbinden Sie das Gerät in diesem Lernprogramm Code und sehen im Allgemeinen Azure IoT Hub [Azure IoT Developer Center].

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Java SE 8. <br/> [Vorbereiten der Umgebung] [ lnk-dev-setup] Java für dieses Lernprogramm unter Windows oder Linux Installation beschrieben.

+ Maven 3.  <br/> [Vorbereiten der Umgebung] [ lnk-dev-setup] beschreibt das Maven für dieses Lernprogramm unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierten geräteanwendung in Cloud Gerät IoT Hub Nachrichten [beginnen mit IoT Hub] erstellt.

1. Mit einem Texteditor die Datei simulated-device\src\main\java\com\mycompany\app\App.java.

2. Fügen Sie die folgende **MessageCallback** Klasse als geschachtelte Klasse innerhalb der **App** -Klasse. Methode **Ausführen** wird aufgerufen, wenn das Gerät eine Nachricht IoT Hub empfängt. In diesem Beispiel benachrichtigt das Gerät immer dem Hub die Nachricht abgeschlossen:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Ändern Sie die **main** -Methode eine **MessageCallback** -Instanz erstellen und die **SetMessageCallback** -Methode aufrufen, bevor der Client wie folgt öffnen:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Wenn Sie HTTP statt MQTT oder AMQP als Transportprotokoll verwenden, überprüft die Instanz **DeviceClient** Nachrichten selten IoT Hub (weniger als 25 Minuten). Finden Sie weitere Informationen zu den Unterschieden zwischen MQTT, AMQP und HTTP-Unterstützung und IoT Hub Drosselung [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Cloud-Gerät senden

In diesem Abschnitt erstellen Sie eine Java-Konsole app, die Cloud-Gerät an die app simulierten Gerät sendet. Sie benötigen die Geräte-Id des Geräts, das im Lernprogramm [Erste Schritte mit IoT Hub] hinzugefügt. Sie benötigen die Verbindungszeichenfolge für Ihre IoT Hub, den in der [Azure-Portal]finden.

1. Erstellen eines Maven **c2d senden** mit dem folgenden Befehl an der Befehlszeile aufgerufen. Beachten Sie, dass eine lange Befehl:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Befehlszeile in den neuen Ordner c2d senden.

3. Mit einem Texteditor, öffnen Sie die Datei pom.xml im Ordner c2d senden und **abhängigen** Knoten fügen Sie die folgende Abhängigkeit hinzu. Die Abhängigkeit hinzufügen können Sie **Iothub-Java-Service -** Clientpaket IoT Hub-Dienst kommunizieren in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Speichern Sie und schließen Sie die Datei pom.xml.

5. Mit einem Texteditor die Datei send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Die folgende Anweisung **Importieren** der Datei hinzufügen:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Der **App** -Klasse ersetzen **{Yourhubconnectionstring}** und **{Yourdeviceid}** die Werte der genannten zuvor fügen Sie die folgenden Variablen hinzu:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. Ersetzen Sie die **main** -Methode mit dem folgenden Code, der Verbindung IoT Hub und sendet eine Nachricht an Ihr Gerät wartet auf eine Bestätigung, dass das Gerät empfangen und die Nachricht verarbeitet:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Der Einfachheit halber implementiert dieses Lernprogramm keine Richtlinien wiederholen. Im Produktionscode sollten Sie wiederholen (z.B. exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]Maßnahmen.

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1. Eine Befehlszeile im Ordner simulierten Gerät führen Sie den folgenden Befehl Telemetrie an IoT Hub senden und Cloud-zu-Gerät-Nachrichten von Ihrem Hub empfangen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Führen Sie die Anwendung simulierten Gerät][img-simulated-device]

2. Eine Befehlszeile im Ordner c2d senden führen Sie den folgenden Befehl zu Cloud-Gerät senden Feedback Bestätigung warten:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Führen Sie den Befehl zum Senden der Nachricht Cloud-Gerät][img-send-command]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Cloud-zu-Gerät-Nachrichten senden und empfangen. 

Beispiele für umfassende End-to-End-Lösung, IoT Hub verwenden, finden Sie unter [Azure IoT Suite].

Weitere Informationen zur Entwicklung mit IoT Hub finden Sie unter [IoT Hub-Entwicklerhandbuch].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Erste Schritte mit IoT Hub]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Entwicklerhandbuch IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Vorübergehender Fehler behandeln]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/