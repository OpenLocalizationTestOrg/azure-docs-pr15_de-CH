<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration FM: Systeme | Microsoft Azure" 
    description="Erfahren Sie, wie Sie FM: mit Azure Active Directory einmaliges, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Lernprogramm: Azure Active Directory Integration FM: Systeme
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und FM:Systems anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein FM:Systems einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der FM:Systems Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die FM:Systems zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für FM:Systems
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Szenario")
##<a name="enabling-the-application-integration-for-fmsystems"></a>Die Application Integration für FM:Systems
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für FM:Systems aktivieren.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für FM:Systems:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **FM:Systems**.

    ![Anwendung Galerie] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **FM:Systems aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![FM: Systeme] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: Systeme")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer FM:Systems mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **FM:Systems** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei FM:Systems** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Einmaliges Anmelden konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **Anmelde-URL FM:Systems** den FM:Systems **Antwort-URL** (z. B.: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] FM Wert erhalten: Systeme Supportteam.

    2.  Klicken Sie auf **Weiter**

4.  Auf der Seite **Konfigurieren einmaliges Anmelden unter FM:Systems** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Einmaliges Anmelden konfigurieren")

5.  Senden die heruntergeladenen Metadatendatei FM: Systeme Supportteam.

    >[AZURE.NOTE] FM: Systeme Support-Team ist auf die SSO-Konfiguration.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD FM:Systems anmelden können, müssen sie in FM:Systems bereitgestellt werden.  
Bei FM:Systems ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie in einem Webbrowserfenster als Administrator der Website Ihres Unternehmens FM:Systems.

2.  Rufen Sie **System Administration \> Sicherheit verwalten \> Benutzer \> Liste**.

    ![Systemadministration] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Systemadministration")

3.  Klicken Sie auf **neuen Benutzer erstellen**.

    ![Neuen Benutzer erstellen] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Neuen Benutzer erstellen")

4.  Folgendermaßen Sie im Abschnitt **Erstellen Benutzer** an:

    ![Benutzer erstellen] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Benutzer erstellen")

    1.  Geben Sie den Benutzernamen, das Kennwort und Bestätigung der e-Mail-Adresse und die Mitarbeiter-ID eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Weiter**.

>[AZURE.NOTE] Können Sie alle anderen FM:Systems Benutzer Konto Erstellungstools oder APIs von FM:Systems Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu FM:Systems:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **FM:Systems **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)