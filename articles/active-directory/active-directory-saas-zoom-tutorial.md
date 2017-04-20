<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Zoom | Microsoft Azure" 
    description="Erfahren Sie, wie mit Zoom Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Lernprogramm: Azure Active Directory-Integration mit Zoom
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Zoom angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Zoom Mieter
  
Am Ende dieses Lernprogramms können Azure AD-Benutzern, Zoom zugewiesen haben, einmaliges in die Website Ihres Unternehmens Zoom (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md) Anwendung
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Zoom aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Szenario")

##<a name="enabling-the-application-integration-for-zoom"></a>Anwendungsintegration für Zoom aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Zoom aktivieren.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Zoom:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Zoom**.

    ![Anwendung Galerie] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Anwendung Galerie")

7.  Im Ergebnisbereich **Zoom**wählen Sie aus und dann auf **vollständig** die Anwendung hinzufügen.

    ![Zoom] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Zoom")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Zoom mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der **Zoom** Application Integrationsseite auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Zoom** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App URL** in das Textfeld **Zoomen Zeichen In URL** den URL dem folgenden Muster "*http://company.zoom.us*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden bei Zoom** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Zoom.

6.  Klicken Sie auf **Single Sign-On** .

    ![Einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Einmaliges Anmelden")

7.  Klicken Sie auf der Registerkarte **Sicherheit** und dann auf den **Einmaliges Anmelden** .

8.  Im Abschnitt Single Sign-On Schritte:

    ![Einmaliges Anmelden] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Einmaliges Anmelden")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei** **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Anmeldung Seiten-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei** **Einzelnen Abmelde Service URL** -Wert und fügen Sie ihn in das Textfeld **URL der Abmeldeseite** .
    3.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    4.  Das Base64-codierte Zertifikat in Editor öffnen und den Inhalt in die Zwischenablage kopieren in **Identity Provider Zertifikat** Textbox einfügen
    5.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    6.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Zoom anmelden können, müssen sie in Zoom bereitgestellt werden.  
Bei Zoom ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihr Unternehmensstandort **Zoom** als Administrator an.

2.  Klicken Sie auf der Registerkarte **Konto** und dann auf **Verwaltung**.

3.  Klicken Sie im Abschnitt Verwaltung auf **Benutzer hinzufügen**.

    ![Verwaltung] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Verwaltung")

4.  Führen Sie die folgenden Schritte auf der Seite **Benutzer hinzufügen** :

    ![Hinzufügen von Benutzern] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Hinzufügen von Benutzern")

    1.  **Als **Typ**auswählen.**
    2.  Geben Sie in das Textfeld **E-Mail** die e-Mail-Adresse eines gültigen Kontos AAD bereitstellen möchten.
    3.  Klicken Sie auf **Hinzufügen**.

>[AZURE.NOTE] Alle anderen Zoom Benutzer Konto Erstellungstools verwenden oder APIs von Zoom bereitstellen AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Um Zoom Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Der **Zoom **Application Integration klicken Sie auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)