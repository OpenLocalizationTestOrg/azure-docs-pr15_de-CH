<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration IdeaScale | Microsoft Azure" 
    description="Erfahren Sie, wie mit IdeaScale Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Lernprogramm: Azure Active Directory-Integration mit IdeaScale
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und IdeaScale anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein IdeaScale einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können sich mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Anwendung für einmaliges Azure AD-Benutzer, die Sie IdeaScale zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für IdeaScale
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Szenario")
##<a name="enabling-the-application-integration-for-ideascale"></a>Die Application Integration für IdeaScale
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration IdeaScale aktivieren.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für IdeaScale:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **IdeaScale**.

    ![Anwendung Galerie] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **IdeaScale aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer IdeaScale Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für IdeaScale erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **IdeaScale** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei IdeaScale** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL IdeaScale** URL zur IdeaScale Anwendung anmelden von Benutzern verwendet (z. B.: "*https://company.IdeaScale.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am IdeaScale** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens IdeaScale.

6.  **Community**-Einstellungen wechseln

    ![Community-Einstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community-Einstellungen")

7.  Klicken Sie auf **Security \> Anmeldung Einstellungen einzelner**.

    ![Gesamtauthentifizierung Einstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Gesamtauthentifizierung Einstellungen")

8.  Wählen Sie als **Typ für einmaliges Anmelden** **SAML 2.0**.

    ![Anmeldung Typ] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Anmeldung Typ")

9.  In den **Einzelnen Anmeldung Einstellungen** die folgenden Schritte:

    ![Gesamtauthentifizierung Einstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Gesamtauthentifizierung Einstellungen")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am IdeaScale** **Entitäts-ID** -Wert und fügen Sie ihn in das Textfeld **SAML IdP Entitäts-ID** .
    2.  Kopieren Sie den Inhalt der heruntergeladenen Metadatendatei, und fügen Sie ihn in das Textfeld **SAML IdP Metadaten** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am IdeaScale** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **Logout Erfolg URL** .
    4.  Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer IdeaScale anmelden können, müssen sie in IdeaScale bereitgestellt werden.  
Bei IdeaScale ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **IdeaScale** Unternehmen als Administrator an.

2.  **Community**-Einstellungen wechseln

    ![Community-Einstellungen] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community-Einstellungen")

3.  Gehen Sie zu **Standardeinstellungen \> Member Management**.

4.  Klicken Sie auf **Element hinzufügen**.

    ![Member Management] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Member Management")

5.  Im Abschnitt neues Mitglied hinzufügen die folgenden Schritte:

    ![Neues Mitglied hinzufügen] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Neues Mitglied hinzufügen")

    1.  Geben Sie in das Textfeld **E-Mail-Adresse** die e-Mail-Adresse eines gültigen Kontos AAD bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail mit einem Link zu das Konto bestätigen aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen IdeaScale Benutzer Konto Erstellungstools oder APIs von IdeaScale Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Um IdeaScale Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **IdeaScale **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)