<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Replicon | Microsoft Azure" 
    description="Erfahren Sie, wie mit Replicon in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-replicon"></a>Lernprogramm: Azure Active Directory-Integration mit Replicon
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Replicon anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Replicon Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Replicon Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Replicon zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Replicon
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-replicon-tutorial/IC777798.png "Szenario")
##<a name="enabling-the-application-integration-for-replicon"></a>Die Application Integration für Replicon
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Replicon aktivieren.

###<a name="to-enable-the-application-integration-for-replicon-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Replicon:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-replicon-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-replicon-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-replicon-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Replicon**.

    ![Anwendung Galerie] (./media/active-directory-saas-replicon-tutorial/IC777799.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Replicon aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Replicon] (./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Replicon mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Replicon** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-replicon-tutorial/IC777801.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Replicon** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-replicon-tutorial/IC777802.png "Einmaliges Anmelden für konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von app-URL] (./media/active-directory-saas-replicon-tutorial/IC777803.png "Konfigurieren von app-URL")

    1.  Geben Sie im Textfeld **Anmelde-URL Replicon** den Replicon Mieter URL (z. B.: *https://na2.replicon.com/company/saml2/sp-sso/post*).
    2.  Geben Sie im Textfeld **Replicon Antwort-URL** den URL Replicon **AssertionConsumerService** (z.B.: *https://global.replicon.com/! / saml2/Firma/Sso/Post*).  

        >[AZURE.NOTE] Abrufen des URLs aus den Metadaten Replicon an:         **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.

    3.  Klicken Sie auf **Weiter**

4.  Auf der Seite **Konfigurieren einmaliges Anmelden unter Replicon** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten auf Ihrem Computer.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-replicon-tutorial/IC777804.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Replicon.

6.  Konfigurieren Sie SAML 2.0 Schritte:

    ![Aktivieren von SAML-Authentifizierung] (./media/active-directory-saas-replicon-tutorial/IC777805.png "Aktivieren von SAML-Authentifizierung")

    1.  Um das Dialogfeld **EnableSAML Authentication2** Anfügen an folgenden Ihre URL Schlüssel des Unternehmens:  
        **/Services/SecurityService1.svc/Help/Test/EnableSAMLAuthentication2**  
        Im folgenden wird das Schema der vollständige URL:  
        **https://Na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
    2.  Klicken Sie auf die **+** **v20Configuration** Abschnitt erweitern.
    3.  Klicken Sie auf die **+** **MetaDataConfiguration** Abschnitt erweitern.
    4.  Klicken Sie auf **Datei auswählen**, um Ihre Identität Anbieter XML-Metadatendatei auswählen, und klicken Sie auf **Senden**.

7.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-replicon-tutorial/IC778418.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Replicon anmelden können, müssen sie in Replicon bereitgestellt werden.  
Bei Replicon ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie in einem Webbrowserfenster als Administrator der Website Ihres Unternehmens Replicon.

2.  Gehen Sie zu **Administration \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-replicon-tutorial/IC777806.png "Benutzer")

3.  Klicken Sie auf **+ Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-replicon-tutorial/IC777807.png "Benutzer hinzufügen")

4.  Führen Sie im Abschnitt **Benutzerprofil** die folgenden Schritte aus:

    ![Benutzerprofil] (./media/active-directory-saas-replicon-tutorial/IC777808.png "Benutzerprofil")

    1.  Geben Sie im Textfeld **Benutzername** Azure AD e-Mail-Adresse des Azure AD-Benutzer, den Sie bereitstellen möchten.
    2.  Wählen Sie als **Authentifizierungstyp** **SSO**.
    3.  Geben Sie im Textfeld **Abteilung** Abteilung des Benutzers.
    4.  Wählen Sie als **Mitarbeitertyp** **Administrator**.
    5.  Klicken Sie auf **Profil**.

>[AZURE.NOTE]Können Sie alle anderen Replicon Benutzer Konto Erstellungstools oder APIs von Replicon Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-replicon-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Replicon:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Replicon **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-replicon-tutorial/IC777809.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)