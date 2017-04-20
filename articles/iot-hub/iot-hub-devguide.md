<properties
 pageTitle="Entwicklerthemen IoT Hub | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch, die IoT Hub Endpunkte, Sicherheit, geräteidentitätsregistrierung, Device-Management und messaging"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Hub-Entwicklerhandbuch

Azure IoT Hub ist ein vollständig verwalteter Dienst, zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten IoT aktivieren und eine Anwendung zurück.

Azure IoT Hub bietet Ihnen:

* Sichere Kommunikation mit Anmeldeinformationen pro Gerät und Zugriffskontrolle.
* Zuverlässiges Gerät Cloud und Cloud Gerät Hyperscale-messaging.
* Einfache Device-Konnektivität mit Gerät für die am häufigsten verwendeten Sprachen und Plattformen.

Diese IoT Hub-Entwicklerhandbuch enthält die folgenden Artikeln:

- [Senden und Empfangen von Nachrichten mit IoT Hub] [ devguide-messaging] beschreibt Messagingfunktionen (Gerät Cloud und Cloud-Gerät), das IoT Hub verfügbar macht. Der Artikel enthält auch Informationen zu Themen wie Nachrichtenformate, und die unterstützten Kommunikationsprotokolle und die Portnummern, die sie verwenden.
- [Hochladen von Dateien von einem Gerät] [ devguide-upload] beschreibt, wie Sie Dateien von einem Gerät hochladen können. Der Artikel enthält auch Informationen über Themen wie die Benachrichtigung Uplaod Prozess senden kann.
- [Verwalten von Identitäten in IoT Hub Gerät] [ devguide-identities] beschreibt, welche Informationen jede IoT Hub Gerät identitätsregistrierung gespeichert und zum Zugriff auf und ändern.
- [Steuern des Zugriffs auf IoT Hub] [ devguide-security] beschreibt das Sicherheitsmodell auf IoT Hub Funktionalität für beide Geräte und cloud-Komponenten verwendet. Der Artikel enthält Informationen über Token und x. 509-Zertifikate und Details zu den Berechtigungen erteilt.
- [Gerät im Vergleich mit dem Zustand und Konfigurationen synchronisiert] [ devguide-device-twins] beschreibt das *Gerät zwei* Konzept und Funktionen wie Synchronisieren eines Geräts mit der macht. Der Artikel enthält Informationen über die Daten in einem Gerät zwei.
- [Eine direkte Methode auf einem Gerät] [ devguide-directmethods] beschreibt den Lebenszyklus direkte Methode Informationen zum Aufrufen von Methoden auf einem Gerät aus der Back-End-Anwendung und die direkte Methode auf dem Gerät verarbeiten.
- [Planen von Aufträgen auf mehreren Geräten] [ devguide-jobs] beschreibt, wie Sie auf mehreren Geräten planen können. Der Artikel beschreibt, wie Aufträge senden, die als Ausführen einer direkten Methode aktualisiert eine mit Devcie und Devcie Aufgaben. Es beschreibt auch den Status eines Auftrags Abfragen.
- [Referenz - Endpunkte IoT Hub] [ devguide-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht. Der Artikel beschreibt auch die Verwendung Feld Gateway einige Geräte Verbindung IoT Hub Endpunkte ermöglichen.
- [Referenz - Abfragesprache für im Vergleich, Methoden und Aufträge] [ devguide-query] beschreibt, Abfragesprache, die Informationen aus den Hub zu Ihrem Gerät im Vergleich Aufträge abrufen kann.
- [Referenz - Kontingente und Drosselung] [ devguide-quotas] fasst die Quoten IoT Hub und Drosselung Verhalten Sie erwarten bei einem Kontingent überschritten.
- [Referenz - Gerät und SDKs] [ devguide-sdks] Listen können SDKs Anwendungsentwicklung Geräte und Dienste, die mit IoT Hub interagieren. Der Artikel enthält Links zu Online-API-Dokumentation.
- [Referenz - Unterstützung IoT Hub MQTT] [ devguide-mqtt] bietet ausführliche Informationen wie IoT Hub die MQTT unterstützt. Der Artikel beschreibt die Unterstützung für das MQTT Protokoll in den SDKs und enthält Informationen über das MQTT Protokoll direkt.
- [Glossar] [ devguide-glossary] eine Liste der IoT Hub-bezogene Begriffe.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

