<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit ScreenSteps | Microsoft Azure" 
    description="Erfahren Sie, wie mit ScreenSteps in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Lernprogramm: Azure Active Directory-Integration mit ScreenSteps
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und ScreenSteps anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   ScreenSteps Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der ScreenSteps Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die ScreenSteps zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für ScreenSteps
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Szenario")
##<a name="enabling-the-application-integration-for-screensteps"></a>Die Application Integration für ScreenSteps
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für ScreenSteps aktivieren.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ScreenSteps:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **ScreenSteps**.

    ![Anwendung Galerie] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **ScreenSteps aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer ScreenSteps mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **ScreenSteps** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ScreenSteps** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **ScreenSteps Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. ScreenSteps.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Konfigurieren von app-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am ScreenSteps** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens ScreenSteps.

6.  Klicken Sie auf **Konto**.

    ![Account-management] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account-management")

7.  Klicken Sie auf **Remote-Authentifizierung**.

    ![Remote-Authentifizierung] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remote-Authentifizierung")

8.  Klicken Sie auf **Create Authentifizierung Endpunkt**.

    ![Remote-Authentifizierung] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remote-Authentifizierung")

9.  Im Abschnitt **Erstellen einer Authentifizierung Endpunkt** Schritte:

    ![Erstellen einer Authentifizierung Endpunkt] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Erstellen einer Authentifizierung Endpunkt")

    1.  Geben Sie im Textfeld **Titel** einen Titel ein.
    2.  Wählen Sie aus der Liste **Modus** **SAML**.
    3.  Klicken Sie auf **Erstellen**.

10. Im Abschnitt **Authentifizierung Remoteendpunkt** Schritte:

    ![Remoteauthentifizierung Endpunkt] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remoteauthentifizierung Endpunkt")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ScreenSteps** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Remote Login-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ScreenSteps** Wert **Remote Logout-URL** und fügen Sie ihn in das Textfeld **URL Abmelden** .
    3.  Klicken Sie auf **Datei auswählen**und uploaden Sie das heruntergeladene Zertifikat.
    4.  Klicken Sie auf **Aktualisieren**.

11. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer **ScreenSteps**anmelden können, müssen sie in **ScreenSteps**bereitgestellt werden.  
Bei **ScreenSteps**ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Um ein Benutzerkonto ScreenSteps bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **ScreenSteps** .

2.  Klicken Sie auf **Konto**.

    ![Account-management] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Account-management")

3.  Klicken Sie auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Benutzer")

4.  Klicken Sie auf **Benutzer erstellen**.

    ![Alle Benutzer] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Alle Benutzer")

5.  Wählen Sie aus der Liste **Rolle** eine Rolle für den Benutzer aus.

6.  Geben Sie im Abschnitt Benutzerrolle**"Vorname**, **Nachname**, **E-Mail**, **Benutzername**, **Kennwort** und **Kennwort bestätigen"**eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.

    ![Neuer Benutzer] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Neuer Benutzer")

7.  Gruppen im Abschnitt wählen Sie **Authentifizierung Gruppe (SAML)**, und klicken Sie auf **Benutzer erstellen**.

    ![Gruppen] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Gruppen")

>[AZURE.NOTE]Können Sie alle anderen ScreenSteps Benutzer Konto Erstellungstools oder APIs von ScreenSteps Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu ScreenSteps:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **ScreenSteps **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Benutzer zuweisen] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Benutzer zuweisen")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)