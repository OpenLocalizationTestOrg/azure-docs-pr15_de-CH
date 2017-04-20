<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Kontiki | Microsoft Azure" 
    description="Erfahren Sie, wie mit Kontiki Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Lernprogramm: Azure Active Directory-Integration mit Kontiki
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Kontiki anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Kontiki einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Kontiki Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Kontiki zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Kontiki
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-kontiki-tutorial/IC790235.png "Szenario")
##<a name="enabling-the-application-integration-for-kontiki"></a>Die Application Integration für Kontiki
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Kontiki aktivieren.

###<a name="to-enable-the-application-integration-for-kontiki-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Kontiki:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-kontiki-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-kontiki-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-kontiki-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Kontiki**.

    ![Anwendung Galerie] (./media/active-directory-saas-kontiki-tutorial/IC790236.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Kontiki aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Kontiki] (./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Kontiki mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Kontiki** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-kontiki-tutorial/IC790238.png "Gesamtauthentifizierung konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Kontiki** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-kontiki-tutorial/IC790239.png "Gesamtauthentifizierung konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Kontiki** URL Kontiki Anmelden von Benutzern verwendet (z. B.: "*https://company.mc.eval.kontiki.com/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-kontiki-tutorial/IC790240.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Kontiki** auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-kontiki-tutorial/IC790241.png "Gesamtauthentifizierung konfigurieren")

5.  Senden der Metadatafile zu Kontiki.

    >[AZURE.NOTE] Die Konfiguration für einzelne Zeichen muss Kontiki Support Team ausgeführt werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-kontiki-tutorial/IC790242.png "Gesamtauthentifizierung konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer Kontiki Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer Kontiki mit der Option Zugriff anmelden, prüft Kontiki der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch vom Kontiki erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-kontiki-perform-the-following-steps"></a>Kontiki Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Kontiki **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-kontiki-tutorial/IC790243.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-kontiki-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)