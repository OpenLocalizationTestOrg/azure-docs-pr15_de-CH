<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Tableaus Online | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Tableaus Online."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Lernprogramm: Azure Active Directory Integration Tableaus Online

In diesem Lernprogramm lernen Sie Online Tableaus in Azure Active Directory (Azure AD) integriert.

Integration in Azure AD Tableaus Online bietet die folgenden Vorteile:

- Sie können in Azure AD steuern Zugriff auf Tableaus Online
- Sie können die Benutzer automatisch online Tableaus (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Tableaus Online benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein **Tableaus Online** Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Tableaus Online aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-tableau-online-from-the-gallery"></a>Tableaus Online aus der Galerie hinzufügen
Zum Konfigurieren der Integration des Tableaus Online in Azure AD müssen Sie aus der Galerie zur Liste der verwalteten SaaS-apps Tableaus Online hinzufügen.

**Um Online Tableaus aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Tableaus Online**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Tableaus Online aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Online Tableaus basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück in Tableaus an einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer Tableaus Online hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Tableaus Online.

Konfigurieren und Azure AD einmaliges Tableaus Online testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen Tableaus Online test](#creating-a-Tableau-Online-test-user)** - Gegenstück Britta Simon Tableaus Online enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in Tableaus Online-Bewerbung.

**Konfigurieren Azure AD einmaliges Online Tableaus Schritte:**

1. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren][6]
2. Klicken Sie im Verwaltungsportal auf **Tableaus Online** Integration Anwendung auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][7] 

3. Auf der Seite **Wie möchten Sie Benutzer Tableaus Online anmelden** **Azure AD einmaliges Anmelden**wählen Sie aus und dann auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    ein. Geben Sie im Textfeld Anmelde-URL eine URL dem folgenden Muster:`https://sso.online.tableau.com`

    c. Klicken Sie auf **Weiter**.

5. Klicken Sie auf der Seite **Konfigurieren einmaliges Tableaus Online** auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]
8. In einem anderen Browserfenster Anmeldung Tableaus Online-Bewerbung. Zur **Einstellung** und **Authentifizierung**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. Im Abschnitt **Authentifizierung** . Das Kontrollkästchen Sie **einmaliges Anmelden mit SAML** SAML aktivieren.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Scrollen Sie nach unten bis zum Abschnitt **Metadatendatei in Tableaus Online importieren** .  Klicken Sie auf Durchsuchen und importieren Sie der Metadaten von Azure AD herunterladen. Klicken Sie dann auf **Übernehmen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. Fügen Sie im Abschnitt **Übereinstimmung Assertionen** den entsprechenden Identitätsanbieter Assertion ein für e-Mail, Vorname und Nachname. Zum Abrufen dieser Informationen von Azure AD:

    ein. Zurück zu Azure AD. **Klicken Sie auf im klassischen Azure-Portal, auf der Seite den **Tableaus Online** Anwendung-Integration im Menü oben.** Werte kopieren: Userprincipalname, Vorname und Nachname.
     
    ![Azure AD einmaliges Anmelden](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Tableaus Online Anwendung wechseln und den Abschnitt **Tableaus Online Attribute** wie folgt festgelegt:
    
    -  E-Mail: **Mail** oder **userprincipalname**
    -  Vorname: **Givenname**
    -  Name: **Name**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-tableau-online-test-user"></a>Tableaus Online Testbenutzer erstellen

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon Tableaus Online.

1. **Tableaus Online**klicken Sie auf **Einstellungen** und Abschnitt **Authentifizierung** . Benutzer **Wählen** einen Bildlauf. Klicken Sie auf **Benutzer hinzufügen** und dann auf **e-Mail-Adressen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Wählen Sie **Benutzer für einmaliges Anmelden (SSO) Authentifizierung hinzufügen**. Fügen Sie in das Textfeld **E-Mail-Adressen eingeben**britta.simon@contoso.com

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Klicken Sie auf **Erstellen**.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Tableaus Online Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon Tableaus Online zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

3. Wählen Sie in der Anwendungsliste **Tableaus Online**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

5. Wählen Sie in der Liste alle Benutzer **Britta Simon**.

6. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf den Tableaus Online Kachel in der Sie sollte automatisch anwendungsspezifische Tableaus Online angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
