<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Work.com | Microsoft Azure" 
    description="Erfahren Sie, wie mit Work.com Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Lernprogramm: Azure Active Directory-Integration mit Work.com
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Work.com anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Work.com einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms weisen die AAD Benutzer, Work.com Zugang können einzelne Zeichen in der Anwendung auf der Work.com Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Work.com
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Szenario")

##<a name="enabling-the-application-integration-for-workcom"></a>Die Application Integration für Work.com
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Work.com aktivieren.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Work.com:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Work.com**.

    ![Anwendung Galerie] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Work.com aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Work.com Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Zertifikat auf Work.com hochladen.

>[AZURE.NOTE] Konfigurieren Sie einmaliges Anmelden, müssen Sie einen benutzerdefinierten Domänennamen Work.com noch einrichten. Sie müssen mindestens einen Domänennamen definieren, testen und in der gesamten Organisation bereitstellen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Melden Sie sich bei Ihrem Mandanten Work.com als Administrator an.

2.  Fahren Sie mit **Setup**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domain Management** den zugehörigen Abschnitt erweitern und klicken Sie auf **Meine Domäne** die **Eigene** Seite öffnen. 

    ![Meine Domäne] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Meine Domäne")

4.  Um sicherzustellen, dass Ihre Domäne richtig installiert wurde, stellen Sie sicher, dass sie in "**Schritt 4 für Benutzer bereitgestellt**" und überprüfen Sie "**Meine Einstellungen**".

    ![Ihre Benutzer bereitgestellt] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Ihre Benutzer bereitgestellt")

5.  Melden Sie sich in verschiedenen Webbrowserfenster der Azure-Verwaltungsportal an.

6.  Klicken Sie auf der Seite Application Integration **Work.com **auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Einmaliges Anmelden konfigurieren")

7.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Work.com** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Einmaliges Anmelden konfigurieren")

8.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Work.com** URL zur Work.com Anwendung anmelden von Benutzern verwendet (z. B.: " *http://company.my.salesforce.com*"), und klicken Sie dann auf **Weiter**: 

    ![Konfigurieren von App-URL] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Konfigurieren von App-URL")

9.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Work.com** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Einmaliges Anmelden konfigurieren")

10. Melden Sie sich bei Ihrem Mandanten Work.com.

11. Fahren Sie mit **Setup**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. Erweitern Sie im Menü **Sicherheit** und klicken Sie **Single Sign-On**.

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Einstellung für einzelne Zeichen")

13. Führen Sie auf der **Einstellungen für einzelne Zeichen** die folgenden Schritte:

    ![SAML aktiviert] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML aktiviert")

    1.  Wählen Sie **SAML aktiviert**.
    2.  **Klicken Sie auf.**

14. Im Abschnitt **SAML Single Sign-On Settings** die folgenden Schritte:

    ![SAML - auf Einstellung] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML - auf Einstellung")

    1.  Geben Sie in das Textfeld **Name** einen Namen für Ihre Konfiguration.  

        >[AZURE.NOTE] Mehrwert für **Name** im Textfeld **API-Name** automatisch ausgefüllt.

    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Work.com** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    3.  Klicken Sie auf **Durchsuchen**, um die heruntergeladene Zertifikat laden.
    4.  Geben Sie im Textfeld **Die Entitäts-Id** **https://salesforce-work.com**.
    5.  Wählen Sie **Assertion enthält die Verbund-ID aus dem** **SAML Identitätstyp aus**.
    6.  **SAML Identität Speicherort**wählen Sie **Identität ist im NameIdentfier-Element der Betreff-Anweisung aus**.
    7.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Work.com** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .
    8.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Work.com** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieter Abmelden URL** .
    9.  Wählen Sie als **Service Provider initiiert Binding anfordern** **HTTP Post**.
    10. Klicken Sie auf **Speichern**.

15. In Ihrem Work.com Verwaltungsportal im linken Navigationsbereich auf **Verwaltung** den zugehörigen Abschnitt erweitern und dann auf **Meine Domäne** die **Eigene** Seite öffnen. 

    ![Meine Domäne] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Meine Domäne")

16. Klicken Sie auf der Seite **Meine Domäne** im Abschnitt **Login Seite Branding** auf **Bearbeiten**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Branding-Anmeldeseite")

17. Auf der Seite **Login Seite Branding** im Abschnitt **Authentifizierungsdienst** wird der Name **SAML SSO-Einstellungen** angezeigt. Wählen sie und klicken Sie dann auf **Speichern**.

    ![Branding-Anmeldeseite] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Branding-Anmeldeseite")

18. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Azure Active Directory Benutzer anmelden müssen sie Work.com bereitgestellt werden.  
Bei Work.com ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich auf der Website Ihres Unternehmens Work.com als Administrator an.

2.  Fahren Sie mit **Setup**.

    ![Setup] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3.  Gehen Sie zu **Benutzer verwalten \> Benutzer**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Verwalten von Benutzern")

4.  Klicken Sie auf **Neuer Benutzer**.

    ![Alle Benutzer] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Alle Benutzer")

5.  Im Abschnitt Benutzer bearbeiten die folgenden Schritte:

    ![Benutzer bearbeiten] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Benutzer bearbeiten")

    1.  Geben Sie den **Nachnamen**, **Alias**, **E-Mail**, Attribute **Username** und **Spitznamen** eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie **Funktion**, **Benutzerlizenz** und **Profil**.
    3.  Klicken Sie auf **Speichern**.  

        >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Work.com Benutzer Konto Erstellungstools oder APIs von Work.com Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Work.com:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite Work.com Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Ja")
  
Sie sollten jetzt 10 Minuten warten und überprüfen, ob das Konto Work.com synchronisiert wurde.
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)