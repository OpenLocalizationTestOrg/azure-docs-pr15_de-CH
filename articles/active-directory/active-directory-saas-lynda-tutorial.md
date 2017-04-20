<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Lynda.com | Microsoft Azure" 
    description="Erfahren Sie, wie mit Lynda.com Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Lernprogramm: Azure Active Directory Integration Lynda.com
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Lynda.com anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Lynda.com Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Lynda.com Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Lynda.com zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration Lynda.com aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Szenario")
##<a name="enabling-the-application-integration-for-lyndacom"></a>Die Anwendungsintegration Lynda.com aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Lynda.com aktivieren.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Lynda.com:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Lynda.com**.

    ![Anwendung Galerie] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Lynda.com aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Lynda.com Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

>[AZURE.IMPORTANT]Um einmaliges Anmelden auf Ihrem Mandanten Lynda.com konfigurieren zu können, müssen Sie zuerst Lynda.com technische Unterstützung dieser Funktion zu erhalten.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Lynda.com** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Lynda.com** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Lynda.com Zeichen In URL** den Lynda.com Mieter URL (z. B.: *Https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-lynda-tutorial/IC781047.png "Konfigurieren von app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Lynda.com** herunterladen Metadaten auf **Metadaten**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Einmaliges Anmelden für konfigurieren")

5.  Senden Sie die heruntergeladenen Metadatendatei Lynda.com Support-Team. Lynda.com Support-Team wird die Einmalanmeldung Konfiguration für Sie.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer Lynda.com Bereitstellung konfigurieren.  
Wenn zugewiesene Benutzer Lynda.com mit der Option Zugriff anmelden, prüft Lynda.com der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch von Lynda.com erstellt.

>[AZURE.NOTE]Alle anderen Lynda.com Benutzer Konto Erstellungstools verwenden oder APIs von Lynda.com Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Lynda.com Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Lynda.com **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)