<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Coupa | Microsoft Azure" 
    description="Erfahren Sie, wie mit Coupa in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Lernprogramm: Azure Active Directory-Integration mit Coupa

Das Ziel dieses Lernprogramms ist die Integration von Azure und Coupa anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Coupa einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können sich mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Anwendung für einmaliges Azure AD-Benutzer, die Coupa zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Coupa
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Szenario")
##<a name="enabling-the-application-integration-for-coupa"></a>Die Application Integration für Coupa

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Coupa aktivieren.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Coupa:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Coupa**.

    ![Anwendung Galerie] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Coupa aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Coupa mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für Coupa erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Melden Sie sich als Administrator auf der Website Ihres Unternehmens Coupa an.

2.  Zur **Installation \> Sicherheit**.

    ![Sicherheitskontrollen] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Sicherheitskontrollen")

3.  Metadatendatei Coupa auf Ihren Computer herunterladen, klicken Sie auf **Download und SP-Metadaten importieren**.

    ![Coupa SP-Metadaten] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP-Metadaten")

4.  Melden Sie sich in einem anderen Browserfenster auf Azure-Verwaltungsportal an.

5.  Klicken Sie auf der Seite Application Integration **Coupa** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Einmaliges Anmelden konfigurieren")

6.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Coupa** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Einmaliges Anmelden konfigurieren")

7.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **Anmelde-URL** zur Coupa Anwendung anmelden von Benutzern verwendete URL (z. B.: "*http://company.Coupa.com*").
    2.  Öffnen Sie die Datei Coupa Metadaten und **AssertionConsumerService Index/URL**kopieren.
    3.  Fügen Sie im Feld **Antwort-URL Coupa** **AssertionConsumerService Index/URL** -Wert.
    4.  Klicken Sie auf **Weiter**.

8.  Auf der Seite **Konfigurieren einmaliges Anmelden am Coupa** zum Laden der Metadatendatei auf **Metadaten**und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Einmaliges Anmelden konfigurieren")

9.  Gehen Sie auf der Website Coupa Unternehmen zu **Installation \> Sicherheit**.

    ![Sicherheitskontrollen] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Sicherheitskontrollen")

10. Im Abschnitt **Coupa Anmeldeinformationen anmelden** Schritte:

    ![Coupa-Anmeldeinformationen anmelden] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Coupa-Anmeldeinformationen anmelden")

    1.  Wählen Sie **SAML anmelden**.
    2.  Klicken Sie auf **Durchsuchen** , um heruntergeladenen Azure Active Metadatendatei hochladen.
    3.  Klicken Sie auf **Speichern**.

11. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Coupa anmelden können, müssen sie in Coupa bereitgestellt werden.  
Bei Coupa ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **Coupa** Unternehmen als Administrator an.

2.  Klicken Sie im Menü oben klicken Sie auf **Setup**und dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Benutzer")

3.  Klicken Sie auf **Erstellen**.

    ![Benutzer erstellen] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Benutzer erstellen")

4.  Folgendermaßen Sie im Abschnitt **Erstellen Benutzer** an:

    ![Benutzerinformationen] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Benutzerinformationen")

    1.  Geben Sie die **Login** **Vorname** **Nachname**, **Single Sign-On ID** **e-Mail-** Attribute eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Erstellen**.

    >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail mit einem Link zu das Konto bestätigen aktiviert wird.

>[AZURE.NOTE] Können Sie alle anderen Coupa Benutzer Konto Erstellungstools oder APIs von Coupa Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Coupa:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Coupa **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
