<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Pufferzeit | Microsoft Azure" 
    description="Erfahren Sie, wie mit Pufferzeit Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-slack"></a>Lernprogramm: Azure Active Directory Integration Pufferzeit
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Pufferzeit angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Puffer einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Pufferzeit Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Puffer zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Puffer
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-slack-tutorial/IC794980.png "Szenario")

##<a name="enabling-the-application-integration-for-slack"></a>Die Application Integration für Puffer
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Pufferzeit aktivieren.

###<a name="to-enable-the-application-integration-for-slack-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Puffer:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-slack-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-slack-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-slack-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-slack-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Pufferzeit**.

    ![Anwendung Galerie] (./media/active-directory-saas-slack-tutorial/IC794981.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Pufferzeit aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Szenario] (./media/active-directory-saas-slack-tutorial/IC796925.png "Szenario")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Pufferzeit Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Pufferzeit** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-slack-tutorial/IC794982.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Benutzer anmelden Pufferzeit** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-slack-tutorial/IC794983.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Pufferzeit Zeichen In URL** den URL der Pufferzeit Mieter (z.B.: "*https://azuread.slack.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-slack-tutorial/IC794984.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden an Pufferzeit** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-slack-tutorial/IC794985.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator die Pufferzeit Unternehmensstandort.

6.  Zur **an Microsoft Azure AD \> Settings Team**.

    ![Team-Einstellung] (./media/active-directory-saas-slack-tutorial/IC794986.png "Team-Einstellung")

7.  Klicken Sie in den **Team** -Einstellungen auf der Registerkarte **Authentifizierung** und klicken Sie dann auf **Anpassen**.

    ![Team-Einstellung] (./media/active-directory-saas-slack-tutorial/IC794987.png "Team-Einstellung")

8.  Gehen Sie im Dialogfeld **SAML Einstellung** :

    ![SAML-Einstellung] (./media/active-directory-saas-slack-tutorial/IC794988.png "SAML-Einstellung")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden an Pufferzeit** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **SAML 2.0 Endpunkt (HTTP)** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden an Pufferzeit** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Aussteller** .
    3.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.
    
        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    4.  Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in das **Öffentliche Zertifikat** Textbox einfügen.
    5.  Deaktivieren Sie **Benutzern erlauben, ihre e-Mail-Adresse ändern**.
    6.  **Benutzer können ihre eigenen Benutzernamen wählen**auswählen
    7.  Wählen Sie als **Authentifizierung für Ihr Team muss von verwendet** **ist optional**.
    8.  Klicken Sie auf **Konfiguration speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-slack-tutorial/IC794989.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Pufferzeit anmelden können, müssen sie in Puffer bereitgestellt.
  
Gibt es keine Aufgabe an Pufferzeit benutzerbereitstellung konfigurieren.  
Zugewiesene Benutzer Pufferzeit anmelden, wird eine Pufferzeit Konto automatisch bei Bedarf erstellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-slack-perform-the-following-steps"></a>Um Pufferzeit Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Pufferzeit **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-slack-tutorial/IC794990.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-slack-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)