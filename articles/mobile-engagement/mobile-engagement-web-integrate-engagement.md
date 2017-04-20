<properties
    pageTitle="Azure Mobile Engagement von SDK-Integration | Microsoft Azure"
    description="Die neuesten Updates und Verfahren für Azure Mobile Engagement Web SDK"
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
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Azure Mobile Engagement in eine Anwendung integrieren

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Die Verfahren in diesem Artikel beschreiben die einfachste Möglichkeit, Analysen und Überwachungsfunktionen in Azure Mobile Engagement in der Webanwendung aktivieren.

Gehen Sie das Protokoll meldet aktivieren, die alle Statistiken Benutzer Sessions, Aktivitäten, Abstürze und technische berechnet. Anwendungsabhängige Statistiken wie Ereignisse, Fehler und Aufträge müssen Sie Protokollberichte manuell mithilfe der Azure Mobile Engagement-API aktivieren. Weitere Informationen dazu [die erweiterte Mobile Engagement tagging-API in einer Web-Anwendung verwenden](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Einführung

[Azure Mobile Engagement Web SDK herunterladen](http://aka.ms/P7b453).
Mobile Engagement Web SDK geliefert, als eine JavaScript-Datei, Azure-engagement.js, die in jede Seite Ihrer Website oder Anwendung aufnehmen.

> [AZURE.IMPORTANT] Bevor Sie dieses Skript ausführen, führen Sie einen Skript oder Code-Ausschnitt, den Sie schreiben Mobile Engagement für die Anwendung konfigurieren.

## <a name="browser-compatibility"></a>Browserkompatibilität

Mobile Engagement Web SDK verwendet systemeigene JSON Codierung und Decodierung neben domänenübergreifende AJAX Anfragen (anhand der Spezifikation W3C CORS). Es ist kompatibel mit den folgenden Browsern:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Mobile Engagement konfigurieren

Schreiben Sie ein Skript, eine globale erstellt `azureEngagement` JavaScript-Objekt, wie im folgenden Beispiel. Da Ihre Site Vielfache Seiten haben, wird angenommen, dass dieses Skript in jeder Seite enthalten ist. In diesem Beispiel das JavaScript-Objekt namens `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Die `connectionString` Wert für Ihre Anwendung in Azure-Portal angezeigt.

> [AZURE.NOTE] `appVersionName`und `appVersionCode` sind optional. Jedoch empfohlen, dass Sie sie so konfigurieren, dass Analytics Informationen verarbeiten kann.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobile Engagement Skripts in Ihre Seiten einfügen
Mobile Engagement Skripts Seiten auf eine der folgenden Arten hinzufügen:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Oder:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Nach dem Laden des Mobile Engagement von SDK-Skripts erstellt **Engagement** Alias zum SDK-APIs zugreifen. Dieses Alias können die SDK-Konfiguration definieren. Dieser Aliasname wird in dieser Dokumentation als Referenz verwendet.

Beachten Sie, dass wenn Konflikt der Standardalias mit anderen globalen Variablen von Ihrer Seite aus Sie sie in der Konfiguration wie folgt definieren können vor dem Laden von Mobile Engagement Web SDK:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Grundlegende Berichterstattung

Grundlegende Berichterstattung in Mobile Engagement umfasst Sitzung Statistiken, wie Statistiken über Benutzer, Sitzung, Aktivitäten und stürzt ab.

### <a name="session-tracking"></a>Session-tracking

Mobile Engagement-Sitzung in eine Sequenz von Aktivitäten unterteilt, jeweils durch einen Namen.

Klassische Website empfohlen, eine andere Aktivität auf jeder Seite Ihrer Website zu deklarieren. Für eine Website oder Anwendung in der aktuelle Seite nie ändert, möchten Sie verfolgen die Aktivitäten in kleinerem Maßstab wie innerhalb der Seite.

In jedem Fall starten oder Ändern der aktuelle Benutzeraktivität, rufen die `engagement.agent.startActivity` Funktion. Zum Beispiel:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Mobile Engagement-Servers endet automatisch eine offene Sitzung innerhalb von drei Minuten nach dem Schließen der Anwendungsseite.

Auch Sie können beenden eine Sitzung manuell durch Aufrufen von `engagement.agent.endActivity`. Dadurch wird der aktuelle Benutzeraktivität "Idle"  Beenden der Sitzung 10 Sekunden, wenn so `engagement.agent.startActivity` wird die Sitzung fortgesetzt.

Sie können 10 Sekunden im globalen Engagement-Objekt wie folgt konfigurieren:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Können keine `engagement.agent.endActivity` in der `onunload` Rückruf da AJAX-Aufrufe in dieser Phase durchführen kann.

## <a name="advanced-reporting"></a>Erweiterte Berichte

Wenn Sie anwendungsspezifische Ereignisse, Fehler und Aufträge melden möchten, müssen Sie gegebenenfalls Mobile Engagement-API verwenden. Mobile Engagement API durch Aufrufen der `engagement.agent` Objekt.

Sie können alle erweiterten Funktionen in Mobile Engagement in Mobile Engagement-API zugreifen. Die API wird in Artikel [wie die erweiterte Mobile Engagement tagging-API in einer Web-Anwendung](mobile-engagement-web-use-engagement-api.md)beschrieben.

## <a name="customize-the-urls-used-for-ajax-calls"></a>Für AJAX-Aufrufe verwendeten URLs anpassen

Sie können URLs, die Mobile Engagement Web SDK verwendet. Zum Definieren der Protokoll-URL (die SDK-Endpunkt für die Protokollierung) können Sie z. B. die Konfiguration wie folgt überschreiben:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Wenn die URL-Funktion eine Zeichenfolge zurück, der mit `/`, `//`, `http://`, oder `https://`, das Standardschema nicht verwendet. Standardmäßig die `https://` Schema für URLs verwendet. Das Standardschema anpassen, überschreiben Sie die Konfiguration wie folgt:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
