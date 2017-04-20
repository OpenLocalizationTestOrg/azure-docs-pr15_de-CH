<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Cisco Webex | Microsoft Azure" 
    description="Erfahren Sie, wie mit Cisco Webex Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Lernprogramm: Azure Active Directory-Integration mit Cisco Webex

Das Ziel dieses Lernprogramms ist die Integration von Azure und Cisco Webex angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Cisco Webex Mieter

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Cisco Webex Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Cisco Webex zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Cisco Webex
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Szenario")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>Die Application Integration für Cisco Webex

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Cisco Webex aktivieren.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Cisco Webex:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Cisco Webex**.

    ![Anwendung Galerie] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Cisco Webex aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Cisco Webex Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Cisco Webex** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Cisco Webex** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Einmaliges Anmelden für konfigurieren")

3.  Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Konfigurieren von app-URL")

    1.  Geben Sie im Textfeld **URL singen** den Cisco Webex Mieter URL (z. B.: *http://contoso.webex.com*).
    2.  Geben Sie im Textfeld **Cisco Webex-Antwort-URL** den **Cisco Webex AssertionConsumerService URL** (z. B.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Cisco Webex** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Cisco Webex.

6.  Klicken Sie im Menü oben auf **Site-Verwaltung**.

    ![Site-Verwaltung] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Site-Verwaltung")

7.  Klicken Sie im Abschnitt **Site verwalten** auf **SSO-Konfiguration**.

    ![SSO-Konfiguration] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-Konfiguration")

8.  Im Abschnitt verbundene Webkonfiguration SSO-Schritte:

    ![Verbundene SSO-Konfiguration] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Verbundene SSO-Konfiguration")

    1.  Wählen Sie die **Föderation** Protokollliste **SAML 2.0**.
    2.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    3.  Das Base64-codierte Zertifikat im Editor öffnen und den Inhalt kopieren.
    4.  Klicken Sie auf **Die SAML Metadaten importieren**, und fügen Sie das Base64-codierte Zertifikat.
    5.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Cisco Webex** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller für SAML (IdP-ID)** .
    6.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Cisco Webex** **Remote Login-URL** -Wert und fügen Sie ihn in das Textfeld **Kunden SSO-Service Anmelde-URL** .
    7.  Wählen Sie aus der Liste **NameID Format** **e-Mail-Adresse**.
    8.  Geben Sie im Textfeld **AuthnContextClassRef** **Urn: Oasis: Namen: Tc: SAML:2.0:ac:classes:Password**.
    9.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Cisco Webex** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL für Kunden SSO Abmelden** .
    10. Klicken Sie auf **Aktualisieren**.

9.  Wählen Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Cisco Webex** Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **abgeschlossen**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Cisco Webex anmelden können, müssen sie in Cisco Webex bereitgestellt werden.  
Bei Cisco Webex ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem **Cisco Webex** -Mandanten.

2.  Gehen Sie zu **Benutzer verwalten \> Benutzer hinzufügen**.

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Hinzufügen von Benutzern")

3.  Gehen Sie auf Benutzer hinzufügen:

    ![Benutzer hinzufügen] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Benutzer hinzufügen")

    1.  Wählen Sie als **Kontoart** **Host**.
    2.  Geben Sie die Informationen eines vorhandenen Benutzers Azure AD in die folgenden Textfelder: **Vorname, Nachname**, **Benutzername**, **E-Mail**, **Kennwort**, **Kennwort bestätigen**.
    3.  Klicken Sie auf **Hinzufügen**.

>[AZURE.NOTE] Alle anderen Cisco Webex Benutzer Konto Erstellungstools verwenden oder APIs von Cisco Webex Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Cisco Webex Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Cisco Webex **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
