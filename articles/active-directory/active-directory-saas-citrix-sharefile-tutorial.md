<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Citrix ShareFile | Microsoft Azure" 
    description="Erfahren Sie, wie mit Citrix ShareFile Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Lernprogramm: Azure Active Directory-Integration mit Citrix ShareFile

Das Ziel dieses Lernprogramms ist die Integration von Azure und Citrix ShareFile anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Citrix ShareFile Mieter

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem Citrix ShareFile Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Citrix ShareFile zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Citrix ShareFile aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Szenario")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Anwendungsintegration für Citrix ShareFile aktivieren

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Citrix ShareFile aktivieren.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Citrix ShareFile:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Citrix ShareFile**.

    ![Anwendung Galerie] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Citrix ShareFile aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Citrix-ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix-ShareFile")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Citrix ShareFile Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Citrix ShareFile** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Citrix ShareFile** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Citrix ShareFile Anmelde-URL** den URL dem folgenden Muster `https://<tenant-name>.shareFile.com`, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix ShareFile** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![ConfigureSingle Anmeldung] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle Anmeldung")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens **Citrix ShareFile** .

6.  Klicken Sie in der oberen Symbolleiste auf **Administration**.

7.  Wählen Sie im linken Navigationsbereich **Konfigurieren Sie einmaliges Anmelden**.

    ![Verwaltung] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Verwaltung")

8.  Auf der **Single Sign-On / SAML 2.0-Konfiguration** Seite unter **Standardeinstellungen**folgendermaßen:

    ![Einmaliges Anmelden] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Einmaliges Anmelden")

    1.  Klicken Sie auf **SAML aktivieren**.
    2.  Im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix ShareFile** **Entitäts-ID** -Wert kopieren und fügen Sie ihn in den **der IDP Emittenten / Entitäts-ID** Textfeld.
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix ShareFile** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf **das Konfigurieren einmaliges Anmelden bei Citrix ShareFile** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Klicken Sie neben dem Feld **X. 509-Zertifikat** auf **Ändern** , und Laden Sie das Zertifikat aus dem klassischen Azure AD-Portal herunterladen.
        ![Standardeinstellungen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Standardeinstellungen")

9.  **Klicken Sie auf die Citrix ShareFile Verwaltungsportal.**

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Citrix ShareFile anmelden können, müssen sie in Citrix ShareFile bereitgestellt werden.  
Bei Citrix ShareFile ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem **Citrix ShareFile** Mandanten.

2.  Klicken Sie auf **Benutzer verwalten \> Benutzern verwalten \> + Mitarbeiter erstellen**.

    ![Mitarbeiter erstellen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Mitarbeiter erstellen")

3.  Geben Sie die **E-Mail**, **Vorname** und **Nachname** einer gültigen Azure Anzeige berücksichtigen Sie bereitstellen möchten.

    ![Grundlegende Informationen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Grundlegende Informationen")

4.  Klicken Sie auf **Benutzer hinzufügen**.

    >[AZURE.NOTE] Kontoinhaber AAD e-Mail und einen Link, um ihr Konto bestätigen aktiviert wird.

>[AZURE.NOTE] Alle anderen Citrix ShareFile Benutzer Konto Erstellungstools verwenden oder APIs von Citrix ShareFile Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Citrix ShareFile:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Citrix ShareFile ** **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
