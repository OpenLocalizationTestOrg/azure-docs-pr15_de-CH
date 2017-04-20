<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration 23 Video | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und 23 Video."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Lernprogramm: Azure Active Directory Integration 23 Video

Das Ziel dieses Lernprogramms ist, wie Azure Active Directory (Azure AD) 23 Video integriert werden.  
Integration von 23 bietet Video mit Azure folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf 23 Video Zugriff 
- Sie können die Benutzer automatisch mit ihren Azure AD-Konten 23 Video (Single Sign-On) angemeldete erhalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit 23 Video benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein 23 Video Single Sign-On Abonnement aktiviert


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. 23 Video hinzufügen aus der Galerie 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-23-video-from-the-gallery"></a>23 Video hinzufügen aus der Galerie
Konfigurieren Sie die Integration von 23 Video in Azure AD müssen Sie der Liste der verwalteten SaaS-apps 23 Video aus der Galerie hinzufügen.

**Um 23 Video aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **23 Video**.

    ![Applikationen][5]

7. Klicken Sie im Ergebnisbereich wählen Sie **23 Video aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges 23 Video basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in 23 Video an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer 23 Video hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in 23 Video.
 
Konfigurieren und Azure AD einmaliges 23 Video testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[23 Video erstellen Benutzer testen](#creating-a-23-video-test-user)** - Gegenstück Britta Simon 23 Video enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der 23 Video-Anwendung.

**Konfigurieren Azure AD einmaliges 23 Video Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **23 Video** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer sich auf 23 Video** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7] 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Azure AD einmaliges Anmelden][8] 
 
     ein. Geben Sie im Feld **Antwort-URL** die URL Ihrer 23 Video Site Anmelden von Benutzern verwendet (z.B.: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] Active Directory-Integration mit SAML 2.0 steht allen 23 Video. Wenden Sie sich an den Support unter [support@23company.com](mailto:support@23company.com) benötigen Sie die zugehörige Metadaten.

     b. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Konfigurieren einmaliges 23 Video** Schritte:

    ![Azure AD einmaliges Anmelden][9] 

    ein. Klicken Sie auf Zertifikat herunterladen und speichern Sie die Datei auf Ihrem Computer.

    b. Wenden Sie die 23 Video über [support@23company.com](mailto:support@23company.com), mit dem heruntergeladenen Zertifikat **Aussteller URL**, **Einzelne Sign-On Service URL**, **Einzelne Abmelde-URL**bereitstellen, und bitten Sie sie SSO für Ihre 23 Video einrichten. 

    c. Klicken Sie auf **Weiter**.


6. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-23-video-test-user"></a>23 Video Testbenutzer erstellen

Dieser Abschnitt soll Benutzer Britta Simon 23 Video erstellen.

**Gehen Sie zum Erstellen eines Benutzers Britta Simon in 23 Video aufgerufen:**

1. Melden Sie sich auf Ihre 23 Video Unternehmensstandort als Administrator an.

1. Gehe zu **Einstellungen**.


2. Klicken Sie im Abschnitt **Benutzer** **Konfigurieren**.

    ![Benutzer zuweisen][400]

3. Klicken Sie auf **neuen Benutzer hinzufügen**. 

    ![Benutzer zuweisen][401]

4. Im Abschnitt **Jemanden zu dieser Website** die folgenden Schritte:

    ![Benutzer zuweisen][402]

    ein. Geben Sie im Textfeld **e-Mail-Adressen** Adresse Britta Simon in Azure AD.

    b. Klicken Sie auf **Benutzer hinzufügen**.   


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges 23 Video Zugang gewähren.

![Benutzer zuweisen][200] 

**Um 23 Video Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **23 Video**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf 23 Video Kachel in der Sie sollte automatisch anwendungsspezifische Video 23 angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




