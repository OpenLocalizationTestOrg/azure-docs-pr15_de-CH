<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit TOPdesk - öffentliche | Microsoft Azure" 
    description="Lernen Sie TOPdesk - mit Azure Active Directory einmaliges, automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Lernprogramm: Azure Verzeichnisintegration mit TOPdesk - öffentlich

Das Ziel dieses Lernprogramms ist die Integration von Azure und TOPdesk - öffentliche anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein TOPdesk - öffentliche SSO-Abonnement aktiviert
  
Am Ende dieses Lernprogramms Azure AD-Benutzer zugeordneten TOPdesk - öffentliche werden einzelne Zeichen in der Anwendung an die TOPdesk - öffentliche Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für TOPdesk - öffentlich
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Szenario")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>Die Application Integration für TOPdesk - öffentlich
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für TOPdesk - öffentliche aktivieren.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>Anwendungsintegration für TOPdesk - aktivieren öffentlicher, gehen Sie folgendermaßen vor:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **TOPdesk - Public**.

    ![Anwendung Galerie] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **TOPdesk - öffentliche aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Öffentliche TOPdesk] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "Öffentliche TOPdesk")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer TOPdesk - mit ihr Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren des einmaligen Anmeldens für TOPdesk - öffentliche erfordert eine Symboldatei Logo hochladen. Um die Symboldatei zu erhalten, wenden Sie die TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Melden Sie sich auf Ihre Site **TOPdesk - öffentliche** Unternehmen als Administrator an.

2.  **Klicken Sie im Menü **TOPdesk** klicken.**

    ![Standardeinstellungen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Standardeinstellungen")

3.  Klicken Sie auf **Anmeldung**.

    ![Anmeldedaten] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Anmeldedaten")

4.  Erweitern Sie das Menü **Anmeldedaten** und klicken Sie dann auf **Allgemein**.

    ![Allgemein] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Allgemein")

5.  Folgendermaßen Sie im **öffentlichen** Abschnitt des Konfigurationsabschnitts **SAML-Anmeldung** an:

    ![Technische Eigenschaften] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technische Eigenschaften")

    1.  Klicken Sie auf herunterladen öffentlicher Metadaten-Datei **herunterladen** und dann lokal auf Ihrem Computer speichern.
    2.  Öffnen Sie Metadaten-Datei, und suchen Sie den Knoten **AssertionConsumerService** .
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Kopieren Sie den Wert **AssertionConsumerService** .  

        >[AZURE.NOTE] Sie benötigen den Wert unter **App-URL konfigurieren** später in diesem Lernprogramm.

6.  Melden Sie in anderen Webbrowserfenster als Administrator Ihre **Azure-Verwaltungsportal** .

7.  Klicken Sie auf der Seite Application Integration **TOPdesk - öffentliche** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Einmaliges Anmelden konfigurieren")

8.  Auf der Seite **Wie möchten Sie Benutzer anmelden TOPdesk - öffentliche** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Einmaliges Anmelden konfigurieren")

9.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **TOPdesk - öffentliche Anmelde-URL** den URL der TOPdesk - öffentliche Anwendung anmelden von Benutzern verwendet (z. B.: "*https://qssolutions.topdesk.net*").
    2.  Fügen Sie im Textfeld **TOPdesk – öffentliche Antwort-URL** **TOPdesk - öffentliche AssertionConsumerService URL** (z. B.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klicken Sie auf **Weiter**.

10. Auf der Seite **Konfigurieren einmaliges Anmelden am TOPdesk - öffentliche** Herunterladen der Metadatendatei auf **Metadaten**und speichern Sie die Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Einmaliges Anmelden konfigurieren")

11. Erstellen Sie eine Zertifikatsdatei Schritte:

    ![Zertifikat] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Zertifikat")

    1.  Öffnen Sie die heruntergeladene Metadatendatei.
    2.  Erweitern Sie den Knoten **RoleDescriptor** , die eine **xsi: Type** der **eingezogen: ApplicationServiceType**.
    3.  Kopieren Sie den Wert der **X509Certificate** -Knoten.
    4.  Speichern Sie kopierten **X509Certificate** Wert lokal auf Ihrem Computer in einer Datei.

12. **Auf die TOPdesk - Aktiengesellschaft Website im Menü **TOPdesk** klicken.**

    ![Standardeinstellungen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Standardeinstellungen")

13. Klicken Sie auf **Anmeldung**.

    ![Anmeldedaten] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Anmeldedaten")

14. Erweitern Sie das Menü **Anmeldedaten** und klicken Sie dann auf **Allgemein**.

    ![Allgemein] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Allgemein")

15. Klicken Sie im **öffentlichen** Abschnitt auf **Hinzufügen**.

    ![SAML-Anmeldung] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML-Anmeldung")

16. Auf der Dialogseite **SAML-Konfigurationsassistenten** Schritte:

    ![SAML-Konfigurations-Assistent] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML-Konfigurations-Assistent")

    1.  Hochladen der heruntergeladenen Metadatendatei unter **Verbundmetadaten**, klicken Sie auf **Durchsuchen**.
    2.  Hochladen der Zertifikatsdatei unter **Zertifikat (RSA)**, klicken Sie auf **Durchsuchen**.
    3.  Klicken Sie zum Hochladen der Logodatei bekam von TOPdesk Support-Team unter **Logo-Symbols**auf **Durchsuchen**.
    4.  Geben Sie im Textfeld **Benutzer Name-Attribut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Geben Sie im Textfeld **Angezeigter Name** einen Namen für Ihre Konfiguration.
    6.  Klicken Sie auf **Speichern**.

17. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Einmaliges Anmelden konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer TOPdesk - Anmeldung aktivieren öffentlicher sie in TOPdesk - öffentlich bereitgestellt werden.  
Bei TOPdesk - öffentliche Bereitstellung ist eine manuelle Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich auf Ihre Site **TOPdesk - öffentliche** Unternehmen als Administrator an.

2.  Klicken Sie im Menü oben auf **TOPdesk \> neu \> Dateien \> Person**.

    ![Person] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Person")

3.  Gehen Sie im Dialogfeld neue Person:

    ![Neue Person] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Neue Person")

    1.  Klicken Sie auf die Registerkarte Allgemein.
    2.  Geben Sie im Textfeld Nachname den Nachnamen ein gültiges Azure Active Directory-Konto, das Sie bereitstellen möchten.
    3.  Wählen Sie eine **Site** für das Konto.
    4.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Alle anderen TOPdesk - Erstellungstools für öffentliche Benutzer Konto verwenden oder APIs von TOPdesk - öffentliche Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>TOPdesk - Benutzer zuweisen öffentlich, gehen Sie folgendermaßen vor:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **TOPdesk - öffentliche ** **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)