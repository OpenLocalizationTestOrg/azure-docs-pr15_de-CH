<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration SanSan | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und SanSan."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Lernprogramm: Azure Active Directory-Integration mit SanSan

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) SanSan integrieren.

Azure AD SanSan Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf SanSan Zugriff
- Sie können die Benutzer automatisch angemeldet-SanSan (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit SanSan benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SanSan Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Microsoft Microsoft Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. SanSan aus der Galerie hinzufügen
2. Konfigurieren und Testen von Microsoft Azure AD einmaliges Anmelden


## <a name="adding-sansan-from-the-gallery"></a>SanSan aus der Galerie hinzufügen
Konfigurieren Sie die Integration der SanSan in Azure AD müssen Sie der Liste der verwalteten SaaS-apps SanSan aus der Galerie hinzufügen.

**Um SanSan aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **SanSan**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **SanSan aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_06.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Microsoft Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen einen Testbenutzer namens "Britta Simon" Azure AD einmaliges Anmelden mit SanSan auf Microsoft.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in SanSan an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in SanSan hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in SanSan.

Konfigurieren und Testen Sie Microsoft Azure AD einmaliges Anmelden mit SanSan müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Microsoft Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Microsoft Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine SanSan testen](#creating-an-sansan-test-user)** - Gegenstück Britta Simon SanSan enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Microsoft Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD einmaliges Anmelden konfigurieren

In diesem Abschnitt Microsoft Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der SanSan-Anwendung konfigurieren.


**Konfigurieren Sie einmaliges Anmelden für Microsoft Azure AD mit SanSan, führen Sie die folgenden Schritte:**


1. Klicken Sie im klassischen Azure-Portal, auf der Seite **SanSan** Application Integration konfigurieren einmaliges Anmelden konfigurieren Single Sign-On-Dialogfeld geöffnet.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png) 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei SanSan** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** URL dem folgenden Muster:

  	| Umgebung             | URL |
  	| :--                     | :-- |
  	| PC-web                  | `https://ap.sansan.com/v/saml2/<company name>/acs`|
  	| Systemeigene Mobile Anwendung       | `https://internal.api.sansan.com/saml2/<company name>/acs` |
  	| Mobile Browsereinstellungen | `https://ap.sansan.com/s/saml2/<company name>/acs` |


    b. **Bezeichner** Textfeld Geben Sie die URL in dem folgenden Muster: 

  	| Umgebung             | URL |
  	| :--                     | :-- |
  	| PC-web                  | `https://ap.sansan.com/v/saml2/<company name>`|
  	| Systemeigene Mobile Anwendung       | `https://internal.api.sansan.com/saml2/<company name>` |
  	| Mobile Browsereinstellungen | `https://ap.sansan.com/s/saml2/<company name>` |


    c. Klicken Sie auf **Weiter**.

4. Auf der Seite **Konfigurieren einmaliges Anmelden am SanSan** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5.  Zu SSO für die Anwendung konfigurierten unterstützen SanSan Support-Team und sie konfigurieren. Bitte geben sie die folgenden Informationen:

    • Das heruntergeladene **Zertifikat**

    • Die **Identität Anbieter-ID**

    • **SAML SSO-URL**

    • **Einmaliges Service URL**

> [AZURE.NOTE] PC-Browser auch für Mobile-app und Mobile Browser mit PC Web festlegen. 


6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD

In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.
Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-sansan-test-user"></a>Erstellen eines Benutzers SanSan test

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in SanSan. SanSan Anwendung benötigt den Benutzer die Anwendung vor der SSO bereitgestellt werden. 


> [AZURE.NOTE] Möchten Sie einen Benutzer manuell erstellen oder batch-Benutzer, müssen Sie den SanSan-Support wenden.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges SanSan Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon SanSan zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SanSan**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

In diesem Abschnitt Testen Sie Ihr Microsoft Azure AD-auf Konfiguration mit der.
Beim Klicken auf die SanSan Fläche in der Sie sollte automatisch zur Anwendung SanSan angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_205.png
