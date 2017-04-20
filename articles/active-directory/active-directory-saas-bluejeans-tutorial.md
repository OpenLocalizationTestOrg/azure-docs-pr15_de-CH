<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration BlueJeans | Microsoft Azure" 
    description="Erfahren Sie, wie mit BlueJeans Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Lernprogramm: Azure AD-Integration mit BlueJeans

Das Ziel dieses Lernprogramms ist die Integration von Azure und BlueJeans anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine BlueJeans einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der BlueJeans Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie BlueJeans zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für BlueJeans
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Szenario")
##<a name="enabling-the-application-integration-for-bluejeans"></a>Die Application Integration für BlueJeans

Dieser Abschnitt soll beschreiben die Anwendungsintegration für BlueJeans aktivieren.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für BlueJeans:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **BlueJeans**.

    ![Anwendung Galerie] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **BlueJeans aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer BlueJeans Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **BlueJeans** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei BlueJeans** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL BlueJeans** den URL dem folgenden Muster "*https://company.BlueJeans.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am BlueJeans** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens **BlueJeans** .

6.  Zu **ADMIN \> Gruppeneinstellungen \> Security**.

    ![Admin] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")

7.  Führen Sie die folgenden Schritte im Bereich **Sicherheit** :

    ![SAML für einmaliges Anmelden] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML für einmaliges Anmelden")

    1.  Wählen Sie **SAML einmaliges Anmelden**.
    2.  Aktivieren Sie **die automatische Bereitstellung**.

8.  Mit den folgenden Schritten fortfahren:

    ![Pfad] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Pfad")

    1.  Klicken Sie auf **Datei auswählen**und uploaden Sie das heruntergeladene Zertifikat.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am BlueJeans** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am BlueJeans** Wert **Ändern Kennwort URL** und fügen Sie ihn in das Textfeld **Kennwort ändern URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am BlueJeans** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .

9.  Mit den folgenden Schritten fortfahren:

    ![Speichern] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Speichern")

    1.  Geben Sie im Textfeld **Benutzer-Id** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  Geben Sie in das Textfeld **E-Mail** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer BlueJeans anmelden können, müssen sie in BlueJeans bereitgestellt werden.  
Bei BlueJeans ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich der Website Ihres Unternehmens **BlueJeans** als Administrator an.

2.  Gehen Sie zu **ADMIN \> Benutzer verwalten \> Benutzer hinzufügen**.

    ![Admin] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")

    >[AZURE.IMPORTANT] " **Benutzer hinzufügen** " ist nur verfügbar, wenn auf der **Registerkarte Sicherheit** **ermöglichen die automatische Bereitstellung** deaktiviert ist.

3.  Führen Sie im Abschnitt **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Benutzer hinzufügen] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Benutzer hinzufügen")

    1.  Geben Sie **BlueJeans Benutzername**eine **e-Mail-Adresse**eine **BlueJeans Besprechung ID**, einen **Moderator Code**, einen **Vollständigen Namen**, das **Unternehmen** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer hinzufügen**.

>[AZURE.NOTE] Können Sie alle anderen BlueJeans Benutzer Konto Erstellungstools oder APIs von BlueJeans Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Um BlueJeans Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **BlueJeans **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
