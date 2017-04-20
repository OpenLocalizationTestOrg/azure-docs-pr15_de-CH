<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Clarizen | Microsoft Azure" 
    description="Erfahren Sie, wie mit Clarizen Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Lernprogramm: Azure Active Directory-Integration mit Clarizen

Das Ziel dieses Lernprogramms ist die Integration von Azure und Clarizen anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Clarizen einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Clarizen Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Clarizen zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Clarizen
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Szenario")
##<a name="enabling-the-application-integration-for-clarizen"></a>Die Application Integration für Clarizen

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Clarizen aktivieren.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Clarizen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Clarizen**.

    ![Anwendung Galerie] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Clarizen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Clarizen Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Clarizen** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Clarizen** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Einmaliges Anmelden konfigurieren")

3.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Clarizen** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Einmaliges Anmelden konfigurieren")

4.  In anderen Webbrowserfenster Anmelden als Administrator der Website Ihres **Clarizen** Unternehmens (z.B.: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Klicken Sie auf Ihren Benutzernamen **Klicken**

    ![Standardeinstellungen] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Standardeinstellungen")

6.  Klicken Sie auf der Registerkarte **Globaleinstellungen** und neben **Authentifizierung verbunden**, klicken Sie auf **Bearbeiten**.

    ![Globale Einstellungen] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globale Einstellungen")

7.  Klicken Sie im Dialogfeld **Authentifizierung verbundene** Schritte:

    ![Verbundene Authentifizierung] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Verbundene Authentifizierung")

    1.  Klicken Sie auf **Hochladen** , um heruntergeladene Zertifikat laden.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Clarizen** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **URL anmelden** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Dialogseite **Konfigurieren einzelne Abmeldung bei Clarizen** den **Einzelnen Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **Abmelde-URL** .
    4.  Wählen **Sie buchen**.
    5.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Clarizen anmelden können, müssen sie in Clarizen bereitgestellt werden.  
Bei Clarizen ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **Clarizen** Unternehmen als Administrator an.

2.  Klicken Sie auf **Benutzer**.

    ![Personen] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Personen")

3.  Klicken Sie auf **Benutzer einladen**.

    ![Benutzer einladen] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Benutzer einladen")

4.  Auf der Personen einladen die folgenden Schritte:

    ![Personen einladen] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Personen einladen")

    1.  Geben Sie im Feld **E-Mail** die e-Mail-Adresse ein gültiges Azure Active Directory-Konto, das Sie bereitstellen möchten.
    2.  Klicken Sie auf **Laden**.

    >[AZURE.NOTE] Kontoinhaber Azure Active Directory eine e-Mail und ein Link, um ihr Konto zu bestätigen, bevor aktiv wird.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Clarizen Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Clarizen **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
