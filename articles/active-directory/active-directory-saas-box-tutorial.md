<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit | Microsoft Azure" 
    description="Erfahren Sie, wie mit Feld Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Lernprogramm: Azure Active Directory-Integration mit


  
Das Ziel dieses Lernprogramms ist die Integration von Azure und im Feld anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Test im Feld
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung an Ihrem Feld Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie im Feld zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Feld
2.  Konfigurieren des einmaligen Anmeldens
3.  Benutzer und Gruppe Bereitstellung konfigurieren
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-box-tutorial/IC769537.png "Szenario")



##<a name="enabling-the-application-integration-for-box"></a>Die Application Integration für Feld
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Feld aktivieren.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Feld:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-box-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-box-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-box-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie in das **Suchfeld** **ein**.

    ![Anwendung Galerie] (./media/active-directory-saas-box-tutorial/IC701023.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich **Aktivieren Sie**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Feld] (./media/active-directory-saas-box-tutorial/IC701024.png "Feld")



##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer ein Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können. Im Rahmen dieses Verfahrens müssen Sie Box.com Metadaten hinzufügen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite **im Feld** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-box-tutorial/IC769538.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer im Feld Anmelden** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-box-tutorial/IC769539.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Im Mieter URL** den URL im Feld Mieter (z.B.: https://<mydomainname>. box.com), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-box-tutorial/IC669826.png "Konfigurieren von app-URL")

4.  Klicken Sie auf der Seite **Konfigurieren einmaliges Feld** Metadaten **Metadaten**und die Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-box-tutorial/IC669824.png "Einmaliges Anmelden für konfigurieren")

5.  Weiterleiten der Metadatendatei Feld-Supportteam. Das Support-Team muss einmaliges Anmelden für Sie konfiguriert.

6.  Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-box-tutorial/IC769540.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Das Ziel dieses Abschnitts ist vor der Bereitstellung von Active Directory-Benutzerkonten im Feld aktivieren.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **im Feld** Application Integration auf Dialogfeld **Benutzerbereitstellung konfigurieren** **Konfigurieren Benutzer bereitstellen** . 

    ![Automatisiertes provisioning aktivieren] (./media/active-directory-saas-box-tutorial/IC769541.png "Automatisiertes provisioning aktivieren")

2. Aktivieren Sie auf der Seite **Bereitstellung ein Benutzer aktivieren** **Benutzer bereitstellen**. 

    ![Automatisiertes provisioning aktivieren] (./media/active-directory-saas-box-tutorial/IC769544.png "Automatisiertes provisioning aktivieren")

3. Anmeldeinformationen Sie auf der Seite **Anmelden gewähren Zugriff auf** erforderlichen und klicken Sie dann auf **Autorisieren**. 

    ![Automatisiertes provisioning aktivieren] (./media/active-directory-saas-box-tutorial/IC769546.png "Automatisiertes provisioning aktivieren")


4. Klicken Sie auf **Zugriff gewähren auf** dieser Vorgang autorisiert und klassischen Azure-Portal zurück. 

    ![Automatisiertes provisioning aktivieren] (./media/active-directory-saas-box-tutorial/IC769549.png "Automatisiertes provisioning aktivieren")


5. Auf der Seite " **Bereitstellung"** können die Kontrollkästchen **Objekttypen bereitstellen** auswählen, ob Objekte ein Benutzer Objekte bereitgestellt werden.  Weitere Informationen finden Sie unter "Zuweisen von Benutzern und Gruppen Abschnitt" unten.


6. Klicken Sie auf vollständig, um die Konfiguration abzuschließen. 

    ![Automatisiertes provisioning aktivieren] (./media/active-directory-saas-box-tutorial/IC769551.png "Automatisiertes provisioning aktivieren")



##<a name="assigning-a-test-user"></a>Testbenutzer zuweisen
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Um im Feld Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1. Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2. Klicken Sie auf der Seite **im Feld **Application Integration auf **Benutzer zuweisen**. 

    ![Benutzer zuweisen] (./media/active-directory-saas-box-tutorial/IC769552.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen. 

    ![Ja] (./media/active-directory-saas-box-tutorial/IC767830.png "Ja")
  
Sie sollten jetzt 10 Minuten und stellen Sie sicher, dass das Konto im Feld synchronisiert wurde.

Überprüfung zunächst können Sie den Bereitstellungsstatus überprüfen Dashboard in der auf der Seite im Feld Application Integration klassischen Azure-Portal.

![Dashboard] (./media/active-directory-saas-box-tutorial/IC769553.png "Dashboard")

Zyklus-Bereitstellung erfolgreich abgeschlossenen Benutzer wird durch den zugehörigen Status:

![Integrationsstatus] (./media/active-directory-saas-box-tutorial/IC769555.png "Integrationsstatus")


In Ihrem Mandanten im Feld aufgelisteten synchronisierte Benutzer **Verwaltet Benutzer** in der **Verwaltungskonsole**.

![Integrationsstatus] (./media/active-directory-saas-box-tutorial/IC769556.png "Integrationsstatus")


##<a name="assigning-users-and-groups"></a>Zuweisen von Benutzern und Gruppen

Die **im Feld > Benutzer und Gruppen** Registerkarte im klassischen Azure-Portal können Sie angeben, welche Benutzer und Gruppen im Feld zugreifen sollten. Zuweisung eines Benutzers oder einer Gruppe wird Folgendes durchgeführt:

* Azure AD ermöglicht zugewiesen (entweder durch direkte Zuweisung oder Gruppenmitgliedschaft) Feld authentifizieren. Wenn ein Benutzer nicht zugewiesen, Azure AD lässt sie Feld anmelden und auf der Azure AD-Seite wird ein Fehler zurückgegeben.

* Der Benutzer [Anwendungsstartprogramm](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)eine app-Kachel Feld hinzugefügt.

* Wenn die automatische Bereitstellung aktiviert ist, werden zugewiesene Benutzer bzw. Gruppen Bereitstellung Warteschlange automatisch bereitgestellt werden hinzugefügt.

    * Nur Benutzerobjekte für die Bereitstellung konfiguriert wurden, alle direkt zugewiesen werden in der Bereitstellung Warteschlange platziert, und alle Benutzer, die Mitglieder der zugewiesenen Gruppen in der Bereitstellung Warteschlange platziert. 
    
    * Gruppenobjekte für die Bereitstellung konfiguriert wurden, werden alle zugeordneten Gruppenobjekte bereitgestellt, Feld sowie alle Benutzer, die Mitglieder dieser Gruppen sind. Die Gruppen- und Mitgliedschaften erhalten auf Feld geschrieben wird.
    
Können die **Attribute > Single Sign-On** Registerkarte zum Konfigurieren der Benutzerattribute (oder Ansprüche) Feld SAML-Authentifizierung vorgestellt werden und die **Attribute > Bereitstellung** Registerkarte konfigurieren wie Benutzer- und Gruppenattribute von Azure AD Feld fließen während der Bereitstellung von Operations. Ressourcen unter Weitere Informationen anzeigen.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Anpassen des SAML-Tokens ausgestellte Ansprüche](active-directory-saml-claims-customization.md)
* [Bereitstellung: Attribute Mappings anpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)
