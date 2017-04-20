<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Clever | Microsoft Azure" 
    description="Erfahren Sie, wie mit Clever Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Lernprogramm: Azure Active Directory Integration Clever

Das Ziel dieses Lernprogramms ist die Integration von Azure und Clever anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine clevere Mieter

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die intelligente Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, die Sie Clever zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Anwendungsintegration Clever aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-clever-tutorial/IC798977.png "Szenario")
##<a name="enabling-the-application-integration-for-clever"></a>Die Anwendungsintegration Clever aktivieren

Dieser Abschnitt soll beschreiben die Anwendungsintegration Clever aktivieren.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Clever:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-clever-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-clever-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-clever-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Clever**.

    ![Anwendung Galerie] (./media/active-directory-saas-clever-tutorial/IC798978.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Clever aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Intelligente] (./media/active-directory-saas-clever-tutorial/IC798979.png "Intelligente")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Clever Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Die intelligente Anwendung erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss.  
Der folgende Screenshot zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-clever-tutorial/IC798980.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Clever** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clever-tutorial/IC784682.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Clever** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clever-tutorial/IC798981.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Raffinierte Anmelde-URL** die URL die Benutzer anmelden, der intelligente Anwendung (z.B.: *https://clever.com/in/azsandbox*), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-clever-tutorial/IC798982.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Clever** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clever-tutorial/IC798983.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator die clevere Unternehmensstandort.

6.  Klicken Sie in der Symbolleiste auf **Instant anmelden**.

    ![Instant-Anmeldung] (./media/active-directory-saas-clever-tutorial/IC798984.png "Instant-Anmeldung")

7.  Auf der Anmeldeseite **Instant** Schritte:

    ![Instant-Anmeldung] (./media/active-directory-saas-clever-tutorial/IC798985.png "Instant-Anmeldung")

    1.  Geben Sie die **Anmelde-URL**.  

        >[AZURE.NOTE] **Anmelde-URL** ist ein benutzerdefinierter Wert.
Den tatsächlichen Wert erhalten intelligente Supports.

    2.  Wählen Sie als **Identität** **ADFS**.
    3.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-clever-tutorial/IC798986.png "Einmaliges Anmelden konfigurieren")

9.  Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen.

    ![Attribute] (./media/active-directory-saas-clever-tutorial/IC795920.png "Attribute")

10. Fügen Sie erforderliche Attribut Mappings Schritte:

    ![SAML-token-Attribute] (./media/active-directory-saas-clever-tutorial/IC795921.png "SAML-token-Attribute")

  	|Attributname|Attributwert|
  	|---|---|
  	|clever.Student.Credentials.District\_Benutzername|User.userPrincipalName|

    1.  Jede Datenzeile in der Tabelle oben klicken Sie auf **Attribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.
    3.  Wählen Sie im Textfeld **Attributwert** Attributwert für die Zeile angezeigt.
    4.  Klicken Sie auf **abgeschlossen**.

11. Klicken Sie auf **Übernehmen**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Clever anmelden können, müssen sie in Clever bereitgestellt werden.  
Bei Clever ist die Bereitstellung einer manuellen Aufgabe, die durch raffinierte Supportteams erfolgen.

>[AZURE.NOTE] Können andere intelligente Benutzer Konto Erstellungstools oder APIs von Clever Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Clever:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Clever **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-clever-tutorial/IC798987.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-clever-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
