<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit @Task| Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und @Task."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Lernprogramm: Azure Active Directory integration@Task

Dieses Lernprogramm soll wie Sie integrieren @Task Azure Active Directory (Azure AD).  
Integration von @Task mit Azure bietet die folgenden Vorteile: 

- Sie können steuern, in Azure AD zu greifen@Task
- Können Sie Ihre Benutzer, automatisch angemeldet an @Task (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfigurieren von Azure AD-Integration mit @Task, Sie Folgendes benötigen:

- Ein Azure AD-Abonnement
- Ein @Task Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. Hinzufügen von @Task aus der Galerie 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-task-from-the-gallery"></a>Hinzufügen von @Task aus der Galerie
So konfigurieren Sie die Integration von @Task in Azure AD müssen Sie hinzufügen @Task aus der Galerie zur Liste der verwalteten SaaS-apps.

**Hinzufügen @Task aus der Galerie folgendermaßen:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1] 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2] 

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3] 

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4] 

6. Geben Sie im Suchfeld **@Task**.

    ![Applikationen][5] 

7. Wählen Sie im Ergebnisbereich **@Task**, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden

Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit @Task basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Einmaliges arbeiten, Azure AD muss wissen, was den entsprechenden Benutzer in @Task Benutzer in Azure AD ist. In anderen Worten eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in @Task muss eingerichtet werden.   
Diese Beziehung hergestellt, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in @Task.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit @Task, müssen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer @Tasktest Benutzer](#creating-a-halogen-software-test-user) ** - eine Entsprechung von Britta Simon @Taskthat Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Dieser Abschnitt soll Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in die @Task Anwendung.

**Azure AD einmaliges Anmelden mit konfigurieren @Task, gehen Sie folgendermaßen vor:**

1. Im klassischen Azure-Portal auf die **@Task** Anwendungsintegration auf **Configure einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der **Wie möchten Sie Benutzer anmelden @Task ** **Seite und **Azure AD einmaliges Anmelden**auswählen**.

    ![Azure AD einmaliges Anmelden][7] 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Anwendung konfigurieren][8] 
 
     ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer anmelden, die @Task Anwendung (z.B.:*https://<Tenant name>.attask ondemand.com*).

     b. Klicken Sie auf **Weiter**.

4. Auf die **Konfigurieren Sie einmaliges Anmelden an @Task ** Seite auf **Metadaten**, Metadaten-Datei lokal auf Ihrem Computer speichern und klicken Sie dann auf **Weiter**.

    ![Was ist Azure AD verbinden][9] 



1. Anmelden an der @Task Website des Unternehmens als Administrator.

2. Werden Sie **einmaliges Anmelden Konfiguration**.


1. Klicken Sie im Dialogfeld **Single Sign-On** gehen

    ![Einmaliges Anmelden konfigurieren][23]

    ein. Wählen Sie als **Typ** **SAML 2.0**.

    b. Wählen Sie **Service-Anbieter**.

    c. Der Azure-Verwaltungsportal kopieren Sie **Remote Login-URL**, und fügen Sie ihn in das Textfeld **Benutzername Portal-URL** .

    d. Der Azure-Verwaltungsportal kopieren Sie **Einzelne Abmelde Service URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** .

    e. Der Azure-Verwaltungsportal **Ändern Kennwort URL**kopieren Sie und fügen Sie ihn in das Textfeld **Kennwort URL ändern** .

    f. Klicken Sie auf **Speichern**.

6. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Was ist Azure AD verbinden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Was ist Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-an-task-test-user"></a>Erstellen einer @Task Testbenutzer

Dieser Abschnitt soll erstellt einen Benutzer namens Britta Simon in @Task.


**Erstellt einen Benutzer namens Britta Simon in @Task, gehen Sie folgendermaßen vor:**

1. Melden Sie sich auf Ihre @Task Website des Unternehmens als Administrator.

2. Klicken Sie im Menü oben auf **Personen**.

3. Klicken Sie auf **neue hinzufügen**. 

4. Gehen Sie im Dialogfeld neue Person:

    ![Erstellen einer @Task Testbenutzer][21] 

    ein. Geben Sie im Textfeld **Vorname** "Britta".

    b. Geben Sie im Textfeld **Nachname** "Simon".

    c. Geben Sie in das Textfeld **E-Mail-Adresse** e-Mail in Azure Active Directory Britta Simon.

    d. Klicken Sie auf **Person hinzufügen**.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges ihr Zugriff auf @Task.

![Benutzer zuweisen][200] 

**Zuweisen von Britta Simon @Task, gehen Sie folgendermaßen vor:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **@Task**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die @Task Kacheln im Zugriff erhalten automatisch angemeldet an die @Task Anwendung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






