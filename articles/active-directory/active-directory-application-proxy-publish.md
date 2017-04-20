<properties
    pageTitle="Apps mit Azure AD-Anwendungsproxy veröffentlichen | Microsoft Azure"
    description="Lokale Anwendung in die Cloud mit Azure AD-Anwendungsproxy zu veröffentlichen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Veröffentlichen Sie mit Azure AD-Anwendungsproxy Applikationen

Azure AD-Anwendungsproxy können Sie remote-Mitarbeiter unterstützen veröffentlichen lokalen Anwendung über das Internet zugegriffen werden. Zu diesem Zeitpunkt haben bereits [aktiviert Anwendungsproxy im klassischen Azure-Portal](active-directory-application-proxy-enable.md)Sie. Dieser Artikel führt Sie durch die Anwendung veröffentlichen, die auf Ihrem lokalen Netzwerk und sicheren remote-Zugriff von außerhalb des Netzwerks. Nach Abschluss dieses Artikels werden Sie persönliche Informationen oder Sicherheit die Anwendung konfigurieren.

> [AZURE.NOTE] Anwendungsproxy kann ist nur verfügbar, wenn Sie Premium oder Basic Edition von Azure Active Directory aktualisiert. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>Veröffentlichen einer Anwendung mithilfe des Assistenten

1. Melden Sie sich als Administrator in [Azure-Verwaltungsportal](https://manage.windowsazure.com/)an.
2. Wechseln Sie zu Active Directory, und wählen Sie das Verzeichnis, in dem Anwendungsproxy aktiviert.

    ![Active Directory - Symbol](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klicken Sie auf die Registerkarte **Programme** , und klicken Sie dann am unteren Bildschirmrand **Hinzufügen**

    ![Anwendung hinzufügen](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Wählen Sie **Veröffentlichen einer Anwendung, die von außerhalb des Netzwerks zugänglich sind**.

    ![Veröffentlichen Sie eine Anwendung, die von außerhalb des Netzwerks zugänglich](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Angaben Sie folgende über die Anwendung:

    - **Name**: der Anzeigename für die Anwendung. Sie müssen im Verzeichnis eindeutig sein.
    - **Interne URL**: die Adresse Application Proxy Connector verwendet, um die Anwendung in Ihrem privaten Netzwerk zuzugreifen. Sie können einen bestimmten Pfad auf dem Back-End-Server veröffentlichen, während der Rest des Servers aufgehoben wird. Auf diese Weise können andere Sites auf demselben Server veröffentlichen und geben jeweils einen eigenen Namen und Regeln.

        > [AZURE.TIP] Wenn Sie einen Pfad veröffentlichen, stellen Sie sicher, dass alle erforderlichen Bilder, Skripts und Stylesheets für die Anwendung enthält. Beispielsweise wenn Ihr https://yourapp/app am und am https://yourapp/media Bilder verwendet, sollte dann https://yourapp/ als Pfad veröffentlichen.

    - **Vorauthentifizierung Methode**: wie Anwendungsproxy überprüft Benutzer vor dem Zugang zu Ihrer Anwendung. Wählen Sie eine der Optionen aus dem Dropdown Menü aus.

        - Azure Active Directory: Anwendungsproxy leitet Benutzer Azure AD anmelden, die ihre Berechtigungen für das Verzeichnis und die Anwendung authentifiziert.
        - Passthrough: Benutzer müssen Zugriff auf die Anwendung zu authentifizieren.

    ![Anwendungseigenschaften](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Klicken Sie auf das Häkchen am unteren Bildschirmrand, um den Assistenten abzuschließen. Die Anwendung wird jetzt in Azure AD definiert.


## <a name="assign-users-and-groups-to-the-application"></a>Weisen Sie Benutzer und Gruppen für die Anwendung

Damit der Benutzer Zugriff auf Ihre veröffentlichten Anwendung müssen Sie diese einzeln oder in Gruppen zuordnen. (Denken Sie daran, sich Zugang zu zuweisen.) Dies erfordert, dass jeder Benutzer eine Lizenz für grundlegende Azure oder höher verfügen. Sie können Lizenzen einzeln oder Gruppen zuweisen. Weitere Informationen finden Sie unter [Zuweisen von Benutzern zu einer Anwendung](active-directory-applications-guiding-developers-assigning-users.md) . 

Für apps, die Vorauthentifizierung erfordern erteilt diese der Anwendung. Für apps, die Vorauthentifizierung nicht erforderlich ist, können Benutzer weiterhin die Anwendung zugewiesen werden, damit es in der Anwendungsliste wie MyApps angezeigt wird.

1. Nach Beendigung des Assistenten App hinzufügen, wird die Seite Schnellstart für Ihre Anwendung angezeigt. Wählen Sie **Benutzer und Gruppen**, um Zugriff auf die Anwendung zu verwalten.

    ![Anwendung Proxy Schnellstart weisen Benutzer - screenshot](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Bestimmte Gruppen in Ihrem Verzeichnis suchen oder alle Benutzer anzeigen. Um die Suchergebnisse anzuzeigen, klicken Sie auf das Häkchen.

    ![Suchen Sie nach Gruppen oder Benutzer - screenshot](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Wählen Sie die Benutzer oder Gruppe dieser Anwendung und klicken Sie auf **zuweisen**. Sie werden aufgefordert, diese Aktion zu bestätigen.

> [AZURE.NOTE] Integrierte Windows-Authentifizierung Apps weisen Sie nur Benutzer und Gruppen synchronisiert werden aus der lokalen Active Directory. Benutzer mit einem Microsoft-Konto und Gäste anmelden können für apps mit Azure Active Directory-Anwendungsproxy veröffentlicht zugewiesen werden. Vergewissern Sie sich Ihre Benutzer Anmeldeinformationen anmelden, die Teil der gleichen Domäne wie die Anwendung, die Sie veröffentlichen.

## <a name="test-your-published-application"></a>Testen Sie die veröffentlichte Anwendung

Nach der Anmeldung können Sie testen sie zu der URL, die Sie veröffentlicht. Stellen Sie sicher, dass Sie zugreifen, ordnungsgemäß ausgibt und alles funktioniert wie erwartet. Probleme oder Fehlermeldungen sollten Sie die [Fehlerbehebung](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurieren Sie Ihre Anwendung

Sie können veröffentlichte apps ändern oder Optionen auf der Seite konfigurieren. Auf dieser Seite können Sie Ihre app durch Ändern des Namens oder Logo hochladen. Sie können auch Regeln wie die Vorauthentifizierung Methode oder kombinierte Authentifizierung verwalten.

![Erweiterte Konfiguration](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Nach dem Veröffentlichen der Anwendung mit Azure Active Directory Application Proxy erscheinen in der Liste in Azure AD und dort verwaltet werden.

Wenn Sie Anwendungsproxy Dienste deaktivieren, nachdem Sie Applikationen veröffentlicht haben, sind sie nicht mehr außerhalb des privaten Netzwerks zugegriffen werden. Dadurch wird die Anwendung nicht gelöscht.

Zum Anzeigen einer Anwendung und stellen Sie sicher, dass es möglich ist, doppelklicken Sie auf den Namen der Anwendung. Die Anwendung ist deaktiviert und die Anwendung ist nicht verfügbar, wird eine Warnung am oberen Bildschirmrand angezeigt.

Um eine Anwendung zu löschen, wählen Sie eine Anwendung in der Liste und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- [Veröffentlichen Sie mit Ihren eigenen Domänennamen Applikationen](active-directory-application-proxy-custom-domains.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)
- [Arbeiten mit Ansprüchen Beachten](active-directory-application-proxy-claims-aware-apps.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
