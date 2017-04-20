<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit interaktiven Yonyx | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und interaktive Leitfäden Yonyx."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Lernprogramm: Azure Active Directory-Integration mit interaktiven Yonyx

Das Ziel dieses Lernprogramms ist, wie Yonyx interaktive Leitfäden in Azure Active Directory (Azure AD) integriert werden.

Integrieren von Azure AD Yonyx interaktive Leitfäden bietet folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf Yonyx interaktive Leitfäden
- Sie können die Benutzer automatisch auf Yonyx interaktive Leitfäden (einmaliges Anmelden) angemeldete erhalten mit ihren Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit interaktiven Yonyx benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine interaktive Leitfäden Yonyx Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Yonyx Interactive Hilfslinien aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Yonyx Interactive Hilfslinien aus der Galerie hinzufügen
Zum Konfigurieren der Integration der Yonyx interaktiven Ratgeber in Azure AD müssen Sie der Liste der verwalteten SaaS-apps interaktive Leitfäden Yonyx aus der Galerie hinzufügen.

**Um interaktive Leitfäden Yonyx aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Yonyx interaktive Leitfäden**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **Interaktive Leitfäden Yonyx aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden mit Yonyx Interactive basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück in interaktive Leitfäden für Benutzer in Azure AD Yonyx ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer in Yonyx interaktive Leitfäden hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in interaktive Leitfäden Yonyx.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit interaktiven Yonyx müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Benutzer erstellen eine interaktive Leitfäden Yonyx test](#creating-a-yonyx-interactive-guides-test-user)** - Gegenstück Britta Simon Yonyx interaktive Leitfäden enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in Ihrer Anwendung Yonyx interaktive Leitfäden konfigurieren.

**Um mit interaktiven Yonyx Azure AD einmaliges Anmelden konfigurieren, führen Sie die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Yonyx interaktive Leitfäden** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden Yonyx interaktive Leitfäden** **Azure AD einmaliges**wählen Sie aus und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_03.png)

3. Führen Sie auf der **App-Einstellungen konfigurieren** die folgenden Schritte aus, und **Klicken Sie auf**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_04.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`.

    b. Geben Sie im Textfeld **ID** eine URL dem folgenden Muster: `https://<company name>.yonyx.com`.

    c. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Beachten Sie, dass diese Werte mit der tatsächlichen Anmelde-URL-ID aktualisieren. Um diese Werte zu erhalten, wenden Sie sich an Yonyx interaktive Leitfäden Supportteam über <mailto:support@yonyx.com>.

4. Auf der Seite **Konfigurieren einmaliges Anmelden am Yonyx interaktive Leitfäden** auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_05.png)

5. Zu SSO für Ihre Anwendung, Kontakt, interaktive Yonyx-Leitfäden Team Unterstützung konfiguriert <mailto:support@yonyx.com> und Ihnen Folgendes:

    • Das heruntergeladene **Zertifikat**

    • Der **Aussteller URL**

    • Die **URL des Diensts für einmaliges Anmelden**

    • **Einzelne Abmeldung Dienst-URL**

6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Geben Sie im Textfeld **Nachname** **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-yonyx-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-yonyx-interactive-guides-test-user"></a>Erstellen einen Testbenutzer Yonyx interaktive Leitfäden

Dieser Abschnitt soll Benutzer Britta Simon in interaktive Leitfäden Yonyx erstellen. Yonyx interaktive Leitfäden unterstützt Just-in-Time-Bereitstellung, die standardmäßig aktiviert.

Es ist keine Aufgabe für Sie in diesem Abschnitt. Ein neuer Benutzer wird Adobe Creative Cloud Zugriff auf noch nicht erstellt.

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen die Yonyx interaktive Leitfäden Kundenservice über <mailto:support@yonyx.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Yonyx interaktive Leitfäden keinen Zugriff erteilen.
    
![Benutzer zuweisen][200]

**Britta Simon Yonyx interaktive Leitfäden zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Yonyx interaktive Leitfäden**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_50.png)

3. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Klicken auf die interaktive Leitfäden Yonyx Kachel in der Sie sollte automatisch zur Anwendung Yonyx interaktive Leitfäden angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_205.png
