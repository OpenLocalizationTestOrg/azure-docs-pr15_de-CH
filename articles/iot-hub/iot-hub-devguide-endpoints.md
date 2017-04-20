<properties
 pageTitle="Entwicklerhandbuch - Endpunkte IoT Hub | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - Referenzinformationen IoT Hub Endpunkte"
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

# <a name="reference---iot-hub-endpoints"></a>Referenz - Endpunkte IoT Hub

## <a name="list-of-iot-hub-endpoints"></a>Liste der Endpunkte IoT Hub

Azure IoT Hub ist ein Multi-Tenant-Dienst, der seine Funktionalität für Akteure macht. Das folgende Diagramm zeigt den verschiedenen Endpunkten, IoT Hub verfügbar macht.

![IoT Hub Endpunkte][img-endpoints]

Im folgenden finden eine Beschreibung der Endpunkte:

* **Ressourcenanbieter**. Der Ressourcenanbieter IoT Hub macht eine [Azure-Ressourcen-Manager] [ lnk-arm] Schnittstelle, ermöglicht Azure Abonnementbesitzer erstellen und Löschen von IoT Hubs IoT Hub Eigenschaften aktualisieren. IoT Hub Eigenschaften steuern [Hub auf Sicherheitsrichtlinien][lnk-accesscontrol], Zugriffskontrolle auf und funktionale Optionen für Cloud-Gerät und Gerät Cloud messaging. IoT Hub-Ressourcenproviders können Sie zudem [Gerät Identitäten exportieren][lnk-importexport].
* **Identitätsmanagement Gerät**. Jede IoT Hub stellt eine Reihe von HTTP-REST-Endpunkte Gerät Identitäten verwalten (erstellen, abrufen, aktualisieren und löschen). [Geräte-Identitäten] [ lnk-device-identities] Authentifizierung und Steuerung verwendet.
* **Gerät und Management**. Jede IoT Hub stellt eine Reihe von Service-Facing HTTP-REST-Endpunkt zum Abfragen und Aktualisieren von [Gerät im Vergleich] [ lnk-twins] (update Tags und Eigenschaften).
* **Aufträge-Management**. Jede IoT Hub stellt eine Reihe von Service-Facing HTTP-REST-Endpunkt Abfragen und Verwalten von [Aufträgen][lnk-jobs].
* **Gerät-Endpunkte**. Für jedes Gerät in der Registrierung des Geräts Identität bereitgestellt macht IoT Hub Endpunkte ein Gerät zum Senden und Empfangen von Nachrichten verwenden kann:
    - *Gerät-zu-Cloud-Nachrichten senden*. Verwenden Sie diesen Endpunkt [Gerät Cloud-Nachrichten]gesendet[lnk-d2c].
    - *Cloud-zu-Gerät-Nachrichten empfangen*. Ein Gerät verwendet diesen Endpunkt gezielte [Cloud Gerät Nachrichten]empfangen[lnk-c2d].
    - *Datei-Uploads zu initiieren*. Ein Gerät verwendet diesen Endpunkt zu einer Azure Storage SAS-URI IoT Hub hochladen [eine Datei][lnk-upload].
    - *Abrufen und aktualisieren und Eigenschaften*. Ein Gerät verwendet diese Endpunkte auf seine [Geräte und][lnk-twins]Eigenschaften.
    - *Die direkten Methoden Anfragen empfangen*. Ein Gerät verwendet diese Endpunkte [direkten Methoden]anhören[lnk-methods]Anfragen.

    Diese Endpunkte verfügbar gemacht werden mit [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 und [1.0 AMQP] [ lnk-amqp] Protokolle. Beachten Sie, dass AMQP auch über [WebSockets] [ lnk-websockets] auf Port 443.
    
    Endpoins Zwillinge und Methoden stehen nur mit [MQTT v3.1.1][lnk-mqtt].

* **Endpunkte**. Jede IoT Hub stellt eine Reihe von Endpunkten, Ihre Back-End der Anwendung mit Geräten kommunizieren kann. Diese Endpunkte sind derzeit nur verfügbar gemachte [AMQP] [ lnk-amqp] Protokoll außer der Methode Aufruf Endpunkt, der über HTTP 1.1 verfügbar gemacht werden.
    - *Gerät-zu-Cloud-Nachrichten empfangen*. Dieser Endpunkt ist [Azure Ereignis Hubs]mit[lnk-event-hubs]. Back-End-Dienst können alle [Geräte Cloud-Nachrichten] lesen[ lnk-d2c] Geräte gesendet.
    - *Cloud-Gerät senden und Empfangen von Empfangsbestätigungen Lieferung*. Diese Endpunkte ermöglichen die Anwendung Backend zuverlässige [Cloud Gerät Nachrichten]senden[lnk-c2d], und die entsprechende Lieferung oder Ablauf Empfangsbestätigungen.
    - *Datei-Benachrichtigung empfangen*. Messaging-Endpunkt können Sie Ihre Geräte erfolgreich eine Datei Hochladen von Benachrichtigungen. 
    - *Direkter Aufruf*. Dieser Endpunkt kann einen Back-End-Dienst eine [direkte Methode] [ lnk-methods] auf einem Gerät.

[IoT Hub APIs und SDKs] [ lnk-sdks] Artikel beschreibt die verschiedenen Methoden auf diese Endpunkte.

Schließlich ist zu beachten, dass alle IoT Hub Endpunkte [TLS] verwenden[ lnk-tls] Protokoll und kein Endpunkt ist immer verfügbar unverschlüsselt/unsichere Kanäle.

## <a name="field-gateways"></a>Feld-gateways

Befindet sich in einer Projektmappe IoT *Feld Gateway* zwischen Ihren Geräten und IoT Hub Endpunkte. Es liegt in der Regel Ihre Geräte. Ihre Geräte kommunizieren direkt mit dem Feld Gateway über ein Protokoll von den Geräten unterstützt. Das Feld Gateway verbindet anliegenden IoT Hub mithilfe eines Protokolls IoT Hub unterstützt. Feld Gateway kann sehr spezielle Hardware oder eine energieeffiziente Computer mit Software, die End-to-End-Szenario führt für das Gateway bestimmt ist.

[Azure IoT Gateway SDK] verwenden[ lnk-gateway-sdk] Feld Gateway implementiert. Dieses SDK bietet spezifische Funktionen wie die Kommunikation von Geräten auf derselben Verbindung IoT Hub bündeln.

## <a name="next-steps"></a>Nächste Schritte

Andere Referenzthemen in diesem Handbuch IoT Hub Entwickler enthalten:

- [Abfragesprache für im Vergleich, Methoden und Aufträge][lnk-devguide-query]
- [Kontingente und Drosselung][lnk-devguide-quotas]
- [IoT Hub MQTT support][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md