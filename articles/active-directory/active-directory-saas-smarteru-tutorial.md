<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit SmarterU | Microsoft Azure" 
    description="Erfahren Sie, wie mit SmarterU in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Lernprogramm: Azure Active Directory-Integration mit SmarterU
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SmarterU anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   SmarterU Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der SmarterU Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die SmarterU zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für SmarterU
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Szenario")

##<a name="enabling-the-application-integration-for-smarteru"></a>Die Application Integration für SmarterU
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für SmarterU aktivieren.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SmarterU:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **SmarterU**.

    ![Anwendung fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Anwendung fallery")

7.  Wählen Sie im Ergebnisbereich **SmarterU aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer SmarterU mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **SmarterU** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei SmarterU** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Einmaliges Anmelden konfigurieren")

3.  **Konfigurieren einmaliges Anmelden am SmarterU** Seite herunterladen die Metadaten auf **Metadaten**und dann die Datei lokal als **c:\\SmarterUMetaData.cer**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Einmaliges Anmelden konfigurieren")

4.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens SmarterU.

5.  Klicken Sie in der oberen Symbolleiste auf **Konto**.

    ![Konto] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Konto")

6.  Führen Sie auf der Konfigurationsseite die folgenden Schritte aus:

    ![Externe Autorisierung] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Externe Autorisierung")

    1.  Aktivieren Sie **externe Autorisierung**.
    2.  Wählen Sie im Abschnitt **Master Anmeldesteuerelement** Registerkarte **SmarterU** .
    3.  Wählen Sie im Abschnitt **Benutzername Standard** **SmarterU** Registerkarte.
    4.  Aktivieren Sie **Okta**.
    5.  Kopieren Sie den Inhalt der heruntergeladenen Metadaten-Datei, und fügen Sie ihn in das Textfeld **Okta Metadaten** .
    6.  Klicken Sie auf **Speichern**.

7.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD SmarterU anmelden können, müssen sie in SmarterU bereitgestellt werden.  
Bei SmarterU ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **SmarterU** .

2.  **Benutzer**wechseln

3.  Führen Sie im Benutzerabschnitt die folgenden Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Neuer Benutzer")

    1.  Klicken Sie auf **+ Benutzer**.
    2.  Geben Sie die Werte verknüpftes Attribut des Benutzerkontos Azure AD in die folgenden Textfelder: **primäre E-Mail** **Personalnummer** **Kennwort**, **Kennwort**, **Namen**, **Nachnamen**.
    3.  Klicken Sie auf **aktiv**.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen SmarterU Benutzer Konto Erstellungstools oder APIs von SmarterU Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu SmarterU:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **SmarterU **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)