<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Panorama9 | Microsoft Azure" 
    description="Erfahren Sie, wie mit Panorama9 in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Lernprogramm: Azure Active Directory-Integration mit Panorama9
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Panorama9 anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Panorama9 einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Panorama9 Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Panorama9 zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Panorama9
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Szenario")
##<a name="enabling-the-application-integration-for-panorama9"></a>Die Application Integration für Panorama9
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Panorama9 aktivieren.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Panorama9:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Panorama9**.

    ![Anwendung Galerie] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Panorama9 aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Panorama9 mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Panorama9 erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Panorama9** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Panorama9** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Panorama9** den URL Panorama9 Anmelden von Benutzern verwendet (z. B.: "*https://dashboard.panorama9.com/saml/access/3262*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Panorama9** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer speichern.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Panorama9.

6.  Klicken Sie in der oberen Symbolleiste auf **Verwalten**und dann auf **Extensions**.

    ![Extensions] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Extensions")

7.  Klicken Sie im Dialogfeld **Extensions** **- Auf**.

    ![Einmaliges Anmelden] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Einmaliges Anmelden")

8.  In **den Einstellungen** die folgenden Schritte:

    ![Standardeinstellungen] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Standardeinstellungen")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Panorama9** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Provider-URL** .
    2.  Kopieren Sie **Fingerabdruck** Wert des exportierten Zertifikats, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** .  

        >[AZURE.TIP]Weitere Informationen finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI)

    3.  Klicken Sie auf **Speichern**.

9.  Azure AD-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Panorama9 anmelden können, müssen sie in Panorama9 bereitgestellt werden.  
Bei Panorama9 ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **Panorama9** Unternehmen als Administrator an.

2.  Klicken Sie im Menü oben klicken Sie auf **Verwalten**und dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Benutzer")

3.  Klicken Sie auf **+**.

4.  Im Datenabschnitt die folgenden Schritte:

    ![Benutzer] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Benutzer")

    1.  Geben Sie im Feld **E-Mail** die e-Mail-Adresse eines gültigen Benutzers Azure Active Directory bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE]Können Sie alle anderen Panorama9 Benutzer Konto Erstellungstools oder APIs von Panorama9 Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Panorama9:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Panorama9** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)