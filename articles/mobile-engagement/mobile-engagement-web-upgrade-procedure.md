<properties
    pageTitle="Azure Mobile Engagement von SDK-Aktualisierungsverfahren | Microsoft Azure"
    description="Die neuesten Updates und Verfahren zum SDK von Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile Engagement von SDK-Upgrade-Verfahren

In der Webanwendung bereits eine frühere Version von Azure Mobile Engagement Web SDK integriert haben, müssen Sie Folgendes beachten, wenn Sie das SDK aktualisieren.

Wenn mehrere Versionen von Mobile Engagement Web SDK übersprungen, müssen Sie mehrere Schritte während der Aktualisierung ausführen. Z. B. Migrieren von 1.4.0 auf 1.6.0 zunächst führen Sie die Schritte zum Aktualisieren von 1.4.0 auf 1.5.0. Befolgen Sie die Verfahren zum Aktualisieren von 1.5.0 auf 1.6.0.

Welche Version Sie aktualisieren, ersetzen alle früheren Versionen der Datei Azure-engagement.js mit der neuesten Version der Datei.

## <a name="upgrade-from-121-to-200"></a>Aktualisieren von 1.2.1 2.0.0

Dieser Abschnitt beschreibt das Mobile Engagement von SDK-Integration der Capptain Service, Capptain SAS Azure Mobile Engagement App migrieren. Migrieren von einer früheren Version finden Sie in der Capptain-Website zunächst 1.2.1 migrieren und wenden Sie die folgenden Verfahren an.

Diese Version von Mobile Engagement Web SDK unterstützen nicht Samsung Smart TV, Opera TV, WebOS oder Reach-Funktion.

>[AZURE.IMPORTANT] Capptain und Azure Mobile Engagement sind nicht denselben Dienst. Die folgende Prozedur zeigt wie die Clientanwendung migrieren. Mobile Engagement Web SDK in die Anwendung migrieren migrieren nicht Daten von einem Server Capptain Mobile Engagement-Server.

### <a name="javascript-files"></a>JavaScript-Dateien

Ersetzen Sie die Datei Capptain-sdk.js mit Datei Azure-engagement.js und aktualisieren Sie Skript importieren entsprechend.

### <a name="remove-capptain-reach"></a>Entfernen Capptain erreichen

Diese Version von Mobile Engagement von SDK unterstützt keine Reach-Funktion. Capptain erreichen in die Anwendung integriert, müssen Sie es entfernen.

CSS erreichen Import aus Ihrer Seite entfernen und Löschen der zugehörigen CSS-Datei (standardmäßig Capptain-reach.css).

Löschen Sie Folgendes erreichen: Schließen Bild (standardmäßig Capptain-close.png) und die Markensymbol (Capptain-Benachrichtigung-Symbol standardmäßig).

Entfernen Sie die Benutzeroberfläche erreichen in app-Benachrichtigungen. Das Standardlayout sieht folgendermaßen aus:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Entfernen der Benutzeroberfläche erreichen für Text und Web Ankündigungen und Umfragen. Das Standardlayout sieht folgendermaßen aus:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Entfernen Sie die `reach` -Objekt aus Konfiguration, falls vorhanden. Es sieht folgendermaßen aus:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Entfernen Sie andere Reach-Anpassung, wie Kategorien.

### <a name="remove-deprecated-apis"></a>Entfernen Sie veraltete APIs

Einige APIs aus Capptain sind in Mobile Engagement Web SDK veraltet.

Entfernen Sie alle Aufrufe an die folgenden APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, und `agent.sendMessageToDevice`.

Entfernen Sie alle Instanzen der folgenden Rückrufe in der Capptain-Konfiguration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, und `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguration

Mobile Engagement verwendet eine Verbindungszeichenfolge SDK Bezeichner, z. B. dem Anwendungsbezeichner konfigurieren.

Die Verbindungszeichenfolge ersetzen Sie die ID. Beachten Sie, dass das globale Objekt für die SDK-Konfiguration von `capptain` , `azureEngagement`.

Vor der Migration:

    window.capptain = {
      appId: ...,
      [...]
    };

Nach der Migration:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Die Verbindungszeichenfolge für die Anwendung wird im Azure-Portal angezeigt.

### <a name="javascript-apis"></a>JavaScript-API

Globale JavaScript-Objekt `window.capptain` wurde `window.azureEngagement` , aber Sie können die `window.engagement` Alias für API-Aufrufe. Den Alias können die SDK-Konfiguration definieren.

Beispielsweise `capptain.deviceId` wird `engagement.deviceId`, `capptain.agent.startActivity` wird `engagement.agent.startActivity`und so weiter.
