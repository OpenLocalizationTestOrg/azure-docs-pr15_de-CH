<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration SCC-Lebenszyklus | Microsoft Azure" 
    description="Erfahren Sie, wie mit SCC-Lebenszyklus Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Lernprogramm: Azure Active Directory Integration SCC-Lebenszyklus
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SCC-Lebenszyklus angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein SCC-Lebenszyklus einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die SCC-Lebenszyklus Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die SCC-Lebenszyklus zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für SCC-Lebenszyklus
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Szenario")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Die Application Integration für SCC-Lebenszyklus
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für SCC-Lebenszyklus zu aktivieren.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SCC-Lebenszyklus:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **SCC-Lebenszyklus**.

    ![Anwendung Galerie] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **SCC-Lebenszyklus aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![SCC-Lebenszyklus] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC-Lebenszyklus")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern Benutzer authentifizieren SCC-Lebenszyklus mit seinem Konto in Azure AD Zusammenschlüsse basierend auf SAML-Protokoll aktivieren.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **SCC-Lebenszyklus** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Einmaliges Anmelden konfigurieren")

2.  Wählen Sie auf der Seite **Wie möchten Sie Benutzer anmelden SCC-Lebenszyklus** **Microsoft Azure AD einmaliges Anmelden**und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL** den URL der SCC-Lebenszyklus-Anwendung mit dem folgenden Muster "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" Anmelden von Benutzern verwendet, und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am SCC-Lebenszyklus** auf **Metadaten**und speichern Sie Metadatendatei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Einmaliges Anmelden konfigurieren")

5.  Der Metadatendatei SCC LifeCycle Support-Team weitergeleitet.

    >[AZURE.NOTE]Einmaliges Anmelden muss aktiviert werden, indem die SCC-Lebenszyklus-Support-Team.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD Benutzer SCC-Lebenszyklus anmelden können, müssen sie in SCC-Lebenszyklus bereitgestellt werden.
  
Gibt es keine Aufgabe SCC-Lebenszyklus benutzerbereitstellung konfigurieren.  
Zugewiesene Benutzer anmelden SCC-Lebenszyklus, wird ein SCC-LifeCycle-Konto automatisch bei Bedarf erstellt.

>[AZURE.NOTE]Alle anderen SCC-Lebenszyklus Benutzer Konto Erstellungstools verwenden oder APIs von SCC-Lebenszyklus Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Um Benutzer SCC-Lebenszyklus zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **SCC-Lebenszyklus **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)