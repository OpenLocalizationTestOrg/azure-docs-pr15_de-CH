<properties
    pageTitle="Verwenden Sie dynamische Telemetrie | Microsoft Azure"
    description="Dieses Lernprogramm Informationen zum dynamischen Telemetrie remote monitoring vorkonfigurierte Lösung verwenden."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Verwenden Sie dynamische Telemetrie remote monitoring vorkonfigurierte Lösung

## <a name="introduction"></a>Einführung

Dynamische Telemetrie kann alle Telemetrie gesendet, remote monitoring vorkonfigurierte Lösung darstellen. Die simulierten Geräten mit vorkonfigurierte Lösung senden Temperatur und Feuchtigkeit Telemetrie im Schaltpult visualisieren können. Wenn vorhandenen simulierten Geräten anpassen, neue simulierte Geräten erstellen oder physische vorkonfigurierte Lösung Geräte können Sie andere telemetriewerte wie Temperatur, u/min oder Windstärke senden. Sie können dann diese zusätzliche Telemetrie im Schaltpult visualisieren.

Diese praktische Einführung verwendet Node.js simuliertes Gerät, das Sie ändern können, um dynamische Telemetrie experimentieren.

Um dieses Lernprogramm benötigen Sie:

- Ein aktives Azure-Abonnement. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion][lnk_free_trial].
- [Node.js] [ lnk-node] Version 0.12.x oder höher.

Sie können dieses Lernprogramm unter jedem Betriebssystem wie Windows oder Linux, Node.js Installation abschließen.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Node.js simulierte Gerät konfigurieren

1. Remote monitoring Dashboard auf **+ Hinzufügen** , und fügen Sie ein benutzerdefiniertes Gerät hinzu. Notieren Sie den IoT Hub Hostname, Geräte-Id und Gerät. Sie brauchen sie später in diesem Lernprogramm, wenn die Clientanwendung remote_monitoring.js Gerät vorbereiten.

2. Stellen Sie sicher, dass Node.js-Version 0.12.x oder höher auf dem Entwicklungscomputer installiert ist. Ausführen `node --version` in der Befehlszeile oder in einer Shell die Version überprüfen. Informationen über Paket-Manager-Installation Node.js unter Linux finden Sie unter [Installation Node.js über Paket-Manager-][node-linux].

3. Bei der Installation von Node.js Klonen Sie die neueste Version von [Azure Iot Sdks] [ lnk-github-repo] Repository auf dem Entwicklungscomputer. Verwenden Sie immer **master** Verzweigung für die neueste Version der Bibliotheken und Beispiele.

4. Aus der lokalen Kopie der [Azure-Iot-Sdks] [ lnk-github-repo] Repository kopieren die folgenden Dateien aus dem Ordner Knoten/Gerätebeispiele in einem leeren Ordner auf dem Entwicklungscomputer:

  - Packages.JSON
  - remote_monitoring.js

5. Öffnen Sie die Datei remote_monitoring.js, und suchen Sie die Definition der folgenden:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Die Verbindungszeichenfolge Gerät **[IoT Hub Gerät Verbindungszeichenfolge]** ersetzen. Verwenden Sie die Werte für IoT Hub Hostname, Geräte-Id und notieren Sie sich in Schritt 1 vorgenommene Geräteschlüssel. Eine Verbindungszeichenfolge Gerät hat das folgende Format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Wenn Ihr Hostname IoT Hub **Contoso** und Ihre Geräte-Id **Mydevice**, sieht die Verbindungszeichenfolge wie folgt:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Speichern Sie die Datei. Führen Sie folgenden Befehl in einer Shell oder die Befehlszeile in den Ordner mit diesen Dateien erforderlichen Pakete installieren und führen Sie die beispielanwendung:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Beobachten Sie dynamische Telemetrie in Aktion

Das Dashboard zeigt Temperatur und Feuchtigkeit Telemetriedaten aus vorhandenen simulierten Geräten:

![Standard-dashboard][image1]

Wenn Node.js simulierte Gerät wählen Sie im vorigen Abschnitt wurden Temperatur, Luftfeuchtigkeit und Temperatur Telemetrie angezeigt:

![Temperatur der Dashboard hinzufügen][image2]

Remote monitoring-Lösung zusätzliche externe Temperaturen Telemetrie Typ erkennt und das Diagramm auf dem Dashboard hinzugefügt.

## <a name="add-a-telemetry-type"></a>Telemetrie Typ hinzufügen

Der nächste Schritt ist Telemetrie Node.js simulierten Gerät mit neuen Werten zu ersetzen:

1. Node.js simulierte Gerät durch Eingabe von **STRG + C** in der Befehlszeile oder der Shell beendet.

2. In der Datei remote_monitoring.js sehen Sie die Stammdatenwerte für vorhandene Temperatur, Luftfeuchtigkeit und Temperatur Telemetrie. Fügen Sie der Stammdaten **Rpm** wie folgt:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js simulierte Gerät verwendet die Funktion **GenerateRandomIncrement** in der Datei remote_monitoring.js die Datenwerte einen zufälligen Wert hinzu. Zufällige **Rpm** -Wert von einer Codezeile nach vorhandenen Transcend wie folgt hinzufügen:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. JSON-Nutzlast, die das Gerät an IoT Hub fügen Sie neue Rpm-Wert hinzu:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Ausgeführt Node.js simulierte Gerät mithilfe des folgenden Befehls:

    ```
    node remote_monitoring.js
    ```

6. Beachten Sie den neuen RPM Telemetrie ein, der im Diagramm im Dashboard zeigt:

![RPM dem Dashboard hinzufügen][image3]

> [AZURE.NOTE] Möglicherweise müssen deaktivieren und aktivieren das Gerät Node.js auf **Geräte** im Dashboard die Änderung sofort sehen.

## <a name="customize-the-dashboard-display"></a>Dashboardanzeige anpassen

**Device Info** Nachricht gehören Metadaten Telemetriedaten Gerät IoT Hub senden kann. Diese Metadaten können telemetrietypen, die das Gerät sendet. Ändern Sie den Wert **DeviceMetaData** in der Datei remote_monitoring.js eine **Telemetrie** Definition nach Definition der **Befehle** . Der folgende Codeausschnitt zeigt die Definition **Befehle** (unbedingt eine `,` nach der Definition der **Befehle** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Die remote monitoring-Lösung verwendet Kleinschreibung Übereinstimmung Metadatendefinition Stream Telemetrie Daten vergleichen.

**Telemetrie** -Definition hinzufügen, wie im vorherigen Codeausschnitt gezeigt ändert nicht das Verhalten des Dashboards. Allerdings kann die Metadaten auch **DisplayName** -Attribut zum Anpassen der Anzeige im Dashboard enthalten. Aktualisieren Sie **Telemetrie** Metadatendefinition, wie im folgenden Codeausschnitt dargestellt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Der folgende Screenshot zeigt, wie diese Änderung im Schaltpult die Diagrammlegende ändert:

![Anpassen der Diagrammlegende][image4]

> [AZURE.NOTE] Möglicherweise müssen deaktivieren und aktivieren das Gerät Node.js auf **Geräte** im Dashboard die Änderung sofort sehen.

## <a name="filter-the-telemetry-types"></a>Welche Telemetrie filtern

Das Diagramm auf dem Dashboard zeigt standardmäßig alle Datenreihen im Stream Telemetrie. Die **Geräteinformationen** Metadaten können unterdrückt die Anzeige von bestimmten Telemetrie im Diagramm. 

Damit das Diagramm nur Temperatur und Feuchtigkeit Telemetriedaten zeigen, unterlassen Sie **ExternalTemperature** **Device Info** **Telemetrie** Metadaten wie folgt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Die **Temperatur** wird nicht mehr im Diagramm angezeigt:

![Filtern der Telemetrie auf dem dashboard][image5]

Diese Änderung betrifft nur die Darstellung des Diagramms. Die Datenwerte **ExternalTemperature** noch gespeichert und zur Back-End-Verarbeitung.

> [AZURE.NOTE] Möglicherweise müssen deaktivieren und aktivieren das Gerät Node.js auf **Geräte** im Dashboard die Änderung sofort sehen.

## <a name="handle-errors"></a>Behandeln von Fehlern

Für einen Datenstream im Diagramm anzeigen muss seinen **Typ** in den Metadaten **Device Info** den Datentyp der telemetriewerte. Beispielsweise Metadaten gibt an, dass der **Typ** Luftfeuchtigkeit **Int ist** und **double** im Stream Telemetrie wird Feuchtigkeit Telemetrie nicht im Diagramm angezeigt. Allerdings **den Feuchtigkeitsgehalt** noch gespeichert und für die Back-End-Verarbeitung zur Verfügung gestellt.

## <a name="next-steps"></a>Nächste Schritte

Nun, man wie dynamische Telemetrie erfahren Sie mehr darüber, wie die vorkonfigurierten Lösungen Geräteinformationen verwenden: [Gerät Informationen Metadaten in der entfernten Datenbank überwachen vorkonfigurierte Lösung][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
