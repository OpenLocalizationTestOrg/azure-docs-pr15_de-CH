<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Aha! | Microsoft Azure" 
    description="Lernen Sie Aha! Azure Active Directory einmaliges, automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Lernprogramm: Azure Active Directory-Integration mit Aha!

Das Ziel dieses Lernprogramms ist die Integration von Azure und Aha anzeigen!  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Aha! Einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms haben Azure AD-Benutzer Sie Aha zugewiesen. Einmaliges in der Anwendung die Aha werden! Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md).

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren die Anwendungsintegration Aha!
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-aha-tutorial/IC798944.png "Szenario")
##<a name="enabling-the-application-integration-for-aha"></a>Aktivieren die Anwendungsintegration Aha!

Dieser Abschnitt soll beschreiben die Anwendungsintegration Aha aktivieren.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Aha!, gehen Sie folgendermaßen vor:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-aha-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-aha-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-aha-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Aha!**.

    ![Anwendung Galerie] (./media/active-directory-saas-aha-tutorial/IC798945.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Aha!**, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Aha!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Sie Benutzer authentifizieren Aha! mit seinem Konto in Azure AD Zusammenschlüsse basierend auf SAML-Protokoll.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Im klassischen Azure-Portal auf die **Aha!** Anwendungsintegration auf **Configure einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-aha-tutorial/IC798946.png "Einmaliges Anmelden konfigurieren")

2.  Auf der **Wie möchten Sie Benutzer anmelden Aha!** Seite, wählen Sie **Microsoft Azure AD einmaliges Anmelden**und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-aha-tutorial/IC798947.png "Einmaliges Anmelden konfigurieren")

3.  Auf der Seite **Konfigurieren App URL** in **Aha! Anmelde-URL** Textfeld, geben die URL verwendet Ihre Benutzer Ihre Aha anmelden! Anwendung (z.B.: "*https://company.aha.io/session/new*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-aha-tutorial/IC798948.png "Konfigurieren von App-URL")

4.  Auf die **Konfigurieren Sie einmaliges Anmelden auf Aha!** Seite Laden der Metadatendatei, auf **Metadaten**und Metadaten-Datei lokal auf Ihrem Computer speichern.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-aha-tutorial/IC798949.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster Ihren Aha! Website des Unternehmens als Administrator.

6.  Klicken Sie im Menü oben auf **Settings**.

    ![Standardeinstellungen] (./media/active-directory-saas-aha-tutorial/IC798950.png "Standardeinstellungen")

7.  Klicken Sie auf **Konto**.

    ![Profil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

8.  Klicken Sie auf **Sicherheit und einmaliges Anmelden**.

    ![Sicherheit und einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798952.png "Sicherheit und einmaliges Anmelden")

9.  **Einmaliges Anmelden** als **Identitätsanbieter**, wählen Sie unter **SAML2.0**.

    ![Sicherheit und einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798953.png "Sicherheit und einmaliges Anmelden")

10. Auf der Seite **Einmaliges** Schritte:

    ![Einmaliges Anmelden] (./media/active-directory-saas-aha-tutorial/IC798954.png "Einmaliges Anmelden")

    1.  Geben Sie in das Textfeld **Name** einen Namen für Ihre Konfiguration.
    2.  Wählen Sie für die **Verwendung konfigurieren** **Metadatendatei**.
    3.  Klicken Sie zum Hochladen der heruntergeladenen Metadatendatei auf **Durchsuchen**.
    4.  Klicken Sie auf **Aktualisieren**.

11. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-aha-tutorial/IC798955.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD Benutzer anmelden Aha!, sie in Aha bereitgestellt werden.  
Bei Aha!, Bereitstellung einer automatisierten Aufgabe ist.  
Es ist keine Aufgabe für Sie.
  
Benutzer werden bei Bedarf automatisch beim ersten single Sign-On erstellt.

>[AZURE.NOTE] Sie können andere Aha! Benutzer Konto Erstellungstools oder Aha-APIs! Benutzerkonten AAD bereitstellen.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Zuweisen von Benutzern zu Aha!, gehen Sie folgendermaßen vor:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Auf der **Aha!** Anwendungsintegration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-aha-tutorial/IC798956.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-aha-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
