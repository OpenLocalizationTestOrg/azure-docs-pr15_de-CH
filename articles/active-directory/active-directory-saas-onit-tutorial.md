<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration On | Microsoft Azure" 
    description="Erfahren Sie, wie mit On Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Lernprogramm: Azure Active Directory Integration On
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und On anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein On SSO-Abonnement aktiviert
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem On Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, on zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration On aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-onit-tutorial/IC791166.png "Szenario")
##<a name="enabling-the-application-integration-for-onit"></a>Die Anwendungsintegration On aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration On aktivieren.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration on:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-onit-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-onit-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-onit-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **On**.

    ![Anwendung Galerie] (./media/active-directory-saas-onit-tutorial/IC791167.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **On aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![On] (./media/active-directory-saas-onit-tutorial/IC795325.png "On")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer On Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für On erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).
  
Die On-Anwendung erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss.  
Der folgende Screenshot zeigt ein Beispiel dafür.

![Einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791168.png "Einmaliges Anmelden")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Azure Verwaltungsportal auf **On** Application Integrationsseite im Menü oben klicken Sie auf **Attribute** , um das **SAML-Token Attribute** zu öffnen.

    ![Attribute] (./media/active-directory-saas-onit-tutorial/IC791169.png "Attribute")

2.  Fügen Sie erforderliche Attribut Mappings Schritte:

    
  	|Attributname|Attributwert|
  	|---|---|
  	|Name|User.userPrincipalName|
  	|E-Mail|User.Mail|

    1.  Jede Datenzeile in der Tabelle oben klicken Sie auf **Attribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.
    3.  Wählen Sie aus **Attributwert** Attributwert für die Zeile angezeigt.
    4.  Klicken Sie auf **abgeschlossen**.

3.  Klicken Sie auf **Übernehmen**.

4.  Klicken Sie in Ihrem Browser auf **wieder** **das Dialogfeld** erneut öffnen.

5.  Klicken Sie auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-onit-tutorial/IC791170.png "Einmaliges Anmelden konfigurieren")

6.  Auf der Seite **Wie möchten Sie Benutzer anmelden On** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-onit-tutorial/IC791171.png "Einmaliges Anmelden konfigurieren")

7.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **On Sign URL** die URL Ihrer Anwendung On Anmelden von Benutzern verwendet (z. B.: "*https://ms-sso-test.onit.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-onit-tutorial/IC791172.png "Konfigurieren von App-URL")

8.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden auf On** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-onit-tutorial/IC791173.png "Einmaliges Anmelden konfigurieren")

9.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens On.

10. Klicken Sie im Menü oben auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-onit-tutorial/IC791174.png "Verwaltung")

11. Klicken Sie auf **Edit Corporation**.

    ![Bearbeiten Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Bearbeiten Corporation")

12. Klicken Sie auf die Registerkarte **Sicherheit** .

    ![Firmendaten bearbeiten] (./media/active-directory-saas-onit-tutorial/IC791176.png "Firmendaten bearbeiten")

13. Auf der Registerkarte **Sicherheit** die folgenden Schritte:

    ![Einmaliges Anmelden] (./media/active-directory-saas-onit-tutorial/IC791177.png "Einmaliges Anmelden")

    1.  Wählen Sie als **Authentifizierungsstrategie** **einmaliges Anmelden und das Kennwort**ein.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden auf On** **Remote Login-URL** -Wert und fügen Sie ihn in das Textfeld **Idp-Ziel-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am On** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **Idp Logout URL** .
    4.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Idp Cert Fingerabdruck (SHA1)** .  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    5.  Wählen Sie als **SSO-Typ** **SAML**.
    6.  Geben Sie im Textfeld **SSO anmelden Schaltflächentext** eine Schaltfläche aus.
    7.  Wählen Sie **mit SSO anmelden: die folgenden Domänen/Benutzer**Geben Sie die e-Mail-Adresse eines Testbenutzers in das verknüpfte Textfeld, und klicken Sie auf **Aktualisieren**.
        ![Bearbeiten Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Bearbeiten Corporation")

14. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-onit-tutorial/IC791179.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer On anmelden können, müssen sie in On bereitgestellt werden.  
Bei On ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich auf der Website Ihres Unternehmens **On** als Administrator an.

2.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Verwaltung] (./media/active-directory-saas-onit-tutorial/IC791180.png "Verwaltung")

3.  Auf der **Benutzer hinzufügen** die folgenden Schritte:

    ![Benutzer hinzufügen] (./media/active-directory-saas-onit-tutorial/IC791181.png "Benutzer hinzufügen")

    1.  Geben Sie den **Namen** und die **E-Mail-Adresse** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Erstellen**.  

        >[AZURE.NOTE] Konto erhalten eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen On Benutzer Konto Erstellungstools oder APIs von On Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Um On Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **On **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-onit-tutorial/IC791182.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-onit-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)