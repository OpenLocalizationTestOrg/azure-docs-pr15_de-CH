<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration durch neue | Microsoft Azure" 
    description="Erfahren Sie, wie mit neuen Relikt Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Lernprogramm: Azure Active Directory-Integration durch neue
  
Dieses Lernprogramm soll veranschaulichen, um einmaliges Anmelden zwischen Azure Active Directory und neue Relikt einzurichten.
  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine neue Relikt einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden Azure Active Directory-Benutzer, die neue Relikt zugewiesen haben einmaliges Abdeckung AAD verwenden können.

1.  Die Application Integration für neue Relikt
2.  Konfigurieren von Single Sign-On
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Szenario")
##<a name="enabling-the-application-integration-for-new-relic"></a>Die Application Integration für neue Relikt
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für neue Relikt aktivieren.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für neue Relikt:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Neue Relikt**.

    ![Anwendung Galerie] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Neue Relikt aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Neue Relikt] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Neue Relikt")
##<a name="configuring-single-sign-on"></a>Konfigurieren von Single Sign-On
  
Dieser Abschnitt beschreibt, wie Benutzer neue Relikt mit seinem Konto in Azure Active Directory Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Neue Relikt** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei neuen Relikt** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Neue Relikt Anmelde-URL** die URL der Anwendung neue Relikt Anmelden von Benutzern verwendet, und klicken Sie auf **Weiter**. 

    Die app URL ist der neue Relikt Mieter (z.B.: *https://rpm.newrelic.com*):

    ![Konfigurieren von App-URL] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am neuen Relikt** das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Datei lokal auf dem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Einmaliges Anmelden konfigurieren")

5.  In anderen Webbrowserfenster melden der Unternehmenswebsite **Neue Relikt** Administrator.

6.  Klicken Sie im Menü oben auf **Konto**.

    ![Konto] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Konto")

7.  Klicken Sie auf die Registerkarte **Sicherheit und Authentifizierung** , und klicken Sie dann auf die Registerkarte **für einmaliges Anmelden** .

    ![Einmaliges Anmelden] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Einmaliges Anmelden")

8.  Auf der Dialogseite SAML Schritte:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Klicken Sie auf **Datei auswählen** , um Ihre heruntergeladenen Azure Active Directory Zertifikat laden.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am neuen Relikt** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Remote Login-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am neuen Relikt** Wert **Remote Logout-URL** und fügen Sie ihn in das Textfeld **URL Zielseite Abmelden** .
    4.  Klicken Sie auf **Änderungen speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure Active Directory Benutzer neue Relikt anmelden können, müssen sie in neue Relikt bereitgestellt werden.  
Bei neuen Relikt ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Bereitstellen ein Benutzerkontos zu neuen Relikt Schritte:

1.  Melden Sie sich Ihre **Neue Relikt** Unternehmensstandort als Administrator an.

2.  Klicken Sie im Menü oben auf **Konto**.

    ![Konto] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Konto")

3.  Klicken Sie im Bereich **Konto** auf der linken Seite auf **Zusammenfassung**, und klicken Sie dann auf **Benutzer hinzufügen**.

    ![Konto] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Konto")

4.  Gehen Sie im Dialogfeld **aktive Benutzer** :

    ![Aktive Benutzer] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktive Benutzer")

    1.  Geben Sie im Feld **E-Mail** die e-Mail-Adresse eines gültigen Benutzers Azure Active Directory bereitstellen möchten.
    2.  Wählen Sie als **Rolle** **Benutzer**.
    3.  Klicken Sie auf **Benutzer hinzufügen**.

>[AZURE.NOTE]Alle anderen neuen Relikt Benutzer Konto Erstellungstools verwenden oder APIs von neuen Relikt Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Um neue Relikt Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Neue Relikt** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)




