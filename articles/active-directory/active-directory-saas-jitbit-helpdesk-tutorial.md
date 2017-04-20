<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Jitbit Helpdesk | Microsoft Azure" 
    description="Erfahren Sie, wie mit Jitbit Helpdesk Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Lernprogramm: Azure Active Directory Integration Jitbit Helpdesk
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Jitbit Helpdesk anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Jitbit Helpdesk Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem Jitbit Helpdesk Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Jitbit Helpdesk zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration Jitbit Helpdesk
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Szenario")
##<a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Die Application Integration Jitbit Helpdesk
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Jitbit Helpdesk aktivieren.

###<a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Jitbit Helpdesk:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Jitbit Helpdesk**.

    ![Anwendung Galerie] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Jitbit Helpdesk aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![JitBit] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Jitbit Helpdesk mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können. .  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

>[AZURE.IMPORTANT] Um auf Ihrem Mandanten Jitbit Helpdesk einmaliges Anmelden konfigurieren können, müssen Sie zu zunächst die Jitbit Helpdesk Support zu dieser Funktion.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **Jitbit Helpdesk** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Jitbit Helpdesk** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Jitbit Helpdesk Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Jitbit.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Konfigurieren von app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Jitbit Helpdesk** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\Jitbit Helpdesk.cer**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Jitbit Helpdesk.

6.  Klicken Sie in der oberen Symbolleiste auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Verwaltung")

7.  Klicken Sie auf **Allgemein**.

    ![Benutzer, Unternehmen und Berechtigungen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Benutzer, Unternehmen und Berechtigungen")

8.  Im Konfigurationsabschnitt **Authentifizierung** die folgenden Schritte:

    ![Authentifizierung] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Authentifizierung")

    1.  Wählen Sie **Aktivieren SAML 2.0 einzelne anmelden** Anmeldung **OneLogin**für einmaliges Anmelden (SSO) mit.
    2.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am Jitbit Helpdesk** **Service Provider (SP) initiiert Endpunkt** Wert und fügen Sie ihn in das Textfeld **Endpunkt-URL** .
    3.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.
        
        >[AZURE.TIP]Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    4.  Öffnen Sie das Base64-codierte Zertifikat, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **X. 509-Zertifikat**
    5.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD Benutzer Jitbit Helpdesk anmelden können, müssen sie in Jitbit Helpdesk bereitgestellt werden.  
Bei Jitbit Helpdesk ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Jitbit Helpdesk** .

2.  Klicken Sie im Menü oben auf **Verwaltung**.

    ![Verwaltung] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Verwaltung")

3.  Klicken Sie auf **Benutzer, Unternehmen und Berechtigungen**.

    ![Benutzer, Unternehmen und Berechtigungen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Benutzer, Unternehmen und Berechtigungen")

4.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Benutzer hinzufügen")

5.  Im Abschnitt Erstellen der Datentyp des Azure AD Bereitstellung in die folgenden Textfelder soll: **Benutzername**, **E-Mail**, **Vorname**, **Nachname**

    ![Erstellen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Erstellen")

6.  Klicken Sie auf **Erstellen**.

>[AZURE.NOTE] Können Sie alle anderen Jitbit Helpdesk Benutzer Konto Erstellungstools oder APIs von Jitbit Helpdesk Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>Um Jitbit Helpdesk Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Jitbit Helpdesk **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)