<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit TimeOffManager | Microsoft Azure" 
    description="Erfahren Sie, wie mit TimeOffManager in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Lernprogramm: Azure Active Directory-Integration mit TimeOffManager
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TimeOffManager anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein TimeOffManager einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der TimeOffManager Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die TimeOffManager zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für TimeOffManager
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Szenario")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>Die Application Integration für TimeOffManager
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für TimeOffManager aktivieren.

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für TimeOffManager:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **TimeOffManager**.

    ![Anwendung Galerie] (./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **TimeOffManager aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![TimeOffManager] (./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer TimeOffManager mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat Ihrem Mandanten TimeOffManager hochladen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **TimeOffManager** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei TimeOffManager** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **TimeOffManager Antwort-URL** den URL TimeOffManager AssertionConsumerService (z. B.: "*Beispiel: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_Id = IC34216*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "Konfigurieren von App-URL")

    Sie erhalten die Antwort-URL von TimeOffManager Einmalanmeldung Seite.

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Einstellung für einzelne Zeichen")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am TimeOffManager** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens TimeOffManager.

6.  Gehen Sie zu **Konto \> Konto Optionen \> Single Sign-On Einstellungen**.

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Einstellung für einzelne Zeichen")

7.  Im Abschnitt **Einstellungen für einzelne Zeichen** folgendermaßen:

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Einstellung für einzelne Zeichen")

    ein.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    b.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie das gesamte Zertifikat in **X. 509-Zertifikat** Textbox.
    
    c.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am TimeOffManager** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Idp Aussteller** .
    
    d.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am TimeOffManager** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **IdP-Endpunkt-URL** .
    
    e.  Wählen Sie **Nein**, als **SAML erzwingen**.
    

    f.  Wählen Sie **Ja**, **Benutzer automatisch erstellen**.
    
    g.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am TimeOffManager** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    
    h.  Klicken Sie auf **Speichern**.

8.  Wählen Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am TimeOffManager** die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie auf **vollständig**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Einmaliges Anmelden konfigurieren")

9.  Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen.

    ![Attribute] (./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Attribute")

10. Fügen Sie erforderliche Attribut Mappings Schritte:

    ![SAML-token-Attribute] (./media/active-directory-saas-timeoffmanager-tutorial/123.png "SAML-token-Attribute")

  	|Attributname|Attributwert|
  	|---|---|
  	|E-Mail|User.Mail|
  	|Vorname|User.GivenName|
  	|Nachname|User.Surname|

    ein.  Jede Datenzeile in der Tabelle oben klicken Sie auf **Attribut hinzufügen**.

    b.  Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.

    c.  Wählen Sie im Textfeld **Attributwert** Attributwert für die Zeile angezeigt.

    d.  Klicken Sie auf **abgeschlossen**.

11. Klicken Sie auf **Übernehmen**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD TimeOffManager anmelden können, müssen sie TimeOffManager bereitgestellt werden.  
TimeOffManager unterstützt nur Zeit Benutzer bereitstellen. Es ist keine Aufgabe für Sie.  
Die Benutzer werden automatisch während der ersten Anmeldung über einmaliges hinzugefügt.

>[AZURE.NOTE] Können Sie alle anderen TimeOffManager Benutzer Konto Erstellungstools oder APIs von TimeOffManager Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu TimeOffManager:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **TimeOffManager** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
