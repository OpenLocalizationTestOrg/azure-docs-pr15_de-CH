<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit AppDynamics | Microsoft Azure" 
    description="Erfahren Sie, wie mit AppDynamics Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Lernprogramm: Azure Active Directory-Integration mit AppDynamics

Das Ziel dieses Lernprogramms ist die Integration von Azure und AppDynamics anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein AppDynamics SSO-Abonnement aktiviert

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der AppDynamics Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie AppDynamics zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für AppDynamics
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Szenario")
##<a name="enabling-the-application-integration-for-appdynamics"></a>Die Application Integration für AppDynamics

Dieser Abschnitt soll beschreiben die Anwendungsintegration AppDynamics aktivieren.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für AppDynamics:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **AppDynamics**.

    ![Anwendung Galerie] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **AppDynamics aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer AppDynamics Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **AppDynamics** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Gesamtauthentifizierung konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei AppDynamics** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Gesamtauthentifizierung konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL AppDynamics** den URL der Benutzer anmelden, AppDynamics ("*https://companyname.saas.appdynamics.com*") verwendet, und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am AppDynamics** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Gesamtauthentifizierung konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens AppDynamics.

6.  **Klicken Sie in der oberen Symbolleiste**und dann auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Verwaltung")

7.  Klicken Sie auf die Registerkarte **Authentifizierung** .

    ![Authentifizierungsanbieter] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Authentifizierungsanbieter")

8.  Im Abschnitt **Authentifizierung** die folgenden Schritte:

    ![SAML-Konfiguration] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML-Konfiguration")

    1.  Wählen Sie als **Authentifizierungsanbieter** **SAML**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am AppDynamics** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am AppDynamics** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    4.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    5.  Das Base64-codierte Zertifikat im Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **Zertifikat** Textbox einfügen
    6.  Klicken Sie auf **Speichern**.
        ![Speichern] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Speichern")

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Gesamtauthentifizierung konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer AppDynamics anmelden können, müssen sie in AppDynamics bereitgestellt werden.  
Bei AppDynamics ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  AppDynamics der Website Ihres Unternehmens als Administrator anmelden.

2.  **Benutzer**wechseln und dann auf **+** , öffnen Sie das Dialogfeld **Benutzer erstellen** .

    ![Benutzer] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Benutzer")

3.  Folgendermaßen Sie im Abschnitt **Erstellen Benutzer** an:

    ![Benutzer erstellen] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Benutzer erstellen")

    1.  Geben Sie **Benutzername**, **Name**, **E-Mail**, **Passwort**, **Neues Kennwort wiederholen** ein gültiges Konto AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen AppDynamics Benutzer Konto Erstellungstools oder APIs von AppDynamics Bereitstellung Azure AD-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>AppDynamics Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **AppDynamics **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
