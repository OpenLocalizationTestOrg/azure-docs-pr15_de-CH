<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Verwaltungsportal für Microsoft Azure Cloud | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Verwaltungsportal für Microsoft Azure Cloud."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Lernprogramm: Azure Active Directory-Integration mit Cloud-Verwaltungsportal für Microsoft Azure

Dieses Lernprogramm soll veranschaulichen Verwaltungsportal für Microsoft Azure Cloud Azure Active Directory (Azure AD) integrieren.  
Integrieren von Azure AD Verwaltungsportal für Microsoft Azure Cloud bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, die für Microsoft Azure Cloud Management-Portal zugreifen
- Sie können die Benutzer automatisch zum Cloud-Verwaltungsportal für Microsoft Azure (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten


Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Cloud-Verwaltungsportal für Microsoft Azure benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cloud-Verwaltungsportal für Microsoft Azure Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Verwaltungsportal für Microsoft Azure Cloud aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Verwaltungsportal für Microsoft Azure Cloud aus der Galerie hinzufügen
Zum Konfigurieren der Integration von Cloud-Verwaltungsportal für Microsoft Azure in Azure AD müssen Sie die Liste der verwalteten SaaS-apps Verwaltungsportal für Microsoft Azure Cloud aus dem Katalog hinzufügen.

**Zum Verwaltungsportal für Microsoft Azure Cloud aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Verwaltungsportal für Microsoft Azure Cloud**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Verwaltungsportal für Microsoft Azure Cloud aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit Cloud-Verwaltungsportal für Microsoft Azure basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück im Verwaltungsportal für Microsoft Azure Cloud Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer im Verwaltungsportal für Microsoft Azure Cloud hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** im Verwaltungsportal für Microsoft Azure Cloud.

Konfigurieren und Azure AD einmaliges Anmelden mit Cloud-Verwaltungsportal für Microsoft Azure testen, müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen eine Cloud-Verwaltungsportal für Microsoft Azure testen Benutzer](#creating-a-newsignature-test-user)** - Gegenstück Britta Simon im Verwaltungsportal für Microsoft Azure Cloud haben, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in der Cloud-Verwaltungsportal für Microsoft Azure-Anwendung.



**Konfiguration mit Cloud-Verwaltungsportal für Microsoft Azure Azure AD einmaliges Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Verwaltungsportal für Microsoft Azure Cloud** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer Verwaltungsportal für Microsoft Azure Cloud anmelden** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_03.png) 

3. Auf der **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_04.png) 

    ein. **Anmelde-URL** -Textfeld Geben Sie den URL die Benutzer anmelden, um Ihre Cloud-Verwaltungsportal für Microsoft Azure-Anwendung das folgende Muster verwendet:`https://portal.igcm.com/<instance name>`

    b. Klicken Sie auf **Weiter**.


4. Auf der Seite **Konfigurieren einmaliges Anmelden am Verwaltungsportal für Microsoft Azure Cloud** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Erhalten Sie Ihre Cloud-Verwaltungsportal zu SSO für die Anwendung konfigurierten Microsoft Azure Support-Team [jczernuszka@newsignature.com](mailTo:jczernuszka@newsignature.com) und heruntergeladene Zertifikatsdatei anfügen. Außerdem Bitte geben Sie Aussteller URL, SAML SSO-URL und einzelnen Zeichen, Dienst-URL, damit sie für SSO-Integration konfiguriert werden können.


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Ein Cloud-Verwaltungsportal für Microsoft Azure Testbenutzer erstellen

Dieser Abschnitt soll Benutzer Britta Simon im Cloud-Verwaltungsportal für Microsoft Azure erstellen. Arbeiten Sie mit Cloud-Verwaltungsportal für Microsoft Azure-Support-Team, die Benutzer in die Cloud-Verwaltungsportal für Microsoft Azure-Konto hinzufügen. 


> [AZURE.NOTE]Möchten Sie einen Benutzer manuell erstellen, müssen Sie Cloud-Verwaltungsportal Microsoft Azure-Support-Team erhalten.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure SSO-Verwaltungsportal für Microsoft Azure Cloud Zugang gewähren.

![Benutzer zuweisen][200] 

**Um Verwaltungsportal für Microsoft Azure Cloud Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Verwaltungsportal für Microsoft Azure Cloud**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die Cloud-Verwaltungsportal für Microsoft Azure Kachel in der Sie sollten automatisch zu Ihrem Cloud-Verwaltungsportal für Microsoft Azure-Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_205.png
