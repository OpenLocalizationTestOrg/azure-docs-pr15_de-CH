<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Zscaler | Microsoft Azure" 
    description="Erfahren Sie, wie mit Zscaler Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Lernprogramm: Azure Active Directory-Integration mit Zscaler
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Zscaler anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Zscaler
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Zscaler
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren von Proxyeinstellungen
4.  Konfigurieren der Benutzer bereitstellen
5.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Szenario")

##<a name="enabling-the-application-integration-for-zscaler"></a>Die Application Integration für Zscaler
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Zscaler aktivieren.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Zscaler:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Zscaler**.

    ![Anwendung Galerie] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Zscaler aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Zscaler Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Zertifikat Zscaler hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Zscaler** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Zscaler** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Konfigurieren der einzelnen anmelden] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Konfigurieren der einzelnen anmelden")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Zscaler Zeichen In URL** Ihren Benutzernamen im URL von Zscaler haben, und klicken Sie auf **Weiter**: 

    >[AZURE.NOTE] Wenden Sie Zscaler Support-Team, wenn Sie nicht wissen, was Ihre URL ist.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Konfigurieren von app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Zscaler** Schritte:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Einmaliges Anmelden für konfigurieren")

    1.  Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\Zscaler.cer**.
    2.  Kopieren Sie die **Authentifizierung Request-URL** in Zwischenablage

5.  Melden Sie Ihrem Mandanten Zscaler.

6.  Klicken Sie im Menü oben auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Verwaltung")

7.  **Administratoren verwalten und Rollen**klicken Sie auf **Benutzer verwalten und Authentifizierung**.

    ![Administratoren und Rollen verwalten] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Administratoren und Rollen verwalten")

8.  Im Abschnitt **Authentifizierungsoption für Ihre Organisation wählen Sie** folgendermaßen:

    ![Wählen Sie Optionen] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Wählen Sie Optionen")

    1.  **Authentifizieren mit SAML einmaliges Anmelden**auswählen.
    2.  Klicken Sie auf **Konfigurieren SAML Single Sign-On**.

9.  Gehen Sie auf der Dialogseite **Konfigurieren SAML Single Sign-On Parameter** und dann auf **Fertig**:

    ![Zertifikat hochladen] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Zertifikat hochladen")

    1.  Fügen Sie im Textfeld **URL des Portals SAML, Benutzer für die Authentifizierung gesendet werden,** den Wert des Felds **Authentifizierung Request-URL** von klassischen Azure-Portal.
    2.  Geben Sie im Textfeld **Attribut enthält Benutzernamen** **NameID**.
    3.  Im Bereich **Öffentliche SSL-Zertifikat hochgeladen** Hochladen des Zertifikats von klassischen Azure-Portal herunterladen.
    4.  Aktivieren Sie **Automatische SAML - Bereitstellung**.

10. Führen Sie auf der **Benutzerauthentifizierung zu konfigurieren** die folgenden Schritte:

    ![Konfigurieren der Benutzerauthentifizierung] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Konfigurieren der Benutzerauthentifizierung")

    1.  Klicken Sie auf **Speichern**.
    2.  Klicken Sie auf **jetzt aktivieren**.

11. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>So konfigurieren Sie die Proxyeinstellungen in Internet Explorer

1.  Starten Sie **InternetExplorer**.

2.  **Wählen Sie aus dem Menü **Extras** das Dialogfeld **Internetoptionen** öffnen.**

    ![Internetoptionen] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internetoptionen")

3.  Klicken Sie auf die Registerkarte **Verbindung** .

    ![Anschlüsse] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Anschlüsse")

4.  Klicken Sie auf **LAN Settings** **LAN Settings** Dialog öffnen.

5.  Im Abschnitt Proxy Server die folgenden Schritte:

    ![Proxyserver] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxyserver")

    1.  Wählen Sie einen Proxyserver für LAN verwenden.
    2.  Geben Sie im Textfeld Adresse **gateway.zscalertwo.net**.
    3.  Geben Sie im Textfeld Port **80**.
    4.  Aktivieren Sie **Proxyserver für lokale Adressen umgehen**.
    5.  Klicken Sie auf **OK** , um das **lokale Netzwerk (LAN) Settings** Dialogfeld zu schließen.

6.  Klicken Sie auf **OK** , um das Dialogfeld **Internetoptionen** zu schließen.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Zscaler anmelden können, müssen sie in Zscaler bereitgestellt werden.  
Bei Zscaler ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich bei Ihrem Mandanten **Zscaler** .

2.  Klicken Sie auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Verwaltung")

3.  Klicken Sie auf **Benutzer**.

    ![Verwaltung] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Verwaltung")

4.  Klicken Sie auf der Registerkarte **Benutzer** auf **Hinzufügen**.

    ![Hinzufügen] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Hinzufügen")

5.  Führen Sie im Abschnitt Benutzer hinzufügen die folgenden Schritte aus:

    ![Benutzer hinzufügen] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Benutzer hinzufügen")

    1.  Geben Sie **UserID**, **Anzeige Benutzername**, **Kennwort**, **Kennwort bestätigen**, und wählen Sie **Gruppen** und **Abteilung** eines gültigen Kontos AAD bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen Zscaler Benutzer Konto Erstellungstool verwenden oder APIs von Zscaler Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Zscaler Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Zscaler** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
