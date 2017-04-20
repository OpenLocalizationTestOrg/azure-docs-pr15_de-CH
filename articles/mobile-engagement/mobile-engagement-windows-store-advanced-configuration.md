<properties
    pageTitle="Erweiterte Konfiguration für Windows Universal Apps Engagement SDK"
    description="Erweiterte Konfigurationsoptionen für Azure Mobile universelle Windows-Apps"                    
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Erweiterte Konfiguration für Windows Universal Apps Engagement SDK

> [AZURE.SELECTOR]
- [Universelle Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Dieses Verfahren beschreibt, wie verschiedene Konfigurationsoptionen für Azure Mobile Engagement apps konfigurieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Erweiterte Konfiguration

### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturzberichte

Sie können den Absturz Berichterstellungsfunktion Engagement deaktivieren. Dann tritt ein Ausnahmefehler nicht Engagement.

> [AZURE.WARNING] Wenn Sie dieses Feature deaktivieren, dann sendet bei in Ihrer Anwendung eine nicht behandelte Absturz Engagement nicht abstürzt **und** keine Sitzung und Projekte schließen.

Deaktivieren der automatischen Absturzberichte passen Sie Ihrer Konfiguration je nach es deklariert an:

#### <a name="from-engagementconfigurationxml-file"></a>Von `EngagementConfiguration.xml` Datei

Legen Sie Bericht Absturz auf `false` zwischen `<reportCrash>` und `</reportCrash>` Tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Von `EngagementConfiguration` zur Laufzeit

Setzen Sie Bericht Absturz auf False das EngagementConfiguration-Objekt verwenden.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Deaktivieren der Echtzeit-reporting

Standardmäßig protokolliert der Engagement Berichte in Echtzeit. Meldet die Anwendung Protokolle häufig, empfiehlt sich die Protokolle Puffer und sie gleichzeitig über die reguläre Zeit. "Burst-Modus" wird aufgerufen.

Rufen Sie dazu die Methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**. Echtzeit-Protokollierung aktivieren möchten, rufen Sie die Methode ohne Parameter oder mit dem Wert 0.

Burst-Modus leicht erhöht die Lebensdauer der Batterie jedoch wirkt sich auf das Engagement: alle Sitzungen und Aufträge Dauer Burst-Schwellenwert (also Sessions und kürzer als Burst-Schwellenwert werden Aufträge) gerundet. Wir empfehlen einen Burst-Schwellenwert nicht mehr als 30000 (30). Gespeicherte Protokolle sind 300 Elemente auf. Wenn senden zu lang ist, verlieren Sie einige Protokolle.

> [AZURE.WARNING] Burst-Schwellenwert kann auf weniger als eine Sekunde konfiguriert werden. Wenn Sie dies tun, das SDK zeigt eine Spur mit dem Fehler und automatisch zurückgesetzt auf den Standardwert 0 Sekunden. Dadurch wird das SDK Protokolle in Echtzeit melden.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
