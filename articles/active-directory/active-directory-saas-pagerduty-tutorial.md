<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Pagerduty | Microsoft Azure" 
    description="Erfahren Sie, wie mit Pagerduty in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Lernprogramm: Azure Active Directory-Integration mit Pagerduty
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Pagerduty anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Pagerduty Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Pagerduty Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Pagerduty zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Pagerduty
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Szenario")
##<a name="enabling-the-application-integration-for-pagerduty"></a>Die Application Integration für Pagerduty
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Pagerduty aktivieren.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Pagerduty:

1.  Klicken Sie in der Azure-Verwaltungsportal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Pagerduty**.

    ![Anwendung Galerie] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Pagerduty aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Pagerduty mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Pagerduty** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Pagerduty** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Pagerduty Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Pagerduty.com*", und klicken Sie dann auf **Weiter**.

    ![App-Url konfigurieren] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "App-Url konfigurieren")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Pagerduty** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Pagerduty.

6.  Klicken Sie im Menü oben auf **Konto**.

    ![Konto] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Konto")

7.  Klicken Sie auf **Einmalanmeldung**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Einmaliges Anmelden")

8.  Auf der Seite aktivieren Sie einmaliges Anmelden (SSO) die folgenden Schritte:

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Einmaliges Anmelden aktivieren")

    1.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    2.  Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **X. 509-Zertifikat**
    3.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am Pagerduty** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am Pagerduty** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Aktivieren Sie das Kontrollkästchen Sie **für einmaliges Anmelden**.
    6.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Pagerduty anmelden können, müssen sie in Pagerduty bereitgestellt werden.  
Bei Pagerduty ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Pagerduty** .

2.  Klicken Sie im Menü oben auf **Benutzer**.

3.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Hinzufügen von Benutzern")

4.  Geben Sie im Dialogfeld **Einladung das Team** **vor- und Nachnamen** und die **e-Mail-** Adresse der Azure AD-Benutzer bereitstellen und auf **Hinzufügen**klicken, **Lädt senden**möchten.

    ![Laden Sie Ihr team] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Laden Sie Ihr team")

    >[AZURE.NOTE] Alle hinzugefügten Benutzer erhalten eine Einladung, ein PagerDuty-Konto erstellen.

>[AZURE.NOTE] Können Sie alle anderen Pagerduty Benutzer Konto Erstellungstools oder APIs von Pagerduty Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Pagerduty:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Pagerduty **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)