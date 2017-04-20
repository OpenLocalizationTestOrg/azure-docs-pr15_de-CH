<properties
  pageTitle="Azure IoT Suite und Logik Apps | Microsoft Azure"
  description="Ein Tutorial Logik Apps Azure IoT Suite für Geschäftsprozess einbinden."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Lernprogramm: Ihre Azure IoT Suite Remoteüberwachung vorkonfigurierte Lösung Logik App herstellen

[Microsoft Azure IoT Suite] [ lnk-internetofthings] vorkonfigurierte Lösung ist eine hervorragende Möglichkeit, schnell mit einer End-to-End-Features beginnen, der eine Lösung IoT veranschaulicht. Dieses Lernprogramm führt Sie wie Ihre Microsoft Azure IoT Suite vorkonfigurierte Lösung Remoteüberwachung Anwendung Logik hinzufügen. Diese Schritte zeigen, wie Sie mit einem Geschäftsprozess IoT Projektmappe weiter nutzen können.

_Wenn Sie eine exemplarische Vorgehensweise zum Bereitstellen einer Remoteüberwachung vorkonfigurierte Lösung suchen, finden Sie unter [Tutorial: IoT vorkonfigurierte Lösung Einstieg][lnk-getstarted]._

Bevor Sie dieses Lernprogramm starten, sollten Sie:

- Die Überwachung Vorschrift vorkonfigurierte Lösung in Azure-Abonnement.

- Erstellen Sie ein Konto SendGrid um eine e-Mail zu senden, die Ihren Geschäftsprozess auslöst. Durch **ausprobieren**klicken, können Sie sich für ein kostenloses Testabo bei [SendGrid](https://sendgrid.com/) anmelden. Nachdem Sie für Ihr kostenloses Testabo registriert haben, müssen Sie einen [API-Schlüssel](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid erstellen, die Berechtigungen zum Senden von Nachrichten erteilt. Sie benötigen diesen API-Schlüssel später im Lernprogramm.

Angenommen, Sie haben bereits bereitgestellt, die Remoteüberwachung vorkonfigurierte Lösung, navigieren Sie zu der Ressourcengruppe für diese Lösung in [Azure-Portal][lnk-azureportal]. Die Ressourcengruppe hat denselben Namen wie den Namen der Projektmappe bei Ihrem remote monitoring-Lösung bereitgestellt haben. In der Ressourcengruppe die bereitgestellten Azure-Ressourcen für Ihre Lösung außer Active Directory Azure-Anwendung angezeigt, die Sie im klassischen Azure-Portal finden. Der folgende Screenshot zeigt ein Beispiel **Ressourcengruppe** Blade eine remote monitoring vorkonfigurierte Lösung:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Zunächst soll der Anwendung Logik vorkonfigurierte Lösung verwenden.

## <a name="set-up-the-logic-app"></a>Die Logik-App einrichten

1. Klicken Sie am oberen Rand der Ressource Blatt im Azure-Portal __Hinzufügen__ .

2. Suchen Sie __Logik-App__, wählen sie und klicken Sie dann auf **Erstellen**.

3. Geben Sie __Namen__ und verwenden Sie dieselbe **Abonnement** **Ressourcengruppe** , die Sie bei Ihrem remote monitoring-Lösung bereitgestellt. Klicken Sie auf __Erstellen__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Wenn die Bereitstellung abgeschlossen ist, sehen Sie App Logik als Ressource in der Ressourcengruppe angezeigt.

5. Klicken Sie auf die Anwendung Logik zum Blatt Logik App, wählen Sie **Leere Logik App** **Logik Apps-Designer**zu öffnen.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Wählen Sie __anfordern__. Diese Aktion gibt an, dass eine eingehende HTTP-Anforderung mit einem bestimmten JSON Nutzlast fungiert als Trigger formatiert.

7. Fügen Sie folgende Anforderung Körper JSON Schema:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Kopieren der URL für HTTP Post nach dem Speichern der Anwendung Logik, aber Sie müssen zuerst eine Aktion hinzufügen.

8. Klicken Sie unter manueller Auslöser __+ Neuer Schritt__ . Klicken Sie auf **Aktion hinzufügen**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Suchen Sie **SendGrid - E-Mail** , und klicken Sie darauf.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Geben Sie einen Namen für die Verbindung wie **SendGridConnection**, den **SendGrid API-Schlüssel** erstellt, wenn Sie SendGrid Konto einrichten, und klicken Sie auf **Erstellen**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Fügen Sie e-Mail-Adressen, **die **aus** und** Felder besitzen. Fügen Sie **Remoteüberwachung Warnung [DeviceId]** in **der Betreffzeile** . Fügen Sie im Feld **E-Mail** **Gerät [DeviceId] hat [MeasurementName] [MeasuredValue] Wert gemeldet.** **[DeviceId]**, **[MeasurementName]**und **[MeasuredValue]** können durch Klicken auf im Abschnitt **Daten aus den vorherigen Schritten einfügen** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. __Klicken Sie im oberen Menü.__

13. Klicken Sie auf den Trigger **anfordern** , und kopieren Sie __Http Post an diesen URL__ -Wert. Dieser URL wird später in diesem Lernprogramm benötigen.

> [AZURE.NOTE] Logik-Apps können Sie [viele verschiedene Aktionen] ausführen[ lnk-logic-apps-actions] einschließlich in Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>Der Webauftrag EventProcessor einrichten

In diesem Abschnitt verbinden Sie vorkonfigurierte Lösung Logik App erstellen. Diese Aufgabe fügen Sie die URL der Anwendung Logik der Aktion ausgelöst, die ausgelöst wird, wenn ein Gerät Sensorwert überschreitet.

1. Git-Client verwenden, um die neueste Version von Duplizieren der [Azure-Iot-Remote-Überwachung Github Repository][lnk-rmgithub]. Zum Beispiel:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Öffnen Sie in Visual Studio __RemoteMonitoring.sln__ aus der lokalen Kopie des Repository.

3. Öffnen Sie die Datei __ActionRepository.cs__ in das **Infrastruktur\\Repository** Ordner.

4. Aktualisieren Sie **ActionIds** Wörterbuch mit der __Http-Post zu diesem URL__ Ihrer App Logik wie folgt angegeben:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Speichern Sie der Projektmappe, und beenden Sie Visual Studio.

## <a name="deploy-from-the-command-line"></a>Bereitstellen von der Befehlszeile aus

In diesem Abschnitt stellen Sie die aktualisierte Version der remote monitoring Lösung derzeit in Azure ausgeführte Version ersetzen.

1. [Dev Einrichtung] nach[ lnk-devsetup] Anleitung zum Einrichten der Umgebung für die Bereitstellung.

2.  Führen Sie lokal Bereitstellen der [lokalen Bereitstellung] [ lnk-localdeploy] Informationen.

3.  Führen Sie in der Cloud bereitstellen und Aktualisieren der vorhandenen Cloudbereitstellung [Cloudbereitstellung] [ lnk-clouddeploy] Informationen. Verwenden Sie den Namen der ursprünglichen Bereitstellung als den Namen. Für Beispiel, wenn die ursprüngliche Bereitstellung **Demologicapp**aufgerufen wurde, verwenden den folgenden Befehl:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Wenn das Skript ausgeführt wird, müssen Sie mit derselben Azure-Konto, Abonnements, Region und Active Directory-Instanz verwendet, wenn die Lösung bereitgestellt.

## <a name="see-your-logic-app-in-action"></a>Finden Sie Ihre Anwendung Logik in Aktion

Remote monitoring vorkonfigurierte Lösung hat zwei Regeln, die bei der Bereitstellung einer Lösung standardmäßig eingerichtet. Beide Regeln sind auf dem Gerät **SampleDevice001** :

* Temperatur > 38.00
* Luftfeuchtigkeit > 48,00

Temperatur-Regel löst eine Aktion **Alarm auslösen** und Feuchtigkeit Regel löst **SendMessage** -Aktion. Sofern Sie denselben URL für beide Aktionen die **ActionRepository** -Klasse verwendet, löst Ihre app Logik für eine Regel. Beide Regeln verwenden SendGrid per e-Mail **an die Details der Warnung** .

> [AZURE.NOTE] App Logik weiterhin Auslösen jedes Mal, wenn der Schwellenwert erreicht wurde. Vermeiden Sie überflüssige e-Mails deaktivieren Sie Regeln im lösungsportal oder Deaktivieren der Anwendung Logik in [Azure-Portal][lnk-azureportal].

Neben e-Mails, sehen Sie auch beim Ausführen der Anwendung Logik im Portal:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Nächste Schritte

Da Sie eine App Logik eines Geschäftsprozesses vorkonfigurierte Lösung Verbindung verwendet haben, erfahren Sie mehr über die Optionen zum Anpassen der vorkonfigurierte Lösung:

- [Verwenden Sie dynamische Telemetrie remote monitoring vorkonfigurierte Lösung][lnk-dynamic]
- [Gerät Informationen Metadaten in die Überwachung vorkonfigurierte Lösung][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
