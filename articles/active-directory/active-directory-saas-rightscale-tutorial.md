<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit RightScale | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und RightScale."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Lernprogramm: Azure Active Directory-Integration mit RightScale

Das Ziel dieses Lernprogramms ist, wie RightScale in Azure Active Directory (Azure AD) integriert werden.  
Azure AD RightScale Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf RightScale Zugriff
- Sie können die Benutzer automatisch angemeldet-RightScale (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit RightScale benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein RightScale Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. RightScale aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-rightscale-from-the-gallery"></a>RightScale aus der Galerie hinzufügen
Konfigurieren Sie die Integration der RightScale in Azure AD müssen Sie der Liste der verwalteten SaaS-apps RightScale aus der Galerie hinzufügen.

**Um RightScale aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.
 
    ![Applikationen][4]

6. Geben Sie im Suchfeld **RightScale**.
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **RightScale aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll konfigurieren und Testen Azure AD SSO-RightScale basierend auf einen Testbenutzer namens "Britta Simon" angezeigt.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in RightScale einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in RightScale hergestellt werden.  


Konfigurieren und Testen Azure AD einmaliges Anmelden mit RightScale müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer RightScale testen Benutzer](#creating-a-rightscale-test-user)** - Gegenstück Britta Simon RightScale enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung RightScale.



**Konfigurieren Azure AD einmaliges Anmelden mit RightScale die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **RightScale** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei RightScale** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_03.png) 

3. Möchten Sie auf der **App-Einstellungen konfigurieren** die Anwendung im **IDP initiiert Modus**konfigurieren, führen Sie die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_04.png) 


    ein. Geben Sie im Feld Antwort-URL den URL in das folgende Muster:`https://login.rightscale.com/login/saml2/consume`

    b. Klicken Sie auf **Weiter**

4. Möchten Sie die Anwendung auf der **App-Einstellungen konfigurieren** im **SP initiiert Modus** konfigurieren, klicken Sie auf **"Show advanced Settings (optional)"** und geben Sie die **Anmelde-URL** , und klicken Sie auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_06.png) 

    ein. Anmelde-URL-Textfeld Geben Sie die URL verwendet Ihre Benutzer anmelden, der RightScale-Anwendung mit dem folgenden Muster:`https://login.rightscale.com/`

    b. Klicken Sie auf **Weiter**

5. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden am RightScale** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie Base64-codierte Zertifikat auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Um SSO für die Anwendung konfiguriert wurde, müssen Sie Ihrem Mandanten RightScale als Administrator anmelden.

    ein. Klicken Sie im Menü oben klicken Sie auf **die Registerkarte** , und wählen Sie **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Der "**neue**" klicken **Der SAML Identitätsprovider**hinzuzufügen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 

    c. Geben Sie im Textfeld **Anzeigename**den Firmennamen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png) 

    d. Wählen Sie **ermöglichen RightScale initiiert SSO mit einem Hinweis Discovery** und geben Sie Ihren **Domänennamen** in der folgenden Textfeld.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    e. SAML SSO-URL Kopieren von Azure AD **SAML SSO-Endpunkt** in RightScale.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_005.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    f. Kopieren Sie Entitäts-ID von Azure AD in **SAML %EntityID** in RightScale.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_007.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    g. Klicken Sie **Browser** , um das Zertifikat hochladen, die Sie in Schritt 4 heruntergeladen haben.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    h. Klicken Sie auf **Speichern**.
    


6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.

    b. Geben Sie im Textfeld **Name** **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-rightscale-test-user"></a>Erstellen einen Testbenutzer RightScale

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in RightScale. Arbeiten Sie mit RightScale Support-Team über support@rightscale.com der Benutzer in der RightScale-Plattform hinzufügen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon RightScale Zugang gewährt Azure einmaliges Anmelden verwenden aktivieren.

![Benutzer zuweisen][200] 

**Britta Simon RightScale zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **RightScale**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die RightScale Fläche in der Sie sollte automatisch zur Anwendung RightScale angemeldete erhalten.
    

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_205.png
