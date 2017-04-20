<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Alcumus Info | Microsoft Azure"
    description="Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und Alcumus Information Exchange konfigurieren."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Lernprogramm: Azure Active Directory-Integration mit Alcumus Informationen

Dieses Lernprogramm soll zeigen, wie Sie Azure Active Directory (Azure AD) Alcumus Info Exchange integriert.  
Integrieren von Azure AD Alcumus Info Exchange bietet folgende Vorteile: 

- Sie können in Azure AD steuern Zugriff auf Alcumus Info Exchange 
- Sie können die Benutzer automatisch auf Alcumus Info Exchange (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit Alcumus Informationen benötigen Sie Folgendes:

- Ein [Azure AD](https://azure.microsoft.com/) -Abonnement
- Ein [Alcumus Info Exchange](http://www.alcumusgroup.com/) Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. Alcumus Info Exchange aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Alcumus Info Exchange aus der Galerie hinzufügen
Zum Konfigurieren der Integration von Alcumus Info Exchange in Azure AD müssen Sie die Liste der verwalteten SaaS-apps Alcumus Info Exchange aus dem Katalog hinzufügen.

**Um Alcumus Info Exchange aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Alcumus Info Exchange**ein.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **Alcumus Info Exchange aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden mit Alcumus Informationen basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Alcumus Info Exchange einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer in Alcumus Info Exchange hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Alcumus Info Exchange.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit Alcumus Informationen müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Alcumus Info Exchange Benutzer testen](#creating-a-alcumus-info-exchange-test-user)** - Gegenstück Britta Simon Alcumus Info Exchange enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Alcumus Info Exchange.

**Um Azure AD einmaliges Anmelden mit Alcumus Informationen zu konfigurieren, führen Sie die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der **Alcumus Info Exchange** Application Integration auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6]

2. Auf der Seite **Wie möchten Sie Benutzer Alcumus Info Exchange anmelden** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7]

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte: 

    ![Azure AD einmaliges Anmelden][8]
 
    ein. Geben Sie im Feld **Antwort-URL** der Consumer-URL, die Sie von Ihrem Supportteam Alcumus Info Exchange konnte.

    > [AZURE.NOTE] Wenn Sie nicht wissen, was der richtige Wert ist, wenden Sie die Alcumus Info Exchange über [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden zu Alcumus Informationen** auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Was ist Azure AD verbinden][9]

5. Wenden Sie die Alcumus Info Exchange über [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com)bieten die Metadatendatei und sie wissen lassen sie SSO für Sie aktivieren.


6. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Was ist Azure AD verbinden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Was ist Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.
  
    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.
  
    c. Klicken Sie auf Weiter.



6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  
  
    b. In den **Namen** Txtbox, Typ, **Simon**.
  
    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.
  
    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
  
    e. Klicken Sie auf **Weiter**.


7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.
  
    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Erstellen einen Testbenutzer Alcumus Info Exchange

Dieser Abschnitt soll Benutzer Britta Simon Alcumus Info Exchange erstellen.

**Erstellen Sie einen Benutzer namens Britta Simon Alcumus Info Exchange folgendermaßen Sie an:**

1. Wenden Sie die Alcumus Info Exchange über [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff auf Alcumus Info Exchange aktivieren.

![Benutzer zuweisen][200]

**Britta Simon Alcumus Info Exchange zuweisen, die folgenden Schritte:**

1. Azure-Portal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Applikationen** im oberen Menü.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Alcumus Info Exchange**.

    ![Benutzer zuweisen][202]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf Alcumus Info Exchange Kachel in der Sie sollte automatisch zur Anwendung Alcumus Info Exchange angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png