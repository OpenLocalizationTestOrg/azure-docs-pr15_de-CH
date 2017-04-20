<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit am Arbeitsplatz | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und lernen am Arbeitsplatz."
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


# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a>Lernprogramm: Azure Active Directory-Integration mit am Arbeitsplatz

In diesem Lernprogramm lernen Sie lernen bei der Arbeit mit Azure Active Directory (Azure AD) integrieren.

Integration lernen bei der Arbeit mit Azure bietet die folgenden Vorteile:

- Sie können in Azure AD steuern lernen am Arbeitsplatz hat
- Sie können die Benutzer automatisch zu lernen am Arbeitsplatz (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit am Arbeitsplatz benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein lernen am Arbeitsplatz (Saba Cloud) Single-Sign-Abonnement aktiviert


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Lernen aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-learning-at-work-from-the-gallery"></a>Lernen aus der Galerie hinzufügen
Zum Konfigurieren der Integration des Lernens am Arbeitsplatz in Azure AD müssen Sie der Liste der verwalteten SaaS-apps lernen am Arbeitsplatz aus der Galerie hinzufügen.

**Gehen Sie zum Lernen am Arbeitsplatz aus der Galerie hinzufügen:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie in das Suchfeld **am Arbeitsplatz lernen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **lernen am Arbeitsplatz**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit lernen anhand Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Gegenstück Benutzer lernen am Arbeitsplatz für einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer lernen am Arbeitsplatz hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** lernen am Arbeitsplatz.

Konfigurieren und Testen Azure AD einmaliges Anmelden bei der Arbeit mit müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Benutzer erstellen lernen am Arbeitsplatz testen](#creating-a-predictix-price-reporting-test-user)** - Gegenstück Britta Simon lernen am Arbeitsplatz haben, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in Ihrem Learning Arbeit Anwendung konfigurieren.


**Konfigurieren Azure AD einmaliges Anmelden bei der Arbeit mit die folgenden Schritte:**

1. Klicken Sie im klassischen Portal auf der Seite **lernen am Arbeitsplatz** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer lernen am Arbeitsplatz anmelden** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** den URL verwendet, die Benutzer zum Anmelden an Ihr Learning Arbeit Anwendung dem folgenden Muster:`https://\<company name\>.sabacloud.com/Saba/Web/<company code>`
    
    b. Geben Sie im Textfeld **ID** die URL dem folgenden Muster: ' https://<company name>.sabacloud.com/Saba/SAML/sso/alias/<company name>``

    c. Klicken Sie auf **Weiter**
 
4. Auf der Seite **Konfigurieren einmaliges lernen am Arbeitsplatz** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_05.png)

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Für die Anwendung konfigurierten SSO, Learning Teams Support (Saba Cloud) erreichen und bieten ihnen Folgendes:

    • Die heruntergeladenen Metadaten

    • Der **Aussteller Url**

    • **SAML SSO-URL**

    • **Einmaliges Service URL**

6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf das **Benutzerprofil** , gehen Sie folgendermaßen vor: ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-learning-at-work-test-user"></a>Eine erstellen Arbeit Testbenutzer

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon lernen am Arbeitsplatz. Arbeiten Sie mit Arbeit Support Team Benutzer lernen zur Plattform hinzufügen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges lernen am Arbeitsplatz Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon lernen am Arbeitsplatz zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **lernen am Arbeitsplatz**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Klicken auf das Lernen an der Arbeit untereinander Sie sollte automatisch an Ihr Learning Arbeit Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_205.png
