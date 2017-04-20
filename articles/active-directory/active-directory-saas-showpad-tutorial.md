<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Showpad | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Showpad."
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


# <a name="tutorial-azure-active-directory-integration-with-showpad"></a>Lernprogramm: Azure Active Directory-Integration mit Showpad

Das Ziel dieses Lernprogramms ist, wie Showpad in Azure Active Directory (Azure AD) integriert werden.

Azure AD Showpad Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Showpad Zugriff
- Sie können die Benutzer automatisch angemeldet-Showpad (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - Azure Active Directory Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Showpad benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Showpad-Abonnement


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Showpad aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-showpad-from-the-gallery"></a>Showpad aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Showpad in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Showpad aus dem Katalog hinzufügen.

**Um Showpad aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Applikationen][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.
 
    ![Applikationen][4]

6. Geben Sie im Suchfeld **Showpad**.

    ![Applikationen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Showpad aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-Showpad basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Showpad einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Showpad hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Showpad.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit Showpad müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer Showpad testen Benutzer](#creating-a-showpad-test-user)** - Gegenstück Britta Simon Showpad enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Showpad.



**Konfigurieren Azure AD einmaliges Anmelden mit Showpad die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Showpad** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Showpad** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Gehen Sie auf der **App-Einstellungen konfigurieren** folgendermaßen vor, und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png) 


    ein. **Anmelde-URL** -Textfeld Geben Sie die URL verwendet Ihre Benutzer anmelden, der Showpad-Anwendung mit dem folgenden Muster:`https://<company name>.showpad.biz/login`

    b. Geben Sie im Textfeld **ID** der URL dem folgenden Muster:`https://<company name>.showpad.biz`

    c. Klicken Sie auf **Weiter**


4. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Showpad** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Anmeldung bei Ihrem Mandanten Showpad als Administrator.

6. Klicken Sie im Menü oben auf **Settings**.

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

7. Navigieren Sie zu "**Einmaliges Anmelden**", und klicken Sie auf "**Aktivieren**".
 
    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. Gehen Sie im Dialogfeld **Hinzufügen eines SAML 2.0-Dienstes** :

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 

    ein. Im Textfeld **Name** Geben Sie den Namen des Provider-ID (z.B.: Firmenname).

    b. Wählen Sie als **Metadaten Quelle** **XML**.

    c. Kopieren Sie den Inhalt der heruntergeladenen Metadaten-XML-Datei und fügen Sie ihn in das Textfeld **XML-Metadaten** .

    d. Wählen Sie **Konten für neue Benutzer beim Anmelden automatisch bereitstellen**.

    e. Klicken Sie auf **Senden**.


10. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]


11. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]






### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Zum Erstellen eines Testbenutzers Showpad in Azure AD Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png) 

    ein. Geben Sie im Textfeld **Benutzername** **BrittaSimon**.

    b. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie als **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.


### <a name="creating-a-showpad-test-user"></a>Erstellen einen Testbenutzer Showpad

Dieser Abschnitt soll Benutzer Britta Simon in Showpad erstellen. 

Showpad unterstützt Just-in-Time-Bereitstellung. Sie haben die Bereitstellung **[Konfigurieren von Azure AD einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**aktiviert. 

Es ist keine Aufgabe für Sie in diesem Abschnitt. 




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff auf Showpad aktivieren.

![Benutzer zuweisen][200]

**Britta Simon Showpad zuweisen, die folgenden Schritte:**

1. Klassischen Azure-Portal, im Menü auf der oberen klicken Sie auf **Programme**.

    ![Benutzer zuweisen][201] 

2. Klicken Sie in der Liste Programme auf **Showpad**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]




### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf die **Showpad** Fläche in der Sie sollte automatisch zur Anwendung Showpad angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png
