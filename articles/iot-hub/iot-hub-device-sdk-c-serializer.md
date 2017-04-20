<properties
    pageTitle="Azure IoT Device SDK c - Serialisierungsprogramm | Microsoft Azure"
    description="Weitere Informationen zur Verwendung der Bibliothek Serialisierungsprogramm in Azure IoT Gerät-SDK für C"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT Device SDK für C-Serialisierungsprogramm über

Im [ersten Artikel](iot-hub-device-sdk-c-intro.md) dieser Reihe **Azure IoT Device SDK für C#**vorgestellt. Im nächste Artikel bereitgestellt eine ausführlichere Beschreibung der [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). Dieser Artikel schließt Abdeckung des SDK durch eine detailliertere Beschreibung des verbleibenden Komponenten: Bibliothek des **Serialisierungsprogramms** .

Einführende Artikel beschrieben, wie die Bibliothek des **Serialisierungsprogramms** Ereignisse senden und Empfangen von IoT Hub verwenden. In diesem Artikel haben wir dieser Diskussion erweitern, indem eine vollständigere Erklärung zum Modellieren von Daten mit der Makrosprache **Serialisierungsprogramms** . Der Artikel enthält weitere Informationen, wie die Bibliothek Nachrichten serialisiert (auch in einigen Fällen steuern das Serialisierungsverhalten). Wir beschreiben auch einige Parameter, die Sie ändern können, die die Größe der Modelle bestimmen, die Sie erstellen.

Schließlich erneut Artikel einige Themen in früheren Artikeln Nachricht wie-Eigenschaft behandeln. Wie wir herausfinden, funktionieren diese Features dasselbe **Serialisierungsprogramm** Bibliothek mit der **IoTHubClient** -Bibliothek verwenden.

Alle in diesem Artikel beschriebenen **Serialisierungsprogramm** SDK-Beispielen basiert. Wenn Sie nachvollziehen möchten, finden Sie unter der **Simplesample\_Amqp** und **Simplesample\_http** Anwendung in Azure IoT Gerät-SDK für c enthalten

Sie finden im [Microsoft Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) GitHub Repository **Azure IoT Device SDK für C** und Einzelheiten der API der [C-API-Referenz](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-modeling-language"></a>Die Modellierungssprache

[Einführende Artikel](iot-hub-device-sdk-c-intro.md) dieser Serie eingeführt Modellierungssprache durch das Beispiel in **Azure IoT Device SDK c** der **Simplesample\_Amqp** Anwendung:

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

Wie Sie sehen können, die Modellierungssprache C Makros basiert. Beginnen Sie immer mit der Beschreibung **beginnen\_NAMESPACE** und mit **Ende\_NAMESPACE**. Es ist üblich, benennen den Namespace für Ihre Firma oder wie in diesem Beispiel das Projekt, das Sie bearbeiten.

Vorgänge innerhalb des Namespace sind Modelldefinitionen. In diesem Fall ist ein einzelnes Modell für ein Anemometer. Wieder das Modell Namen etwas, aber in der Regel heißt dies für das Gerät oder den Datentyp mit IoT Hub austauschen möchten.  

Modelle enthalten eine Definition der Ereignisse können Eindringen IoT Hub ( *Daten*) sowie Nachrichten IoT Hub ( *Aktionen*) erhalten. Wie aus dem Beispiel sehen können, haben Ereignisse einen Typ und einen Namen. Aktionen haben einen Namen und optionale Parameter (jeweils ein).

Was nicht in diesem Beispiel gezeigt werden zusätzliche Datentypen, die vom SDK unterstützt werden. Als Nächstes behandelt.

> [AZURE.NOTE] IoT Hub bezieht sich auf die Daten, die ein Gerät, als *Ereignisse sendet*während die Modellierungssprache als *Daten* (mit **WITH_DATA**) bezeichnet. IoT Hub bezieht sich auch auf die gesendeten Daten für Geräte wie *Nachrichten*, während die Modellierungssprache als *Aktionen* (mit **WITH_ACTION**definiert) bezeichnet. Beachten Sie, dass diese Begriffe in diesem Artikel synonym verwendet werden können.

### <a name="supported-data-types"></a>Unterstützte Datentypen

Die folgenden Datentypen werden in Modellen mit Bibliothek des **Serialisierungsprogramms** erstellt:

| Typ                    | Beschreibung                            |
|-------------------------|----------------------------------------|
| Doppelte                  | doppelt präzise Gleitkommazahl |
| int                     | 32-Bit-Ganzzahl                         |
| float                   | Gleitkommazahl mit einfacher Genauigkeit |
| lange                    | Ganzzahl vom Typ Long                           |
| int8\_t                 | 8-Bit-Ganzzahl                          |
| Int16\_t                | 16-Bit-Ganzzahl                         |
| Int32\_t                | 32-Bit-Ganzzahl                         |
| int64\_t                | 64-Bit-Ganzzahl                         |
| bool                    | boolescher Wert                                |
| ASCII\_Char\_Ptr        | ASCII-Zeichenfolge                           |
| EDM\_Datum\_Zeit\_VERSETZT | Datums-/ Uhrzeitoffset                       |
| EDM\_GUID               | GUID                                   |
| EDM\_Binär             | Binär                                 |
| Deklarieren Sie\_Struktur         | komplexer Datentyp                      |

Beginnen Sie mit dem letzten Datentyp. Die **DECLARE\_Struktur** komplexe Datentypen definieren Sie die Gruppierung der primitiven Typen. Diese Gruppen können Sie ein Modell definieren, die folgendermaßen aussieht:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Unser Modell enthält einzelne Ereignis eines Typs **TestType**. **TestType** ist ein komplexer Typ, der zusammen das **Serialisierungsprogramm** Modellierungssprache unterstützten primitiven Typen zeigen mehrere Member enthält.

Mit einem Modell können wir schreiben Code zum Senden von Daten an IoT Hub, der wie folgt aussieht:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Grundsätzlich jedes Mitglied der **Test** Struktur einen Wert zuweisen und Aufruf von **SendAsync** Datenereignis **Test** in die Cloud zu senden. **SendAsync** ist eine Hilfsfunktion, die ein Ereignis eines einzelnen IoT Hub sendet:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Diese Funktion das Ereignis Daten serialisiert und an IoT Hub mit **IoTHubClient\_SendEventAsync**. Dies ist der gleiche Code in früheren Artikeln (**SendAsync** kapselt die Logik in eine praktische Funktion) erläutert.

Eine Hilfsfunktion verwendet im vorherigen Code ist **GetDateTimeOffset**. Diese Funktion wandelt den Zeitpunkt in einen Wert vom Typ **EDM\_Datum\_Zeit\_VERSETZT**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Wenn Sie diesen Code ausführen, wird die folgende Meldung an IoT Hub gesendet:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Beachten Sie, dass die Serialisierung JSON ist das **Serialisierungsprogramm** Bibliothek generierte Format. Beachten Sie, dass jedes Mitglied der serialisierten JSON-Objekt Mitglied **TestType** übereinstimmt, die wir in unserem Modell definiert. Auch genau entsprechen den Werten im Code verwendet. Beachten Sie, dass die Binärdaten base64-codiert ist: "Wurde" ist die base64 Codierung {0 x 01, 0 x 02, 0 x 03}.

Dieses Beispiel veranschaulicht den Vorteil der Bibliothek **Serialisierungsprogramm** – ermöglicht die Cloud JSON an, ohne explizit mit der Serialisierung in unserer Anwendung. Wir kümmern müssen setzt die Werte der Ereignisse unser Modell und dann einfach APIs Ereignisse in die Cloud zu senden.

Mit diesen Informationen können wir Modelle, die den Bereich der unterstützten Datentypen, einschließlich komplexe Typen (Es konnte sogar komplexe Typen in anderen komplexen Typen enthalten). Aber er erzeugte JSON serialisiert im obigen Beispiel wird ein wichtiger Punkt. Bestimmt, *wie* wir Daten mit dem **Serialisierungsprogramm** Bibliothek senden genau wie JSON gebildet wird. Dieser Punkt ist es weiter erläutern werde.

## <a name="more-about-serialization"></a>Weitere Informationen über Serialisierung

Im vorherige Abschnitt zeigt ein Beispiel für die Ausgabe vom **Serialisierungsprogramm** Bibliothek. In diesem Abschnitt erklären wir, wie die Bibliothek Daten serialisiert und wie Sie dieses Verhalten mithilfe der Serialisierung APIs steuern können.

Um die Diskussion über die Serialisierung voraus, arbeiten wir mit einem neuen Modell basierend auf ein. Erstens bieten Hintergrundinformationen für dieses Szenario wollen wir Adresse.

Ein Modell, die Temperatur und Feuchtigkeit gemessen werden soll. Jedes Datenelement wird anders IoT Hub gesendet werden. Standardmäßig Thermostat druckflüssigkeit Temperatur-Ereignis alle 2 Minuten; Luftfeuchtigkeit Ereignis wird alle 15 Minuten fertigungsräumlichkeiten. Bei beiden Ereignisse fertigungsräumlichkeiten, muss einen Zeitstempel enthalten, der der Zeit zeigt, dass die entsprechenden Temperatur und Luftfeuchtigkeit gemessen wurde.

Dieses Szenario demonstrieren wir zwei Arten, das Datenmodell und erläutern wir die Auswirkung der serialisierten Ausgabe hat Modellierung.

### <a name="model-1"></a>Modell 1

Hier ist die erste Version eines Modells, das vorherige Szenario unterstützt:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Beachten Sie, dass das Modell zwei Ereignisse enthält: **Temperatur** und **Luftfeuchtigkeit**. Im Gegensatz zu den vorherigen Beispielen ist der Ereignistyp definiert mit **DECLARE\_Struktur**. **TemperatureEvent** enthält eine Messung und einen Zeitstempel. **HumidityEvent** enthält eine feuchtemessung und einen Zeitstempel. Dieses Modell ermöglicht uns eine natürliche Datenmodell für das oben beschriebene Szenario. Beim Senden eines Ereignisses in die Cloud senden wir entweder Temperatur/Timestamp oder eine Feuchtigkeit-Zeitstempel.

Wir schicken Temperatur Ereignis in die Cloud mit Code wie dem folgenden:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Wir werden hartcodierte Werte für Temperatur und Luftfeuchtigkeit im Beispielcode verwenden, aber angenommen, wir tatsächlich diese Werte abrufen von stichprobenweise die entsprechenden Sensoren auf den Thermostat.

Der obige Code verwendet **GetDateTimeOffset** -Hilfsprogramm, die zuvor eingeführt wurde. Dieser Code getrennt deutlich höher werden Gründen explizit der Serialisierung und senden. Der vorherige Code serialisiert Temperatur Ereignis in einen Puffer. **SendMessage** wird dann eine Hilfsfunktion (enthalten **Simplesample\_Amqp**), der das Ereignis IoT Hub sendet:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Dieser Code ist eine Teilmenge der im vorherigen Abschnitt beschrieben, damit wir darauf wieder hier geht **SendAsync** -Hilfsprogramm.

Wenn den vorherigen Code an das Ereignis Temperatur ausgeführt wird, wird diese serialisierte Form des Ereignisses IoT Hub gesendet:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Wir senden eine Temperatur vom Typ **TemperatureEvent** und diese Struktur enthält Mitglied **Temperatur** und **Zeit** . Dies spiegelt sich direkt in den serialisierten Daten.

Ebenso können wir ein Ereignis Feuchtigkeit mit diesem Code senden:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Die serialisierte Form an IoT Hub wird folgendermaßen angezeigt:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Dies ist wiederum wie erwartet.

Bei diesem Modell stellen wie Ereignisse können einfach hinzugefügt werden. Definieren Sie weitere Strukturen mit **DECLARE\_Struktur**, und das entsprechende Ereignis in das Modell mit **mit\_Daten**.

Nun, wir ändern das Modell so, dass dieselben Daten beinhaltet jedoch mit einer anderen Struktur.

### <a name="model-2"></a>Modell 2

Berücksichtigen Sie diese alternative Modell oben:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Wir haben in diesem Fall eliminiert die **DECLARE\_Struktur** Makros und einfach Daten aus einfachen Typen aus der Modellierungssprache Szenario definieren.

Für den Moment einfach ignorieren wir **das Ereignis** . Mit dieser Seite ist hier der Code Eindringen **Temperatur**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Dieser Code sendet serialisierten Ereignisses IoT Hub:

```
{"Temperature":75}
```

Und der Code für die Luftfeuchtigkeit Ereignis wie folgt:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Dieser Code an IoT Hub gesendet:

```
{"Humidity":45}
```

Bisher sind noch keine Überraschung. Jetzt ändern Sie Verwendung das SERIALIZE-Makro.

**SERIALIZE** Makro kann mehrere Ereignisse als Argumente annehmen. Dies ermöglicht das Ereignis **Temperatur** und **Feuchtigkeit** zusammen serialisiert und in einem Aufruf an IoT Hub senden:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tippen Sie vielleicht, dass das Ergebnis dieses Codes, dass zwei Ereignisse IoT Hub gesendet werden:

[

{"Temperature": 75},

{"Feuchtigkeit": 45}

]

In anderen Worten, erwarten Sie, dass dieser Code genauso wie **Temperatur** und **Feuchtigkeit** getrennt. Es ist lediglich eine Vereinfachung für beide Ereignisse im selben Aufruf an **SERIALIZE** übergeben. Das ist jedoch nicht der Fall. Stattdessen sendet der obige Code dieses Ereignis Daten IoT Hub:

{"Temperature": 75 "Feuchtigkeit": 45}

Dies mag seltsam da unser Modell **Temperatur** und **Feuchtigkeit** als zwei *separate* Ereignisse definiert:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Um den Punkt Modell nicht wir mehr dieser Ereignisse **Temperatur** und **Feuchtigkeit** in der gleichen Struktur befinden:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Wenn wir dieses Modell verwendet, wäre es leichter zu verstehen, wie **Temperatur** und **Feuchtigkeit** serialisierten Nachricht gesendet wird. Jedoch löschen möglicherweise nicht warum es so funktioniert, wenn Sie beide Ereignisse an **SERIALIZE** übergeben Modell 2.

Dies ist einfacher zu verstehen, sollten Sie die Annahmen, die Bibliothek des **Serialisierungsprogramms** vornehmen. Dadurch gehen wir zurück zu unserem Modell sinnvoll:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Denken dieses Modells objektorientierten. In diesem Fall haben wir ein physisches Gerät (ein) modellieren und das Gerät enthält Attribute wie **Temperatur** und **Luftfeuchtigkeit**.

Den gesamten Zustand unser Modell mit folgendem Code senden kann:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Wenn die Werte der Temperatur, Luftfeuchtigkeit und Uhrzeit festlegen, sehen wir eine Veranstaltung IoT Hub gesendet:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Manchmal können Sie nur *einige* Eigenschaften des Modells in die Cloud senden möchten (Dies ist besonders dann, wenn Ihr Modell zahlreiche Ereignisse enthält). Es empfiehlt sich, nur eine Teilmenge der Ereignisse, wie im vorherigen Beispiel senden:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Wenn Sie generiert genau das gleiche serialisierte Ereignis wie wir ein **TemperatureEvent** Mitglied **Temperatur** und **Zeit** definiert wie mit 1. In diesem Fall konnten wir genau das gleiche serialisierte Ereignis mit einem anderen Modell (2), denn wir **SERIALIZE** anders generiert.

Wichtig ist, die mehrere Ereignisse an **SERIALIZE,** übergeben, und jedes Ereignis eine Eigenschaft in einem JSON-Objekt ist.

Am besten hängt Sie und wie Sie Ihr Modell denken. Wenn Sie "Ereignisse" in die Cloud senden und jedes Ereignis einen definierten Satz von Eigenschaften enthält, wird der erste Ansatz sinnvoll. Verwenden Sie in diesem Fall **DECLARE\_Struktur** definieren die Struktur jedes Ereignis und klicken Sie dann im Modell mit den **mit\_Daten** Makro. Senden Sie jedes Ereignis wie im ersten Beispiel oben. Hierbei würden Sie ein Ereignis eines einzelnen nur **SERIALISIERUNGSPROGRAMM**übergeben.

Wenn Sie Ihr Modell in einer objektorientierten denken, kann der zweite Sie entsprechen. In diesem Fall die Elemente anhand **mit\_Daten** "Eigenschaften" des Objekts. Übergeben Sie welche Teilmenge von Ereignissen an **SERIALIZE** , je nach der "den Status des Objekts" möchten Sie in die Cloud senden möchten.

Unter Ansatz ist richtig oder falsch. Einfach Beachten Sie **Serialisierungsprogramm** Bibliothek Funktionsweise und wählen Sie beruhender Ansatz, der Ihren Bedürfnissen entspricht.

## <a name="message-handling"></a>Nachrichtenbehandlung

Bisher in diesem Artikel IoT Hub senden Ereignisse nur behandelt wurde und noch nicht angesprochen empfangen. Der Grund dafür ist, dass über Nachrichten, brauchen wir in einem [früheren Artikel](iot-hub-device-sdk-c-intro.md)weitgehend abgedeckt wurde. Wie in Artikel, Nachrichten verarbeiten, indem Sie eine Callback-Funktion registrieren:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Schreiben Sie anschließend die Rückruffunktion aufgerufen wird, wenn eine Nachricht empfangen wird:

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

Diese Implementierung von **IoTHubMessage** Ruft die spezifische Funktion für jede Aktion im Modell. Angenommen, Ihr Modell diese Aktion definiert:

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

**SetAirResistance** wird aufgerufen, wenn die Nachricht an Ihr Gerät gesendet wird.

Was wir noch nicht erläutert sieht noch die serialisierte Version der Nachricht. In anderen Worten, wenn Sie eine **SetAirResistance** -Nachricht an Ihr Gerät senden möchten sieht, die wie?

Wenn Sie eine Nachricht an ein Gerät senden, würde durch den Azure IoT Service SDK möchten. Weiterhin müssen wissen, welche Zeichenfolge an eine bestimmte Aktion aufrufen. Das allgemeine Format zum Senden von Nachrichten wird folgendermaßen angezeigt:

```
{"Name" : "", "Parameters" : "" }
```

Sie senden ein serialisiertes JSON-Objekt mit zwei Eigenschaften: **Name** ist der Name der Aktion (Meldung) und **Parameter** enthält die Parameter der Aktion.

Zum Aufrufen von **SetAirResistance** können Sie z. B. diese Meldung an ein Gerät senden:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Der Aktionsname muss exakt eine Aktion in Ihrem Modell definiert. Die Parameternamen müssen ebenfalls übereinstimmen. Beachten Sie Groß-/Kleinschreibung. **Name** und **Parameter** werden immer in Großbuchstaben. Achten Sie darauf, dass Groß-Aktionsname und Parameter im Modell. In diesem Beispiel heißt der Aktion "SetAirResistance" und nicht "Setairresistance".

In diesem Abschnitt beschriebenen alles Sie wenn Ereignisse senden und Empfangen von Nachrichten mit Bibliothek des **Serialisierungsprogramms** . Bevor Sie fortfahren, wir einige Parameter, die Sie konfigurieren können, die steuern, wie groß ist Ihr Modell behandelt.

## <a name="macro-configuration"></a>Makro-Konfiguration

Verwenden Sie die Bibliothek des **Serialisierungsprogramms** Azure c freigegeben Dienstprogramm Library Bestandteil des SDK zu entnehmen.
Wenn Azure Iot Sdks Repository von GitHub mit der Option - rekursiv geklont haben, finden dieses freigegebene Dienstprogrammbibliothek Sie:

```
.\\c\\azure-c-shared-utility
```

Wenn die Bibliothek nicht geklont haben, finden Sie es [hier](https://github.com/Azure/azure-c-shared-utility).

In der Dienstprogrammbibliothek freigegebene finden Sie im folgenden Ordner:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Dieser Ordner enthält eine Visual Studio-Projektmappe aufgerufen **Makro\_Utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Das Programm in dieser Lösung wird die **Makro\_utils.h** Datei. Ist ein Standardmakro\_utils.h im SDK enthaltene Datei. Mit dieser Lösung können Sie einige Parameter ändern und erstellen Sie anhand dieser Parameter die Header-Datei neu.

Sind die zwei wichtigsten Parameter mit **nArithmetic** und **nMacroParameters** in diesen zwei Linien in Makro definiert\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Diese Werte sind im SDK enthaltenen Standardparameter. Jeder Parameter hat folgende Bedeutung:

-   nMacroParameters-steuert, wie viele Parameter können in einer DECLARE\_Modell Makrodefinition.

-   nArithmetic-steuert die Gesamtzahl der Elemente in einem Modell zulässig.

Sind diese Parameter wichtig deshalb, weil sie steuern, wie groß Ihr Modell sein kann. Angenommen Sie, diese Modelldefinition:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Wie zuvor erwähnt, **DECLARE\_Modell** ist nur ein C-Makro. Die Namen des Modells und **mit\_Daten** -Anweisung (noch ein anderes Makro) sind Parameter **DECLARE\_Modell**. **nMacroParameters** definiert, wie viele Parameter Bestandteil können **DECLARE\_Modell**. Dies definiert, wie viele Ereignis- und Datendeklarationen haben können. Mit standardmäßig maximal 124 bedeutet dies so eine Modell mit einer Kombination von ca. 60 Aktionen und Ereignisse definieren. Wenn Sie versuchen, diese Grenze überschreiten, erhalten Sie Compilerfehler, die etwa wie folgt aussehen:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Der **nArithmetic** -Parameter ist mehr über die interne Funktionsweise der Makrosprache der Anwendung.  Sie steuert die Gesamtzahl der Elemente in Ihrem Modell einschließlich Makros **DECLARE_STRUCT** haben. Wenn Sie Compiler-Fehler wie diese starten, versuchen Sie **nArithmetic**erhöhen:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Wenn Sie diese Parameter ändern möchten, ändern Sie die Werte im Makro\_utils.tt Datei, kompilieren Sie das Makro\_Utils\_h\_generator.sln-Lösung, und führen Sie das kompilierte Programm. Wenn Sie dies tun, ein neues Makro\_utils.h Datei wird erstellt und in der. \\allgemeinen\\Inc. Verzeichnis.

Um die neue Version des Makros\_utils.h, **Serialisierungsprogramm** NuGet-Paket aus der Projektmappe entfernen und stattdessen das **Serialisierungsprogramm** Visual Studio-Projekt enthalten. Dies kann der Code für den Quellcode der Bibliothek des Serialisierungsprogramms kompiliert. Dies schließt das aktuelle Makro\_utils.h. Wenn Sie für möchten **Simplesample\_Amqp**, Starten von NuGet-Paket für das Serialisierungsprogramm Bibliothek aus der Projektmappe entfernen:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Fügen Sie diesem Projekt der Visual Studio-Projektmappe:

> . \\c\\Serialisierungsprogramm\\erstellen\\Windows\\serializer.vcxproj

Ihre Projektmappe sollte wie folgt aussehen:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Jetzt wird beim Kompilieren der Projektmappe den aktualisierten Makro\_utils.h in die Binärdatei enthalten ist.

Beachten Sie, dass diese Werte hoch genug erhöhen Compiler überschreiten kann. An diesem Punkt ist **nMacroParameters** der wichtigste Parameter mit dem betroffenen. C99-Spezifikation gibt an, dass mindestens 127 Parameter in einer Makrodefinition zulässig sind. Microsoft-Compiler die Spezifikation genau folgt und maximal 127, Sie **nMacroParameters** über den Standardwert erhöhen können. Andere Compiler können Sie (z. B. der GNU-Compiler unterstützt einen höheren Grenzwert).

Bisher haben wir alles behandelt **Serialisierungsprogramm** Bibliothek Programmieren wissen müssen. Vor Abschluss, wir einige Themen aus früheren Artikeln, denen Sie über Fragen möglicherweise erneut.

## <a name="the-lower-level-apis"></a>Die Low-Level-APIs

Der Schwerpunkt dieses Artikels handelt **Simplesample\_Amqp**. In diesem Beispiel wird die übergeordnete (der nicht "LL") APIs Ereignisse senden und Empfangen von Nachrichten. Wenn Sie diese APIs verwenden, führt ein Hintergrundthread kümmert sich Ereignisse senden und Empfangen von Nachrichten. Allerdings können Sie APIs (LL) auf niedrigerer Ebene zu beseitigen dieser Hintergrundthread explizite Steuerung beim Ereignisse senden oder Empfangen von Nachrichten aus der Cloud.

Wie im [vorherigen Artikel](iot-hub-device-sdk-c-iothubclient.md)beschrieben, gibt es eine Reihe von Funktionen, die APIs höherer Ebene aus:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_löschen

Diese APIs in demonstrierte **Simplesample\_Amqp**.

Es entspricht auch eine Reihe von Low-Level-APIs.

-   IoTHubClient\_r\_CreateFromConnectionString

-   IoTHubClient\_r\_SendEventAsync

-   IoTHubClient\_r\_SetMessageCallback

-   IoTHubClient\_r\_löschen

Beachten Sie, dass Low-Level-APIs genauso wie in früheren Artikeln beschrieben. Den ersten Satz von APIs können einen Hintergrundthread Ereignisse senden und Empfangen von Nachrichten behandeln soll. Den zweiten Satz von APIs werden ggf. die expliziten Steuerung beim Senden und Empfangen von Daten vom IoT Hub verwenden. Eine Reihe von APIs arbeiten auch mit Bibliothek des **Serialisierungsprogramms** .

Beispielsweise Verwendung von Low-Level-APIs **Serialisierungsprogramm** Bibliothek finden Sie unter der **Simplesample\_http** Anwendung.

## <a name="additional-topics"></a>Weitere Themen

Einigen Themen erwähnenswert sind wieder-Eigenschaft behandeln, mit alternativen geräteanmeldeinformationen und Konfigurationsoptionen. Alle Themen in einem [früheren Artikel](iot-hub-device-sdk-c-iothubclient.md)sind. Der wichtigste Punkt ist, dass all diese Funktionen mit Bibliothek des **Serialisierungsprogramms** arbeiten, wie mit der **IoTHubClient** -Bibliothek. Wenn Sie ein Ereignis aus dem Modell Eigenschaften zuordnen möchten Sie z. B. **IoTHubMessage\_Eigenschaften** und **Zuordnung**\_**AddorUpdate**, genauso wie zuvor beschrieben:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Das Ereignis wurde aus der Bibliothek **Serialisierungsprogramm** generiert oder manuell mithilfe der **IoTHubClient** -Bibliothek erstellt spielt keine Rolle.

Anderes Gerät Anmeldeinformationen mit **IoTHubClient\_LL\_erstellen** funktioniert ebenso gut als **IoTHubClient\_CreateFromConnectionString** für die Zuweisung einer **IOTHUB\_CLIENT\_BEHANDELN**.

Schließlich Wenn **Serialisierungsprogramm** Bibliothek verwenden, lassen sich Optionen mit **IoTHubClient\_r\_SetOption** wie Sie bei Verwendung der **IoTHubClient** -Bibliothek.

Eine Bibliothek des **Serialisierungsprogramms** ist sind die Initialisierung APIs. Bevor Sie können mit der Bibliothek arbeiten, rufen Sie **Serialisierungsprogramm\_Init**:

```
serializer_init(NULL);
```

Dies ist nur vor dem Aufruf von **IoTHubClient\_CreateFromConnectionString**.

Auch wenn Sie fertig sind mit der Bibliothek arbeiten, Sie machen, letzte Aufruf werden **Serialisierungsprogramm\_Deinit**:

```
serializer_deinit();
```

Andernfalls funktionieren alle oben aufgeführten Funktionen in die Bibliothek des **Serialisierungsprogramms** wie in der **IoTHubClient** -Bibliothek. Weitere Informationen zu diesen Themen finden Sie in der [vorherigen Artikel](iot-hub-device-sdk-c-iothubclient.md) dieser Reihe.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt ausführlich die besonderen Aspekte des **Serialisierungsprogramms** Bibliothek in **Azure IoT Device SDK c**enthalten. Informationen erhalten, sollten Sie gute Kenntnisse wie mit Modellen Ereignisse senden und Empfangen von Nachrichten von IoT Hub.

Dies schließt auch der dreiteiligen Reihe Anwendungsentwicklung mit **Azure IoT Device SDK für C**. Dies sollte genügend Informationen, um nicht nur Sie aber ein Verständnis der Funktionsweise der APIs bieten. Weitere Informationen gibt es Beispiele im SDK hier nicht behandelt. Andernfalls ist die [SDK-Dokumentation](https://github.com/Azure/azure-iot-sdks) eine gute Ressource für zusätzliche Informationen.


Weitere Informationen zur Entwicklung für IoT Hub finden Sie unter [IoT Hub SDKs][lnk-sdks].

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
