<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Citrix GoToMeeting | Microsoft Azure" 
    description="Erfahren Sie, wie mit Citrix GoToMeeting Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Lernprogramm: Azure Active Directory-Integration mit Citrix GoToMeeting  
Betrifft: Azure

>[AZURE.TIP]Feedback, finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=522412).

Das Ziel dieses Lernprogramms ist die Integration von Azure und Citrix GoToMeeting anzeigen. In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Citrix GoToMeeting

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Anwendungsintegration für Citrix GoToMeeting aktivieren
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Konfiguration] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfiguration")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Anwendungsintegration für Citrix GoToMeeting aktivieren

Dieser Abschnitt soll beschreiben die Anwendungsintegration für Citrix GoToMeeting aktivieren.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Citrix GoToMeeting:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Eine Anwendung aus Galerie hinzufügen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Eine Anwendung aus Galerie hinzufügen")

6.  Geben Sie im **Suchfeld** **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  Klicken Sie im Ergebnisbereich wählen Sie **Citrix GoToMeeting aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer Citrix GoToMeeting Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat auf Ihrem Citrix GoToMeeting Mandanten hochladen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie auf der Seite Application Integration **Citrix GoToMeeting** **Konfigurieren einmaliges Anmelden** **Konfigurieren SINGLE SIGN-ON** -Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Einmaliges Anmelden aktivieren")

2.  Wählen Sie auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Citrix GoToMeeting** **Microsoft Azure AD einmaliges Anmelden**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Einmaliges Anmelden für konfigurieren")


3. Klicken Sie auf der Seite **Konfigurieren App Settings** auf **Weiter**. 

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Einmaliges Anmelden aktivieren")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Einmaliges Anmelden für konfigurieren")

5.  In einem anderen Browserfenster melden Sie sich bei Ihrem Citrix Organisation Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Klicken Sie auf der Registerkarte **Identitätsanbieter** , und führen Sie die folgenden Schritte aus:  

    ![SAML-setup] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-setup")

    ein. Wählen Sie **manuell**

    
    b. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** **-Im Seiten-URL** -Wert, und fügen Sie ihn in das Textfeld **Anmeldung Seiten-URL** . 

    
    c. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** den **URL der Abmelde** -Wert, und fügen Sie ihn in das Textfeld **URL der Abmeldeseite** .

    
    d. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Citrix GoToMeeting** **Entitäts-ID** -Wert und fügen Sie ihn in das Textfeld **Identität Anbieter Entitäts-ID** .

   
    e. Um das heruntergeladene Zertifikat hochzuladen, klicken Sie auf **Zertifikat hochladen**.

    
    f. Klicken Sie auf **Speichern**.

6.  Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Einmaliges Anmelden für konfigurieren")


7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.

    ![SAML-setup] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "SAML-setup")





##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Ziel dieses Abschnitts ist vor der Bereitstellung von Active Directory-Benutzerkonten, Citrix GoToMeeting aktivieren.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Citrix GoToMeeting** Dialogfeld **Benutzerbereitstellung konfigurieren** **Konfigurieren Benutzer bereitstellen** .

    ![Konfigurieren Benutzer bereitstellen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Konfigurieren Benutzer bereitstellen")

2.  Auf der Seite **Einstellungen und Administratoranmeldeinformationen** Schritte:

    ![Konfigurieren Benutzer bereitstellen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Konfigurieren Benutzer bereitstellen")

    ein. Geben Sie im Textfeld **Citrix GoToMeeting Admin-Benutzernamen** den Benutzernamen Administrator ein.

    
    b. Im Textfeld **Citrix GoToMeeting Administratorkennwort** das Administratorkennwort ein.

    
    c. Klicken Sie auf **Weiter**.

3.  Klicken Sie auf **der Bestätigungsseite** auf das Häkchen, um die Konfiguration zu speichern.

4.  Klicken Sie **Überprüfen** , um die Konfiguration zu überprüfen.


##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Um Citrix GoToMeeting Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Citrix GoToMeeting** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Ja")

Sie sollten jetzt 10 Minuten warten und überprüfen, ob das Konto dropbox für Unternehmen synchronisiert wurde.

Überprüfung zunächst können Sie den Bereitstellungsstatus überprüfen Dashboard in der auf der Seite **Citrix GoToMeeting** Application Integration klassischen Azure-Portal.

![Dashboard] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Dashboard")

Zyklus-Bereitstellung erfolgreich abgeschlossenen Benutzer wird durch den zugehörigen Status:

![Integrationsstatus] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Integrationsstatus")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der.

Weitere Informationen über die anzeigen Sie [Einführung der](https://msdn.microsoft.com/library/dn308586)
