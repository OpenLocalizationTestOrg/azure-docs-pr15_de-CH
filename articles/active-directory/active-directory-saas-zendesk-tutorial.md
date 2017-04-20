<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Zendesk | Microsoft Azure" 
    description="Erfahren Sie, wie mit Zendesk Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Lernprogramm: Azure Active Directory-Integration mit Zendesk
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Zendesk anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Zendesk Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem Zendesk Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Zendesk zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Zendesk
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Szenario")

##<a name="enabling-the-application-integration-for-zendesk"></a>Die Application Integration für Zendesk
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Zendesk aktivieren.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Zendesk:

1.  Klicken Sie in der Azure-Verwaltungsportal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Zendesk**.

    ![Anwendung Galerie] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Zendesk aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Zendesk Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Zendesk erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im Portal Azure AD auf Anwendungsseite Integration **Zendesk** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Einmaliges Anmelden")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Zendesk** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Einmaliges Anmelden für konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von app-URL] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Konfigurieren von app-URL")
  
    ein. Geben Sie im Textfeld **Zendesk Zeichen In URL** den URL dem folgenden Muster:`https://<tenant-name>.zendesk.com`

    b. Klicken Sie auf **Weiter**.



4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Zendesk** auf **Zertifikat herunterladen**und dann lokal auf Ihrem Compiter gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Zendesk.

6.  Klicken Sie auf **Administration**.

7.  Klicken Sie im linken Navigationsbereich **Klicken Sie**und dann auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Sicherheit")

8.  Klicken Sie auf der Seite **Sicherheit** auf der Registerkarte **Admin und Agents** .

9.  Wählen Sie **Single Sign-On (SSO) und SAML**, und wählen Sie dann **SAML**.

10. Kopieren Sie im Portal Azure AD auf der Seite **Konfigurieren einmaliges Anmelden am Zendesk** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **SAML SSO-URL** .

11. Kopieren Sie im Portal Azure AD auf der Seite **Konfigurieren einmaliges Anmelden am Zendesk** Wert **Remote Logout-URL** und fügen Sie ihn in das Textfeld **Remote Logout-URL** .

    ![Einmaliges Anmelden] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Einmaliges Anmelden")

12. Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** .

    >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

13. Klicken Sie auf **Speichern**.

14. Portal Azure AD wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer **Zendesk**anmelden können, müssen sie in **Zendesk**bereitgestellt werden.  
Bei **Zendesk**ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Um ein Benutzerkonto Zendesk bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Zendesk** .

2.  Wählen Sie die Registerkarte **Kunden** .

3.  Wählen Sie die Registerkarte **Benutzer** , und klicken Sie auf **Hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Benutzer hinzufügen")

4.  Geben Sie die e-Mail-Adresse eines vorhandenen Azure AD-Kontos bereitstellen möchten, und klicken Sie dann auf **Speichern**.

    ![Neuer Benutzer] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Neuer Benutzer")

>[AZURE.NOTE] Können Sie alle anderen Zendesk Benutzer Konto Erstellungstools oder APIs von Zendesk Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Zendesk Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in Azure AD-Portal.

2.  Klicken Sie auf der Seite **Zendesk **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
