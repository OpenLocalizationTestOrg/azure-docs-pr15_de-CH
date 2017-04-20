<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Integration Druva | Microsoft Azure" 
    description="Erfahren Sie, wie mit Druva Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Lernprogramm: Azure Active Directory Integration Integration Druva

Das Ziel dieses Lernprogramms ist die Integration von Azure und Druva anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Druva einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Druva Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie Druva zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Druva
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-druva-tutorial/IC795084.png "Szenario")
##<a name="enabling-the-application-integration-for-druva"></a>Die Application Integration für Druva

Dieser Abschnitt soll beschreiben die Anwendungsintegration Druva aktivieren.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Druva:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-druva-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-druva-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-druva-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Druva**.

    ![Anwendung Galerie] (./media/active-directory-saas-druva-tutorial/IC795085.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Druva aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Druva Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

Die Anwendung Druva erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss.  
Der folgende Screenshot zeigt ein Beispiel dafür.

![SAML-Token-Attribute] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML-Token-Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Druva** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-druva-tutorial/IC795027.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Druva** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-druva-tutorial/IC795088.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Druva** URL zur Druva Anwendung anmelden von Benutzern verwendet (z. B.: "*https://cloud.druva.com/home/*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-druva-tutorial/IC795089.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Druva** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-druva-tutorial/IC795090.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Druva.

6.  Gehen Sie zu **verwalten \> Settings**.

    ![Standardeinstellungen] (./media/active-directory-saas-druva-tutorial/IC795091.png "Standardeinstellungen")

7.  Gehen Sie im Dialogfeld Optionen für einzelne Zeichen:

    ![Einzelne Druckseite anmelden Settings] (./media/active-directory-saas-druva-tutorial/IC795092.png "Einzelne Druckseite anmelden Settings")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Druva** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **ID Provider Anmelde-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Druva** **Remote Logout URL** -Wert, und fügen Sie ihn in das Textfeld **ID-Anbieter Abmelden URL** .
    3.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    4.  Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **ID Anbieter Zertifikat** Textbox einfügen
    5.  Klicken Sie auf **Speichern**, um **die Einstellungen** zu öffnen.

8.  Klicken Sie auf **der Einstellungsseite** **SSO-Token zu generieren**.

    ![Standardeinstellungen] (./media/active-directory-saas-druva-tutorial/IC795093.png "Standardeinstellungen")

9.  Klicken Sie im Dialogfeld **Single Sign-On Authentifizierungstoken** Schritte:

    ![Token für einmaliges Anmelden] (./media/active-directory-saas-druva-tutorial/IC795094.png "Token für einmaliges Anmelden")

    1.  Klicken Sie auf **Kopieren**.
    2.  Klicken Sie auf **Schließen**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-druva-tutorial/IC795095.png "Einmaliges Anmelden konfigurieren")

11. Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen.

    ![Attribute] (./media/active-directory-saas-druva-tutorial/IC795096.png "Attribute")

12. Fügen Sie erforderliche Attribut Mappings Schritte:

  	|Attributname|Attributwert|
  	|---|---|
  	|Datenträgerkonfiguration\_Auth\_token|<*Zwischenablage-Wert*>|

    1.  Jede Datenzeile in der Tabelle oben klicken Sie auf **Attribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.
    3.  Geben Sie im Textfeld **Attributwert** Attributwert für diese Zeile angezeigt.
    4.  Klicken Sie auf **abgeschlossen**.

13. Klicken Sie auf **Übernehmen**.
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer Druva anmelden können, müssen sie in Druva bereitgestellt werden.  
Bei Druva ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **Druva** Unternehmen als Administrator an.

2.  Gehen Sie zu **verwalten \> Benutzer**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-druva-tutorial/IC795097.png "Verwalten von Benutzern")

3.  Klicken Sie auf **neu erstellen**.

    ![Verwalten von Benutzern] (./media/active-directory-saas-druva-tutorial/IC795098.png "Verwalten von Benutzern")

4.  Gehen Sie im Dialogfeld neuen Benutzer erstellen:

    ![Erstellen neuer Benutzer] (./media/active-directory-saas-druva-tutorial/IC795099.png "Erstellen neuer Benutzer")

    1.  Geben Sie die e-Mail-Adresse und den Namen eines gültigen Benutzerkontos Azure Active Directory in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer erstellen**.

>[AZURE.NOTE] Können Sie alle anderen Druva Benutzer Konto Erstellungstools oder APIs von Druva Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Druva Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Druva **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-druva-tutorial/IC795100.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-druva-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
