<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Learningpool | Microsoft Azure" 
    description="Erfahren Sie, wie mit Learningpool in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Lernprogramm: Azure Active Directory-Integration mit Learningpool
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Learningpool anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Learningpool einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Learningpool Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Learningpool zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Learningpool
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Szenario")
##<a name="enabling-the-application-integration-for-learningpool"></a>Die Application Integration für Learningpool
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Learningpool aktivieren.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Learningpool:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Learningpool**.

    ![Anwendung Galerie] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Learningpool aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Learningpool mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.
  
Die Learningpool-Anwendung erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss.  
Der folgende Screenshot zeigt ein Beispiel dafür.

![SAML-Token-Attribute] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML-Token-Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Integration **Learningpool** Anwendung im Menü oben auf **Attribute** **SAML-Token-Eigenschaften** -Dialogfeld öffnen.

    ![Attribute] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Attribute")

2.  Fügen Sie erforderliche Attribut Mappings Schritte:

    ###  

  	|Attributname                |Attributwert            |
  	|------------------------------|---------------------------|

     Urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     Urn: oid:2.5.4.42|User.GivenName   
  	|Urn: oid:0.9.2342.19200300.100.1.3|User.Mail
  	|Urn: oid:2.5.4.4|User.Surname

    1.  Jede Datenzeile in der Tabelle oben klicken Sie auf **Attribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.
    3.  Wählen Sie aus **Attributwert** Attributwert für die Zeile angezeigt.
    4.  Klicken Sie auf **abgeschlossen**.

3.  Klicken Sie auf **Übernehmen**.

4.  Klicken Sie in Ihrem Browser auf **wieder** **das Dialogfeld** erneut öffnen.

5.  Klicken Sie auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Singel Anmelden konfigurieren] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Singel Anmelden konfigurieren")

6.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Learningpool** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Einmaliges Anmelden konfigurieren")

7.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Learningpool** URL zur Learningpool Anwendung anmelden von Benutzern verwendet (z.B.: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Konfigurieren von App-URL")

8.  Auf der Seite **Konfigurieren einmaliges Anmelden unter Learningpool** herunterladen Metadaten auf **Metadaten**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Einmaliges Anmelden konfigurieren")

9.  Der Metadatendatei Ihre Learningpool Support-Team weitergeleitet.

    >[AZURE.NOTE]Einmaliges Anmelden muss aktiviert werden, indem Learningpool Support-Team.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD Learningpool anmelden können, müssen sie in Learningpool bereitgestellt werden.
  
Gibt es keine Aufgabe Benutzer Learningpool Bereitstellung konfigurieren.  
Benutzer müssen Ihr Supportteam Learningpool erstellt werden.

>[AZURE.NOTE]Können Sie alle anderen Learningpool Benutzer Konto Erstellungstools oder APIs von Learningpool Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Learningpool:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Learningpool **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)