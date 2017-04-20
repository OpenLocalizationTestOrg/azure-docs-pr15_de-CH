<properties 
   pageTitle="Problembehandlung bei Azure Mobile Engagement-Handbücher" 
   description="Problembehandlung bei Azure Mobile Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile Engagement - Fehlerbehebung

## <a name="introduction"></a>Einführung
Die folgenden Fehlerbehebung hilft Ihnen zu verstehen, dass Ursachen und einige häufig Probleme selbst beheben können. 

## <a name="general"></a>Allgemein

Im Allgemeinen sollten Sie immer Folgendes sicherstellen:

1. Stellen Sie sicher, dass Sie alle für die Integration erforderlich, wie unsere [Lernprogramme für erste Schritte](mobile-engagement-windows-store-dotnet-get-started.md) durchlaufen haben
2. Sie verwenden die neueste Version des Platform SDKs. 
3. Da einige Probleme nur Emulator sind auf ein Gerät und einen Emulator zu testen. 
4. Sie schlagen keine Grenzwerte/Drosselungen aus Mobile dokumentiert sind [hier](../azure-subscription-service-limits.md)
5. Wenn Sie nicht mit Mobile Engagement Service Backend verbinden oder sehen nicht zu ladenden Daten kontinuierlich dann sicherzustellen, dass keine laufende Dienstereignisse überprüfen [hier](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>"Monitor" Probleme

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Das Gerät auf der Registerkarte Monitor nicht angezeigt
Registerkarte Monitor zeigt Geräte Ihrer Plattform Mobile Engagement in Echtzeit. Beim Debuggen auf einem Emulator und Gerät sollte mindestens eine Sitzung hier angezeigt werden. Wenn die Anwendung verteilt wurde, sehen Sie aktive Sessions Monitor Geräte entsprechend auf die Plattform in Echtzeit verbunden. 

Wenn Sie Ihr Gerät nicht auf der Registerkarte Monitor sehen ist es wahrscheinlich ein Problem der SDK-Integration. Einige allgemeine Schritte zur Problembehandlung sind wie folgt:

1. Sicherstellen Sie, dass Sie die richtige Verbindungszeichenfolge in der app und der SDK Schlüssel und nicht den API-Schlüssel ist. Die Verbindungszeichenfolge verbindet der mobilen Anwendung auf die Instanz der app Mobile Engagement in der Sie das Gerät auf der Registerkarte Monitor sehen. 
2. Für Windows-Plattform - Wenn Ihr überschreibt die `OnNavigatedTo` Methode, müssen Sie aufrufen `base.OnNavigatedTo(e)`.
3. Wenn Sie Mobile Engagement in eine vorhandene app integrieren Sie können sicherstellen, dass Sie keine Schritte fehlen anhand der erweiterten Integrationsschritte [hier](mobile-engagement-windows-store-integrate-engagement.md)
4. Sicherstellen Sie, dass Sie mindestens Bildschirmaktivitäten senden, durch Überschreiben der Seite je nach Plattform arbeiten Sie unter [Lernprogramme für erste Schritte](mobile-engagement-windows-store-dotnet-get-started.md)mit EngagementActivity.

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Ich sehe die Registerkarte Monitor zeigt eine Sitzung auch wenn ich getrennt oder geschlossen Meine app / Emulator. 
Wenn Sie die einzige nun mit der Plattform verbunden und dabei einen Emulator die app öffnen ist dieser Emulator Quirks wahrscheinlich. Im Allgemeinen müssen Sie sicherstellen, dass Sie wieder zum Startbildschirm auf dem Emulator für die app-Sitzung erfolgreich getrennt. Darüber hinaus auf Windows-Plattform müssen beim Debuggen mit Visual Studio Sie sicherstellen, dass in Visual Studio Sie **Lebenszyklusereignisse** Menüleiste und klicken Sie auf **Anhalten** die Sitzung wirklich schließen. Details finden Sie im [Windows-Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md) . 

## <a name="analytics-issues"></a>"Analysen" Probleme

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>Ich sehe keine Daten / Data Analytics Register aktualisiert 
Analysedaten in regelmäßigen Abständen neu berechnet und bis zu 24 Stunden dauern für diese Aktualisierung. Diese Daten nicht Echtzeit und Sie sehen in dieser 24 Stunden aktualisiert.
Führen Sie hierfür jedoch mindestens einen Bildschirm oder Aktivität Backend Plattform durch eine überschreibende mindestens einer Seite senden `EngagementActivity` oder `SendActivity` explizit. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Ich sehe falsch aufgezeichnetes Zeitpunkt ein Register Analytics
Der Zeitraum für die Analyse basiert auf Anfangsdatum der Benutzer Einstellungen. So sicher, dass das Gerät das Datum korrekt festgelegt. 

## <a name="segment-issues"></a>'Segment' Probleme

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Ich ein Segment erstellt und angezeigt abgeblendet oder keine Daten angezeigt
Segment erstellen ist derzeit nicht in Echtzeit. Analytics-Daten aggregiert werden und bis zu 24 Stunden dauern kann, wird Sie zur gleichen Zeit berechnet. Sie sollten später aber inzwischen Sie sollten auch sicherstellen, dass Ihr mobiler apps tatsächlich Daten senden der Segmente bilden. Z.b. Wenn ein Ereignis sagen "Foo" wird nicht von jedem Gerät gesendeten gäbe es nicht Segmentdaten für Segment erstellt mit EventName = Foo als Kriterium. Sie sollten auch überprüfen, die SDK-Integration der mobilen Anwendung sicherstellen Daten korrekt senden. 

## <a name="reach-or-push-notifications-issues"></a>Probleme erreichen"oder Pushbenachrichtigungen

### <a name="my-push-messages-are-not-being-delivered"></a>Push Nachrichten werden nicht übermittelt 

1. Senden Sie Benachrichtigungen Testgerät zuerst um sicherzustellen, dass alle Komponenten - mobile-app, SDK und der Dienst korrekt angeschlossen und Pushbenachrichtigungen liefern. 
2. Die einfachste "Out-of-app-Benachrichtigung immer zuerst über nicht geplante Kampagne senden und noch ein Publikum Kriterium angegeben. Dies ist wieder Benachrichtigung Konnektivität ordnungsgemäß nachzuweisen. 
3. Wenn Sie Probleme bei in-app-Benachrichtigungen dann ist auch ein guter erster Schritt versucht, zuerst eine Out-of-app-Benachrichtigung senden. 
4. Sicherstellen Sie, dass die "einheitlichen verbreiten" für die mobile Anwendung ordnungsgemäß konfiguriert ist. Je nach Plattform umfasst es entweder Schlüssel (Android, Windows) oder Zertifikate (iOS). Benutzeroberfläche [- Einstellung](mobile-engagement-user-interface-settings.md)
5. App Benachrichtigungen auch so durch die Benutzer über mobile OS blockiert können sicherstellen Sie, dass dies nicht der Fall ist. 
6. Stellen Sie sicher, dass Sie nicht die Option *ignorieren Publikum Push erhalten Benutzer API* im Abschnitt **Kampagne** Kampagnen erreichen festlegen da dadurch sicherstellen, dass Pushbenachrichtigungen nur über APIs gesendet werden konnten. 
7. Sicherstellen Sie, dass Sie pushkampagne mit beide Geräte über WIFI und Telefon Operator Netzwerkanschluss als mögliche Ursache von Problemen zu testen.
8. Überprüfen Sie das Systemdatum/-Zeit für den Geräteemulator da synchron Gerät auch Push-Benachrichtigungsdienst Benachrichtigungen beeinträchtigen kann. 

Weitere plattformspezifisch Problembehandlung beschrieben:

1. **iOS** 

    - Sicherstellen Sie, dass die Zertifikate gültig und nicht abgelaufene für iOS Pushbenachrichtigungen. 
    - Sicherstellen Sie, dass ordnungsgemäß *Produktion* Zertifikat Mobile Engagement app konfigurieren. 
    - Stellen Sie sicher, dass Tests auf einer *physikalischer Geräte.* IOS-Simulator kann Push Nachrichten nicht verarbeiten.
    - Sicherstellen Sie, dass die Paket-ID in der app richtig konfiguriert ist. Finden Sie [hier](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Beim Testen der verwenden Sie "Ad-hoc-Verteilung in Ihrem mobilen Bereitstellung Profil. Sie werden nicht benachrichtigt, wenn Ihre Anwendung mit "Debug" kompiliert wird

2. **Android**

    - Sicherstellen Sie, dass die richtige Projektnummer in der mobilen Anwendung AndroidManifest.xml Datei angegeben haben, \n Zeichen folgen. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Damit sind nicht vorhanden oder falsch konfigurierten Berechtigungen in Android Manifestdatei 
    - Sicherstellen Sie, dass die Projektnummer, die die Clientanwendung hinzufügen dasselbe Konto ist, wo Sie den Serverschlüssel GCM. Jede Abweichung zwischen den beiden verhindert die Push ausgehen. 
    - Systembenachrichtigungen jedoch nicht innerhalb der app und Überprüfung [Geben Sie ein Symbol für Benachrichtigungen Abschnitt](mobile-engagement-android-get-started.md) als empfangen Sie nicht das richtige Symbol in die Android-Manifestdatei fest. 
    - Wenn BigPicture Benachrichtigung senden Sie sicherstellen, dass bei Bild Server müssen sie zu Support HTTP "GET" und "HEAD".

3. **Windows**
    
    - Stellen Sie sicher, dass Sie eine gültige Windows Store-app die app zugeordnet. In Visual Studio - müssen Sie wählen Sie die die Option "App mit Speicher zuordnen" und wählen die app im Windows Store erstellten. Windows Store-app sollte identisch, von wo Sie die systemeigenen Push Anmeldeinformationen im Mobile Engagement-Portal konfigurieren.
    - Wenn Sie Out-of-app Pushbenachrichtigungen aber nicht app Benachrichtigungen erhalten `EngagementOverlay` Integration dann sicherzustellen, dass ein Stammelement Raster auf der Seite. EngagementOverlay wird in der XAML-Datei zwei findet das erste "Grid"-Element auf der Seite anzeigt. Wenn Sie festgelegtem Webansicht suchen möchten, definieren Sie ein Raster mit dem Namen "EngagementGrid", wie dies jedoch müssen Sie sicherzustellen, dass ausreichend Höhe und Breite für die beiden nachfolgenden Web zeigt die Benachrichtigung und folgende Meldung als in app-Benachrichtigung zeigt:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Eine Push-Benachrichtigung/Ankündigung erstellte Kampagne und sogar nachdem es mir eine Benachrichtigung, als "Aktiv" angezeigt. Was bedeutet das? 
Die **Kampagne** , die Sie in Mobile Engagement erstellt wird, ist ein langer Push Notification Bedeutung wie neue Ihrer Plattform mobile Engagement Geräte, sie automatisch die Benachrichtigung, die Sie hier konfigurieren gesendet, solange sie das Kriterium erfüllen, die, das Sie der Kampagne festgelegt, bezeichnet. Dies ist kein Schuss Benachrichtigung. Sie müssen manuell auf die Schaltfläche **Fertig stellen** , Kampagne, weiteren Benachrichtigungen senden nicht beenden. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Pushkampagne erstellte und ich erhalte Benachrichtigungen erfolgreich jedoch wenn ich die app öffnen, die gleiche Mitteilung man selbst wenn habe verarbeitet vor? 
Dies wahrscheinlich während der Testphase ist und wenn Sie Emulatoren oder einige Testframework wie niedriger. Was geschieht hier ist, dass jede Instanz app ist eine neue Geräte-ID abrufen und Senden an unsere-Back-End, Mobile Engagement-Plattform sowie ein neues Gerät senden der Benachrichtigung behandelt wird. 

## <a name="getting-support"></a>Unterstützung

Wenn Sie nicht das Problem kann selbst dann:

1. Das Problem in vorhandenen Threads StackOverflow Forum und [MSDN-Forum](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) suchen und wenn nicht, dann Frage vorhanden. 
2. Wenn Sie eine fehlende dann hinzufügen/Abstimmung für die Anforderung [UserVoice Forum](https://feedback.azure.com/forums/285737-mobile-engagement/) finden
3. Haben Sie Microsoft unterstützen offene Supportanfragen durch folgende Angaben: 
    - Azure-Abonnement-ID
    - Plattform (z.B. iOS, Android usw.)
    - App-ID
    - Kampagnen-ID (für Push-Benachrichtigung (Probleme)
    - Geräte-ID
    - Mobile Engagement SDK-Version (z.B. Android SDK-v2.1.0)
    - Fehlerdetails Szenario mit genauen Fehlermeldung
