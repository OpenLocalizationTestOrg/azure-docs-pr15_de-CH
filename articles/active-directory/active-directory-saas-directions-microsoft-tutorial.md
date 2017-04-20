<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Microsoft | Microsoft Azure" 
    description="Erfahren Sie, wie mit Richtung auf Microsoft Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Lernprogramm: Azure Active Directory-Integration mit Microsoft

Das Ziel dieses Lernprogramms ist Microsoft Azure Active Directory und die Integration angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Eine Anleitung Microsoft-Abonnement

Haben Sie eine federated Richtung auf Microsoft-Abonnement noch, e-Mail-Anforderung an "*service@DirectionsOnMicrosoft.com*".

Am Ende dieses Lernprogramms werden einzelne Anwendung einmaliges Anmelden in Azure Active Directory-Benutzer, die auf Microsoft zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Aktivieren der Anwendungsintegration auf Microsoft
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Szenario")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Aktivieren der Anwendungsintegration auf Microsoft

Dieser Abschnitt soll beschreiben die Anwendungsintegration für auf Microsoft aktivieren.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für auf Microsoft:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **auf Microsoft**.

    ![Anwendung Galerie] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich auswählen **auf Microsoft**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Szenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Szenario")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer auf Microsoft Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **auf der Microsoft** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden auf Microsoft** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Microsoft Azure AD Singel anmelden] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel anmelden")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld Anmelde-URL **https://www.directionsonmicrosoft.com/user/login**, und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am auf Microsoft** auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Einmaliges Anmelden konfigurieren")

5.  Die Metadatendatei an der Anleitung Microsoft Support-Team senden (*service@DirectionsOnMicrosoft.com*). Aktivieren Sie Richtung Microsoft Support Team zu verbundenen Standortmitgliedschaft enthalten Sie Informationen zu Ihrem Unternehmen in Ihrer e-Mail.

    >[AZURE.NOTE] Einmaliges Anmelden für auf Microsoft muss aktiviert werden, indem der Anleitung Microsoft Support-Team.
Sie erhalten eine Benachrichtigung, wenn einmaliges Anmelden aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Gibt es keine Aufgabe Benutzer auf Microsoft Bereitstellung konfigurieren.  
Wenn zugewiesenen Benutzer auf Microsoft Access Bedienfeld anmelden, auf Microsoft überprüft, ob der Benutzer vorhanden ist. Wenn es kein Benutzerkonto vorhanden noch, wird es auf Microsoft automatisch.
##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Um Benutzer auf Microsoft zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **auf der Microsoft **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Ja")
