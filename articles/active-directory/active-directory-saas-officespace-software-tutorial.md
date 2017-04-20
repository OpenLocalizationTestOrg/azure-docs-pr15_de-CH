<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Büroflächen Software | Microsoft Azure" 
    description="Erfahren Sie, wie mit Büroflächen Software mit Active Directory Azure-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Lernprogramm: Azure Active Directory Integration Büroflächen Software
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Büroflächen Software anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Büroflächen Software SSO-Abonnement aktiviert
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Büroflächen Software Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Büroflächen Software zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration Büroflächen Software
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Szenario")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Die Application Integration Büroflächen Software
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Büroflächen Software aktivieren.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Büroflächen Software:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Büroflächen Software**.

    ![Anwendung Galerie] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Büroflächen Software aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Büroflächen Software] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "Büroflächen Software")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Büroflächen Software mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Einmaliges Anmelden für Büroflächen Software konfigurieren muss Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Büroflächen Software** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges konfigurieren auf =] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Einmaliges konfigurieren auf =")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Büroflächen Software** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Büroflächen Software Anmelde-URL** die URL Ihrer Anwendung Büroflächen Software Anmelden von Benutzern verwendet (z. B.: "*https://company.officespacesoftware.com*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Büroflächen Software** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Büroflächen Software.

6.  Gehen Sie zu **Admin \> Connectors**.

    ![Admin] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Admin")

7.  Klicken Sie auf **SAML-Autorisierung**.

    ![Anschlüsse] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Anschlüsse")

8.  Im Abschnitt **Authorization SAML** Schritte:

    ![SAML-Konfiguration] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "SAML-Konfiguration")

    1.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Büroflächen Software** **Remote Login-URL** -Wert und fügen Sie ihn in das Textfeld **Logout Provider-Url** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Büroflächen Software** **Remote Logout URL** -Wert und fügen Sie ihn in das Textfeld **Client idp Ziel-Url** .
    3.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Client idp Cert Fingerabdruck** .  

        >[AZURE.TIP]
        Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    4.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Büroflächen Software anmelden können, müssen sie in Büroflächen Software bereitgestellt werden. Bei Büroflächen Software ist die Bereitstellung einer automatisierten Aufgabe.  
Es ist keine Aufgabe für Sie.  
Benutzer werden bei Bedarf automatisch beim ersten single Sign-On erstellt.

>[AZURE.NOTE]Können Sie alle anderen Büroflächen Benutzer Konto erstellen Softwaretools oder APIs von Büroflächen Software Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Büroflächen Software:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Büroflächen Software **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)