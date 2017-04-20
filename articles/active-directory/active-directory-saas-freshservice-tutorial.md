<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit FreshService | Microsoft Azure" 
    description="Erfahren Sie, wie mit FreshService in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Lernprogramm: Azure Active Directory-Integration mit FreshService
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und FreshService anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein FreshService einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können sich mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Anwendung für einmaliges Azure AD-Benutzer, die FreshService zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für FreshService
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Szenario")
##<a name="enabling-the-application-integration-for-freshservice"></a>Die Application Integration für FreshService
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für FreshService aktivieren.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für FreshService:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **FreshService**.

    ![Anwendung Galerie] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **FreshService aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer FreshService mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für FreshService erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **FreshService** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei FreshService** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **FreshService Zeichen URL** den URL der Anwendung Freshdesk Anmelden von Benutzern verwendet (z. B.: "*http://democompany.freshservice.com/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am FreshService** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens FreshService.

6.  Klicken Sie im Menü oben auf **Administration**.

    ![Admin] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Admin")

7.  Klicken Sie im **Kundenportal**auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Sicherheit")

8.  Führen Sie die folgenden Schritte im Bereich **Sicherheit** :

    ![Einmaliges Anmelden] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Einmaliges Anmelden")

    1.  Wechseln Sie **einmaliges OnON**
    2.  **SAML-SSO**auswählen.
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am FreshService** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **SAML Anmelde-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am FreshService** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Sicherheit Zertifikat Fingerabdruck** .
    
        >[AZURE.TIP]Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer FreshService anmelden können, müssen sie in FreshService bereitgestellt werden.  
Bei FreshService ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **FreshService** Unternehmen als Administrator an.

2.  Klicken Sie im Menü oben auf **Administration**.

    ![Admin] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Admin")

3.  Klicken Sie im Abschnitt **Verwaltung** auf **anfordern**.

    ![Antragsteller] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Antragsteller")

4.  Klicken Sie auf **neue Antragsteller**.

    ![Neue anfordern] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Neue anfordern")

5.  Im Abschnitt **Neue Antragsteller** Schritte:

    ![Neue Antragsteller] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Neue Antragsteller")

    1.  Geben Sie **Namen** und **e-Mail-** Attribute eines gültigen Azure Active Directory-Kontos gewünschte Bereitstellung in verknüpfte Textfelder.
    2.  Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird

>[AZURE.NOTE] Können Sie alle anderen FreshService Benutzer Konto Erstellungstools oder APIs von FreshService Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu FreshService:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **FreshService **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)