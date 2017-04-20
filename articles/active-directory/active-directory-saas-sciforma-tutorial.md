<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Sciforma | Microsoft Azure" 
    description="Erfahren Sie, wie mit Sciforma in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-ad-integration-with-sciforma"></a>Lernprogramm: Azure AD-Integration mit Sciforma
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Sciforma anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Sciforma Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Sciforma Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sciforma zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Sciforma
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sciforma-tutorial/IC777369.png "Szenario")
##<a name="enabling-the-application-integration-for-sciforma"></a>Die Application Integration für Sciforma
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Sciforma aktivieren.

###<a name="to-enable-the-application-integration-for-sciforma-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Sciforma:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sciforma-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sciforma-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-sciforma-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-sciforma-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Sciforma**.

    ![Anwendung Galerie] (./media/active-directory-saas-sciforma-tutorial/IC777370.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Sciforma aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Sciforma] (./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Sciforma mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Sciforma** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sciforma-tutorial/IC777372.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Sciforma** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sciforma-tutorial/IC777373.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Sciforma Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Sciforma.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-sciforma-tutorial/IC777374.png "Konfigurieren von app-URL")

4.  **Konfigurieren einmaliges Anmelden am Sciforma** Seite herunterladen die Metadaten auf **Metadaten**und dann die Datei lokal als **c:\\SciformaMetaData.xml**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sciforma-tutorial/IC777375.png "Einmaliges Anmelden für konfigurieren")

5.  Weiterleiten der Metadatendatei, Sciforma-Supportteam. Das Support-Team muss einmaliges Anmelden für Sie konfiguriert.

6.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sciforma-tutorial/IC777376.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer Sciforma Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer die Abdeckung mit Sciforma anmelden, überprüft Sciforma, ob der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch vom Sciforma erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-sciforma-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Sciforma:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Sciforma **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-sciforma-tutorial/IC777377.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sciforma-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)