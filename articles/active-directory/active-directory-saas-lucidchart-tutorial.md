<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Lucidchart | Microsoft Azure" 
    description="Erfahren Sie, wie mit Lucidchart Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Lernprogramm: Azure Active Directory-Integration mit Lucidchart
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Lucidchart anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Lucidchart einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Lucidchart Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Lucidchart zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Lucidchart
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Szenario")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Die Application Integration für Lucidchart
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Lucidchart aktivieren.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Lucidchart:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Lucidchart**.

    ![Anwendung Galerie] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Lucidchart aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Lucidchart mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Lucidchart** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Lucidchart** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Lucidchart** URL zur Lucidchart Anwendung anmelden von Benutzern verwendet (z. B.: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Lucidchart** herunterladen Metadaten auf **Metadaten**und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Lucidchart.

6.  Klicken Sie im Menü oben auf **Team**.

    ![Team] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Team")

7.  Klicken Sie auf **Anwendung \> SAML verwalten**.

    ![SAML verwalten] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "SAML verwalten")

8.  Folgendermaßen Sie auf der Dialogseite **SAML Einstellung** an:

    1.  Wählen Sie **SAML-Authentifizierung aktivieren**und dann auf **Optional**.
        ![SAML Einstellung] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML Einstellung")
    2.  Im Feld **Domäne** die Domäne und dann auf **Zertifikat ändern**.
        ![Zertifikat ändern] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Zertifikat ändern")
    3.  Öffnen Sie der heruntergeladenen Metadatendatei, kopieren Sie den Inhalt und fügen Sie ihn in das Textfeld **Metadaten laden** .
        ![Hochladen von Metadaten] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Hochladen von Metadaten")
    4.  Wählen Sie **Neuer Benutzer dem Team automatisch hinzufügen**und dann auf **Speichern**.
        ![Speichern] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Speichern")

9.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer Lucidchart Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer Lucidchart mit der Option Zugriff anmelden, prüft Lucidchart der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto verfügbar noch, wird es automatisch vom Lucidchart erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Um Lucidchart Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Lucidchart **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)