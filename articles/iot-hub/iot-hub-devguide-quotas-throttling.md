<properties
 pageTitle="Entwicklerhandbuch - Kontingente und Drosselung | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - Beschreibung der Kontingente auf IoT Hub und Drosselung erwartet"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="reference---quotas-and-throttling"></a>Referenz - Kontingente und Drosselung

## <a name="quotas-and-throttling"></a>Kontingente und Drosselung

Jede Azure-Abonnement können höchstens 10 IoT Hubs.

Jede IoT Hub eine bestimmte Anzahl von Einheiten in einem bestimmten SKU bereitgestellt wird (Weitere Informationen finden Sie unter [Azure IoT Hub Preise][lnk-pricing]). SKU und Anzahl Einheiten bestimmen tägliche Kontingent Nachrichten senden können.

Die SKU bestimmt auch die Drosselung Grenzwerte IoT Hub für alle Operationen erzwingt.

## <a name="operation-throttles"></a>Vorgang drosseln

Operation Drosselungen sind Rate Grenzen, die in Minuten Adressbereiche angewendet, und Missbrauch verhindert. IoT Hub vermeiden Sie Fehler nach Möglichkeit versucht, aber es überhaupt Ausnahmen verletzt die Schubkontrolle zu lang ist.

Folgendes ist die Liste der erzwungene drosseln. Werte beziehen sich auf einen einzelnen Hub.

| Drosselklappe | Wert pro hub |
| -------- | ------------- |
| Identität Registrierungsvorgängen (erstellen, abrufen, auflisten, aktualisieren und löschen) | 5000/min/Einheit (für S3) <br/> 100/min/Einheit (für S1 und S2). |
| Verbindung | 6000/Sek. / (für S3) 120/Sek./Einheit (für S2) 12/Sek./Einheit (für S1). <br/>Mindestens 100/Sekunde. <br/> Zwei S1 sind 2\*12 = 24/s, aber Sie müssen mindestens 100/s über Ihre Einheiten. Mit neun S1 108/s haben (9\*12) über Ihre Einheiten. |
| Gerät Cloud sendet | 6000/Sek. / (für S3) 120/Sek./Einheit (für S2) 12/Sek./Einheit (für S1). <br/>Mindestens 100/Sekunde. <br/> Zwei S1 sind 2\*12 = 24/s, aber Sie müssen mindestens 100/s über Ihre Einheiten. Mit neun S1 108/s haben (9\*12) über Ihre Einheiten. |
| Cloud-Gerät sendet | 5000/min / (für S3) 100/min/Einheit (für S1 und S2). |
| Cloud-Gerät empfangen | 50000/min / (für S3) 1000/min/Einheit (für S1 und S2). |
| Datei-Upload-Vorgänge | 5000 Dateiupload Benachrichtigungen/min/Einheit (für S3), 100 Datei hochladen Benachrichtigungen/min/Einheit (für S1 und S2). <br/> 10000 SAS-URIs möglich, ein Azure Storage-Konto gleichzeitig.<br/> 10 SAS-URIs-Gerät kann gleichzeitig aus. | 

Es ist wichtig zu verdeutlichen, dass *Verbindung* Schubkontrolle Rate steuert mit der neuen Verbindung IoT Hub und nicht die maximale Anzahl der gleichzeitig verbundenen Geräte hergestellt werden können. Die Schubkontrolle hängt von der Anzahl der Einheiten, die für den Hub bereitgestellt werden.

Wenn S1 Einheit kaufen, erhalten Sie Schubkontrolle 100 Verbindungen pro Sekunde. Dies bedeutet, dass um 100.000 Geräten mindestens 1000 Sekunden (16 Minuten dauert). Allerdings haben Sie beliebig viele gleichzeitig verbundenen Geräte Geräte in Ihrem Gerät identitätsregistrierung registriert haben.

Eine ausführliche Erläuterung der IoT Hub Drosselung Verhalten finden Sie im Blogbeitrag [IoT Hub Drosselung und][lnk-throttle-blog].

>[AZURE.NOTE] Zu jedem Zeitpunkt kann Kontingente oder Schubkontrolle Grenzen durch Erhöhen der Anzahl der bereitgestellten Einheiten IoT Hub erhöht.

>[AZURE.IMPORTANT] Identität Registrierungsvorgängen sind für die Verwendung in Device-Management und provisioning Szenarien vorgesehen. Lesen oder Aktualisieren einer großen Anzahl von Gerät Identitäten werden durch [Importieren und Exportieren von Aufträgen][lnk-importexport].

## <a name="next-steps"></a>Nächste Schritte

Andere Referenzthemen in diesem Handbuch IoT Hub Entwickler enthalten:

- [IoT Hub Endpunkte][lnk-devguide-endpoints]
- [Abfragesprache für im Vergleich, Methoden und Aufträge][lnk-devguide-query]
- [IoT Hub MQTT support][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md