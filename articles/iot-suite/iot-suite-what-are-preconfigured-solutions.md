<properties
 pageTitle="Azure IoT vorkonfigurierte Lösung | Microsoft Azure"
 description="Eine Beschreibung der Azure IoT vorkonfiguriert Solutions und Architektur mit Links zu weiteren Ressourcen."
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

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Was sind vorkonfiguriert Azure IoT Suite Solutions?

Vorkonfigurierte Azure IoT Suite Solutions sind Implementierungen von häufig IoT Mustern, die Sie mit Ihrem Abonnement Azure bereitstellen können. Vorkonfigurierte Lösung können:

- Als Ausgangspunkt für eigene IoT-Lösungen.
- Gängige Muster in IoT Entwurf und Entwicklung erfahren.

Jede vorkonfigurierte Lösung ist eine vollständige, End-to-End-Implementierung, simulierte Geräten Telemetrie generiert.

Bereitstellen und die Lösungen in Azure ausgeführt, können Sie den vollständigen Quellcode herunterladen anpassen und erweitern die Lösung Ihrer IoT Anforderungen.

> [AZURE.NOTE] Um eine vorkonfigurierte Lösung bereitzustellen, besuchen Sie [Microsoft Azure IoT Suite][lnk-azureiotsuite]. [Einstieg mit den IoT vorkonfiguriert] Artikel[ lnk-getstarted-preconfigured] bietet weitere Informationen zum Bereitstellen und Ausführen einer Lösung.

Die folgende Tabelle zeigt die Zuordnung Lösungen IoT Besonderheiten:

| Lösung | Erfassung von Daten | Geräteidentität | Befehl und Steuerung | Regeln und Aktionen | Vorhersageanalytik |
|------------------------|-----|-----|-----|-----|-----|
| [Remote-Überwachung][lnk-getstarted-preconfigured] | Ja | Ja | Ja | Ja | -   |
| [Vorbeugende Wartung][lnk-predictive-maintenance] | Ja | Ja | Ja | Ja | Ja |

- *Erfassung der Daten*: Eindringen von Daten in der Cloud.
- *Geräteidentität*: eindeutige Identitäten jedes angeschlossene Gerät verwalten.
- *Befehl und Steuerung*: Senden von Nachrichten an ein Gerät aus der Cloud zu Gerät handeln.
- *Regeln und Aktionen*: Lösung Backend verwendet Regeln auf bestimmte Geräte Cloud-Daten.
- *Vorhersageanalytik*: Back-End Lösung gilt analysiert Gerät Cloud Daten Vorhersagen, wann bestimmte Aktionen durchgeführt werden soll. Z. B. analysieren Luftfahrzeuge Engine Telemetrie um festzustellen, wann die Wartung fällig ist.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Remoteüberwachung vorkonfiguriert Lösungsübersicht

Wir haben die remote monitoring vorkonfigurierte Lösung in diesem Artikel behandelt, da viele gemeinsamen Designelemente veranschaulicht, die die anderen Projektmappen genutzt.

Das folgende Diagramm veranschaulicht die wichtigsten Elemente der remote monitoring-Lösung. Die folgenden Abschnitte bieten weitere Informationen zu diesen Elementen.

![Remoteüberwachung vorkonfiguriert Lösungsarchitektur][img-remote-monitoring-arch]

## <a name="devices"></a>Geräte

Wenn Sie remote monitoring vorkonfigurierte Lösung bereitstellen, sind vier simulierte vorab bereitgestellte Lösung, die ein Kühlgerät simulieren. Diese simulierten Geräten haben eine integrierte Modell Temperatur und Feuchtigkeit, die Telemetrie ausgibt. Diese simulierten Geräten sind den End-to-End-Datenfluss durch die Lösung veranschaulichen und bieten eine bequeme Telemetrie und ein Ziel für Befehle eine Backend-Entwickler mit der Lösung als Ausgangspunkt für eine benutzerdefinierte Implementierung enthalten.

Wenn ein Gerät remote monitoring vorkonfigurierte Lösung zuerst IoT Hub angeschlossen, listet IoT Hub Nachricht Gerät Informationen die Liste der Befehle, denen das Gerät reagieren kann. In der remote monitoring vorkonfigurierte Lösung sind die Befehle: 

- *Ping-Gerät*: Gerät reagiert auf einen Befehl. Dies ist hilfreich für die Überprüfung, dass das Gerät noch aktiv und hören.
- *Telemetrie starten*: weist das Gerät senden Telemetrie.
- *Telemetrie beenden*: weist das Gerät senden Telemetrie.
- *Solltemperatur ändern*: steuert die simulierten telemetriewerte das Gerät sendet. Dies ist nützlich für Tests Backend-Logik.
- *Diagnostische Telemetrie*: steuert, ob das Gerät die Außentemperatur als Telemetrie senden soll.
- *Gerätestatus ändern*.: Legt die Metadateneigenschaft Gerät Zustand, der dem Gerät. Dies ist nützlich für Tests Backend-Logik.

Sie können mehr simulierte Geräten die Lösung, die dieselbe Telemetrie ausgeben und auf dieselben Befehle hinzufügen. 

## <a name="iot-hub"></a>IoT Hub

Vorkonfigurierte Lösung IoT Hub-Instanz entspricht *Cloud Gateway* in einer normalen [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

IoT Hub empfängt Geräte an einen einzelnen Endpunkt Telemetrie. IoT Hub verwaltet auch Gerät bestimmte Endpunkte, in denen jeweils die Befehle abrufen können, die an sie gesendet.

IoT Hub zur Verfügung empfangenen Telemetrie über dienstseitigen Telemetrie Endpunkt zu lesen.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Vorkonfigurierte Lösung verwendet drei [Azure Stream Analytics] [ lnk-asa] (ASA) Aufträge Telemetrie Stream von den Geräten filtern:


- *DeviceInfo Job* - gibt die Daten an einen Event-Hub, der Gerät Registrierung bestimmte Nachrichten, wenn zuerst ein Gerät angeschlossen oder auf einen Befehl **des Geräts ändern** Registrierung Geräts Lösung (einer DocumentDB) weiterleitet. 
- *Telemetrie Job* - sendet alle raw Telemetrie Azure Blob-Speicher für Speicher und Telemetrie Aggregationen, die im lösungsdashboard anzeigen berechnet.
- *Regeln Job* - Filter Telemetrie Stream für Werte, die jede Regelschwellenwerte und gibt die Daten an einen Hub Ereignis. Wenn eine Regel ausgelöst wird, wird dieses Ereignis als neue Zeile in die alarmtabelle Portal Dashboardansicht Lösung und löst eine Aktion auf der Basis für die Regeln und Aktionen im lösungsportal definiert.

In dieser vorkonfigurierte Lösung ASA Aufträge Bestandteil **IoT-Lösung back-End** in einer normalen [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Ereignisprozessor

In dieser vorkonfigurierte Lösung der Ereignisprozessor Bestandteil **IoT-Lösung back-End** in einer normalen [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

**DeviceInfo** und **Regeln** ASA Aufträge senden ihre Ausgabe an Event Hubs an anderen Back-End-Dienste. Die Lösung verwendet eine [EventPocessorHost] [ lnk-event-processor] beispielsweise [Webauftrag]aus[lnk-web-job], diese Ereignis Hubs Nachrichten gelesen. **EventProcessorHost** verwendet die **DeviceInfo** Daten zum Aktualisieren der Daten in der Datenbank DocumentDB und verwendet Daten **Regeln** Logik app und lösungsportal Alarme an Update aufrufen.

## <a name="device-identity-registry-and-documentdb"></a>Geräteidentitätsregistrierung und DocumentDB

Jede IoT Hub enthält eine [geräteidentitätsregistrierung] [ lnk-identity-registry] Gerät Schlüssel speichert. IoT Hub verwendet diese Informationen authentifizieren Geräte - ein Gerät muss registriert sein und einen gültigen Schlüssel bevor Hub anschließen können.

Diese Lösung speichert zusätzliche Informationen zu Geräten Zustand, die Befehle unterstützen und andere Metadaten. Die Lösung verwendet eine Datenbank DocumentDB für diese Lösung Geräte-Daten und lösungsportal Ruft Daten aus der Datenbank DocumentDB anzeigen und bearbeiten.

Die Lösung hält auch die Informationen in die Registrierung des Geräts Identität mit dem Inhalt der DocumentDB-Datenbank synchronisiert. **EventProcessorHost** verwendet Daten aus **DeviceInfo** Stream Analytics Auftrag die Synchronisierung verwalten.

## <a name="solution-portal"></a>Lösungsportal

![Lösungsdashboard][img-dashboard]

Lösungsportal ist eine webbasierte Benutzeroberfläche, die als Teil der vorkonfigurierte Lösung zur Cloud bereitgestellt wird. Sie können:

- Telemetrie und Alarm-Verlauf in einem Dashboard anzeigen.
- Bereitstellung neuer Geräte.
- Verwalten und Überwachen von Geräten.
- Befehle für bestimmte Geräte.
- Verwalten Sie Regeln und Aktionen.

In dieser vorkonfigurierte Lösung forms lösungsportal Teil **IoT-Lösung back-End** und **Verarbeitung und Business Connectivity** in [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über IoT Lösungsarchitekturen finden Sie [Microsoft Azure IoT Services: Referenzarchitektur][lnk-refarch].

Nun wissen Sie, was eine vorkonfigurierte Lösung ist, können Sie durch Bereitstellen der *Remoteüberwachung* vorkonfigurierte Lösung beginnen: [Erste Schritte mit vorkonfigurierten Lösungen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md