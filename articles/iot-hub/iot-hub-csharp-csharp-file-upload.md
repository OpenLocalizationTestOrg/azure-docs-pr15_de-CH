<properties
    pageTitle="Hochladen von Dateien von Geräten mit IoT Hub | Microsoft Azure"
    description="Dieses Lernprogramm Informationen zum Hochladen von Dateien von Azure IoT Hub mit C#-Geräten."
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Exemplarische Vorgehensweise: Dateien von Geräten mit IoT Hub in die Cloud hochladen

## <a name="introduction"></a>Einführung

Azure IoT Hub einen komplett verwalteten Service, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und eine Anwendung beenden. Vorherigen Lernprogramme ([mit IoT Hub] und [Cloud-zu-Gerät-Nachrichten mit IoT Hub]) veranschaulichen die grundlegende Gerät Cloud und Cloud-zu-Gerät-Messagingfunktionen IoT Hub und [Prozess-Gerät zu Cloud-Nachrichten] Tutorial beschreibt das Gerät Cloud-Nachrichten zuverlässig in Azure BLOB-Speicher zu speichern. In einigen Szenarios können nicht Sie jedoch einfach die Daten zuordnen, die Geräte in kleinen Gerät Cloud-Nachrichten senden, die IoT Hub akzeptiert. Beispiele sind große Dateien enthalten Bilder, Videos, Vibration Daten Samplerate hoher Frequenz oder eine Form von vorbereiteten Daten enthalten. Diese Dateien sind in der Regel Stapel in der Cloud wie [Azure Data Factory] oder [Hadoop] Stapel verarbeitet. Beim Upload von Dateien von einem Gerät bevorzugt Ereignisse gesendet werden, können Sie dennoch IoT Hub Sicherheit und Zuverlässigkeit Funktionalität.

Dieses Lernprogramm baut auf den Code im [Cloud-zu-Gerät-Nachrichten mit IoT Hub] Lernprogramm zeigen die Datei hochladen Funktionen IoT Hub. Zeigt, wie Sie sicher bereitstellen ein Gerät mit einer Azure BLOB-URI für den Upload einer Datei und wie IoT Hub Datei hochladen Benachrichtigung verwenden, um die Datei in die app-Back-End verarbeiten.

Am Ende dieser praktischen Einführung führen Sie zwei Windows-Konsole Applications.

* **SimulatedDevice**, eine geänderte Version der Anwendung erstellt das Lernprogramm [Cloud Gerät Nachrichten mit IoT Hub] , das eine Datei mit einem SAS-URI IoT Hub bereitgestellten Speicher hochgeladen.
* **ReadFileUploadNotification**, die Datei hochladen Benachrichtigung von IoT Hub empfängt.

> [AZURE.NOTE] IoT Hub unterstützt zahlreiche Plattformen und Sprachen (z. B. C, Java und Javascript) über Azure IoT Gerät SDKs. Finden Sie in [Azure IoT Developer Center] schrittweise Anleitung auf das Gerät, der Code in diesem Lernprogramm und im Allgemeinen Azure IoT Hub anschließen.

Um dieses Lernprogramm benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015

+ Ein aktives Azure-Konto. (Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Zuordnen einer Firma Azure Storage IoT Hub

Da das simulierte Gerät in ein Blob eine Datei hochgeladen wird, müssen Sie ein Konto [Azure Storage] IoT Hub. Wenn Sie einen IoT Hub Azure Storage-Konto zuordnen, erzeugen Hub SAS-URI, mit denen ein Gerät sicher Hochladen einer Datei auf einen BLOB-Container. IoT Hub-Dienst und den Geräte-SDKs koordiniert die, die SAS-URI generiert und für ein Gerät, eine Datei hochzuladen.

Anweisungen in [Konfigurieren Dateiuploads Azure-Portal mit] [ lnk-configure-upload] ein Konto Azure Storage IoT Hub zuordnen.

## <a name="upload-a-file-from-a-simulated-device"></a>Upload einer Datei von einem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierten geräteanwendung in Cloud Gerät IoT Hub Nachrichten [Nachrichten mit IoT Cloud-Gerät] erstellt.

1. In Visual Studio mit der rechten Maustaste des **SimulatedDevice** Projekts, klicken Sie auf **Hinzufügen**und klicken Sie dann auf **Vorhandenes Element**. Bilddatei navigieren Sie und es in Ihrem Projekt. Diese praktische Einführung geht das Bild mit dem Namen `image.jpg`.

2. Mit der rechten Maustaste auf das Bild, und klicken Sie dann auf **Eigenschaften**. Stellen Sie sicher, dass **In Ausgabeverzeichnis kopieren** auf **immer kopieren**festgelegt ist.

    ![][1]

3. Fügen Sie in der Datei **Program.cs** die folgende Anweisung am Anfang der Datei:

        using System.IO;

4. Der **Program** -Klasse die folgende Methode hinzugefügt:
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    Die `UploadToBlobAsync` hat in der Datei Namen und Stream die Datei hochgeladen werden und den Upload Speicher behandelt. Die Konsolenanwendung zeigt den Zeitaufwand für das Hochladen der Datei.

5. Fügen Sie die folgende Methode in die **Main** -Methode vor der `Console.ReadLine()` Zeile:

        SendToBlobAsync();

> [AZURE.NOTE] Der Einfachheit halber implementiert dieses Lernprogramm keine Richtlinien wiederholen. Im Produktionscode sollten Sie wiederholen (z.B. exponentielle Backoff) wie im MSDN-Artikel [Vorübergehende Fehler behandeln]Maßnahmen.

## <a name="receive-a-file-upload-notification"></a>Benachrichtigung Datei hochladen

In diesem Abschnitt schreiben Sie eine Windows-Konsole app, die Datei hochladen IoT Hub Benachrichtigung.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe ein neues Visual C# Windows-Projekt mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **ReadFileUploadNotification**.

    ![Neues Projekt in Visual Studio][2]

2. Im Projektmappen-Explorer mit der rechten Maustaste des **ReadFileUploadNotification** Projekts und dann auf **NuGet-Pakete verwalten**.

    NuGet-Pakete verwalten-Fenster angezeigt.

2. Suchen Sie nach `Microsoft.Azure.Devices`klicken Sie auf **Installieren**und die Vereinbarung akzeptieren. 

    Diese downloads installiert und ein Verweis [Azure IoT - Service SDK NuGet-Paket] im **ReadFileUploadNotification** -Projekt hinzugefügt.

3. Fügen Sie in der Datei **Program.cs** die folgende Anweisung am Anfang der Datei:

        using Microsoft.Azure.Devices;

4. Fügen Sie die folgenden Felder der Klasse **Anwendung** . Ersetzen Sie Platzhalterwert mit IoT Hub-Verbindungszeichenfolge aus [IoT Hub Einstieg]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Der **Program** -Klasse die folgende Methode hinzugefügt:
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Beachten Sie, dass das Muster empfangen derselbe zur Cloud Gerät Gerät app Nachrichten.

6. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt können Sie die Anwendung ausführen.

1. In Visual Studio die Projektmappe Maustaste und **Startprojekte festlegen**auswählen. Wählen Sie **mehrere Startprojekte**und dann **die Startaktion für **ReadFileUploadNotification** und **SimulatedDevice**** .

2. Drücken Sie **F5**. Beide Programme startet. Den Upload abgeschlossen in einer Konsole app und die Upload-Nachricht durch andere Konsole app sollte angezeigt werden. [Azure-Portal] oder Visual Studio Server-Explorer können Sie Ihre Azure Storage-Konto für die hochgeladene Datei einchecken.

  ![][50]


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie die Datei hochladen Fähigkeiten IoT Hub Dateiuploads von Geräten vereinfacht. Sie können weiter erkunden IoT Hub Features und Szenarios mit den folgenden Artikeln:

- [IoT Hub programmgesteuert erstellen][lnk-create-hub]
- [Einführung in C# SDK][lnk-c-sdk]
- [IoT Hub SDKs][lnk-sdks]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Ein SDK Gateway IoT simulieren][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure-portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Nachrichten Sie Cloud-zu-Gerät-mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Prozess Gerät Cloud-Nachrichten]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Vorübergehender Fehler behandeln]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-Speicher]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - Service SDK NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


