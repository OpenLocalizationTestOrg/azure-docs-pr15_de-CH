<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit ThousandEyes | Microsoft Azure" 
    description="Erfahren Sie, wie mit ThousandEyes in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Lernprogramm: Azure Active Directory-Integration mit ThousandEyes
  
Das Ziel dieses Lernprogramms ist einrichten einmaliges Anmelden zwischen Azure Active Directory (Azure AD) und ThousandEyes anzeigen.
  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein ThousandEyes für einmaliges Anmelden aktiviert Abonnement
  
Nach diesem Lernprogramm werden der AAD Benutzer, zuweisen, ThousandEyes Zugriff auf, einzelne Zeichen in der Anwendung auf der ThousandEyes Website (Service Provider initiiert anmelden) oder mit der AAD Zugriff können.

1.  Die Application Integration für ThousandEyes
2.  Konfigurieren von Single Sign-On
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-thousandeyes-tutorial/IC790059.png "Szenario")

##<a name="enabling-the-application-integration-for-thousandeyes"></a>Die Application Integration für ThousandEyes
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für ThousandEyes aktivieren.

###<a name="to-enable-the-application-integration-for-thousandeyes-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ThousandEyes:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **ThousandEyes**.

    ![Anwendung Galerie] (./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **ThousandEyes aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ThousandEyes] (./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

##<a name="configuring-single-sign-on"></a>Konfigurieren von Single Sign-On
  
Dieser Abschnitt beschreibt, wie Benutzer ThousandEyes mit seinem Konto in Azure Active Directory Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **ThousandEyes** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "Gesamtauthentifizierung konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ThousandEyes** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "Gesamtauthentifizierung konfigurieren")

3.  Auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL ThousandEyes** URL-Benutzer mit der Anwendung ThousandEyes anmelden Typ (z. B.: "*https://app.thousandeyes.com/login/sso*"), und klicken Sie dann auf **Weiter**. 

    ![Konfigurieren von App-URL] (./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden unter ThousandEyes** das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Datei lokal auf dem Computer.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "Gesamtauthentifizierung konfigurieren")

5.  In verschiedenen Webbrowserfenster melden Sie sich auf der Website Ihres Unternehmens **ThousandEyes** als Administrator.

6.  Klicken Sie im Menü oben auf **Settings**.

    ![Standardeinstellungen] (./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Standardeinstellungen")

7.  Klicken Sie auf **Konto**

    ![Konto] (./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Konto")

8.  Klicken Sie auf die Registerkarte **Sicherheit und Authentifizierung** .

    ![Sicherheit und Authentifizierung] (./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "Sicherheit und Authentifizierung")

9.  Im Abschnitt **Installation einmaliges** Schritte:

    ![Einmaliges Setup] (./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "Einmaliges Setup")

    1.  Aktivieren Sie **Einmaliges Anmelden**.
    2.  Kopieren Sie im klassischen Microsoft Azure-Portal auf der Seite **Konfigurieren einmaliges Anmelden am ThousandEyes** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **URL-Adresse** .
    3.  Kopieren Sie im klassischen Microsoft Azure-Portal auf der Seite **Konfigurieren einmaliges Anmelden am ThousandEyes** Wert **Remote Logout-URL** und fügen Sie ihn in das Textfeld **URL der Abmeldung** .
    4.  Kopieren Sie im klassischen Microsoft Azure-Portal auf der Seite **Konfigurieren einmaliges Anmelden am ThousandEyes** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Identität Anbieter Aussteller** .
    5.  **Identitätszertifikat Anbieter**auf **Datei auswählen**und uploaden Sie das Zertifikat aus dem klassischen Microsoft Azure-Portal herunterladen.
    6.  Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "Gesamtauthentifizierung konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD ThousandEyes anmelden können, müssen sie in ThousandEyes bereitgestellt werden.  
Bei ThousandEyes ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-account-to-thousandeyes-perform-the-following-steps"></a>Um ein Benutzerkonto ThousandEyes bereitzustellen, führen Sie die folgenden Schritte:

1.  ThousandEyes der Website Ihres Unternehmens als Administrator anmelden.

2.  **Klicken Sie auf.**

    ![Standardeinstellungen] (./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Standardeinstellungen")

3.  Klicken Sie auf **Konto**.

    ![Konto] (./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Konto")

4.  Klicken Sie auf der Registerkarte **Konten und Benutzer** .

    ![Konten und Benutzer] (./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Konten und Benutzer")

5.  Im Abschnitt **Benutzer hinzufügen und Konten** Schritte:

    ![Hinzufügen von Benutzerkonten] (./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Hinzufügen von Benutzerkonten")

    1.  Geben Sie **Namen**, **E-Mail** und andere Details eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **neue Benutzer Konto**.

        >[AZURE.NOTE] Kontoinhaber AAD erhalten eine e-Mail mit einem Link zu bestätigen und das Konto zu aktivieren.

>[AZURE.NOTE] Können Sie alle anderen ThousandEyes Benutzer Konto Erstellungstools oder APIs von ThousandEyes Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-thousandeyes-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu ThousandEyes:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **ThousandEyes** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
