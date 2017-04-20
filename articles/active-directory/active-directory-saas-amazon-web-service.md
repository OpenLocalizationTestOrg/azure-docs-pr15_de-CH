<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Amazon Web Service (AWS) | Microsoft Azure"
    description="Erfahren Sie, wie mit Amazon Web Services (AWS) mit Active Directory Azure-auf automatisierte Bereitstellung und mehr!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Lernprogramm: Azure Active Directory-Integration mit Amazon Web Service (AWS)

Das Ziel dieses Lernprogramms ist, wie Amazon Web Service (AWS) in Azure Active Directory (Azure AD) integriert werden.  
Integration von Amazon Web Service (AWS) in Azure AD bietet folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf Amazon Web Service (AWS Zugriff) 
- Sie können die Benutzer automatisch auf Amazon Web Service (AWS) (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Um Azure AD-Integration mit Amazon Web Service (AWS) konfigurieren, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Amazon Web Service (AWS) Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. Hinzufügen von Amazon Web Service (AWS) aus der Galerie 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Service (AWS) aus der Galerie
Zum Konfigurieren der Integration von Amazon Web Service (AWS) in Azure AD müssen Sie die Liste der verwalteten SaaS-apps Amazon Web Service (AWS) aus dem Katalog hinzufügen.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Um Amazon Web Service (AWS) aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1] 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** . 
   
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** . 
   
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**. 
   
    ![Applikationen][4]

6. Geben Sie im Suchfeld **Amazon Web Service (AWS)**.
   
    ![Applikationen][5]

7. Klicken Sie im Ergebnisbereich wählen Sie **Amazon Web Service (AWS aus)**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit Amazon Web Service (AWS) basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Amazon Web Service (AWS) an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Amazon Web Service (AWS) hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Amazon Web Service (AWS).
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit Amazon Web Service (AWS) müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelne Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Amazon Web Service (AWS) testen](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon in Amazon Web Service (AWS) aufweisen, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfiguration von Azure AD-einem Single Sign-On

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Amazon Web Service (AWS).  
Ihre Amazon Web Service (AWS) erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss. Der folgende Screenshot zeigt ein Beispiel dafür.


![Einmaliges Anmelden konfigurieren][27]

**Konfigurieren Sie einmaliges Anmelden für Azure AD mit Amazon Web Service (AWS) die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Amazon Web Service (AWS)** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][7]

2. Auf der Seite **Wie möchten Sie Benutzer anmelden, Amazon Web Service (AWS)** **Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren][8]

3. Klicken Sie auf der **App-Einstellungen zu konfigurieren** auf Weiter. 

    ![Anwendung konfigurieren][9]
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden am Amazon Web Service (AWS)** auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren][10]

5. In einem anderen Browserfenster Anmelden der Unternehmenswebsite Amazon Web Service (AWS) als Administrator.

6. Klicken Sie auf **Konsole Homepage**.

    ![Einmaliges Anmelden konfigurieren][11]

7. Klicken Sie auf **Identitäts- und Zugriffsmanagement**. 

    ![Einmaliges Anmelden konfigurieren][12]

8. **Identitätsanbieter**auf und dann auf **Provider erstellen**. 

    ![Einmaliges Anmelden konfigurieren][13]

9. Führen Sie auf der **Anbieter konfigurieren** die folgenden Schritte: 

    ![Einmaliges Anmelden konfigurieren][14]

     ein. Wählen Sie als **Typ** **SAML**.

     b. Geben Sie im Textfeld **Name des Anbieters** einen Anbieternamen (z.B.: *WAAD*).

     c. Klicken Sie zum Hochladen der heruntergeladenen Metadatendatei auf **Datei auswählen**.

     d. Klicken Sie auf **Nächstes**.


10. Klicken Sie auf die **Informationen zu überprüfen** auf **Erstellen**. 

    ![Einmaliges Anmelden konfigurieren][15]

11. Klicken Sie auf **Rollen**, und klicken Sie auf **Neue Rolle erstellen**. 

    ![Einmaliges Anmelden konfigurieren][16]

12. Gehen Sie im Dialogfeld **Rollenname festgelegt** : 

    ![Einmaliges Anmelden konfigurieren][17]

    ein. Geben Sie in das Textfeld **Rollenname** ein Rollenname (z.B.: *TestUser*).

    b. Klicken Sie auf **Nächstes**.

13. Gehen Sie im Dialogfeld **Typ der Rolle auswählen** : 

    ![Einmaliges Anmelden konfigurieren][18]

    ein. Wählen Sie die **Rolle für Identität Anbieter zugreifen**.

    b. Klicken Sie im Abschnitt **Gewähren von Single Sign-On (WebSSO) auf SAML-Anbieter** **Wählen**.


14. Gehen Sie im Dialogfeld **Vertrauensstellung einzurichten** :  

    ![Einmaliges Anmelden konfigurieren][19]
     
     ein. SAML-Anbieter wählen Sie SAML erstellten Previousley (z.B.: *WAAD*) 

     b. Klicken Sie auf **Nächstes**.


15. Klicken Sie im Dialogfeld **Überprüfen Rolle vertrauen** auf **Nächstes**. 

    ![Einmaliges Anmelden konfigurieren][32]


16. Klicken Sie auf **Nächstes**Dialogfeld **Richtlinie anfügen** .  

    ![Einmaliges Anmelden konfigurieren][33]


17. Gehen Sie im Dialogfeld **Überprüfung** :   

    ![Einmaliges Anmelden konfigurieren][34]

     ein. Kopieren Sie den **Rolle ARN** Wert.

     b. **Vertrauenswürdige Entitäten** ARN Wert kopieren.

     c. Klicken Sie auf **Rolle erstellen**. 

18. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Was ist Azure AD verbinden][20]

19. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** **vollständig** **Konfigurieren SSO -** Dialogfeld zu schließen.

    ![Was ist Azure AD verbinden][22]


20. Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen. 

    ![Einmaliges Anmelden konfigurieren][21]

21. Klicken Sie auf **Attribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][23]

22. Gehen Sie im Dialogfeld Attribut hinzufügen. 

    ![Einmaliges Anmelden konfigurieren][24] 

    ein. Geben Sie im Textfeld **Attribut** **https://aws.amazon.com/SAML/Attributes/Role**.

    b. Geben Sie im Textfeld **Attributwert** **[Rolle ARN-Value] [vertrauenswürdige Entität ARN Wert]**.

    >[AZURE.TIP] Bei der Erstellung Ihrer Rolle im Dialogfeld Überprüfung kopierten Werte sind. 

    c. Klicken Sie auf **vollständig** das Dialogfeld **Attribut hinzufügen** zu schließen.

23. Klicken Sie auf **Attribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][23]


24. Gehen Sie im Dialogfeld Attribut hinzufügen. 

    ![Einmaliges Anmelden konfigurieren][25]


     ein. Geben Sie im Textfeld **Attribut** **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. Geben Sie im Textfeld **Attributwert** oder **user.userprincipalname** aus der Dropdown-Liste auswählen.
     
    ![Einmaliges Anmelden konfigurieren][35]
    

     c. Klicken Sie auf **vollständig** das Dialogfeld **Attribut hinzufügen** zu schließen.


25. Klicken Sie auf **Übernehmen**. 

    ![Einmaliges Anmelden konfigurieren][26]





### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD

Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.
  2. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.
  3. Klicken Sie auf Weiter.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. In den **Namen** Txtbox, Typ, **Simon**.
  
    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
  
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.
  
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Erstellen einen Testbenutzer Amazon Web Service (AWS)

Dieser Abschnitt soll Benutzer Britta Simon in Amazon Web Service (AWS) erstellen.

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Erstellen Sie einen Benutzer namens Britta Simon in Amazon Web Service (AWS) die folgenden Schritte:

1. Melden Sie sich Ihr Unternehmensstandort **Amazon Web Service (AWS)** als Administrator an.

2. Klicken Sie auf **Konsole Homepage** . 

    ![Einmaliges Anmelden konfigurieren][11]

3. Klicken Sie auf Identität und Management. 

    ![Einmaliges Anmelden konfigurieren][28]

4. Klicken Sie im Dashboard auf Benutzer, und klicken Sie auf neuen Benutzer erstellen. 

    ![Einmaliges Anmelden konfigurieren][29]

5. Gehen Sie im Dialogfeld Benutzer erstellen: 

    ![Einmaliges Anmelden konfigurieren][30]

     ein. Geben Sie in die Textfelder **Eingeben Benutzernamen** Brita Simon Benutzernamen (Userprincipalname) in Azure AD.

     b. Klicken Sie auf **Erstellen**.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges Anmelden ihren Zugriff auf Amazon Web Service (AWS) aktivieren.

![Benutzer zuweisen][31]

**Britta Simon CloudPassage zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][26]

2. Wählen Sie in der Anwendungsliste **Amazon Web Service (AWS)**.

    ![Benutzer zuweisen][27]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][25]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][29]

### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Anklicken der Kachel Amazon Web Service (AWS), in der Sie sollte automatisch anwendungsspezifische Amazon Web Service (AWS) angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















