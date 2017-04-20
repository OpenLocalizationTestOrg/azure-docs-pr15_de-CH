<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration oC Gerber - AppreciateHub | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und oC Gerber - AppreciateHub."
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


# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a>Lernprogramm: Azure Active Directory Integration oC Gerber - AppreciateHub

Dieses Lernprogramm soll oC Integration anzeigen Gerber - AppreciateHub mit Azure Active Directory (Azure AD).  
Integration von oC Gerber - bietet AppreciateHub mit Azure folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf oC Zugriff Gerber - AppreciateHub 
- Sie können die Benutzer automatisch angemeldet-oC auf Abrufen Gerber - AppreciateHub (einmaliges Anmelden) mit ihren Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Azure AD-Integration mit OC konfigurieren Gerber - AppreciateHub, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- EINE OC Gerber - AppreciateHub Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. Hinzufügen von oC Gerber - AppreciateHub aus der Galerie 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-oc-tanner---appreciatehub-from-the-gallery"></a>Hinzufügen von oC Gerber - AppreciateHub aus der Galerie
Die Integration von OC konfigurieren Gerber - AppreciateHub in Azure AD müssen oC hinzufügen Gerber - AppreciateHub aus der Galerie zur Liste der verwalteten SaaS-apps.

**OC Tanner - AppreciateHub aus der Galerie hinzufügen die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1] 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2] 

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3] 

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4] 

6. Geben Sie im Suchfeld **OC Tanner - AppreciateHub**.

    ![Applikationen][5] 

7. Wählen Sie im Ergebnisbereich **OC Tanner - AppreciateHub aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][25] 




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden

Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit oC Gerber - Testbenutzer namens "Britta Simon" AppreciateHub Grundlage.

Einmaliges Anmelden funktioniert muss Azure AD den Gegenstück in oC Benutzer kennen Tanner - ist ein Benutzer in Azure AD AppreciateHub. In anderen Worten eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer oC Gerber - AppreciateHub eingerichtet werden muss.  
Diese Beziehung hergestellt, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in oC Gerber - AppreciateHub.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit oC Gerber - AppreciateHub, müssen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Gerber oC - AppreciateHub-Testbenutzer erstellen](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon oC enthalten Gerber - AppreciateHub, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Dieser Abschnitt soll Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren Sie einmaliges Anmelden in der oC Gerber - AppreciateHub Anwendung.


**Konfigurieren von Azure AD folgendermaßen einmaliges Anmelden mit oC Tanner - AppreciateHub:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **OC Tanner - AppreciateHub** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6]

2. Auf der Seite **Wie möchten Sie Benutzer anmelden oC Tanner - AppreciateHub** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7]

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Anwendung konfigurieren][8]
 
     ein. Öffnen Sie die Metadatendatei über den folgenden Link: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).

     b. Suchen Sie den Knoten **Md:AssertionConsumerService** . 

     c. Kopieren Sie den Wert des **Location** -Attributs. 

     ![Anwendung konfigurieren][12]
     
     d. Im Textfeld **Anmelde-URL** hinter den Wert haben Sie im vorherigen Schritt erhalten.

     > [AZURE.NOTE] Erste Antwort-URL von der Metadatendatei erleben Probleme, wenden Sie die oC Gerber - Supportteam über AppreciateHub [sso@octanner.com](mailto:sso@octanner.com).

     e. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden bei oC Tanner - AppreciateHub** auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Was ist Azure AD verbinden][9]

5. Wenden Sie sich an die oC Gerber - Supportteam über Xyz, AppreciateHub ihnen mit der Metadatendatei, und sie wissen lassen sie SSO für Sie aktivieren.


6. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Was ist Azure AD verbinden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Was ist Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_06.png)
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a>Erstellen einer oC Gerber - Testbenutzer AppreciateHub

Dieser Abschnitt soll Benutzer Britta Simon in oC erstellen Gerber - AppreciateHub.

**Erstellen Benutzer Britta Simon in oC Tanner - AppreciateHub, die folgenden Schritte:**

1. Bitten Sie einen Benutzer erstellen, die wie NameID-Attribut den gleichen Wert wie die Benutzernamen von Britta Simon in Azure AD Supportteams OC Tanner.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges oC Zugang gewähren aktivieren Gerber - AppreciateHub.

![Benutzer zuweisen][200]

**Britta Simon Tanner oC - AppreciateHub, zuweisen Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **OC Tanner - AppreciateHub**.

    ![Benutzer zuweisen][202]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die oC Gerber - AppreciateHub untereinander die Abdeckung Sie sollten erhalten automatisch angemeldet an Ihre oC Gerber - AppreciateHub Anwendung.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_01.png
[25]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_25.png


[6]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_02.png
[8]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_03.png
[9]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_04.png
[10]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_05.png
[11]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_06.png
[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png
[20]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_07.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_205.png






