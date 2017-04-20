<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Envoy | Microsoft Azure" 
    description="Erfahren Sie, wie mit Envoy Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Lernprogramm: Azure Active Directory Integration Envoy
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Envoy anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Envoy Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem Envoy Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Envoy zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration Envoy aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Szenario")
##<a name="enabling-the-application-integration-for-envoy"></a>Die Anwendungsintegration Envoy aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Envoy aktivieren.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration beauftragten:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Envoy**.

    ![Anwendung Galerie] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Envoy aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Envoy Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Envoy erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Envoy** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Envoy** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Envoy Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Envoy.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Konfigurieren von app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Envoy** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\Envoy.cer**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator Ihre Website beauftragten Unternehmens.

6.  **Klicken Sie auf der Symbolleiste im oberen Bereich.**

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  Klicken Sie auf **Unternehmen**.

    ![Unternehmen] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Unternehmen")

8.  Klicken Sie auf **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  Im Konfigurationsabschnitt **SAML-Authentifizierung** die folgenden Schritte:

    ![SAML-Authentifizierung] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML-Authentifizierung")

    >[AZURE.NOTE] Der Wert für die ID HQ wird automatisch von der Anwendung generiert.

    1.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Fingerabdruck** .  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Envoy** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **Identität Provider HTTP SAML-URL** .
    3.  Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer gesandten Bereitstellung konfigurieren.  
Wenn zugewiesene Benutzer Envoy mit der Option Zugriff anmelden, prüft Envoy der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch von Envoy erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Um Envoy Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Envoy **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)