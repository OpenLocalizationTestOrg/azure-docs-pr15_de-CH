<properties
 pageTitle="IoT Hub MQTT Support | Microsoft Azure"
 description="Beschreibung des MQTT auf IoT Hub-Unterstützung"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT Hub MQTT support

IoT Hub ermöglicht Kommunikation mit IoT Hub Gerät Endpunkte mit [MQTT v3.1.1] [ lnk-mqtt-org] Protokoll Port 8883 oder MQTT v3.1.1 über das WebSocket-Protokoll Port 443. IoT Hub muss alle Gerätekommunikation mithilfe von TLS/SSL gesichert werden (daher IoT Hub nicht unterstützen nicht sichere Verbindung über Port 1883).

## <a name="connecting-to-iot-hub"></a>Verbindung IoT Hub

Ein Gerät können MQTT Protokoll einen IoT Hub entweder mit [Microsoft Azure IoT SDKs] Bibliotheken herstellen[ lnk-device-sdks] oder direkt mit der MQTT Protokoll.

## <a name="using-the-device-client-sdks"></a>Mithilfe des geräteclients SDKs

[Gerät Client SDKs] [ lnk-device-sdks] , unterstützen das Protokoll MQTT stehen für Java, Node.js C, C# und Python. Gerät Client SDKs verwenden standard IoT Hub herstellen eine Verbindung IoT Hub. Um das MQTT-Protokoll verwenden, muss der Client-Protokoll-Parameter **MQTT**festgelegt werden. Standardmäßig Gerät Client SDKs ein IoT Hub mit **CleanSession** verbinden flag auf **0** gesetzt und **QoS-1** für den Nachrichtenaustausch mit IoT Hub verwenden.

Wenn ein Gerät einen IoT Hub verbunden ist, der Gerät Client SDKs Methoden, mit denen das Gerät senden und Empfangen von Nachrichten von einem IoT Hub bereitstellen.

In der folgenden Tabelle enthält Links zu Codebeispielen für jede unterstützte Sprache und gibt den Parameter Verbindung IoT Hub mit dem MQTT-Protokoll verwenden.

| Sprache                   | Protokoll-parameter        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure Iot-Gerät mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migrieren einer app Gerät von AMQP zu MQTT
Verwenden der [Geräte Client SDKs][lnk-device-sdks], mit AMQP, MQTT Wechsel erfordert Startparameter Initialisierung Client ändern, wie bereits erwähnt.

Dabei überprüfen Sie Folgendes:

* AMQP zurückgegeben Fehler für viele Zwecke, während MQTT die Verbindung beendet wird. Daher müssen die Ausnahmebehandlung Logik geändert.
* MQTT unterstützt keine Vorgänge *Ablehnen* , beim Empfangen von [Nachrichten C2D][lnk-messaging]. Back-End benötigt eine Antwort vom Gerät app, sollten [direkte]Methoden[lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Verwenden das MQTT Protokoll direkt

Ein Gerät Client SDKs verwenden kann, können mit öffentlichen Geräte Endpunkte mit dem Protokoll MQTT noch verbinden. Im Paket **Verbinden** verwenden das Gerät die folgenden Werte:

- Verwenden Sie für das Feld **ClientId** **DeviceId**. 
- Verwenden Sie für das Feld **Benutzername** `{iothubhostname}/{device_id}`, wobei {Iothubhostname} die vollständige CName IoT Hub.

    IoT Hub **contoso.azure devices.net** ist und der Name Ihres Geräts ist **MyDevice01**, Feld für den vollständige **Benutzernamen** sollten beispielsweise `contoso.azure-devices.net/MyDevice01`.

- Verwenden Sie für **das Kennwortfeld** ein SAS-Token. Das Format der SAS-Token entspricht der Protokolle HTTP und AMQP:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Weitere Informationen zu SAS-Token finden Sie im Abschnitt Gerät [verwenden IoT Hub Sicherheitstoken][lnk-sas-tokens].
    
    Beim Testen können Sie auch das [Gerät Explorer] [ lnk-device-explorer] Tool schnell ein SAS-Token zu generieren, die Sie kopieren und in Ihren eigenen Code einfügen:
    
    1. Wechseln Sie zur Registerkarte **Management** im Gerät Explorer.
    2. Klicken Sie auf **SAS-Token** (oben rechts).
    3. Auf **SASTokenForm**, wählen Ihr Gerät **DeviceID** Dropdown-Liste. Legen Sie die **Gültigkeitsdauer**.
    4. Klicken Sie auf **generieren** , um das Token zu erstellen.
    
    SAS-Token, das generiert wird, hat diese Struktur:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Teil dieses Token wie **das Kennwortfeld mit MQTT** ist:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT verbinden und trennen Pakete IoT Hub Probleme mit zusätzlichen Informationen zur Behandlung von Verbindungsproblemen kann ein Ereignis auf dem Kanal **Vorgänge überwachen** .

### <a name="sending-messages-to-iot-hub"></a>Senden von Nachrichten an IoT Hub

Stellen Sie eine Verbindung, ein Gerät kann Nachrichten an IoT Hub mit `devices/{device_id}/messages/events/` oder `devices/{device_id}/messages/events/{property_bag}` **Thema**. Die `{property_bag}` Element aktiviert das Gerät zum Senden von Nachrichten mit zusätzlichen Eigenschaften in einem Url-codierten Format. Zum Beispiel:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Diese `{property_bag}` Element verwendet dieselbe Codierung für Abfragezeichenfolgen in das HTTP-Protokoll.

Die Clientanwendung Gerät können auch `devices/{device_id}/messages/events/{property_bag}` als das **Thema wird** als Telemetrie Nachricht weitergeleitet *werden Nachrichten* definieren.

IoT Hub unterstützt QoS-2 Nachrichten nicht. Gerät-Client eine Nachricht mit **QoS-2**veröffentlicht, schließt IoT Hub des Netzwerks.
IoT Hub ist nicht beibehalten Nachrichten dauerhaft. Ein Gerät sendet eine Nachricht mit **beibehalten** Flag auf 1 gesetzt IoT Hub fügt der **X-opt-behalten** Application-Eigenschaft der Nachricht. In diesem Fall übergeben statt beibehalten der Nachricht beibehalten, IoT Hub für die Back-End-Anwendung.

### <a name="receiving-messages"></a>Empfangen von Nachrichten

Um Nachrichten von IoT Hub sollte ein abonnieren, mit `devices/{device_id}/messages/devicebound/#` als **Thema filtern**. Mehrstufige Platzhalter **#** im Thema Filter wird nur für das Gerät in den Themennamen zusätzliche Eigenschaften angezeigt. IoT Hub erlaubt nicht die Verwendung der **#** oder **?** Platzhalter für die Filterung von Themen. Da IoT Hub keine allgemeine Pub-Sub messaging Broker, unterstützt nur die dokumentierten Themen und Thema Filter.

Bitte beachten Sie, dass das Gerät nicht erhalten Fehlermeldungen IoT Hub, bevor es Endpunkts bestimmte Gerät abonniert hat dargestellt von der `devices/{device_id}/messages/devicebound/#` Thema filtern. Nach dem erfolgreichen Abonnement eingerichtet wurde, beginnt des Geräts empfangen nur Cloud-Geräte, die sie wieder das Abonnement gesendet wurde. Verbindet das Gerät mit **CleanSession** -Flag auf **0**festgelegt wird das Abonnement in anderen Sitzungen beibehalten werden. In diesem Fall erhalten weiter mit **CleanSession 0** Gerät verbindet Nachrichten, die sie offline wurde gesendet wurde. Wenn das Gerät **CleanSession** -Flag auf **1** festgelegt, wenn es nicht Fehlermeldungen IoT Hub erhalten bis Device-Endpunkt abonniert.

IoT Hub übermittelt Nachrichten mit dem **Thema** `devices/{device_id}/messages/devicebound/`, oder `devices/{device_id}/messages/devicebound/{property_bag}` gibt es Eigenschaften. `{property_bag}`Url-codierte Schlüssel-Wert-Paaren von Nachrichteneigenschaften. Nur Anwendungseigenschaften und benutzerdefinierbare Systemeigenschaften (oder **MessageId** **CorrelationId**) sind in der Eigenschaftensammlung enthalten. System-Eigenschaftennamen enthalten das Präfix **$**, Anwendungseigenschaften verwenden den ursprünglichen Eigenschaftennamen ohne Präfix.

Wenn ein Thema mit **QoS-2**ein Gerät Client abonniert gewährt IoT Hub maximale QoS-1 im Paket **SUBACK** . Danach liefern IoT Hub Nachrichten mit QoS-1 Gerät.

### <a name="additional-considerations"></a>Weitere Aspekte

Als endgültige Gegenleistung MQTT Protokollverhalten auf Cloud anpassen möchten Sie sollten [Azure IoT Protokollgateway] [ lnk-azure-protocol-gateway] , ermöglicht leistungsfähige benutzerdefinierte Protokollgateway bereitstellen, die direkt mit IoT Hub. Azure IoT Protokollgateway können Sie das Gerät Protokoll Brache MQTT Bereitstellung aufnehmen oder andere benutzerdefinierte Protokolle anpassen. Dieser Ansatz ist jedoch erforderlich: ausführen und benutzerdefinierte Protokollgateway betreiben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Hinweise MQTT unterstützen] [ lnk-mqtt-devguide] in Azure IoT Hub-Entwicklerhandbuch.

Über das MQTT-Protokoll finden Sie in der [Dokumentation MQTT][lnk-mqtt-docs].

Informationen zum Planen der Bereitstellung IoT Hub finden Sie unter:

- [Azure zertifiziert für IoT gerätekatalog][lnk-devices]
- [Unterstützung für weitere Protokolle][lnk-protocols]
- [Vergleichen Sie mit Event Hubs][lnk-compare]
- [Skalieren, HA und DR][lnk-scaling]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
