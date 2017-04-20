<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Litmos | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Litmos."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Lernprogramm: Azure Active Directory-Integration mit Litmos

Das Ziel dieses Lernprogramms ist, wie Litmos in Azure Active Directory (Azure AD) integriert werden.  
Azure AD Litmos Integration bietet Ihnen folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf Litmos Zugriff 
- Sie können die Benutzer automatisch angemeldet-Litmos (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - Azure Active Directory verwalten. 

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit Litmos benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Litmos Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. Litmos aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-litmos-from-the-gallery"></a>Litmos aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Litmos in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Litmos aus dem Katalog hinzufügen.

**Um Litmos aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Litmos**.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **Litmos aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-Litmos basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Litmos einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Litmos hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Litmos.
 
Konfigurieren und Testen Azure AD einmaliges Anmelden mit Litmos müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Litmos testen Benutzer](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon Litmos enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden in Azure AD-Verwaltungsportal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Litmos.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

Als Teil der Konfiguration müssen Sie **SAML-Token-Attribute** für die Litmos-Anwendung anpassen.  

![Azure AD einmaliges Anmelden][17] 

**Konfigurieren Azure AD einmaliges Anmelden mit Litmos die folgenden Schritte:**

1. Klicken Sie im klassischen Azure AD-Portal auf Anwendungsseite Integration **Litmos** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Litmos** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
 
    ![Azure AD einmaliges Anmelden][7] 


1. Zeichen auf der Website Ihres Unternehmens Litmos (z.B.: *https://azureapptest.litmos.com/account/Login*) als Administrator.

    ![Azure AD einmaliges Anmelden][21] 


1. Klicken Sie in der Navigationsleiste auf der linken Seite auf **Konten**.

    ![Azure AD einmaliges Anmelden][22] 


1. Klicken Sie auf die Registerkarte **Integration** .

    ![Azure AD einmaliges Anmelden][23] 


1. Scrollen Sie auf der Registerkarte **Integration** **3rd Party Integrationen**und klicken Sie **SAML 2.0** .

    ![Azure AD einmaliges Anmelden][24] 

1. Kopieren Sie den Wert unter **wird der SAML-Endoiint für Litmos:**.

    ![Azure AD einmaliges Anmelden][26] 


3. Der Azure-Verwaltungsportal auf der **App-Einstellungen konfigurieren** , führen Sie die folgenden Schritte aus:

    ![Azure AD einmaliges Anmelden][8] 
 
    ein. Geben Sie im Textfeld **ID** die URL Ihrer Anwendung Litmos Anmelden von Benutzern verwendet (z.B.: *https://azureapptest.litmos.com/account/Login*).
     
    b. Fügen Sie im Feld **Antwort-URL** von Litmos Anwendung im vorherigen Schritt kopierten Wert.

    c. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden am Litmos** Schritte:

    ![Azure AD einmaliges Anmelden][2] 

    ein. Klicken Sie auf Zertifikat herunterladen und speichern Sie die Datei auf Ihrem Computer.


1. In der Anwendung **Litmos** Schritte:

    ![Azure AD einmaliges Anmelden][25] 

    ein. Klicken Sie auf **SAML aktivieren**.

    b. Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

    >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    c. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **SAML x. 509-Zertifikat** .

    d. Klicken Sie auf **Speichern**.


6. Azure AD-Verwaltungsportal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  
    ![Azure AD einmaliges Anmelden][11]


20. Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen. 

    ![Einmaliges Anmelden konfigurieren][12]


24. Gehen Sie im Dialogfeld **Attribut hinzufügen** : 

    ![Einmaliges Anmelden konfigurieren][14]

  	| Attributname | Attributwert |
  	| ---            | ---             |
  	| E-Mail          | User.Mail       |
  	| Vorname      | User.GivenName  |
  	| Nachname       | User.Surname    |

    Für jede Datenzeile in der Tabelle oben aufgeführten Schritte:
   
    ein. Klicken Sie auf **Attribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][15]


    ein. Geben Sie im Textfeld **Attribut** den **Attributnamen** für diese Zeile.

    b. Wählen Sie den **Attributwert** für die Zeile.

    c. Klicken Sie auf **vollständig** das Dialogfeld **Attribut hinzufügen** zu schließen.


25. Klicken Sie auf **Übernehmen**. 

    ![Einmaliges Anmelden konfigurieren][16]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    ein. Wählen Sie als **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Erstellen einen Testbenutzer Litmos

Dieser Abschnitt soll Benutzer Britta Simon in Litmos erstellen.  
Die Litmos-Anwendung unterstützt Just-in-Time-Bereitstellung. Dadurch wird ein Benutzerkonto automatisch bei Bedarf bei die Anwendung der Zugriff erstellt.

**Gehen Sie zum Erstellen eines Benutzers Britta Simon in Litmos aufgerufen:**


1. Zeichen auf der Website Ihres Unternehmens Litmos (z.B.: *https://azureapptest.litmos.com/account/Login*) als Administrator.

    ![Azure AD einmaliges Anmelden][21] 


1. Klicken Sie in der Navigationsleiste auf der linken Seite auf **Konten**.

    ![Azure AD einmaliges Anmelden][22] 


1. Klicken Sie auf die Registerkarte **Integration** .

    ![Azure AD einmaliges Anmelden][23] 


1. Scrollen Sie auf der Registerkarte **Integration** **3rd Party Integrationen**und klicken Sie **SAML 2.0** .

    ![Azure AD einmaliges Anmelden][24] 

1. Wählen Sie **Autogenerate Benutzer:**.

    ![Azure AD einmaliges Anmelden][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff auf Litmos aktivieren.

![Benutzer zuweisen][200] 

**Britta Simon Litmos zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Litmos**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die Litmos Fläche in der Sie sollte automatisch zur Anwendung Litmos angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





