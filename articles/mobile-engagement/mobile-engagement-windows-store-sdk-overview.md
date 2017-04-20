<properties
    pageTitle="Windows SDK Universal-Integration"
    description="Universellen Integrations SDK für Windows Azure Mobile Engagement"                                     
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Universelle SDK Integration von Windows Azure Mobile Engagement

Dieses Dokument beschreibt alle Integration und Konfiguration verfügbaren Optionen für Azure Mobile Engagement Windows Universal SDK.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm starten, müssen Sie zunächst unsere [15-Minuten-Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md)ausführen.

## <a name="advanced-features"></a>Erweiterte Funktionen

### <a name="reporting-features"></a>Reporting-Funktionen
Sie können diese Funktionen hinzufügen:

1. [Erweiterte reporting-Optionen](mobile-engagement-windows-store-advanced-reporting.md)

2. [Erweiterte Konfigurationsoptionen](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Benachrichtigung

[Reichweite (Benachrichtigung) Universal Windows App integrieren](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Tag Umsetzung:

[Verwendung der erweiterte Mobile Engagement Kennzeichnung API in universelle Windows app](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Versionshinweise

###<a name="340-04192016"></a>3.4.0 (19/04/2016)

-   Verbesserung der Überlagerung zu erreichen.
-   Zusätzliche "TestLogLevel"-API aktivieren/deaktivieren/Filter konsolenprotokolle vom SDK ausgegeben.
-   Starten Sie feste in Aktivität Benachrichtigungen für die erste Aktivität App nicht angezeigt.

Frühere Versionen finden Sie im [vollständigen Versionsinformationen](mobile-engagement-windows-store-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn bereits eine ältere Version des Projekts in die Anwendung integriert haben, müssen Sie Punkte beim Aktualisieren des SDK.

Wenn mehrere Versionen des SDK verpasst haben, müssen Sie mehrere Verfahren. Vollständige [Aktualisierung Prozeduren](mobile-engagement-windows-store-upgrade-procedure.md)anzeigen Zum Beispiel wenn Sie migrieren von 0.10.1, Sie zuerst die Prozedur "von 0.9.0 zu 0.10.1 müssen" 0.11.0 "von 0.10.1 zu 0.11.0" Verfahren.

###<a name="from-330-to-340"></a>Von 3.3.0 auf 3.4.0

####<a name="test-logs"></a>Testprotokolle

Konsolenprotokolle erzeugt vom SDK können jetzt aktiviert/deaktiviert/gefiltert werden. Anpassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der Werte aus der `EngagementTestLogLevel` -Enumeration, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Ressourcen

Die Reichweite Überlagerung verbessert. Es ist Teil des SDK NuGet-Paket.

Während der Aktualisierung auf die neue Version des SDK können Sie wählen, ob vorhandenen Dateien aus dem Ordner Overlay Ressourcen oder nicht beibehalten werden sollen:

* Vorherige Overlay für Sie arbeiten, ob Sie integrieren das `WebView` Elemente manuell, dann können Sie entscheiden zu Ihrem beenden Dateien weiterhin funktioniert.
* Aktualisierung auf die neue Überlagerung ersetzen die gesamte `overlay` Ordner Ihrer Ressourcen durch das neue SDK-Paket (UWP-apps: nach der Aktualisierung erhalten Sie Overlay-Ordner von % USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Verwenden neue überschreibt Anpassungen in der vorherigen Version erstellt.

### <a name="upgrade-from-older-versions"></a>Aktualisieren von älteren Versionen

Finden Sie unter [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure.md)
