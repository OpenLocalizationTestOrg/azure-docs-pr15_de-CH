<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Tableaus Server | Microsoft Azure"
    description="Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und Tableaus Server konfigurieren."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Lernprogramm: Azure Active Directory Integration Tableaus Server

Dieses Lernprogramm soll zeigen, wie Sie Azure Active Directory (Azure AD) Tableaus Server integrieren.

Integration von Azure AD Tableaus Server bietet folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf Tableaus Server
- Sie können die Benutzer automatisch Tableaus Server (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Tableaus Server benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Tableaus Server Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Tableaus Server aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-tableau-server-from-the-gallery"></a>Tableaus Server aus der Galerie hinzufügen
Zum Konfigurieren der Integration des Tableaus Servers in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Tableaus Server aus dem Katalog hinzufügen.

**Um Tableaus Server aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 
 
    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Tableaus Server**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Tableaus Server aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Ziel dieses Abschnitts wird erläutert, wie Sie konfigurieren und Testen Azure AD einmaliges Tableaus Server anhand des Namens "Britta Simon" Testbenutzer.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück Tableaus Server einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Tableaus Server hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Tableaus Server.

Konfigurieren und Testen Azure AD einmaliges Tableaus Server müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen eines Servers Tableaus Benutzer testen](#creating-a-tableauserver-test-user)** - Gegenstück Britta Simon Tableaus Server enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und einmaliges Anmelden in Ihrer Anwendung Tableaus Server konfigurieren.

Tableaus erwartet Server SAML-Assertionen in einem bestimmten Format. Der folgende Screenshot zeigt ein Beispiel dafür. 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Konfigurieren Azure AD einmaliges Tableaus Server die folgenden Schritte:**


1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **Tableaus Server** -Anwendung im Menü oben.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. Gehen Sie im Dialogfeld **SAML-token-Attribute** :

    

    ein. Klicken Sie auf **Hinzufügen Benutzer Attribure** Dialogfeld **Attribut hinzufügen** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. Geben Sie im Textfeld **Attrubute** **Benutzername**.

    c. Aus der **Attributwert** , Selsect **user.displayname**.

    d. Klicken Sie auf **abgeschlossen**.  
    



1. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Klicken Sie auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 



2. Auf der Seite **Wie möchten Sie Benutzer Tableaus Server anmelden** **Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Führen Sie auf der **App-Einstellungen konfigurieren** die folgenden Schritte aus, und **Klicken Sie auf**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    ein. Geben Sie im Textfeld **Zeichen In URL** die URL des Servers Tableaus. 

    b. Die Kennung im Feld kopieren die 

    c. Klicken Sie auf **Weiter**


4. Führen Sie auf der Seite **Konfigurieren einmaliges Tableaus Server** die folgenden Schritte aus, und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


6. Um SSO für die Anwendung konfiguriert wurde, müssen Sie Ihrem Mandanten Tableaus Server als Administrator anmelden.

    ein. Klicken Sie in der Serverkonfiguration Tableaus **SAML** -Registerkarte.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Aktivieren Sie das Kontrollkästchen **SAML für einmaliges Anmelden verwenden**.

    c. Die Verbundmetadaten Datei Azure-Verwaltungsportal heruntergeladen und **SAML Idp-Datei**hochladen.

    d. Tableaus Server Rückgabe-URL-der URL Tableaus Server-Benutzer, wie http://tableau_server zugreifen. Mit http://localhost wird nicht empfohlen. Über einen URL mit einem nachgestellten Schrägstrich (z. B. http://tableau_server/) wird nicht unterstützt. Kopieren Sie **Tableaus Server Rückgabe-URL** und Azure AD **Anmelde-URL** -Textfeld fügen Sie ein, wie in Schritt 3

    e. SAML Entitäts-ID-Element-ID eindeutig die Tableaus Server Installation IdP. Können Sie den Tableaus Server URL wieder hier eingeben, wenn Sie möchten, aber keinen der Tableaus Server URL sein. Kopieren Sie **SAML Entitäts-ID** und Azure AD **BEZEICHNER** Textfeld fügen Sie ein, wie in Schritt 3 gezeigt.

    f. Klicken Sie auf die **Metadatendatei exportieren** und im Text-Editor-Anwendung geöffnet. Assertion Consumer Service URL mit Http Post suchen und Index 0, und kopieren Sie die URL. Nun fügen Sie Azure AD **Antwort-URL** -Textfeld wie in Schritt 3 gezeigt. 

    g. Klicken Sie **OK** auf Tableaus Server Configiuration.

    > [AZURE.NOTE] Benötigen Sie Informationen zur Konfiguration von SAML auf Tableaus Server wenden Sie sich in diesem Artikel [SAML konfigurieren](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**. 
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.

    b. Geben Sie im Textfeld **Benutzername** **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-tableau-server-test-user"></a>Erstellen einen Testbenutzer Tableaus Server

Dieser Abschnitt soll Benutzer Britta Simon Tableaus Server erstellen. Sie müssen alle Benutzer Tableaus Server bereitstellen. Beachten Sie außerdem, Benutzernamen des Benutzers in Azure AD benutzerdefiniertes Attribut **UserName**konfigurierte Wert übereinstimmen. Die korrekte Zuordnung sollten die Integration [Konfigurieren von Azure AD einmaliges](#configuring-azure-ad-single-single-sign-on)funktionieren.

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie mit Tableaus Server Administrator in Ihrer Organisation.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Tableaus Server keinen Zugriff erteilen.

![Benutzer zuweisen][200] 

**Britta Simon Tableaus Server zuzuweisen, führen Sie die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
 
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Tableaus Server**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Anklicken der Kachel Tableaus Server in der Sie sollte automatisch anwendungsspezifische Tableaus Server angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
