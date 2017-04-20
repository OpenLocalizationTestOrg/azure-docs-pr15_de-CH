<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit TOPdesk - sichere | Microsoft Azure"
    description="Informationen zur Verwendung von TOPdesk - Sicherheit mit Azure Active Directory einmaliges, automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Lernprogramm: Azure Active Directory-Integration mit TOPdesk - Sicherheit
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TOPdesk - Sicherheit angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein TOPdesk - sichere SSO-Abonnement aktiviert
  
Nach diesem Lernprogramm werden können einzelne Zeichen in der Anwendung an die TOPdesk - sichere Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, TOPdesk - sichere zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für TOPdesk - Sicherheit aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Szenario")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Anwendungsintegration für TOPdesk - Sicherheit aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für TOPdesk - Sicherheit aktivieren.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Zum Aktivieren der Anwendungsintegration für TOPdesk - sichern Sie, gehen Sie folgendermaßen vor:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **TOPdesk - Sicherheit**.

    ![Anwendung Galerie] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Anwendung Galerie")

7.  Aktivieren Sie im Ergebnisbereich **TOPdesk - Sicherheit**und dann auf **vollständig** die Anwendung hinzufügen.

    ![TOPdesk - Sicherheit] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Sicherheit")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll wie Sie Benutzer authentifizieren TOPdesk - Gliederung schützen Konto in Azure AD Zusammenschlüsse basierend auf SAML-Protokoll.  
Konfigurieren von SSO für TOPdesk - Sicherheit erfordert eine Symboldatei Logo hochladen. Um die Symboldatei zu erhalten, wenden Sie die TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Melden Sie sich auf der Website Ihres Unternehmens **TOPdesk - Sicherheit** als Administrator.

2.  **Klicken Sie im Menü **TOPdesk** klicken.**

    ![Standardeinstellungen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Standardeinstellungen")

3.  Klicken Sie auf **Anmeldung**.

    ![Anmeldedaten] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Anmeldedaten")

4.  Erweitern Sie das Menü **Anmeldedaten** und klicken Sie dann auf **Allgemein**.

    ![Allgemein] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allgemein")

5.  Im Abschnitt **sichere** **Anmeldung SAML** -Konfigurationsabschnitt Schritte:

    ![Technische Eigenschaften] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technische Eigenschaften")

    1.  Klicken Sie auf herunterladen öffentlicher Metadaten-Datei **herunterladen** und dann lokal auf Ihrem Computer speichern.
    2.  Öffnen Sie Metadaten-Datei, und suchen Sie den Knoten **AssertionConsumerService** .
        ![Assertion-Kundendienst] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion-Kundendienst")
    3.  Kopieren Sie den Wert **AssertionConsumerService** .  

        >[AZURE.NOTE] Sie benötigen den Wert unter **App-URL konfigurieren** später in diesem Lernprogramm.

6.  Melden Sie in anderen Webbrowserfenster als Administrator Ihre **Azure-Verwaltungsportal** .

7.  Klicken Sie auf der Seite Application Integration **TOPdesk - Sicherheit** **Konfigurieren einmaliges** öffnen das Dialogfeld **Konfigurieren Sie einmaliges Anmelden** .

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Einmaliges Anmelden konfigurieren")

8.  Auf der Seite **Wie möchten Sie Benutzer TOPdesk - sichere Anmeldung** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Einmaliges Anmelden konfigurieren")

9.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **TOPdesk - Secure Anmelde-URL** den URL der TOPdesk - sichere Anwendung anmelden von Benutzern verwendet (z. B.: "*https://qssolutions.topdesk.net*").
    2.  Fügen Sie im Textfeld **TOPdesk – öffentliche Antwort-URL** **TOPdesk - sichere AssertionConsumerService URL** (z. B.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klicken Sie auf **Weiter**.

10. Auf der Seite **Konfigurieren einmaliges Anmelden am TOPdesk - Sicherheit** zum Laden der Metadatendatei auf **Metadaten**und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Einmaliges Anmelden konfigurieren")

11. Erstellen Sie eine Zertifikatsdatei Schritte:

    ![Zertifikat] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Zertifikat")

    1.  Öffnen Sie die heruntergeladene Metadatendatei.
    2.  Erweitern Sie den Knoten **RoleDescriptor** , die eine **xsi: Type** der **eingezogen: ApplicationServiceType**.
    3.  Kopieren Sie den Wert der **X509Certificate** -Knoten.
    4.  Speichern Sie kopierten **X509Certificate** Wert lokal auf Ihrem Computer in einer Datei.

12. **Auf die TOPdesk - sichere Unternehmens-Website im Menü **TOPdesk** klicken.**

    ![Standardeinstellungen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Standardeinstellungen")

13. Klicken Sie auf **Anmeldung**.

    ![Anmeldedaten] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Anmeldedaten")

14. Erweitern Sie das Menü **Anmeldedaten** und klicken Sie dann auf **Allgemein**.

    ![Allgemein] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Allgemein")

15. Klicken Sie im **öffentlichen** Abschnitt auf **Hinzufügen**.

    ![Hinzufügen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Hinzufügen")

16. Auf der Dialogseite **SAML-Konfigurationsassistenten** Schritte:

    ![SAML-Konfigurations-Assistent] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML-Konfigurations-Assistent")

    1.  Hochladen der heruntergeladenen Metadatendatei unter **Verbundmetadaten**, klicken Sie auf **Durchsuchen**.
    2.  Hochladen der Zertifikatsdatei unter **Zertifikat (RSA)**, klicken Sie auf **Durchsuchen**.
    3.  Klicken Sie zum Hochladen der Logodatei bekam von TOPdesk Support-Team unter **Logo-Symbols**auf **Durchsuchen**.
    4.  Geben Sie im Textfeld **Benutzer Name-Attribut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Geben Sie im Textfeld **Angezeigter Name** einen Namen für Ihre Konfiguration.
    6.  Klicken Sie auf **Speichern**.

17. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer TOPdesk - Anmeldung aktivieren müssen sicher, sie in TOPdesk - Sicherheit bereitgestellt werden.  
Bei TOPdesk - ist sicher, Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich auf der Website Ihres Unternehmens **TOPdesk - Sicherheit** als Administrator an.

2.  Klicken Sie im Menü oben auf **TOPdesk \> neu \> Dateien \> Operator**.

    ![Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")

3.  Im Dialogfeld " **Neuer Operator** " die folgenden Schritte:

    ![New-Operator] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New-Operator")

    1.  Klicken Sie auf die Registerkarte Allgemein.
    2.  Geben Sie im Textfeld **Name** den Abschnitt **Allgemein** den Nachnamen ein gültiges Azure Active Directory-Konto, das Sie bereitstellen möchten.
    3.  Wählen Sie eine **Site** für das Konto im Abschnitt **Speicherort** .
    4.  Geben Sie im Textfeld **Benutzername** des Abschnitts **TOPdesk Login** einen Benutzernamen für den Benutzer.
    5.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen TOPdesk - sicheren Account Creation Tools oder TOPdesk - sichere Bereitstellung AAD Benutzerkonten-APIs können.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>TOPdesk - Benutzer zuweisen sichern Sie, gehen Sie folgendermaßen vor:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **TOPdesk - Sicherheit **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)