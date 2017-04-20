<properties
    pageTitle="Azure IoT Hub für C# Einstieg | Microsoft Azure"
    description="Azure IoT Hub mit C# Schritte. Verwenden Sie Azure IoT Hub und C# mit Microsoft Azure IoT SDKs Internet der Dinge-Lösung implementieren."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Erste Schritte mit Azure IoT Hub für .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Lernprogramms haben Sie Windows-Konsole Applikationen:

* **CreateDeviceIdentity**, ein geräteidentität zugeordneten Sicherheitsschlüssel simulierte Gerät verbunden werden.
* **ReadDeviceToCloudMessages**, dem simulierten Gerät per Telemetrie angezeigt.
* **SimulatedDevice**, Verbindung IoT Hub mit der zuvor erstellten Gerät und sendet eine Nachricht Telemetrie pro Sekunde mithilfe des Protokolls AMQP.

> [AZURE.NOTE] Finden Sie Informationen über die verschiedenen SDKs, mit denen Sie sowohl Anwendung auf Geräte und Ihre Lösung Back-End [IoT Hub SDKs][lnk-hub-sdks].

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben jetzt Ihre IoT Hub, und Sie haben den Hostnamen und die Verbindung, die Sie den Rest dieses Lernprogramms abgeschlossen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine geräteidentität

In diesem Abschnitt erstellen Sie eine Windows-Konsole app, die in der identitätsregistrierung IoT Hub geräteidentität erstellt. Ein Gerät Verbindung keine IoT Hub, wenn einen Eintrag in die Registrierung des Geräts Identität verfügt. Weitere Informationen finden Sie im Abschnitt "Geräteidentitätsregistrierung" [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity]. Beim Ausführen dieser Konsolenanwendung generiert er einen eindeutigen Geräte-ID und Schlüssel, mit dem das Gerät selbst ermitteln, wenn Gerät Cloud-Nachrichten IoT Hub sendet.

1. In Visual Studio ein Visual C# herkömmlichen Windows-Desktop Projekt hinzufügen der aktuellen Projektmappe mit der Projektvorlage **Konsolenanwendungsprojekt** . Stellen Sie sicher, dass die.NET Framework-Version 4.5.1 oder höher. Nennen Sie das Projekt **CreateDeviceIdentity**.

    ![Neue Visual C# herkömmlichen Windows-Desktop-Projekt][10]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **CreateDeviceIdentity** Projekts und dann auf **Nuget-Pakete verwalten**.

3. Wählen Sie im Fenster **Nuget Paket-Manager** **Durchsuchen**, suchen Sie **microsoft.azure.devices**wählen Sie zur Installation des **Microsoft.Azure.Devices** -Pakets **Installieren** und akzeptieren Sie die Vereinbarung. Dieses Verfahren heruntergeladen, installiert und ein Verweis auf die [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-Paket und die zugehörigen Dateien.

    ![NuGet Paket-Manager Fenster][11]

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **"Program.cs"** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Diese Methode erstellt eine geräteidentität mit ID **MyFirstDevice**. (, Geräte-ID in der Registrierung bereits vorhanden, ruft der Code einfach vorhandenen Geräteinformationen.) Die Anwendung zeigt dann den Primärschlüssel für die Identität. Dieser Schlüssel wird in der simulierten Verbindung IoT Hub verwenden.

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Führen Sie diese Anwendung, und notieren Sie sich den Geräteschlüssel.

    ![Von der Anwendung generierte Geräteschlüssel][12]

> [AZURE.NOTE] IoT Hub identitätsregistrierung speichert nur Identitäten Gerät an den Hub sicheren Zugriff zu ermöglichen. Es speichert Geräte-IDs und Schlüssel als Anmeldeinformationen und ein Flag aktiviert/deaktiviert, mit denen Sie Zugriff für ein einzelnes Gerät deaktivieren. Wenn die Anwendung andere gerätespezifische Metadaten speichern muss, sollte einen anwendungsspezifische Speicher verwendet werden. Weitere Informationen finden Sie unter [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Gerät-zu-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Windows-Konsole app, die Gerät zu Cloud-Nachrichten IoT Hub liest. IoT Hub macht eine [Azure Ereignis Hubs][lnk-event-hubs-overview]-kompatiblen Endpunkt Gerät Cloud-Nachrichten lesen können. In diesem Lernprogramm erstellt zur Vereinfachung, einen grundlegenden Leser, der nicht für einen hohen Durchsatz geeignet ist. Finden Sie Informationen zum Gerät Cloud Nachrichten Ebene [Gerät Cloud-Nachrichten verarbeiten] [ lnk-process-d2c-tutorial] Tutorial. Weitere Informationen zum Verarbeiten von Nachrichten von Ereignis, finden Sie unter [Erste Schritte mit Event Hubs] [ lnk-eventhubs-tutorial] Tutorial. (In diesem Lernprogramm ist die IoT Hub Ereignis Hub-kompatible Endpunkte für.)

> [AZURE.NOTE] Ereignis Hub-kompatiblen Endpunkt zum Lesen von Gerät Cloud-Nachrichten immer verwendet das AMQP-Protokoll.

1. In Visual Studio ein Visual C# herkömmlichen Windows-Desktop Projekt hinzufügen der aktuellen Projektmappe mit der Projektvorlage **Konsolenanwendungsprojekt** . Stellen Sie sicher, dass die.NET Framework-Version 4.5.1 oder höher. Nennen Sie das Projekt **ReadDeviceToCloudMessages**.

    ![Neue Visual C# herkömmlichen Windows-Desktop-Projekt][10]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **ReadDeviceToCloudMessages** Projekts und dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget Paket-Manager** **WindowsAzure.ServiceBus**suchen, wählen Sie **Installieren**und die Vereinbarung akzeptieren. Dieses Verfahren heruntergeladen, installiert und ein Verweis auf [Azure Service Bus][lnk-servicebus-nuget], mit sämtlichen abhängigen. Dieses Paket kann die Anwendung das Ereignis Hub-kompatiblen Endpunkt IoT Hub verbinden.

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **"Program.cs"** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge im Abschnitt "Erstellen einer IoT Hub" erstellten IoT Hub.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Diese Methode verwendet eine **EventHubReceiver** -Instanz empfangen Nachrichten aus allen der IoT Hub Gerät-zu-Cloud Partitionen. Beachten Sie, wie Sie übergeben ein `DateTime.Now` Parameter für das **EventHubReceiver** -Objekt erstellen, sodass es nur gesendete Nachrichten empfängt, nachdem es gestartet. Dieser Filter eignet sich in einer Umgebung für die aktuelle Gruppe von Nachrichten zu sehen. In einer produktiven Umgebung sollte Ihr Code sicherstellen, dass alle Nachrichten verarbeitet. Weitere Informationen finden Sie unter [wie IoT Hub Gerät Cloud-Nachrichten] [ lnk-process-d2c-tutorial] Tutorial.

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Gerät

In diesem Abschnitt erstellen Sie eine Windows-Konsole app, die ein Gerät simuliert, das Gerät zu Cloud an IoT Hub sendet.

1. In Visual Studio ein Visual C# herkömmlichen Windows-Desktop Projekt hinzufügen der aktuellen Projektmappe mit der Projektvorlage **Konsolenanwendungsprojekt** . Stellen Sie sicher, dass die.NET Framework-Version 4.5.1 oder höher. Nennen Sie das Projekt **SimulatedDevice**.

    ![Neue Visual C# herkömmlichen Windows-Desktop-Projekt][10]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **SimulatedDevice** Projekts und dann auf **Nuget-Pakete verwalten**.

3. Wählen Sie im Fenster **Nuget Paket-Manager** **Durchsuchen**, suchen Sie **Microsoft.Azure.Devices.Client**wählen Sie zur Installation des **Microsoft.Azure.Devices.Client** -Pakets **Installieren** und akzeptieren Sie die Vereinbarung. Dieses Verfahren downloads installiert und ein Verweis auf den [Azure IoT - Gerät SDK Nuget-Paket] [ lnk-device-nuget] und die zugehörigen Dateien.

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie die Platzhalterwerte mit IoT Hub Hostname, den Sie im Abschnitt "Erstellen einen IoT Hub" abgerufen und der Geräteschlüssel im Abschnitt "Erstellen eine geräteidentität" abgerufen.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Diese Methode sendet eine Nachricht Gerät Cloud pro Sekunde. Die Nachricht enthält ein Objekt JSON serialisiert die Geräte-ID und eine zufällig generierte Zahl ein Geschwindigkeitssensor Wind simulieren.

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Standardmäßig erstellt die **Create** -Methode eine **DeviceClient** -Instanz, die das AMQP-Protokoll kommunizieren mit IoT Hub verwendet. Um das HTTP-Protokoll verwenden, verwenden Sie die Außerkraftsetzung der Methode **Erstellen** , die das Protokoll angeben kann. Wenn Sie das HTTP-Protokoll verwenden, sollten Sie auch **Microsoft.AspNet.WebApi.Client** Nuget-Paket zum Projekt, um den **System.Net.Http.Formatting** -Namespace enthalten hinzufügen.

Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer IoT Hub-Gerät. Sie können auch [Service für Azure IoT Hub angeschlossen] [ lnk-connected-service] Visual Studio-Erweiterung für den erforderlichen Code zur Clientanwendung Gerät hinzufügen.


> [AZURE.NOTE] Zur Vereinfachung, implementiert keine wiederholen Richtlinien in diesem Lernprogramm. In Produktionscode sollten Sie wiederholungsrichtlinien (z. B. eine exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]implementieren[lnk-transient-faults].

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun die Anwendung ausführen.

1.  In Visual Studio im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe und dann auf **Startprojekte festlegen**. Wählen Sie **mehrere Startprojekte**, und **Wählen Sie dann die Aktion für die **ReadDeviceToCloudMessages** und **SimulatedDevice** ** .

    ![Start-Eigenschaften][41]

2.  Drücken Sie **F5** starten beide apps ausgeführt. Die Konsolenausgabe **SimulatedDevice** App simulierte Gerät IoT Hub sendet Nachrichten angezeigt. Die Konsolenausgabe **ReadDeviceToCloudMessages** App zeigt IoT Hub empfängt Nachrichten.

    ![Ausgabe in der Konsole von apps][42]

3. Die **Verwendung** Kachel in [Azure-Portal] [ lnk-portal] zeigt die Anzahl der Nachrichten an den Hub gesendet:

    ![Azure Portal Verwendung Kachel][43]


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm IoT Hub in Azure-Portal konfiguriert und eine geräteidentität in den Hub identitätsregistrierung erstellt. Mithilfe dieses geräteidentität der simulierten Gerät App Gerät Cloud-Nachrichten an den Hub senden. Sie erstellt auch eine Anwendung, die Nachrichten vom Hub angezeigt. 

Zum Einstieg IoT Hub und andere Szenarien IoT zu:

- [Verbinden Ihr Gerät][lnk-connect-device]
- [Erste Schritte mit Device-management][lnk-device-management]
- [Erste Schritte mit IoT Gateway SDK][lnk-gateway-SDK]

Die Projektmappe IoT Erweiterung Gerät Cloud-Nachrichten Ebene verarbeiten finden Sie unter [Device Cloud-Nachrichten verarbeiten] [ lnk-process-d2c-tutorial] Tutorial.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
