<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Overdrive | Microsoft Azure" 
    description="Erfahren Sie, wie mit Overdrive Bücher mit Active Directory Azure-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Lernprogramm: Azure Active Directory-Integration mit Overdrive
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und OverDrive anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Intel® OverDrive® SSO-Abonnement aktiviert
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem OverDrive Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, OverDrive zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration OverDrive aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Szenario")
##<a name="enabling-the-application-integration-for-overdrive"></a>Die Anwendungsintegration OverDrive aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration OverDrive aktivieren.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration OverDrive:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **OverDrive**.

    ![Anwendung Galerie] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **OverDrive aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer OverDrive Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **OverDrive** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Benutzer anmelden OverDrive** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **OverDrive Zeichen In URL** den URL dem folgenden Muster "*http://mslibrarytest.libraryreserve.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "Konfigurieren von App-URL")

4.  Auf der **Konfigurieren einmaliges Anmelden am OverDrive** Seite Metadaten herunterladen und an Intel® OverDrive® Support-Team senden.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Einmaliges Anmelden für konfigurieren")

    >[AZURE.NOTE]Die OverDrive einmaliges Anmelden für Sie konfiguriert und erhalten Sie eine Benachrichtigung, wenn die Konfiguration abgeschlossen ist.

5.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe für Benutzer bereitstellen, OverDrive konfigurieren.  
Zugewiesene Benutzer OverDrive anmelden, wird ein Konto OverDrive automatisch bei Bedarf erstellt.

>[AZURE.NOTE]Alle anderen OverDrive Benutzer Konto Erstellungstools verwenden oder APIs von OverDrive Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>OverDrive Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **OverDrive **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)