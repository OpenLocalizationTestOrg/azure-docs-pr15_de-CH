<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Jobscience | Microsoft Azure" 
    description="Erfahren Sie, wie mit Jobscience in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Lernprogramm: Azure Active Directory-Integration mit Jobscience
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Jobscience anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Abonnement Jobscience einmaliges Anmelden aktiviert
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Jobscience Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Jobscience zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Jobscience
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Szenario")
##<a name="enabling-the-application-integration-for-jobscience"></a>Die Application Integration für Jobscience
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Jobscience aktivieren.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Jobscience:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Jobscience**.

    ![Anwendung Galerie] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Jobscience aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Jobscience mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Jobscience erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Melden Sie sich Ihre Site Jobscience Unternehmen als Administrator an.

2.  Fahren Sie mit **Setup**.

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domain Management** den zugehörigen Abschnitt erweitern und klicken Sie auf **Meine Domäne** die **Eigene** Seite öffnen. 

    ![Meine Domäne] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Meine Domäne")

4.  Um sicherzustellen, dass Ihre Domäne richtig installiert wurde, stellen Sie sicher, dass sie in "**Schritt 4 für Benutzer bereitgestellt**" und überprüfen Sie "**Meine Einstellungen**".

    ![Ihre Benutzer bereitgestellt] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Ihre Benutzer bereitgestellt")

5.  Melden Sie sich in verschiedenen Webbrowserfenster der Azure-Verwaltungsportal an.

6.  Klicken Sie auf der Seite Application Integration **Jobscience** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Einmaliges Anmelden konfigurieren")

7.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Jobscience** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Einmaliges Anmelden konfigurieren")

8.  Geben Sie auf der Seite **Konfigurieren App URL** in das Textfeld **Jobscience Zeichen In URL** den URL dem folgenden Muster "*http://company.my.salesforce.com*" und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Konfigurieren von App-URL")

9.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Jobscience** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Einmaliges Anmelden konfigurieren")

10. Auf der Website Jobscience Unternehmen auf **Sicherheit**und dann auf **Einstellungen für einzelne Zeichen**.

    ![Sicherheitskontrollen] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Sicherheitskontrollen")

11. Im Abschnitt **Einstellungen für einzelne Zeichen** folgendermaßen:

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Einstellung für einzelne Zeichen")

    1.  Wählen Sie **SAML aktiviert**.
    2.  **Klicken Sie auf.**

12. Gehen Sie im Dialogfeld **SAML Single Sign-On Einstellung bearbeiten** :

    ![SAML - auf Einstellung] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML - auf Einstellung")

    1.  Geben Sie in das Textfeld **Name** einen Namen für Ihre Konfiguration.
    2.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am Jobscience** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller**
    3.  Geben Sie im Textfeld **Die Entitäts-Id** **https://salesforce-jobscience.com**
    4.  Klicken Sie auf **Durchsuchen** , um Ihre Azure AD-Zertifikat laden.
    5.  Wählen Sie **Assertion enthält die Verbund-ID aus dem** **SAML Identitätstyp aus**.
    6.  **SAML Identität Speicherort**wählen Sie **Identität ist im NameIdentfier-Element der Betreff-Anweisung aus**.
    7.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am Jobscience** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL**
    8.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am Jobscience** **Remote Logout URL** -Wert und fügen Sie ihn in das Textfeld **Identität Anbieter Abmelden URL**
    9.  Klicken Sie auf **Speichern**.

13. Im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domain Management** den zugehörigen Abschnitt erweitern und klicken Sie auf **Meine Domäne** die **Eigene** Seite öffnen. 

    ![Meine Domäne] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Meine Domäne")

14. Klicken Sie auf der Seite **Meine Domäne** im Abschnitt **Login Seite Branding** auf **Bearbeiten**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Branding-Anmeldeseite")

15. Auf der Seite **Login Seite Branding** im Abschnitt **Authentifizierungsdienst** wird der Name **SAML SSO-Einstellungen** angezeigt. Wählen sie und klicken Sie dann auf **Speichern**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Branding-Anmeldeseite")

16. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Einmaliges Anmelden konfigurieren")
  
SP zu klicken Sie auf **Einmalanmeldung Einstellungen** im Menüabschnitt **Sicherheitskontrollen** initiierte Einmalanmeldung Anmelde-URL.

![Sicherheitskontrollen] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Sicherheitskontrollen")
  
Klicken Sie auf SSO-Profil, das Sie im vorigen Schritt erstellt haben.  
Diese Seite zeigt die einmaliges URL für Ihr Unternehmen (z.B. *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Jobscience anmelden können, müssen sie in Jobscience bereitgestellt werden.  
Bei Jobscience ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **Jobscience** Unternehmen als Administrator an.

2.  Konfiguration

    ![Setup] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")

3.  Gehen Sie zu **Benutzer verwalten \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Benutzer")

4.  Klicken Sie auf **Neuer Benutzer**.

    ![Alle Benutzer] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Alle Benutzer")

5.  Dialogfeld " **Benutzer bearbeiten** " die folgenden Schritte:

    ![Benutzer bearbeiten] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Benutzer bearbeiten")

    1.  Geben Sie Vorname, Nachname, Alias, e-Mail, Benutzereigenschaften Name und Spitznamen Azure AD-Benutzerobjekt in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Kontoinhaber Azure AD erhalten eine e-Mail, die enthält einen Link, um das Konto zu bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Jobscience Benutzer Konto Erstellungstools oder APIs von Jobscience Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Jobscience:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Jobscience **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)