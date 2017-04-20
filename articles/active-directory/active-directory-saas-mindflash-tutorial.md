<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Mindflash | Microsoft Azure" 
    description="Erfahren Sie, wie mit Mindflash in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Lernprogramm: Azure Active Directory-Integration mit Mindflash
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Mindflash anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mindflash einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Mindflash Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Mindflash zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Mindflash
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Szenario")
##<a name="enabling-the-application-integration-for-mindflash"></a>Die Application Integration für Mindflash
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Mindflash aktivieren.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Mindflash:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Mindflash**.

    ![Anwendung Galerie] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Mindflash aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Mindflash mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Mindflash** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Mindflash** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL** den URL dem folgenden Muster "*http://company.mindflash.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Mindflash** auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie die Metadatafile zu Mindflash.

    >[AZURE.NOTE] Die Konfiguration für einzelne Zeichen muss Mindflash Support Team ausgeführt werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Mindflash anmelden können, müssen sie in Mindflash bereitgestellt werden.  
Bei Mindflash ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **Mindflash** Unternehmen als Administrator an.

2.  Gehen Sie zum **Verwalten von Benutzern**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Verwalten von Benutzern")

3.  Klicken Sie auf **Benutzer hinzufügen**und dann auf **neu**.

4.  Im Abschnitt **Neue Benutzer hinzufügen** die folgenden Schritte:

    ![Neue Benutzer hinzufügen] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Neue Benutzer hinzufügen")

    1.  Geben Sie **Vorname**, **Nachname** und **E-Mail** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Hinzufügen**.

>[AZURE.NOTE]Können Sie alle anderen Mindflash Benutzer Konto Erstellungstools oder APIs von Mindflash Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Mindflash:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Mindflash **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)