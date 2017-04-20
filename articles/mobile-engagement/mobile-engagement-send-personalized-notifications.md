<properties 
    pageTitle="Personalisierte Benachrichtigung mit Azure Mobile Engagement" 
    description="Wie Sie personalisierte Benachrichtigungen senden Benutzerprofilinformationen Benachrichtigung wie ihre Namen"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Personalisieren von Benachrichtigungen mit Benutzername

Bei der Suche zu Benachrichtigungen ansprechender gestalten app sollten Sie sie personalisieren. Eine leistungsfähige Methode umfasst selektiv mit Benutzernamen app Benachrichtigungen zu persönlicher. -Vorsicht sollten Sie Hinzufügen von Benutzernamen Meldungen sorgfältig Ansatz denn wenn dieser Strategie viele dann es für einige Benutzer app als Unheimliches konnte über. Außerdem sollten Sie nutzen diese Daten Ihnen volle Zustimmung in der app bieten vor dieser Benutzer lassen. 

Technisch gesehen mit Azure Mobile Engagement erreichen Sie personalisieren der Benachrichtigung durch die folgenden Schritte in denen nun das Szenario einschließlich Benutzernamen in der Benachrichtigung verwendet. Verwendet das Konzept der App Info oder Tags, deren Werte entweder durch die SDKs übergeben konnte, in der Mobile-Anwendung oder über APIs integriert. Diese App-Infos oder Tags können dann verwendet werden:

1. für die Ausrichtung an bestimmte Benutzer basierend auf den Werten des App-Info oder 
2. als Platzhalter in der Benachrichtigung der Werte für Benutzer oder Geräts ersetzt beim Senden von Benachrichtigungen an das Gerät. 

> [AZURE.IMPORTANT] Beachten Sie, dass die Geschwindigkeit der Benachrichtigungen eine aufgrund dieses zusätzliche Verarbeitung jeder Benachrichtigung app Info Werte ersetzen angezeigt wird. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Registrieren Sie App-Informationen im Mobile Engagement-Portal

1) Sie beginnen mit App Info oder Tags in Azure-Portal registriert. **Standardeinstellungen**zum -> für diesen**Tag (App-Info)** .  

![][1]  

2) Klicken Sie auf **Neues Tag (app-Info)** und den Namen als *Benutzername* und der Typ als *Zeichenfolge* und **Absenden**. 

![][2]

3) Abschließend sehen Sie diese app-Informationen wie folgt registriert:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>App-Informationen vom Client SDK senden

Hier verwenden wir Universal Windows app wird aber entsprechende Methoden für unsere SDKs auch. 

Vorausgesetzt, Sie haben eine Methode in der app Sie die Profilinformationen des Benutzers wie ihre Namen wahrscheinlich erhalten nach authentifizieren, rufen Sie `SendAppInfo` Methode und den Wert der `user_name` app-Informationen, die Sie zuvor in Mobile Engagement Service Backend registriert. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Personalisierte Benachrichtigung

Jetzt können Sie auf dieser **Benutzername**-Benachrichtigung senden. 

1) Zum Mobile Engagement-Portal auf der Registerkarte **erreichen** eine Benachrichtigung erstellt und können diese Platzhalter im folgenden Format in der Benachrichtigungstitel oder Textkörper. 

![][4]  

> [AZURE.NOTE] Benutzer für die Benutzername app Info nicht festgelegt ist, erhalten Sie keine Benachrichtigung. Wenn Notification-Kampagne im Testmodus ausführen und wenn Sie nicht dann senden wir app-Info '?' Zeichen Platzhalter ersetzen. 

2) Mobile Engagement wählen als Benachrichtigung zu senden, dann wird diese app Info und Ersetzen Sie den Wert im Platzhalter ein.  
Angenommen, wir legen `str = "Scott"` für einen Benutzer als Gerät Registrierung erhalten mit der app Info **Benutzername = SCOTT** für Benutzer und dieser Benutzer nicht app Push-Benachrichtigung im folgenden Format angezeigt wird. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

