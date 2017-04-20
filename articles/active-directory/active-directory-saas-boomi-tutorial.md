<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Boomi | Microsoft Azure" 
    description="Erfahren Sie, wie mit Boomi Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Lernprogramm: Azure Active Directory-Integration mit Boomi

Das Ziel dieses Lernprogramms ist die Integration von Azure und Boomi anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Boomi einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Boomi Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Boomi zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Boomi
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Szenario")
##<a name="enabling-the-application-integration-for-boomi"></a>Die Application Integration für Boomi

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Boomi aktivieren.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Boomi:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Boomi**.

    ![Anwendung Galerie] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Boomi aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Boomi Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Boomi** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Boomi** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Boomi Antwort-URL** der **Boomi ist Anmelde-URL** (z. B.: "*https://platform.boomi.com/sso/AccountName/saml*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Boomi** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Boomi.

6.  Klicken Sie in der oberen Symbolleiste auf Ihren Firmennamen und **Setup**.

    ![Setup] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Setup")

7.  Klicken Sie auf **SSO-Optionen**.

    ![SSO-Optionen] (./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO-Optionen")

8.  Im Abschnitt **Optionen für einzelne Zeichen** folgendermaßen:

    ![Single Sign-On - Optionen] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Single Sign-On - Optionen")

    1.  Aktivieren Sie **SAML einmaliges Anmelden**.
    2.  Klicken Sie auf **Importieren**, um heruntergeladene Zertifikat laden.
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Boomi** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .
    4.  **Föderation Id Speicherort**wählen Sie **Verbund-Id wird NameID Element des Betreffs aus**.
    5.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Boomi anmelden können, müssen sie in Boomi bereitgestellt werden.  
Bei Boomi ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **Boomi** Unternehmen als Administrator an.

2.  Gehen Sie zu **Verwaltung \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Benutzer")

3.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Benutzer hinzufügen")

4.  Führen Sie auf der **Benutzerrollen hinzufügen** die folgenden Schritte:

    ![Benutzer hinzufügen] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Benutzer hinzufügen")

    1.  Geben Sie den **Vornamen**, **Nachnamen** und **E-Mail** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf OK.

>[AZURE.NOTE] Können Sie alle anderen Boomi Benutzer Konto Erstellungstools oder APIs von Boomi Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Boomi Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Boomi **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
