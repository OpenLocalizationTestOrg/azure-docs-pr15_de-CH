<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit RightAnswers | Microsoft Azure" 
    description="Erfahren Sie, wie mit RightAnswers in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Lernprogramm: Azure Active Directory-Integration mit RightAnswers
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und RightAnswers anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein RightAnswers einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können sich mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Anwendung für einmaliges Azure AD-Benutzer, die RightAnswers zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für RightAnswers
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Szenario")
##<a name="enabling-the-application-integration-for-rightanswers"></a>Die Application Integration für RightAnswers
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für RightAnswers aktivieren.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für RightAnswers:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **RightAnswers**.

    ![Anwendung Galerie] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **RightAnswers aus**und dann auf **vollständig** die Anwendung hinzufügen.
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer RightAnswers mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **RightAnswers** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei RightAnswers** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren Appeinstellungen** im Textfeld **Anmelde-URL** die URL von den Benutzern Ihrer Anwendung RightAnswers anmelden (z.B.: *https://fortify.rightanswers.com/portal/ss/*), und klicken Sie dann auf **Weiter**.

    ![Anwendung konfigurieren] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Anwendung konfigurieren")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden unter RightAnswers** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie die heruntergeladenen Metadatendatei Ihre RightAnswers Support-Team

    >[AZURE.NOTE] Das Supportteam RightAnswers bezieht SSO-Konfiguration.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD RightAnswers anmelden können, müssen sie in RightAnswers bereitgestellt werden.  
Bei RightAnswers ist die Bereitstellung einer automatisierten Aufgabe.  
Es ist keine Aufgabe für Sie.
  
Benutzer werden bei Bedarf automatisch beim ersten single Sign-On erstellt.

>[AZURE.NOTE]Können Sie alle anderen RightAnswers Benutzer Konto Erstellungstools oder APIs von RightAnswers Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu RightAnswers:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **RightAnswers **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)