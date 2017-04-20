<properties
 pageTitle="Azure IoT Hub Skalierung | Microsoft Azure"
 description="Beschreibt, wie Azure IoT Hub."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Skalierung IoT Hub

Azure IoT Hub kann bis zu einer Million gleichzeitig angeschlossene Geräte unterstützen. Weitere Informationen finden Sie unter [IoT Hub Preise][lnk-pricing]. Jede Einheit IoT Hub ermöglicht eine bestimmte Anzahl von täglichen Nachrichten.

Um die Lösung ordnungsgemäß skalieren, sollten Sie mit IoT Hub. Insbesondere sollten Sie den erforderlichen Spitzendurchsatz für folgende Vorgänge:

* Gerät-zu-Cloud-Nachrichten
* Cloud-zu-Gerät-Nachrichten
* Identität Registrierungsvorgängen

Zusätzlich Durchsatz Siehe [IoT Hub Kontingente und verringert][] und Entwerfen Ihrer Lösung entsprechend.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Gerät Cloud und Cloud Gerät Nachrichtendurchsatz

Die beste Möglichkeit, eine Lösung IoT Hub Größe ist Datenverkehr auf Basis pro Einheit ausgewertet.

Gerät-zu-Cloud-Nachrichten befolgen Sie diese Richtlinien konstanter Datendurchsatz.

| Ebene | Konstanter Datendurchsatz | Anhaltende Übertragungsrate |
| ---- | -------------------- | ------------------- |
| S1 | Bis 1111 KB pro Minute pro Einheit<br/>(1,5 GB/Tag/Einheit) | Durchschnitt der 278 Nachrichten pro Minute pro Einheit<br/>(400.000 Nachrichten/Tag pro Einheit) |
| S2 | Bis zu 16 MB pro Minute pro Einheit<br/>(22,8 GB/Tag/Einheit) | Durchschnittliche 4167 Nachrichten pro Minute pro Einheit<br/>(6 Millionen Nachrichten pro Tag pro Einheit) |
| S3 | Bis 814 MB pro Minute pro Einheit<br/>(1144.4 GB/Tag/Einheit) | Durchschnittliche 208,333 Nachrichten pro Minute pro Einheit<br/>(300 Millionen Nachrichten pro Tag pro Einheit) |

## <a name="identity-registry-operation-throughput"></a>Identität Registrierung Operation Durchsatz

IoT Hub Identität Registrierungsvorgängen sollen nicht Runtime-Operationen werden hauptsächlich zur gerätebereitstellung zusammenhängen.

Bestimmte Burst-Performance-Werte finden Sie unter [IoT Hub Kontingente und verringert][].

## <a name="sharding"></a>Sharding

Während ein einzelner IoT Hub an Millionen von Geräten skaliert werden, erfordert die Lösung manchmal Leistungsmerkmale, die ein einzelner IoT Hub nicht garantieren kann. In diesem Fall wird empfohlen, dass Geräte in mehrere IoT Hubs aufgeteilt. Mehrere IoT Hubs Datenverkehr Bursts glätten und die erforderlichen Durchsatz oder Operation Raten, die erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub Kontingente und verringert]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
