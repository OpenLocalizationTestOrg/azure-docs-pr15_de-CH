<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Kintone | Microsoft Azure" 
    description="Erfahren Sie, wie mit Kintone in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Lernprogramm: Azure Active Directory-Integration mit Kintone
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Kintone anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Kintone einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Kintone Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Kintone zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Kintone
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Szenario")
##<a name="enabling-the-application-integration-for-kintone"></a>Die Application Integration für Kintone
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Kintone aktivieren.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Kintone:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Kintone**.

    ![Anwendung Galerie] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Kintone aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Kintone mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Kintone** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Kintone** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Kintone** den URL dem folgenden Muster "*https://company.kintone.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Kintone** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens **Kintone** .

6.  **Klicken Sie auf.**

    ![Standardeinstellungen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Standardeinstellungen")

7.  Klicken Sie auf **Benutzer und Systemadministration**.

    ![Benutzer & Systemadministration] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Benutzer & Systemadministration")

8.  Unter **System Administration \> Security** klicken Sie auf **Anmelden**.

    ![Anmeldung] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Anmeldung")

9.  Klicken Sie auf **Aktivieren SAML-Authentifizierung**.

    ![SAML-Authentifizierung] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML-Authentifizierung")

10. Im Abschnitt SAML-Authentifizierung die folgenden Schritte:

    ![SAML-Authentifizierung] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML-Authentifizierung")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Kintone** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Kintone** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    3.  Klicken Sie auf **Durchsuchen** , um die heruntergeladene Zertifikat laden.
    4.  Klicken Sie auf **Speichern**.

11. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Kintone anmelden können, müssen sie in Kintone bereitgestellt werden.  
Bei Kintone ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **Kintone** Unternehmen als Administrator an.

2.  **Klicken Sie auf.**

    ![Standardeinstellungen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Standardeinstellungen")

3.  Klicken Sie auf **Benutzer und Systemadministration**.

    ![Benutzer & Systemadministration] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Benutzer & Systemadministration")

4.  Klicken Sie unter **Benutzer-Administration** **Abteilungen und Benutzer**.

    ![Abteilung & Benutzer] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Abteilung & Benutzer")

5.  Klicken Sie auf **Neuer Benutzer**.

    ![Neue Benutzer] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Neue Benutzer")

6.  Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:

    ![Neue Benutzer] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Neue Benutzer")

    1.  Geben Sie einen **Anzeigenamen**, **Benutzername**, **Passwort**, **Kennwort bestätigen**, **E-Mail-Adresse** und andere Details eines gültigen Kontos AAD Bestimmung in den zugehörigen Texboxes möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen Kintone Benutzer Konto Erstellungstools oder APIs von Kintone Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Kintone:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Kintone **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)