<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Iglu | Microsoft Azure" 
    description="Erfahren Sie, wie mit Iglu Software mit Active Directory Azure-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Lernprogramm: Azure Active Directory-Integration mit Iglu
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Iglu Software anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein [Iglu Software](http://www.igloosoftware.com/) für einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an die Iglu Software Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Iglu Software zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Iglu
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Szenario")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Die Application Integration für Iglu
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Iglu-Software zu aktivieren.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Iglu:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Iglu Software**.

    ![Anwendung Galerie] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Iglu Software**, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Iglu] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Iglu")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Iglu Software mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat Ihrem Mandanten zentrale Desktop hochgeladen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Iglu Software** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Iglu Software** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Iglu Software Zeichen In URL** den URL dem folgenden Muster "*https://company.igloocommunities.com/?signin*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Iglu Software** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Iglu Software.

6.  Wechseln Sie in das **Bedienfeld**.

    ![Bedienfeld] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Bedienfeld")

7.  Klicken Sie auf der Registerkarte **Mitgliedschaft** auf **Sign In**.

    ![Einstellungen anmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Einstellungen anmelden")

8.  Klicken Sie im SAML-Konfigurationsabschnitt auf **SAML-Authentifizierung konfigurieren**.

    ![SAML-Konfiguration] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML-Konfiguration")

9.  Folgendermaßen Sie im Abschnitt **Allgemeine Konfiguration** an:

    ![Allgemeine Konfiguration] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Allgemeine Konfiguration")

    1.  Geben Sie im Textfeld **Name der Verbindung** einen benutzerdefinierten Namen für die Konfiguration ein.
    2.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren SSO-Iglu-Software** **Remote Login-URL** -Wert und fügen Sie ihn in das Textfeld **IdP Anmelde-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Iglu Software** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **IdP Logout URL** .
    4.  Wählen Sie **Logout Antwort und HTTP-Anforderungstyp** **Bereitstellen**.
    5.  Erstellen Sie eine Textdatei aus dem heruntergeladenen Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    6.  Datei Textversion des Zertifikats die erste und letzte Zeile entfernen, den verbleibenden Zertifikat Text und fügen Sie ihn in das **Öffentliche Zertifikat** Textbox.

10. Führen Sie in der **Antwort und Konfiguration**die folgenden Schritte:

    ![Antworten und Konfiguration] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Antworten und Konfiguration")

    1.  Wählen Sie als **Identitätsanbieter** **Microsoft ADFS**.
    2.  Wählen Sie als **Typ ID** **E-Mail-Adresse**.
    3.  Geben Sie in das Textfeld **E-Mail-Attribut** **Emailaddress**.
    4.  Geben Sie im Textfeld **Vorname Attribut** **"givenName"**.
    5.  Geben Sie im Textfeld **Last Name-Attribut** **Name**.

11. Durchführen Sie die Konfiguration der folgenden Schritte:

    ![Die Erstellung des Benutzers auf] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Die Erstellung des Benutzers auf")

    1.  **Erstellt auf**wählen Sie **erstellen einen neuen Benutzer in Ihrer Site, wenn sie sich anmelden**.
    2.  Als **Einstellung anmelden**Schaltfläche **verwenden SAML Bildschirm "Anmelden"**.
    3.  Klicken Sie auf **Speichern**.

12. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Konfigurieren Benutzer Iglu Software bereitstellen.  
Wenn der zugewiesene Benutzer Iglu-Software mit der Option Zugriff anmelden, prüft Iglu Software der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch von Iglu Software erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Iglu Software:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Iglu Software **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)