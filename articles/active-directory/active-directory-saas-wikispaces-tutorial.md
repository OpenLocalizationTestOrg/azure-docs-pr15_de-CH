<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Wikispaces | Microsoft Azure" 
    description="Erfahren Sie, wie mit Wikispaces in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-wikispaces"></a>Lernprogramm: Azure Active Directory-Integration mit Wikispaces
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Wikispaces anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Wikispaces einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Wikispaces Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Wikispaces zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Wikispaces
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-wikispaces-tutorial/IC787182.png "Szenario")

##<a name="enabling-the-application-integration-for-wikispaces"></a>Die Application Integration für Wikispaces
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Wikispaces aktivieren.

###<a name="to-enable-the-application-integration-for-wikispaces-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Wikispaces:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-wikispaces-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-wikispaces-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-wikispaces-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-wikispaces-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Wikispaces**.

    ![Anwendung Galerie] (./media/active-directory-saas-wikispaces-tutorial/IC787186.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Wikispaces aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wikispaces] (./media/active-directory-saas-wikispaces-tutorial/IC787187.png "Wikispaces")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Wikispaces mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Wikispaces** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-wikispaces-tutorial/IC787188.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Wikispaces** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-wikispaces-tutorial/IC787189.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Wikispaces** den URL dem folgenden Muster "*http://company.wikispaces.net*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-wikispaces-tutorial/IC787190.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Wikispaces** auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-wikispaces-tutorial/IC787191.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie die Metadatafile zu Wikispaces.

    >[AZURE.NOTE] Die Konfiguration für einzelne Zeichen muss Wikispaces Support Team ausgeführt werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-wikispaces-tutorial/IC787192.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Wikispaces anmelden können, müssen sie in Wikispaces bereitgestellt werden.  
Bei Wikispaces ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **Wikispaces** Unternehmen als Administrator an.

2.  Werden Sie **Mitglieder**.

    ![Mitglieder] (./media/active-directory-saas-wikispaces-tutorial/IC787193.png "Mitglieder")

3.  Klicken Sie auf **einladen**.

    ![Personen einladen] (./media/active-directory-saas-wikispaces-tutorial/IC787194.png "Personen einladen")

4.  Im Abschnitt **Personen einladen** Schritte:

    ![Personen einladen] (./media/active-directory-saas-wikispaces-tutorial/IC787208.png "Personen einladen")

    1.  Geben Sie den **Benutzernamen oder e-Mail-Adresse** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Senden**.  

        >[AZURE.NOTE] Azure Active Directory Kontoinhaber erhält eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Wikispaces Benutzer Konto Erstellungstools oder APIs von Wikispaces Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-wikispaces-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Wikispaces:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Wikispaces **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-wikispaces-tutorial/IC787195.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-wikispaces-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)