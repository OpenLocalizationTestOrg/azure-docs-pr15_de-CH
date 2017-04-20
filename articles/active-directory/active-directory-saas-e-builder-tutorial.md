<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit e | Microsoft Azure" 
    description="Erfahren Sie, wie mit e-Generators Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Lernprogramm: Azure Active Directory-Integration mit e-Generator
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und e-Generators angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter e-Generator
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an die e-Generator-Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, e-Generator zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration e-Generator
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Szenario")
##<a name="enabling-the-application-integration-for-e-builder"></a>Die Application Integration e-Generator
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für e-Generator aktivieren.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für e-Generator:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **E-Generators**.

    ![Anwendung Galerie] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **E-Generators aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![E-Generator] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "E-Generator")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer e-Generators mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **e-Builder** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer e-Generator anmelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **e-Generator anmelden URL** den URL dem folgenden Muster "*https://\<Name Mieters\>Builder e*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Konfigurieren von app-URL")

4.  **Konfigurieren einmaliges Anmelden am e-Generator-** Seite zum Herunterladen der Metadaten auf **Metadaten**und dann die Datei lokal als **c:\\e BuilderMetaData.xml**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Einmaliges Anmelden für konfigurieren")

5.  Weiterleiten der Metadatendatei e-Generator-Supportteam. Das Support-Team muss einmaliges Anmelden für Sie konfiguriert.

6.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer e-Generator-Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer e-Generators mit der Option Zugriff anmelden, überprüft e Generator, ob der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch vom e-Generator erstellt.
##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Um e-Builder Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **e-Builder **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
