<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit LMS | Microsoft Azure" 
    description="Erfahren Sie, wie mit Canvas LMS Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Lernprogramm: Azure Active Directory-Integration mit LMS

Das Ziel dieses Lernprogramms ist die Integration von Azure und Leinwand angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Canvas-Mandanten

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an die Leinwand Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Leinwand zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Canvas
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Szenario")
##<a name="enabling-the-application-integration-for-canvas"></a>Die Application Integration für Canvas

Dieser Abschnitt soll beschreiben die Anwendungsintegration Leinwand aktivieren.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Leinwand:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Canvas**.

    ![Anwendung Galerie] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Leinwand aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Leinwand] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Leinwand")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Canvas mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Canvas erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der **Leinwand** Integration Anwendungsseite **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden auf Leinwand** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Leinwand Zeichen In URL** den URL dem folgenden Muster `https://<tenant-name>.instructure.com`, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden auf** Ihr Zertifikat herunterladen auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Leinwand Unternehmenswebsite.

6.  Gehen Sie zu **Kurse \> Konten \> Microsoft**.

    ![Leinwand] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Leinwand")

7.  Klicken Sie im Navigationsbereich auf der linken Seite Wählen Sie **Authentifizierung aus**und dann auf **Neue SAML-Konfiguration hinzufügen**.

    ![Authentifizierung] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentifizierung")

8.  Auf der Seite aktuelle Integration Schritte:

    ![Aktuelle Integration] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Aktuelle Integration")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden auf** **Entitäts-ID** -Wert und fügen Sie ihn in das Textfeld **IdP Entitäts-ID** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden auf** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **Protokoll-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden auf** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **, Protokoll-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden auf** Wert **Ändern Kennwort URL** und fügen Sie ihn in das Textfeld **Kennwort Verknüpfung ändern** .
    5.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** .  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    6.  Wählen Sie aus der Liste **Benutzername Attribut** **NameID**.
    7.  Wählen Sie aus der Liste **Format Bezeichner** **EmailAddress**.
    8.  Klicken Sie auf **Authentifizierung**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Leinwand anmelden können, müssen sie in Canvas bereitgestellt werden.  
Bei Leinwand ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **Canvas** .

2.  Gehen Sie zu **Kurse \> Konten \> Microsoft**.

    ![Leinwand] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Leinwand")

3.  Klicken Sie auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Benutzer")

4.  Klicken Sie auf **neuen Benutzer hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Benutzer")

5.  Gehen Sie auf der Seite Dialogfeld neuen Benutzer hinzufügen:

    ![Benutzer hinzufügen] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Benutzer hinzufügen")

    1.  Geben Sie im Feld **Vollständiger Name** den Namen des Benutzers.
    2.  Geben Sie im Feld **E-Mail** die e-Mail-Adresse.
    3.  Geben Sie im Textfeld **Benutzername** Azure AD e-Mail-Adresse des Benutzers ein.
    4.  Wählen Sie **E-Mail-Benutzer über dieses Konto erstellen**.
    5.  Klicken Sie auf **Benutzer hinzufügen**.

>[AZURE.NOTE] Alle anderen Leinwand Benutzer Konto Erstellungstools verwenden oder APIs von Canvas Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Um Leinwand Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der **Leinwand **Integration Anwendungsseite auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Zuweisen von Benutzern")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
