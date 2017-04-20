<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Skydesk E-Mail | Microsoft Azure"
    description="Dazu konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und e-Mail-Skydesk."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Lernprogramm: Azure Active Directory-Integration mit e-Mail-Skydesk

Das Ziel dieses Lernprogramms ist Azure Active Directory (Azure AD) Skydesk e-Mail-Integration zeigen.

Azure AD Skydesk e-Mail-Integration bietet folgende Vorteile:

- Sie können in Azure AD steuern, die Zugriff auf e-Mail-Skydesk
- Sie können die Benutzer automatisch Skydesk e-Mail (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - Active Directory Azure-Verwaltungsportal verwalten.

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Skydesk E-Mail benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine e-Mail-Skydesk Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. E-Mail-Skydesk aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-skydesk-email-from-the-gallery"></a>E-Mail-Skydesk aus der Galerie hinzufügen
Konfigurieren Sie die Integration der e-Mail-Skydesk in Azure AD müssen Sie der Liste der verwalteten SaaS-apps e-Mail-Skydesk aus der Galerie hinzufügen.

**Um e-Mail-Skydesk aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Skydesk E-Mail**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. Wählen Sie im Ergebnisbereich **Skydesk E-Mail aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Das Ziel dieses Abschnitts wird erläutert, wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit e-Mail-Skydesk namens "Britta Simon" Testbenutzer basierend auf.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück Skydesk e-Mail an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer Skydesk e-Mail hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Skydesk e-Mail.

Konfigurieren und Azure AD einmaliges Skydesk e-Mail testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen eine Skydesk E-Mail testen Benutzer](#creating-a-Skydesk-Email-test-user)** - Gegenstück Britta Simon Skydesk e-Mail haben, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in Skydesk e-Mail-Anwendung.



**Führen Sie die folgenden Schritte konfigurieren Azure AD SSO-Skydesk e-Mail:**

1. Klicken Sie im klassischen Azure-Portal, auf Integration **Skydesk e-Mail-** Anwendung auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer Skydesk E-Mail anmelden** **Azure AD einmaliges**wählen Sie aus und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    ein. Geben Sie im Textfeld Anmelde-URL die URL die Benutzer anmelden, der Skydesk e-Mail-Anwendung mit dem folgenden Muster: **"https://mail.skydesk.jp/portal/\<Firmenname\>"**.

    b. Klicken Sie auf **Weiter**.


4. Folgendermaßen Sie auf der Seite **Konfigurieren einmaliges Anmelden am e-Mail-Skydesk** an:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. So aktivieren Sie SSO **Skydesk**e-Mail:
 
    ein. Anmeldung Ihr Skydesk e-Mail-Konto als Administrator.

    b. Klicken Sie im Menü oben klicken Sie auf Setup, und wählen Sie Org. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. Klicken Sie im linken Bereich auf Domänen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klicken Sie auf Domäne hinzufügen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Geben Sie den Domänennamen und überprüfen Sie Domäne.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klicken Sie im linken Bereich auf **SAML-Authentifizierung**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Auf der Dialogseite **SAML-Authentifizierung** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] SAML-basierte Authentifizierung entweder muss **die Domäne überprüft** oder **Portal URL** Setup. Legen Sie das Portal URL mit dem eindeutigen Namen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    ein. Kopieren Sie in Azure AD-Verwaltungsportal **SAML SSO-URL** -Wert, und fügen Sie ihn in das Textfeld **Anmelde-URL** .

    b. Azure AD-Verwaltungsportal kopieren Sie **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **Logout** URL.

    c. **Ändern Kennwort URL** ist optional so leer.

    d. Klicken Sie auf **Schlüssel aus Datei erhalten** Ihre heruntergeladene Skydesk e-Mail-Zertifikat auswählen, und dann auf **Öffnen** Hochladen des Zertifikats.

    e. Wählen Sie als **Algorithmus** **RSA**.

    f. Klicken Sie auf **Ok,** um die Änderungen zu speichern.


7. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-skydesk-email-test-user"></a>Erstellen eines Benutzers Skydesk e-Mail-test

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon Skydesk e-Mail.

ein. Klicken Sie auf **Benutzerzugriff** linken Skydesk e-Mail, und geben Sie Ihren Benutzernamen. 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Benötigen Sie Bulk-Benutzer erstellen, müssen Sie den Skydesk e-Mail-Support wenden.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure SSO-Skydesk e-Mail-Zugang gewähren.

![Benutzer zuweisen][200] 

**Gehen Sie zum Zuweisen von Britta Simon Skydesk e-Mail:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
 
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Skydesk E-Mail**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf die Skydesk e-Mail-Kachel in der Sie sollte automatisch auf Ihr Skydesk e-Mail-Programm angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
