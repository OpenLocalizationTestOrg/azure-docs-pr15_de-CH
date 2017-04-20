<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Adobe EchoSign | Microsoft Azure" 
    description="Erfahren Sie, wie mit Adobe EchoSign Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Lernprogramm: Azure Active Directory-Integration mit Adobe EchoSign

Das Ziel dieses Lernprogramms ist die Integration von Azure und Adobe EchoSign anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine einzelne Adobe EchoSign aktivierten Abonnement anmelden

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung in Ihre Adobe EchoSign Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Adobe EchoSign zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Adobe EchoSign aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Szenario")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Anwendungsintegration für Adobe EchoSign aktivieren

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Adobe EchoSign aktivieren.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Adobe EchoSign:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Adobe EchoSign**.

    ![Anwendung Galerie] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Adobe EchoSign aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Adobe EchoSign Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Adobe EchoSign** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Adobe EchoSign** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Adobe EchoSign Anmelde-URL** den URL dem folgenden Muster "*https://company.echosign.com/*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Adobe EchoSign.

6.  Klicken Sie im Menü oben auf **Konto**und, klicken Sie im Navigationsbereich auf der linken klicken Sie **SAML** unter **Account Settings**.

    ![Konto] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Konto")

7.  SAML-Einstellungen folgendermaßen Sie fest:

    ![SAML-Einstellung] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML-Einstellung")

    1.  **SAML-Modus**wählen Sie **SAML obligatorisch**.
    2.  Wählen Sie **EchoSign Konto-Administratoren mit ihrer EchoSign-Anmeldeinformationen anmelden können**.
    3.  **Erstellt**wählen Sie **Benutzerauthentifizierung über SAML automatisch hinzugefügt aus**.

8.  Verschieben Sie, die folgenden Schritte ausführen:

    ![SAML-Einstellung] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML-Einstellung")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** **Entitäts-ID** -Wert und fügen Sie ihn in das Textfeld **IdP Entitäts-ID** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **IdP Anmelde-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Adobe EchoSign** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **IdP Logout URL** .
    4.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie, **IdP Zertifikat** Textbox 6.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Adobe EchoSign anmelden können, müssen sie in Adobe EchoSign bereitgestellt werden.  
Bei Adobe EchoSign ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich der Website Ihres Unternehmens **Adobe EchoSign** als Administrator an.

2.  Klicken Sie auf **Konto**, klicken Sie im Menü oben klicken Sie im Navigationsbereich auf der linken auf **Benutzer und Gruppen**, und klicken Sie auf **neuen Benutzer erstellen**.

    ![Konto] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Konto")

3.  Folgendermaßen Sie im Abschnitt **Erstellen neuer Benutzer** an:

    ![Benutzer erstellen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Benutzer erstellen")

    1.  Geben Sie die **E-Mail-Adresse**, **Vorname** und **Nachname** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer erstellen**.

        >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail, die enthält einen Link, um das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Adobe EchoSign Benutzer Konto Erstellungstools oder APIs von Adobe EchoSign Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Um Adobe EchoSign Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Adobe EchoSign **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
