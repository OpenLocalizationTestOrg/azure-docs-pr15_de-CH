<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit RunMyProcess | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und RunMyProcess."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Lernprogramm: Azure Active Directory-Integration mit RunMyProcess

Das Ziel dieses Lernprogramms ist, wie RunMyProcess in Azure Active Directory (Azure AD) integriert werden.

Azure AD RunMyProcess Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf RunMyProcess Zugriff
- Sie können die Benutzer automatisch angemeldet-RunMyProcess (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit RunMyProcess benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein RunMyProcess Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. RunMyProcess aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess aus der Galerie hinzufügen
Konfigurieren Sie die Integration von RunMyProcess in Azure AD müssen Sie der Liste der verwalteten SaaS-apps RunMyProcess aus dem Katalog hinzufügen.

**Um RunMyProcess aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **RunMyProcess**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. Wählen Sie im Ergebnisbereich **RunMyProcess aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-RunMyProcess basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Einmaliges arbeiten Azure AD muss wissen, was Benutzer Gegenstück in RunMyProcess an einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in RunMyProcess hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in RunMyProcess.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit RunMyProcess müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer RunMyProcess testen Benutzer](#creating-a-runmyprocess-test-user)** - Gegenstück Britta Simon RunMyProcess enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der RunMyProcess-Anwendung konfigurieren.

**Konfigurieren Azure AD einmaliges Anmelden mit RunMyProcess die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **RunMyProcess** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei RunMyProcess** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Beachten Sie, dass den Wert mit dem tatsächlichen Anmelde-URL aktualisieren. Um diesen Wert zu erhalten, wenden Sie RunMyProcess über <mailto:support@runmyprocess.com>.
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden am RunMyProcess** auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. Anmelden Sie in anderen Webbrowserfenster Ihrem Mandanten RunMyProcess als Administrator.

6. Klicken Sie im linken Fensterausschnitt auf **Konto** , und wählen Sie **Konfiguration**.

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. **Authentifizierungsmethode** Abschnitt, und führen Sie folgende Schritte aus:

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    ein. Wählen Sie als **Methode** **SSO mit Samlv2**.

    b. **Umleiten von SSO** setzen Textbox **SAML SSO-URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    c. **Abmeldung umgeleitet** setzen Textbox **Einzelne Abmelde Service URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    d. Das **Namensformat Id** setzen Textbox **Namensformat Bezeichner** von Azure AD-Anwendung-Konfigurationsassistenten.

    e. Kopieren Sie den Inhalt der heruntergeladenen Datei, und fügen Sie ihn in das Textfeld **Zertifikat** . 

    f. Klicken Sie auf **Speichern** .
    
8. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

9. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf das **Benutzerprofil** , gehen Sie folgendermaßen vor: ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

### <a name="creating-a-runmyprocess-test-user"></a>Erstellen einen Testbenutzer RunMyProcess
  
Um Benutzer Azure AD RunMyProcess anmelden können, müssen sie in RunMyProcess bereitgestellt werden. Bei RunMyProcess ist die Bereitstellung einer manuellen Aufgabe.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site RunMyProcess Unternehmen als Administrator an.

2.  Klicken Sie auf **Konto** wählen Sie **Benutzer** im linken Navigationsbereich, und klicken Sie auf **Neuer Benutzer**.

    ![Neuer Benutzer] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Neuer Benutzer")

3.  Im Abschnitt **User Settings** die folgenden Schritte:

    ![Profil] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profil")

    ein. Geben Sie den **Namen** und die **E-mail** eine gültige AAD Konto bereitstellen in verknüpfte Textfelder.

    b. Wählen Sie eine **IDE Language**, eine **Sprache** und ein **Profil**.

    c. Wählen Sie **Konto erstellen E-mail an mich senden**.

    d. Klicken Sie auf **Speichern**.

    >[AZURE.NOTE] Können Sie alle anderen RunMyProcess Benutzer Konto Erstellungstools oder APIs von RunMyProcess Bereitstellung Azure Active Directory-Benutzerkonten.
    

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff auf RunMyProcess aktivieren.

![Benutzer zuweisen][200] 

**Britta Simon RunMyProcess zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **RunMyProcess**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Balken unten, klicken Sie auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Klicken auf die RunMyProcess Fläche in der Sie sollte automatisch zur Anwendung RunMyProcess angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
