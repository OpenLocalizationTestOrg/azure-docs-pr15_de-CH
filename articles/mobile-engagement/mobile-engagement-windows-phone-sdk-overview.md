<properties 
    pageTitle="Windows Phone Silverlight-SDK-Übersicht" 
    description="Übersicht über die Windows Phone Silverlight-SDK Azure Mobile Engagement"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight-SDK-Übersicht für Azure Mobile Engagement

Hier die Einzelheiten zum Azure Mobile Engagement in einer Windows Phone Silverlight-App integrieren zu starten. Möchten Sie es zuerst versuchen, stellen Sie sicher, dass unser [15 Minuten-Lernprogramm](mobile-engagement-windows-phone-get-started.md)abgeschlossen.

Klicken Sie, um die [SDK-Inhalt](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Integrationsverfahren

1. Hier beginnen: [Mobile Engagement in Windows Phone Silverlight-app integrieren](mobile-engagement-windows-phone-integrate-engagement.md)

2. Für Benachrichtigungen: [Reichweite (Benachrichtigung) in Ihrer Windows Phone Silverlight-app integrieren](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Tag-Umsetzung: [Verwendung der erweiterte Mobile Engagement tagging-API in Ihre Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Versionshinweise

###<a name="330-04192016"></a>3.3.0 (19/04/2016)
Teil der *MicrosoftAzure.MobileEngagement* Nuget-Paket **v3.4.0**

-   Zusätzliche "TestLogLevel"-API aktivieren/deaktivieren/Filter konsolenprotokolle vom SDK ausgegeben.

Frühere Version finden Sie in der [vollständigen Versionsinformationen](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn bereits eine ältere Version unseres SDK in die Anwendung integriert haben, müssen Sie Punkte im SDK aktualisieren.

Möglicherweise müssen mehrere Verfahren, wenn Sie mehrere Versionen des SDK verpasst. Vollständige [Aktualisierung Prozeduren](mobile-engagement-windows-phone-upgrade-procedure.md)anzeigen Zum Beispiel wenn Sie migrieren von 0.10.1, Sie zuerst die Prozedur "von 0.9.0 zu 0.10.1 müssen" 0.11.0 "von 0.10.1 zu 0.11.0" Verfahren.

###<a name="from-200-to-330"></a>Von 2.0.0 auf 3.3.0

####<a name="test-logs"></a>Testprotokolle

Konsolenprotokolle erzeugt vom SDK können jetzt aktiviert/deaktiviert/gefiltert werden. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen Wert von der `EngagementTestLogLevel` -Enumeration, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Aktualisieren von älteren Versionen

Finden Sie unter [Upgrade-Verfahren](mobile-engagement-windows-phone-upgrade-procedure.md)
 
