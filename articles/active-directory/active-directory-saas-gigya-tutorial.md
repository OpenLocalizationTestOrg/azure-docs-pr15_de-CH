<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Gigya | Microsoft Azure" 
    description="Erfahren Sie, wie mit Gigya Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Lernprogramm: Azure Active Directory-Integration mit Gigya
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Gigya anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Gigya für einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Gigya Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Gigya zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Gigya
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Einmaliges Anmelden konfigurieren")
##<a name="enabling-the-application-integration-for-gigya"></a>Die Application Integration für Gigya
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Gigya aktivieren.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Gigya:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-gigya-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Gigya**.

    ![Anwendung Galerie] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Gigya aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Gigya Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Gigya** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Gigya** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Gigya** den URL dem folgenden Muster "*http://company.gigya.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-gigya-tutorial/IC789530.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Gigya** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Gigya.

6.  Gehen Sie zu **Settings \> SAML-Anmeldung**, und klicken Sie dann auf die Schaltfläche **Hinzufügen** .

    ![SAML-Anmeldung] (./media/active-directory-saas-gigya-tutorial/IC789532.png "SAML-Anmeldung")

7.  Folgendermaßen Sie im Abschnitt **SAML-Anmeldung** an:

    ![SAML-Konfiguration] (./media/active-directory-saas-gigya-tutorial/IC789533.png "SAML-Konfiguration")

    1.  Geben Sie in das Textfeld **Name** einen Namen für Ihre Konfiguration.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Gigya** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Gigya** **Einzelne Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Einzelne Sign-On Service URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Gigya** **Bezeichner Namensformat** Wert und fügen Sie ihn in das Textfeld **Name ID-Format** .
    5.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **X. 509-Zertifikat** .
    7.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Gigya anmelden können, müssen sie in Gigya bereitgestellt werden.  
Bei Gigya ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich der Website Ihres Unternehmens **Gigya** als Administrator an.

2.  Gehen Sie zu **Admin \> Benutzer verwalten**, und klicken Sie dann auf **Benutzer einladen**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Verwalten von Benutzern")

3.  Gehen Sie im Dialogfeld Benutzer einladen:

    ![Benutzer einladen] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Benutzer einladen")

    1.  Geben Sie in das Textfeld **E-Mail** den e-Mail-Alias ein gültiges Azure Active Directory-Konto, das Sie bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer einladen**.
    
        >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail, die enthält einen Link, um das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Gigya Benutzer Konto Erstellungstools oder APIs von Gigya Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Um Gigya Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Gigya **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)