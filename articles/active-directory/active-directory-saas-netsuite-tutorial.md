<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit NetSuite | Microsoft Azure"
    description="Erfahren Sie, wie mit NetSuite Azure Active Directory-auf automatisierte Bereitstellung und mehr!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Exemplarische Vorgehensweise: NetSuite Azure Active Directory integrieren

Dieses Lernprogramm zeigt Ihnen Azure Active Directory (Azure AD) Ihrer Umgebung NetSuite Verbindung. Ermöglicht automatisches Benutzer bereitstellen einmaliges Anmelden NetSuite konfigurieren und Zuweisen von Benutzern auf NetSuite lernen. 

##<a name="prerequisites"></a>Erforderliche Komponenten

1. Zugriff auf Azure Active Directory über das [klassische Azure-Portal](https://manage.windowsazure.com)müssen Sie ein gültiges Azure-Abonnement verfügen.

2. Sie müssen Administrator [NetSuite](http://www.netsuite.com/portal/home.shtml) Abonnement zugreifen. Sie können ein kostenloses Testabo für Dienst.

##<a name="step-1-add-netsuite-to-your-directory"></a>Schritt 1: Hinzufügen von NetSuite zum Verzeichnis

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com)im linken Navigationsbereich auf **Active Directory**.

    ![Wählen Sie im linken Navigationsbereich des Active Directory.][0]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis, dem Sie möchten NetSuite zu.

3. Klicken Sie im oberen Menü auf **Applications** .

    ![Klicken Sie auf Anwendung.][1]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Klicken Sie auf Hinzufügen, um eine neue Anwendung hinzufügen.][2]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Klicken Sie auf eine Anwendung aus dem Katalog hinzufügen.][3]

6. Geben Sie im **Suchfeld** **NetSuite**. Wählen Sie **NetSuite** aus den Ergebnissen aus, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![NetSuite hinzufügen][4]

7. Sie sollten jetzt Quick Start Page NetSuite finden Sie unter:

    ![Quick Start-Seite in Azure AD NetSuite][5]

##<a name="step-2-enable-single-sign-on"></a>Schritt 2: Aktivieren Sie einmaliges Anmelden

1. Azure AD auf Schnellstart für NetSuite klicken auf die Schaltfläche **Konfigurieren einmaliges Anmelden** .

    ![Die einzelnen anmelden Schaltfläche Konfigurieren][6]

2. Ein Dialogfeld wird geöffnet, und Sie sehen einen Bildschirm mit der Frage "Wie Benutzer NetSuite anmelden möchten?" **Azure AD einmaliges**wählen Sie aus und dann auf **Weiter**.

    ![Wählen Sie Azure AD einmaliges Anmelden][7]

    > [AZURE.NOTE] Weitere Informationen zu den verschiedenen einzelnen Zeichen auf Optionen, [Klicken Sie hier](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work) zu

3. Geben Sie auf der Seite **Konfigurieren Appeinstellungen** im Feld **Antwort-URL** in Ihrem NetSuite Tenant-URL in einem der folgenden Formate:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Geben Sie die URL Mieter][8]

4. Auf der Seite **Konfigurieren einmaliges Anmelden am NetSuite** auf **Metadaten**und dann lokal auf Ihrem Computer gespeichert.

    ![Downloaden Sie die Metadaten.][9]

5. Öffnen einer neuen Registerkarte im Browser und der Website Ihres Netsuite Unternehmens als Administrator anmelden.

6. Klicken Sie in der Symbolleiste am oberen Rand der Seite auf **Setup**und dann auf **Installations-Manager**.

    ![Fahren Sie mit dem Installations-Manager][10]

7. Wählen Sie aus der Liste **Installationsaufgaben** **Integration**.

    ![Gehe zu Integration][11]

8. Klicken Sie im Abschnitt **Authentifizierung verwalten** **SAML einmaliges Anmelden**.

    ![Gehe zu SAML einmaliges Anmelden][12]

9. Auf der Seite **Setup SAML** Schritte:

    - In Azure Active Directory Wert **Remote Login-URL** kopieren und Einfügen im Feld **Identität Anbieter Anmeldeseite** NetSuite.

        ![Der SAML-Setup-Seite.][13]

    - Wählen Sie im NetSuite **Primäre Authentifizierungsmethode**.

    - Wählen Sie für das Feld **SAMLV2 Identität Metadaten** **IDP-Datei hochladen**. Klicken Sie auf **Durchsuchen** , um Metadaten-Datei hochladen, die Sie in Schritt 4 # heruntergeladen.

        ![Hochladen der Metadaten][16]

    - Klicken Sie auf **Senden**.

9. Kontrollkästchen Sie in Azure AD Konfiguration für einzelne Zeichen Bestätigung des Zertifikats aktivieren, die in NetSuite hochgeladen. Klicken Sie auf **Weiter**.

    ![Aktivieren Sie das Kontrollkästchen Bestätigung][14]

10. Geben Sie auf der letzten Seite des Dialogfelds eine e-Mail-Adresse möchten Sie e-Mail-Benachrichtigungen für Fehler und Warnungen im Zusammenhang mit der Wartung dieser Konfiguration für einzelne Zeichen. 

    ![Geben Sie Ihre e-Mail-Adresse.][15]

11. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen. Klicken Sie auf **Attribute** am oberen Rand der Seite.

    ![Klicken Sie auf Eigenschaften.][17]

12. Klicken Sie auf **Attribut hinzufügen**.

    ![Klicken Sie auf Attribut hinzufügen][18]

13. Geben Sie für das Feld **Attributname** in `account`. Das Feld **Attributwert** Geben Sie Ihre Konto-ID NetSuite Wie Sie Ihre Konto-ID gefunden werden unten aufgeführt:

    ![Fügen Sie Ihre Konto-ID für NetSuite][19]

    - NetSuite klicken Sie auf **Setup** Menü oben.
    - Klicken Sie im linken Navigationsmenü im Abschnitt **Einrichtungsaufgaben** und Einstellungen Sie **Web Services**wählen Sie **Abschnitt aus** .
    - Kopieren Sie Ihre Kundennummer NetSuite und fügen Sie ihn im Feld **Attributwert** in Azure AD.

        ![Die Konto-ID abrufen][20]

14. Klicken Sie in Azure AD **abgeschlossen** SAML-Attribut hinzugefügt haben. Klicken Sie im unteren Menü **Änderungen** .

    ![Speichern.][21]

15. Bevor Benutzer in NetSuite einmaliges ausführen können, müssen sie zunächst die entsprechenden Berechtigungen in NetSuite zugewiesen. Gehen Sie diese Berechtigungen zuzuweisen.

    - Im oberen Navigationsbereich auf **Setup**klicken **Installations-Manager**.

        ![Fahren Sie mit dem Installations-Manager][10]

    - Im linken Navigationsbereich auf ausgewählte **Benutzer/Rollen**und dann auf **Rollen verwalten**.

        ![Gehe zum Verwalten von Rollen][22]

    - Klicken Sie auf **neue Rolle**.

    - Geben Sie einen **Namen** für die neue Rolle und aktivieren Sie das Kontrollkästchen **Single Sign-On nur** .

        ![Name der neuen Rolle.][23]

    - Klicken Sie auf **Speichern**.

    - Klicken Sie im Menü oben auf **Berechtigungen**. Klicken Sie auf **Setup**.

        ![Gehe zu Berechtigungen][24]

    - **Festlegen von SAM Single Sign-On**wählen Sie aus und dann auf **Hinzufügen**.

    - Klicken Sie auf **Speichern**.

    - Im oberen Navigationsbereich auf **Setup**klicken **Installations-Manager**.

        ![Fahren Sie mit dem Installations-Manager][10]

    - Im linken Navigationsbereich auf ausgewählte **Benutzer/Rollen**, und klicken Sie auf **Benutzer verwalten**.

        ![Gehe zum Verwalten von Benutzern][25]

    - Wählen Sie einen Testbenutzer. Klicken Sie auf **Bearbeiten**.

        ![Gehe zum Verwalten von Benutzern][26]

    - Wählen Sie im Dialogfeld Rollen die Rolle, die Sie erstellt haben, und klicken Sie auf **Hinzufügen**.

        ![Gehe zum Verwalten von Benutzern][27]

    - Klicken Sie auf **Speichern**.

19. Testen Ihrer Konfiguration finden Sie im Abschnitt unten [NetSuite Benutzer zuweisen](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Schritt 3: Automatisierte Bereitstellung von Benutzer

> [AZURE.NOTE] Standardmäßig werden Root Tochtergesellschaft der NetSuite Umgebung bereitgestellten Benutzer hinzugefügt.

1. Azure Active Directory auf Schnellstart für NetSuite, klicken Sie auf **Konfigurieren Benutzer bereitstellen**.

    ![Konfigurieren Sie Benutzer bereitstellen][28]

2. Das Dialogfeld Geben Sie die Administratoranmeldeinformationen für NetSuite und dann auf **Weiter**.

    ![Geben Sie die Administratoranmeldeinformationen NetSuite.][29]

3. Geben Sie auf der Bestätigungsseite Ihre e-Mail-Adresse der Bereitstellung Fehler Benachrichtigungen.

    ![Bestätigen.][30]

4. Klicken Sie auf **vollständig** , um das Dialogfeld zu schließen.

##<a name="step-4-assign-users-to-netsuite"></a>Schritt 4: Zuweisen von Benutzern zu NetSuite

1. Starten Sie Ihre Konfiguration einen Test-Account im Verzeichnis erstellen.

2. Klicken Sie auf der Seite NetSuite Schnellstart auf **Benutzer zuweisen** .

    ![Klicken Sie auf Benutzer zuweisen][31]

3. Wählen Sie Ihre Testbenutzer und klicken Sie **weisen** am unteren Bildschirmrand:

 - Wenn Sie nicht aktivieren Sie benutzerbereitstellung automatisierter sehen die folgende Meldung zu bestätigen:

        ![Confirm the assignment.][32]

 - Wenn Automatisches provisioning Benutzer aktiviert haben, dann sehen Sie aufgefordert, die definieren, welche Rolle der Benutzer in NetSuite muss. Nach einigen Minuten sollte neu eingerichtete Benutzer in Ihrer Umgebung NetSuite angezeigt.

4. Testen Sie die Einstellungen für einzelnen Zeichen, öffnen Sie die Abdeckung am [https://myapps.microsoft.com](https://myapps.microsoft.com/), Test-Account anmelden und auf **NetSuite**.

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
