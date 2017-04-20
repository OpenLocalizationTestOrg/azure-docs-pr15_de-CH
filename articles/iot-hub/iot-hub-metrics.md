<properties
 pageTitle="IoT Hub Diagnose Metriken"
 description="Eine Übersicht über Azure IoT Hub Metriken können den Gesamtzustand ihrer Ressource bewerten"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Einführung in die Diagnose Metriken

Diagnostische Metriken geben bessere Daten über den Zustand der Azure-Ressourcen in Azure-Abonnement. Metriken können Sie den allgemeinen Zustand des Dienstes und der angeschlossenen Geräte zu bewerten. Benutzer Statistiken sind wichtig, da sie können Sie was mit Ihrem IoT Hub und Hilfe Ursache geht ohne Azure Support wenden.

Sie können Diagnose Metriken Azure-Portal.

## <a name="how-to-enable-diagnostic-metrics"></a>Aktivieren der Diagnose Metriken

1. IoT Hub zu erstellen. Finden Sie Informationen zum Erstellen von IoT Hub im [Einstieg] [ lnk-get-started] Guide.

2. Öffnen Sie das Blade IoT Hub. Klicken Sie dort auf **Diagnose**.

    ![][1]

3. Konfigurieren der Diagnose den Status **auf** und Azure Storage-Konto die Diagnosedaten gespeichert. Überprüfen von **Metriken**und drücken dann **Speichern**. Beachten Sie, dass Azure Storage-Konto muss vorab erstellt werden Aufpreis für Speicher. Sie können auch ein Ereignis Hubs Endpunkt die Diagnosedaten an.

    ![][2]

4. Wechseln Sie die Diagnose eingerichtet haben, zur **Übersicht** IoT Hub Blade. Metriken Informationen im Abschnitt **Überwachung** des Blades aufgefüllt. Auf das Diagramm wird geöffnet der Metriken, anzeigen Metriken Informationen für IoT Hub und bearbeiten die Auswahl von Metriken, die im Diagramm angezeigt. Sie können auch Alerts für metrische Werte.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Metriken und deren Verwendung

IoT Hub bietet verschiedene Messgrößen geben Sie eine Übersicht über den Zustand des Haupt- und die Gesamtzahl der angeschlossenen Geräte. Sie können mehrere Messgrößen für größeres Bild des Zustands der IoT Hub Informationen kombinieren. Die folgende Tabelle beschreibt die Metriken, die jede IoT Hub verfolgt, und wie jede Metrik Gesamtstatus IoT Hub.

| Metrik | Metrische Beschreibung | Verwendungszweck die Metrik |
| ---- | ---- | ---- |
| d2c.telemetry.Ingress.allProtocol | Die Anzahl der Nachrichten, die auf allen Geräten | Übersicht über Daten Nachricht sendet |
| d2c.telemetry.Ingress.Success | Die Anzahl aller erfolgreich Nachrichten an den hub | Übersicht über die eingehende Nachricht erfolgreich an den hub |
| c2d.Commands.Egress.Complete.Success | Die Anzahl aller Befehl Nachrichten auf allen Geräten durch das empfangende Gerät abgeschlossen | Mit Metriken verlassen und Ablehnen Überblick C2D Befehl Erfolgsquote |
| c2d.Commands.Egress.Abandon.Success | Die Anzahl aller Nachrichten, die erfolgreich auf allen Geräten vom empfangenden Gerät abgebrochen | Probleme hervorgehoben, wenn Nachrichten immer häufiger als erwartet abgebrochen |
| c2d.Commands.Egress.Reject.Success | Die Anzahl aller Nachrichten, die auf allen Geräten erfolgreich vom empfangenden Gerät abgelehnt | Probleme hervorgehoben, Nachrichten immer häufiger als erwartet abgelehnt |
| devices.totalDevices | Mittelwert, min und Max Anzahl der Geräte an den Hub IoT registriert | Die Anzahl der Geräte an den Hub registriert |
| devices.connectedDevices.allProtocol | Mittelwert, min und Max Anzahl der gleichzeitig verbundenen Geräte | Übersicht über die Anzahl der an den Hub angeschlossenen Geräte |

## <a name="next-steps"></a>Nächste Schritte

Nun, man Überblick Diagnose Metriken führen Sie hier weitere Informationen zum Verwalten von Azure IoT Hub:

- [Überwachung von Vorgängen][lnk-monitor]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Das Entwicklerhandbuch][lnk-devguide]
- [Ein SDK Gateway IoT simulieren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
