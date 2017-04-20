<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit 15Five | Microsoft Azure" 
    description="Erfahren Sie, wie mit 15Five in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Lernprogramm: Azure Active Directory-Integration mit 15Five

Das Ziel dieses Lernprogramms ist die Integration von Azure und 15Five anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein 15Five einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der 15Five Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die 15Five zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für 15Five
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-15five-tutorial/IC784667.png "Szenario")
##<a name="enabling-the-application-integration-for-15five"></a>Die Application Integration für 15Five

Dieser Abschnitt soll beschreiben die Anwendungsintegration für 15Five aktivieren.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für 15Five:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-15five-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-15five-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-15five-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **15Five**.

    ![Anwendung Galerie] (./media/active-directory-saas-15five-tutorial/IC784668.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **15Five aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer 15Five mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **15Five** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-15five-tutorial/IC784670.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei 15Five** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-15five-tutorial/IC784671.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App URL** in das Textfeld **15Five Zeichen In URL** den URL dem folgenden Muster "*https://company.15Five.com*" und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-15five-tutorial/IC784672.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am 15Five** auf **Metadaten**und Weiterleiten der Metadatendatei zu 15Five.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-15five-tutorial/IC784673.png "Einmaliges Anmelden für konfigurieren")

    >[AZURE.NOTE] Einmaliges Anmelden muss aktiviert werden, indem 15Five Support-Team.

5.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-15five-tutorial/IC784674.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Benutzer Azure AD 15Five anmelden können, müssen sie in 15Five bereitgestellt werden.  
Bei 15Five ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **15Five** Unternehmen als Administrator an.

2.  Wechseln Sie zu **Unternehmen**.

    ![Unternehmen verwalten] (./media/active-directory-saas-15five-tutorial/IC784675.png "Unternehmen verwalten")

3.  Gehen Sie zu **Personen \> Personen hinzufügen**.

    ![Personen] (./media/active-directory-saas-15five-tutorial/IC784676.png "Personen")

4.  Im Abschnitt neue Person hinzufügen die folgenden Schritte:

    ![Neue Person hinzufügen] (./media/active-directory-saas-15five-tutorial/IC784677.png "Neue Person hinzufügen")

    1.  Geben Sie **Vorname**, **Nachname**, **Titel**, **e-Mail-Adresse** eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Fertig**.

    >[AZURE.NOTE] Kontoinhaber Azure AD erhalten eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen 15Five Benutzer Konto Erstellungstools oder APIs von 15Five Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu 15Five:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **15Five **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-15five-tutorial/IC784678.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-15five-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
