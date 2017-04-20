<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Concur | Microsoft Azure" 
    description="Erfahren Sie, wie mit Concur Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Lernprogramm: Azure Active Directory-Integration mit Concur  


Das Ziel dieses Lernprogramms ist die Integration von Azure und Concur anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter in Concur

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Concur
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-concur-tutorial/IC769766.png "Szenario")

>[AZURE.NOTE] Die Konfiguration des Abonnements Concur verbundenen SSO über SAML ist separaten Vorgang ausführen Concur kontaktieren müssen.

##<a name="enabling-the-application-integration-for-concur"></a>Die Application Integration für Concur

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Concur aktivieren.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Concur:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  Aus **Verzeichnis** , wählen Sie das Verzeichnis für die Verzeichnisintegration aktivieren möchten.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-concur-tutorial/IC700994.png "Applikationen")

4.  Öffnen Sie die **Anwendung Galerie**auf **Einer Anwendung hinzufügen**und dann auf **eine Anwendung für meine Organisation hinzufügen**.

    ![Was möchten Sie tun?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Was möchten Sie tun?")

5.  Geben Sie im **Suchfeld** **Concur**.

    ![Anwendung Galerie] (./media/active-directory-saas-concur-tutorial/IC721727.png "Anwendung Galerie")

6.  Klicken Sie im Ergebnisbereich wählen Sie **Concur aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Stimme] (./media/active-directory-saas-concur-tutorial/IC721728.png "Stimme")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Concur mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

>[AZURE.NOTE] Die Konfiguration des Abonnements Concur verbundenen SSO über SAML ist separaten Vorgang ausführen Concur kontaktieren müssen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Concur **auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-concur-tutorial/IC769767.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Concur** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-concur-tutorial/IC769768.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **URL Zeichen stimmen** den Concur Mieter anmelden URL, und klicken Sie auf **Weiter**: 

    ![Konfigurieren von URL anmelden] (./media/active-directory-saas-concur-tutorial/IC769769.png "Konfigurieren von URL anmelden")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Concur** Schritte.

    ![Konfigurieren von URL anmelden] (./media/active-directory-saas-concur-tutorial/IC769770.png "Konfigurieren von URL anmelden")

    1.  Auf Metadaten und dann sicher die Datei auf Ihren Computer heruntergeladen.
    2.  Wenden Sie die Concur um SSO für Ihren Mandanten zu konfigurieren.
    3.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.  

    >[AZURE.NOTE] Die Konfiguration des Abonnements Concur verbundenen SSO über SAML ist separaten Vorgang ausführen Concur kontaktieren müssen.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Das Ziel dieses Abschnitts ist vor der Bereitstellung von Active Directory-Benutzerkonten zu Concur aktivieren.

Zum Aktivieren des Diensts, es apps müssen korrekte Einrichtung und Verwendung eines Web Service Admin. Nicht einfach hinzufügen der WS-Administratorrolle Ihre vorhandenen Administratorprofil für T & E administrative Funktionen verwenden.

Stimme Berater muss oder Client Administrator muss ein Webdienstadministrator Profil erstellen und Client Administrator dieses Profil für Web Services Administrator Funktionen (z.B. apps aktivieren). Diese Profile müssen von Client Administrator täglich T & E Admin-Profil getrennt werden (T & E Admin-Profil müssen nicht die Rolle WSAdmin).

Beim Erstellen des Profils für die Anwendung aktivieren Geben Sie Client Administrator in die Felder des Benutzerprofils. Dadurch wird das Profil zuständig. Nachdem die Profile erstellt, muss dieses Profil auf die Schaltfläche "*Aktivieren*" für Partner-Anwendung im Menü Web Services Client einloggen.

Aus folgenden Gründen diese Aktion nicht mit dem Profil erfolgt für normale T & E Verwaltung verwenden.

1.  Der Client muss die "*Ja*" klickt im Dialogfenster, das angezeigt wird, nachdem eine Anwendung aktiviert ist. Dieses erkennt der Client ist für die Anwendung Partner auf ihre Daten zugreifen, so dass Sie oder der Partner auf die Schaltfläche Ja klicken kann.
2.  Wenn ein Client-Administrator die app aktivierten mit T & E Admin Profil das Unternehmen verlässt (wodurch das Profil deaktiviert wird), apps mit Profil nicht funktionieren, solange die Anwendung mit einem anderen aktiven Webdienstadministrator-Profil aktiviert ist aktiviert. Deshalb soll unterschiedliche Webdienstadministrator-Profile erstellen.
3.  Wenn ein Administrator das Unternehmen verlässt, kann Namen Webdienstadministrator-Profil Ersatz-Administrator geändert werden ggf. beeinträchtigt, da das Profil nicht aktivierte Anwendung deaktiviert

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie Ihrem Mandanten **Concur** .

2.  Wählen Sie im Menü **Verwaltung** **Webdienste**.

    ![Concur Mieter] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur Mieter")

3.  Auf der linken Seite im **Web Services** wählen Sie **Partner-Anwendung aktivieren aus**.

    ![Partner-Anmeldung aktivieren] (./media/active-directory-saas-concur-tutorial/IC721730.png "Partner-Anmeldung aktivieren")

4.  Wählen Sie aus **Anwendung aktivieren** **Azure Active Directory aus**und dann auf **Aktivieren**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Klicken Sie auf **Ja,** um das Dialogfeld **Aktion bestätigen** schließen.

    ![Bestätigen] (./media/active-directory-saas-concur-tutorial/IC721732.png "Bestätigen")

6.  Wählen Sie in der Azure-Verwaltungsportal **Concur** Applikationen **Concur** dialogfeldseite öffnen aus.

7.  Klicken Sie auf **Konfigurieren Benutzer bereitstellen**, öffnen Sie die Seite **Bereitstellung der Benutzer konfigurieren** .

8.  Geben Sie den Benutzernamen und das Kennwort für den Administrator Concur, und klicken Sie dann auf **Weiter**.

9.  Zum Abschluss dieser Konfiguration auf der Seite **Bestätigung** klicken Sie auf **abschließen** .

Sie können jetzt erstellen Sie ein Testkonto 10 Minuten warten und überprüfen, ob das Konto mit Concur synchronisiert wurde.
##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Concur Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Concur **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-concur-tutorial/IC769771.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-concur-tutorial/IC767830.png "Ja")

Sie sollten jetzt 10 Minuten warten und überprüfen, ob das Konto mit Concur synchronisiert wurde.

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
