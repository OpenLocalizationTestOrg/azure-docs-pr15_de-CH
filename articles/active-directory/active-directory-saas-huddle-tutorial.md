<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration drängen | Microsoft Azure" 
    description="Erfahren Sie, wie mit drängen Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Lernprogramm: Azure Active Directory Integration drängen
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und drängen anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein drängen einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem drängen Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, drängen zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration drängen aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Einmaliges Anmelden konfigurieren")
##<a name="enabling-the-application-integration-for-huddle"></a>Die Anwendungsintegration drängen aktivieren
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration drängen aktivieren.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration drängen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **zusammen**.

    ![Anwendung Galerie] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Anwendung Galerie")

7.  Im Ergebnisbereich **drängen**wählen Sie aus und dann auf **vollständig** die Anwendung hinzufügen.

    ![Drängen] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Drängen")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer drängen Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf **drängen** Application Integrationsseite auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Benutzer anmelden drängen** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Drängen Anmelde-URL** den URL der drängen Mieter dem folgenden Muster "*http://company.huddle.com*" und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am drängen** Schritte:

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Einmaliges Anmelden konfigurieren")

    1.  Klicken Sie auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.
    2.  Kopieren der **Aussteller URL** -Wert, der **SAML SSO-URL** -Wert heruntergeladene Zertifikat und senden zu drängen.

    >[AZURE.NOTE] Einmaliges Anmelden muss aktiviert werden, indem das Supportteam drängen.
Sie erhalten eine Benachrichtigung, wenn die Konfiguration abgeschlossen ist.

5.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer drängen anmelden können, müssen sie in drängen bereitgestellt werden.  
Bei drängen ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihr Unternehmensstandort **drängen** als Administrator an.

2.  Klicken Sie auf **Arbeitsplatz**.

3.  Klicken Sie auf **Personen \> Personen**.

    ![Personen] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Personen")

4.  Führen Sie im Abschnitt **Erstellen Sie eine neue Einladung** die folgenden Schritte aus:

    ![Neue Einladung] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Neue Einladung")

    1.  Wählen Sie in der Liste **ein Team einladen von Personen zu** **Team**.
    2.  Geben Sie die **E-Mail-Adresse** eines gültigen Kontos AAD Bestimmung in das verknüpfte Textfeld soll.
    3.  Klicken Sie auf **Laden**.

    >[AZURE.NOTE] Kontoinhaber Azure AD erhalten eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Alle anderen drängen Benutzer Konto Erstellungstools verwenden oder APIs von drängen Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Um drängen Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie die **drängen **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)