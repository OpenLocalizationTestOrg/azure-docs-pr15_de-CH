<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Adaptive | Microsoft Azure"
    description="Erfahren Sie, wie mit Adaptive Suite Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Lernprogramm: Azure Active Directory-Integration mit Adaptive

Das Ziel dieses Lernprogramms ist die Integration von Azure und Adaptive Suite anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Adaptive Suite Mieter

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Adaptive Suite Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Adaptive Suite zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration für Adaptive Suite
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Szenario")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Aktivieren die Anwendungsintegration für Adaptive Suite

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Adaptive Suite aktivieren.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Adaptive Suite:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Adaptive Suite**.

    ![Anwendung Galerie] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Adaptive Suite aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Adaptive Suite] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptive Suite")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Adaptive Suite mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Adaptive Suite erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Adaptive Suite** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Adaptive Suite** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren Appeinstellungen** im Feld **Antwort-URL** den URL dem folgenden Muster "*Samlsso/Https://login.adaptiveinsights.com:443/RlJFRVRSSUFMMTI3MTE =*", und klicken Sie dann auf **Weiter**.

    >[AZURE.NOTE] Sie können diese Adaptive Suite **SAML SSO-** Seite nutzen.

    ![Anwendung konfigurieren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Anwendung konfigurieren")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Adaptive Suite** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Adaptive Suite.

6.  Fahren Sie mit **Admin**.

    ![Admin] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

7.  Klicken Sie im Abschnitt **Benutzer und Rollen** **Verwalten SAML SSO-Standardeinstellungen**.

    ![SAML SSO verwalten] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "SAML SSO verwalten")

8.  Auf der Einstellungsseite von **SAML SSO-** Schritte:

    ![SAML SSO-Einstellung] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO-Einstellung")

    1.  Geben Sie einen Namen für die Konfiguration im Textfeld **Identität Anbietername** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Adaptive Suite** **Entitäts-ID** -Wert und fügen Sie ihn in das Textfeld **Identitätsanbieter Entitäts-ID** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Adaptive Suite** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **Identitätsanbieter SSO-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Adaptive Suite** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **benutzerdefinierter Logout URL** .
    5.  Um das heruntergeladene Zertifikat hochzuladen, klicken Sie auf **Datei auswählen**.
    6.  **Mitgliedsname SAML**wählen Sie **Adaptive Insights Benutzernamen aus**
    7.  Als **Speicherort für SAML-Id**wählen Sie **Mitgliedsname NameID Thema aus**
    8.  Wählen Sie **SAML NameID Format** **e-Mail-Adresse**.
    9.  Als **SAML aktivieren**wählen Sie **SAML SSO zulassen und direkte Adaptive Insights-Anmeldung aus**.
    10. Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Adaptive Suite anmelden können, müssen sie in Adaptive Suite bereitgestellt werden.  
Bei Adaptive Suite ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich auf der Website Ihres Unternehmens **Adaptive Suite** als Administrator.

2.  Fahren Sie mit **Admin**.

    ![Admin] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")

3.  Klicken Sie im Abschnitt **Benutzer und Rollen** auf **Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Benutzer hinzufügen")

4.  Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:

    ![Senden] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Senden")

    1.  Geben Sie **Name**, **Benutzername**, **E-Mail**, **Kennwort** eines gültigen Azure Active Directory-Benutzers in verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie eine **Rolle**aus.
    3.  Klicken Sie auf **Senden**.

>[AZURE.NOTE] Alle anderen Adaptive Suite Benutzer Konto Erstellungstools verwenden oder APIs von Adaptive Suite Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Um Adaptive Suite Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Adaptive Suite **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
