<properties
 pageTitle="Azure IoT Hub Übersicht | Microsoft Azure"
 description="Überblick über Azure IoT Hub Service: Was ist Iot Hub, Geräte, Internet der Dinge Kommunikationsmuster und Kommunikationsmuster Service unterstützt"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Was ist Azure IoT Hub?

Willkommen bei Azure IoT Hub. Dieser Artikel bietet eine Übersicht über Azure IoT Hub und beschreibt, warum Sie diesen Dienst sollte um eine Internet der Dinge (IoT) Lösung zu implementieren. Azure IoT Hub ist ein vollständig verwalteter Dienst, der zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und Back-End Lösung ermöglicht. Azure IoT Hub:

- Bietet zuverlässige Gerät zu Cloud und Cloud-zu-Gerät-Ebene messaging.
- Ermöglicht sichere Kommunikation mit Anmeldeinformationen pro Gerät und Zugriffskontrolle.
- Bietet umfassende Überwachungsfunktionen für Geräte und Identity Management-Ereignisse.
- Bibliotheken für die am häufigsten verwendeten Sprachen und Plattformen Geräte umfasst.

Artikel [Vergleich IoT Hub und Ereignis Hubs] [ lnk-compare] beschreibt die wichtigsten Unterschiede zwischen diesen beiden Diensten und hebt die Vorteile von IoT Hub in IoT-Projektmappen.

![Azure IoT Hub Cloud Gateway in das Internet der Dinge-Lösung][img-architecture]

> [AZURE.NOTE] Eine ausführliche Erläuterung der IoT-Architektur finden Sie unter [Microsoft Azure IoT Referenzarchitektur][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT Gerätekonnektivität Probleme

IoT Hub und Gerätebibliotheken unterstützen Sie wie zuverlässig und sicher zu Lösung Geräte zu bewältigen. IoT-Geräte:

- Sind oft eingebettete Systeme menschlichen Operator.
- Remote-Standorte können teuer der physische Zugriff ist.
- Kann nur über Back-End Lösung erreicht werden.
- Möglicherweise macht und Ressourcen besitzen.
- Möglicherweise zeitweise langsame und teure Netzwerkkonnektivität.
- Müssen eigene, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
- Können mithilfe einer Reihe von gängigen Hardware- und Softwareplattformen.

Neben den oben genannten Vorschriften muss IoT-Lösung auch skalieren, Sicherheit und Zuverlässigkeit bieten. Die resultierende Konnektivitätsanforderungen ist schwierig und zeitaufwändig Verwendung traditioneller Technologie wie Web-Container und messaging Broker implementieren.

## <a name="why-use-azure-iot-hub"></a>Verwendung von Azure IoT Hub

Azure IoT Hub Adressen die Gerätekonnektivität Probleme auf folgende Weise:

-   **Pro Gerät Authentifizierung und sichere Verbindung**. Jedes Gerät mit eigenem [Sicherheitsschlüssel] bereitstellen[ lnk-devguide-security] zu Verbindung IoT Hub. [IoT Hub identitätsregistrierung] [ lnk-devguide-identityregistry] Gerät Identitäten und Schlüssel in einer Projektmappe gespeichert. Back-End Lösung können einzelne Geräte oder Listen ermöglichen die vollständige Kontrolle über den Zugriff verweigern.

-   **Überwachung der Device Connectivity-Operationen**. Sie erhalten detaillierte Protokolle zum Gerät Identität und der Konnektivität Ereignisse. Diese Überwachung ermöglicht Projektmappe IoT zu Verbindungsproblemen, wie Geräte, her mit falschen Anmeldeinformationen, zu häufig Nachrichten senden oder alle Cloud-zu-Gerät-Nachrichten ablehnen.

-   **Ein umfangreicher Satz von Bibliotheken für Geräte**. [Azure IoT Gerät SDKs] [ lnk-device-sdks] verfügbar sind und für verschiedene Sprachen und Plattformen – C für viele Linux-Distributionen, Windows und Echtzeit-Betriebssysteme unterstützt. Azure IoT Geräte-SDKs unterstützen auch verwaltete Sprachen wie C#, Java und JavaScript.

-   **IoT Protokolle und Erweiterbarkeit**. Wenn Ihre Lösung Gerätebibliotheken verwenden kann, macht IoT Hub ein öffentlicher Protokoll, Geräte direkt MQTT v3.1.1, HTTP 1.1 oder 1.0 AMQP Protokolle ermöglicht. Sie können auch IoT Hub, um benutzerdefinierte Protokolle unterstützen erweitern:

    - Erstellen eines Gateways Feld [Azure IoT Gateway SDK] [ lnk-gateway-sdk] , das benutzerdefinierte Protokoll in eines der drei Protokolle IoT Hub verständlich konvertiert. 
    - Anpassen von [Azure IoT Protokollgateway][protocol-gateway], eine open-Source-Komponente, die in der Cloud ausgeführt wird.

-   **Skalieren**. Azure IoT Hub skaliert Millionen von Geräten gleichzeitig zu Millionen Ereignisse pro Sekunde.

Diese Vorteile sind für viele Kommunikationsmuster. IoT Hub können Sie derzeit die folgenden spezifischen Kommunikationsweise implementieren:

-   **Event-basierte Gerät Cloud Einnahme.** IoT Hub können Geräte zuverlässig Millionen Ereignisse pro Sekunde erhalten. Sie können dann diese Langsamster Pfad verarbeitet mit einem Ereignis Prozessor-Modul. Es kann auch kalte Pfad zur Analyse Ihrer speichern. IoT Hub behält Daten für bis zu sieben Tage zuverlässige und Spitzen die Last aufnehmen.

-   * *Reliable messaging Cloud-zu-Gerät (oder *Befehle*). ** Lösung Back-End können IoT Hub senden von Nachrichten mit einem einmal am wenigsten Liefergarantie einzelner Geräte. Jede Nachricht verfügt über eine einzelne TTL Einstellung und Back-End kann Lieferung und Ablaufdatum Empfangsbestätigungen anfordern. Diese Belege sicher Einsicht in den Lebenszyklus einer Cloud-zu-Gerät-Nachricht. Sie können dann Geschäftslogik implementieren, die Vorgänge enthält, die auf Geräten ausgeführt.

-   **Hochladen Sie Dateien und zwischengespeicherte Daten in die cloud** Geräte können in Azure Storage mithilfe SAS-URIs IoT Hub verwaltet Dateien hochladen. IoT Hub erzeugen Benachrichtigung beim Eintreffen von Dateien in der Cloud Back-End verarbeiten können.

## <a name="gateways"></a>Gateways

Ein Gateway IoT-Lösung ist normalerweise ein [Protokollgateway] [ lnk-gateway] in die Cloud oder [Feld Gateway] bereitgestellt[ lnk-field-gateway] lokal mit Ihren Geräten bereitgestellt wird. Protokollgateway führt Protokoll Übersetzung, beispielsweise MQTT, AMQP. Feld Gateway kann Analytics am Rande ausführen, zeitkritische Entscheidungen Latenz, Device-Management-Services, Sicherheit und Datenschutz-Integritätsregeln erzwingen und Protokoll auch übersetzen. Beide Gateway fungieren als Vermittler zwischen Ihren Geräten und IoT Hub.

Feld Gateway unterscheidet sich von einfachen Verkehr routing ein (z. B. ein Gerät Network Address Translation oder Firewall) normalerweise aktiv ausgeführt Zugriff und den Informationsfluss in der Projektmappe verwalten.

Eine Lösung kann Protokoll und Feld-Gateways enthalten.

## <a name="how-does-iot-hub-work"></a>Funktionsweise von IoT Hub

Azure IoT Hub implementiert [Service unterstützt Kommunikation] [ lnk-service-assisted-pattern] Muster Interaktionen zwischen Ihrer Geräte und Ihre Lösung Backend vermitteln. Dienst unterstützt Kommunikation vertrauenswürdig herstellen soll, bidirektionale Kommunikationswege zwischen einem Verwaltungssystem IoT Hub und spezielle Geräte, die in bereitgestellt werden nicht vertrauenswürdig Platzbedarf. Muster gelten folgende Grundsätze:

- Sicherheit hat Vorrang vor allen anderen Funktionen.
- Geräte akzeptieren keine unerwünschten Netzwerkinformationen. Ein Gerät wird alle Verbindungen und Routen in einer ausgehenden. Ein auf einen Befehl vom Back-End muss das Gerät regelmäßig eine Verbindung prüfen alle ausstehenden Befehle zu initiieren.
- Geräte sollten nur Verbindung oder leitet an bekannte Dienste, die sie, wie IoT Hub dies einzurichten.
- Kommunikationspfad zwischen Gerät und Dienst oder Gerät und Gateway ist auf der Anwendungsschicht-Protokoll gesichert.
- Pro Gerät Identitäten basieren auf Systemebene Autorisierung und Authentifizierung. Sie stellen Berechtigungen und Anmeldeinformationen fast sofort widerrufen.
- Bidirektionale Kommunikation für Geräte, die Stromversorgung oder Verbindung Probleme sporadisch verbunden, wird erleichtert, Befehle und Gerät Benachrichtigungen halten, bis ein Gerät angeschlossen wird, deren Empfang. IoT Hub verwaltet Warteschlangen gerätespezifische Befehle sendet.
- Nutzlastdaten ist für die geschützte Übertragung über Gateways für einen bestimmten Dienst separat gesichert.

Der Mobilbranche verwendet hat das Service unterstützt Kommunikationsmuster enormen Push Notification Services wie [Windows Push Notification Services]implementiert[lnk-wns], [Google Cloud Messaging][lnk-google-messaging], und [Apple Push Notification Service][lnk-apple-push].

IoT Hub wird über ExpressRoutes öffentlichen peeringpfad unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Wie ermöglicht Azure IoT Hub standardbasierte IoT Device Management für Sie remote verwalten, konfigurieren und Aktualisieren Ihrer Geräte finden Sie unter [Übersicht über Azure IoT Hub Device-Management][lnk-device-management].

Zum Implementieren von Clientanwendungen auf einer Vielzahl von Hardware-Plattformen und Betriebssystemen können Sie das Gerät IoT SDKs. IoT Gerät SDKs sind Bibliotheken, die sendende Telemetrie IoT Hub und Cloud-zu-Gerät-Befehle ermöglichen. Bei Verwendung der SDKs können Sie verschiedene Netzwerkprotokolle Kommunikation mit IoT Hub auswählen. Weitere [Informationen zum Geräte-SDKs]finden Sie[lnk-device-sdks].

Zunächst Code schreiben und Ausführen von Beispielen finden Sie [Erste Schritte mit IoT Hub] [ lnk-get-started] Tutorial.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Dienst unterstützt Kommunikation Blogbeitrag von Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
