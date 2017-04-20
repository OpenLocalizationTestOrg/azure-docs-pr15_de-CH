<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit PerformanceCentre | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und PerformanceCentre."
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


# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Lernprogramm: Azure Active Directory-Integration mit PerformanceCentre

Das Ziel dieses Lernprogramms ist, wie PerformanceCentre in Azure Active Directory (Azure AD) integriert werden.  
Azure AD PerformanceCentre Integration bietet Ihnen folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf PerformanceCentre Zugriff 
- Sie können die Benutzer automatisch angemeldet-PerformanceCentre (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - Active Directory Azure-Verwaltungsportal verwalten.

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit PerformanceCentre benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein PerformanceCentre Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. PerformanceCentre aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-performancecentre-from-the-gallery"></a>PerformanceCentre aus der Galerie hinzufügen
Konfigurieren Sie die Integration von PerformanceCentre in Azure AD müssen Sie der Liste der verwalteten SaaS-apps PerformanceCentre aus dem Katalog hinzufügen.

**Um PerformanceCentre aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **PerformanceCentre**.
 
    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **PerformanceCentre aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-PerformanceCentre basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in PerformanceCentre einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in PerformanceCentre hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in PerformanceCentre.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit PerformanceCentre müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer PerformanceCentre testen Benutzer](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon PerformanceCentre enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden in Azure AD-Verwaltungsportal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung PerformanceCentre.

**Konfigurieren Azure AD einmaliges Anmelden mit PerformanceCentre die folgenden Schritte:**

1. Klicken Sie im klassischen Azure AD-Portal auf Anwendungsseite Integration **PerformanceCentre** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei PerformanceCentre** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7] 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Azure AD einmaliges Anmelden][8] 
 
     ein. Geben Sie im Textfeld **Anmelde-URL** die URL von Benutzern zur Website PerformanceCentre anmelden (z.B.: *http://companyname.performancecentre.com/saml/SSO*).
 
     b. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden am PerformanceCentre** Schritte:

    ![Azure AD einmaliges Anmelden][9] 

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.



1. Anmeldung bei der Website Ihres Unternehmens **PerformanceCentre** als Administrator.

2. Klicken Sie im Register auf der linken Seite auf **Konfigurieren**.

    ![Azure AD einmaliges Anmelden][10]

2. Klicken Sie im Register auf der linken Seite auf **Sonstiges**und dann auf **Einmalanmeldung**.

    ![Azure AD einmaliges Anmelden][11]

2. Wählen Sie als **Protokoll** **SAML**.

    ![Azure AD einmaliges Anmelden][12]

2. Öffnen Sie der heruntergeladenen Metadatendatei in Editor kopieren Sie den Inhalt, **Identität Metadaten** Textbox fügen Sie ein und klicken Sie auf **Speichern**.

    ![Azure AD einmaliges Anmelden][13]

2. Überprüfen Sie die Werte für die **Entität Basis-URL** und **Entity ID URL** .

    ![Azure AD einmaliges Anmelden][14]


6. Azure AD-Verwaltungsportal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][15]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][16]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-performancecentre-test-user"></a>Erstellen einen Testbenutzer PerformanceCentre

Dieser Abschnitt soll Benutzer Britta Simon in PerformanceCentre erstellen.

**Gehen Sie zum Erstellen eines Benutzers Britta Simon in PerformanceCentre aufgerufen:**

1. Melden Sie sich als Administrator auf der Website Ihres Unternehmens PerformanceCentre an.

2. Klicken Sie im Menü auf der linken Seite auf **Interrelate**und klicken Sie dann auf **Teilnehmer erstellen**.

    ![Benutzer erstellen][400]

4. Gehen Sie im Dialogfeld **Interrelate - Teilnehmer erstellen** :

    ![Benutzer erstellen][401]

    ein. Geben Sie die erforderlichen Attribute für Britta Simon in verknüpfte Textfelder.
    
    > [AZURE.IMPORTANT] Brittas User Name-Attribut in PerformanceCentre muss der Benutzername identisch in Azure AD.


    b. Wählen Sie als **Rolle** **Client Administrator** . 

    c. Klicken Sie auf **Speichern**.   


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff auf PerformanceCentre aktivieren.

![Benutzer zuweisen][200] 

**Britta Simon PerformanceCentre zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **PerformanceCentre**.

    ![Benutzer zuweisen][202]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die PerformanceCentre Fläche in der Sie sollte automatisch zur Anwendung PerformanceCentre angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_01.png
[500]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_02.png

[6]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_03.png
[8]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_04.png
[9]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_05.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png
[15]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_06.png
[16]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_50.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png
[402]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_402.png



