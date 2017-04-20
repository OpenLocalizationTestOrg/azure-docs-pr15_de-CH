<properties
 pageTitle="Entwicklerhandbuch - Glossar | Microsoft Azure"
 description="Ein Glossar der Begriffe für IoT Hub"
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

# <a name="glossary-of-iot-hub-terms"></a>IoT Hub Glossar

Dieser Artikel listet einige der gängigen Begriffe IoT Hub zugeordnet.

## <a name="advanced-message-queueing-protocol-amqp"></a>Erweiterte Message Queuing Protocol (AMQP)

[AMQP](https://www.amqp.org/) ist eine Messaging-Protokolle, die IOT Hub für die Kommunikation mit Geräten unterstützt. Weitere Informationen über IoT Hub unterstützt messaging-Protokolle finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Cloud-Geräte (C2D)

In der Regel verwendet auf Nachrichten von IoT Hub auf ein angeschlossenes Gerät. Häufig sind diese Nachrichten Befehle, die anweisen, das Gerät handeln. Weitere Informationen finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Bedingung

Verweist auf Gerätestatusinformationen wie Konnektivitätsmethode verwendet, eine geräteanwendung gemeldet. Geräte können auch ihre Funktionen melden. Zustand und Funktion mit zwei Geräte möglich.

## <a name="data-point-message"></a>Datenpunkt Nachricht

Datenpunkt Nachricht ist eine Cloud-zu-Gerät-Nachricht mit Telemetriedaten Geschwindigkeit oder Temperatur.

## <a name="desired-properties"></a>Eigenschaften

Im Kontext des Geräts im Vergleich Eigenschaften zusammen mit *Eigenschaften gemeldet* wird Gerätekonfiguration oder Bedingung synchronisieren. Eigenschaften sind können nur festgelegt werden von der Anwendung wieder Ende und von Gerät app. 

## <a name="device-to-cloud-d2c"></a>Gerät Cloud (D2C)

In der Regel verwendet auf Nachrichten von einem angeschlossenen Gerät IoT Hub. Diese Nachrichten möglicherweise [Daten](#data-point-message) oder [interaktive](#interactive-message) Nachrichten. Weitere Informationen finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Geräteidentitätsregistrierung

Die [geräteidentitätsregistrierung](iot-hub-devguide-identity-registry.md) ist die Komponente ein IoT Hub, der Informationen über die einzelnen Geräte dürfen an einen Hub anschließen.

## <a name="device"></a>Gerät

Im Kontext des IoT ist ein Gerät in der Regel eine kleine, eigenständige Computer, die Daten oder andere Geräte steuern. Angenommen ein Gerät möglicherweise, environmental monitoring Gerät oder ein Controller für die Bewässerung und Belüftung in Ihre.

## <a name="device-twin"></a>Gerät und

[Gerät und](iot-hub-devguide-device-twins.md) ist eine IoT Hub der Bedingung und Konfiguration eines physikalischen Geräts. Gerät zwei können Sie um die Konfiguration des physischen Geräts zu verwalten.

## <a name="direct-method"></a>Direkte Methode

Eine [direkte Methode](iot-hub-devguide-direct-methods.md) ist eine Möglichkeit, eine Methode auslösen durch Aufrufen einer API IoT Hub auf einem Gerät ausgeführt.

## <a name="event-hub-compatible-endpoint"></a>Ereignis-Hub-kompatiblen Endpunkt

Gerät Cloud Nachrichten IoT Hub lesen können Sie zu einem Endpunkt auf Ihrem Haupt-Verbindung und alle Ereignis Hub-kompatible Methode Nachrichten lesen. Hub-kompatiblen Ereignismethoden auch Ereignis Hubs SDKs und Azure Stream Analytics.

## <a name="field-gateway"></a>Feld gateway

Feld Gateway ermöglicht Konnektivität für Geräte können nicht direkt Verbindung IoT Hub und ist typischerweise lokal mit Ihren Geräten. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Auftrag

Ihre Lösung-Back-End können Projekte planen und Nachverfolgen von Aktivitäten auf Geräten mit IoT Hub registriert. Zählen und gewünschten Eigenschaften aktualisieren, aktualisieren Gerät zwei Tags und direkte Methoden aufrufen.

## <a name="protocol-gateway"></a>Protokollgateway

Protokollgateway typischerweise in der Cloud und Protokoll Übersetzungsdienstleistungen für Verbindung IoT Hub-Geräte bietet. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interaktive Meldung

Interaktive Meldung ist eine Cloud-zu-Gerät-Nachricht, die im Back-End Anwendung sofortige eine Aktion auslöst. Beispielsweise kann ein Alarm zu einem Fehler senden, die in einem CRM-System automatisch protokolliert werden sollen.

## <a name="iot-hub"></a>IoT Hub

IoT Hub ist ein vollständig verwalteter Azure Dienst, der zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und Back-End Lösung ermöglicht. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT Suite

Azure IoT Suite Pakete zusammen mehrere Azure Services mit vorkonfigurierten Lösungen. Diese vorkonfigurierten Lösungen können Sie schnell mit End-to-End-Implementierung von Szenarien IoT beginnen. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Suite?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Auftrag

Ein [Auftrag](iot-hub-devguide-jobs.md) in IoT Hub ermöglicht wie eine Firmware Upgrade über mehrere Geräte an den Hub angeschlossen.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) ist eine Messaging-Protokolle, die IOT Hub für die Kommunikation mit Geräten unterstützt. Weitere Informationen über IoT Hub unterstützt messaging-Protokolle finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Gemeldete Eigenschaften

Im Kontext des Geräts im Vergleich gemeldete Eigenschaften zusammen mit den *gewünschten Eigenschaften* dienen Gerätekonfiguration oder Bedingung synchronisieren. Gemeldete Eigenschaften kann nur festgelegt werden, von der app Gerät lesen und Back-End Anwendung abgefragt.

## <a name="tags"></a>Tags

Tags werden im Kontext des Devcie im Vergleich Gerät Metadaten gespeichert und abgerufen, indem die Anwendung Backend in Form eines JSON. Tags sind nicht auf einem Gerät angezeigt.

## <a name="system-properties"></a>Systemeigenschaften

Im Kontext des Geräts im Vergleich Systemeigenschaften sind schreibgeschützt und enthalten Informationen über die geräteverwendung wie letzte Aktivität und Verbindung.