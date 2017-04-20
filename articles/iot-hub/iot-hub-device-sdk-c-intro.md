<properties
    pageTitle="Mithilfe von Azure IoT Gerät-SDK c | Microsoft Azure"
    description="Lernen und erste Schritte zum Arbeiten mit dem Beispielcode in Azure IoT Gerät-SDK für C."
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Einführung in Azure IoT Gerät-SDK für C

**Azure IoT Gerät-SDK** ist eine Gruppe von Bibliotheken Ereignisse senden und Empfangen von Nachrichten vom Dienst **Azure IoT Hub** vereinfachen soll. Es gibt Variationen des SDK jedes für eine bestimmte Plattform **Azure IoT Device SDK c**beschrieben.

Azure IoT Gerät-SDK c steht in ANSI C (C99) Portabilität optimieren. Dadurch auf eine Reihe von Plattformen und Geräten ausgeführt werden, insbesondere bei der Minimierung der Datenträger geeignet und Speicherbedarf steht.  

Es gibt eine Vielzahl von Plattformen auf denen SDK getestet (Siehe den [Azure Certified IoT Gerät Katalog](https://catalog.azureiotsuite.com/) Details). Obwohl dieser Artikel exemplarischen Vorgehensweisen Beispielcode auf der Windows-Plattform enthält, denken Sie daran, dass der Code in diesem Artikel beschriebenen genau dasselbe in unterschiedlichsten Plattformen.

In diesem Artikel werden Sie der Architektur von Azure IoT Gerät-SDK c eingeführt werden Wir demonstrieren, wie die Bibliothek initialisiert Ereignisse IoT Hub senden sowie Nachrichten erhalten. Die Informationen in diesem Artikel sollte ausreichen, um das SDK verwenden, aber auch enthält Verweise auf zusätzliche Informationen zu den Bibliotheken.

## <a name="sdk-architecture"></a>SDK-Architektur

Sie finden im [Microsoft Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) GitHub Repository **Azure IoT Device SDK für C** und Einzelheiten der API der [C-API-Referenz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

Die neueste Version der Bibliotheken finden in der **master** dieses Repository:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Dieses Repository enthält die ganze Familie Azure IoT Gerät SDKs. Allerdings ist dieser Artikel Azure IoT Gerät SDK *c* in den Ordner " **c** " befinden.

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* Die zentrale Implementierung des SDK finden Sie der **Iothub\_Client** Ordner enthält die Implementierung der niedrigsten Ebene API im SDK: die **IoTHubClient** -Bibliothek. Die **IoTHubClient** -Bibliothek enthält APIs implementieren raw messaging Nachrichten an IoT Hub senden sowie Nachrichten erhalten. Bei Verwendung dieser Bibliothek sind Sie verantwortlich für das Implementieren von Meldungsserialisierung (schließlich mit dem Serialisierungsprogramm Beispiel beschrieben) weitere Kommunikation mit IoT Hub für Sie behandelt werden
* Der Ordner **Serialisierungsprogramm** enthält Hilfsfunktionen und Beispiele zum Serialisieren von Daten vor dem Senden an Azure IoT Hub mit der Clientbibliothek. Beachten Sie, dass die Verwendung des Serialisierungsprogramms nicht obligatorisch und nur aus Gründen der Einfachheit. Bei Verwendung die Bibliothek **Serialisierungsprogramm** beginnen Sie zunächst ein Modell, das die Ereignisse gibt IoT Hub sowie erwartete aus Nachrichten senden möchten. Definiertes Modell bietet das SDK eine API-Oberfläche, die problemlos mit Ereignissen und Nachrichten ohne Details Serialisierung ermöglicht.
Die Bibliothek hängt von anderen open Source-Bibliotheken, die Übertragung mehrere Protokolle (MQTT, AMQP) zu implementieren.
* Die **IoTHubClient** -Bibliothek hängt von anderen open-Source-Bibliotheken:
   * [Azure C Dienstprogramm Freigegebene](https://github.com/Azure/azure-c-shared-utility) Bibliothek bietet allgemeine Funktionen für grundlegende Aufgaben (wie Zeichenfolge Liste bearbeiten, EA, etc....) über mehrere Azure-bezogene C SDKs erforderlich
   * Der [Azure-uAMQP](https://github.com/Azure/azure-uamqp-c) -Bibliothek Client Side Implementierung von AMQP für Ressource Einschränkung Geräte optimiert.
   * Die [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) -Bibliothek ist eine allgemeine Implementierung des Protokolls MQTT und Ressource Einschränkung Geräte optimiert.

All dies ist einfacher zu verstehen, betrachten Sie Beispielcode. Die folgenden Abschnitte führen Sie durch ein paar Anwendungsbeispiele, die im SDK enthalten sind. Dies sollte Ihnen ein gutes Gefühl für die verschiedenen Funktionen von der Architektur des SDK sowie eine Einführung in die Funktionsweise der APIs.

## <a name="before-running-the-samples"></a>Vor dem Ausführen der Beispiele

Die Beispiele in der Azure IoT SDK c ausführen müssen Sie eine Instanz des Dienstes Azure-Abonnements erstellen, bereits vorhanden und 2 Aufgaben:
* Vorbereiten der Umgebung
* Anmeldeberechtigungen des Geräts zu erhalten.

Wenn eine Instanz von Azure IoT Hub auf Azure-Abonnement erstellen möchten, gehen Sie [hier](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

Mit dem SDK enthaltenen [Readme-Datei](https://github.com/Azure/azure-iot-sdks/tree/master/c) finden Sie Informationen zum Vorbereiten der Umgebung und geräteanmeldeinformationen erhalten.
Die folgenden Abschnitte enthalten einige zusätzliche Kommentare zu dieser Anleitung.

### <a name="preparing-your-development-environment"></a>Vorbereiten der Umgebung

Pakete dienen einige Plattformen (NuGet für Windows oder Apt_get für Debian und Ubuntu) und die Beispiele verwenden diese Pakete, sofern verfügbar, die unter Anleitung erläutert die Library und die Beispiele direkt Formularcode.

Zunächst müssen Sie eine Kopie des SDK GitHub und die Quelle zu erstellen. Sie erhalten eine Kopie der Quelle vom **master** Zweig des [GitHub-Repository](https://github.com/Azure/azure-iot-sdks).

Wenn Sie eine Kopie der Quelle heruntergeladen haben, müssen Sie in der SDK-Artikel ["Umgebung vorbereiten"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)beschriebenen Schritte.


Nachfolgend einige Tipps zur in Vorbereitungshandbuch beschriebenen Verfahren:

-   Wählen Sie beim Installieren des Dienstprogramms **CMake** **CMake** System Pfad für **alle Benutzer** ( **der aktuelle Benutzer** arbeiten hinzufügen) hinzufügen:

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Installieren Sie vor dem Öffnen der **Befehlszeile VS2015 Developer**Git Befehlszeilentools. Gehen Sie folgendermaßen vor, um diese Tools zu installieren:

    1. Starten Sie die **Visual Studio 2015** Installationsprogramm (oder Auswahl **Microsoft Visual Studio 2015** **Programme und Funktionen** Bedienfeld und wählen Sie **Ändern**).
    
    2. Stellen Sie sicher das Installationsprogramm **Git für Windows** -Feature aktiviert ist aber möglicherweise möchten die Option **GitHub-Erweiterung für Visual Studio** IDE Integration zu aktivieren:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Schließen Sie den Assistenten zum Installieren der Tools.

    4. Die Umgebungsvariable **PATH** Git Tools **Bin** -Verzeichnis hinzufügen. Unter Windows sieht dies folgendermaßen aus:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Abschluss in der Seite ["Prepare Umgebung"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) beschriebenen Schritte können Sie die Beispiel-Anwendung kompilieren.

### <a name="obtaining-device-credentials"></a>Gerät Anmeldeinformationen

Umgebung eingerichtet ist, ist das nächste Gerät Anmeldeinformationen abrufen.  Ein IoT Hub zugreifen können müssen Sie zuerst das Gerät IoT Hub Gerät Registrierung hinzufügen. Wenn Sie Ihr Gerät hinzufügen erhalten Gerät Anmeldeinformationen Sie damit Gerät Verbindung IoT Hub zu müssen. Beispiel-Applikationen, die wir im nächsten Abschnitt betrachten erwarten diese Anmeldeinformationen in Form einer **Verbindungszeichenfolge Gerät**.

Es gibt einige Tools im SDK open Source-Repository zu IoT Hub verwalten. Eine Windows Anwendung zweiten Gerät Explorer ist eine node.js plattformübergreifende CLI Tool namens Iothub Explorer gehört. Weitere Informationen zu diesen Tools [hier](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Wie wir durch die Beispiele in diesem Artikel unter Windows ausgeführt, verwenden wir mit dem Geräte-Explorer. Aber Sie können auch Iothub-Explorer bevorzugen CLI-Tools.

[Gerät](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) Explorertools verwendet Dienstbibliotheken Azure IoT verschiedene Funktionen auf IoT Hub, einschließlich Geräte. Wenn Sie ein Gerät hinzufügen Gerät Explorer verwenden, erhalten Sie eine entsprechende Verbindungszeichenfolge. Sie benötigen diese Verbindungszeichenfolge zu dem Beispiel ausgeführt.

Falls Sie nicht bereits mit dem Prozess vertraut, beschreibt das folgende Verfahren Gerät Explorer Gerät hinzufügen und eine Verbindungszeichenfolge Gerät verwenden.

WindowsInstaller finden Sie für das Gerät Explorer Tool auf dem [SDK Seite freizugeben](https://github.com/Azure/azure-iot-sdks/releases). Aber Sie können auch das Tool direkt über den Code **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** in **Visual Studio 2015** und die Lösung ausführen.

Beim Ausführen des Programms sehen Sie diese Schnittstelle:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Geben Sie im ersten Feld Ihre **IoT Hub-Verbindungszeichenfolge** und klicken Sie auf **Aktualisieren**. Dies konfiguriert das Tool, sodass mit IoT Hub kommunizieren kann.

Nach der Konfiguration der Verbindungszeichenfolge IoT Hub klicken auf die **Verwaltung** :

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Dies ist verwalten Geräte im IoT Hub registriert werden.

Ein Gerät können durch Klicken auf die Schaltfläche **Erstellen** . Ein Dialogfeld wird mit einem Satz vorkonfigurierter Schlüssel (Primär und sekundär) angezeigt. Müssen Sie lediglich eine **Geräte-ID** eingeben und dann auf **Erstellen**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Nachdem das Gerät erstellt wurde, wird die Geräteliste alle registrierten Geräte, einschließlich des soeben erstellten aktualisiert. Maustaste das neue Gerät wird dieses Menü angezeigt:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Bei Auswahl die Option **Verbindungszeichenfolge für ausgewählte Gerät kopieren,** wird die Verbindungszeichenfolge für das Gerät in die Zwischenablage kopiert. Eine Kopie der Verbindungszeichenfolge. Sie benötigen sie, wenn in den nächsten Abschnitten beschriebenen Beispiel-Anwendung ausgeführt.

Nachdem Sie die oben genannten Schritte abgeschlossen haben, können Sie Code führen. In beiden Beispielen haben eine Konstante oben Hauptquelldatei, mit dem Sie eine Verbindungszeichenfolge eingeben kann. Angenommen, die entsprechende Zeile aus der **Iothub\_Client\_Beispiel\_Amqp** Anwendung wie folgt.

```
static const char* connectionString = "[device connection string]";
```

Wenn Sie folgen, geben Sie Ihr Gerät-Verbindungszeichenfolge, Kompilieren die Projektmappe, und Sie zum Ausführen des Beispiels werden.

## <a name="iothubclient"></a>IoTHubClient

In der **Iothub\_Client** Ordner im Repository Azure Iot Sdks gibt es ein Ordner mit **Beispielen** , die von einer aufgerufen Anwendung **Iothub\_Client\_Beispiel\_Amqp**.

Die Windows-Version von der **Iothub\_Client\_Beispiel\_Ampq** Anwendung enthält die folgende Visual Studio-Lösung:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Diese Lösung enthält ein einzelnes Projekt. Es ist erwähnenswert, dass es vier NuGet-Pakete in dieser Lösung installiert:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Brauchen Sie das **Microsoft.Azure.C.SharedUtility** -Paket bei der Arbeit mit dem SDK. Da dieses Beispiel auf AMQP, müssen Sie die Pakete **Microsoft.Azure.uamqp** und **Microsoft.Azure.IoTHub.AmqpTransport** auch (es sind entsprechende Pakete für MQTT und HTTP). Da im Beispiel die **IoTHubClient** -Bibliothek verwendet, müssen Sie auch das Paket **Microsoft.Azure.IoTHub.IoTHubClient** darin einschließen.

Die Implementierung der Beispiel-Anwendung in der **Iothub\_Client\_Beispiel\_amqp.c** Datei.

Wir verwenden diese Anwendung, wie Sie die **IoTHubClient** -Bibliothek erforderlich.

### <a name="initializing-the-library"></a>Beim Initialisieren der Bibliothek

> [AZURE.NOTE] Vor Beginn der Arbeit mit den Bibliotheken müssen Sie einige spezifische Plattform-Initialisierung auszuführen. Wenn Sie AMQP unter Linux verwenden müssen Sie die OpenSSL-Bibliothek initialisieren. Die Beispiele im [GitHub Repository](https://github.com/Azure/azure-iot-sdks) aufrufen Dienstprogramm Funktion **Platform_init** Wenn der Client startet und rufen die **Platform_deinit** -Funktion vor dem Beenden. Diese Funktionen sind in der Headerdatei "platform.h" deklariert. Überprüfen Sie die Definitionen dieser Funktionen für Ihre Zielplattform im [Repository](https://github.com/Azure/azure-iot-sdks) zu ermitteln, ob Sie Initialisierungscode Plattform im Client enthalten.

Zum Arbeiten mit Bibliotheken Sie müssen zuerst ein Clienthandle IoT Hub zuweisen:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Beachten Sie, dass wir übergeben einer Kopie der Verbindungszeichenfolge Gerät dieser Funktion (eine Gerät Explorer erhalten). Wir bezeichnen das Protokoll, das wir verwenden möchten. Dieses Beispiel verwendet AMQP MQTT und HTTP sind jedoch auch eine Option.

Wenn Ihnen eine gültige **IOTHUB\_CLIENT\_BEHANDELT**, starten Sie die Ereignisse senden und Empfangen von Nachrichten von IoT Hub-APIs aufrufen. Wir betrachten als Nächstes.

### <a name="sending-events"></a>Senden von Ereignissen

Senden von Ereignissen an IoT Hub abzuschließen, müssen Sie die folgenden Schritte aus:

Erstellen Sie zunächst eine Nachricht:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Senden der Nachricht:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Jedes Mal, wenn Sie eine Nachricht senden, geben Sie einen Verweis auf eine Callback-Funktion, die aufgerufen wird, wenn die Daten gesendet werden:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Beachten Sie den Aufruf der **IoTHubMessage\_Destroy** funktionieren, wenn Sie mit der Nachricht fertig sind. Sie müssen diesen Aufruf freizugebende Ressourcen bei der Erstellung der Nachricht.

### <a name="receiving-messages"></a>Empfangen von Nachrichten

Empfangen einer Nachricht ist ein asynchroner Vorgang. Registrieren Sie einen Rückruf aufgerufen, wenn das Gerät eine Meldung:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Der letzte Parameter ist ein void-Zeiger auf beliebig. Im Beispiel wird ein Zeiger auf eine ganze Zahl, aber es wäre ein Zeiger auf eine komplexere Datenstruktur. Dadurch wird die Rückruffunktion auf freigegebenen Status mit dem Aufrufer dieser Funktion.

Wenn das Gerät eine Nachricht empfängt, wird die registrierte Callback-Funktion aufgerufen:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Beachten Sie, dass Sie die **IoTHubMessage\_GetByteArray** Funktion zum Abrufen der Nachricht, in diesem Beispiel eine Zeichenfolge wird.

### <a name="uninitializing-the-library"></a>Die Bibliothek deinitialisieren

Wenn Sie Ereignisse senden und Empfangen von Nachrichten abgeschlossen, können die Bibliothek IoT deinitialisieren. Geben Sie hierzu den folgenden Funktionsaufruf:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Das zuvor von reservierten Ressourcen frei der **IoTHubClient\_CreateFromConnectionString** Funktion.

Wie Sie sehen können, lässt sich Ereignisse senden und Empfangen von Nachrichten mit der **IoTHubClient** -Bibliothek. Die Bibliothek behandelt die Details mit IoT Hub, einschließlich das zu verwendende Protokoll (aus Sicht des Entwicklers ist das einfache Konfigurationsoption).

Außerdem stellt die Bibliothek **IoTHubClient** Kontrolle über wie Ereignisse serialisiert, die das Gerät IoT Hub sendet. In einigen Fällen ist dies ein Vorteil, aber in anderen Fällen ist dies ein Implementierungsdetail mit dem betroffenen möchten. Wenn dies der Fall ist, sollten Sie die **Serialisierungsprogramm** -Bibliothek im nächsten Abschnitt beschrieben werden.

## <a name="serializer"></a>Serialisierungsprogramm

Bibliothek des **Serialisierungsprogramms** befindet sich konzeptionell auf **IoTHubClient** -Bibliothek im SDK. Verwendet die **IoTHubClient** -Bibliothek für die zugrunde liegenden Kommunikation mit IoT Hub, aber fügt Modellierungsfunktionen, die Entwickler die Last mit Meldungsserialisierung entfernen. Wie zeigt diese Bibliothek wird am besten ein Beispiel.

Das **Serialisierungsprogramm** Ordner im Repository Azure Iot Sdks liegt ein **Beispiele** Ordner mit einer Anwendung namens **Simplesample\_Amqp**. Die Windows-Version dieses Beispiels umfasst die folgende Visual Studio-Lösung:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Wie im vorherigen Beispiel enthält diese mehrere NuGet-Pakete:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Wir haben die meisten im vorherigen Beispiel gesehen, aber **Microsoft.Azure.IoTHub.Serializer** ist neu. Dies ist erforderlich, wenn wir das **Serialisierungsprogramm** benutzen.

Die Implementierung derselben Anwendung in den **Simplesample\_amqp.c** Datei.

Die folgenden Abschnitte führen Sie durch die wichtigsten Teile dieses Beispiels.

### <a name="initializing-the-library"></a>Beim Initialisieren der Bibliothek

Zum Arbeiten mit der Bibliothek **Serialisierungsprogramm** , müssen die Initialisierung APIs aufgerufen werden:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Der Aufruf der **Serialisierungsprogramm\_Init** ist eine einmalige und die zugrunde liegenden Bibliothek initialisiert werden. Rufen Sie dann die **IoTHubClient\_CreateFromConnectionString** Funktion wie die **IoTHubClient** Probe derselben API. Dieser Aufruf wird die Verbindungszeichenfolge Gerät (Dies ist auch in dem das Protokoll auswählen verwenden möchten). Beachten Sie, dass dieses Beispiel verwendet AMQP als Transportmechanismus hätte HTTP.

Rufen Sie abschließend die **Erstellen\_Modell\_Instanz** Funktion. Beachten Sie, dass **Wetterstation** den Namespace und **ContosoAnemometer** ist der Name des Modells. Nachdem die Modellinstanz erstellt, können Sie Ereignisse senden und Empfangen von Nachrichten. Es ist jedoch wichtig zu verstehen, was ein Modell.

### <a name="defining-the-model"></a>Definieren des Modells

Ein Modell in die Bibliothek des **Serialisierungsprogramms** definiert die Ereignisse, die das Gerät IoT Hub und *bezeichnet die Modellierungssprache erhalten können* Nachrichten senden können. Definieren ein Modells mit einer Reihe von C-Makros in den **Simplesample\_Amqp** -Beispiel:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Die **beginnt\_NAMESPACE** und **Ende\_NAMESPACE** Makros sowohl den Namespace des Modells als Argument akzeptieren. Es wird erwartet, dass zwischen diesen Makros die Definition der Modelle und Datenstrukturen ist, die Modelle verwenden.

In diesem Beispiel wird ein einheitliches **ContosoAnemometer**aufgerufen. Dieses Modell definiert zwei Ereignisse, die das Gerät IoT Hub senden können: **DeviceId** und **Windstärke**. Außerdem definiert drei Aktionen (Nachrichten), die das Gerät empfangen können: **TurnFanOn**, **TurnFanOff**und **SetAirResistance**. Jedes Ereignis hat einen Typ, und jede Aktion verfügt über einen Namen (und optional eine Reihe von Parametern).

Ereignisse und Aktionen im Modell definierte definieren eine API-Oberfläche, mit denen Sie Ereignisse an IoT Hub senden sowie Antworten auf Nachrichten an das Gerät. Dies wird am besten durch ein Beispiel verstanden.

### <a name="sending-events"></a>Senden von Ereignissen

Das Modell definiert die Ereignisse, die Sie IoT Hub senden können. In diesem Beispiel bedeutet, dass einer der beiden Ereignisse definiert das **WITH_DATA** -Makro. Z. B. Wenn Sie ein Ereignis **Windstärke** IoT Hub senden möchten, umfasst Schritte dazu. Die erste ist der Datensatz zu senden:

```
myWeather->WindSpeed = 15;
```

Das zuvor definierte ermöglicht uns einen Member einer **Struktur**festlegen. Als Nächstes serialisieren wir das Ereignis zu senden:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Dieser Code serialisiert das Ereignis in einen Puffer ( **Ziel**verweist). Abschließend senden wir das Ereignis IoT Hub mit diesem Code:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Dies ist eine Hilfsfunktion in diesem Beispiel, die unsere serialisierten Ereignisses IoT Hub sendet:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Dieser Code ähnelt was wir in sahen der **Iothub\_Client\_Beispiel\_Amqp** Anwendung, in der wir eine Nachricht aus einem Bytearray erstellt und dann **IoTHubClient\_SendEventAsync** IoT Hub an. Danach wir das Nachricht Handle freigeben und serialisiert Datenpuffer früher zugeteilt.

Die zweite letzten Parameter der **IoTHubClient\_SendEventAsync** ist ein Verweis auf eine Callback-Funktion, die aufgerufen wird, wenn die Daten erfolgreich gesendet wurden. Hier ist ein Beispiel für eine Callback-Funktion:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Der zweite Parameter ist ein Zeiger auf den Benutzerkontext. Derselbe Zeiger wir an **IoTHubClient\_SendEventAsync**. In diesem Fall ist des Kontexts eines einfachen Leistungsindikators kann beliebig

Das ist alles Ereignisse senden soll. Links auf lediglich Nachrichten empfangen.

### <a name="receiving-messages"></a>Empfangen von Nachrichten

Empfangen einer Nachricht auf ähnliche Weise, wie Nachrichten in der **IoTHubClient** -Bibliothek funktioniert. Registrieren Sie eine Callback-Funktion:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Notieren Sie die Rückruffunktion aufgerufen wird, wenn eine Nachricht empfangen wird:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Dieser Code vorgegebenen ist für jede Lösung ist. Diese Funktion empfängt die Nachricht und übernimmt Weiterleitung an die entsprechende Funktion über den Aufruf von **Ausführen\_Befehl**. Die aufgerufenen Funktion hängt von der Definition der Aktionen in unserem Modell an dieser Stelle.

Wenn Sie eine Aktion in Ihrem Modell definieren, müssen Sie eine Funktion implementieren, die aufgerufen wird, wenn das Gerät die Nachricht empfängt. Angenommen, Ihr Modell diese Aktion definiert:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Sie müssen eine Funktion mit dieser Signatur definieren:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Beachten Sie, dass der Name der Funktion mit dem Namen der Aktion im Modell übereinstimmt und die Parameter der Funktion für die Aktion angegebenen Parametern übereinstimmt. Der erste Parameter ist immer erforderlich, und enthält einen Zeiger auf die Instanz des Modells.

Wenn das Gerät eine Nachricht, die dieser Signatur entspricht empfängt, wird die entsprechende Funktion aufgerufen. Also abgesehen Standardcode aus **IoTHubMessage**enthalten, Nachrichten nur wenigen definieren eine einfache Funktion für jede Aktion im Modell definiert.

### <a name="uninitializing-the-library"></a>Die Bibliothek deinitialisieren

Beim Senden und Empfangen von Nachrichten danach kann die Bibliothek IoT deinitialisieren:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Jede dieser drei Funktionen richtet die drei beschriebenen Initialisierungsfunktionen. Diese APIs aufrufen, stellt sicher, dass Sie zuvor reservierte Ressourcen frei.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel behandelt die Grundlagen der Verwendung der Bibliotheken in **Azure IoT Device SDK für C**. Darauf Sie genügend Informationen zu verstehen, was im SDK enthaltene Architektur und erste Schritte mit Windows-Beispiele. Im nächste Artikel wird die Beschreibung des SDK erklärt, [mehr über die IoTHubClient-Bibliothek](iot-hub-device-sdk-c-iothubclient.md).

Weitere Informationen zur Entwicklung für IoT Hub finden Sie unter [IoT Hub SDKs][lnk-sdks].

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Ein SDK Gateway IoT simulieren][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
