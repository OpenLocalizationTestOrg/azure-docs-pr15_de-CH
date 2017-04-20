<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Keylight | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Keylight."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Lernprogramm: Azure Active Directory Integration Keylight

In diesem Lernprogramm lernen Sie Keylight in Azure Active Directory (Azure AD) integriert.

Integrieren von Azure AD Keylight bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Keylight Zugriff
- Sie können die Benutzer automatisch zum Keylight (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Keylight benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Ein Keylight Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Keylight aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-keylight-from-the-gallery"></a>Keylight aus der Galerie hinzufügen
Konfigurieren die Integration Keylight Azure AD müssen Sie der Liste der verwalteten SaaS-apps Keylight aus dem Katalog hinzufügen.

**Um Keylight aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Keylight**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Keylight aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Keylight basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Konfigurieren und Azure AD einmaliges Anmelden mit Keylight testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Keylight test](#creating-a-keylight-test-user)** - Gegenstück Britta Simon Keylight enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und einmaliges Anmelden in Ihrer Anwendung Keylight konfigurieren.


**Um mit Keylight Azure AD einmaliges Anmelden konfigurieren möchten, führen Sie die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Keylight** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 


2. Auf der Seite **Wie möchten Sie Benutzer anmelden Keylight** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    ein. Geben Sie im Textfeld Anmelde-URL die URL die Benutzer anmelden, der Keylight-Anwendung mit dem folgenden Muster: **"https://\<Firmenname\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Auf der Seite **Konfigurieren einmaliges Anmelden am Keylight** Schritte:
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. So aktivieren Sie SSO in Keylight:
 
    ein. Anmeldung bei Ihrem Konto Keylight als Administrator.

    b. Klicken Sie im Menü oben auf **Person**und wählen Sie **Keylight-Setup aus**.
       
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. Klicken Sie in der Strukturansicht auf der linken Seite auf **SAML**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. Klicken Sie im Dialogfeld **SAML-Einstellungen** auf **Bearbeiten**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Führen Sie auf der **SAML-Einstellungen bearbeiten** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/405.png) 

    ein. **SAML-Authentifizierung** auf **aktiv**festgelegt.


    b. Kopieren Sie in Azure AD-Verwaltungsportal **SAML SSO-URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieter Anmelde-URL** .

    c. Azure AD-Verwaltungsportal kopieren Sie **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieter Abmelden URL** .

    d. Klicken Sie auf **Datei auswählen** , um Ihre heruntergeladenen Keylight Zertifikat auswählen, und dann auf **Öffnen** Hochladen des Zertifikats.


    e. Legen Sie **SAML-Benutzer-Id** auf **NameIdentifier Element der Betreff-Anweisung**.
   
    f. Bereitstellen der **Keylight Service Provider mit dem folgenden Muster: **https://&lt;Company Name&gt;. keylightgrc.com**.

    g. **Auto - Benutzer** auf **aktiv**festgelegt.

    h. **Automatische Bereitstellung Kontotyp** auf **Benutzer**festgelegt.

    Ich. **Automatische Bereitstellung Sicherheitsrolle**wählen Sie **Standardbenutzer SAML aus**
   
    j. **Automatische Bereitstellung Sicherheit Config**wählen Sie **Standardkonfiguration Benutzer aus**
   
    k. Geben Sie in das Textfeld e-Mail-Attribut **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Geben Sie im Textfeld **Vorname Attribut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. Geben Sie im Textfeld **Last Name-Attribut** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Klicken Sie auf **Speichern**.
   
  
   
  
6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer in klassischen Azure-Portal namens Britta Simon.

Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]



**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-keylight-test-user"></a>Erstellen einen Testbenutzer Keylight

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Keylight. Keylight unterstützt Just-in-Time-Bereitstellung ist standardmäßig aktiviert.

Es ist keine Aufgabe für Sie in diesem Abschnitt. Ein neuer Benutzer wird erstellt, wenn Keylight zugreifen, wenn der Benutzer noch nicht existiert. 

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie den Keylight-Support wenden.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Keylight Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon Keylight zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Keylight**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Anklicken der Kachel Keylight in der Sie sollte automatisch zur Anwendung Keylight angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
