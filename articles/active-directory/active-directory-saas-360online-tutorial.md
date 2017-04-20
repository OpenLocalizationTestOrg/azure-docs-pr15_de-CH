<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Online 360 Grad | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und 360° Online."
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


# <a name="tutorial-azure-active-directory-integration-with-360-online"></a>Lernprogramm: Azure Active Directory-Integration mit 360° Online


Dieses Lernprogramm soll zeigen, wie Sie Online 360° in Azure Active Directory (Azure AD) integriert.

Integration von 360 Grad stellt Online mit Azure folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf 360 ° Online Server
- Sie können die Benutzer automatisch bis 360° Online (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit 360° Online benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- 360° Online Mieter


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. 360° Online aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden



## <a name="adding-360-online-from-the-gallery"></a>360° Online aus der Galerie hinzufügen
Zum Konfigurieren der Integration von 360 ° Online in Azure AD müssen Sie der Liste der verwalteten SaaS-apps 360° Online aus dem Katalog hinzufügen.


**Um 360° Online aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **360° Online**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_01.png)

7. Wählen Sie im Ergebnisbereich **360° Online aus**und dann auf **vollständig** die Anwendung hinzufügen.
 
    ![Applikationen](./media/active-directory-saas-360online-tutorial/tutorial_360online_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit 360° Testbenutzer namens "Britta Simon" Online abhängig.

Azure AD muss für einmaliges Anmelden zu den Gegenstück 360° Benutzer wissen, Online, um einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzerobjekt und dem entsprechenden Benutzer 360° Online hergestellt werden.


Konfigurieren und Azure AD einmaliges Online 360 ° testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer 360 ° Online Benutzer testen](#creating-a-360-online-test-user)** : zu Azure AD Darstellung Ihres verknüpft ist eine Entsprechung von Britta Simon 360° Online.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und einmaliges Anmelden in der 360 ° Online-Anwendung konfigurieren.

**Konfigurieren Azure AD einmaliges Online 360 ° Schritte:**


1. In der Azure Verwaltungsportal auf Anwendungsseite Integration **360° Online** klicken Sie **Konfigurieren einmaliges Anmelden** **Konfigurieren Single Sign-On** -Dialogfeld geöffnet.

    ![Einmaliges Anmelden konfigurieren][13] 

2. Auf der Seite **Wie möchten Sie Benutzer sich auf 360 ° Online** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-360online-tutorial/tutorial_360online_03.png) 

3. Gehen Sie auf der **App-URL konfigurieren** folgendermaßen vor, und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-360online-tutorial/tutorial_360online_04.png)

    ein. **Anmelde-URL** -Textfeld Geben Sie den URL die Benutzer anmelden und die 360 Grad Online-Anwendung das folgende Muster verwendet:`https://<company name>.public360online.com`

    b. Klicken Sie auf **Weiter**

4. Gehen Sie auf der **App-URL konfigurieren** folgendermaßen vor, und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-360online-tutorial/tutorial_360online_05.png) 

    ein. Auf **Metadaten**und auf dem Computer speichern.

    b. Klicken Sie auf **Weiter**.


5. Zu SSO für die Anwendung konfigurierten wenden die 360° Online über [360online@software-innovation.com](mailto:360online@software-innovation.com) und die heruntergeladenen Metadatendatei auf e-Mail.

6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 


4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.

    b. Geben Sie im Textfeld **Benutzername** **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_07.png) 


8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   




### <a name="creating-a-360-online-test-user"></a>Erstellen einer 360° Online-Prüfung

Dieser Abschnitt soll Benutzer Britta Simon 360° Online erstellen. 

Um einen Benutzer 360° Online erstellt, müssen Sie Ihre 360° Online Kundenservice über [360online@software-innovation.com](mailto:360online@software-innovation.com).


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges Anmelden ihren Zugriff auf 360° Online aktivieren.

![Benutzer zuweisen][200] 

**Um 360° Online Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
 
    ![Benutzer zuweisen][201] 


2. Wählen Sie in der Anwendungsliste **360° Online**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-360online-tutorial/tutorial_360online_50.png) 


1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Wenn Sie nebeneinander in der 360 ° Online klicken, Sie sollten automatisch Ihre 360 ° Online-Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)







<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[10]: ./media/active-directory-saas-360online-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-360online-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-360online-tutorial/tutorial_general_08.png
[13]: ./media/active-directory-saas-360online-tutorial/tutorial_general_09.png
[20]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-360online-tutorial/tutorial_general_205.png
