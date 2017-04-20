<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Dropbox für Unternehmen | Microsoft Azure" 
    description="Erfahren Sie, wie mit Dropbox mit Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Lernprogramm: Azure Active Directory Integration Dropbox für Unternehmen
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Dropbox für Unternehmen anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Test in Dropbox für Unternehmen
  
Am Ende dieses Lernprogramms, Unternehmen Sie Ablage zugewiesen haben, für Unternehmen einmaliges in der Anwendung in der Ablage für Unternehmen kann Azure AD-Benutzer Site (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md).
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Dropbox für Unternehmen
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Szenario")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Die Application Integration für Dropbox für Unternehmen
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Dropbox für Unternehmen aktivieren.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Dropbox für Unternehmen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Dropbox für Unternehmen**.

    ![Anwendung Galerie] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Dropbox für Unternehmen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Dropbox für Unternehmen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox für Unternehmen")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Dropbox für Unternehmen mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat auf Ihrer Dropbox Business Mieter hochladen. Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Dropbox für Unternehmen** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Dropbox für Unternehmen** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Einmaliges Anmelden für konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ein. Anmeldung Ihrer Dropbox Business Mieter. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Einmaliges Anmelden für konfigurieren")

    b. Klicken Sie im Navigationsbereich auf der linken Seite auf **Verwaltungskonsole**. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Einmaliges Anmelden für konfigurieren")

    c. Klicken Sie in der **Verwaltungskonsole**im linken Navigationsbereich auf **Authentifizierung** . 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Einmaliges Anmelden für konfigurieren")

    d. Im Abschnitt **einmaliges** **einmaliges**aktivieren Sie und dann auf **Weitere** um diesen Abschnitt zu erweitern.  

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Einmaliges Anmelden für konfigurieren")

    e. Kopieren Sie die URL **können Benutzer durch Eingabe ihrer e-Mail-Adresse anmelden**oder sie können direkt zu wechseln. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Einmaliges Anmelden für konfigurieren")

    f. Fügen Sie der Azure-Verwaltungsportal im Textfeld URL **DropBox für Unternehmen anmelden** die URL ein. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Einmaliges Anmelden für konfigurieren")  



4. Auf der Seite **Konfigurieren einmaliges Anmelden bei Dropbox für Unternehmen** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.  

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Einmaliges Anmelden für konfigurieren")


5. Auf Ihrer Dropbox Business Mieter im Bereich **auf** **der Authentifizierungsseite** Schritte: 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Einmaliges Anmelden für konfigurieren")

    ein. Klicken Sie auf **erforderlich**.

    b. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden bei Dropbox für Unternehmen** **- im Seiten-URL** -Wert, und fügen Sie ihn in das Textfeld **URL anmelden** .


    c. Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat. 

    > [AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).


    d. Klicken Sie auf **"Zertifikat auswählen"** und wählen Sie die **Base64 - codierte Datei**.


    e. Klicken Sie auf **"Speichern"** , um die Konfiguration auf Ihre DropBox Business Mieter abzuschließen.


6. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Einmaliges Anmelden für konfigurieren")



##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Dieser Abschnitt soll aktivieren benutzerbereitstellung von Active Directory-Benutzerkonten dropbox für Unternehmen zu beschreiben.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **Dropbox für Business** Application Integration Dialogfeld **Benutzerbereitstellung konfigurieren** **Konfigurieren Benutzer bereitstellen** .

2. Die aktivieren benutzerbereitstellung dropbox für Seite, klicken Sie auf Enable benutzerbereitstellung anmelden dropbox mit Azure AD-Dialogfeld öffnen.  

    ![Benutzer bereitstellen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Benutzer bereitstellen")

3. Melden Sie sich im Dialogfeld **Anmelden dropbox mit Azure AD** in Ihrem Dropbox Business Mieter an. 

    ![Benutzer bereitstellen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Benutzer bereitstellen")



4. Klicken Sie auf **Allow** Azure AD Dropbox Zugriff gewähren. 

    ![Benutzer bereitstellen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Benutzer bereitstellen")



5. Um die Konfiguration zu beenden, klicken Sie auf **abschließen** .  

    ![Benutzer bereitstellen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Benutzer bereitstellen")




##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Um Dropbox für geschäftliche Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Dropbox für Unternehmen **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Ja")
  


Sie sollten jetzt 10 Minuten warten und überprüfen, ob das Konto dropbox für Unternehmen synchronisiert wurde.

Überprüfung zunächst können Sie den Bereitstellungsstatus überprüfen **Dashboard** auf der Seite **Dropbox für Business** Application Integration klassischen Azure-Portal.

![Benutzer zuweisen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Benutzer zuweisen")


Bereitstellung Zyklus erfolgreich abgeschlossenen Benutzer wird durch den zugehörigen Status angezeigt.

![Benutzer zuweisen] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Benutzer zuweisen")


So testen Sie die einzelnen Einstellungen anmelden, Öffnen der.
Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)




## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)