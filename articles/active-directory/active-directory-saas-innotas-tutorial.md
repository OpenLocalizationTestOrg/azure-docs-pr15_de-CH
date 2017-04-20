<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Innotas | Microsoft Azure"
    description="Erfahren Sie, wie mit Innotas in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-innotas"></a>Lernprogramm: Azure Active Directory-Integration mit Innotas
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Innotas anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Innotas Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Innotas Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Innotas zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Innotas
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-innotas-tutorial/IC777331.png "Szenario")
##<a name="enabling-the-application-integration-for-innotas"></a>Die Application Integration für Innotas
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Innotas aktivieren.

###<a name="to-enable-the-application-integration-for-innotas-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Innotas:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-innotas-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-innotas-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-innotas-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-innotas-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Innotas**.

    ![Anwendung Galerie] (./media/active-directory-saas-innotas-tutorial/IC777332.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Innotas aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Innotas] (./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Innotas mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Innotas** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-innotas-tutorial/IC777334.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Innotas** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-innotas-tutorial/IC777335.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Innotas Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Innotas.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-innotas-tutorial/IC777336.png "Konfigurieren von app-URL")

4.  **Konfigurieren einmaliges Anmelden am Innotas** Seite herunterladen die Metadaten auf **Metadaten**und dann die Datei lokal als **c:\\InnotasMetaData.xml**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-innotas-tutorial/IC777337.png "Einmaliges Anmelden für konfigurieren")

5.  Weiterleiten der Metadatendatei, Innotas-Supportteam. Das Support-Team muss einmaliges Anmelden für Sie konfiguriert.

6.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-innotas-tutorial/IC777338.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer Innotas Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer die Abdeckung mit Innotas anmelden, überprüft Innotas, ob der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch vom Innotas erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-innotas-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Innotas:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Innotas **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-innotas-tutorial/IC777339.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-innotas-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)