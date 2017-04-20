<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Brightspace von Desire2Learn | Microsoft Azure" 
    description="Erfahren Sie, wie mit Brightspace von Desire2Learn Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Lernprogramm: Azure Active Directory-Integration mit Brightspace von Desire2Learn

Das Ziel dieses Lernprogramms ist die Integration von Azure und Brightspace von Desire2Learn anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Brightspace von Desire2Learn SSO-Abonnement aktiviert

Am Ende dieses Lernprogramms werden einmaliges in der Anwendung an die Brightspace von Desire2Learn Website (Service Provider initiiert anmelden) oder mit der [Einführung in die Abdeckung](active-directory-saas-access-panel-introduction.md)in Azure AD-Benutzern, Brightspace durch Desire2Learn zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Brightspace von Desire2Learn
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Szenario")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Die Application Integration für Brightspace von Desire2Learn

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Brightspace von Desire2Learn aktivieren.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Brightspace von Desire2Learn:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Brightspace von Desire2Learn**.

    ![Apllication-Galerie] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication-Galerie")

7.  Wählen Sie im Ergebnisbereich **Brightspace von Desire2Learn aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Brightspace von Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace von Desire2Learn")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Brightspace von Desire2Learn Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Brightspace von Desire2Learn** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Brightspace von Desire2Learn** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Einmaliges Anmelden konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **Anmelde-URL** den URL der **Brightspace von Desire2Learn** Anmelden von Benutzern verwendet (z.B.: **).
    2.  Klicken Sie auf **Weiter**

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Brightspace von Desire2Learn** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie heruntergeladene Metadatendatei an Ihre Brightspace Desire2Learn Support-Team

    >[AZURE.NOTE] Die Brightspace von Desire2Learn Support-Team ist die SSO-Konfiguration.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD Benutzer Brightspace von Desire2Learn anmelden können, müssen sie durch Desire2Learn in Brightspace bereitgestellt werden.  
Benutzerkonten müssen bei Brightspace von Desire2Learn von Desire2Learn Support-Team von der Brightspace erstellt werden.

>[AZURE.NOTE] Können andere Brightspace Desire2Learn Benutzer Konto Erstellungstools oder APIs von Brightspace von Desire2Learn Bereitstellung Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Um Brightspace von Desire2Learn Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Brightspace von Desire2Learn **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
