<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration und zentrale Desktop | Microsoft Azure" 
    description="Erfahren Sie, wie mit zentralen Desktop Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Lernprogramm: Azure Active Directory-Integration und zentrale Desktop

Das Ziel dieses Lernprogramms ist die Integration von Azure und zentrale Desktop anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Zentrale desktop einmaliges Anmelden aktiviert Abonnement / zentrale Desktop Mieter

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für zentrale Desktop aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Szenario")
##<a name="enabling-the-application-integration-for-central-desktop"></a>Anwendungsintegration für zentrale Desktop aktivieren

Dieser Abschnitt soll beschreiben die Anwendungsintegration für zentrale Desktop aktivieren.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für zentrale Desktop:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Zentrale Desktop**.

    ![Anwendung Galerie] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Zentrale Desktop aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Zentrale Desktop] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Zentrale Desktop")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer zentralen Desktop mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat Ihrem Mandanten zentrale Desktop hochgeladen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Zentrale Desktop** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zentralen Desktop anmelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Einmaliges Anmelden für konfigurieren")

3.  Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus und klicken Sie dann auf **Weiter**: 

    -   **Zentrale Desktop Zeichen In URL** -Textfeld Geben Sie die URL der zentrale Desktop-Tenant (z.B.: *http://contoso.centraldesktop.com*).
    -   Geben Sie im Textfeld zentrale Desktop-Antwort-URL den zentralen Desktop AssertionConsumerService URL (z. B.: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Rufen Sie den Wert aus dem zentralen desktop Metadaten (z.B.: *http://contoso.centraldesktop.com*).

    ![Konfigurieren von app-URL] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Konfigurieren von app-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am zentralen Desktop** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie sich bei Ihrem **Zentralen Desktop** Mandanten.

6.  **Einstellungen**, klicken Sie auf **Erweitert**und **Einmaliges Anmelden**klicken.

    ![Setup - erweitert] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - erweitert")

7.  Auf die **einzelnen Zeichen auf** die folgenden Schritte:

    ![Einmaliges auf] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Einmaliges auf")

    1.  Wählen Sie **Enable SAML v2 für einmaliges Anmelden**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am zentralen Desktop** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **SSO-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges zentrale Desktop** **Remote Login-URL** -Wert und fügen Sie ihn in das Textfeld **SSO Anmelde-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges zentrale Desktop** **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **SSO Logout URL** .

8.  Im Abschnitt **Nachricht Signatur Verifizierungsmethode** Schritte:

    ![Nachricht Signatur Verifizierungsmethode] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Nachricht Signatur Verifizierungsmethode")

    1.  **Zertifikat**auswählen
    2.  Wählen Sie aus der Liste **SSO-Zertifikat** **RSH SHA256**.
    3.  Erstellen Sie eine Datei aus dem heruntergeladenen Zertifikat, kopieren Sie den Inhalt der Textdatei und fügen Sie ihn in das Feld **SSO-Zertifikat** .  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    4.  Wählen Sie **eine Verknüpfung mit der SAMLv2-Anmeldeseite**.

9.  Klicken Sie auf **Aktualisieren**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

AAD Benutzer anmelden müssen sie die zentrale Desktop-Anwendung bereitgestellt werden. Dieser Abschnitt beschreibt die AAD zentrale Desktop erstellen.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Bereitstellung von Benutzerkonten zentrale Desktop

1.  Melden Sie sich bei Ihrem zentralen Desktop Mandanten.

2.  Gehen Sie zu **Personen \> interne Member**.

3.  Klicken Sie auf **interne Member**.

    ![Personen] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Personen")

4.  Geben Sie in das Textfeld **E-Mail-Adresse der neuen Mitglieder** ein Konto AAD bereitstellen möchten, und klicken Sie dann auf **Weiter**.

    ![E-Mail-Adressen der neuen Mitglieder] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-Mail-Adressen der neuen Mitglieder")

5.  Klicken Sie auf **interne fügen Mitglieder**.

    ![Internen Member hinzufügen] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Internen Member hinzufügen")

    >[AZURE.NOTE] Die hinzugefügten Benutzer erhalten eine e-Mail, die einen Bestätigungslink enthält, müssen sie das Konto aktivieren klicken.

>[AZURE.NOTE] Andere zentrale Desktop Benutzer Konto Erstellungstools verwenden oder APIs bereitgestellt durch zentrale Desktop Bereitstellung AAD Benutzerkonten

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Um zentrale Desktop Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Zentrale Desktop** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
