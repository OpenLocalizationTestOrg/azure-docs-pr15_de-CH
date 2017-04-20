<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Zscaler 1 | Microsoft Azure" 
    description="Erfahren Sie, wie mit Zscaler eine Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a>Lernprogramm: Azure Active Directory Integration Zscaler ein

Ziel dieses Lernprogramms ist die Integration von Azure und einem ZScaler angezeigt.  
 In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:  

-   Ein gültiges Azure-Abonnement
-   Ein ZScaler ein einmaliges Abonnement aktiviert  

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die ZScaler einer Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die einen ZScaler zugewiesen haben.  

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:  

1.  Die Application Integration für ZScaler
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren von Proxyeinstellungen
4.  Konfigurieren der Benutzer bereitstellen
5.  Zuweisen von Benutzern  

![Szenario] (./media/active-directory-saas-zscaler-one-tutorial/IC800214.png "Szenario")  

##<a name="enabling-the-application-integration-for-zscaler-one"></a>Die Application Integration für ZScaler

Dieser Abschnitt soll beschreiben die Anwendungsintegration für ZScaler aktivieren.  

###<a name="to-enable-the-application-integration-for-zscaler-one-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ZScaler ein:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.  

    ![Active Directory] (./media/active-directory-saas-zscaler-one-tutorial/IC700993.png "Active Directory")  

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.  

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .  

    ![Applikationen] (./media/active-directory-saas-zscaler-one-tutorial/IC700994.png "Applikationen")  

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .  

    ![Anwendung hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC749321.png "Anwendung hinzufügen")  

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.  

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-zscaler-one-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")  

6.  Geben Sie im **Suchfeld** **ZScaler ein**.  

    ![Anwendung Galerie] (./media/active-directory-saas-zscaler-one-tutorial/IC800215.png "Anwendung Galerie")  

7.  Wählen Sie im Ergebnisbereich **Eine ZScaler aus**und dann auf **vollständig** die Anwendung hinzufügen.  

    ![ZScaler 1] (./media/active-directory-saas-zscaler-one-tutorial/IC800216.png "ZScaler 1")  

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer ZScaler ein Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie die wichtigste ZScaler eine Base64-codierte Zertifikat hinzufügen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)  

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf **Einem ZScaler** Integration Anwendung auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.  

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zscaler-one-tutorial/IC800217.png "Einmaliges Anmelden konfigurieren")  

2.  Auf der Seite **Wie möchten Sie Benutzer für einen ZScaler anmelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.  

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zscaler-one-tutorial/IC800218.png "Einmaliges Anmelden konfigurieren")  

3.  Geben Sie auf der Seite **Konfigurieren App URL** in das Textfeld **ZScaler ein Anmelde-URL** die URL von den Benutzern der ZScaler einer Anwendung anmelden und klicken Sie dann auf **Weiter**.  

    ![Konfigurieren von App-URL] (./media/active-directory-saas-zscaler-one-tutorial/IC800219.png "Konfigurieren von App-URL")  

    >[AZURE.NOTE]Sie erhalten den tatsächlichen Wert für Ihre Umgebung eine ZScaler Supports bei Bedarf.  

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden bei einem ZScaler** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.  

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zscaler-one-tutorial/IC800220.png "Einmaliges Anmelden konfigurieren")  

5.  Melden Sie in anderen Webbrowserfenster als Administrator ZScaler einem Unternehmensstandort.  

6.  Klicken Sie im Menü oben auf **Verwaltung**.  

    ![Verwaltung] (./media/active-directory-saas-zscaler-one-tutorial/IC800206.png "Verwaltung")  

7.  **Administratoren verwalten und Rollen**klicken Sie auf **Benutzer verwalten und Authentifizierung**.  

    ![Verwalten von Benutzern und Authentifizierung] (./media/active-directory-saas-zscaler-one-tutorial/IC800207.png "Verwalten von Benutzern und Authentifizierung")  

8.  Im Abschnitt **Wählen Sie Authentifizierungsoptionen für Ihre Organisation** Schritte:  

    ![Authentifizierung] (./media/active-directory-saas-zscaler-one-tutorial/IC800208.png "Authentifizierung")  

    1.  **Authentifizieren mit SAML einmaliges Anmelden**auswählen.  
    2.  Klicken Sie auf **Konfigurieren SAML Single Sign-On**.  

9.  Gehen Sie auf der Dialogseite **Konfigurieren SAML Single Sign-On Parameter** und dann auf **Fertig**:  

    ![Einmaliges Anmelden] (./media/active-directory-saas-zscaler-one-tutorial/IC800209.png "Einmaliges Anmelden")  

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ZScaler eine** **Authentifizierung anfordern URL** -Wert und fügen Sie ihn in das Textfeld **URL des Portals SAML, Benutzer für die Authentifizierung gesendet werden** .  
    2.  Geben Sie im Textfeld **Attribut enthält Benutzernamen** **NameID**.  
    3.  Klicken Sie zum Hochladen des heruntergeladenen Zertifikats auf **Zscaler Pem**.  
    4.  Aktivieren Sie **Automatische SAML - Bereitstellung**.  

10. Führen Sie auf der **Benutzerauthentifizierung zu konfigurieren** die folgenden Schritte:  

    ![Verwaltung] (./media/active-directory-saas-zscaler-one-tutorial/IC800210.png "Verwaltung")  

    1.  Klicken Sie auf **Speichern**.  
    2.  Klicken Sie auf **jetzt aktivieren**.  

11. Wählen Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ZScaler eine** Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **abgeschlossen**.  

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zscaler-one-tutorial/IC800221.png "Einmaliges Anmelden konfigurieren")  

##<a name="configuring-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>So konfigurieren Sie die Proxyeinstellungen in Internet Explorer

1.  Starten Sie **InternetExplorer**.  

2.  **Wählen Sie aus dem Menü **Extras** das Dialogfeld **Internetoptionen** öffnen.**  

    ![Internetoptionen] (./media/active-directory-saas-zscaler-one-tutorial/IC769492.png "Internetoptionen")  

3.  Klicken Sie auf die Registerkarte **Verbindung** .  

    ![Anschlüsse] (./media/active-directory-saas-zscaler-one-tutorial/IC769493.png "Anschlüsse")  

4.  Klicken Sie auf **LAN Settings** **LAN Settings** Dialog öffnen.  

5.  Im Abschnitt Proxy Server die folgenden Schritte:  

    ![Proxyserver] (./media/active-directory-saas-zscaler-one-tutorial/IC769494.png "Proxyserver")  

    1.  Wählen Sie einen Proxyserver für LAN verwenden.  
    2.  Geben Sie im Textfeld Adresse **gateway.zscalerone.net**.  
    3.  Geben Sie im Textfeld Port **80**.  
    4.  Aktivieren Sie **Proxyserver für lokale Adressen umgehen**.  
    5.  Klicken Sie auf **OK** , um das **lokale Netzwerk (LAN) Settings** Dialogfeld zu schließen.  

6.  Klicken Sie auf **OK** , um das Dialogfeld **Internetoptionen** zu schließen.  

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD Benutzer in ZScaler zu aktivieren, müssen sie einen ZScaler bereitgestellt werden.  
 Bei einem ZScaler ist die Bereitstellung einer manuellen Aufgabe.  

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre wichtigste **Zscaler** .  

2.  Klicken Sie auf **Verwaltung**.  

    ![Verwaltung] (./media/active-directory-saas-zscaler-one-tutorial/IC781035.png "Verwaltung")  

3.  Klicken Sie auf **Benutzer**.  

    ![Hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Hinzufügen")  

4.  Klicken Sie auf der Registerkarte **Benutzer** auf **Hinzufügen**.  

    ![Hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC781037.png "Hinzufügen")  

5.  Führen Sie im Abschnitt Benutzer hinzufügen die folgenden Schritte aus:  

    ![Benutzer hinzufügen] (./media/active-directory-saas-zscaler-one-tutorial/IC781038.png "Benutzer hinzufügen")  

    1.  Geben Sie **UserID**, **Anzeige Benutzername**, **Kennwort**, **Kennwort bestätigen**, und wählen Sie **Gruppen** und **Abteilung** eines gültigen Kontos AAD bereitstellen möchten.  
    2.  Klicken Sie auf **Speichern**.  

>[AZURE.NOTE]Können Sie alle anderen ZScaler ein Benutzer Konto Erstellungstools oder APIs von einer ZScaler Bestimmung AAD Benutzerkonten.  

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.  

###<a name="to-assign-users-to-zscaler-one-perform-the-following-steps"></a>Um Benutzern eine ZScaler zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.  

2.  Der **ZScaler einer** Anwendung Integration klicken Sie auf **Benutzer zuweisen**.  

    ![Benutzer zuweisen] (./media/active-directory-saas-zscaler-one-tutorial/IC800222.png "Benutzer zuweisen")  

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.  

    ![Ja] (./media/active-directory-saas-zscaler-one-tutorial/IC767830.png "Ja")  

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)  
