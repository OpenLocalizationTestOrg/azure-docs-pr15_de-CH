<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Feder CM | Microsoft Azure" 
    description="Erfahren Sie, wie mit Feder CM Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Lernprogramm: Azure Active Directory Integration Feder CM
  
Das Ziel dieses Lernprogramms ist einrichten einmaliges Anmelden zwischen Azure Active Directory und Faxaufkommen anzeigen.
  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Faxaufkommen einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden Azure Active Directory-Benutzer, die Sie Faxaufkommen zugewiesen haben einmaliges Abdeckung AAD verwenden können.

1.  Die Application Integration für Faxaufkommen
2.  Konfigurieren von Single Sign-On
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Szenario")

##<a name="enabling-the-application-integration-for-springcm"></a>Die Application Integration für Faxaufkommen
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Faxaufkommen aktivieren.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Faxaufkommen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Faxaufkommen**.

    ![Anwendung Galerie] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Faxaufkommen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Faxaufkommen] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "Faxaufkommen")

##<a name="configuring-single-sign-on"></a>Konfigurieren von Single Sign-On
  
Dieser Abschnitt beschreibt, wie Benutzer Faxaufkommen mit seinem Konto in Azure Active Directory Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Faxaufkommen** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Faxaufkommen** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL Faxaufkommen** die URL der Anwendung Faxaufkommen Anmelden von Benutzern verwendet, und klicken Sie auf **Weiter**. 

    Die app URL ist der Faxaufkommen Mieter (z.B.: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![Konfigurieren von App-URL] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Faxaufkommen** das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Datei lokal auf dem Computer.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Gesamtauthentifizierung konfigurieren")

5.  In anderen Webbrowserfenster melden Sie sich auf der Website Ihres Unternehmens **Faxaufkommen** als Administrator.

6.  Im Menü oben auf **GEHE zu**, klicken Sie auf **Voreinstellungen**und klicken Sie im **Konto** -Einstellungen **SAML SSO**.

    ![SAML SSO] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SAML SSO")

7.  Im Abschnitt Identität Anbieterkonfiguration Schritte:

    ![Identität Anbieterkonfiguration] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identität Anbieterkonfiguration")

    1.  Klicken Sie zum Hochladen des heruntergeladenen Azure Active Directory Zertifikats **Ausstellerzertifikat wählen** oder **Ausstellerzertifikat ändern**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Faxaufkommen** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Faxaufkommen** **Singel Sign-On Service URL** -Wert und fügen Sie ihn in das Textfeld **Service Provider (SP) initiiert Endpunkt** .
    4.  **Als **SAML aktiviert**aktivieren.**
    5.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Gesamtauthentifizierung konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure Active Directory-Benutzer Faxaufkommen anmelden können, müssen sie in Faxaufkommen bereitgestellt werden.  
Bei Faxaufkommen ist die Bereitstellung einer manuellen Aufgabe.

>[AZURE.NOTE] Weitere Informationen finden Sie unter [Erstellen und Bearbeiten eines Benutzers Faxaufkommen](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Um ein Benutzerkonto Faxaufkommen bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **Faxaufkommen** Unternehmen als Administrator an.

2.  Klicken Sie auf **Gehe zu**, und klicken Sie auf **Adressbuch**.

    ![Benutzer erstellen] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Benutzer erstellen")

3.  Klicken Sie auf **Benutzer erstellen**.

4.  Wählen Sie eine **Rolle**aus.

5.  Wählen Sie **E-Mail senden**.

6.  Geben Sie Vorname, Nachname und e-Mail-Adresse eines gültigen Benutzerkontos Azure Active Directory in verknüpfte Textfelder bereitstellen möchten.

7.  Den Benutzer zu einer **Sicherheitsgruppe**hinzufügen.

8.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen Faxaufkommen Benutzer Konto Erstellungstools oder APIs von Faxaufkommen Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Um Faxaufkommen Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Faxaufkommen** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)




