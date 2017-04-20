<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Bonus.ly | Microsoft Azure" 
    description="Erfahren Sie, wie mit Bonus.ly in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Lernprogramm: Azure Active Directory-Integration mit Bonus.ly

Das Ziel dieses Lernprogramms ist die Integration von Azure und Bonus.ly anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Test in Bonus.ly

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Bonus.ly
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Szenario")
##<a name="enabling-the-application-integration-for-bonusly"></a>Die Application Integration für Bonus.ly

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Bonus.ly aktivieren.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Bonus.ly:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Einmaliges Anmelden aktivieren")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Bonus.ly**.

    ![Anwendung Galerie] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Bonus.ly aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Bonus.ly mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Bonus.ly erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Bonus.ly** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Bonus.ly** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Bonus.ly Mieter URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Bonus.ly*", und klicken Sie dann auf **Weiter**: 

    ![Konfigurieren von app-URL] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Konfigurieren von app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Bonus.ly** auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\Bonusly.cer**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie sich in einem anderen Browserfenster Ihrem Mandanten **Bonus.ly** an.

6.  **Klicken Sie in der oberen Symbolleiste**, und wählen Sie die **Integration und apps**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  Wählen Sie unter **Single Sign-On** **SAML**.

8.  Auf der Dialogseite **SAML** Schritte:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Bonus.ly** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **IdP SSO-Ziel-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Bonus.ly** Wert **Aussteller-ID** und fügen Sie ihn in das Textfeld **IdP Aussteller** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Bonus.ly** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **IdP Anmelde-URL** .
    4.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Cert Fingerabdruck** .

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

9.  Klicken Sie auf **Speichern**.

10. Microsoft Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Benutzer Azure AD Bonus.ly anmelden können, müssen sie in Bonus.ly bereitgestellt werden.  
Bei Bonus.ly ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie Ihrem Mandanten Bonus.ly in einem Webbrowserfenster.

2.  **Klicken Sie**

    ![Standardeinstellungen] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Standardeinstellungen")

3.  Klicken Sie auf der Registerkarte **Benutzer und Boni** .

    ![Benutzer und Boni] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Benutzer und Boni")

4.  Klicken Sie auf **Benutzer verwalten**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Verwalten von Benutzern")

5.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Benutzer hinzufügen")

6.  Gehen Sie im Dialogfeld **Benutzer hinzufügen** :

    ![Benutzer hinzufügen] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Benutzer hinzufügen")

    1.  Geben Sie den "**E-Mail** **Vorname** **Nachname**" eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Kontoinhaber AAD erhalten eine e-Mail, die enthält einen Link, um das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Bonus.ly Benutzer Konto Erstellungstools oder APIs von Bonus.ly Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Bonus.ly:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite Bonus.ly Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
