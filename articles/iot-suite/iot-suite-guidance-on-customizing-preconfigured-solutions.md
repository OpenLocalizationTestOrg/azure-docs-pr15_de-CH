<properties
    pageTitle="Anpassen von Lösungen vorkonfiguriert | Microsoft Azure"
    description="Anleitung zum Azure IoT Suite vorkonfigurierte Lösung anpassen."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Vorkonfigurierte Lösung anpassen

Die vorkonfigurierten Lösungen mit den Azure IoT zeigen Services Suite gemeinsam eine End-to-End-Lösung. Von diesem Ausgangspunkt sind vielen Stellen in denen können Sie erweitern und Anpassen die Lösung für bestimmte Szenarien. Den folgenden Abschnitten werden diese gemeinsamen Anpassungspunkte.

## <a name="finding-the-source-code"></a>Quellcode suchen

Quellcode für vorkonfigurierte Lösung ist auf GitHub in den folgenden Quellen:

- Remoteüberwachung: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Vorbeugende Wartung: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Der Quellcode für vorkonfigurierte Lösung wird die Muster und Praktiken der End-to-End-Funktionalität einer Azure IoT Suite mit IoT-Lösung bereitgestellt. Finden Sie weitere Informationen zum Erstellen und Bereitstellen von Projektmappen in GitHub Repositories.

## <a name="changing-the-preconfigured-rules"></a>Die vorkonfigurierten Regeln ändern

Remote monitoring-Lösung umfasst drei [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -Jobs zum Gerät, Telemetrie und die Regellogik angezeigt, für die Lösung implementieren.

Die drei Stream Analytics-Aufträge und ihre Syntax wird ausführlich [Remoteüberwachung vorkonfigurierte Lösung exemplarischen Vorgehensweise](iot-suite-remote-monitoring-sample-walkthrough.md)beschrieben. 

Sie können diese Aufträge direkt ändern die Logik bearbeiten oder Ihr Szenario spezifische Logik hinzufügen. Finden Sie den Stream Analytics-Aufträge wie folgt:
 
1. [Azure](https://portal.azure.com)-Portal wechseln
2. Navigieren Sie zu der Ressourcengruppe mit demselben Namen wie die Projektmappe IoT. 
3. Wählen Sie den Einzelvorgang Azure Stream Analytics ändern möchten. 
4. Beenden Sie **Beenden**in der Befehle auswählen. 
5. Bearbeiten Sie die Eingaben, Abfrage und Ausgaben.

    Eine einfache Änderung soll die Abfrage dafür **Regeln** mit einer **"<"** statt einer **">"**. Das lösungsportal zeigt **">"** Wenn Sie bearbeiten, aber das Verhalten aufgrund der zugrunde liegende Auftrag gekippt sehen.

6. Der Auftrag wird gestartet

> [AZURE.NOTE] Remote monitoring Dashboard hängt von bestimmten Daten so ändern die Einzelvorgänge Dashboard fehlschlagen kann.

## <a name="adding-your-own-rules"></a>Eigene Regeln

Ändern die vorkonfigurierten Azure Stream Analytics-Aufträge, sondern können Azure-Portal Arbeitsplätze oder hinzufügen neue Abfragen Arbeitsplätze.

## <a name="customizing-devices"></a>Anpassen von Geräten

Eine der am häufigsten verwendete Erweiterung arbeitet mit Geräten für Ihr Szenario. Es gibt mehrere Methoden zum Arbeiten mit Geräten. Diese Methoden umfassen simuliertes Gerät entsprechend Szenario ändern oder mit [IoT Device SDK][] die Lösung das physische Gerät herstellen.

Eine schrittweise Anleitung zum Hinzufügen von Geräten zu remote monitoring vorkonfigurierte Lösung finden Sie unter [Verbindung Iot Suite-Geräte](iot-suite-connecting-devices.md) und [Remote monitoring C SDK-Beispiel](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) , das remote monitoring vorkonfigurierte Lösung arbeiten soll.

### <a name="creating-your-own-simulated-device"></a>Erstellen eigener simulierten Gerät

Remote monitoring Lösung Quellcode (oben verwiesen) enthalten, ist ein .NET. Dieser Simulator wird als Teil der Lösung bereitgestellt und kann geändert werden, um verschiedene Metadaten Telemetrie senden oder auf verschiedene Befehle reagieren.

Vorkonfigurierte Simulator remote monitoring vorkonfigurierte Lösung ist ein Kühler, die Temperatur und Feuchtigkeit Telemetrie gibt den Simulator im Projekt [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) können beim GitHub Repository gespalten haben.

### <a name="available-locations-for-simulated-devices"></a>Verfügbare Speicherorte für simulierten Geräten

Der Standardsatz von Standorten ist in Seattle, Redmond, Washington, USA. Können Sie diese Verzeichnisse in [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Erstellen und Verwenden eigener (Gerät)

[Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) bieten Bibliotheken für zahlreiche Gerätetypen (Sprachen und Betriebssysteme) Verbindung IoT-Lösungen.

## <a name="modifying-dashboard-limits"></a>Ändern von Dashboard-Grenzwerte

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Anzahl der Geräte im Dashboard Dropdown angezeigt

Der Standardwert ist 200. Ändern Sie diese Zahl in [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Anzahl der Stifte in Bing Maps-Steuerelement anzeigen

Der Standardwert ist 200. Ändern Sie diese Zahl in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Zeitraum der Telemetrie-Diagramm

Der Standardwert ist 10 Minuten. Sie können dies im [TelmetryApiController.cs]ändern[lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Anwendungsrollen einrichten

Im folgenden wird beschrieben, wie eine vorkonfigurierte Lösung **Admin** und **ReadOnly** Anwendungsrollen hinzugefügt. Beachten Sie, dass vorkonfigurierte Lösungen bereitgestellt von azureiotsuite.com Website bereits die Rollen **Administrator** und **ReadOnly** .

Mitglieder der Rolle **ReadOnly** Dashboard und der Liste sehen jedoch dürfen Geräte hinzufügen oder ändern Geräteattribute Befehle.  Mitglieder der **Administrator** -Rolle haben vollen Zugriff auf alle Funktionen in der Projektmappe.

1. [Azure-Verwaltungsportal]zu[lnk-classic-portal].

2. Wählen Sie **Active Directory**.

3. Klicken Sie auf AAD-Mandanten, die Sie bei die Lösung bereitgestellt.

4. Klicken Sie auf **Anwendung**.

5. Klicken Sie auf den Namen der Anwendung, die die vorkonfigurierte Lösung übereinstimmt. Wenn die Anwendung in der Liste nicht angezeigt wird, Dropdown- **Applikationen Mein Unternehmen** **Anzeigen** und klicken Sie auf das Häkchen.

6.  Klicken Sie am unteren Rand der Seite auf **Manifest verwalten** und anschließend auf **Download Manifest**.

7. Dies eine JSON-Datei auf Ihrem Computer gedownloadet.  Öffnen Sie diese Datei für die Bearbeitung in einem Text-Editor Ihrer Wahl.

8. In der dritten Zeile der JSON-Datei finden Sie:

  ```
  "appRoles" : [],
  ```
  Ersetzen Sie dies durch Folgendes:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Speichern Sie die aktualisierte JSON-Datei (Sie können die vorhandene Datei überschreiben).

10.  Wählen Sie in der Azure-Verwaltungsportal am unteren Rand der Seite **Manifest verwalten** und **Hochladen Manifest** JSON-Datei hochladen, die Sie im vorherigen Schritt gespeichert.

11. Sie haben nun die Rollen **Administrator** und **ReadOnly** Ihrer Anwendung hinzugefügt.

12. Um eine dieser Rollen ein Benutzer im Verzeichnis zuzuweisen, finden Sie unter [Berechtigungen auf der Website azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Feedback

Eine Anpassung verfügen möchten Sie finden in diesem Dokument behandelt? Fügen Sie neue Features vorschlagen [Benutzer Sprach-](https://feedback.azure.com/forums/321918-azure-iot)oder Kommentar zu diesem Artikel. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Optionen zum Anpassen der vorkonfigurierte Lösung finden Sie unter:

- [Ihre Azure IoT Suite Remoteüberwachung vorkonfigurierte Lösung Logik App herstellen][lnk-logicapp]
- [Verwenden Sie dynamische Telemetrie remote monitoring vorkonfigurierte Lösung][lnk-dynamic]
- [Gerät Informationen Metadaten in die Überwachung vorkonfigurierte Lösung][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
