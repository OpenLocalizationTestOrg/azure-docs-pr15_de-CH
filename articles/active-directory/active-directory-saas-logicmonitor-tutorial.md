<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit LogicMonitor | Microsoft Azure" 
    description="Erfahren Sie, wie mit LogicMonitor Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Lernprogramm: Azure Active Directory-Integration mit LogicMonitor
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und LogicMonitor anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   LogicMonitor Mieter
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für LogicMonitor
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Szenario")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Die Application Integration für LogicMonitor
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für LogicMonitor aktivieren.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für LogicMonitor:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Logicmonitor**.

    ![Anwendung Galerie] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **LogicMonitor aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer LogicMonitor Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **LogicMonitor **auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei LogicMonitor** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL** den URL LogicMonitor Anmelden von Benutzern zum \(e, g: "*http://company.logicmonitor.com*"\), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am LogicMonitor** auf **Metadaten**und auf dem Computer speichern.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie sich Ihre Site **LogicMonitor** Unternehmen als Administrator an.

6.  Klicken Sie im Menü oben auf **Settings**.

    ![Standardeinstellungen] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Standardeinstellungen")

7.  Klicken Sie in der Navigationsleiste auf der linken Seite **Einmaliges Anmelden**

    ![Einmaliges Anmelden] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Einmaliges Anmelden")

8.  Im Abschnitt **Single Sign-On (SSO) Settings** Schritte:

    ![Einstellung für einzelne Zeichen] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Einstellung für einzelne Zeichen")

    1.  Aktivieren Sie **einmaliges Anmelden**.
    2.  Wählen Sie als **Standard Rollenzuordnung** **Readonly**.
    3.  Öffnen Sie heruntergeladene Metadaten-Datei in Editor, und fügen Sie Inhalt der Datei in das Textfeld **Identität Metadaten** .
    4.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
AAD Benutzer anmelden müssen sie ihre Azure Active Directory-Benutzernamen für die LogicMonitor Anwendung bereitgestellt werden.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site LogicMonitor Unternehmen als Administrator an.

2.  Klicken Sie im Menü oben **Klicken Sie**und dann auf **Rollen und Benutzer**.

    ![Rollen und Benutzer] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Rollen und Benutzer")

3.  Klicken Sie auf **Hinzufügen**.

4.  Folgendermaßen Sie im Abschnitt **Hinzufügen eines Kontos** an:

    ![Konto hinzufügen] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Konto hinzufügen")

    1.  Geben Sie **Benutzername**, **E-Mail**, **Kennwort** und **Kennwort** Werte von Azure Active Directory-Benutzers in verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie **Rollen**, **Anzeigeberechtigungen** und den **Status**.
    3.  Klicken Sie auf **Senden**.

>[AZURE.NOTE]Können Sie alle anderen LogicMonitor Benutzer Konto Erstellungstools oder APIs von LogicMonitor Bereitstellung Azure Active Directory-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Um LogicMonitor Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **LogicMonitor** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)




