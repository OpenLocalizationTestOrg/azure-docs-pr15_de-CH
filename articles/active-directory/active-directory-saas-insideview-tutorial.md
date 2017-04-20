<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration InsideView | Microsoft Azure" 
    description="Erfahren Sie, wie mit InsideView Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Lernprogramm: Azure Active Directory-Integration mit InsideView
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und InsideView anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   InsideView Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der InsideView Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie InsideView zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für InsideView
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Szenario")
##<a name="enabling-the-application-integration-for-insideview"></a>Die Application Integration für InsideView
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für InsideView aktivieren.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für InsideView:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-insideview-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **InsideView**.

    ![Anwendung Galerie] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **InsideView aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer InsideView Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **InsideView** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Gesamtauthentifizierung konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei InsideView** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Gesamtauthentifizierung konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **InsideView Antwort-URL** den URL InsideView SSO (z.B.: `https://my.insideview.com/iv/<STS Name>/login.iv`), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-insideview-tutorial/IC794133.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am InsideView** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Gesamtauthentifizierung konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens InsideView.

6.  In der oberen Symbolleiste auf **Admin** **SingleSignOn-Einstellungen**und dann auf **SAML hinzufügen**.

    ![Einmaliges auf SAML] (./media/active-directory-saas-insideview-tutorial/IC794135.png "Einmaliges auf SAML")

7.  Im Bereich **Hinzufügen ein neues SAML** Schritte:

    ![Hinzufügen einer neuen SAML] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Hinzufügen einer neuen SAML")

    1.  Geben Sie einen Namen für die Konfiguration im Textfeld **STS** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am InsideView** **Service Provider (SP) initiiert Endpunkt** Wert und fügen Sie ihn in das Textfeld **SamlP, WS-Fed Unsolicated Endpunkt** .
    3.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    4.  Das Base64-codierte Zertifikat im Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **STS-Zertifikat** Textbox einfügen
    5.  Geben Sie im Textfeld **Crm Benutzer Id zuordnen** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  Geben Sie im Textfeld **Crm e-Mail-Zuordnung** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Geben Sie im Textfeld **Crm Vorname Zuordnung** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  Geben Sie im Textfeld **Nachname Crm zuordnen** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Gesamtauthentifizierung konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer InsideView anmelden können, müssen sie in InsideView bereitgestellt werden.  
Bei InsideView ist die Bereitstellung einer manuellen Aufgabe.
  
Benutzer oder Kontakte erstellt InsideView wenden Sie sich an Ihren Kundenmanager Erfolg oder e-Mail**support@insideview.com**

>[AZURE.NOTE] Können Sie alle anderen InsideView Benutzer Konto Erstellungstools oder APIs von InsideView Bereitstellung Azure AD-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Um InsideView Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **InsideView **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)