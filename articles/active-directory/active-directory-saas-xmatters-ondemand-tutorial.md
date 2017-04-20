<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit xMatters OnDemand | Microsoft Azure"
    description="Erfahren Sie, wie mit xMatters OnDemand Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Lernprogramm: Azure Active Directory-Integration mit xMatters OnDemand
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und xMatters OnDemand anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   XMatters OnDemand Mieter
  
Nach diesem Lernprogramm werden einmaliges in die Anwendung xMatters OnDemand der Website Ihres Unternehmens (Service Provider initiiert anmelden), oder die [Einführung in die Abdeckung](active-directory-saas-access-panel-introduction.md)in Azure AD-Benutzern, xMatters OnDemand zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für xMatters OnDemand
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Szenario")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Die Application Integration für xMatters OnDemand
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration xMatters OnDemand aktivieren.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration xMatters OnDemand:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **xMatters OnDemand**.

    ![Anwendung Galerie] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **XMatters OnDemand aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![xMatters OnDemand] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer XMatters OnDemand Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **XMatters OnDemand** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzern, XMatters OnDemand anzumelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Einmaliges Anmelden für konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von app-URL] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Konfigurieren von app-URL")

    ein. Geben Sie im Textfeld **XMatters OnDemand Zeichen In URL** den URL dem folgenden Muster:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Klicken Sie auf **Weiter**.


4.  Auf der Seite **Konfigurieren einmaliges Anmelden am XMatters OnDemand** herunterladen Sie das Zertifikat auf **Zertifikat herunterladen**und speichern Sie die Zertifikatsdatei lokal als **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Sie müssen das Zertifikat an das xMatters weitergeleitet. Das Zertifikat muss vom Team Unterstützung xMatters hochgeladen werden, bevor Sie die Konfiguration für einzelne Zeichen abschließen können.

    ![Konfigurieren der einzelnen anmelden] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Konfigurieren der einzelnen anmelden")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens XMatters OnDemand.

6.  In der oberen Symbolleiste auf **Admin**und klicken Sie **Daten** in der Navigationsleiste auf der linken Seite.

    ![Admin] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

7.  Führen Sie auf der Seite **SAML-Konfiguration** die folgenden Schritte:

    ![SAML-Konfiguration] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-Konfiguration")

    1.  Aktivieren Sie **SAML**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am XMatters OnDemand** **Identität Anbieter-ID** -Wert und fügen Sie ihn in das Textfeld **Identität Anbieter-ID** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am XMatters OnDemand** **Einzelne Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Einzelne Anmelde-URL** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am XMatters OnDemand** **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **Einzelne Logout URL** .
    5.  Die Daten oben klicken Sie auf **Speichern**.
        ![Firmendaten] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Firmendaten")

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Konfigurieren der einzelnen anmelden] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Konfigurieren der einzelnen anmelden")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer XMatters OnDemand anmelden können, müssen sie in XMatters OnDemand bereitgestellt werden.  
Bei XMatters OnDemand ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **XMatters OnDemand** .

2.  Klicken Sie auf die Registerkarte **Benutzer** .

3.  Klicken Sie auf **Benutzer hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Benutzer")

4.  Wählen Sie **aktiv**.

5.  Führen Sie im Abschnitt **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Hinzufügen eines Benutzers")

    1.  Geben Sie **UserID**, **Vorname**, **Nachname**, **Site** ein gültiges Konto AAD bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen XMatters OnDemand Benutzer Konto Erstellungstools oder APIs von XMatters OnDemand Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Um XMatters OnDemand Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **XMatters OnDemand **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)