<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration & offen | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und & offen."
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
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Lernprogramm: Azure Active Directory Integration und offen

Dieses Lernprogramm soll eine Anleitung zum Integrieren mit Azure Active Directory (Azure AD) offen.

Integration und ehrlich mit Azure bietet die folgenden Vorteile:

- Sie können in Azure AD steuern hat Zugriff auf & offen
- Können die Benutzer automatisch angemeldet- & offen abgerufen (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfigurieren von Azure AD benötigen Integration und ehrlich gesagt, Folgendes:

- Ein Azure AD-Abonnement
- A & offen Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Hinzufügen & offen aus der Galerie
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-frankly-from-the-gallery"></a>Hinzufügen & offen aus der Galerie
Zum Konfigurieren der Integration und offen in Azure AD müssen hinzufügen & offen aus der Galerie zur Liste der verwalteten SaaS-apps.

**Hinzufügen & offen aus der Galerie folgendermaßen:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **& offen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **& offen**und **abgeschlossen** Anwendung hinzufügen klicken.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll konfigurieren und einzelnen Test Azure AD Anmelden mit & offen basierend auf einen Testbenutzer namens "Britta Simon" angezeigt.

Für einmaliges Anmelden zu arbeiten muss Azure AD Gegenstück Benutzer und an einen Benutzer in Azure AD offen ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer & offen hergestellt werden.

Diese Beziehung wird hergestellt, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** & offen.

Konfigurieren und Testen von Azure AD müssen einmaliges Anmelden mit &, Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer & offen Testbenutzer](#creating-a-&frankly-test-user)** - eine Entsprechung von Britta Simon & offen also mit Azure AD Darstellung Ihres.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren Sie einmaliges Anmelden in der & Anwendung offen.

**Azure konfigurieren AD-einem Anmelden mit &, gehen Sie folgendermaßen vor:**

1. Klicken Sie im Verwaltungsportal auf die offen Application Integrationsseite **und** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden & offen** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Möchten Sie auf der **App-Einstellungen konfigurieren** die Anwendung im **IDP initiiert Modus**konfigurieren, führen Sie die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    ein. Geben Sie im Textfeld **ID** eine URL dem folgenden Muster:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. Geben Sie im Feld **Antwort-URL** eine URL dem folgenden Muster:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c. Klicken Sie auf **Weiter**

4. Möchten Sie die Anwendung auf der **App-Einstellungen konfigurieren** im **SP initiiert Modus** konfigurieren, klicken Sie auf **"Show advanced Settings (optional)"** und geben Sie die **Anmelde-URL** , und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Beachten Sie, dass diese nicht die tatsächlichen Werte. Sie müssen diese Werte mit der tatsächlichen Anmelde-URL Bezeichner und Antwort-URL aktualisieren. Kontakt [help@andfrankly.com](emailTo:help@andfrankly.com) , diese Werte abzurufen.

5. Auf die **Konfigurieren Sie einmaliges Anmelden in & offen** Seite, führen die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

6. Für die Anwendung konfigurierten SSO erhalten Sie Ihre & offen-Supportteam über [help@andfrankly.com](emailTo:help@andfrankly.com). Anfügen der heruntergeladenen Metadatendatei und gemeinsam mit team offen SSO auf Seite einrichten.

7. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-frankly-test-user"></a>Erstellen einer & offen Testbenutzer

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon & offen. Arbeiten Sie mit & offen-Supportteam über [help@andfrankly.com](emailTo:help@andfrankly.com) zum Hinzufügen von Benutzern in & offen Plattform.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges & offen ihre Zugriff aktivieren.
    
![Benutzer zuweisen][200]

**Britta Simon & offen zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Liste Programme **und offen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Wenn Sie auf & offen in der Kachel, erhalten automatisch angemeldet an Ihre & offen Anwendung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
