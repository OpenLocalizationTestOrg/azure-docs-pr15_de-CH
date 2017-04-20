<properties
    pageTitle="Azure Mobile Web SDK Projektübersicht | Microsoft Azure"
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
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure Mobile Engagement Web SDK

Beginnen Sie hier alle Informationen Azure Mobile Engagement im Web app integrieren. Möchten Sie es zunächst mit Ihrer eigenen Anwendung versuchen, anzeigen Sie unsere [15-Minuten-Lernprogramm](mobile-engagement-web-app-get-started.md)

## <a name="integration-procedures"></a>Integrationsverfahren
1. Informationen Sie [zum Mobile Engagement in Ihrer Anwendung zu integrieren](mobile-engagement-web-integrate-engagement.md).

2. Erfahren Sie für Tag Umsetzung [die erweiterte Mobile Engagement tagging-API in Ihrer Anwendung verwenden](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Versionshinweise

### <a name="202-10182016"></a>2.0.2 (18/10/2016)

-   Absturz private Surfen (Safari).
-   Absturz auf Browser Cookies deaktiviert.

Alle Versionen finden Sie unter [vollständige Versionsinformationen](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade-Verfahren

### <a name="upgrade-from-121-to-200"></a>Aktualisieren von 1.2.1 2.0.0

In den folgenden Abschnitten wird beschrieben, wie Mobile Engagement von SDK-Integration von der Capptain Service, Capptain SAS Azure Mobile Engagement App migrieren. Wenn Sie von einer Version vor 1.2.1 migrieren, finden Sie in der Capptain-Website zunächst in 1.2.1 migrieren und wenden Sie die folgenden Verfahren an.

Diese Version von Mobile Engagement Web SDK unterstützen nicht Samsung Smart TV, Opera TV, WebOS oder Reach-Funktion.

>[AZURE.IMPORTANT] Capptain und Azure Mobile Engagement sind nicht denselben Dienst und nachfolgend markieren wie die Clientanwendung zu migrieren. Mobile Engagement Web SDK in die Anwendung migrieren migrieren nicht Daten von einem Server Capptain Mobile Engagement-Server.

#### <a name="javascript-files"></a>JavaScript-Dateien

Ersetzen Sie die Datei Capptain-sdk.js mit Datei Azure-engagement.js und aktualisieren Sie Skript importieren entsprechend.

#### <a name="remove-capptain-reach"></a>Entfernen Capptain erreichen

Diese Version von Mobile Engagement von SDK unterstützt keine Reach-Funktion. Capptain Reichweite in die Anwendung integriert haben, müssen Sie es entfernen.

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

#### <a name="remove-deprecated-apis"></a>Entfernen Sie veraltete APIs

Einige APIs aus Capptain sind in Mobile Engagement Web SDK veraltet.

Entfernen Sie alle Aufrufe an die folgenden APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, und `agent.sendMessageToDevice`.

Entfernen Sie in der Capptain-Konfiguration die folgenden Rückrufe: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, und `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguration

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

#### <a name="javascript-apis"></a>JavaScript-API

Globale JavaScript-Objekt `window.capptain` wurde `window.azureEngagement`, aber Sie können die `window.engagement` Alias für API-Aufrufe. Dieses Alias können die SDK-Konfiguration definieren.

Beispielsweise `capptain.deviceId` wird `engagement.deviceId`, `capptain.agent.startActivity` wird `engagement.agent.startActivity`und so weiter.

Wenn bereits eine frühere Version von Azure Mobile Engagement Web SDK in die Anwendung integriert haben, lesen Sie über [upgrade-Verfahren](mobile-engagement-web-upgrade-procedure.md).
