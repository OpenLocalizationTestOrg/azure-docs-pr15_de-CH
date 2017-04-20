<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Verwaltungskonsole Mimecast | Microsoft Azure" 
    description="Lernen Sie Mimecast Verwaltungskonsole Azure Active Directory einmaliges, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Lernprogramm: Azure Active Directory-Integration mit Mimecast-Verwaltungskonsole
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Mimecast-Verwaltungskonsole anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mimecast-Verwaltungskonsole einmaliges Anmelden aktiviert Abonnement
  
Nach diesem Lernprogramm werden können einzelne Zeichen in der Anwendung in der Verwaltungskonsole Mimecast Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Mimecast-Verwaltungskonsole zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Mimecast-Verwaltungskonsole aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Szenario")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Anwendungsintegration für Mimecast-Verwaltungskonsole aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Mimecast-Verwaltungskonsole aktivieren.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Mimecast-Verwaltungskonsole:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Mimecast-Verwaltungskonsole**.

    ![Anwendung Galerie] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Verwaltungskonsole Mimecast aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Mimecast-Verwaltungskonsole] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast-Verwaltungskonsole")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Admin-Konsole Mimecast Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Mimecast-Verwaltungskonsole** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer Mimecast Admin-Konsole anmelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Mimecast Admin Console Anmelde-URL** den URL der Anwendung Mimecast Admin-Konsole anmelden von Benutzern verwendet (z. B.: "https://webmail-uk.mimecast.com" oder "https://webmail-us.mimecast.com"), und klicken Sie dann auf **Weiter**.

    >[AZURE.NOTE] Die Anmeldung URL ist regionsspezifisch.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Mimecast Admin-Konsole** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator in der Verwaltungskonsole Mimecast.

6.  Gehen Sie zu **Services \> Anwendung**.

    ![Dienste] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Dienste")

7.  Klicken Sie auf **Authentifizierungsprofile**.

    ![Authentifizierungsprofile] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Authentifizierungsprofile")

8.  Klicken Sie auf **neue Authentifizierungsprofil**.

    ![Neue Authentifizierungsprofile] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Neue Authentifizierungsprofile")

9.  Im Abschnitt **Authentifizierungsprofil** Schritte:

    ![Authentifizierungsprofil] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Authentifizierungsprofil")

    1.  Geben Sie im Textfeld **Beschreibung** einen Namen für Ihre Konfiguration.
    2.  Wählen Sie **Mimecast Verwaltungskonsole SAML Authentifizierung erzwingen**.
    3.  Wählen Sie als **Anbieter** **Azure Active Directory**.
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Mimecast-Verwaltungskonsole** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **URL Aussteller** .
    5.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Mimecast-Verwaltungskonsole** **Remote Login-URL** -Wert und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    6.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Mimecast-Verwaltungskonsole** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **URL Abmelden** .  

        >[AZURE.NOTE]Die Anmelde-URL und dem Wert der Logout URL stimmen für Mimecast-Verwaltungskonsole.

    7.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

    8.  Öffnen das Base64-codierte Zertifikat in Editor entfernen die erste Zeile ("*--*") und die letzte Zeile ("*--*"), den verbleibenden Inhalt in die Zwischenablage kopieren und fügen Sie ihn in das Textfeld **Anbieter Identitätszertifikat (Metadaten)** .
    9.  Wählen Sie **einzelne Zeichen auf zulassen**.
    10. Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Mimecast Admin-Konsole anmelden können, müssen sie in der Verwaltungskonsole für Mimecast bereitgestellt werden.  
Bei Mimecast-Verwaltungskonsole ist die Bereitstellung einer manuellen Aufgabe.
  
Sie müssen eine Domäne registrieren, bevor Sie Benutzer erstellen können.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich als Administrator auf **Mimecast-Verwaltungskonsole** an.

2.  Gehen Sie zu **Verzeichnisse \> interne**.

    ![Verzeichnisse] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Verzeichnisse")

3.  Klicken Sie auf **neue Domäne registrieren**.

    ![Neue Domäne registrieren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Neue Domäne registrieren")

4.  Nachdem die neue Domäne erstellt wurde, klicken Sie auf **Neue Adresse**.

    ![Neue Adresse] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Neue Adresse")

5.  Klicken Sie im Dialogfeld neue Adresse folgendermaßen Sie an:

    ![Speichern] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Speichern")

    1.  Geben Sie die **E-Mail-Adresse**, **Globale Name**, **Kennwort** und **Kennwort bestätigen** Attribute eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE]Sie können andere Mimecast-Verwaltungskonsole Benutzer Konto Erstellungstools und APIs zur Bereitstellung AAD Benutzerkonten mit Mimecast-Verwaltungskonsole verwenden.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Um Mimecast-Verwaltungskonsole Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Mimecast-Verwaltungskonsole **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)