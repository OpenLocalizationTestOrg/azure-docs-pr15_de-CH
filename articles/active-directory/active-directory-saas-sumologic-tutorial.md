<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit SumoLogic | Microsoft Azure" 
    description="Erfahren Sie, wie mit SumoLogic in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Lernprogramm: Azure Active Directory-Integration mit SumoLogic
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SumoLogic anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   SumoLogic Mieter
  
Nach Abschluss dieses Tutorials, Unternehmen Sie SumoLogicwill zugewiesen haben können einzelne Zeichen in der Anwendung an Ihrem SumoLogic Azure AD-Benutzer Site (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für SumoLogic
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Szenario")

##<a name="enabling-the-application-integration-for-sumologic"></a>Die Application Integration für SumoLogic
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für SumoLogic aktivieren.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SumoLogic:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Sumologic**.

    ![Anwendung Galerie] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **SumoLogic aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer SumoLogic mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie Ihr SumoLogictenant eine Base64-codierte Zertifikat hinzufügen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **SumoLogic** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei SumoLogic** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **SumoLogic Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. SumoLogic.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren Aoo URL] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Konfigurieren Aoo URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am SumoLogic** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens SumoLogic.

6.  Gehen Sie zu **verwalten \> Security**.

    ![Verwalten] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Verwalten")

7.  Klicken Sie auf **SAML**.

    ![Globale Sicherheit] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globale Sicherheit")

8.  Wählen Sie aus der Liste **Wählen Sie eine Konfiguration oder eine neue erstellen** **Azure Anzeige aus**und dann auf **Konfigurieren**.

    ![Konfigurieren von SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Konfigurieren von SAML 2.0")

9.  Gehen Sie im Dialogfeld **Konfigurieren SAML 2.0** :

    ![Konfigurieren von SAML 2.0] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Konfigurieren von SAML 2.0")

    1.  Geben Sie im Textfeld **Name der Konfiguration** **Azure AD**.
    2.  **Debug-Modus**wählen.
    3.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am SumoLogic** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    4.  Kopieren Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am SumoLogic** **Authentifizierung anfordern URL** -Wert, und fügen Sie ihn in das Textfeld **Authn Request URL** .
    5.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    6.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie das gesamte Zertifikat in **X. 509-Zertifikat** Textbox.
    7.  Wählen Sie als **E-Mail-Attribut** **verwenden SAML Betreff**.
    8.  Wählen Sie **SP initiiert Login-Konfiguration**.
    9.  Geben Sie im Textfeld **Benutzername Pfad** **Azure**.
    10. Klicken Sie auf **Speichern**.

10. Wählen Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am SumoLogic** die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie auf **vollständig**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD SumoLogic anmelden können, müssen sie SumoLogic bereitgestellt werden.  
Bei SumoLogic ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **SumoLogic** .

2.  Gehen Sie zu **verwalten \> Benutzer**.

    ![Benutzer] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Benutzer")

3.  Klicken Sie auf **Hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Benutzer")

4.  Gehen Sie im Dialogfeld **Neuer Benutzer** :

    ![Neuer Benutzer] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Neuer Benutzer")

    1.  Geben Sie Informationen in die Textfelder **Vorname**, **Nachname** und **e-Mail-** Bereitstellung gewünschte Azure AD-Konto.
    2.  Wählen Sie eine Rolle aus.
    3.  Wählen Sie **Status** **aktiv**.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen SumoLogic Benutzer Konto Erstellungstools oder APIs von SumoLogic Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu SumoLogic:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **SumoLogic** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)