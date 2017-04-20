<properties
 pageTitle="Optionaler Abschnitt - ein- und Ausschalten der LED-Verhalten ändern | Microsoft Azure"
 description="Anpassen der Nachrichten der LEDS ein- und ausschalten Verhalten ändern."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 Optionaler Abschnitt: ein- und Ausschalten der LED-Verhalten zu ändern

## <a name="421-what-you-will-do"></a>4.2.1 Was Sie tun

Anpassen der Nachrichten der LEDS ein- und ausschalten Verhalten ändern. Erfüllt Probleme suchen Sie Lösungen [Seite Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 Lerninhalte

Mithilfe zusätzliche Node.js-Funktionen der LEDS ein- und ausschalten Verhalten.

## <a name="423-what-you-need"></a>4.2.3 benötigen Sie

Sie müssen erfolgreich [4.1 ausführen eine beispielanwendung Himbeeren Pi Cloud Gerät Nachrichten empfangen](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)abgeschlossen haben.

## <a name="424-add-nodejs-functions"></a>4.2.4 Node.js Funktionen hinzufügen

1. Öffnen Sie die beispielanwendung in Visual Studio Code durch Ausführen der folgenden Befehle:

    ```bash
    cd Lesson4
    code .
    ```

2. Öffnen der `app.js` Datei, und fügen Sie am Ende die folgenden Funktionen hinzu:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![app.js aktualisieren](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Fügen Sie Folgendes vor dem Standard im Switch-Case-Block von der `receiveMessageCallback` Funktion:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Jetzt haben Sie Beispiel-Anwendung reagieren auf Weitere Informationen über Nachrichten konfiguriert. Die Anweisung "on" schaltet die LED und die Anweisung "off" deaktiviert die LED.

4. Öffnen Sie die Datei gulpfile.js, und fügen Sie eine neue Funktion vor der Funktion `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![Gulpfile aktualisieren](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. In der `sendMessage` und Ersetzen Sie die Zeile `var message = buildMessage(sentMessageCount);` mit der neuen Zeile im folgenden Codeausschnitt dargestellt:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Speichern Sie alle Änderungen.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 bereitstellen und Ausführen der beispielanwendung

Bereitstellen und Ausführen der beispielanwendung Pi durch Ausführen des folgenden Befehls:

```bash
gulp
```

Die LED für zwei Sekunden aktiviert und anschließend deaktiviert weitere zwei Sekunden sollte angezeigt werden. Die letzte Meldung "stop" beendet die beispielanwendung ausgeführt.

![ein- und ausschalten](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Herzlichen Glückwunsch! Sie haben erfolgreich die Nachrichten angepasst, die die PI von IoT Hub gesendet werden.

### <a name="427-summary"></a>4.2.7 Zusammenfassung

In diesem optionale Abschnitt demos Nachrichten anpassen, damit die beispielanwendung ein- und Ausschalten der LED-Verhalten anders steuern kann.

