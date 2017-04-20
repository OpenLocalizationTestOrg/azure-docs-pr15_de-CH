<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Salesforce | Microsoft Azure"
    description="Erfahren Sie, wie mit Salesforce Azure Active Directory-auf automatisierte Bereitstellung und mehr!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Exemplarische Vorgehensweise: Salesforce Azure Active Directory integrieren

In diesem Lernprogramm wird wie der Salesforce-Umgebung zu Azure Active Directory verbunden angezeigt. Ermöglicht automatisches Benutzer bereitstellen einmaliges Salesforce konfigurieren und Zuweisen von Benutzern auf Salesforce erfahren.

##<a name="prerequisites"></a>Erforderliche Komponenten

1. Zugriff auf Azure Active Directory über das [klassische Azure-Portal](https://manage.windowsazure.com)müssen Sie ein gültiges Azure-Abonnement verfügen.

2. Einen gültigen Mieter benötigen in [Salesforce.com](https://www.salesforce.com/).

> [AZURE.IMPORTANT] Bei Verwendung **einer Salesforce.com-Testkonto** wird dann konfigurieren automatisierte benutzerbereitstellung möglich. Testkonten haben nicht den erforderlichen API-Zugriff aktiviert, bis sie erworben wurden.
> 
> Diese Einschränkung erhalten über ein [kostenloses Entwicklerkonto](https://developer.salesforce.com/signup) zum Bearbeiten dieses Lernprogramms.

Bei Verwendung von Salesforce Sandbox-Umgebung finden Sie unter [Salesforce Sandbox Integration Tutorial](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Video-Lehrgänge

Sie können mit den folgenden Videos Lernprogramms.

**Video-Tutorial Teil 1: Single Sign-On aktivieren**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Video-Tutorial Teil 2: Benutzerbereitstellung Automatisieren von**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Schritt 1: Hinzufügen von Salesforce zum Verzeichnis

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Wählen Sie im linken Navigationsbereich des Active Directory.][0]

2. Wählen Sie aus der Liste **Verzeichnis** Verzeichnis Salesforce hinzufügen möchten.

3. Klicken Sie im oberen Menü auf **Applications** .

    ![Klicken Sie auf Anwendung.][1]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Klicken Sie auf Hinzufügen, um eine neue Anwendung hinzufügen.][2]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Klicken Sie auf eine Anwendung aus dem Katalog hinzufügen.][3]

6. Geben Sie im **Suchfeld** **Salesforce**. Wählen Sie **Salesforce** aus den Ergebnissen aus, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Salesforce hinzufügen.][4]

7. Sie sollten nun Seite Schnellstart Salesforce finden Sie unter:

    ![Salesforce Quick Start Page in Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Schritt 2: Aktivieren Sie einmaliges Anmelden

1. Bevor Sie einmaliges Anmelden konfigurieren können, müssen Sie einrichten und Bereitstellen eine benutzerdefinierte Domäne für Ihre Salesforce-Umgebung. Informationen dazu finden Sie unter [Festlegen ein Domänenname](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Salesforce Seite Schnellstart in Azure AD klicken Sie auf **Configure einmaliges Anmelden** .

    ![Die einzelnen anmelden Schaltfläche Konfigurieren][6]

3. Ein Dialogfeld wird geöffnet, und Sie sehen einen Bildschirm mit der Frage "Wie Benutzer Salesforce anmelden möchten?" **Azure AD einmaliges**wählen Sie aus und dann auf **Weiter**.

    ![Wählen Sie Azure AD einmaliges Anmelden][7]

    > [AZURE.NOTE] Weitere Informationen zu den verschiedenen einzelnen Zeichen auf Optionen, [Klicken Sie hier](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work) zu

4. Füllen Sie auf der Seite **Konfigurieren App Settings** **Anmelde-URL** in der Salesforce-Domänen-URL im folgenden Format eingeben:
 - Enterprise-Konto:`https://<domain>.my.salesforce.com`
 - Entwicklerkonto:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Geben Sie die Zeichen im URL][8]

5. Auf der Seite **Konfigurieren einmaliges Anmelden bei Salesforce** auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Zertifikat herunterladen][9]

6. Öffnen einer neuen Registerkarte im Browser und melden Sie sich bei Ihrem Administratorkonto Salesforce.

7. Klicken Sie im Navigationsbereich **Administrator** **Sicherheitsmaßnahmen** zum entsprechenden Abschnitt erweitern. Klicken Sie auf **einzelne anmelden**.

    ![Klicken Sie auf einzelne Anmeldung Einstellungen Sicherheitskontrollen][10]

8. Klicken Sie auf die **einzelnen anmelden** auf die Schaltfläche **Bearbeiten** .

    ![Klicken Sie auf die Schaltfläche Bearbeiten][11]

    > [AZURE.NOTE] Wenn Sie einmaliges Anmelden für Ihr Konto Salesforce können können, müssen Sie Vertriebs Support wenden, um die Sie aktiviert haben.

9. Wählen Sie **SAML aktiviert**, und klicken Sie auf **Speichern**.

    ![Wählen Sie SAML aktiviert][12]

10. Um Ihre SAML einzelne Anmelden konfigurieren, klicken Sie auf **neu**.

    ![Wählen Sie SAML aktiviert][13]

11. Überprüfen Sie auf der Seite **SAML Single Sign-On Einstellung bearbeiten** die folgenden Konfigurationen:

    ![Screenshot der Konfigurationen, die Sie durchführen sollten][14]

 - Das Feld **Name** Geben Sie einen Namen für diese Konfiguration. Ein Mehrwert für **Namen** im Textfeld **API-Name** automatisch ausgefüllt.

 - Kopieren Sie in Azure AD Wert **Aussteller URL** und fügen Sie ihn in **das Ausstellerfeld in Salesforce** .

 - Geben Sie im **Textfeld Entitäts-Id**Salesforce DomainName dem folgenden Muster:
     - Enterprise-Konto:`https://<domain>.my.salesforce.com`
     - Entwicklerkonto:`https://<domain>-dev-ed.my.salesforce.com`

 - Klicken Sie auf **Durchsuchen** **Aus Datei** öffnen **Datei zum Hochladen auswählen** , wählen Sie Ihr Zertifikat Salesforce und **Klicken** zum Hochladen des Zertifikats.

 - Wählen Sie für **SAML Identitätstyp** **Assertion enthält salesforce.com Benutzername**.

 - Wählen Sie für **SAML Identität Speicherort** **Identität ist im NameIdentifier-Element der Betreff-Anweisung**

 - Kopieren Sie in Azure AD **Remote Login-URL** -Wert und fügen Sie ihn in das Feld **Identität Anbieter Anmelde-URL** Salesforce.

 - Wählen Sie für **Service Provider initiiert Binding anfordern** **HTTP-Umleitung**.

 - Klicken Sie abschließend auf **Speichern** , um SAML einzelnen Anmeldung Einstellungen anzuwenden.

12. Im linken Navigationsbereich in Salesforce auf **Domain Management** den zugehörigen Abschnitt erweitern und klicken Sie dann auf **Meine Domäne**.

    ![Klicken Sie auf meine Domäne][15]

13. Bildlauf zum Abschnitt **Konfiguration** , und klicken Sie auf die Schaltfläche **Bearbeiten** .

    ![Klicken Sie auf die Schaltfläche Bearbeiten][16]

14. Wählen Sie im Abschnitt **Authentifizierungsdienst** den Anzeigenamen der SAML SSO-Konfiguration aus und dann auf **Speichern**.

    ![Wählen Sie die SSO-Konfiguration][17]

    > [AZURE.NOTE] Wenn mehr als ein Authentifizierungsdienst aktiviert ist, werden dann Wenn Benutzer versuchen, einmaliges Anmelden für Ihre Umgebung Salesforce initiieren sie aufgefordert, welcher Authentifizierungsdienst wählen sie anmelden möchten. Wenn Sie dies nicht möchten, dann sollten Sie **Alle anderen deaktivierten Authentifizierungsdienste lassen**.

15. Kontrollkästchen Sie in Azure AD Konfiguration für einzelne Zeichen Bestätigung ermöglichen Salesforce Hochladen des Zertifikats. Klicken Sie auf **Weiter**.

    ![Aktivieren Sie das Kontrollkästchen Bestätigung][18]

16. Geben Sie auf der letzten Seite des Dialogfelds eine e-Mail-Adresse möchten Sie e-Mail-Benachrichtigungen für Fehler und Warnungen im Zusammenhang mit der Wartung dieser Konfiguration für einzelne Zeichen. 

    ![Geben Sie Ihre e-Mail-Adresse.][19]

17. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen. Testen Ihrer Konfiguration finden Sie weiter unten im Abschnitt [Benutzer Salesforce zuweisen](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Schritt 3: Automatisierte benutzerbereitstellung

1. Klicken Sie auf Azure AD Schnellstart für Salesforce auf die Schaltfläche **Konfigurieren Benutzer bereitstellen** .

    ![Klicken Sie auf Benutzerbereitstellung konfigurieren][20]

2. Geben Sie im Dialogfeld **Konfigurieren benutzerbereitstellung** in der Salesforce-Benutzername und Kennwort.

    ![Geben Sie Ihren Benutzernamen Admin bzw.][21]

    > [AZURE.NOTE] Konfigurieren einer empfiehlt, erstellen Sie ein neues Administratorkonto in Salesforce speziell für diesen Schritt. Dieses Konto muss in Salesforce zugewiesene **System Administrator** -Profil.

3. Um Ihre Salesforce-Sicherheit token anzuzeigen, öffnen Sie eine neue Registerkarte und Zeichen in demselben Salesforce-Administratorkonto. In der oberen rechten Ecke der Seite auf Ihren Namen und klicken Sie dann auf **Meine Einstellungen**.

    ![Klicken Sie auf Ihren Namen und klicken Sie auf Meine Einstellungen][22]

4. Klicken Sie im linken Navigationsbereich auf **Persönliche** zugehörigen Abschnitt erweitern und klicken auf **Zurücksetzen Meine Sicherheitstoken**.

    ![Klicken Sie auf Ihren Namen und klicken Sie auf Meine Einstellungen][23]

5. Klicken Sie auf der Seite **Zurücksetzen Meine Sicherheitstoken** auf **Sicherheitstoken zurücksetzen** .

    ![Lesen Sie die Warnung.][24]

6. Überprüfen der e-Mail-Posteingangs dieses Administratorkonto zugeordnet. Suchen einer e-Mail aus "Salesforce.com", die das neue Token enthält.

7. Kopieren Sie das Token der Azure AD Fenster und **Benutzer Sicherheitstoken** Feld einfügen. Klicken Sie auf **Weiter**.

    ![Fügen Sie das token][25]

8. Auf der Bestätigungsseite können Sie e-Mail benachrichtigt bei der Bereitstellung Fehler auftreten. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen.

    ![Geben Sie Ihre e-Mail-Adresse verfügbar][26]

##<a name="step-4-assign-users-to-salesforce"></a>Schritt 4: Zuweisen von Benutzern zu Salesforce

1. Testen Sie Ihre Konfiguration erstellen Sie zunächst einen Test-Account im Verzeichnis.

2. Klicken Sie auf der Seite Salesforce Schnellstart auf **Benutzer zuweisen** .

    ![Klicken Sie auf Benutzer zuweisen][27]

3. Wählen Sie Ihre Testbenutzer und klicken Sie **weisen** am unteren Bildschirmrand:

 - Wenn Sie nicht aktivieren Sie benutzerbereitstellung automatisierter sehen die folgende Meldung zu bestätigen:

        ![Confirm the assignment.][28]

 - Wenn Automatisches provisioning Benutzer aktiviert haben, dann sehen Sie aufgefordert, die definieren, welche Salesforce-Profil der Benutzer haben. Neu eingerichtete Benutzer sollte nach einigen Minuten in der Salesforce-Umgebung angezeigt.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Wenn Sie eine Salesforce **Entwickler** -Umgebung bereitstellen, haben Sie wenige Lizenzen für jedes Profil. Daher empfiehlt es sich Benutzer auf **Chatter freien** Benutzerprofil 4.999 Lizenzen hat.

4. Testen die Einstellungen für einzelnen Zeichen, öffnen Sie die Abdeckung am [https://myapps.microsoft.com](https://myapps.microsoft.com/)und das Testkonto melden Sie an und **Salesforce**auf.

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
