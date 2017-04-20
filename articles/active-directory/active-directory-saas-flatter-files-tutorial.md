<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit flacher | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und flacher Dateien."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Lernprogramm: Azure Active Directory-Integration mit flacher

Dieses Lernprogramm soll veranschaulichen flacher Dateien mit Azure Active Directory (Azure AD) integrieren.  
Integrieren von Azure AD flacher Dateien bietet Ihnen folgende Vorteile: 

- Sie können in Azure AD steuern, die flachere Dateien zugreifen 
- Sie können die Benutzer automatisch flacher Dateien (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - Active Directory Azure-Verwaltungsportal verwalten.

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit flacher Dateien benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein flacher Dateien Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Flacher Dateien aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-flatter-files-from-the-gallery"></a>Flacher Dateien aus der Galerie hinzufügen
Zum Konfigurieren der Integration flacher Dateien in Azure AD müssen Sie die Liste der verwalteten SaaS-apps flacher Dateien aus dem Katalog hinzufügen.

**Um flacher Dateien aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Flacher Dateien**.


    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Flacher Dateien aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden mit flacher Dateien auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in flacher Dateien an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer in flacher Dateien hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in flacher Dateien.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit flacher müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine flachere Dateien testen](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon flacher Dateien enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden in Azure AD-Verwaltungsportal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung flacher Dateien. Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen. Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

Konfigurieren Sie einmaliges Anmelden für flacher Dateien benötigen Sie eine registrierte Domäne. Haben Sie einen registrierten Domänennamen noch Kontakt flacher Dateien Supportteam über [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Konfigurieren Azure AD einmaliges Anmelden mit flacher Schritte:**

1. Klicken Sie im klassischen Azure AD-Portal auf Anwendungsseite Integration **Flacher Dateien** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden flacher Dateien** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Klicken Sie auf der **App-Einstellungen zu konfigurieren** auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Flachere Dateien verwendet die gleiche SSO-Anmelde-URL für alle Kunden: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Folgendermaßen Sie auf der Seite **Konfigurieren einmaliges flacher Dateien** an:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


1. Anmeldung flacher Dateien Anwendung als Administrator.

2. Klicken Sie auf Dashboard. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. **Klicken Sie**, und führen Sie die folgenden Schritte auf der Registerkarte **Unternehmen** : 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    ein. Wählen **Sie SAML 2.0 für die Authentifizierung**.

    b. Klicken Sie auf **SAML konfigurieren**.



2. Gehen Sie im Dialogfeld **SAML-Konfiguration** : 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    ein. Geben Sie im Textfeld Domain Ihre registrierte Domäne.

    > [AZURE.NOTE] Haben Sie einen registrierten Domänennamen noch Kontakt flacher Dateien Supportteam über [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. In der Azure Verwaltungsportal auf einzelne konfigurieren anmelden flacher Dateien Dialog kopte die Single Sign-On URL und fügen Sie ihn in das Textfeld Identität Provider-URL.

    c.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

    >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    d.  Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in **FlatterFiles Identität Anbieter Zertifikat** Textbox einfügen.

    e. Klicken Sie auf **Aktualisieren**.

6. Wählen Sie in Azure AD-Verwaltungsportal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Erstellen einen Testbenutzer flacher Dateien

Dieser Abschnitt soll Benutzer Britta Simon in flacher Dateien erstellen.

**Erstellen Sie einen Benutzer namens Britta Simon in flacher Dateien folgendermaßen Sie an:**

1. Melden Sie sich auf der Website Ihres Unternehmens **Flacher Dateien** als Administrator an.

2. Klicken Sie im Navigationsbereich auf der linken Seite **Klicken Sie**und klicken Sie dann auf Benutzer **Registerkarte**.

    ![Ein flacher Dateien Benutzer Cfreate](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Klicken Sie auf **Benutzer hinzufügen**. 

4. Gehen Sie im Dialogfeld **Benutzer hinzufügen** :

    ![Ein flacher Dateien Benutzer Cfreate](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.

    b. Geben Sie im Textfeld **Nachname** **Simon**. 

    c. Geben Sie im Textfeld **Adresse** Brittas e-Mail-Adresse im klassischen Azure-Portal.

    d. Klicken Sie auf **Senden**.   


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges flacher Dateien keinen Zugriff erteilen.

![Benutzer zuweisen][200] 

**Britta Simon flacher Dateien zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Flacher Dateien**.

    ![Benutzer zuweisen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Anklicken der Kachel flacher Dateien in der Sie sollte automatisch zur Anwendung flacher Dateien angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






