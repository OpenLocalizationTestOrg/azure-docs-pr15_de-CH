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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Lernprogramm: Azure Active Directory-Integration mit

Dieses Lernprogramm soll wie Sie vorne in Azure Active Directory (Azure AD) integriert.

Integration von vorne mit Azure bietet folgende Vorteile:

- Sie können in Azure AD steuern hat Zugang zum
- Sie können die Benutzer automatisch nach vorne (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Vorderseite einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Vorderseite aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-front-from-the-gallery"></a>Vorderseite aus der Galerie hinzufügen
Konfigurieren Sie die Integration der Vorderseite in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Vorderseite aus dem Katalog hinzufügen.

**Um Vorderseite aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Front**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **Front aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden mit basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was ein Benutzer in Azure AD Gegenstück Benutzer vor. Also muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzerobjekt und vor dem entsprechenden Benutzer eingerichtet werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** vor.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Benutzer erstellen eine test](#creating-a-front-test-user)** - Gegenstück Britta Simon vor aufweisen, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der Vordergrund-Anwendung konfigurieren.

**Konfigurieren Azure AD einmaliges Anmelden mit die folgenden Schritte:**

1. Klicken Sie im klassischen Portal auf der **ersten** Seite der Anwendung-Integration auf **Konfigurieren einmaliges** öffnen das Dialogfeld **Konfigurieren Sie einmaliges Anmelden** .
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer vor Anmeldung** **Azure AD einmaliges**wählen Sie aus und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Möchten Sie auf der **App-Einstellungen konfigurieren** die Anwendung im **IDP initiiert Modus**konfigurieren, führen Sie die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    ein. Geben Sie im Textfeld **ID** eine URL dem folgenden Muster:`https://<company name>.frontapp.com`

    b. Geben Sie im Feld **Antwort-URL** eine URL dem folgenden Muster:`https://<company name>.frontapp.com/sso/saml/callback`

    c. Klicken Sie auf **Weiter**

4. Möchten Sie die Anwendung auf der **App-Einstellungen konfigurieren** im **SP initiiert Modus** konfigurieren, klicken Sie auf **"Show advanced Settings (optional)"** und geben Sie die **Anmelde-URL** , und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster:`https://<company name>.frontapp.com`

    b. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Beachten Sie, dass diese nicht die tatsächlichen Werte. Sie müssen diese Werte mit der tatsächlichen Anmelde-URL Bezeichner und Antwort-URL aktualisieren. Um diese Werte abzurufen, können Details finden Sie in **Schritt 12** oder Vorderseite über [support@frontapp.com](emailTo:support@frontapp.com).

5. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden vor** die folgenden Schritte aus, und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

6. Anmelden Sie an der Vorderseite Mieter als Administrator.

7. Gehen Sie zu **Settings (Zahnrad-Symbol am unteren Rand der linken Randleiste) > Voreinstellungen**.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Klicken Sie auf **Einmalanmeldung** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. Wählen Sie in der Dropdown-Liste der **Einmalanmeldung** **SAML** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. **Einstiegspunkt** setzen Textbox **Einzelne Sign-On Service URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. Kopieren Sie den Inhalt der heruntergeladenen Datei, und fügen Sie ihn in das Textfeld **Signaturzertifikat** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Bestätigen Sie, dass diese URls der in Schritt 3 übereinstimmen.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

13. Klicken Sie auf **Speichern** .

14. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

15. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-front-test-user"></a>Vorderseite Testbenutzer erstellen

Dieser Abschnitt soll Benutzer Britta Simon Front.Please Arbeit mit Ihrem Supportteam Vorderseite fügen die Benutzer im Vordergrund-Konto erstellen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Vorderseite Zugang gewähren.
    
![Benutzer zuweisen][200]

**Britta Simon Vordergrund zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Front**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Klicken auf die Vorderseite Kachel in der Sie sollte automatisch an der Vorderseite Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
