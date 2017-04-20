> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

In dieser exemplarischen Vorgehensweise der [simulierten Gerät Cloud hochladen Beispiel] veranschaulicht, wie [Azure IoT Gateway SDK] [ lnk-sdk] Gerät Cloud Telemetrie IoT Hub von simulierten Geräten senden.

Diese exemplarische Vorgehensweise umfasst:

1. **Architektur**: wichtige Informationen zur Architektur von Beispiel simuliert Gerät Cloud hochladen.

2. **Erstellen und Ausführen**: die erforderlichen Schritte zum Erstellen und Ausführen des Beispiels.

## <a name="architecture"></a>Architektur

Simulierten Gerät Cloud hochladen Beispiel veranschaulicht, wie mit dem SDK Gateway sendet Telemetrie von simulierten Geräten an IoT Hub. Die simulierte kann nicht direkt mit IoT Hub da Geräte:

- Geräte verwenden ein Kommunikationsprotokoll IoT Hub verständlich.
- Die Geräte sind nicht intelligent genug an die Identität von IoT Hub zugewiesen.

Das Gateway löst diese Probleme für den simulierten Geräten auf folgende Weise:

- Gateway versteht das Protokoll den simulierten Geräten Geräte Gerät Cloud Telemetrie empfängt und leitet Nachrichten an IoT Hub mithilfe eines Protokolls vom Hub verstanden.
- Das Gateway IoT Hub Identitäten für den simulierten Geräten gespeichert und fungiert als Proxy, wenn die simulierten Geräten IoT Hub Nachrichten senden.

Die folgende Abbildung zeigt die Hauptkomponenten der Stichprobe Gateway-Module:

![][1]


> [AZURE.NOTE] Die Module übergeben Nachrichten direkt. Die Module veröffentlichen Nachrichten ein interner Broker, die die Nachrichten über einen Abonnement-Mechanismus wie in der Abbildung unten dargestellt Module bietet. Weitere Informationen finden Sie unter [Erste Schritte mit IoT Gateway SDK][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Aufnahme-Modul

Dieses Modul ist der Ausgangspunkt für das Abrufen von Daten von Geräten über das Gateway und in der Cloud. Im Beispiel führt das Modul vier Aufgaben:

1.  Simulierte Daten erstellt.
    
    Hinweis: Wenn Sie reale Geräte verwenden, würde das Modul Daten aus diesen Geräten lesen.

2.  Wird die simulierten Daten in den Inhalt einer Nachricht.

3.  Die Nachricht, die simulierten Daten enthält, hinzugefügt eine Eigenschaft mit einer gefälschten Adresse.

4.  Es wird die Nachricht in der Kette mit der nächsten Unterrichtseinheit verfügbar.

> [AZURE.NOTE] Das **Protokoll X Ingestion** -Modul im obigen Diagramm ist im Quellcode **simulierten Gerät** aufgerufen.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT Hub ID-Modul

Dieses Modul sucht nach Nachrichten mit einer Eigenschaft, die MAC-Adresse, die vom Modul Einnahme Protokoll des simulierten Ausgabegeräts hinzugefügt. Findet das Modul eine Eigenschaft der Nachricht einer anderen Eigenschaft mit einem IoT Hub-Gerät hinzugefügt und dann wird die Nachricht in der Kette der nächsten Unterrichtseinheit zur. Dies ist wie das Beispiel IoT Hub Gerät Identitäten mit simulierten Geräten verknüpft. Der Entwickler richtet die Zuordnung von MAC-Adressen und IoT Hub Identitäten manuell als Teil der Modulkonfiguration. 

> [AZURE.NOTE]  Dieses Beispiel verwendet eine MAC-Adresse als Bezeichner Gerätetreiber und IoT Hub geräteidentität entspricht. Allerdings können Sie Ihr eigenes Modul schreiben, das eine andere Kennung verwendet. Beispielsweise müssen Sie Geräte mit eindeutigen Seriennummern oder Daten, die einen eindeutigen Gerätenamen eingebettet, mit denen Sie IoT Hub geräteidentität ermitteln konnte.

### <a name="iot-hub-communication-module"></a>Kommunikationsmodul IoT Hub

Dieses Modul nimmt Nachrichten mit IoT Hub geräteidentität vom vorherigen Modul zugewiesen und den Nachrichteninhalt IoT Hub über HTTP sendet. HTTP ist eines der drei Protokolle IoT Hub verständlich.

Anstatt für jedes simulierten Gerät eine Verbindung IoT Hub, dieses Modul öffnet eine einzige HTTP-Verbindung vom Gateway IoT Hub und Multiplexen Verbindungen von den simulierten Geräten über diese Verbindung. Dies ermöglicht ein einzelnes Gateway Geräte viele weitere, simulierten oder andernfalls nicht möglich wäre, wenn sie für jedes Gerät eine eindeutige Verbindung geöffnet.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Beispiel für den simulierten Gerät Cloud hochladen]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md