<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Intacct | Microsoft Azure" 
    description="Erfahren Sie, wie mit Intacct in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Lernprogramm: Azure Active Directory-Integration mit Intacct
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Intacct anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Intacct Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Intacct Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Intacct zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Intacct
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Szenario")
##<a name="enabling-the-application-integration-for-intacct"></a>Die Application Integration für Intacct
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Intacct aktivieren.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Intacct:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Intacct**.

    ![Anwendung Galerie] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Intacct aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Intacct mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Intacct** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Intacct** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Intacct** den URL dem folgenden Muster "*https://Intacct.com/company*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Intacct** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Intacct.

6.  Klicken Sie auf **Unternehmen** und dann auf **Unternehmensinformationen**.

    ![Unternehmen] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Unternehmen")

7.  Klicken Sie auf die Registerkarte **Sicherheit** , und klicken Sie dann auf **Bearbeiten**.

    ![Sicherheit] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Sicherheit")

8.  Im Abschnitt **für einmaliges Anmelden (SSO)** die folgenden Schritte:

    ![Einmaliges Anmelden] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Einmaliges Anmelden")

    1.  Wählen Sie **einmaliges aktivieren**.
    2.  Wählen Sie als **Identität Anbietertyp** **SAML 2.0**.
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Intacct** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **URL Aussteller** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Intacct** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Anmelde-URL** .
    5.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    6.  Das Base64-codierte Zertifikat im Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **Zertifikat** Textbox einfügen
    7.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Intacct anmelden können, müssen sie in Intacct bereitgestellt werden.  
Bei Intacct ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Intacct** .

2.  Klicken Sie auf die Registerkarte **Firma** und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Benutzer")

3.  Klicken Sie auf **Hinzufügen**

    ![Hinzufügen] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Hinzufügen")

4.  Im Abschnitt **Benutzerinformationen** Schritte:

    ![Benutzerinformationen] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Benutzerinformationen")

    1.  Geben Sie die **Benutzer-ID**, **Nachname**, **Vorname**, **e-Mail-Adresse**, **Titel** und **Telefon** von Azure Konto in verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie die **Administratorrechte** von Azure Konto, das Sie bereitstellen möchten.
    3.  Klicken Sie auf **Speichern**.
        
        >[AZURE.NOTE] Kontoinhaber AAD e-Mail und einen Link, um ihr Konto bestätigen aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Intacct Benutzer Konto Erstellungstools oder APIs von Intacct Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Intacct:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Intacct **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)