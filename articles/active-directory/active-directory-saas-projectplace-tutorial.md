<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Projectplace | Microsoft Azure" 
    description="Erfahren Sie, wie mit Projectplace Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Lernprogramm: Azure Active Directory-Integration mit Projectplace
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Projectplace anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Projectplace einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem Projectplace Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Projectplace zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Projectplace
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Szenario")
##<a name="enabling-the-application-integration-for-projectplace"></a>Die Application Integration für Projectplace
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Projectplace aktivieren.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Projectplace:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Projectplace**.

    ![Anwendung Galerie] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Projectplace aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Projectplace Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Projectplace** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Gesamtauthentifizierung konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Projectplace** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Gesamtauthentifizierung konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Projectplace** Ihre ProjectPlace Tenant-URL (z. B.: "*http://company.projectplace.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Projectplace** auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Gesamtauthentifizierung konfigurieren")

5.  Senden der Metadatafile zu Projectplace.

    >[AZURE.NOTE] Die Konfiguration für einzelne Zeichen muss Projectplace Support Team ausgeführt werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Gesamtauthentifizierung konfigurieren] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Gesamtauthentifizierung konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Projectplace anmelden können, müssen sie in Projectplace bereitgestellt werden.  
Bei Projectplace ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **Projectplace** Unternehmen als Administrator an.

2.  Gehe **zu**und klicken dann auf **Mitglieder**.

    ![Personen] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Personen")

3.  Klicken Sie auf **Element hinzufügen**.

    ![Hinzufügen von Mitgliedern] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Hinzufügen von Mitgliedern")

4.  Im Abschnitt **Den** Schritte:

    ![Neue Mitglieder] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Neue Mitglieder")

    1.  Geben Sie im Textfeld **Neue Mitglieder** der e-Mail-adressedes eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Senden**

        >[AZURE.NOTE] Kontoinhaber Azure Active Directory wird eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird gesendet.
    
>[AZURE.NOTE]Können Sie alle anderen Projectplace Benutzer Konto Erstellungstools oder APIs von Projectplace Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Projectplace Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Projectplace **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)