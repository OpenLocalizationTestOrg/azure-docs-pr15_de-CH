<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit ArcGIS | Microsoft Azure" 
    description="Erfahren Sie, wie mit ArcGIS Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Lernprogramm: Azure Active Directory-Integration mit ArcGIS

Das Ziel dieses Lernprogramms ist die Integration von Azure und ArcGIS anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein ArcGIS SSO-Abonnement aktiviert

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der ArcGIS Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie ArcGIS zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für ArcGIS
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Szenario")
##<a name="enabling-the-application-integration-for-arcgis"></a>Die Application Integration für ArcGIS

Dieser Abschnitt soll beschreiben die Anwendungsintegration für ArcGIS aktivieren.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ArcGIS:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **ArcGIS**.

    ![Applcation-Galerie] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation-Galerie")

7.  Wählen Sie im Ergebnisbereich **ArcGIS aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer ArcGIS Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **ArcGIS** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ArcGIS** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Einmaliges Anmelden konfigurieren")

3.  Auf der Seite **Konfigurieren App-URL** im Textfeld **ArcGIS Zeichen In URL** Geben Sie die URL zur Benutzer melden Sie sich mit dem folgenden Muster "*https://company.maps.arcgis.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am ArcGIS** auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens ArcGIS.

6.  Klicken Sie auf **Bearbeiten**.

    ![Bearbeiten] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Bearbeiten")

7.  Klicken Sie auf **Sicherheit**.

    ![Sicherheit] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Sicherheit")

8.  Klicken Sie unter **Enterprise-Anmeldenamen** **Identitätsanbieter festgelegt**.

    ![Enterprise-Benutzernamen] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Enterprise-Benutzernamen")

9.  Führen Sie die folgenden Schritte auf der Konfigurationsseite **Identitätsanbieter festgelegt** :

    ![Identitätsanbieter festlegen] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Identitätsanbieter festlegen")

    1.  Geben Sie in das Textfeld Name den Namen Ihrer Organisation ein.
    2.  **Metadaten für Enterprise Identitätsanbieter mit angegeben werden**wählen Sie **Eine Datei**.
    3.  Klicken Sie zum Hochladen der heruntergeladenen Metadatendatei auf **Datei auswählen**.
    4.  Klicken Sie auf **Identitätsanbieter**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer ArcGIS anmelden können, müssen sie in ArcGIS bereitgestellt werden.  
Bei ArcGIS ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich bei Ihrem Mandanten **ArcGIS** .

2.  Klicken Sie auf **Mitglieder einladen**.

    ![Mitglieder einladen] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Mitglieder einladen")

3.  Spaltenelemente **Hinzufügen automatisch ohne e-Mail**und klicken Sie dann auf **Weiter**.

    ![Automatisches Hinzufügen von Mitgliedern] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Automatisches Hinzufügen von Mitgliedern")

4.  Folgendermaßen Sie auf der Seite **Mitglieder** Dialogfeld an:

    ![Hinzufügen und überprüfen] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Hinzufügen und überprüfen")

    1.  Geben Sie den **Vornamen**, **Nachnamen** und **E-Mail** eine gültige AAD Konto bereitstellen.
    2.  Klicken Sie auf **Hinzufügen und überprüfen**.

5.  Überprüfen Sie die eingegebenen Daten, und klicken Sie dann auf **Mitglieder hinzufügen**.

    ![Mitglied hinzufügen] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Mitglied hinzufügen")

>[AZURE.NOTE] Können Sie alle anderen ArcGIS Benutzer Konto Erstellungstools oder APIs von ArcGIS Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu ArcGIS:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **ArcGIS **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
