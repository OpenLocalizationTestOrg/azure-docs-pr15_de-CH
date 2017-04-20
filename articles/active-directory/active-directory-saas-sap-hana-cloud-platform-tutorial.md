<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit SAP HANA Cloud | Microsoft Azure" 
    description="Erfahren Sie, wie mit SAP HANA Cloudplattform Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Lernprogramm: Azure Active Directory-Integration mit SAP HANA Cloud-Plattform
  
Ziel dieses Lernprogramms ist die Integration von Azure und SAP HANA Cloud-Plattform angezeigt.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Konto SAP HANA Cloud-Plattform
  
Am Ende dieses Lernprogramms werden können sich mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Anwendung für einmaliges Azure AD-Benutzern, SAP HANA Cloudplattform zugewiesen haben.

>[AZURE.IMPORTANT]Bereitstellen Ihrer Anwendung oder eine Anwendung auf Ihrem Konto SAP HANA Cloudplattform einmaliges testen müssen. In diesem Lernprogramm wird eine Anwendung im Konto bereitgestellt.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für SAP HANA Cloud
2.  Konfigurieren des einmaligen Anmeldens
3.  Zuweisen einer Rolle zu einem Benutzer
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Szenario")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Die Application Integration für SAP HANA Cloud
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für SAP HANA Cloud aktivieren.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SAP HANA Cloud:

1.  Klicken Sie in der Azure-Verwaltungsportal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **SAP HANA Cloud-Plattform**.

    ![Anwendung Galerie] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **SAP HANA Cloud-Plattform aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![SAP Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer SAP HANA Cloud-Plattform mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat Ihrem Mandanten SAP HANA Cloud-Plattform hochladen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Integration **SAP HANA Cloud-Plattform** -Anwendung auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei SAP HANA Cloud-Plattform** **Microsoft Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Einmaliges Anmelden konfigurieren")

3.  Melden Sie sich in verschiedenen Webbrowserfenster auf SAP HANA Cloud-Plattform Cockpit am https://account an. \<Querformat Host\>.ondemand.com/cockpit (z.B.: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Klicken Sie auf die Registerkarte **vertrauen** .

    ![Vertrauen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Vertrauen")

5.  Im Abschnitt Verwaltung vertrauen Schritte:

    ![Abrufen von Metadaten] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Abrufen von Metadaten")

    1.  Klicken Sie auf die Registerkarte **Service-Anbieter** .
    2.  Klicken Sie die Metadatendatei SAP HANA Cloudplattform **Metadaten abzurufen**.

6.  Azure Active Verwaltungsportal auf **App-URL konfigurieren** gehen Sie und dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **Anmelde-URL** die URL der Anwendung **SAP HANA Cloud-Plattform** Anmelden von Benutzern verwendet. Dies ist die URL Konto einer geschützten Ressource in der Anwendung SAP HANA Cloud-Plattform. Die URL basiert auf dem folgenden Muster: https:// *\<ApplicationName\>\<Kontoname\>.\< Querformat Host\>.ondemand.com/\<Pfad\_,\_geschützt\_Resource\> * (z.B.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Dies ist die URL in der SAP HANA Cloud-Plattform-Anwendung, die Authentifizierung des Benutzers erfordert.

    2.  Öffnen Sie SAP HANA Cloudplattform Metadatendatei, und suchen Sie das **Ns3:AssertionConsumerService** -Tag.
    3.  Kopieren Sie den Wert des **Standortattributs** , und fügen Sie ihn in das Textfeld **SAP HANA Cloud Plattform Antwort-URL** .

7.  Auf der Seite **Konfigurieren einmaliges Anmelden am SAP HANA Cloud-Plattform** herunterladen Metadaten auf **Metadaten**und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Einmaliges Anmelden konfigurieren")

8.  Für SAP HANA Cloud-Plattform Cockpit im Abschnitt **Lokale Dienstanbieter** folgendermaßen:

    ![Verwaltung von Vertrauensstellungen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Verwaltung von Vertrauensstellungen")

    1.  Klicken Sie auf **Bearbeiten**.
    2.  **Konfiguration-Typ**die Option **Benutzerdefiniert**aus.
    3.  **Lokale Anbieternamen**übernehmen Sie den Standardwert.
    4.  Um einen **Signaturschlüssel** und ein **Zertifikat** generieren, klicken Sie auf **Schlüssel generieren**.
    5.  Wählen Sie als **Haupt Weitergabe** **deaktiviert**.
    6.  Wählen Sie als **Kraft Authentifizierung** **deaktiviert**.
    7.  Klicken Sie auf **Speichern**.

9.  Klicken Sie auf die Registerkarte **Vertrauenswürdige Identitätsanbieter** und **Vertrauenswürdigen Identitätsanbieter hinzufügen**klicken.

    ![Verwaltung von Vertrauensstellungen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Verwaltung von Vertrauensstellungen")

    >[AZURE.NOTE]Zum Verwalten der Liste der vertrauenswürdigen Identitätsanbieter müssen Sie benutzerdefinierte Konfigurationstyp im Abschnitt lokale Dienstanbieter ausgewählt haben. Standardtyp-Konfiguration müssen Sie eine nicht bearbeitbare und implizite Vertrauensstellung SAP ID-Dienst. Für keine haben nicht vertrauenswürdigen Einstellungen.

10. Klicken Sie auf die Registerkarte **Allgemein** , und klicken Sie auf **Durchsuchen** , um heruntergeladene Metadaten-Datei hochladen.

    ![Verwaltung von Vertrauensstellungen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Verwaltung von Vertrauensstellungen")

    >[AZURE.NOTE] Nach dem Metadaten-Datei hochladen, werden die Werte für **einzelne Zeichen in URL**und **Einzelne Logout URL** **Signaturzertifikat** automatisch ausgefüllt.

11. Klicken Sie auf die Registerkarte **Attribute** .

12. Auf der Registerkarte **Attribute** die folgenden Schritte:

    ![Attribute] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attribute")

    1.  Fügen Sie auf **Add Assertion-Based Attribut**die folgenden Attributen Behauptung basiert:

        |Assertion-Attribut| Principal-Attribut|
        |-------------------|--------------------|
        |http://Schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName|   Vorname|--------------------|--------------------|
        |http://Schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Surname|        Nachname|-----------|
        |http://Schemas.xmlsoap.org/ws/2005/05/Identity/Claims/EmailAddress|E-Mail|

    >[AZURE.NOTE]Die Konfiguration der Attribute hängt davon ab, wie die Anwendung(en) HCP entwickelt, d. h. welche Attribute auf SAML erwarten und unter welchem Namen (Prinzipal Attribut) sie dieses Attribut im Code zugreifen.
    >  
    >ein.  Das **Default-Attribut** im Screenshot wird nur zur Veranschaulichung. Es ist nicht erforderlich, das Szenario funktioniert.  
    >
    >b.  Die Namen und Werte für **Prinzipal Attribut** dem Screenshot, hängt davon ab, wie die Anwendung entwickelt wurde. Es ist möglich, dass die Anwendung eine andere Zuordnung erfordert.

13. Wählen Sie im klassischen Azure-Portal, auf Dialog **Konfigurieren einmaliges Anmelden am SAP HANA Cloudplattform** Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **abgeschlossen**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Einmaliges Anmelden konfigurieren")
  
Als optionalen Schritt können Sie Assertion Gruppen für Ihre Azure Active Directory Identitätsanbieter konfigurieren.

>[AZURE.NOTE]Gruppen können auf SAP HANA Cloud-Plattform Sie eine oder mehrere Rollen in Ihrer SAP HANA Cloud-Plattform-Anwendung festgelegten Werte von Attributen Behauptung SAML 2.0 dynamisch einen oder mehrere Benutzer zuweisen. Beispielsweise die Assertion enthält das Attribut "*Vertrag temporäre =*", sollten Sie alle betroffenen Benutzer der Gruppe "*TEMPORARY*" hinzugefügt werden. Die Gruppe "*TEMPORARY*" enthalten eine oder mehrere Rollen mindestens eine Anwendung in Ihrem Konto SAP HANA Cloud-Plattform bereitgestellt.
>  
>Verwenden Sie Assertion Gruppen, wenn Sie viele Benutzer eine oder mehrere Rollen in Ihrem Konto SAP HANA Cloud-Plattform-Anwendung Masse zuweisen möchten. Nur eine einzelne oder kleine Anzahl von Benutzern (eine) bestimmten Rollen zuweisen möchten sollten auf der Registerkarte "**Berechtigungen**" SAP HANA Cloudplattform Cockpit zuweisen.

##<a name="assigning-a-role-to-a-user"></a>Zuweisen einer Rolle zu einem Benutzer
  
Um Azure AD Benutzer in SAP HANA Cloud-Plattform zu aktivieren, müssen Sie Rollen in SAP HANA Cloud-Plattform sie zuweisen.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Um einem Benutzer eine Rolle zuweisen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem **SAP HANA Cloudplattform** Cockpit.

2.  Führen Sie die folgenden Schritte aus:

    ![Berechtigungen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Berechtigungen")

    1.  Klicken Sie auf **Autorisierung**.
    2.  Klicken Sie auf die Registerkarte **Benutzer** .
    3.  Geben Sie im Textfeld **Benutzer** die e-Mail-Adresse ein.
    4.  Klicken Sie auf **zuweisen** , um den Benutzer einer Rolle zuzuweisen.
    5.  Klicken Sie auf **Speichern**.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu SAP HANA Cloud-Plattform:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **SAP HANA Cloud-Plattform** auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)