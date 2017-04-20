<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit CloudPassage | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und CloudPassage."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Lernprogramm: Azure Active Directory-Integration mit CloudPassage

Das Ziel dieses Lernprogramms ist, wie CloudPassage in Azure Active Directory (Azure AD) integriert werden.  
Azure AD CloudPassage Integration bietet Ihnen folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf CloudPassage Zugriff 
- Sie können die Benutzer automatisch angemeldet-CloudPassage (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - Azure Active Directory verwalten. 


Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit CloudPassage benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein CloudPassage Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Haben Sie eine Umgebung für Azure AD, erhalten Sie eine kostenlose Testversion der einmonatigen Azure [hier](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. CloudPassage aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-cloudpassage-from-the-gallery"></a>CloudPassage aus der Galerie hinzufügen
Konfigurieren Sie die Integration von CloudPassage in Azure AD müssen Sie der Liste der verwalteten SaaS-apps CloudPassage aus dem Katalog hinzufügen.

### <a name="to-add-cloudpassage-from-the-gallery-perform-the-following-steps"></a>Um CloudPassage aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 
 
    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **CloudPassage**.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **CloudPassage aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-CloudPassage basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in CloudPassage einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in CloudPassage hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in CloudPassage.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit CloudPassage müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelne Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer CloudPassage testen Benutzer](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon CloudPassage enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden in Azure AD-Verwaltungsportal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung CloudPassage.  
Die CloudPassage-Anwendung erwartet SAML-Assertionen in einem bestimmten Format hinzufügen benutzerdefiniertes Attribut Mappings der SAML-token Attribute Konfiguration erfordern. Der folgende Screenshot zeigt ein Beispiel dafür.

![Einmaliges Anmelden konfigurieren][21]

**Konfigurieren Azure AD einmaliges Anmelden mit CloudPassage die folgenden Schritte:**

1. Klicken Sie im klassischen Azure AD-Portal auf Anwendungsseite Integration **CloudPassage** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][7]

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei CloudPassage** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren][8]

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte: 

    ![Anwendung konfigurieren][9]
 
    ein. Geben Sie im Textfeld **Anmelde-URL** die URL von Benutzern für Ihre Anwendung CloudPassage anmelden (z.B.: *https://portal.cloudpassage.com/saml/init/accountid*). 

    b. Geben Sie im Feld **Antwort-URL** den URL AssertionConsumerService (z.B.: *https://portal.cloudpassage.com/saml/consume/accountid*). Den Wert erhalten für dieses Attribut **SSO-Dokumentation** im Abschnitt **Einstellungen für einzelne Zeichen** CloudPassage Portal klicken.  
    ![Einmaliges Anmelden konfigurieren][10]

    C. Klicken Sie auf **Weiter**.



4. Auf der Seite **Konfigurieren einmaliges Anmelden am CloudPassage** auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert. 

    ![Einmaliges Anmelden konfigurieren][11]

5. In einem anderen Browserfenster Anmelden der Unternehmenswebsite CloudPassage als Administrator.

6. Klicken Sie im Menü oben **Klicken Sie**und dann auf **Site-Verwaltung**. 

    ![Einmaliges Anmelden konfigurieren][12]

7. Klicken Sie auf die Registerkarte **Authentifizierung** . 

    ![Einmaliges Anmelden konfigurieren][13]


8. Im Abschnitt **Einstellungen für einzelne Zeichen** folgendermaßen: 

    ![Einmaliges Anmelden konfigurieren][14]


    ein. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am CloudPassage** Wert **Aussteller URL** und fügen Sie ihn in das Textfeld **SAML Aussteller URL** .

    b. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am CloudPassage** **Service Provider (SP) initiiert Endpunkt** Wert und fügen Sie ihn in das Textfeld **SAML-Endpunkt-URL** .

    c. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am CloudPassage** **Logout URL** -Wert, und fügen Sie ihn in das Textfeld **Logout Zielseite** .

    d. Erstellen Sie eine **Base64 -** codierte Datei heruntergeladene Zertifikat. 
          
    >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

    e. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **x 509-Zertifikat** .

    f. Klicken Sie auf **Speichern**.


9. Azure AD-Verwaltungsportal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Einmaliges Anmelden konfigurieren][15]


10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**. 

    ![Einmaliges Anmelden konfigurieren][16]



11. Nun oben klicken Sie auf **Attribute** , um das **SAML-Token Attribute** zu öffnen. 

    ![Einmaliges Anmelden konfigurieren][17]

12. Fügen Sie den Benutzer folgendermaßen Attribute für jede Zeile in der Tabelle: 

  	| Attributname | Attributwert |
  	| --- | --- |
  	| Vorname | User.GivenName |
  	| Nachname | User.Surname |
  	| E-Mail | User.Mail |

 

    ein. Klicken Sie auf **Attribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][18]

    b. Geben Sie im Textfeld **Attributname** Attribut für diese Zeile und **Attribure**angezeigt, wählen Sie den Attributwert für die Zeile angezeigt. 

    ![Einmaliges Anmelden konfigurieren][19]
     
    c. Klicken Sie auf **abgeschlossen**.


13. Klicken Sie auf unten auf **Anwenden**. 

    ![Einmaliges Anmelden konfigurieren][20]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD

Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.
  
    c. Klicken Sie auf Weiter.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png) 
  
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  
  
    b. Im Feld **Nachname** Typ **Simon**.
  
    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.
  
    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
  
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.
  
    b. Klicken Sie auf **abgeschlossen**.   


  
 
### <a name="creating-a-cloudpassage-test-user"></a>Erstellen einen Testbenutzer CloudPassage

Dieser Abschnitt soll Benutzer Britta Simon in CloudPassage erstellen.

#### <a name="to-create-a-user-called-britta-simon-in-cloudpassage-perform-the-following-steps"></a>Gehen Sie zum Erstellen eines Benutzers Britta Simon in CloudPassage aufgerufen:

1.  Anmeldung bei der Website Ihres Unternehmens **CloudPassage** als Administrator. 

2.  **Klicken Sie in der oberen Symbolleiste**und dann auf **Site-Verwaltung**. 

    ![Erstellen einen Testbenutzer CloudPassage][22] 

3.  Klicken Sie auf die Registerkarte **Benutzer** , und klicken Sie dann auf **Neuen Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer CloudPassage][23]
    
4.  Abschnitt **Hinzufügen neuer Benutzer** folgendermaßen Sie fest: 

    ![Erstellen einen Testbenutzer CloudPassage][24]

    ein. Geben Sie im Textfeld **Vorname** Britta.

    b. Geben Sie im Textfeld **Nachname** Simon.

    c. Geben Sie in das Textfeld für den **Benutzernamen** das **E-Mail** -Textfeld und **Geben Sie e-Mail-** Textfeld Brittas Benutzernamen in Azure AD.

    d. Wählen Sie als **Zugriffstyp** **Halo Portal Zugriff aktivieren**.

    e. Klicken Sie auf **Hinzufügen**.










### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges CloudPassage Zugang gewähren können.

![Benutzer zuweisen][30]

**Britta Simon CloudPassage zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][26]

2. Wählen Sie in der Anwendungsliste **CloudPassage**.

    ![Benutzer zuweisen][27]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][25]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][29]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die CloudPassage Fläche in der Sie sollte automatisch zur Anwendung CloudPassage angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_01.png
[6]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_02.png
[7]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_05.png
[8]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_03.png
[9]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_04.png
[10]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png
[11]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_06.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[16]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_11.png
[17]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_10.png
[18]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_11.png
[19]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_12.png
[20]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_14.png
[21]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_14.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png
[25]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_15.png
[26]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_12.png
[27]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_13.png
[28]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[29]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_16.png
[30]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_17.png





















