<properties
 pageTitle="Vorbeugende Wartung Walkthrough | Microsoft Azure"
 description="Eine exemplarische Vorgehensweise Azure IoT vorbeugende Wartung vorkonfigurierte Lösung."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Vorbeugende Wartung vorkonfigurierte Lösung Exemplarische Vorgehensweise

## <a name="introduction"></a>Einführung

IoT Suite vorbeugende Wartung vorkonfigurierte Lösung ist eine End-to-End-Lösung für Business-Szenario, das den Punkt geschätzt wird Fehler auftreten. Diese vorkonfigurierte Lösung können proaktiv für Aktivitäten wie Wartung optimieren. Die Lösung kombiniert Schlüsseldienste zur Azure IoT Suite, einschließlich einer [Azure Machine Learning] [ lnk_machine_learning] Arbeitsbereich. Dieser Arbeitsbereich enthält Experimente basierend auf einem öffentlichen Beispieldataset der verbleibenden nützliche Leben (RUL) eines Luftfahrzeugs Motors vorhersagen. Die Lösung implementiert das Geschäftsszenario IoT vollständig als Ausgangspunkt für die Planung und Implementierung einer Lösung, die Ihre eigenen Unternehmen erfüllt.

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierte Lösung:

![][img-architecture]

Die blaue Elemente sind Azure Services am Speicherort wählen, wenn Sie die vorkonfigurierte Lösung bereitstellen bereitgestellt werden. Sie können die vorkonfigurierte Lösung im südostasiatischen US, Nordeuropa oder Ostasien bereitstellen.

Einige Ressourcen sind nicht verfügbar in den Regionen, in denen die vorkonfigurierte Lösung bereitstellen. Orangefarbene Elemente im Diagramm darstellen Azure Services im nächsten verfügbaren Bereich (südlichen zentralen USA, Westeuropa und Südostasien) erhalten den ausgewählten Bereich bereitgestellt.

Grüne Element ist ein Ausgabegerät, das Luftfahrzeug-Modul darstellt. Sie können diese simulierten Geräten im Abschnitt Weitere Informationen.

Die graue Elemente repräsentieren Komponenten *Gerät* Verwaltungsfunktionen implementiert. Die aktuelle Version der vorbeugende vorkonfigurierte wartungslösung stellt diese Ressourcen nicht bereit. Erfahren Sie mehr über die geräteverwaltung von finden Sie [vorkonfigurierte Lösung Remoteüberwachung][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simulierte Geräte

Vorkonfigurierte Lösung stellt ein simuliertes Gerät ein Flugzeug-Modul. Die Lösung wird mit zwei bereitgestellt, die ein Luftfahrzeug zuordnen. Jedes Modul gibt vier Arten von Telemetriedaten: Sensor 9, Sensor 11, Sensor 14 und 15 Sensor für maschinelles lernen Modell berechnet die verbleibenden nützliche Leben (RUL) für das Modul erforderlichen Daten bereitzustellen. Jedes simulierten Gerät die folgenden Telemetrie Nachrichten an IoT Hub gesendet:

*Inventur*. Ein Zyklus stellt einen abgeschlossenen Flug mit variabler Länge zwischen 2 bis 10 Stunden in denen Telemetriedaten jede halbe Stunde während des Fluges erfasst.

*Telemetrie*. Gibt es vier Sensoren, die Modul-Attribute darstellen. Die Sensoren sind generisch Sensor 9, Sensor 11, Sensor 14 und 15 Sensor gekennzeichnet. 4 Sensoren sind ausreichend, um nützliche Ergebnisse aus dem Modell maschinelles lernen für RUL Telemetrie. Dieses Modell wird von einem öffentlichen Datensatz erstellt motordaten. Weitere Hinweise, wie das Modell aus dem ursprünglichen Dataset erstellt wurde, finden Sie unter [Cortana Intelligence Gallery vorbeugende Wartung Vorlage][lnk-cortana-analytics].

Die simulierten Geräten können die folgenden Befehle von IoT Hub gesendet behandeln:

| Befehl | Beschreibung |
|---------|-------------|
| StartTelemetry | Steuert den Zustand der Simulation.<br/>Startet das Gerät Telemetrie senden     |
| StopTelemetry  | Steuert den Zustand der Simulation.<br/>Das Gerät sendet Telemetrie beendet |

IoT Drehkreuz Gerät Befehl Bestätigung.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-Auftrag

**Arbeit: Telemetrie** arbeitet mit den eingehenden Gerät Telemetrie Stream mit zwei Anweisungen. Die erste Geräte alle Telemetriedaten auswählt und sendet diese Daten auf BLOB-Speicher, in der Web app dargestellt. Die zweite Anweisung berechnet durchschnittliche Sensorwerte über ein Zeitfenster von zwei Minuten und sendet diese Daten über den Ereignis-Hub ein **Ereignisprozessor**.

## <a name="event-processor"></a>Ereignisprozessor

Der **Ereignisprozessor** wird durchschnittliche Sensorwerte für einen abgeschlossenen. Diese Bahnen diese Werte eine API, die macht der geschult Modell RUL für ein Modul berechnet.

## <a name="azure-machine-learning"></a>Azure maschinelles lernen

Weitere Hinweise, wie das Modell aus dem ursprünglichen Dataset erstellt wurde, finden Sie unter [Cortana Intelligence Gallery vorbeugende Wartung Vorlage][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Wir gehen Sie

Dieser Abschnitt führt Sie durch die Komponenten der Lösung, den gewünschten Anwendungsfall beschreibt und Beispiele.

### <a name="predictive-maintenance-dashboard"></a>Vorbeugende Wartung Dashboard

Diese Seite in der Anwendung verwendet PowerBI JavaScript-Steuerelemente (siehe [Grafik PowerBI Repository][lnk-powerbi]) visuell darstellen:

- Die Ausgabedaten von Stream Analytics-Aufträge im BLOB-Speicher.
- Die RUL und zählen pro Luftfahrzeuge.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Beobachten das Verhalten der Cloud-Lösung

Navigieren Sie in Azure-Portal der Ressourcengruppe mit dem Lösungsnamen gewählte bereitgestellten Ressourcen anzeigen.

![][img-resource-group]

Wenn Sie vorkonfigurierte Lösung bereitstellen, erhalten Sie eine e-Mail mit einem Link zum Arbeitsbereich Computerlernen. Sie können auch Computerlernen Arbeitsbereich navigieren, von [azureiotsuite.com] [ lnk-azureiotsuite] Seite für die bereitgestellte Lösung wird im Zustand **bereit** .

![][img-machine-learning]

Im lösungsportal sehen Sie das Beispiel mit vier simulierte, zwei mit zwei Module pro Flugzeug mit Sensoren bereitgestellt wird. Beim ersten zu dem lösungsportal navigieren wird die Simulation angehalten.

![][img-simulation-stopped]

Klicken Sie auf **Start-Simulation** , um die Simulation zu starten, in dem Sie den Sensor Verlauf RUL, Zyklen, und RUL Verlauf füllen das Dashboard.

![][img-simulation-running]

RUL kleiner (einer beliebigen Schwellenwert zu Demonstrationszwecken ausgewählt) 160 lösungsportal zeigt ein Warnsymbol neben dem RUL Display und Luftfahrzeugen Engine in gelb hervorgehoben. Beachten, wie die RUL allgemeinen insgesamt Abwärtstrend dürfen jedoch meist auf und ab. Dieses Verhalten ergibt sich aus der Zyklus unterschiedlich und die Genauigkeit des Modells.

![][img-simulation-warning]

Die vollständige Simulation dauert ca. 35 Minuten 148 Zyklen. 160 RUL Schwellenwert erreicht, erstmals auf 5 Minuten und beide Module erreicht den Schwellenwert rund 8 Minuten.

Simulation durch vollständigen Dataset 148 Zyklen und für endgültige RUL und Zyklus gleicht.

Die Simulation jederzeit beenden, aber auf **Simulation starten** die Simulation vom Anfang des Datasets gibt.

## <a name="next-steps"></a>Nächste Schritte

Jetzt Sie vorbeugende vorkonfigurierte wartungslösung haben Sie möchten ändern, finden [Hinweise zum Anpassen von vorkonfigurierten Lösungen][lnk-customize].

Blogbeitrag [IoT Suite - Details - vorbeugende Wartung](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet bietet weitere Informationen zur vorbeugenden Wartung vorkonfigurierte Lösung.

Sie können auch die Features und Funktionen IoT Suite vorkonfigurierte Lösung vertraut machen:

- [Häufig gestellte Fragen zum IoT Suite][lnk-faq]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
