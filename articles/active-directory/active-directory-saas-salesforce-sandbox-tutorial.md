<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Salesforce Sandbox | Microsoft Azure"
    description="Erfahren Sie, wie mit Salesforce Sandbox Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Lernprogramm: Azure Active Directory Integration Salesforce Sandbox
>[AZURE.TIP]Feedback, finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Salesforce Sandbox anzeigen.  
Sandboxen können Sie mehrere Kopien Ihrer Organisation in einer separaten Umgebung für verschiedene Zwecke wie Entwicklung, Test und Schulung, ohne die Daten und in Ihrer Organisation Salesforce-Anwendung erstellen.  
Weitere Informationen finden Sie in der [Sandbox-Übersicht](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Sandbox in "Salesforce.com"
  
Wenn Sie noch nicht über eine gültige Sandbox in Salesforce.com, mit Salesforce müssen.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Salesforce Sandbox aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Aktivieren der Domäne
4.  Konfigurieren der Benutzer bereitstellen
5.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Szenario")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Anwendungsintegration für Salesforce Sandbox aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Salesforce Sandbox aktivieren.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Salesforce Sandbox:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Applikationen")

4.  Öffnen Sie die **Anwendung Galerie**auf **Einer Anwendung hinzufügen**und dann auf **eine Anwendung für meine Organisation hinzufügen**.

    ![Was möchten Sie tun?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Was möchten Sie tun?")

5.  Geben Sie im **Suchfeld** **Salesforce-Sandbox**.

    ![Anwendung Galerie] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Anwendung Galerie")

6.  Wählen Sie im Ergebnisbereich **Salesforce Sandbox aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Salesforce-Sandbox] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce-Sandbox")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Salesforce Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Salesforce Sandbox** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Salesforce Sandbox** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Salesforce-Sandbox] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce-Sandbox")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL** den URL dem folgenden Muster `http://company.my.salesforce.com`, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Konfigurieren von App-URL")

4. Wenn Sie bereits einmaliges Anmelden für eine andere Instanz von Salesforce Sandbox im Verzeichnis konfiguriert haben, müssen Sie auch **Bezeichner** um denselben Wert wie die **URL anmelden**konfigurieren. **Das Bezeichnerfeld** finden durch Aktivieren des Kontrollkästchens **Erweiterte Einstellungen anzeigen** des Dialogfelds auf **App-URL konfigurieren** .

4.  Auf der Seite **Konfigurieren einmaliges Anmelden in Salesforce Sandbox** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator Ihre Salesforce-Sandbox.

6.  Klicken Sie im Menü oben auf **Setup**.

    ![Setup] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")

7.  Klicken Sie im Navigationsbereich auf der linken Seite klicken Sie auf **Sicherheit**und dann auf **Einstellungen für einzelne Zeichen**.

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Einstellung für einzelne Zeichen")

8.  Gehen Sie auf Einstellungen für einzelne Zeichen:

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Einstellung für einzelne Zeichen")

    ein.  Wählen Sie **SAML aktiviert**.
    
    b.  **Klicken Sie auf.**

9.  Gehen Sie auf SAML einzelnen Einstellungen anmelden:

    ![SAML einzelnen Einstellungen anmelden] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML einzelnen Einstellungen anmelden")

    ein.  Geben Sie in das Textfeld Name den Namen der Konfiguration (z. B.: *SPSSOWAAD\_Test*).
    
    b.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden in Salesforce Sandbox** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    
    c.  Geben Sie im Textfeld **Die Entitäts-Id** **https://test.salesforce.com** ist dies zunächst Salesforce-Sandbox, die Sie zum Verzeichnis hinzufügen. Wenn Sie bereits eine Instanz der Salesforce-Sandbox und für die **Entitäts-ID** -Typ im **Anmelde-URL**hinzugefügt haben in diesem Format werden sollten:`http://company.my.salesforce.com`
    
    d.  Klicken Sie auf **Durchsuchen** , um heruntergeladene Zertifikat laden.
    
    e.  Wählen Sie **Assertion enthält die Verbund-ID aus dem** **SAML Identitätstyp aus**.
    
    f.  **SAML Identität Speicherort**wählen Sie **Identität ist im NameIdentifier-Element der Betreff-Anweisung aus**.
    
    g.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden in Salesforce Sandbox** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .
    
    h.  SFDC unterstützt SAML Abmeldung nicht.  Um dieses Problem zu umgehen, fügen Sie 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' in Textbox **Identität Anbieter Abmelden URL** .
    
    Ich.  Wählen Sie als **Service Provider initiiert Binding anfordern** **HTTP POST**.
    
    j. Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Einmaliges Anmelden konfigurieren")

##<a name="enabling-your-domain"></a>Aktivieren der Domäne
  
In diesem Abschnitt wird davon ausgegangen, dass bereits eine Domäne erstellt haben.  
Weitere Informationen finden Sie unter [Domänennamen definieren](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>So aktivieren Sie Ihre Domäne:

1.  Klicken Sie im linken Navigationsbereich auf **Verwaltung**und dann auf **Meine Domäne.**

    ![Meine Domäne] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Meine Domäne")

    >[AZURE.NOTE]Stellen Sie sicher, dass Ihre Domäne richtig konfiguriert wurde.

2.  Klicken Sie im Abschnitt **Seite Anmeldedaten** auf **Bearbeiten**als **Authentifizierungsdienst**wählen Sie die SAML-auf Einstellung aus dem vorherigen Abschnitt und klicken Sie dann auf **Speichern**.

    ![Meine Domäne] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Meine Domäne")
  
Als Sie eine Domäne konfiguriert haben, sollten die Benutzer Domänen-URL bei der Salesforce-Sandbox verwenden.  
Klicken Sie den Wert der URL SSO-Profil, das Sie im vorherigen Abschnitt erstellt haben.
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Dieser Abschnitt soll aktivieren benutzerbereitstellung von Active Directory-Benutzerkonten Salesforce Sandbox gliedern.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Wählen Sie im Portal Salesforce in der oberen Navigationsleiste Ihren Namen Benutzermenü erweitern:

    ![Meine Einstellungen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Meine Einstellungen")

2.  Wählen Sie Benutzermenü **Einstellungen** auf **Meine** Einstellungen.

3.  Im linken Bereich auf **Persönliche** persönlichen Bereich erweitern und dann auf **Reset My Security Token**:

    ![Meine Einstellungen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Meine Einstellungen")

4.  Klicken Sie auf **Reset My Security Token** auf **Sicherheitstoken zurücksetzen** eine e-Mail anfordern, die die Salesforce.com-Sicherheitstoken.

    ![Neue Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Neue Token")

5.  Posteingang eine e-Mail aus "Salesforce.com" mit "**salesforce.com.com Security Bestätigung**" als Betreff anzeigen

6.  Lesen Sie diese e-Mail und kopieren Sie Sicherheit Tokenwert.

7.  Klicken Sie im klassischen Azure-Portal, auf Integration **Salesforce Sandbox** -Anwendung auf Dialogfeld **Benutzerbereitstellung konfigurieren** **Konfigurieren Benutzer bereitstellen** .

    ![Konfigurieren Benutzer bereitstellen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Konfigurieren Benutzer bereitstellen")

8.  Bieten Sie auf der Seite **Anmeldeinformationen Salesforce Sandbox aktivieren automatisiertes provisioning** folgende Einstellungen:

    ![Salesforce-Sandbox] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce-Sandbox")

    ein.  Geben Sie im Textfeld **Salesforce Sandbox Admin-Benutzernamen** Salesforce Sandbox Konto ein, der **System Administrator** -Profil in "Salesforce.com" zugewiesen wurde.

    b.  Geben Sie im Textfeld **Salesforce Sandbox Admin-Kennwort** das Kennwort für dieses Konto.

    c.  Fügen Sie im Textfeld **Benutzer Sicherheitstoken** token Sicherheitswert.

    d.  Klicken Sie auf **Überprüfen** der Konfiguration.

    e.  Klicken Sie auf **Weiter** , um **die Bestätigungsseite** zu öffnen.

9.  Klicken Sie auf **der Bestätigungsseite** auf **vollständig** , um die Konfiguration zu speichern.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Um Salesforce Sandbox Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Salesforce Sandbox **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")
  
Sie sollten jetzt 10 Minuten warten und überprüfen, ob das Konto Salesforce Sandbox synchronisiert wurde.
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](https://msdn.microsoft.com/library/dn308586)
