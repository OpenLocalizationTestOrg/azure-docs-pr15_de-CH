<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit YouEarnedIt | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und YouEarnedIt."
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


# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a>Lernprogramm: Azure Active Directory-Integration mit YouEarnedIt

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) YouEarnedIt integrieren.

Azure AD YouEarnedIt Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf YouEarnedIt Zugriff
- Sie können die Benutzer automatisch angemeldet-YouEarnedIt (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit YouEarnedIt benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein YouEarnedIt Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. YouEarnedIt aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-youearnedit-from-the-gallery"></a>YouEarnedIt aus der Galerie hinzufügen
Konfigurieren Sie die Integration von YouEarnedIt in Azure AD müssen Sie der Liste der verwalteten SaaS-apps YouEarnedIt aus dem Katalog hinzufügen.

**Um YouEarnedIt aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **YouEarnedIt**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_01.png)

7. Wählen Sie im Ergebnisbereich **YouEarnedIt aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit YouEarnedIt basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in YouEarnedIt an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in YouEarnedIt hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in YouEarnedIt.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit YouEarnedIt müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer YouEarnedIt testen Benutzer](#creating-a-predictix-price-reporting-test-user)** - Gegenstück Britta Simon YouEarnedIt enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der YouEarnedIt-Anwendung konfigurieren.


**Konfigurieren Azure AD einmaliges Anmelden mit YouEarnedIt die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **YouEarnedIt** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei YouEarnedIt** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_04.png) 

    ein. **Anmelde-URL** -Textfeld Geben Sie die URL verwendet Ihre Benutzer anmelden, der YouEarnedIt-Anwendung mit dem folgenden Muster: 

    - Sandbox-Umgebung:`https://<company name>.sandbox.youearnedit.com/users/sign_in`
    - Produktions-Umgebung:`https://<company name>.youearnedit.com/users/sign_in`
    
    b. Klicken Sie auf **Weiter**
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden am YouEarnedIt** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_05.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Für die Anwendung konfigurierten SSO, wenden YouEarnedIt und bieten ihnen Folgendes:

    • Das heruntergeladene **Zertifikat**

    • **SAML SSO-URL**

6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf das **Benutzerprofil** , gehen Sie folgendermaßen vor: ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-youearnedit-test-user"></a>Erstellen eines Benutzers YouEarnedIt test

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in YouEarnedIt. Arbeiten Sie mit YouEarnedIt Support-Team, die Benutzer in der YouEarnedIt-Plattform hinzufügen.

> [AZURE.NOTE]  YouEarnedIt erwarten, dass der Identitätsanbieter eine EmailAddress oder Benutzername im NameID-Attribut angeben. Authentifizierung schlägt fehl, wenn eine entsprechende Benutzername EmailAddress in der Datenbank nicht gefunden ist oder nicht genau übereinstimmen. Dies erfordert das YouEarnedIt System vor der SSO-Integration (in der Regel entweder über API oder CSV-Import) Konten importiert werden.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges YouEarnedIt Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon YouEarnedIt zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **YouEarnedIt**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Klicken auf die YouEarnedIt Fläche in der Sie sollte automatisch zur Anwendung YouEarnedIt angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_205.png
