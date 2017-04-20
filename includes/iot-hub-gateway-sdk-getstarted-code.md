## <a name="typical-output"></a>Normale Ausgabe

Es folgt ein Beispiel für die Ausgabe von Hello World-Beispiel in die Protokolldatei geschrieben. Zeilenvorschub und Tabulatorzeichen wurden für Leserlichkeit hinzugefügt:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Codeausschnitte

Dieser Abschnitt beschreibt einige wichtige Teile des Codes in Hello World-Beispiel.

### <a name="gateway-creation"></a>Gateway erstellen

Der Entwickler muss *Gatewayprozess*schreiben. Dieses Programm erstellt die interne Infrastruktur (Broker), lädt die Module und richtet alles ordnungsgemäß funktioniert. Das SDK enthält die **Gateway_Create_From_JSON** -Funktion, um ein Gateway aus einer JSON-Datei starten können. Um die **Gateway_Create_From_JSON** -Funktion verwenden, müssen Sie es den Pfad JSON-Datei übergeben, die Module laden angibt. 

Suchen Sie den Code für den Gatewayprozess im Hello World-Beispiel in [main.c] [ lnk-main-c] Datei. Für Leserlichkeit zeigt Snippet unten eine verkürzte Version des Codes Prozess Gateway. Dieses Programm erstellt ein Gateway und wartet dann, bis der Benutzer **die EINGABETASTE** drücken, bevor Sie das Gateway Tränen. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

JSON-Einstellungsdatei enthält eine Liste der Module laden. Jedes Modul geben a:

- **Modulname**: einen eindeutigen Namen für das Modul.
- **Module_path**: der Pfad zu der Bibliothek, die das Modul enthält. Linux ist eine Datei so, unter Windows ist eine DLL-Datei.
- **Argumente**: das Modul Konfigurationsinformationen.

Die JSON-Datei enthält auch Links zwischen den Modulen, die an die Bank. Eine Verknüpfung verfügt über zwei Eigenschaften:
- **Quelle**: ein Modul aus der `modules` Abschnitt oder "\*".
- **Empfänger**: ein Modul aus der `modules` Abschnitt

Jede Verknüpfung definiert eine Nachrichtenroute und Richtung. Nachrichten von Modul `source` an das Modul übermittelt werden sollen `sink`. Die `source` kann "\*", dass Nachrichten von jedem Modul erhält `sink`.

Das folgende Beispiel zeigt die JSON-Einstellungsdatei Hello World-Beispiel unter Linux konfiguriert. Jede Nachricht von Modul erzeugten `hello_world` verbraucht Modul `logger`. Ein Modul erfordert Argument auf den Entwurf des Moduls ab. In diesem Beispiel Logger-Modul ein Argument ist der Pfad zur Ausgabedatei und das Hello World-Modul nicht Argumente:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hello World-Modul Nachricht veröffentlichen

Finden Sie den Code von der "Hello World" zum Veröffentlichen von Nachrichten in ['hello_world.c'] verwendet[ lnk-helloworld-c] Datei. Der folgende Ausschnitt zeigt Fassung zusätzliche Kommentare und einige Fehlerbehandlungscode für Leserlichkeit entfernt:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Hello World-Modul Verarbeitung

Hello World-Modul muss nicht zu anderen Modulen Broker veröffentlichen Nachrichten. Dadurch Implementierung des Rückrufs Nachricht im Modul "Hello World" eine Funktion keine Aktion ausgeführt.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Protokollierung Modul Nachricht veröffentlichen und verarbeiten

Protokollierung-Modul empfängt Nachrichten vom Broker und in eine Datei geschrieben. Alle Nachrichten veröffentlicht nie. Daher ruft der Code des Moduls Protokollierung niemals die Funktion **Broker_Publish** .

Die Funktion **Logger_Recieve** [logger.c] [ lnk-logger-c] Datei ist der Rückruf der Broker zum Übermitteln von Nachrichten Protokollierung Modul aufgerufen. Der folgende Ausschnitt zeigt Fassung zusätzliche Kommentare und einige Fehlerbehandlungscode für Leserlichkeit entfernt:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Nächste Schritte

Zur Verwendung von IoT Gateway SDK finden Sie Folgendes:

- [IoT Gateway SDK-Nachrichten Gerät Cloud mit einem simulierten Gerät Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK] [ lnk-gateway-sdk] auf GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md