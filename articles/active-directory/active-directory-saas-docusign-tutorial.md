<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit DocuSign | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und DocuSign."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Lernprogramm: Azure Active Directory-Integration mit DocuSign

Das Ziel dieses Lernprogramms ist die Integration von Azure und DocuSign anzeigen.
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

- Ein gültiges Azure-Abonnement
- Ein Mieter DocuSign



In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1. [Die Application Integration für DocuSign](#enabling-the-application-integration-for-docusign) 


2. [Konfigurieren des einmaligen Anmeldens](#configuring-single-sign-on) 


3. [Konfigurieren der Bereitstellung](#configuring-account-provisioning) 


4. [Zuweisen von Benutzern](#assigning-users) 

    ![Konfigurieren des einmaligen Anmeldens][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Die Application Integration für DocuSign

Dieser Abschnitt soll beschreiben die Anwendungsintegration für DocuSign aktivieren.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für DocuSign:

1. Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Konfigurieren des einmaligen Anmeldens][1]

2. Wählen Sie aus der Liste Verzeichnis das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Konfigurieren des einmaligen Anmeldens][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Auf der Seite Was möchten Sie Dialogfeld, klicken **eine Anwendung aus dem Katalog hinzufügen**.

    ![Konfigurieren des einmaligen Anmeldens][4]


6. Geben Sie im Suchfeld **DocuSign**.

    ![Konfigurieren des einmaligen Anmeldens][5]

7. Klicken Sie im Ergebnisbereich wählen Sie **DocuSign aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Konfigurieren des einmaligen Anmeldens][6]


## <a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer DocuSign Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **DocuSign Anwendungsintegration** **Konfigurieren einmaliges Anmelden** konfigurieren Einmalanmeldung Dialogfeld öffnen.

    ![Konfigurieren des einmaligen Anmeldens][7]

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei DocuSign** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf Weiter.

    ![Konfigurieren des einmaligen Anmeldens][8]

3. Auf der Seite **Konfigurieren App Settings** Schritte:

    ![Konfigurieren des einmaligen Anmeldens][61]

    ein. Geben Sie im Textfeld **URL anmelden** `https://account.docusign.com/*`.  

    b. Geben Sie im Textfeld **ID** `https://account.docusign.com/*`.  
   
    c. Klicken Sie auf **Weiter**. 


    > [AZURE.TIP] Die Anmelde-URL und die ID-Werte sind nur Platzhalter. Anleitung zum Abrufen der tatsächlichen Werte für Ihre Umgebung werden später in diesem Thema behandelt.
 

4. Auf der Seite **Konfigurieren einmaliges Anmelden am DocuSign** auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Konfigurieren des einmaligen Anmeldens][10]


5. Melden Sie in anderen Webbrowserfenster als Administrator der **Administratorportal DocuSign** .


6. Klicken Sie im Navigationsmenü auf der linken Seite auf **Domänen**.

    ![Konfigurieren des einmaligen Anmeldens][51]

7. Klicken Sie im rechten Bereich auf **Antrag Domäne**.

    ![Konfigurieren des einmaligen Anmeldens][52]

8. Dialogfeld **Anspruch einer Domäne** in das Textfeld **Domänenname** Geben Sie Ihrer Unternehmensdomäne ein und dann auf **Antrag**. Stellen Sie sicher, dass Domäne überprüfen und der Status aktiv.

    ![Konfigurieren des einmaligen Anmeldens][53]

9. Klicken Sie im Menü auf der linken Seite **Identitätsanbieter**  

    ![Konfigurieren des einmaligen Anmeldens][54]

10. Klicken Sie im rechten Fensterbereich auf **Identitätsanbieter hinzufügen**. 
    
    ![Konfigurieren des einmaligen Anmeldens][55]

11. Auf der Seite **Identität Anbieter Settings** Schritte:

    ![Konfigurieren des einmaligen Anmeldens][56]


    ein. Geben Sie in das Textfeld **Name** einen eindeutigen Namen für Ihre Konfiguration. Verwenden Sie keine Leerzeichen.

    b. Kopieren Sie in der Azure-Verwaltungsportal Aussteller URL und fügen Sie ihn in das Textfeld **Identität Anbieter Aussteller** .

    c. Kopieren Sie in der Azure-Verwaltungsportal **Remote Login-URL**, und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .

    d. In der Azure-Verwaltungsportal **Remote Logout-URL**kopieren Sie und fügen Sie ihn in das Textfeld **Identität Anbieter Abmelden URL** .

    e. Wählen Sie **Zeichen AuthN Anforderung**.

    f. Wählen Sie als **Anforderung senden AuthN** **Bereitstellen**.

    g. Wählen Sie **Logout Anforderung senden** **Bereitstellen**. 


12. Wählen Sie im Abschnitt **Benutzerdefinierte Attribut zuordnen** Feld mit Azure AD zuordnen möchten. In diesem Beispiel wird mit dem Wert **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** **Emailaddress** Anspruch zugeordnet. Dies ist der Standardname für die Forderung von Azure AD für e-Mail-Anspruch. 

    > [AZURE.NOTE] Verwenden der entsprechenden **Benutzer-ID** den Benutzer von Azure AD Docusign Benutzerzuordnungsdatei zuzuordnen. Wählen Sie das richtige Feld, und geben Sie den entsprechenden Wert basierend auf der Organisation.

    ![Konfigurieren des einmaligen Anmeldens][57]

13. Im Abschnitt **Identität Anbieter Zertifikat** auf **Zertifikat hinzufügen**und Hochladen von Azure AD-Verwaltungsportal heruntergeladene Zertifikat.   

    ![Konfigurieren des einmaligen Anmeldens][58]

14. Klicken Sie auf **Speichern**.

15. Klicken Sie im Abschnitt **Identitätsprovider** auf **Aktionen**und dann auf **Endpunkte**.   

    ![Konfigurieren des einmaligen Anmeldens][59]



10. Auf der Azure-Verwaltungsportal zurück zur Einstellungsseite **Konfigurieren App** . 

16. **Verwaltungsportal von DocuSign**im Bereich **Ansicht SAML 2.0 Endpunkte** abgedeckt, die:

    ![Konfigurieren des einmaligen Anmeldens][60]

    ein. **Service Provider Aussteller URL**kopieren Sie und fügen Sie ihn in das Textfeld **Bezeichner** der klassischen Azure-Portal.

    b. Kopieren Sie der **Service Provider Anmelde-URL**, und fügen Sie in das Textfeld **Anmelde-URL** klassischen Azure-Portal.

    c.  Klicken Sie auf **Schließen**  


10. Klicken Sie auf den Azure-Verwaltungsportal auf **Weiter**. 


15. Klassischen Azure-Portal wählen Sie die **Konfiguration für einzelne Zeichen bestätigen**, und klicken Sie auf **Weiter**.

    ![Applikationen][14]

10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.

    ![Applikationen][15]
 

## <a name="configuring-account-provisioning"></a>Konfigurieren der Bereitstellung

Dieser Abschnitt soll aktivieren benutzerbereitstellung von Active Directory-Benutzerkonten DocuSign beschreiben.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1. Klicken Sie auf Dialogfeld Benutzerbereitstellung konfigurieren **Konfigurieren Bereitstellung** im **klassischen Azure-Portal**auf der Seite **DocuSign Anwendungsintegration** .

    ![Konfigurieren der Bereitstellung][30]

2. Auf der Seite **Einstellungen und Administratoranmeldeinformationen** automatische Benutzer bereitstellen kann, Anmeldeinformationen Sie eines DocuSign-Kontos mit der Berechtigung und klicken Sie dann auf **Weiter**. 

    ![Konfigurieren der Bereitstellung][31]

3. Dialogfeld " **Verbindung testen** " klicken Sie auf **Test starten**und nach erfolgreichem Test auf **Weiter**.

    ![Konfigurieren der Bereitstellung][32]

3. Klicken Sie auf **der Bestätigungsseite** auf **abgeschlossen**.

    ![Konfigurieren der Bereitstellung][33]
 

## <a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>DocuSign Benutzern zuweisen, die folgenden Schritte:

1. Erstellen Sie im **klassischen Azure-Portal**ein Testkonto.

2. Klicken Sie auf der Seite **DocuSign Anwendungsintegration** auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern][40]
 

3. Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Zuweisen von Benutzern][41]


Sie sollten jetzt 10 Minuten warten und überprüfen, ob das Konto mit DocuSign synchronisiert wurde.

Überprüfung zunächst können Sie den Bereitstellungsstatus überprüfen Dashboard in der auf der Seite DocuSign Application Integration klassischen Azure-Portal.

![Zuweisen von Benutzern][42]

Zyklus-Bereitstellung erfolgreich abgeschlossenen Benutzer wird durch den zugehörigen Status:

![Zuweisen von Benutzern][43]


So testen Sie die einzelnen Einstellungen anmelden, Öffnen der.

Weitere Informationen über die anzeigen Sie Einführung der


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png