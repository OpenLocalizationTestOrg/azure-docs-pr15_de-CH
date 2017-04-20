<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit UserVoice | Microsoft Azure" 
    description="Erfahren Sie, wie mit UserVoice Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Lernprogramm: Azure Active Directory-Integration mit UserVoice
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und UserVoice anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   UserVoice Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der UserVoice Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie UserVoice zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für UserVoice
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Szenario")

##<a name="enabling-the-application-integration-for-uservoice"></a>Die Application Integration für UserVoice
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration UserVoice aktivieren.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für UserVoice:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **UserVoice**.

    ![Anwendung Galerie] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **UserVoice aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![UserVoice] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer UserVoice Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für UserVoice erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **UserVoice** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei UserVoice** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **UserVoice Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. UserVoice.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am UserVoice** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\UserVoice.cer**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens UserVoice.

6.  Klicken Sie in der oberen Symbolleiste und wählen Sie im Menü Web-Portal.

    ![Standardeinstellungen] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Standardeinstellungen")

7.  Klicken Sie auf der Registerkarte **Web-Portal** im Abschnitt **Benutzerauthentifizierung** auf **Bearbeiten** , um die Dialogseite **Benutzerauthentifizierung bearbeiten** öffnen

    ![Web-portal] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web-portal")

8.  Führen Sie auf der **Benutzerauthentifizierung bearbeiten** die folgenden Schritte:

    ![Benutzer bearbeiten] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Benutzer bearbeiten")

    1.  Klicken Sie auf **Single Sign-On (SSO)**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am UserVoice** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **SSO-Remote-Anmeldung** .
    3.  Im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am UserVoice** **Remote Logout URL** -Wert kopieren, und dann **Remote Abmelde SSO-Textfeld**einfügen.
    4.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **aktuelle Zertifikat SHA1 Fingerabdruck** .  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    5.  Klicken Sie auf **Authentifizierung**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer UserVoice anmelden können, müssen sie in UserVoice bereitgestellt werden.  
Bei UserVoice ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **UserVoice** .

2.  Gehe zu **Einstellungen**.

    ![Standardeinstellungen] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Standardeinstellungen")

3.  Klicken Sie auf **Allgemein**.

4.  Klicken Sie auf **Berechtigungen und Agents**.

    ![Agents und Berechtigungen] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agents und Berechtigungen")

5.  Klicken Sie auf **Administratoren hinzufügen**.

    ![Administratoren hinzufügen] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Administratoren hinzufügen")

6.  Gehen Sie im Dialogfeld **Einladung Administratoren** :

    ![Administratoren laden] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Administratoren laden")

    1.  Geben Sie in die TextBox E-Mails die e-Mail-Adresse des Kontos bereitstellen möchten, und klicken Sie dann auf **Hinzufügen**.
    2.  Klicken Sie auf **Laden**.

>[AZURE.NOTE] Können Sie alle anderen UserVoice Benutzer Konto Erstellungstools oder APIs von UserVoice Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>UserVoice Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **UserVoice **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)