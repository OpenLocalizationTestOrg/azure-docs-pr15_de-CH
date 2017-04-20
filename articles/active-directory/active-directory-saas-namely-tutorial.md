<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration: | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und zwar."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Lernprogramm: Azure Active Directory Integration:

Das Ziel dieses Lernprogramms ist nämlich Integration in Azure Active Directory (Azure AD) zeigen.

Integrieren nämlich in Azure AD bietet folgende Vorteile: 

- Sie können steuern, in Azure AD, die zwar Zugriff hat 
- Können die Benutzer automatisch angemeldet an: Abrufen (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfigurieren von Azure AD Integration: Sie benötigen Folgendes:

- Ein Azure AD-Abonnement
- Nämlich einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Nämlich hinzufügen aus der Galerie 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-namely-from-the-gallery"></a>Nämlich hinzufügen aus der Galerie
Konfigurieren die Integration: Azure AD müssen Sie die Liste der verwalteten SaaS-apps nämlich aus der Galerie hinzufügen.

**Um nämlich aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **:**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. Im Ergebnisbereich **nämlich**wählen Sie aus und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit nämlich basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in zu einem Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in nämlich hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in nämlich.
 
Konfigurieren und Testen von Azure AD müssen einmaliges Anmelden, Sie den folgenden Bausteinen durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer nämlich testen Benutzer](#creating-a-namely-test-user)** - eine Entsprechung von Britta Simon in nämlich, verknüpft ist der Azure AD Darstellung Ihres.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und einmaliges Anmelden in Ihrem nämlich Anwendung konfigurieren. 




**Konfigurieren von Azure AD folgendermaßen einmaliges Anmelden mit::**

1. Klicken Sie im klassischen Azure-Portal, auf die **nämlich** Application Integration auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer nämlich anmelden** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Auf der **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** den URL zur Ihre Benutzer Ihre Anwendung zwar anmelden (z.B.: *https://fabrikam.Namely.com/*).

    b. Klicken Sie auf **Weiter**.
 
 
4. Auf der Seite **Konfigurieren Sie einmaliges Anmelden an:** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


1. In einem anderen Browserfenster, melden Sie sich auf der Website als Administrator nämlich Unternehmens.

1. Klicken Sie in der oberen Symbolleiste auf **Unternehmen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Klicken Sie auf **die Registerkarte** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Klicken Sie auf **SAML**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Auf der Einstellungsseite **SAML** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    ein. Klicken Sie auf **SAML aktivieren**. 

    b. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren Sie einmaliges Anmelden am nämlich** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieter DDO Url** . 

    c. Öffnen Sie heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt und fügen Sie ihn in das Textfeld **Identitätszertifikat Anbieter** .    

    d. Klicken Sie auf **Speichern**.


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]


**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-namely-test-user"></a>Erstellt ein Benutzer nämlich testen

Dieser Abschnitt soll Benutzer Britta Simon in nämlich erstellen.

**Gehen Sie zum Erstellen eines Benutzers in nämlich Britta Simon aufgerufen:**

1. Anmeldung Ihr nämlich Unternehmensstandort als Administrator.

1. Klicken Sie in der oberen Symbolleiste auf **Personen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Klicken Sie auf die Registerkarte **Verzeichnis** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Klicken Sie auf **neue Person hinzufügen**.



1. Gehen Sie im Dialogfeld **Neue Person hinzufügen** :

    ein. Geben Sie im Textfeld **Vorname** **Britta**.

    b. Geben Sie im Textfeld **Nachname** **Simon**.

    c. Geben Sie im Textfeld **E-Mail** Adresse Brittas klassischen Azure-Portal.

    d. Klicken Sie auf **Speichern**.





### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon Azure einmaliges Anmelden verwenden, nämlich den Zugang zu gewähren.

![Benutzer zuweisen][200] 

**Zuweisen von Britta Simon, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. In der Anwendungsliste auswählen **:**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf die nämlich in der Kachel, Sie sollten erhalten automatisch angemeldet an Ihre nämlich Anwendung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






