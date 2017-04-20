<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Novatus | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und sichere Bereitstellung."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Lernprogramm: Azure Active Directory Integration BIETEN sichere

Das Ziel dieses Lernprogramms ist, wie sichere Bereitstellung in Azure Active Directory (Azure AD) integriert werden.  
Azure AD Integration BIETEN sichere bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer zu schützen Zugriff
- Sie können die Benutzer automatisch sichern (Single Sign-On) zu angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit SECURE BIETEN benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Eine sichere Bereitstellung Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Sichere Bereitstellung aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-secure-deliver-from-the-gallery"></a>Sichere Bereitstellung aus der Galerie hinzufügen
Konfigurieren die Integration BIETEN sichere Azure AD müssen Sie der Liste der verwalteten SaaS-apps BIETEN sichere aus der Galerie hinzufügen.

**Um sichere Bereitstellung aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **SECURE liefern**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. Im Ergebnisbereich **Sichere Bereitstellung**wählen Sie aus und dann auf **vollständig** die Anwendung hinzufügen.
    
    ![App Logo und Name in der Galerie](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll konfigurieren und Testen Azure AD einmaliges Anmelden sichern liefern basierend auf einen Testbenutzer namens "Britta Simon" angezeigt.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück SECURE liefern einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer sichere liefern hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** SECURE liefern.

Konfiguriert und getestet Azure AD einmaliges Anmelden mit SECURE liefern, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Benutzer erstellen eine sichere Bereitstellung testen](#creating-a-secure-deliver-test-user)** - Gegenstück Britta Simon Personen enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung SECURE liefern.



**Konfigurieren Azure AD einmaliges Anmelden mit SECURE BIETEN die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der **Sichere Bereitstellung** Application Integrationsseite auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie möchten Sie Benutzer anmelden zu sichern** **Azure AD Single Sign-On**und klicken Sie auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png) 

3. Gehen Sie auf der **App-Einstellungen konfigurieren** folgendermaßen vor, und klicken Sie auf **Weiter**:
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer anmelden, der BIETEN sichere Anwendung das folgende Muster: **"https://i-securedeliver.jp/sd/\<Firmenname\>/jsf/login/sso"**.

    b. Wenden Sie die sichere Bereitstellung über [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) der Tenant-URL zu, wenn Sie den Wert nicht kennen.

    c. Geben Sie im Textfeld **ID** die Mieter URL. 

    d. Klicken Sie auf **Weiter**


4. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden unter Sichere Bereitstellung** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Zu SSO für die Anwendung konfigurierten wenden die sichere Bereitstellung über [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) und Ihnen Folgendes:
    
    • Der heruntergeladenen Datei

    • Die **Entitäts-ID**

    • Die **URL des Diensts für einmaliges Anmelden**

    • **Einzelne Abmeldung Dienst-URL**



6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer sichere Bereitstellung in Azure AD erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
  
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-secure-deliver-test-user"></a>Sichere Bereitstellung Testbenutzer erstellen

Dieser Abschnitt soll Benutzer Britta Simon in sichere Bereitstellung erstellen. Arbeiten Sie mit SECURE BIETEN Support-Team die Benutzer liefern schützen Konto hinzuzufügen.

> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie die sichere Bereitstellung Support kontaktieren.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Anmelden ihren Zugriff zu schützen.

![Benutzer zuweisen][200] 

**Gehen Sie zum Zuweisen von Britta Simon zu sichern:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste Programme **Sichern liefern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die sichere Bereitstellung Kachel in der Sie sollte automatisch zur Anwendung BIETEN sichere angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png
