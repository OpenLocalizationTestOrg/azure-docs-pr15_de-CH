<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit TeamSeer | Microsoft Azure" 
    description="Erfahren Sie, wie mit TeamSeer in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Lernprogramm: Azure Active Directory-Integration mit TeamSeer
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und TeamSeer anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   TeamSeer Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der TeamSeer Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die TeamSeer zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für TeamSeer
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Szenario")

##<a name="enabling-the-application-integration-for-teamseer"></a>Die Application Integration für TeamSeer
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für TeamSeer aktivieren.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für TeamSeer:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **TeamSeer**.

    ![Anwendung Galerie] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **TeamSeer aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer TeamSeer mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **TeamSeer** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei TeamSeer** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App URL** in das Textfeld **TeamSeer Zeichen In URL** den URL dem folgenden Muster "*http://www.teamseer.com/companyid*" und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am TeamSeer** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens TeamSeer.

6.  Gehen Sie auf **HR**.

    ![HR-Verwaltung] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR-Verwaltung")

7.  Klicken Sie auf **Setup**.

    ![Setup] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Setup")

8.  Klicken Sie auf **SAML-Anbieter Informationen einrichten**.

    ![SAML-Einstellung] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "SAML-Einstellung")

9.  Im Detailbereich SAML-Anbieter die folgenden Schritte:

    ![SAML-Einstellung] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "SAML-Einstellung")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am TeamSeer** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **URL** .
    2.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    3.  Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **IdP öffentliches Zertifikat** Textbox einfügen.

10. Zum Abschließen der Anbieterkonfiguration SAML Schritte:

    ![SAML-Einstellung] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "SAML-Einstellung")

    1.  **Testen Sie e-Mail-Adressen**Geben Sie die Testbenutzer e-Mail-Adresse.
    2.  Geben Sie im Textfeld **Aussteller** Aussteller URL des Dienstanbieters.
    3.  Klicken Sie auf **Speichern**.

11. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD TeamSeer anmelden können, müssen sie in ShiftPlanning bereitgestellt werden.  
Bei TeamSeer ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **TeamSeer** Unternehmen als Administrator an.

2.  Führen Sie die folgenden Schritte aus:

    ![HR-Verwaltung] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR-Verwaltung")

    1.  Gehen Sie zu **HR Admin \> Benutzer**.
    2.  Klicken Sie auf **den neuen Assistenten**.

3.  Folgendermaßen Sie im Detailbereich **Benutzer** an:

    ![Benutzerinformationen] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Benutzerinformationen")

    1.  Geben Sie den **Vornamen**, **Nachnamen**, **Benutzernamen (e-Mail-Adresse)** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Weiter**.

4.  Gehen Sie auf Bildschirm für einen neuen Benutzer hinzufügen, und klicken Sie auf **Fertig stellen**.

>[AZURE.NOTE] Können Sie alle anderen TeamSeer Benutzer Konto Erstellungstools oder APIs von TeamSeer Bereitstellung Azure AD-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu TeamSeer:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **TeamSeer **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)