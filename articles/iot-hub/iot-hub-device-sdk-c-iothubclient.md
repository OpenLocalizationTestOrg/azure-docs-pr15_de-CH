<properties
    pageTitle="Azure IoT Device SDK c - IoTHubClient | Microsoft Azure"
    description="Weitere Informationen zur Verwendung der IoTHubClient-Bibliothek in Azure IoT Gerät-SDK für C"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT Device SDK für C-IoTHubClient über

Im [ersten Artikel](iot-hub-device-sdk-c-intro.md) dieser Reihe **Microsoft Azure IoT Device SDK für C#**vorgestellt. Dieser Artikel erklärt, dass SDK zwei Schichten der Architektur umfasst. Ist die **IoTHubClient** -Bibliothek die direkte Kommunikation mit IoT Hub verwaltet. Es gibt auch **Serialisierungsprogramm** -Bibliothek, die builds, zum Bereitstellen von Serialisierungsdiensten. In diesem Artikel bieten wir zusätzliche Details in der **IoTHubClient** -Bibliothek.

Im vorherige Artikel beschrieben, wie die **IoTHubClient** -Bibliothek verwenden, um Ereignisse an IoT Hub senden und Empfangen von Nachrichten. In diesem Artikel erweitert diese Diskussion zu genauer *beim* senden und Empfangen von Daten in den **Low-Level-APIs**erläutert. Außerdem erläutert Eigenschaften an Ereignisse anfügen (und Abrufen von Nachrichten) mit der Eigenschaft Behandlung von Funktionen in der **IoTHubClient** -Bibliothek. Außerdem bieten wir zusätzliche Erläuterung verschiedene Nachrichten von IoT Hub behandelt.

Artikel endet mit ein paar verschiedene Themen mehr über Anmeldeberechtigungen des Geräts und das Verhalten der **IoTHubClient** über Konfigurationsoptionen ändern.

Wir verwenden die **IoTHubClient** SDK-Beispielen erläutert. Wenn Sie nachvollziehen möchten, finden Sie unter der **Iothub\_Client\_Beispiel\_http** und **Iothub\_Client\_Beispiel\_Amqp** Anwendung in Azure IoT Gerät-SDK enthaltenen für c alles in den folgenden Abschnitten in diesen Beispielen veranschaulicht wird.

Sie finden im [Microsoft Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) GitHub Repository **Azure IoT Device SDK für C** und Einzelheiten der API der [C-API-Referenz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Die Low-Level-APIs

Im vorherige Artikel beschrieben die grundlegende Funktionsweise von **IotHubClient** im Rahmen der **Iothub\_Client\_Beispiel\_Amqp** Anwendung. Zum Beispiel erläutert wie mit dieser Bibliothek initialisiert.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Es wird beschrieben, wie Ereignisse mit diesem Funktionsaufruf zu senden.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Artikel beschrieben, wie Nachrichten durch eine Callback-Funktion registrieren.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Außerdem zeigte, wie Ressourcen mithilfe von Code wie im folgenden Artikel.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Jedoch sind diese APIs Companion Funktionen:

-   IoTHubClient\_r\_CreateFromConnectionString

-   IoTHubClient\_r\_SendEventAsync

-   IoTHubClient\_r\_SetMessageCallback

-   IoTHubClient\_r\_löschen


Diese Funktionen umfassen "LL" in der API-Name. Ansonsten sind die Parameter der einzelnen Funktionen Pendants nicht alles identisch. Das Verhalten dieser Funktionen unterscheidet sich jedoch in einem wichtigen Punkt.

Beim Aufruf von **IoTHubClient\_CreateFromConnectionString**, die zugrunde liegenden Bibliotheken erstellen einen neuen Thread, der im Hintergrund ausgeführt wird. Dieser Thread Ereignisse sendet und empfängt Nachrichten von IoT Hub. Dieser Thread wird erstellt, wenn "LL" APIs verwenden. Die Erstellung des Hintergrund-Threads ist eine Vereinfachung für Entwickler. Sie müssen nicht explizit Ereignisse senden und Empfangen von IoT Hub - erfolgt automatisch im Hintergrund kümmern. Im Gegensatz dazu enthalten die APIs "LL" expliziten Steuerung der Kommunikation mit IoT Hub Bedarf.

Um besser zu verstehen, betrachten wir ein Beispiel:

Beim Aufruf von **IoTHubClient\_SendEventAsync**, tatsächlich tun setzt das Ereignis in einem Puffer. Beim Aufrufen erstellt Hintergrundthread **IoTHubClient\_CreateFromConnectionString** ständig überwacht diesen Puffer und sendet alle Daten an IoT Hub. Dies geschieht im Hintergrund gleichzeitig, dass der Hauptthread andere Aufgaben ausführt.

Auch beim Registrieren einer Callback-Funktion für Nachrichten mit **IoTHubClient\_SetMessageCallback**, sind im SDK zu den Hintergrund-Thread die Rückruffunktion aufgerufen, wenn eine Nachricht empfangen, unabhängig vom primären Thread angewiesen.

Die APIs "LL" erstellen ein Hintergrundthreads. Stattdessen muss eine neue API aufgerufen werden, um explizit Daten senden und Empfangen von IoT Hub. Dies wird im folgenden Beispiel veranschaulicht.

Die **Iothub\_Client\_Beispiel\_http** im SDK enthaltene Anwendung veranschaulicht die Low-Level-APIs. In diesem Beispiel senden wir Ereignisse IoT Hub, mit Code wie dem folgenden:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Die ersten drei Zeilen die Nachricht erstellen, und die letzte Zeile sendet das Ereignis. Aber wie bereits erwähnt, senden"" das Ereignis bedeutet, dass die Daten einfach in einem Puffer platziert werden. Nichts über das Netzwerk übertragen wird, beim Aufrufen **IoTHubClient\_r\_SendEventAsync**. Tatsächlich Daten an IoT Hub eindringen, Sie müssen Aufrufen **IoTHubClient\_r\_DoWork**, wie im folgenden Beispiel:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Dieser Code (aus der **Iothub\_Client\_Beispiel\_http** Anwendung) wiederholt aufgerufen **IoTHubClient\_alles\_DoWork**. Jedem **IoTHubClient\_alles\_DoWork** wird aufgerufen, sendet einige Ereignisse aus dem Puffer IoT Hub und eine Nachricht in der Warteschlange an das Gerät gesendet wird. Letzteres bedeutet, dass wir eine Callback-Funktion für Nachrichten registriert dann der Rückruf aufgerufen wird (vorausgesetzt, dass alle Nachrichten in der Warteschlange befinden). Wir würden diese Callback-Funktion mit folgendem Code registriert haben:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Der Grund, **IoTHubClient\_r\_DoWork** bezeichnet in einer Schleife ist, jeden Aufruf sendet *einige* gepufferten Ereignisse IoT Hub und ruft *die nächste* Nachricht in die Warteschlange für das Gerät. Jeder Aufruf nicht garantiert alle gepufferten Ereignisse senden, oder um alle Nachrichten in die Warteschlange. Wenn alle Ereignisse im Puffer senden und fahren Sie mit weiteren Verarbeitung können Sie diese Schleife durch folgenden Code ersetzen:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Dieser Code ruft **IoTHubClient\_r\_DoWork** bis IoT Hub alle Ereignisse im Puffer gesendet wurden. Beachten Sie, dass dies nicht auch bedeutet, dass alle Nachrichten in der Warteschlange eingegangen. Grund dafür ist als deterministisch Aktion nicht "alle" Nachrichten überprüfen. Was geschieht, wenn Sie "alle" Nachrichten abrufen, jedoch ein an das Gerät gesendet nach? Eine bessere Möglichkeit, damit umzugehen ist mit programmiert. Die Callback-Funktion konnte z. B. einen Zeitgeber zurückgesetzt, jedes Mal, wenn sie aufgerufen wird. Dann schreiben Sie Logik, um die Verarbeitung fortzusetzen, wenn keine Nachrichten in den letzten *X* Sekunden eingegangen sind.

Wenn Sie fertig eindringendes Ereignisse und empfangen, müssen Sie die entsprechende Funktion Ressourcen zu bereinigen.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Grundsätzlich gibt es nur einen Satz von APIs zum Senden und Empfangen von Daten mit einem Hintergrundthread und einen weiteren Satz von APIs, die das gleiche ohne Hintergrund-Thread. Viele Entwickler bevorzugen die nicht alle APIs, aber die Low-Level-APIs sind nützlich, wenn der Entwickler explizite Kontrolle über Netzwerk-Übertragung. Beispielsweise sammeln einige Geräte Daten und nur eingehende Ereignisse in bestimmten Intervallen (beispielsweise einmal pro Stunde oder einmal pro Tag). Die Low-Level-APIs können Sie die explizit Steuerelement beim Senden und Empfangen von Daten vom IoT Hub. Andere werden der Einfachheit halber vorziehen, die Low-Level-APIs bieten. Alles auf den Hauptthread als einige Arbeit geschieht im Hintergrund.

Welches Modell entscheiden, müssen Sie konsequent die APIs, die Sie verwenden. Wenn nun durch Aufrufen von **IoTHubClient\_LL\_CreateFromConnectionString**, müssen Sie nur die entsprechenden untergeordneten APIs für alle Nachbereitung verwenden:

-   IoTHubClient\_r\_SendEventAsync

-   IoTHubClient\_r\_SetMessageCallback

-   IoTHubClient\_r\_löschen

-   IoTHubClient\_r\_DoWork

Das Gegenteil gilt auch. Wenn Sie mit **IoTHubClient\_CreateFromConnectionString**, verwenden Sie die nicht - LL-APIs für zusätzliche Verarbeitung.

In Azure IoT Gerät-SDK für C, finden Sie in der **Iothub\_Client\_Beispiel\_http** für ein vollständiges Beispiel für die Low-Level-APIs. Die **Iothub\_Client\_Beispiel\_Amqp** Anwendung verwiesen werden, um ein vollständiges Beispiel für die nicht - LL-APIs.

## <a name="property-handling"></a>Eigenschaft Behandlung

Bisher haben beschriebenen senden wir in den Hauptteil der Nachricht verweisen wurde. Betrachten Sie beispielsweise diesen Code:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

In diesem Beispiel sendet eine Nachricht an IoT Hub mit dem Text "Hello World". Allerdings können IoT Hub Eigenschaften jeder Nachricht an. Eigenschaften sind Name-Wert-Paare, die an die Nachricht angehängt werden können. Beispielsweise können wir den vorherigen Code eine Eigenschaft an die Nachricht anfügen:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Wir beginnen durch Anruf an **IoTHubMessage\_Eigenschaften** und das Handle der Botschaft. Wieder get ist eine **Karte\_BEHANDELN** Verweis, der zum Hinzufügen von Eigenschaften ermöglicht. Letzteres geschieht durch Aufruf **Karte\_AddOrUpdate**, der auf einer Karte akzeptiert\_HANDLE, der Eigenschaftenname und Wert der Eigenschaft. Mit dieser API können so viele Eigenschaften wie hinzufügen.

Wenn das Ereignis von **Ereignis**gelesen wird, kann der Empfänger Aufzählen der Eigenschaften und deren entsprechende Werte abrufen. Beispielsweise würde in .NET dies erfolgen auf die [Properties-Auflistung des EventData-Objekts](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Im vorherigen Beispiel haben wir ein Ereignis Eigenschaften anfügen, die wir IoT Hub senden. Eigenschaften können auch Nachrichten von IoT Hub zugeordnet. Wenn Eigenschaften aus einer Nachricht abgerufen werden soll, können wir in unserer Rückruffunktion Nachricht Code wie den folgenden:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Der Aufruf von **IoTHubMessage\_Eigenschaften** gibt die **Karte\_BEHANDELN** Verweis. Wir übergeben diese auf **Karte\_GetInternals** ein Verweis auf ein Array von Name-Wert-Paare (sowie die Anzahl der Eigenschaften). An diesem Punkt wird lediglich Eigenschaften Werte zu wollen wir auflisten.

Sie müssen Eigenschaften in der Anwendung verwenden. Allerdings benötigen Sie für Ereignisse festlegen oder Abrufen von Nachrichten, erleichtert die **IoTHubClient** -Bibliothek.

## <a name="message-handling"></a>Nachrichtenbehandlung

Wie bereits erwähnt, bei IoT Hub Ankunft reagiert die **IoTHubClient** -Bibliothek durch eine registrierte Callback-Funktion aufrufen. Gibt es ein Rückgabeparameter dieser Funktion, die eine zusätzliche Erklärung verdient. Auszug der Callback-Funktion in der **Iothub\_Client\_Beispiel\_http** Beispiel:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Beachten Sie, dass der Rückgabetyp **IOTHUBMESSAGE\_DISPOSITION\_Ergebnis** und in diesem Fall wir wieder **IOTHUBMESSAGE\_akzeptiert**. Sind andere Werte, die wir dieser Funktion zurückgeben kann, die ändern, wie die **IoTHubClient** -Bibliothek das Nachrichten-Callback reagiert. Hier sind die Optionen.

-   **IOTHUBMESSAGE\_akzeptiert** – die Meldung erfolgreich verarbeitet wurde. **IoTHubClient** -Bibliothek wird nicht erneut mit derselben Nachricht die Rückruffunktion aufgerufen.

-   **IOTHUBMESSAGE\_abgelehnt** -die Nachricht wurde nicht verarbeitet, und es gibt in Zukunft tun wollen. Die **IoTHubClient** -Bibliothek sollte die Rückruffunktion erneut mit derselben Nachricht nicht aufrufen.

-   **IOTHUBMESSAGE\_abgebrochen** -die Nachricht wurde nicht verarbeitet, aber die **IoTHubClient** -Bibliothek sollte die Rückruffunktion erneut mit derselben Nachricht.

Für die ersten beiden Rückgabecodes sendet die **IoTHubClient** -Bibliothek eine Nachricht an IoT Hub, dass die Nachricht aus der Warteschlange gelöscht und nicht erneut übermittelt. Das Ergebnis entspricht (die Nachricht wird aus der Warteschlange gelöscht), sondern gibt an, ob die Meldung akzeptiert oder abgelehnt wurde noch aufgezeichnet.  Diese Unterscheidung Aufzeichnung eignet sich an Absender der Nachricht Feedback überwachen und herausfinden, ob ein Gerät angenommen oder eine bestimmte Nachricht zurückgewiesen.

Im letzten Fall wird eine Nachricht auch an IoT Hub gesendet, aber es gibt an, dass die Nachricht zugestellt sollten. Normalerweise wird eine Nachricht verlassen, wenn einige Fehler jedoch versuchen, die Nachricht erneut verarbeiten. Im Gegensatz dazu eignet Ablehnen einer Nachricht beim Auftreten eines nicht behebbaren Fehlers (oder einfach nicht zur Verarbeitung der Nachricht werden sollen).

In jedem Fall Beachten der unterschiedlichen Rückgabecodes, damit das Verhalten hervorrufen können aus der **IoTHubClient** -Bibliothek.

## <a name="alternate-device-credentials"></a>Anderes geräteanmeldeinformationen

Wie bereits erläutert, wird zunächst beim Arbeiten mit der **IoTHubClient** -Bibliothek zu einer **IOTHUB\_CLIENT\_BEHANDELN** mit einem Aufruf wie den folgenden:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Die Argumente für **IoTHubClient\_CreateFromConnectionString** die Verbindungszeichenfolge für das Gerät und einen Parameter, der das Protokoll gibt es IoT Hub zu verwenden sind. Die Verbindungszeichenfolge hat ein Format, das wie folgt aussieht:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Es gibt vier Arten von Informationen in dieser Zeichenfolge: IoT Hub Name, IoT Hub Suffix, Geräte-ID und freigegebenen Schlüssel. Der vollqualifizierte Domänenname (FQDN) des IoT Hub zu erhalten, beim Erstellen der Instanz IoT Hub in Azure-Portal – dadurch IoT-Hubnamen (der erste Teil des vollqualifizierten Domänennamens) und IoT Hub Suffix (der Rest des FQDN). Wenn Sie Ihr Gerät mit IoT Hub registrieren (wie im [vorherigen Artikel](iot-hub-device-sdk-c-intro.md)beschrieben), erhalten Sie die Geräte-ID und Schlüssel Zugriff freigegeben.

**IoTHubClient\_CreateFromConnectionString** bietet eine Möglichkeit, die Bibliothek initialisiert werden. Alternativ können Erstellen einer neuen **IOTHUB\_CLIENT\_BEHANDELN** mithilfe dieser einzelnen Parameter anstelle der Verbindungszeichenfolge. Dies geschieht durch folgenden Code:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Dies führt das gleiche wie **IoTHubClient\_CreateFromConnectionString**.

Es mag einleuchtend verwenden möchten **IoTHubClient\_CreateFromConnectionString** diese ausführlicher Methode der Initialisierung. Beachten Sie jedoch, dass beim Registrieren eines Geräts in IoT Hub erhalten Sie eine Geräte-ID und Geräteschlüssel (keine Verbindungszeichenfolge). Eingeführt im [vorherigen Artikel](iot-hub-device-sdk-c-intro.md) **Gerätemanager** SDK-Tool wird mit Bibliotheken in **Azure IoT Service SDK** Verbindungszeichenfolge Device ID, Gerät und IoT Hub Hostnamen erstellt. So fordert **IoTHubClient\_r\_erstellen** vorzuziehen, da Sie den Schritt zum Erstellen einer Verbindungszeichenfolge gespeichert. Verwenden Sie die Methode passt.

## <a name="configuration-options"></a>Konfigurationsoptionen

Bisher entspricht alles über die Funktionsweise die **IoTHubClient** -Bibliothek sein Standardverhalten. Es gibt jedoch einige Optionen, die Sie festlegen können, wie die Bibliothek funktioniert. Dies geschieht durch Nutzung der **IoTHubClient\_alles\_SetOption** API. Beispiel:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Es gibt verschiedene Optionen, die häufig verwendet werden:

-   **SetBatching** (boolesch) – Wenn **true**und IoT Hub gesendeten Daten in Batches gesendet. Wenn **false**, dann Nachrichten einzeln gesendet werden. Der Standardwert ist **false**. Beachten Sie, dass die **SetBatching** -Option nur das HTTP-Protokoll und nicht MQTT oder AMQP-Protokolle.

-   **Zeitlimit** (unsigned Int) – dieser Wert wird in Millisekunden dargestellt. Wenn eine HTTP-Anforderung oder eine Antwort länger als in dieser Zeit und Timeout der Verbindung erreicht.

Die Batchverarbeitung Option ist wichtig. Standardmäßig die Bibliothek druckflüssigkeit Ereignisse einzeln (ein einzelnes Ereignis ist was Sie, **IoTHubClient\_r\_SendEventAsync**). Wenn die Batchverarbeitung Option **true**ist, sammelt die Bibliothek aus dem Puffer (bis die maximale Nachrichtengröße, die akzeptiert IoT Hub) können Ereignisse.  Der Ereignisbatch ist IoT Hub gesendet, in einem einzigen HTTP-Aufruf (Ereignisse werden in eine JSON-Array zusammengefasst). Aktivieren der Batchverarbeitung normalerweise ergibt große Leistungsgewinne da Sie Netzwerkroundtrips reduzieren. Bandbreite wird auch erheblich reduziert, da Sie einen Satz von HTTP-Header mit einem Ereignisbatch keine Reihe von Headern für jedes einzelne Ereignis senden. Wenn Sie einen bestimmten Grund dafür haben, sollten Sie normalerweise Batchverarbeitung aktivieren.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt ausführlich das Verhalten der **IoTHubClient** -Bibliothek in **Azure IoT Device SDK c**gefunden. Diese Informationen benötigen Sie ein gutes Verständnis der Funktionen der **IoTHubClient** -Bibliothek. Im [nächsten Artikel](iot-hub-device-sdk-c-serializer.md) enthält ähnliche Informationen über Bibliothek des **Serialisierungsprogramms** .

Weitere Informationen zur Entwicklung für IoT Hub finden Sie unter [IoT Hub SDKs][lnk-sdks].

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
