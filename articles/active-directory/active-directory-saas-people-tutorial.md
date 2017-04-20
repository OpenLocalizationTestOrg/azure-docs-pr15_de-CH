<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit | Microsoft Azure"
    description="Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und konfigurieren."
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


# <a name="tutorial-azure-active-directory-integration-with-people"></a>Lernprogramm: Azure Active Directory-Integration mit

Das Ziel dieses Lernprogramms ist, wie Menschen in Azure Active Directory (Azure AD) integriert werden.

Integration von Personen mit Azure bietet die folgenden Vorteile:

- Sie können in Azure AD steuern Benutzer hat
- Sie können die Benutzer automatisch zu (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Personen benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Ein Benutzer Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Hinzufügen von Personen aus der Galerie
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-people-from-the-gallery"></a>Hinzufügen von Personen aus der Galerie
Konfigurieren Sie die Integration von Personen in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Personen aus der Galerie hinzufügen.

**Um Personen aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 
 
    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Menschen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Personen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden mit Menschen Testbenutzer "Britta Simon" bezeichnet.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen von Personen Benutzer testen](#creating-a-people-test-user)** - Gegenstück Britta Simon Personen enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Personen Anwendung.



**Konfigurieren Azure AD einmaliges Anmelden für Personen, die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Personen** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    [Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden zu** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_03.png) 

3. Gehen Sie auf der **App-Einstellungen konfigurieren** folgendermaßen vor, und klicken Sie auf **Weiter**:
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer anmelden, der Personen Anwendung mit dem folgenden Muster: **"https://\<Firmenname\>.peoplehr.com/"**. 

    b. Wenn Sie Ihre Tenant-URL nicht kennen, wenden Sie die Personen über [customerservices@peoplehr.com](mailto:customerservices@peoplehr.com) zu.  

    c. Geben Sie im Textfeld **ID** die Mieter URL. 

    d. Geben Sie im Feld **Antwort-URL** den URL in Folgendes Muster: "**https://itgs.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx**".

    e. Klicken Sie auf **Weiter**


4. Gehen Sie auf der Seite **Konfigurieren einmaliges Menschen** , und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_05.png) 

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Um SSO für die Anwendung konfiguriert, müssen Sie Ihrem Mandanten Personen als Administrator anmelden.
    
    ein. **Klicken Sie im Menü auf der linken Seite klicken.**
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_001.png) 

    b. Klicken Sie auf **"Unternehmen"**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_002.png) 

    c. Auf die **"Hochladen" einmaliges auf ' SAML Metadaten Datei "**, klicken Sie auf **Durchsuchen** , um heruntergeladene Metadaten-Datei hochladen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)




6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.
 
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.
Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]


**Um einen Testbenutzer Personen in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-people-test-user"></a>Erstellen einen Testbenutzer Personen

Dieser Abschnitt soll Benutzer Britta Simon in Mitarbeiter erstellen. Just-in-Time-Bereitstellung Supportteam Personen um einen Benutzer manuell erstellen kontaktieren müssen Benutzer nicht unterstützt.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges Anmelden ihren Zugriff zu aktivieren.

![Benutzer zuweisen][200] 

**Um Personen Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Personen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
Beim Anklicken der Kachel Personen in der Sie sollte automatisch für die Personen Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-people-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-people-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-people-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-people-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-people-tutorial/tutorial_general_205.png
