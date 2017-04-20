<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Freshdesk | Microsoft Azure" 
    description="Erfahren Sie, wie mit Freshdesk Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Lernprogramm: Azure Active Directory-Integration mit Freshdesk
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Freshdesk anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Freshdesk Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Freshdesk Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Freshdesk zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Freshdesk
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Szenario")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Die Application Integration für Freshdesk
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Freshdesk aktivieren.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Freshdesk:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Freshdesk**.

    ![Anwendung Galerie] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Freshdesk aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Freshdesk Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Freshdesk erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Freshdesk** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Freshdesk** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Freshdesk Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Freshdesk.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Freshdesk** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\Freshdesk.cer**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Freshdesk.

6.  Klicken Sie im Menü oben auf **Administration**.

    ![Admin] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")

7.  Klicken Sie auf der Registerkarte **General Settings** auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Sicherheit")

8.  Führen Sie die folgenden Schritte im Bereich **Sicherheit** :

    ![Einmaliges Anmelden] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Einmaliges Anmelden")

    1.  Wählen Sie für **Einzelne anmelden (SSO)** **auf**.
    2.  **SAML-SSO**auswählen.
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Freshdesk** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **SAML Anmelde-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Freshdesk** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Sicherheit Zertifikat Fingerabdruck** .  

        >[AZURE.TIP]Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    6.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Freshdesk anmelden können, müssen sie in Freshdesk bereitgestellt werden.  
Bei Freshdesk ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Freshdesk** .

2.  Klicken Sie im Menü oben auf **Administration**.

    ![Admin] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")

3.  Klicken Sie auf der Registerkarte **General Settings** auf **Agents**.

    ![Agenten] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agenten")

4.  Klicken Sie auf **neue Agent**.

    ![Neuer Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Neuer Agent")

5.  Gehen Sie im Dialogfeld Agent-Informationen:

    ![Agent-Informationen] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent-Informationen")

    1.  Geben Sie den Namen des Azure AD-Kontos, das Sie bereitstellen möchten, im Feld **Vollständiger Name** .
    2.  Geben Sie in das Textfeld **E-Mail** Azure AD e-Mail-Adresse des Azure AD-Kontos, das Sie bereitstellen möchten.
    3.  Geben Sie im Textfeld **Titel** den Titel des Azure AD-Kontos, das Sie bereitstellen möchten.
    4.  Wählen Sie **Agenten-Rolle**und klicken Sie dann auf **zuweisen**.
    5.  Klicken Sie auf **Speichern**.
    
        >[AZURE.NOTE] Kontoinhaber Azure AD erhalten eine e-Mail, die enthält einen Link, um das Konto zu bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Freshdesk Benutzer Konto Erstellungstools oder APIs von Freshdesk Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Freshdesk Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Freshdesk **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)