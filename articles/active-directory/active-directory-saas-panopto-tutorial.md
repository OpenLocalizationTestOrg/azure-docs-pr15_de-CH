<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Panopto | Microsoft Azure" 
    description="Erfahren Sie, wie mit Panopto in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Lernprogramm: Azure Active Directory-Integration mit Panopto
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Panopto anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Panopto Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der Panopto Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Panopto zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Panopto
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Szenario")
##<a name="enabling-the-application-integration-for-panopto"></a>Die Application Integration für Panopto
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Panopto aktivieren.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Panopto:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Panopto**.

    ![Appkication-Galerie] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Appkication-Galerie")

7.  Wählen Sie im Ergebnisbereich **Panopto aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Panopto mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Panopto** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Panopto** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **Panopto Zeichen In URL** den URL dem folgenden Muster "https://*\<Name Mieters\>. Panopto.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Konfigurieren von app-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Panopto** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens Panopto.

6.  Klicken Sie in der Symbolleiste auf der linken Seite auf **System**und dann auf **Identitätsanbieter**.

    ![System] (./media/active-directory-saas-panopto-tutorial/IC777670.png "System")

7.  Klicken Sie auf **Anbieter**.

    ![Identitätsanbieter] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Identitätsanbieter")

8.  Im Abschnitt Anbieter SAML Schritte:

    ![SaaS-Konfiguration] (./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS-Konfiguration")

    1.  Wählen Sie aus der Liste **Typ** **SAML20**
    2.  Geben Sie im Textfeld **Name der Instanz** einen Namen für die Instanz.
    3.  Geben Sie im Textfeld **Beschreibung** eine Beschreibung ein.
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Panopto** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **Aussteller** .
    5.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Panopto** Wert **SAML SSO-URL** und fügen Sie ihn in das Textfeld **Url der springen** .
    6.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    7.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **Öffentlichen Schlüssel**
    8.  Klicken Sie auf **Speichern**.
        ![Speichern] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Speichern")

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Gibt es keine Aufgabe Benutzer Panopto Bereitstellung konfigurieren.  
Wenn der zugewiesene Benutzer die Abdeckung mit Panopto anmelden, überprüft Panopto, ob der Benutzer vorhanden ist.  
Wenn es kein Benutzerkonto vorhanden noch, wird es automatisch vom Panopto erstellt.

>[AZURE.NOTE]Können Sie alle anderen Panopto Benutzer Konto Erstellungstools oder APIs von Panopto Bereitstellung Azure AD-Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Panopto:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Panopto **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)