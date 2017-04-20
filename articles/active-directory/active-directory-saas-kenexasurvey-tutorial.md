<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration mit IBM Kenexa Umfrage | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und IBM Kenexa Umfrage."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Lernprogramm: Azure Active Directory Integration mit IBM Kenexa-Umfrage

In diesem Lernprogramm lernen Sie IBM Kenexa Umfrage Enterprise in Azure Active Directory (Azure AD) integriert.

Integration von IBM Kenexa Umfrage Enterprise in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf IBM Kenexa Umfrage Enterprise
- Sie können die Benutzer automatisch auf IBM Kenexa Umfrage Enterprise (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit IBM Kenexa Umfrage benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein IBM Kenexa Umfrage Enterprise Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. IBM Kenexa Umfrage Unternehmen aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa Umfrage Unternehmen aus der Galerie hinzufügen
Zum Konfigurieren der Integration von IBM Kenexa Umfrage Enterprise in Azure AD müssen Sie der Liste der verwalteten SaaS-apps IBM Kenexa Umfrage Enterprise aus dem Katalog hinzufügen.

**Um IBM Kenexa Umfrage Unternehmen aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **IBM Kenexa Umfrage Enterprise**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)

7. Wählen Sie im Ergebnisbereich **IBM Kenexa Umfrage Unternehmen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit IBM Kenexa Umfrage basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer gegenüber IBM Kenexa Umfrage Unternehmen einem Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in IBM Kenexa Umfrage Unternehmen hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in IBM Kenexa Umfrage Unternehmen.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit IBM Kenexa Umfrage müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen von IBM Kenexa Umfrage Unternehmen testen Benutzer](#creating-an-kenexasurvey-test-user)** - Gegenstück Britta Simon in IBM Kenexa Umfrage Unternehmen haben, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
6. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der IBM Kenexa Umfrage Enterprise-Anwendung konfigurieren.


**Konfigurieren Azure AD einmaliges Anmelden mit IBM Kenexa Umfrage Schritte:**

1. Klicken Sie im Verwaltungsportal auf der Seite **IBM Kenexa Umfrage Enterprise** Application Integration **Konfigurieren einmaliges** öffnen das Dialogfeld **Konfigurieren Sie einmaliges Anmelden** .

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei IBM Kenexa Umfrage Enterprise** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)

    ein. Geben Sie im Textfeld **ID** eine URL dem folgenden Muster:`https://surveys.kenexa.com/<company code>`

    b. Geben Sie im Feld **Antwort-URL** eine URL dem folgenden Muster:`https://surveys.kenexa.com/<company code>/tools/sso.asp`

    c. Klicken Sie auf **Weiter**.

    > [AZURE.NOTE] Beachten Sie, dass diese nicht die tatsächlichen Werte. Sie müssen diese Werte mit der tatsächlichen Bezeichner und Antwort-URL aktualisieren. Wenden Sie sich an IBM Kenexa Umfrage Enterprise Support-Team, diese Werte abzurufen.

4. Auf der Seite **Konfigurieren einmaliges IBM Kenexa Umfrage Unternehmens** auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png) 

5. Um SSO für die Anwendung konfigurierten, wenden Sie IBM Kenexa und bieten Sie ihnen Folgendes:

    • Der heruntergeladenen Datei

    • Der **Aussteller URL**

    • **SAML SSO-URL**

    • **Einzelne Abmeldung Dienst-URL**

    > [AZURE.NOTE] Ist Notiz behaupten NameID Wert in der Antwort, mit SSO-ID im Kenexa System konfiguriert. So geben Sie Arbeit mit Kenexa-Supportteam Bezeichner für den entsprechenden Benutzer in Ihrer Organisation als SSO-ID zuordnen Standardmäßig wird Azure AD die NameIdentifier als UPN-Wert festgelegt. Sie können diese Registerkarte Attribut, wie im folgenden Screenshot gezeigt ändern. Die Integration funktioniert nur nach Abschluss die korrekte Zuordnung. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)

6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]

8. **Klicken Sie auf im klassischen Azure-Portal, auf **IBM Kenexa Umfrage Enterprise** Application Integrationsseite im Menü oben.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_06.png)

9. Gehen Sie im Dialogfeld **SAML-token-Attribute** :

    ein. Wählen Sie das Attribut **NameIdentifier** , und klicken Sie auf **Bearbeiten** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_07.png)
    
    b. Geben Sie aus **Attributwert** Attributwert SSO-ID, die im Kenexa System konfiguriert ist.
    
    c. Klicken Sie auf **vollständige**

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-ibm-kenexa-survey-enterprise-test-user"></a>Erstellen einen Testbenutzer IBM Kenexa Umfrage Enterprise

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in IBM Kenexa Umfrage Unternehmen. Arbeiten Sie mit IBM Kenexa Support-Team für alle Benutzer die SSO-ID zuordnen. Auch sollte dieser SSO-ID-Wert der NameIdentifier-Wert von Azure AD zugeordnet werden. Sie können diese Standardeinstellungen in der Registerkarte Attribut ändern.


> [AZURE.NOTE] Benötigen Sie einen Benutzer manuell erstellen, müssen Sie das IBM Kenexa Umfrage Enterprise Support Team.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges IBM Kenexa Umfrage Unternehmen Zugang gewähren.

![Benutzer zuweisen][200] 

**Um IBM Kenexa Umfrage Enterprise Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **IBM Kenexa Umfrage Enterprise**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Anklicken von IBM Kenexa Umfrage Unternehmen nebeneinander in der Sie sollte automatisch der IBM Kenexa Umfrage Enterprise-Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png
