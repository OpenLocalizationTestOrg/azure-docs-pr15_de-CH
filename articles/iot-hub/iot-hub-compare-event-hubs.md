<properties
 pageTitle="Vergleich von Azure IoT Hub Azure Ereignis Hubs | Microsoft Azure"
 description="Ein Vergleich von Azure IoT Hub und Azure Ereignis Hubs Hervorhebung funktionale Unterschiede und Anwendungsfälle."
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
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergleich von Azure IoT Hub und Azure Ereignis Hubs

Die Hauptanwendungsfälle IoT Hub gehört zu Telemetrie von Geräten. Aus diesem Grund IoT Hub häufig [Azure Ereignis Hubs][]gegenüber. Wie IoT Hub ist Event Hubs eine Ereignisverarbeitungsregel Dienst, Ereignis und Telemetrie Eindringen in die Cloud in großem Maßstab mit geringer Latenz und hoher Zuverlässigkeit ermöglicht.

Die Dienste müssen jedoch viele Unterschiede in der folgenden Tabelle aufgeführt.

| Bereich | IoT Hub | Ereignis-Hubs |
| ---- | ------- | ---------- |
| Kommunikationsmuster | Aktiviert Gerät Cloud und Cloud-zu-Gerät-messaging. | Aktiviert nur Ereignis Eindringen (in der Regel für Szenarien Gerät Cloud betrachtet). |
| Unterstützung für Geräte-Protokoll | MQTT, AMQP, AMQP WebSockets und HTTP unterstützt. IoT Hub funktioniert außerdem mit dem [Azure IoT Protokollgateway][lnk-azure-protocol-gateway], eine anpassbare Protokoll Gateway Implementierung benutzerdefinierte Protokolle unterstützen. | AMQP AMQP WebSockets und HTTP unterstützt. |
| Sicherheit | Stellt pro Gerät Identität und widerrufliche Zugriffskontrolle. Siehe [Abschnitt Sicherheit IoT Hub-Entwicklerhandbuch]. | Ereignis-Hubs [freigegebene Zugriffsrichtlinien]bietet[Event Hubs - security], mit begrenzten Sperrung durch [Publisher Richtlinien]unterstützen[Event Hubs publisher policies]. IoT-Lösungen müssen häufig eine benutzerdefinierte Lösung zur Unterstützung von Anmeldeinformationen pro Gerät und Anti-spoofing Maßnahmen implementieren. |
| Überwachung von Vorgängen | Ermöglicht IoT umfassende Identity Management und Konnektivität Ereignisse Gerät Authentifizierungsfehler Drosselung und ungültiges Format Ausnahmen abonnieren. Diese Ereignisse können Sie Verbindungsprobleme auf einzelne schnell identifizieren. | Stellt nur eine Zusammenfassung der Kennzahlen. |
| Skalierung | Zur Unterstützung mehrerer Millionen Geräte gleichzeitig optimiert wird. | Begrenztere Anzahl Verbindungen – bis zu 5.000 AMQP Anschlüsse nach [Azure Service Bus Quoten][]unterstützen. Auf der anderen Seite können Ereignis Hubs Sie die Partition für jede gesendete Nachricht angeben. |
| Geräte-SDKs | Bietet [SDKs] [ Azure IoT Hub SDKs] für eine Vielzahl von Plattformen und Sprachen. | Wird für .NET und C. Außerdem bietet AMQP und HTTP-Schnittstellen senden. |
| Datei hochladen | Ermöglicht IoT Dateien von Geräten in die Cloud hochladen. Enthält einen Datei Benachrichtigung Endpunkt für Workflowintegration und eine Kategorie für die Debugunterstützung Überwachung von Vorgängen. | Verwendet eine Forderung Karomuster manuell Geräte Dateien anfordern und Geräte mit einem Speicher für die Buchung. |

Selbst ist der einzige Anwendungsfall Gerät Cloud Telemetrie Eindringen bietet IoT Hub einen Dienst, der speziell für IoT Device-Konnektivität. Weiterhin nutzen für diese Szenarien IoT-spezifische Funktionen erweitern. Ereignis-Hubs dient zum Ereignis Eindringen auf massiv, sowohl zwischen Rechenzentrum und Intra-Rechenzentrum.

Es ist nicht ungewöhnlich IoT Hub und Event-Hubs in der gleichen Projektmappe – wo IoT Hub Gerät Cloud-Kommunikation, und Ereignis Hubs später Ereignis eindringen zu Echtzeit-Verarbeitung verwenden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Planen der Bereitstellung IoT Hub finden Sie unter [Skalierung, HA und Dr.][lnk-scaling].

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Sicherheitsabschnitt der IoT Hub-Entwicklerhandbuch]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Service Bus Kontingente]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
