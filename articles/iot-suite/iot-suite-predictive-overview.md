<properties
 pageTitle="Vorbeugende Wartung vorkonfigurierte Lösung | Microsoft Azure"
 description="Eine Beschreibung der Azure IoT vorbeugende Wartung vorkonfigurierte Lösung."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
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

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Vorbeugende Wartung vorkonfiguriert Lösungsübersicht

*Vorbeugende Wartung* vorkonfigurierte Lösung ist eine [vorkonfigurierte Lösung] [ lnk_preconfigured_solutions] als Teil des [Microsoft Azure IoT Suite][lnk_iot_suite]. Diese Lösung integriert Echtzeit Gerät Telemetrie Auflistung eines Vorhersagemodells mit [Azure Machine Learning]erstellt[lnk_machine_learning].


Azure IoT Suite können Unternehmen schnell und einfach Verbinden mit Ressourcen überwachen und Analysieren von Daten in Echtzeit. Die vorbeugende vorkonfigurierte wartungslösung, Daten und umfangreiche Dashboards und Visualisierung zu Unternehmen neue Intelligenz, die Effizienz und verbessern Umsatzquellen verwendet.

## <a name="the-scenario"></a>Das Szenario

Fabrikam ist eine regionale Fluggesellschaft auf große Kundenzufriedenheit zu wettbewerbsfähigen Preisen. Ein flugverzögerungen verursacht Probleme und Wartung Engine ist besonders schwierig. Modul-Fehler während des Fluges muss Fabrikam Motoren regelmäßig überprüft und ein durchdachtes Wartungsprogramm entspricht, vermieden werden. Allerdings tragen nicht Flugzeugmotoren gleich. Einige unnötige auf gewartet. Darüber hinaus treten Probleme die Flugzeuges Boden können bis Wartung durchgeführt wird. Dadurch wird teure Verzögerungen besonders ist ein Luftfahrzeug an einer Stelle, wo die richtigen Techniker oder Ersatzteile nicht verfügbar sind.

Module Flugzeugen von Fabrikam mit instrumentiert die Engine Vorschriften während des Fluges zu überwachen. Fabrikam verwenden Azure IoT Suite Sensordaten während des Fluges zu sammeln. Sammeln Jahre Modul betriebsbereit und Fehlerdaten haben Fabrikam datenwissenschaftler können die verbleibenden nützliche Leben (RUL) eines Luftfahrzeugs Motors vorhersagen nachgebildet. Was sie erkannt haben, ist eine Korrelation zwischen den vier Engine Sensoren mit der Engine, die eventuelle Fehler führt. Fabrikam weiterhin regelmäßige Inspektionen Sicherheit ausführen, jetzt können die Modelle RUL für jedes Modul berechnet, nachdem jeder Flug Telemetriedaten aus den Motoren während des Fluges gesammelt. Fabrikam kann Vorhersagen zukünftigen Punkte und Plan für die Wartung und Reparatur im voraus.

> [AZURE.NOTE] Die Lösung verwendet Verschleiß tatsächlichen Daten.

Vorhersagen des Punkts bei Wartungsarbeiten, optimieren Fabrikam Vorgänge zur Kostensenkung. Wartung Koordinatoren arbeiten mit Planer Planen Wartung zeitgleich mit einem an einer bestimmten Position beenden und genügend Zeit für die Flugzeuge aus ohne Unterbrechung der Zeitplan sicherzustellen. Fabrikam kann Techniker entsprechend planen. Sicherstellung Flugzeuge sind ohne Wartezeit effizient verarbeitet. Lagerleiter Steuerelement erhalten Wartungspläne, Optimieren Sie ihre Bestellung und Ersatzteile Teilebestands. Dies ermöglicht Fabrikam Luftfahrzeuge Boden minimieren und Betriebskosten senken und gleichzeitig die Sicherheit der Fahrgäste und Besatzung.

Zu verstehen wie [Azure IoT Suite] [ lnk_iot_suite] bietet Kunden Funktionen Potenzial vorbeugende Wartung, überprüfen Sie diese [INFOGRAFIK][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Aufbau die wartungslösung für die vorbeugende

Die Lösung nutzt ein vorhandenes Azure Machine Learning-Modell als eine Vorlage für diese Funktionen von Gerät Telemetrie gesammelten IoT Suite Services anzeigen. Microsoft hat ein [Regressionsmodell] erstellt[ lnk_regression_model] eines Luftfahrzeugs Motors und vollständige Vorlage Daten<sup>\[1\]</sup>, und schrittweise Anleitung zur Verwendung des Modells.

Azure IoT vorbeugende Wartung vorkonfigurierte Lösung verwendet Regressionsmodell anhand dieser Vorlage erstellt. Es ist in der Azure-Abonnement und über eine automatisch generierte API verfügbar gemacht. Die Lösung umfasst eine Teilmenge der Testdaten 4 (insgesamt 100) darstellt Motoren und 4 (insgesamt 21) Sensor Datenströme ein richtiges Ergebnis von ausgebildeten Modell bieten.

*\[1\] Saxena a und K. Goebel (2008). "Turbofan Engine Beeinträchtigung Simulation Datengruppe" NASA Ames Vorzeichen Daten-Repository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) NASA Ames Research Center, Moffett Feld, CA*

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie, wie Azure IoT vorbeugende Wartung Szenarien ermöglicht, lesen [erfassen Wert über das Internet der Dinge][lnk_capture_value].

Eine [Exemplarische Vorgehensweise] nehmen[ lnk-predictive-walkthrough] vorbeugende Wartung vorkonfigurierte Lösung.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Sie können auch die Features und Funktionen IoT Suite vorkonfigurierte Lösung vertraut machen:

- [Häufig gestellte Fragen zum IoT Suite][lnk-faq]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
