<properties
    pageTitle="Microsoft Azure IoT Suite Übersicht | Microsoft Azure"
    description="Übersicht über Azure IoT Suite bietet Internet der Dinge vorkonfigurierte Lösung zum Sammeln, analysieren und Datenspeicher bieten Visualisierung und Integration mit anderen Systemen."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Was ist Azure IoT Suite?

Azure Internet der Dinge (IoT) Services bieten eine Vielzahl von Funktionen. Diese Klasse Unternehmensdienste können Sie:

- Sammeln von Daten von Geräten
- Analysieren von Daten-Streams in Bewegung
- Speichern und Abfragen von Daten
- Echtzeit- und Verlaufsdaten Daten
- Integration mit Back-Office-Systemen

Zu diesen Funktionen, Azure IoT Suite Pakete zusammen mehrere Azure Services mit der benutzerdefinierten Erweiterung als *vorkonfigurierte Lösung*. Diese vorkonfigurierten Lösungen sind grundlegende Implementierung von gemeinsamen IoT Mustern, mit deren Hilfe um Sie Ihre IoT Lösungen zu verkürzen. [IoT Software Development Kits]mit[lnk-sdks], anpassen und erweitern diese Lösungen für Ihre Bedürfnisse. Außerdem können Sie dazu als Beispiele oder Vorlagen beim Entwickeln von Lösungsmöglichkeiten IoT.

Das folgende Video bietet eine Einführung in Azure IoT Suite:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT Dienste in Azure IoT Suite

Vorkonfigurierte Lösung verwenden in der Regel die folgenden Dienste:

- Azure IoT Suite ist der [Azure IoT Hub] [ lnk-iot-hub] Service. Dieser Service bietet das Gerät Cloud und Cloud-zu-Gerät-messaging und fungiert als Gateway für die Cloud und die wichtigsten IoT Suite Dienste. Der Dienst können Sie Nachrichten von Ihren Geräten Ebene und Befehle an Geräte senden.

- [Azure Stream Analytics] [ lnk-asa] in Bewegung analysiert. IoT Suite nutzt dieser Dienst verarbeitet eingehenden Telemetrie, Aggregation, und erkennen. Vorkonfigurierte Lösung verwenden auch Stream Analytics informative Nachrichten verarbeitet, die Daten wie Metadaten oder Befehl Antworten von Geräten enthalten. Verarbeitung der Nachrichten von Ihren Geräten und Nachrichten für andere Dienste verwenden Projektmappen Stream Analytics.

- [Azure-Speicher] [ lnk-azure-storage] und [Azure DocumentDB] [ lnk-document-db] die Storage-Funktionen bereitstellen. Vorkonfigurierte Lösung verwenden BLOB-Speicher Telemetrie speichern und für die Analyse verfügbar zu machen. Verwenden Projektmappen DocumentDB Gerätemetadaten speichern und Device Management-Funktionen der Lösung zu aktivieren.

- [Azure Web Apps] [ lnk-web-apps] und [Microsoft Power BI] [ lnk-power-bi] Visualisierungsfunktionen Daten bereitstellen. Die Flexibilität von Power BI können Sie interaktive Dashboards erstellen, die Daten IoT Suite verwenden.

Eine Übersicht über die Architektur einer normalen IoT-Lösung finden Sie unter [Microsoft Azure und das Internet der Dinge (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Vorkonfigurierte Lösung

IoT Suite enthält vorkonfigurierte Lösungen, mit denen Sie schnellen Einstieg und IoT-Szenarien, wie *Remote-Überwachung* und *vorbeugende Wartung*, die Azure IoT Suite ermöglicht. Die Bereitstellung dieser Projektmappen zu Ihrem Azure-Abonnement, und führen Sie eine vollständige End-to-End IoT Szenario.

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie eine Übersicht was IoT Suite und die wichtigsten Komponenten können Sie vorkonfigurierten Lösungen IoT Suite erfahren, siehe [Solutions was Azure IoT sind vorkonfiguriert?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
