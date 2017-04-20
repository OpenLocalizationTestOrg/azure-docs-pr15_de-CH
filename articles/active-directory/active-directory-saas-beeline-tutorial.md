<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Luftlinie | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Luftlinie."
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


# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Lernprogramm: Azure Active Directory Integration Luftlinie

In diesem Lernprogramm lernen Sie Luftlinie in Azure Active Directory (Azure AD) integriert.

Integrieren von Azure AD Luftlinie bietet folgende Vorteile:

- Sie können in Azure AD Steuern auf Luftlinie hat
- Sie können die Benutzer automatisch angemeldet-Luftlinie (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Luftlinie benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Einen Abstecher Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Luftlinie aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-beeline-from-the-gallery"></a>Luftlinie aus der Galerie hinzufügen
Konfigurieren Sie die Integration der Luftlinie in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Luftlinie aus der Galerie hinzufügen.

**Um Luftlinie aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Luftlinie**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Luftlinie aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Luftlinie basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück in Luftlinie an einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Luftlinie hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Luftlinie.

Konfigurieren und Azure AD einmaliges Anmelden mit Luftlinie testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Luftlinie testen](#creating-an-beeline-test-user)** - Gegenstück Britta Simon Luftlinie enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der Luftlinie Anwendung konfigurieren.

Die Anwendung Luftlinie erwartet SAML-Assertionen in einem bestimmten Format. Arbeiten Sie mit Luftlinie zuerst die richtige Benutzer-ID bestimmt, die in der Anwendung zugeordnet werden. Außerdem nehmen Sie die Anleitung Luftlinie Team über das Attribut das sie für diese Zuordnung verwenden möchten. Microsoft empfiehlt, das Attribut **"NameIdentifier"** als Benutzer-ID verwenden. Sie können den Wert dieses Attributs auf der Registerkarte **"Polysaccharid"** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel dafür. Hier wurden die Nameidentifier Forderung mit dem Attribut **Userprincipalname** zugeordnet mit eindeutigen Benutzer-ID mit Anwendung Luftlinie in jeder erfolgreichen SAML-Antwort gesendet wird.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_07.png) 


**Um Azure AD einmaliges Anmelden mit Abstecher zu konfigurieren, führen Sie die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Luftlinie** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

     ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden Luftlinie** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_03.png) 

3. Auf der **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_04.png) 


    ein. Geben Sie im Textfeld **Bezeichner** den URL der Luftlinie Anwendung dem folgenden Muster Anmelden von Benutzern verwendet:`https://projects.beeline.net/<instance name>`

    b. Geben Sie den URL in Folgendes Muster Antwort-URL: `https://projects.beeline.net/<instance name>/SSO_External.ashx` oder`https://projects.beeline.net/<company name>/SSO_External.ashx`


4. Auf der Seite **Konfigurieren einmaliges Anmelden am Luftlinie** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_05.png) 

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5.  Zu SSO für die Anwendung konfigurierten unterstützen Luftlinie Support-Team und sie konfigurieren. Beachten Sie, dass Sie e-Mail und heruntergeladene Datei anfügen und Entitäts-ID und einzelne Zeichen, Service URL.
  
6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-beeline-test-user"></a>Erstellen einen Testbenutzer Luftlinie

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Luftlinie. Luftlinie Anwendung benötigen alle Benutzer in der Anwendung vor der Einmalanmeldung bereitgestellt werden. So geben Sie Arbeit Luftlinie Customer Support zuordnen alle Benutzer in der Anwendung bereitstellen. 


> [AZURE.NOTE] Möchten Sie einen Benutzer manuell erstellen oder batch-Benutzer, müssen Sie die Luftlinie Support kontaktieren.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Luftlinie Zugang gewähren.

![Benutzer zuweisen][200] 

**Um Luftlinie Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Luftlinie**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.
Beim Anklicken der Luftlinie Kachel in der Sie sollte automatisch zur Anwendung Luftlinie angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_205.png
