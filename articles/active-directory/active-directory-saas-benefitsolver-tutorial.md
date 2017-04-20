<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Benefitsolver | Microsoft Azure"
    description="Erfahren Sie, wie mit Benefitsolver in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Lernprogramm: Azure Active Directory-Integration mit Benefitsolver

Das Ziel dieses Lernprogramms ist die Integration von Azure und Benefitsolver anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Benefitsolver einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können sich mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Anwendung für einmaliges Azure AD-Benutzer, die Benefitsolver zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Benefitsolver
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Szenario")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Die Application Integration für Benefitsolver

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Benefitsolver aktivieren.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Benefitsolver:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Benefitsolver**.

    ![Anwendung Galerie] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Benefitsolver aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Benefitsolver mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Die Benefitsolver-Anwendung erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss.  
Der folgende Screenshot zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Benefitsolver** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Benefitsolver** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Einmaliges Anmelden konfigurieren")

3.  Auf der Seite **Konfigurieren App Settings** Schritte:

    ![Anwendung konfigurieren] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Anwendung konfigurieren")

    1.  Geben Sie im Textfeld **Anmelde-URL** **http://azure.benefitsolver.com**.
    2.  Geben Sie im Feld **Antwort-URL** **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Klicken Sie auf **Weiter**.

4.  Auf der Seite **Konfigurieren einmaliges Anmelden unter Benefitsolver** herunterladen Metadaten auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Einmaliges Anmelden konfigurieren")

5.  Senden Sie die heruntergeladenen Metadatendatei Ihre Benefitsolver Support-Team

    >[AZURE.NOTE] Das Supportteam Benefitsolver bezieht SSO-Konfiguration.
Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Einmaliges Anmelden konfigurieren")

7.  Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen.

    ![Attribute] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attribute")

8.  Fügen Sie erforderliche Attribut Mappings Schritte:

    ![Attribute] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attribute")

  	|Attributname|Attributwert|
  	|---|---|
  	|ClientID|Sie möchten diesen Wert vom Benefitsolver-Supportteam.|
  	|ClientKey|Sie möchten diesen Wert vom Benefitsolver-Supportteam.|
  	|LogoutURL|Sie möchten diesen Wert vom Benefitsolver-Supportteam.|
  	|Personal-Nr|Sie möchten diesen Wert vom Benefitsolver-Supportteam.|

    1.  Jede Datenzeile in der Tabelle oben klicken Sie auf **Attribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.
    3.  Wählen Sie im Textfeld **Attributwert** Attributwert für die Zeile angezeigt.
    4.  Klicken Sie auf **abgeschlossen**.

9.  Klicken Sie auf **Übernehmen**.

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Benutzer Azure AD Benefitsolver anmelden können, müssen sie in Benefitsolver bereitgestellt werden.  
Bei Benefitsolver werden Mitarbeiter in der Anwendung über eine Zählung von Ihrem HRIS-System (in der Regel nachts) aufgefüllt.  

>[AZURE.NOTE] Können Sie alle anderen Benefitsolver Benutzer Konto Erstellungstools oder APIs von Benefitsolver Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Benefitsolver:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Benefitsolver **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
