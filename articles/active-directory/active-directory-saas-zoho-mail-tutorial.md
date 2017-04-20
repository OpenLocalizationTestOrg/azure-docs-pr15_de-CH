<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Zoho Mail | Microsoft Azure" 
    description="Erfahren Sie, wie mit Zoho Mail Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Lernprogramm: Azure Active Directory-Integration mit Zoho Mail
  
Ziel dieses Lernprogramms ist die Integration von Azure und Zoho Mail anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Zoho Mail Mieter
  
Nach diesem Lernprogramm werden können einzelne Zeichen in der Anwendung an Ihrem Zoho Mail-Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Zoho Mail zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Zoho Mail aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Szenario")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Anwendungsintegration für Zoho Mail aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Zoho Mail aktivieren.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Zoho Nachrichten:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Zoho Mail**.

    ![Anwendung Galerie] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Zoho Mail aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Zoho-Mail] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho-Mail")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Zoho e-Mail-Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Zoho Mail** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer Zoho Mail anmelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Einmaliges Anmelden konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Konfigurieren von App-URL")

    ein. Geben Sie im Textfeld **Zoho Mail Anmelde-URL** den URL dem folgenden Muster:`http://<company name>.ZohoMail.com`

    b. Klicken Sie auf **Weiter**.


4.  Auf der Seite **Konfigurieren einmaliges Zoho Mail** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Zoho Mail.

6.  Wechseln Sie in das **Bedienfeld**.

    ![Bedienfeld] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Bedienfeld")

7.  Klicken Sie auf die Registerkarte **SAML-Authentifizierung** .

    ![SAML-Authentifizierung] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML-Authentifizierung")

8.  Im Abschnitt **SAML Authentifizierungsdetails** Schritte:

    ![SAML Authentifizierungsdetails] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML Authentifizierungsdetails")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Zoho Mail** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Zoho Mail** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Zoho Mail** **Ändern Kennwort URL** -Wert und fügen Sie ihn in das Textfeld **Kennwort URL ändern** .
    4.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    5.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **PublicKey**
    6.  Wählen Sie als **Algorithmus** **RSA**.
    7.  Klicken Sie auf **OK**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Zoho Mail anmelden können, müssen sie in Zoho Mail bereitgestellt werden.  
Bei Zoho Mail ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich auf der Website Ihres Unternehmens **Zoho Mail** als Administrator.

2.  Gehen Sie zu **Bedienfeld \> Nachrichten und Dokumente**.

3.  Gehen Sie zu **Benutzerdetails \> Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Benutzer hinzufügen")

4.  Gehen Sie im Dialogfeld **Benutzer hinzufügen** :

    ![Benutzer hinzufügen] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Benutzer hinzufügen")

    1.  Geben Sie **Vorname**, **Nachname**, **E-Mail-ID**, **Kennwort** für ein gültiges Azure Active Directory-Konto in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **OK**.  

        >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten Sie eine e-Mail mit einem Link zu das Konto bestätigen aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Zoho E-Mail Benutzer Konto Erstellungstools oder APIs von Zoho e-Mail-Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Zoho Mail-Benutzer zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Zoho Mail **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)