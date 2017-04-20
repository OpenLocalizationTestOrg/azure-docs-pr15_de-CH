<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Abintegro | Microsoft Azure" 
    description="Erfahren Sie, wie mit Abintegro in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-abintegro"></a>Lernprogramm: Azure Active Directory-Integration mit Abintegro

Das Ziel dieses Lernprogramms ist die Integration von Azure und Abintegro anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Abintegro SSO-Abonnement aktiviert

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Abintegro Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Abintegro zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Abintegro
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-abintegro-tutorial/IC790076.png "Szenario")
##<a name="enabling-the-application-integration-for-abintegro"></a>Die Application Integration für Abintegro

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Abintegro aktivieren.

###<a name="to-enable-the-application-integration-for-abintegro-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Abintegro:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-abintegro-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-abintegro-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-abintegro-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-abintegro-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Abintegro**.

    ![Anwendung Galerie] (./media/active-directory-saas-abintegro-tutorial/IC790077.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Abintegro aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Abintegro] (./media/active-directory-saas-abintegro-tutorial/IC790078.png "Abintegro")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Abintegro mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Abintegro** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-abintegro-tutorial/IC790079.png "Gesamtauthentifizierung konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Abintegro** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-abintegro-tutorial/IC790080.png "Gesamtauthentifizierung konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Abintegro** URL Abintegro Anmelden von Benutzern verwendet (z.B.: `https://dev.abintegro.com/Shibboleth.sso/Login?entityID=<Issuer>&target=https://dev.abintegro.com/secure/`), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-abintegro-tutorial/IC790081.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Abintegro** auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-abintegro-tutorial/IC790082.png "Gesamtauthentifizierung konfigurieren")

5.  Senden Sie die Metadatafile zu Abintegro.

    >[AZURE.NOTE] Die Konfiguration für einzelne Zeichen muss Abintegro Support Team ausgeführt werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-abintegro-tutorial/IC790083.png "Gesamtauthentifizierung konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Gibt es keine Aufgabe Benutzer Abintegro Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer die Abdeckung mit Abintegro anmelden, überprüft Abintegro, ob der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch vom Abintegro erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-abintegro-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Abintegro:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Abintegro **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-abintegro-tutorial/IC790084.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-abintegro-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
