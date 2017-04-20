<properties 
    pageTitle="Authentifizieren Sie mit Mobile Engagement REST APIs - Manuelles Einrichten"
    description="Beschreibt die Authentifizierung für Mobile Engagement REST APIs manuell einrichten" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Authentifizieren Sie mit Mobile Engagement REST APIs - Manuelles Einrichten

Dies ist eine Anlage Dokumentation authentifizieren [mit Mobile Engagement REST-APIs](mobile-engagement-api-authentication.md). Sicherstellen Sie, dass zuerst zu Kontext lesen. Beschreibt eine alternative Möglichkeit der einmaligen Einrichtung für die Authentifizierung für Mobile Engagement REST APIs mithilfe der Azure-Portal einrichten. 

>[AZURE.NOTE] Hinweise zu diesem [Handbuch Active Directory](../resource-group-create-service-principal-portal.md) basieren und angepasste Authentifizierung für Mobile Engagement-APIs erforderlich ist. So finden sie ggf. die Schritte im Detail zu verstehen. 

1. Melden Sie Ihre Azure-Konto über das [Verwaltungsportal](https://manage.windowsazure.com/).

2. Wählen Sie im linken Bereich **Active Directory** .

     ![Wählen Sie Active Directory][1]

3. Wählen Sie das **Active Directory standardmäßig** in Azure-Portal. 

     ![Wählen Sie das Verzeichnis][2]

    >[AZURE.IMPORTANT] Dieser Ansatz funktioniert nur bei im Standard Active Directory Ihres Kontos arbeiten und funktioniert nicht, wenn Sie in Active Directory dabei, die Sie in Ihrem Konto erstellt haben. 

4. Klicken Sie die Programme im Verzeichnis auf **Anwendung**.

     ![Programme anzeigen][3]

5. Klicken Sie auf **Hinzufügen**. 

     ![Anwendung hinzufügen][4]

6. Klicken Sie auf **Mein Unternehmen entwickelten Anwendung hinzufügen**

     ![neue Anwendung][5]

6. Namen Sie der Anwendung und wählen Sie Anwendung als **Web-Anwendung oder WEB-API** und klicken Sie auf die Schaltfläche Weiter.

     ![Name-Anwendung][6]

7. Dummy URLs können Sie **Zeichen auf URL** und **APP-ID-URI**. Sie werden nicht in diesem Szenario verwendet und URLs selbst werden nicht überprüft.  

     ![Anwendungseigenschaften][7]

8. Am Ende dieser haben eine AAD-Anwendung mit dem Namen Sie zuvor wie folgt bereitgestellt. Dies ist die **AD\_APP\_Namen** und der.  

     ![Anw.-name][8]

9. Klicken Sie auf Anwendung, und klicken Sie auf **Konfigurieren**.

     ![Anwendung konfigurieren][9]

10. Notieren Sie sich die CLIENT-ID, die als verwendet werden **CLIENT\_ID** für Ihre API aufruft. 

     ![Anwendung konfigurieren][10]

11. **Der Abschnitt** und einen Schlüssel mit vorzugsweise 2 Jahre (Ablauf) und klicken Sie auf **Speichern**. 

     ![Anwendung konfigurieren][11]


12. Kopieren Sie den Wert für den Schlüssel angezeigt wird, wird jetzt nur angezeigt und nicht gespeichert und nicht wieder angezeigt werden sofort. Wenn es verloren geht, müssen Sie einen neuen Schlüssel generieren. **CLIENT_SECRET** für API-Aufrufe werden. 

     ![Anwendung konfigurieren][12]

    >[AZURE.IMPORTANT] Dieser Schlüssel läuft am Ende der Dauer, die Sie angegeben, müssen Sie bei der andernfalls API Authentifizierung erneuern nicht mehr funktioniert. Sie können auch löschen und dieser Schlüssel erstellen, wenn Sie vermuten, dass dieser gefährdet ist.
 
13. Klicken Sie auf **Ansicht ENDPUNKTE** nun öffnet das Dialogfeld **App Endpunkte** . 

    ![][13]

14. Kopieren Sie im Dialogfeld Anwendung Endpunkte **OAUTH 2.0 TOKEN ENDPUNKT**. 

    ![][14]

15. In der folgenden Form werden die GUID in der URL ist die **TENANT_ID** so Notieren sie diesen Endpunkt: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Nun werden wir die Berechtigungen für diese Anwendung konfigurieren. Dafür müssen Sie das [Azure-Portal](https://portal.azure.com)öffnen. 

17. Klicken Sie auf **Ressourcen** und finden Sie Ressourcengruppe **Mobile Engagement** .  

    ![][15]

18. Klicken Sie auf die Ressourcengruppe **Mobile Engagement** und Blatt **Einstellungen** hier navigieren. 

    ![][16]

19. **Benutzer** im Blatt Einstellungen auf und klicken Sie dann auf **Hinzufügen** , um einen Benutzer hinzuzufügen. 

    ![][17]

20. Klicken Sie auf **Wählen Sie eine Rolle**

    ![][18]

21. Klicken Sie auf **Besitzer**

    ![][19]

22. Suche nach dem Namen der Anwendung **AD\_APP\_namens** in das Feld Suchen. Dies wird nicht hier standardmäßig angezeigt werden. Sobald Sie sie gefunden haben, wählen sie und klicken Sie auf, **Wählen Sie** unten das Blade. 

    ![][20]

23. Auf dem Blatt **Zugriff hinzufügen** wird es als **Benutzer 1 und 0 Gruppen**angezeigt. Klicken Sie auf diese Änderung bestätigen auf **OK** . 

    ![][21]

Konfiguriert den AAD ist nun abgeschlossen und Sie alle soll die APIs aufrufen. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



