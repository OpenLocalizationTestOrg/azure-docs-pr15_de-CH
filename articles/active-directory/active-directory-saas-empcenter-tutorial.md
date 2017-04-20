<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit EmpCenter | Microsoft Azure" 
    description="Erfahren Sie, wie mit EmpCenter in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Lernprogramm: Azure Active Directory-Integration mit EmpCenter
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und EmpCenter anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein EmpCenter SSO-Abonnement aktiviert
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der EmpCenter Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die EmpCenter zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für EmpCenter
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Szenario")
##<a name="enabling-the-application-integration-for-empcenter"></a>Die Application Integration für EmpCenter
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für EmpCenter aktivieren.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für EmpCenter:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **EmpCenter**.

    ![Anwendung Galerie] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **EmpCenter aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer EmpCenter mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **EmpCenter** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei EmpCenter** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Einmaliges Anmelden konfigurieren")

3.  Auf der Seite **Konfigurieren App Settings** Schritte:

    ![Anwendung konfigurieren] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Anwendung konfigurieren")

    1.  Geben Sie im Textfeld **Anmelde-URL** die URL von den Benutzern Ihrer Anwendung EmpCenter anmelden (z.B.: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Klicken Sie auf **Weiter**

4.  Auf der Seite **Konfigurieren einmaliges Anmelden unter EmpCenter** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie die heruntergeladenen Metadatendatei Ihre EmpCenter Support-Team

    >[AZURE.NOTE] Das Supportteam EmpCenter bezieht SSO-Konfiguration.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD EmpCenter anmelden können, müssen sie in EmpCenter bereitgestellt werden.  
Bei EmpCenter müssen die Benutzerkonten von Ihrem Supportteam EmpCenter erstellt werden.

>[AZURE.NOTE] Können Sie alle anderen EmpCenter Benutzer Konto Erstellungstools oder APIs von EmpCenter Bereitstellung Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu EmpCenter:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **EmpCenter **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)