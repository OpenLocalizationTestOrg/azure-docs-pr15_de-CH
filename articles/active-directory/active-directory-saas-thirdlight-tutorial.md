<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Thirdlight | Microsoft Azure" 
    description="Erfahren Sie, wie mit Thirdlight in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Lernprogramm: Azure Active Directory-Integration mit Thirdlight
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Thirdlight anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Thirdlight einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Thirdlight Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Thirdlight zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Thirdlight
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Szenario")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Die Application Integration für Thirdlight
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Thirdlight aktivieren.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Thirdlight:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Thirdlight**.

    ![Anwendung Galerie] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Thirdlight aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Thirdlight mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Thirdlight erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Thirdlight** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Thirdlight** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Thirdlight Zeichen In URL** den URL der Anwendung Thirdlight Anmelden von Benutzern verwendet (z. B.: "*http://azuresso2.thirdlight.com/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden unter Thirdlight** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Thirdlight.

6.  Gehen Sie zu **Konfiguration \> System Administration**, und klicken Sie dann auf **SAML2**.

    ![Systemadministration] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Systemadministration")

7.  Führen Sie im Konfigurationsabschnitt SAML2 die folgenden Schritte aus:

    ![SAML einmaliges Anmelden] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML einmaliges Anmelden")

    1.  **SAML2 einmaliges**aktivieren Sie möchten.
    2.  Wählen Sie als **Quelle für IdP Metadaten** **Laden IdP Metadaten aus XML**.
    3.  Öffnen Sie heruntergeladene Metadaten-Datei, kopieren Sie den Inhalt und fügen Sie ihn in das Textfeld **IdP Metadaten XML**
    4.  Klicken Sie auf **SAML2 speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Thirdlight anmelden können, müssen sie in Thirdlight bereitgestellt werden.  
Bei Thirdlight ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **Thirdlight** Unternehmen als Administrator an.

2.  Wechseln Sie zur Registerkarte **Benutzer** .

3.  Wählen Sie **Benutzer und Gruppen**.

4.  Klicken Sie auf **neuen Benutzer hinzufügen** .

5.  Geben Sie **Benutzername, Name oder Beschreibung, E-Mail, wählen Sie eine Voreinstellung oder Gruppe neue Mitglieder** eines gültigen Kontos AAD gewünschte bereitstellen.

6.  Klicken Sie auf **Erstellen**.

>[AZURE.NOTE] Können Sie alle anderen Thirdlight Benutzer Konto Erstellungstools oder APIs von Thirdlight Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Thirdlight:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Thirdlight **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)