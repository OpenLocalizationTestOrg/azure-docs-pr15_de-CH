<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration ITRP | Microsoft Azure" 
    description="Erfahren Sie, wie mit ITRP Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Lernprogramm: Azure Active Directory-Integration mit ITRP
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und ITRP anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   ITRP Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der ITRP Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie ITRP zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für ITRP
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Szenario")
##<a name="enabling-the-application-integration-for-itrp"></a>Die Application Integration für ITRP
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration ITRP aktivieren.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ITRP:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **ITRP**.

    ![Anwendung Galerie] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **ITRP aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer ITRP Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für ITRP erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **ITRP** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ITRP** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **ITRP Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. ITRP.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am ITRP** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\ITRP.cer**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens ITRP.

6.  **Klicken Sie auf der Symbolleiste im oberen Bereich.**

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Wählen Sie im linken Navigationsbereich **Einmaliges Anmelden**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Einmaliges Anmelden")

8.  Im Konfigurationsabschnitt Single Sign-On Schritte:

    ![Einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Einmaliges Anmelden")

    ![Einmaliges Anmelden] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Einmaliges Anmelden")

    1.  Klicken Sie auf **Aktivieren**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ITRP** Wert **Remote Logout-URL** und fügen Sie ihn in das Textfeld **Remote Logout-URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ITRP** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **SAML SSO-URL** .
    4.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** .
        
        >[AZURE.TIP]Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    5.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer ITRP anmelden können, müssen sie in ITRP bereitgestellt werden.  
Bei ITRP ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **ITRP** .

2.  Klicken Sie auf **Datensätze**in der oberen Symbolleiste.

    ![Admin] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Admin")

3.  Wählen Sie im Einblendmenü **Personen**.

    ![Personen] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Personen")

4.  Klicken Sie auf **neue Person hinzufügen** ("+").

    ![Admin] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Admin")

5.  Gehen Sie im Dialogfeld neue Person hinzufügen:

    ![Benutzer] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Benutzer")

    1.  Geben Sie den **Namen**, **E-Mail** eines gültigen Kontos AAD bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen ITRP Benutzer Konto Erstellungstools oder APIs von ITRP Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Um ITRP Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in Azure AD-Portal.

2.  Klicken Sie auf der Seite **ITRP **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)