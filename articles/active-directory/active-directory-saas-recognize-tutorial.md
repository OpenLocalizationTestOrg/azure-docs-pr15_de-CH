<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration erkennen | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und erkennen."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Lernprogramm: Azure Active Directory Integration erkennen

Dieses Lernprogramm soll zeigen, wie Sie erkennen in Azure Active Directory (Azure AD) integriert.

Integrieren von Azure AD erkennen bietet folgende Vorteile:

- Sie können in Azure AD steuern, Zugang zu erkennen
- Sie können die Benutzer automatisch zu erkennen (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit erkennen benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Erkennen einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Erkennen von der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-recognize-from-the-gallery"></a>Erkennen von der Galerie hinzufügen
Konfigurieren die Integration erkennen Azure AD müssen Sie der Liste der verwalteten SaaS-apps erkennen aus dem Katalog hinzufügen.

**Gehen Sie zum Erkennen von der Galerie hinzufügen:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **erkennen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **erkennen**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges basierend auf einen Testbenutzer namens "Britta Simon" erkennen.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück erkennen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer erkennen hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in erkennen.

Zum Konfigurieren und Testen Azure AD einmaliges Anmelden mit erkennen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer erkennen Benutzer testen](#creating-a-recognize-test-user)** - Gegenstück Britta Simon erkennen enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren auf in der Anwendung erkennen.

**Konfigurieren Azure AD einmaliges Anmelden mit erkennen die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **erkennen** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer erkennen anmelden** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. Führen Sie auf der **App-Einstellungen konfigurieren** die folgenden Schritte aus, und **Klicken Sie auf**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Geben Sie im Textfeld **ID** eine URL dem folgenden Muster: `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Wenn Sie diese URLs nicht kennen, geben Sie Beispiel-URLs mit Beispiel. Um diese Werte abzurufen, können weitere Details finden Sie in Schritt 9 oder erkennen Supportteam über <mailto:support@recognizeapp.com>.

4. Auf der Seite **Konfigurieren einmaliges Anmelden am erkennen** auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. Anmelden Sie in anderen Webbrowserfenster Ihrem Mandanten erkennen als Administrator.

6. Klicken Sie in der oberen rechten Ecke auf **Menü**. Werden Sie **Unternehmensadministrator**.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. **Im linken Navigationsbereich auf klicken.**

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. Führen Sie die folgenden Schritte auf **SSO-** Einstellungen.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    ein. **Aktivieren von SSO**wählen Sie **auf aus**.

    b. **IDP Entitäts-ID** setzen Textbox **Aussteller URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    c. Die **SSO-Ziel-Url** setzen Textbox **Einzelne Sign-On Service URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    d. In der **Ziel-Url Slo** setzen Textbox **Einzelne Abmelde Service URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    e. Die heruntergeladene Datei im Editor öffnen, den Inhalt in die Zwischenablage kopieren und in das **Zertifikat** Textbox einfügen 

    f. Klicken Sie auf die Schaltfläche **Speichern** . 

9. Kopieren Sie die URL unter **Service Metadaten Url**neben **SSO** -Einstellungen.
    
    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Öffnen Sie die **Metadaten-URL-Verknüpfung** ein leeres Browserfenster das Metadatendokument herunterladen. Verwenden Sie anschließend den EntityDescriptor Wert eingegebenen erkennen **Bezeichner** in der **App-Einstellungen konfigurieren** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

12. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Geben Sie im Textfeld **Nachname** **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-recognize-test-user"></a>Erstellen einen Testbenutzer erkennen

Um Azure AD Benutzer erkennen anmelden können, müssen sie in erkennen bereitgestellt werden. Bei erkennen ist die Bereitstellung einer manuellen Aufgabe.

Diese app SCIM Bereitstellung unterstützt aber hat einen alternativen synchronisieren, die Benutzer Vorschriften. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Um ein Benutzerkonto bereitstellen, führen Sie die folgenden Schritte:

1.  Erkennen der Website Ihres Unternehmens als Administrator anmelden.

2.  Klicken Sie in der oberen rechten Ecke auf **Menü**. Werden Sie **Unternehmensadministrator**.

3.  **Im linken Navigationsbereich auf klicken.**

4.  Die folgenden Schritte auf **Benutzer synchronisieren** .
    
    ![Neuer Benutzer] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Neuer Benutzer")

    ein. **Synchronisierung aktiviert** **auf**auswählen.

    b. **Wählen Sie Synchronisierungsanbieter**wählen **Microsoft / Office 365**.

    c. Klicken Sie auf **Benutzer synchronisieren**

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges erkennen keinen Zugriff erteilen.
    
![Benutzer zuweisen][200]

**Gehen Sie zum Zuweisen von Britta Simon zu erkennen:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **erkennen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Anklicken der Kachel erkennen, in der Sie sollte automatisch zur Anwendung erkennen angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
