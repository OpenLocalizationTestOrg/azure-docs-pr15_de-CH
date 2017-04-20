<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Bime | Microsoft Azure" 
    description="Erfahren Sie, wie mit Bime in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Lernprogramm: Azure Active Directory-Integration mit Bime

Das Ziel dieses Lernprogramms ist die Integration von Azure und Bime anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Bime Mieter

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Bime Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Bime zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Bime
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bime-tutorial/IC775552.png "Szenario")
##<a name="enabling-the-application-integration-for-bime"></a>Die Application Integration für Bime

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Bime aktivieren.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Bime:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bime-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-bime-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-bime-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Bime**.

    ![Anwendung Galerie] (./media/active-directory-saas-bime-tutorial/IC775553.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Bime aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Bime mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Bime erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Bime** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bime-tutorial/IC771709.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Bime** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bime-tutorial/IC775555.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Bime Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Bimeapp.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-bime-tutorial/IC775556.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Bime** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\Bime.cer**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bime-tutorial/IC775557.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Bime.

6.  Klicken Sie in der Symbolleiste auf **Admin**und **Konto**.

    ![Admin] (./media/active-directory-saas-bime-tutorial/IC775558.png "Admin")

7.  Führen Sie auf der Konfigurationsseite die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bime-tutorial/IC775559.png "Einmaliges Anmelden konfigurieren")

    1.  Wählen Sie **Aktivieren SAML-Authentifizierung**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Bime** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Remote Login-URL** .
    3.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** .  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    4.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-bime-tutorial/IC775560.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Bime anmelden können, müssen sie in Bime bereitgestellt werden.  
Bei Bime ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich bei Ihrem Mandanten **Bime** .

2.  Klicken Sie in der Symbolleiste auf **Administrator**und **Benutzer**.

    ![Admin] (./media/active-directory-saas-bime-tutorial/IC775561.png "Admin")

3.  Klicken Sie in der **Liste Benutzer**auf **Neuen Benutzer hinzufügen** ("+").

    ![Benutzer] (./media/active-directory-saas-bime-tutorial/IC775562.png "Benutzer")

4.  Klicken Sie auf Dialogfeld **Benutzerangaben** Schritte:

    ![Benutzerinformationen] (./media/active-directory-saas-bime-tutorial/IC775563.png "Benutzerinformationen")

    1.  Geben Sie Vorname, Nachname, Benutzername, E-Mail eines gültigen Kontos AAD bereitstellen möchten.
    2.  Klicken Sie auf Speichern.

>[AZURE.NOTE] Können Sie alle anderen Bime Benutzer Konto Erstellungstools oder APIs von Bime Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Bime:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Bime **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-bime-tutorial/IC775564.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bime-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
