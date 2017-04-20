<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Trello | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Trello."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Lernprogramm: Azure Active Directory-Integration mit Trello

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) Trello integrieren.

Azure AD Trello Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Trello Zugriff
- Sie können die Benutzer automatisch angemeldet-Trello (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Trello benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein **Trello** Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Trello aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-trello-from-the-gallery"></a>Trello aus der Galerie hinzufügen
Konfigurieren Sie die Integration der Trello in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Trello aus der Galerie hinzufügen.

**Um Trello aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Trello**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Trello aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Trello basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in Trello an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Trello hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Trello. Konfigurieren und Testen Azure AD einmaliges Anmelden mit Trello müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Trello testen Benutzer](#creating-a-the-funding-portal-test-user)** - Gegenstück Britta Simon Trello enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Trello.

Trello erwartet SAML-Assertionen bestimmte Attribute enthalten. Konfigurieren Sie die folgenden Attribute für diese Anwendung. Sie können die Werte dieser Attribute auf der Registerkarte **"Atrributes"** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel dafür.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_03.png) 

**Konfigurieren Azure AD einmaliges Anmelden mit Trello die folgenden Schritte:**

1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **Trello** Anwendung im Menü oben.**
     
    ![Einmaliges Anmelden konfigurieren][5]


2. Klicken Sie im Dialogfeld **SAML-token Attribute** für jede Zeile in der Tabelle angezeigten folgendermaßen:
    

  	| Attributname | Attributwert |
  	| --- | --- |    
  	| User.Email | User.Mail |
  	| User.FirstName | User.GivenName |
  	| User.LastName | User.Surname |

    ein. Klicken Sie auf **Hinzufügen Benutzer Attribure** Dialogfeld **Attribut hinzufügen** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_05.png)
    
    b. Geben Sie im Textfeld **Attributname** Attributnamen für diese Zeile angezeigt.
    
    c. Wählen Sie aus **Attributwert** den Wert für die Zeile.
    
    d. Klicken Sie auf **abgeschlossen**. Dann **Änderungen** am unteren Rand der Seite.

3. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren][6]

4. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Trello** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][7] 

5. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Trello** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_06.png)

6. Auf der **App-Einstellungen konfigurieren** Wenn Sie die Anwendung im **IDP initiiert Modus**konfigurieren möchten, folgendermaßen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_07.png)


    ein. Geben Sie im Feld Antwort-URL eine URL dem folgenden Muster: `https://trello.com/auth/saml/consume/<enterprise>`.

    b. Klicken Sie auf **Weiter**.

> [AZURE.NOTE] Sie erhalten die ** \<Enterprise\> ** Slug aus Trello. Wenn Sie Slug-Wert haben, wenden Sie sich an Trello Support-Team <support@trello.com> zu den Infobereich zur Enterprise.

6. Wenn die Anwendung auf der **App-Einstellungen konfigurieren** im **SP initiiert Modus** konfigurieren möchten, klicken Sie auf **"Show advanced Settings (optional)"** und geben Sie die **Anmelde-URL**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_08.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster:`https://trello.com/auth/saml/consume/<enterprise>`

    b. Klicken Sie auf **Weiter**

7. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am Trello** auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_09.png)

6. Finden Sie für die Anwendung konfigurierten SSO [Trello Enterprise-SSO-Konfiguration](https://trello.com/sso-configuration) Seite senden Trello die Anmelde-URL und heruntergeladene Zertifikat.

7. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-trello-test-user"></a>Erstellen einen Testbenutzer Trello

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Trello. In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Trello. Trello unterstützt Just-in-Time-Bereitstellung und ein neues Konto wird beim ersten von Azure AD anmelden erstellt.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Trello Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon Trello zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Trello**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-trello-tutorial/tutorial_trello_10.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste alle Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf die Trello Fläche in der Sie sollte automatisch zur Anwendung Trello angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-trello-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-trello-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-trello-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-trello-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-trello-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-trello-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-trello-tutorial/tutorial_general_205.png
