<properties
 pageTitle="IoT Hub Device Management Übersicht | Microsoft Azure"
 description="Dieser Artikel bietet eine Übersicht über Device-Management in Azure IoT Hub: Enterprise Gerät Lebenszyklus Neustart Funktion Firmwareupdate Konfiguration Gerät im Vergleich Abfragen, Aufträge"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Überblick über Azure IoT Hub-Gerät (Vorschau)

## <a name="introduction"></a>Einführung

Azure IoT Hub bietet Features und ein Erweiterungsmodell, die Geräte und Backend-Entwickler stabile IoT Device-Management-Lösungen zu ermöglichen. IoT Geräte zwischen eingeschränkten Sensoren und einem Zweck Mikrocontroller leistungsstarke Gateways, die Kommunikation für Gruppen Geräte weiterleiten.  Darüber hinaus unterschiedlich Anwendungsfälle und IoT Operatoren an Branchen.  Trotz dieser Variante bietet Azure IoT Hub Device-Management-Funktionen, Muster und Codebibliotheken auf vielfältige Geräte und Benutzer.

Ein wesentlicher Bestandteil ein erfolgreiches Unternehmens IoT Lösung eine Strategie für die Behandlung von Operatoren der laufenden Verwaltung der Sammlung von Geräten erstellen. IoT Operatoren erfordern einfache und zuverlässige Tools und Programme, die sich auf strategische Aspekte ihrer Arbeit konzentrieren können. Dieser Artikel enthält:

- Eine kurze Übersicht der Azure IoT Hub Device Management.
- Eine Beschreibung der Grundsätze der Device-Management.
- Eine Beschreibung des Lebenszyklus Gerät.
- Eine Übersicht über allgemeine Device Management Muster.

## <a name="iot-device-management-principles"></a>IoT Gerät Grundsätze

IoT bringt einen eindeutigen Satz von Gerät Probleme und jede Enterprise-Lösung müssen folgende Grundsätze:

![Azure IoT Hub Device Management Prinzipien Grafik][img-dm_principles]

- **Skalierung und Automatisierung**: IoT Lösungen erfordern einfache Tools, Automatisierung von Routineaufgaben und kleinen Betriebspersonal Millionen von Geräten zu ermöglichen. Tägliche, Operatoren, Arbeitsgänge Gerät Remote gebündelt und nur benachrichtigt werden, wenn Probleme auftreten, müssen ihre Aufmerksamkeit erwarten.

- **Offenheit und Kompatibilität**: das IoT Gerät Ökosystem ist außerordentlich unterschiedlich. Verwaltungstools müssen angepasst werden, um eine Vielzahl von Geräteklassen, Plattformen und Protokolle aufzunehmen. Operatoren müssen viele Gerätetypen, die eingeschränkten eingebetteten einzelnen Prozess Chips leistungsstarken und voll funktionsfähige Computern unterstützt werden.

- **Erkennung**: IoT sind dynamische, sich ständig ändernder. Zuverlässigkeit ist. Gerät Verwaltungsvorgänge Faktor müssen SLA Wartungsfenster, Netzwerk- und Stromversorgungsstatus Zustände in Gebrauch und Gerät Geolocation sichergestellt, dass Ausfallzeiten Wartung nicht kritischen Geschäftsabläufe beeinträchtigen oder gefährliche Situationen erstellen.

- **Viele Rollen Service**: Unterstützung für eindeutige Workflows und Prozesse IoT Betriebsfunktionen. Das Betriebspersonal müssen mit bestimmten Auflagen der internen IT-Abteilung harmonisch zusammenarbeiten.  Sie müssen auch nachhaltige Wege Fläche Gerät Operationen Echtzeitinformationen Vorgesetzte und andere Unternehmen Führungspositionen finden.

## <a name="iot-device-lifecycle"></a>IoT Gerät Lebenszyklus

Es gibt eine Reihe von allgemeinen Management Phasen, die in allen Enterprise-Projekten IoT. In Azure IoT sind fünf Phasen im Lebenszyklus IoT Gerät:

![Die fünf Azure IoT Gerät Lebenszyklus Phasen: Planen, bereitstellen, konfigurieren, überwachen, zurückziehen][img-device_lifecycle]

In jedem dieser fünf Phasen sind mehrere Operator Anforderungen, die erfüllt werden müssen, um eine umfassende Lösung:

- **Planen**: Betreiber ein Gerät Metadaten erstellen, können sie einfach und genau Abfragen und eine Gruppe von Geräten für Massenvorgänge Management. Zwei Geräte können um dieses Gerätemetadaten in Form von Tags und Eigenschaften zu speichern.

    *Weitere Informationen*: [Erste Schritte mit Gerät im Vergleich][lnk-twins-getstarted], [verstehen Gerät im Vergleich][lnk-twins-devguide], [Verwendung und Eigenschaften][lnk-twin-properties]

- **Bereitstellung**: sichere Bereitstellung neuer Geräte IoT Hub und Operatoren Gerätefunktionen sofort zu aktivieren.  Verwenden Sie Registrierung Geräts IoT Hub flexible Gerät Identitäten und Anmeldeinformationen, und führen Sie diesen Vorgang in Massen mithilfe eines Einzelvorgangs. Bauen Sie Geräte ihrer Funktionen und Eigenschaften in zwei Gerät Gerät melden.

    *Weitere Informationen*: [Gerät Identitäten verwalten][lnk-identity-registry], [Bulk Management Gerät Identitäten][lnk-bulk-identity], [Verwendung und Eigenschaften][lnk-twin-properties]

- **Konfigurieren**: konfigurationsänderungen und Firmware-Updates für Geräte gleichzeitig Gesundheit und Sicherheit vereinfachen. Operationen Sie diese Device Management gebündelt mit Eigenschaften oder mit direkten Methoden und broadcast.

    *Literaturangaben*: [direkte Methoden][lnk-c2d-methods], [Invoke direkte Methode auf einem Gerät][lnk-methods-devguide], [wie zwei Eigenschaften][lnk-twin-properties], [Zeitplan und broadcast Aufträge][lnk-jobs], [Planen von Projekten auf mehreren Geräten][lnk-jobs-devguide]

- **Monitor**: Gerät Auflistung den Gesamtzustand der Status laufender Vorgänge überwacht und warnt Operatoren auf Probleme, die ihre Aufmerksamkeit erfordern.  Wenden Sie zwei Geräte um Geräte Bericht Echtzeit Betriebs-und Status Aktualisierungsvorgänge an. Leistungsstarke Builddashboard gemeldet Fläche unmittelbar mit Gerät zwei Abfragen.

    *Weitere Informationen*: [wie zwei Eigenschaften][lnk-twin-properties], [Abfragesprache im Vergleich zu Aufträgen][lnk-query-language]

- **Zurückziehen**: Ersetzen oder Außerbetriebnahme von Geräten nach einem Ausfall, upgrade-Zyklus oder am Ende der Lebensdauer.  Verwenden Sie Gerät und Geräteinformationen zu, wenn das physische Gerät ersetzt oder archiviert zurückgezogen. Verwenden der Registrierung IoT Hub-Gerät für Gerät Identitäten und Anmeldeinformationen sicher sperren.

    *Weitere Informationen*: [zwei Eigenschaften wie][lnk-twin-properties], [Gerät Identitäten verwalten][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT Hub Device Management Muster

IoT Hub bietet die folgenden Device Management Muster.  [Device Management Lernprogramme] [ lnk-get-started] Sie ausführlich anzeigen, wie diese Muster entsprechend Ihrem Szenario erweitern und neue Muster basierend auf diese wichtige Vorlagen entwerfen.

- Dem Gerät über eine direkte Methode **Neustart** - Back-End-Anwendung informiert werden, dass einen Neustart initiiert hat.  Das Gerät verwendet das Gerät zwei Eigenschaften zum Aktualisieren des Status Neustart des Geräts gemeldet.

    ![Azure IoT Hub Gerätemanagement Neustart Muster Grafik][img-reboot_pattern]

- Informiert das Gerät **Zurücksetzen Factory** - Back-End-Anwendung über eine direkte Methode, dass es ein Werk Rücksetzen initiiert hat.  Das Gerät verwendet zwei Gerät gemeldete Eigenschaften Werk aktualisieren zurücksetzen Status des Geräts.

    ![Azure IoT Hub Device Management Factory zurücksetzen Muster Grafik][img-facreset_pattern]

- **Konfiguration** - Back-End-Anwendung verwendet zwei gewünschten Geräteeigenschaften Software auf dem Gerät konfigurieren.  Das Gerät verwendet das Gerät zwei Eigenschaften aktualisieren Konfigurationsstatus des Geräts gemeldet.

    ![Azure IoT Hub Device Management Konfiguration Muster Grafik][img-config_pattern]

- Dem Gerät über eine direkte Methode **Firmwareupdates** - Back-End-Anwendung informiert werden, dass eine Firmware-Aktualisierung initiiert hat.  Das Gerät initiiert ein Vorgang Paket Firmware, Firmwarepaket anwenden und schließlich Service IoT Hub verbinden.  Dabei Mult Schritt das Gerät verwendet das Gerät zwei Eigenschaften Aktualisieren des Fortschritts und Status des Geräts gemeldet.

    ![Azure IoT Hub Management Gerätefirmware Muster Grafik aktualisieren][img-fwupdate_pattern]

- **Fortschritt und reporting** - Anwendung Back-End Gerät zwei Abfragen über verschiedene Geräte zum Bericht über den Status und den Fortschritt von Aktionen auf den Geräten ausgeführt.

    ![Azure IoT Hub Device Management-reporting-Fortschritt und Muster-Grafik][img-report_progress_pattern]

## <a name="next-steps"></a>Nächste Schritte

Sie können Funktionen, Muster und Azure IoT Hub Device Management bietet IoT Applikationen zu erstellen, die die Enterprise IoT Operator in in jeder Lebenszyklusphase Gerät erfüllen, Codebibliotheken.

Weiter über Azure IoT Hub Device Management-Funktionen finden Sie unter [Erste Schritte mit Azure IoT Hub Device Management] [ lnk-get-started] Tutorial.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md