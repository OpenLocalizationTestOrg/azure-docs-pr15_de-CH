<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Lifesize Cloud | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Lifesize."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Lernprogramm: Azure Active Directory-Integration mit Lifesize Cloud

In diesem Lernprogramm lernen Sie Lifesize Cloud in Azure Active Directory (Azure AD) integriert.

Integrieren von Azure AD Lifesize Cloud bietet die folgenden Vorteile:

- Sie können in Azure AD steuern mit Zugriff auf Lifesize Cloud
- Sie können die Benutzer automatisch Lifesize Cloud (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Lifesize Cloud benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Lifesize Cloud Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Lifesize Cloud aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize Cloud aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Lifesize Cloud in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Lifesize Cloud aus dem Katalog hinzufügen.

**Um Lifesize Cloud aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Lifesize Cloud**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Lifesize Cloud aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Test Azure AD einmaliges Anmelden mit Lifesize Cloud basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in Lifesize Cloud für einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Lifesize Cloud hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Lifesize Cloud.

Konfigurieren und Azure AD einmaliges Anmelden mit Lifesize Cloud testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer Lifesize Cloud testen Benutzer](#creating-a-lifesize-cloud-test-user)** - Gegenstück Britta Simon Lifesize Cloud enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und -auf der Lifesize Cloud-Anwendung konfigurieren.


**Um Azure AD einmaliges Anmelden mit Lifesize Cloud zu konfigurieren, führen Sie die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Lifesize Cloud** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer Lifesize Cloud anmelden** **Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer anmelden, der Lifesize Cloud-Anwendung mit dem folgenden Muster: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Klicken Sie auf **Weiter**
 
4. Auf der Seite **Konfigurieren einmaliges Lifesize Cloud** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. SSO für Ihre Anwendung anmelden Lifesize Cloudanwendung mit Administratorrechten konfiguriert wird abgerufen.

6. Klicken Sie in der oberen rechten Ecke auf Ihren Namen, und klicken Sie dann auf **Erweiterte Einstellungen**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. In den erweiterten Einstellungen jetzt bitte **SSO-Konfiguration** . Dadurch wird die SSO-Konfigurationsseite für die Instanz geöffnet.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Konfigurieren Sie die folgenden Werte in der SSO-Konfiguration UI jetzt.    

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Den Wert Aussteller URL von Azure AD kopieren und einfügen, die **Identität Anbieter Aussteller** Textfeld.

    • Wert Remote Login-URL von Azure AD kopieren und einfügen, die **Anmelde-URL** -Textfeld.

    • Heruntergeladene Zertifikat im Editor öffnen und kopieren Sie den Inhalt des Zertifikats, ausgenommen die Zeilen Begin Certificate und End Certificate, **X. 509-Zertifikat** -Textfeld einzufügen.

    • Zuordnung SAML-Attribut für das Textfeld **Vorname** Geben Sie den Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • Zuordnung SAML-Attribut für das Textfeld **Nachname** Geben Sie den Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • Zuordnung SAML-Attribut für das **E-Mail** -Textfeld Geben Sie den Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Überprüfen die Konfiguration können Sie auf die Schaltfläche **Testen** klicken.

    > [AZURE.NOTE] Für erfolgreiche Tests müssen Sie den Konfigurations-Assistenten in Azure AD und bieten auch Zugriff auf Benutzer oder Gruppen, die den Test durchführen können.
    
10. Aktivieren Sie das SSO auf **SSO zu aktivieren** .

11. Klicken Sie jetzt auf die Schaltfläche **Aktualisieren** , damit die Einstellung gespeichert werden. Dadurch wird den RelayState Wert generiert. Kopieren Sie den RelayState-Wert im Textfeld generiert wird. Wir brauchen diesen Wert in den nächsten Schritten.

12. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

13. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]

14. Jetzt anmelden mit Administratoranmeldeinformationen Azure-Verwaltungsportal- **https://portal.azure.com**

15. Klicken Sie auf **Weitere Dienste** im linken Navigationsbereich
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Suche nach Azure Active Directory, und klicken Sie auf den Link **Azure Active Directory**
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. Die SaaS-Anwendung unter der Schaltfläche **Anwendung** finden.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Klicken Sie auf **Alle** links auf dem nächsten Blatt
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Suchen Sie nach Lifesize Anwendung der RelayState eingerichtet werden soll. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Klicken Sie auf Link **auf** Blatt

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Sie sehen das Kontrollkästchen **Erweiterte URL-Einstellungen anzeigen** . Klicken Sie auf das Kontrollkästchen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Nun konfigurieren Sie RelayState für die Anwendung im Lifesize Anwendung SSO-Konfigurationsseite angezeigt. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. Speichern.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf das **Benutzerprofil** , gehen Sie folgendermaßen vor: ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Erstellen einen Testbenutzer Lifesize Cloud

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon Lifesize Cloud. Lifesize Cloud unterstützt automatische Benutzer bereitstellen. Nach erfolgreicher Authentifizierung bei Azure AD wird der Benutzer automatisch in der Anwendung bereitgestellt werden. 


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Lifesize Cloud Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon Lifesize Cloud zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Lifesize Cloud**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Klicken auf die Lifesize Cloud Kachel in der Sie sollte automatisch zur Anwendung Lifesize Cloud angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
