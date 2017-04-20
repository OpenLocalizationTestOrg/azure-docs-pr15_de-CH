<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Arbeitstag | Microsoft Azure" 
    description="Erfahren Sie, wie mit Arbeitstag Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Lernprogramm: Azure Active Directory Integration Arbeitstag
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Arbeitstag anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Arbeitstag
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren der Anwendungsintegration Arbeitstag
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Konfigurieren der Benutzer bereitstellen

![Szenario] (./media/active-directory-saas-workday-tutorial/IC782919.png "Szenario")

##<a name="enabling-the-application-integration-for-workday"></a>Aktivieren der Anwendungsintegration Arbeitstag
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Salesforce aktivieren.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Arbeitstag:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-workday-tutorial/IC700994.png "Applikationen")

4.  Öffnen Sie die **Anwendung Galerie**auf **Einer Anwendung hinzufügen**und dann auf **eine Anwendung für meine Organisation hinzufügen**.

    ![Was möchten Sie tun?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Was möchten Sie tun?")

5.  Geben Sie im **Suchfeld** **Arbeitstag**.

    ![Arbeitstag] (./media/active-directory-saas-workday-tutorial/IC701021.png "Arbeitstag")

6.  Klicken Sie im Ergebnisbereich wählen Sie **Arbeitstag aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Arbeitstag] (./media/active-directory-saas-workday-tutorial/IC701022.png "Arbeitstag")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Arbeitstag mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie auf der Seite Application Integration **Arbeitstag** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-workday-tutorial/IC782920.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Arbeitstag** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-workday-tutorial/IC782921.png "Einmaliges Anmelden für konfigurieren")

3.  Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-workday-tutorial/IC782957.png "Konfigurieren von App-URL")

    ein. Geben Sie im Textfeld **Anmelde-URL** den URL Arbeitstag der folgenden Muster Anmelden von Benutzern verwendet:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  Geben Sie im Textfeld **Arbeitstag Antwort-URL** Arbeitstag Antwort-URL dem folgenden Muster:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Antwort-URL muss eine untergeordnete Domäne (z. B.: Www, wd2, wd3, wd3-Impl, wd5, wd5 Impl). 
    >Funktioniert mit etwas "*http://www.myworkday.com*", "*http://myworkday.com*" jedoch nicht. 
 
4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-workday-tutorial/IC782922.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Arbeitstag.

6.  Rufen Sie **Menü \> Workbench**.

    ![Arbeitsfläche] (./media/active-directory-saas-workday-tutorial/IC782923.png "Arbeitsfläche")

7.  Gehe zu **Verwalten**.

    ![Verwaltung] (./media/active-directory-saas-workday-tutorial/IC782924.png "Verwaltung")

8.  Gehe zu **Bearbeiten Mieter Setup – Security**.

    ![Mieter Sicherheit bearbeiten] (./media/active-directory-saas-workday-tutorial/IC782925.png "Mieter Sicherheit bearbeiten")

9.  Im Abschnitt **Umleitung URLs** Schritte:

    ![Umleitung URLs] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Umleitung URLs")

    ein. Klicken Sie auf **Zeile**.

    b. Geben Sie im Textfeld **Anmelde-URL umleiten** und **Mobile URL umleiten** Textbox **App URL konfigurieren** auf klassischen Azure-Portal eingegebene **Arbeitstag Mieter URL** .
    
    c. Im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** **Einzelnen Abmelde Service URL**kopieren Sie und fügen Sie ihn in das Textfeld **Logout URL umleiten** .

    d.  Geben Sie im Textfeld **Umgebung** der Name.  


    >[AZURE.NOTE] Der Wert der Tenant-URL der Wert des Attributs Umgebung verbunden:
    >
    >-   Der Domänenname des Mieters Arbeitstag beginnt mit Impl URL (z. B.: *https://impl.workday.com/\<Mieter\>/login-saml2.htmld*), Implementierung **Umgebung** Attribut festgelegt werden.
    >-   Beginnt der Domänennamen mit etwas müssen Sie mit Arbeitstag um übereinstimmende **Umgebung** nutzen.

10. Im Abschnitt **SAML Setup** folgendermaßen:

    ![SAML-Setup] (./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-Setup")

    ein.  Aktivieren Sie **SAML-Authentifizierung**.

    b.  Klicken Sie auf **Zeile**.

11. Im Abschnitt SAML Identitätsprovider Schritte:

    ![SAML Identitätsanbieter] (./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identitätsanbieter")

    ein. Geben Sie im Textfeld Anbieternamen Identität einen Anbieternamen (z.B.: *SPInitiatedSSO*).

    b. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** **Identität Anbieter-ID** -Wert und fügen Sie ihn in das Textfeld **Aussteller** .

    c. Aktivieren Sie **Arbeitstag Initialted Abmelden**.

    d. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** den **Einzelnen Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **Logout URL anfordern** .


    e. **Identität Anbieter öffentliches Schlüsselzertifikat**auf und klicken Sie dann auf **Erstellen**. 

    ![Erstellen] (./media/active-directory-saas-workday-tutorial/IC782928.png "Erstellen")

    f. Klicken Sie auf **X509 Erstellen öffentlicher Schlüssel**. 
        
    ![Erstellen] (./media/active-directory-saas-workday-tutorial/IC782929.png "Erstellen")


1. Im Abschnitt **Öffentlichen Schlüssel anzeigen X509** Schritte: 

    ![Öffentlichen Schlüssel anzeigen x509] (./media/active-directory-saas-workday-tutorial/IC782930.png "Öffentlichen Schlüssel anzeigen x509") 

    ein. Geben Sie in das Textfeld **Name** einen Namen für das Zertifikat (z.B.: *PSA\_SP*).
        
    b. Geben Sie im Feld **Gültig ab** des gültigen Attributwert des Zertifikats.
    
    c.  Geben Sie im Textfeld **Gültig bis** -Attributwert des Zertifikats ungültig.
        
    >[AZURE.NOTE] Sie erhalten gültigen Datum und gültigen, auf das heruntergeladene Zertifikat durch Doppelklick. Die Daten werden unter der Registerkarte **Details** aufgeführt.

    d. Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

    >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    e.  Das Base64-codierte Zertifikat im Editor öffnen und den Inhalt kopieren.
    
    f.  Fügen Sie im Textfeld **Zertifikat** den Inhalt der Zwischenablage ein.
    
    g.  Klicken Sie auf **OK**.

12.  Führen Sie die folgenden Schritte aus: 

    ![SSO-Konfiguration] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO-Konfiguration")

    ein.  Aktivieren der **X509 Private Key Pair**.

    b.  Geben Sie im Textfeld **Service Provider-ID** **http://www.workday.com**.

    c.  Wählen Sie **Enable SP SAML-Authentifizierung initiiert**.

    d.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **IdP SSO-Dienst-URL** .
     
    e. Wählen Sie **SP initiierte Authentifizierungsanforderung nicht verkleinern**.

    f. Wählen Sie als **Anforderung Signatur Authentifizierungsmethode** **SHA256**. 
        
    ![Die Signaturmethode Authentifizierung anfordern] (./media/active-directory-saas-workday-tutorial/IC782932.png "Die Signaturmethode Authentifizierung anfordern") 
 
    g. Klicken Sie auf **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Klicken Sie auf **Weiter**im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Arbeitstag** . 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-workday-tutorial/IC782934.png "Einmaliges Anmelden für konfigurieren")

13. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Einmaliges Anmelden für konfigurieren")



##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um get Testbenutzer in Arbeitstag bereitgestellt müssen Sie den Arbeitstag-Support wenden.  
Arbeitstag-Supportteam wird den Benutzer erstellt.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Arbeitstag:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Arbeitstag **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-workday-tutorial/IC782935.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-workday-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)