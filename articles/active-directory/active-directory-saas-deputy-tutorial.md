<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration stellvertretende | Microsoft Azure"
    description="Dazu konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Stellvertreter."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Lernprogramm: Azure Active Directory Integration Stellvertreter

Dieses Lernprogramm soll wie Sie Stellvertreter in Azure Active Directory (Azure AD) integriert.

Integrieren von Azure AD stellvertretende bietet folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf Stellvertreter
- Sie können die Benutzer automatisch angemeldet-Stellvertreter (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit stellvertretende benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein stellvertretender Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Stellvertreter aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-deputy-from-the-gallery"></a>Stellvertreter aus der Galerie hinzufügen
Konfigurieren Sie die Integration der stellvertretende in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Stellvertreter aus der Galerie hinzufügen.

**Um Stellvertreter aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Stellvertreter**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **Stellvertreter aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Stellvertreter basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück in stellvertretender Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer stellvertretende hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in stellvertretende.

Konfigurieren und Testen Azure AD einmaliges Stellvertreter müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellt ein stellvertretender Benutzer testen](#creating-a-deputy-test-user)** - Gegenstück Britta Simon stellvertretende enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der stellvertretende Anwendung konfigurieren.

**Konfigurieren Azure AD einmaliges Stellvertreter Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **stellvertretende** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden stellvertretende** **Azure AD einmaliges**wählen Sie aus und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Möchten Sie auf der **App-Einstellungen konfigurieren** die Anwendung im **IDP initiiert Modus**konfigurieren, führen Sie die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    ein. Geben Sie im Textfeld **ID** eine URL dem folgenden Muster: `https://<your-subdomain>.<region>.deputy.com`.

    b. Geben Sie im Feld **Antwort-URL** eine URL dem folgenden Muster: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c. Klicken Sie auf **Weiter**.

4. Möchten Sie die Anwendung auf der **App-Einstellungen konfigurieren** im **SP initiiert Modus** konfigurieren, klicken Sie auf **"Show advanced Settings (optional)"** und geben Sie die **Anmelde-URL** , und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster: `https://<your-subdomain>.<region>.deputy.com`.

    b. Klicken Sie auf **Weiter**.

    > [AZURE.NOTE] Stellvertretender Region Suffix ist opitional oder Verwenden eines dieser: au | Na | EU | als | la | af | ein | ent au | ent-Na | ent Eu | ent-als | ent-la | ent-af | ent-ein

5. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden am stellvertretende** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    
6. Navigieren Sie zur folgenden URL: Https://(your-subdomain).deputy.com/exec/config-System_config. **Sicherheit** und klicken Sie auf **Bearbeiten**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. Kopieren Sie im klassischen Azure-Portal, auf das Konfigurieren einmaliges stellvertretende Seite SAML SSO-URL 

8. Führen Sie auf dieser Seite **Sicherheit** folgende Schritte aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    ein. Aktivieren Sie **soziale Anmeldung**.

    b. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **OpenSSL-Zertifikat**

    c. Geben Sie im Textfeld SAM SSO-URL`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Ersetzen Sie im Textfeld SAM SSO-URL `<your subdomain>` mit der Domäne.

    e. Ersetzen Sie im Textfeld SAM SSO-URL `<saml sso url>` mit SAML SSO-URL ist der Azure-Verwaltungsportal aus.

    f. Klicken Sie auf **Speichern**.

9. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-deputy-test-user"></a>Stellvertretender Testbenutzer erstellen

Um Azure AD Benutzer stellvertretende anmelden können, müssen sie in stellvertretende bereitgestellt werden. Bei Stellvertreter ist die Bereitstellung einer manuellen Aufgabe.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Um ein Benutzerkonto bereitstellen, führen Sie die folgenden Schritte:

1.  Der stellvertretende Unternehmenswebsite als Administrator anmelden.

2.  Klicken Sie im obersten Navigationsbereich auf **Personen**.

    ![Personen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Personen")

3.  Klicken Sie auf **Personen hinzufügen** , und klicken Sie auf **eine Person hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Benutzer hinzufügen")

4.  Gehen Sie folgendermaßen vor, und klicken Sie auf **Speichern und Laden**.

    ![Neuer Benutzer] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Neuer Benutzer")

    ein. Geben Sie in das Textfeld **Name** **Britta** und **Simon**.  

    b. Geben Sie im Feld **E-Mail** die e-Mail-Adresse von Azure Konto, das Sie bereitstellen möchten.

    c. Geben Sie im Textfeld **Arbeit** den Namen Bussniess.

    d. Klicken Sie auf **Speichern und Laden** .

    >[AZURE.NOTE]Kontoinhaber AAD e-Mail und einen Link, um ihr Konto bestätigen aktiviert wird. Können Sie alle anderen stellvertretenden Benutzer Konto Erstellungstools oder APIs von stellvertretenden Bereitstellung AAD Benutzerkonten.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges stellvertretende Zugang gewähren.
    
![Benutzer zuweisen][200]

**Britta Simon Stellvertreter zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Stellvertreter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Anklicken der stellvertretende Kachel in der Sie sollte automatisch zur Anwendung stellvertretende angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
