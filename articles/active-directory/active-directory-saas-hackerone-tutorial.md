<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit HackerOne | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und HackerOne."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Lernprogramm: Azure Active Directory-Integration mit HackerOne

In diesem Lernprogramm wird Azure Active Directory (Azure AD) HackerOne integrieren.

Azure AD HackerOne Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf HackerOne Zugriff
- Sie können die Benutzer automatisch angemeldet-HackerOne (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit HackerOne benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Ein HackerOne Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Lernprogramm konfigurieren und Azure AD einmaliges Anmelden in einer Umgebung testen.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. HackerOne aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-hackerone-from-the-gallery"></a>HackerOne aus der Galerie hinzufügen
Um Azure AD HackerOne integrieren, müssen Sie der Liste der verwalteten SaaS-apps HackerOne aus der Galerie hinzufügen.

**Um HackerOne aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

![Applikationen][4]

6. Geben Sie im Suchfeld **HackerOne**.

![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. Wählen Sie im Ergebnisbereich **HackerOne aus**und dann auf **vollständig** die Anwendung hinzufügen.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Als Nächstes konfigurieren und Testen Azure AD einmaliges Anmelden mit HackerOne basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in HackerOne einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in HackerOne hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in HackerOne.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit HackerOne müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer HackerOne testen Benutzer](#creating-a-hackerone-test-user)** - Gegenstück Britta Simon Zertifizierung enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Anschließend können Sie Azure AD einmaliges Anmelden im klassischen Portal und einmaliges Anmelden in der HackerOne-Anwendung konfigurieren.

Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

**Konfigurieren Azure AD einmaliges Anmelden mit HackerOne die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **HackerOne** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei HackerOne** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. Gehen Sie auf der **App-Einstellungen konfigurieren** folgendermaßen vor, und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer anmelden, der HackerOne-Anwendung mit dem folgenden Muster: **"https://hackerone.com/\<Firmenname\>/authentication"**. 

    b. Wenden Sie die HackerOne über [support@hackerone.com](mailto:support@hackerone.com) den Tenant-URL abrufen, wenn Sie es nicht wissen.

    c. Geben Sie im Textfeld **ID** die Mieter URL. 

    d. Klicken Sie auf **Weiter**.


4. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden am HackerOne** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


1. Anmeldung bei Ihrem Mandanten HackerOne als Administrator.

1. Klicken Sie im Menü oben auf **Settings**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Navigieren Sie zu "**Authentifizierung**" und "**Hinzufügen SAML-Einstellungen**" auf.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. Gehen Sie im Dialogfeld **SAML-Einstellung** :

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    ein. Geben Sie in das Textfeld **E-Mail-Domäne** einer registrierten Domäne.

    b. Der Azure-Verwaltungsportal kopieren Sie **Einzelne Sign-On Service URL**und fügen Sie ihn in das Textfeld einzelne Anmelde-URL.

    c. Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

       >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)
    
    d. Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und fügen Sie es der **X509 Zertifikat** Textfeld.

    e. Klicken Sie auf **Speichern**


1. Klicken Sie im Dialogfeld Authentifizierung die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    ein. Klicken Sie auf **Test**.

    b. Der Wert des **Status** gleich Feld **letzten Teststatus: erstellt**, wenden Sie die HackerOne über [support@hackerone.com](mailto:support@hackerone.com) , eine Überprüfung der Konfiguration.


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD

Anschließend erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer sichere Bereitstellung in Azure AD erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   


### <a name="creating-a-hackerone-test-user"></a>Erstellen einen Testbenutzer HackerOne

Anschließend erstellen Sie einen Benutzer namens Britta Simon in HackerOne. HackerOne unterstützt Just-in-Time-Bereitstellung ist standardmäßig aktiviert.

Es ist keine Aufgabe für Sie in diesem Abschnitt. Beim Aufrufen von HackerOne wird ein neuer Benutzer noch nicht erstellt. [Konfiguration von Azure AD einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie die Zertifizierung Support kontaktieren.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Anschließend können Sie Britta Simon mit Azure einmaliges HackerOne Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon HackerOne zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **HackerOne**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Schließlich testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.  
Beim Klicken auf die HackerOne Fläche in der Sie sollte automatisch zur Anwendung HackerOne angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png