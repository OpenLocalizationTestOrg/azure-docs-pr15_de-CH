<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Cherwell | Microsoft Azure" 
    description="Erfahren Sie, wie mit Cherwell in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Lernprogramm: Azure Active Directory-Integration mit Cherwell

Das Ziel dieses Lernprogramms ist die Integration von Azure und Cherwell anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Cherwell einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Cherwell Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Cherwell zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Cherwell
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Szenario")
##<a name="enabling-the-application-integration-for-cherwell"></a>Die Application Integration für Cherwell

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Cherwell aktivieren.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Cherwell:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Cherwell aus**und dann auf **vollständig** die Anwendung hinzufügen.
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Dieser Abschnitt soll Gliedern wie Benutzer Cherwell Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Cherwell** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Cherwell** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Einmaliges Anmelden konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Konfigurieren von App-URL")

    ein.  Geben Sie im Textfeld **Anmelde-URL** den URL der **Cherwell** Anmelden von Benutzern verwendet (z.B.: *https://\<Firmenname\>.cherwellondemand.com/cherwellclient*).

    b.  Klicken Sie auf **Weiter**

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Cherwell** Schritte:

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Einmaliges Anmelden konfigurieren")

    ein.  Klicken Sie auf **Zertifikat herunterladen**und speichern Sie das Zertifikat lokal auf Ihrem Computer.

    b.  Kopieren Sie die **Identität Provider-URL**.

    c.  Kopieren Sie die **URL des Diensts für einmaliges Anmelden**.

    d.  Klicken Sie auf **Weiter**.

5.  Senden Sie das heruntergeladene Zertifikat **Identität Provider-URL** und die **Einzelnen Sign-On Service URL** der Cherwell Support-Team.

    >[AZURE.NOTE] Das Supportteam Cherwell bezieht SSO-Konfiguration.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Cherwell anmelden können, müssen sie in Cherwell bereitgestellt werden.  
Bei Cherwell müssen die Benutzerkonten von Ihrem Supportteam Cherwell erstellt werden.

>[AZURE.NOTE] Können Sie alle anderen Cherwell Benutzer Konto Erstellungstools oder APIs von Cherwell Bereitstellung Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Cherwell Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Cherwell** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
