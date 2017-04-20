<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Eckpfeiler OnDemand | Microsoft Azure" 
    description="Erfahren Sie, wie mit Eckpfeiler OnDemand Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Lernprogramm: Azure Active Directory Integration Eckpfeiler OnDemand

Das Ziel dieses Lernprogramms ist die Integration von Azure und Eckpfeiler OnDemand anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eckpfeiler OnDemand Mieter

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Eckpfeiler OnDemand Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Eckpfeiler OnDemand zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration Eckpfeiler OnDemand aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Szenario")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>Die Anwendungsintegration Eckpfeiler OnDemand aktivieren

Dieser Abschnitt soll beschreiben die Anwendungsintegration Eckpfeiler OnDemand aktivieren.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Eckpfeiler OnDemand:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Eckpfeiler Ondemand**.

    ![Anwendung Galerie] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Eckpfeiler OnDemand aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Eckpfeiler OnDemand] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Eckpfeiler OnDemand")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Eckpfeiler OnDemand Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Eckpfeiler OnDemand** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Sie Benutzer Eckpfeiler OnDemand anzumelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Eckpfeiler OnDemand Zeichen In URL** den URL dem folgenden Muster "*http://company.csod.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Eckpfeiler OnDemand** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie die folgenden Elemente der Grundstein OnDemand Team Unterstützung:

    1.  Heruntergeladene Zertifikat
    2.  **Remote Login-URL** -Wert
    3.  **Remote Logout URL** -Wert.

    >[AZURE.NOTE] Einmaliges muss Eckpfeiler OnDemand-Supportteam konfiguriert werden.
Sie erhalten eine Benachrichtigung über das Support-Team nach Abschluss die Konfiguration.

6.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Eckpfeiler OnDemand anmelden können, müssen sie in Eckpfeiler OnDemand bereitgestellt werden.  
Bei Eckpfeiler OnDemand ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Senden Sie die Informationen (z. B.: Name, Emial) über Azure AD Benutzer zu Eckpfeiler OnDemand sollen-Supportteam.

>[AZURE.NOTE] Alle anderen Eckpfeiler OnDemand Benutzer Konto Erstellungstools verwenden oder APIs von Eckpfeiler OnDemand Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Um Eckpfeiler OnDemand Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Eckpfeiler OnDemand **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Benutzer zuweisen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Benutzer zuweisen")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
