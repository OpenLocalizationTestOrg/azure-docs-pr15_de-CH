<properties
    pageTitle="Cloud-zu-Gerät-Nachrichten mit IoT Hub | Microsoft Azure"
    description="Dieses Lernprogramm Informationen zum Azure IoT Hub mit C# Cloud-zu-Gerät-Nachrichten senden."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Exemplarische Vorgehensweise: Cloud Gerät Nachrichten IoT Hub und .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten IoT aktivieren und eine Anwendung zurück. Das Lernprogramm [Erste Schritte mit IoT Hub] veranschaulicht IoT Hub erstellen und Bereitstellen einer geräteidentität code eines simulierten Geräts, das Gerät zu Cloud-Nachrichten sendet.

Dieses Lernprogramm baut auf [Erste Schritte mit IoT Hub]. Es veranschaulicht an:

- Nachrichten Sie von der Anwendung Cloud Back-End Cloud-Gerät an einem einzigen Gerät IoT Hub.
- Nachrichten Sie Cloud-zu-Gerät-auf einem Gerät.
- Ihre Anwendung Cloud back-End, Nachrichten an ein Gerät von IoT Hub Übermittlung Bestätigung (*Feedback*) anfordern.

Finden Sie mehr Cloud-zu-Gerät-Nachrichten [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

Am Ende dieser praktischen Einführung führen Sie zwei Windows-Konsole Applications:

* **SimulatedDevice**, eine geänderte Version der app in [Schritten mit IoT Hub], Verbindung IoT Hub und empfängt Nachrichten, Cloud-Gerät erstellt.
* **SendCloudToDevice**, die Cloud-zu-Gerät-Nachricht mit dem simulierten Gerät IoT Hub sendet und erhält die Lieferung Bestätigung.

> [AZURE.NOTE] IoT Hub unterstützt SDK für zahlreiche Plattformen und Sprachen (z. B. C, Java und Javascript) durch Azure IoT Geräte-SDKs. Eine ausführliche Anleitung zum Verbinden Sie das Gerät in diesem Lernprogramm Code und sehen im Allgemeinen Azure IoT Hub [Azure IoT Developer Center].

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierten geräteanwendung in Cloud Gerät IoT Hub Nachrichten [beginnen mit IoT Hub] erstellt.

1. Fügen Sie die folgende Methode in Visual Studio im Projekt **SimulatedDevice** an der **Program** -Klasse.

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    Die `ReceiveAsync` -Methode gibt asynchron empfangene Meldung bei vom Gerät empfangen wird. Es gibt *null* zurück, nachdem ein Zeitlimit festzulegen (in diesem Fall wird die Standardeinstellung von einer Minute verwendet). In diesem Fall sollte der Code neue Nachrichten warten. Dies ist der Grund für die `if (receivedMessage == null) continue` Linie.

    Der Aufruf von `CompleteAsync()` IoT Hub benachrichtigt, dass die Meldung erfolgreich verarbeitet wurde. Die Nachricht kann aus der Warteschlange entfernt werden. Wenn etwas, das Gerät app Abschluss der Verarbeitung der Nachricht verhindert passiert, übermittelt IoT Hub erneut. Unbedingt dann Nachricht Verarbeitungslogik Gerät App *Idempotent*sein, damit die gleiche Nachricht mehrmals empfangen dasselbe Ergebnis liefert. Eine Anwendung kann auch eine Meldung verlassen was IoT Hub Meldung in der Warteschlange für zukünftige Verbrauch beibehalten. Oder die Ablehnung kann einer Nachricht, die die Nachricht aus der Warteschlange entfernt. Finden Sie weitere Informationen des Lebenszyklus Cloud-zu-Gerät-Nachricht [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Wenn HTTP MQTT oder AMQP Verkehr, anstelle der `ReceiveAsync` Methode kehrt sofort zurück. Unterstützte Muster für Cloud-zu-Gerät-Nachrichten mit HTTP ist zeitweise verbundene Geräte, die überprüfen selten Nachrichten (weniger als 25 Minuten). Weitere HTTP Ausgabe erhält Ergebnisse IoT Hub Drosselung Anfragen. Finden Sie weitere Informationen zu den Unterschieden zwischen MQTT, AMQP und HTTP-Unterstützung und IoT Hub Drosselung [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

2. Fügen Sie die folgende Methode in die **Main** -Methode vor der `Console.ReadLine()` Zeile:

        ReceiveC2dAsync();

> [AZURE.NOTE] Der Einfachheit halber implementiert dieses Lernprogramm keine Richtlinien wiederholen. Im Produktionscode sollten Sie wiederholen (z.B. exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]Maßnahmen.

## <a name="send-a-cloud-to-device-message"></a>Cloud-Gerät senden

In diesem Abschnitt werden Sie eine Windows Konsolenanwendung schreiben, die Cloud-Gerät an die app simulierten Gerät sendet.

1. In der aktuellen Visual Studio-Projektmappe mithilfe eines Visual C# Desktop App neue Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **SendCloudToDevice**.

    ![Neues Projekt in Visual Studio][20]

2. Im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe und dann auf **NuGet-Pakete verwalten, für die Lösung**. 

    Öffnet das Fenster **NuGet-Pakete verwalten** .

3. Suchen Sie nach `Microsoft Azure Devices`klicken Sie auf **Installieren**und die Vereinbarung akzeptieren. 

    Diese downloads installiert und ein Verweis auf den [Azure IoT - Service SDK NuGet-Paket].

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **Program.cs** :

        using Microsoft.Azure.Devices;

5. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie Platzhalterwert mit IoT Hub-Verbindungszeichenfolge aus [IoT Hub Einstieg]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Diese Methode sendet eine Nachricht Cloud-Gerät auf das Gerät mit der ID `myFirstDevice`. Ändern Sie diesen Parameter entsprechend bei in [Erste Schritte mit IoT Hub]verwendet geändert.

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. In Visual Studio die Projektmappe Maustaste und wählen Sie **legen Startprojekte...**. Wählen Sie **mehrere Startprojekte**und dann **die Startaktion für **ProcessDeviceToCloudMessages**, **SimulatedDevice**und **SendCloudToDevice**** .

9.  Drücken Sie **F5**. Alle drei Programme startet. Aktivieren Sie das Fenster **SendCloudToDevice** , und drücken Sie die **EINGABETASTE**. Die Meldung durch die simulierte Anwendung sollte angezeigt werden.

    ![Empfangende App-Nachricht][21]

## <a name="receive-delivery-feedback"></a>Lieferung Feedback erhalten
Kann auf Anforderung Lieferung (oder Ablaufdatum) Empfangsbestätigungen IoT Hub für jede Nachricht Cloud-Gerät. Cloud-Back-End leicht wiederholen oder Entschädigung Logik informieren können. Informationen Feedback Cloud-Gerät finden Sie unter [IoT Hub-Entwicklerhandbuch][IoT Hub Developer Guide - C2D].

In diesem Abschnitt ändern Sie **SendCloudToDevice** app Feedback anfordern und IoT Hub empfangen.

1. Fügen Sie die folgende Methode in Visual Studio im Projekt **SendCloudToDevice** an der **Program** -Klasse.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Beachten Sie, dass das Muster empfangen derselbe zur Cloud Gerät Gerät app Nachrichten.

2. Fügen Sie die folgende Methode in die **Main** -Methode nach dem `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` Zeile:

        ReceiveFeedbackAsync();

3. Um Feedback für die Bereitstellung einer Cloud-zu-Gerät-Nachricht anzufordern, müssen Sie eine Eigenschaft in der **SendCloudToDeviceMessageAsync** -Methode angeben. Fügen Sie die folgende Zeile nach der `var commandMessage = new Message(...);` Zeile:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Führen Sie die apps durch Drücken von **F5 aus**. Alle drei Programme starten sollte angezeigt werden. Aktivieren Sie das Fenster **SendCloudToDevice** , und drücken Sie die **EINGABETASTE**. Die empfangene Nachricht die simulierte Anwendung und nach einigen Sekunden durch die Anwendung **SendCloudToDevice** Feedback-Meldung sollte angezeigt werden.

    ![Empfangende App-Nachricht][22]

> [AZURE.NOTE] Der Einfachheit halber implementiert dieses Lernprogramm keine Richtlinien wiederholen. Im Produktionscode sollten Sie wiederholen (z.B. exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]Maßnahmen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Cloud-zu-Gerät-Nachrichten senden und empfangen. 

Beispiele für umfassende End-to-End-Lösung, IoT Hub verwenden, finden Sie unter [Azure IoT Suite].

Weitere Informationen zur Entwicklung mit IoT Hub finden Sie unter [IoT Hub-Entwicklerhandbuch].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - Service SDK NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Vorübergehender Fehler behandeln]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Entwicklerhandbuch IoT Hub]: iot-hub-devguide.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/