<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Schnellhilfe | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Schnellhilfe."
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


# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Lernprogramm: Azure Active Directory Integration Schnellhilfe

Dieses Lernprogramm soll zeigen, wie Sie die Schnellhilfe in Azure Active Directory (Azure AD) integriert.  
Integrieren von Azure AD Schnellhilfe bietet folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf Schnellhilfe Zugriff 
- Sie können die Benutzer automatisch angemeldet-Schnellhilfe (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit Schnellhilfe benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Schnellhilfe einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Schnellhilfe aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-quickhelp-from-the-gallery"></a>Schnellhilfe aus der Galerie hinzufügen
Konfigurieren die Integration Schnellhilfe Azure AD müssen Sie der Liste der verwalteten SaaS-apps Schnellhilfe aus dem Katalog hinzufügen.

**Um Schnellhilfe aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Schnellhilfe**.

    ![Applikationen][5]

7. Klicken Sie im Ergebnisbereich wählen Sie **Schnellhilfe**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Schnellhilfe basierend auf einen Testbenutzer "Britta Simon" bezeichnet.


Konfigurieren und Azure AD einmaliges Anmelden mit Schnellhilfe testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Schnellhilfe test](#creating-a-quickhelp-test-user)** - Gegenstück Britta Simon Schnellhilfe enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Schnellhilfe.


**Um mit Schnellhilfe Azure AD einmaliges Anmelden konfigurieren möchten, führen Sie die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Schnellhilfe** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden Schnellhilfe** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][7] 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Anwendung konfigurieren][8] 
 
     ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer Ihrer Site Schnellhilfe anmelden (z.B.:* https://quickhelp.com/bsiazure/*).

     > [AZURE.NOTE] Wenden Sie das Schnellhilfe Support-Team den Wert der Anmelde-URL nicht kennen.

     b. Klicken Sie auf **Weiter**.

 
4. Führen Sie auf der Seite **Konfigurieren einmaliges Anmelden am Schnellhilfe** die folgende Schritte aus: Klicken Sie auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.

    ![Was ist Azure AD verbinden][9] 



1. Anmeldung bei der Website Ihres Unternehmens Schnellhilfe als Administrator.

2. Klicken Sie im Menü oben auf **Administration**.

    ![Einmaliges Anmelden konfigurieren][21]


1. **Klicken Sie im Menü **Schnellhilfe Admin** klicken.**

    ![Einmaliges Anmelden konfigurieren][22]

1. Klicken Sie auf **Authentifizierung**.

1. Auf der Seite **Authentifizierung** gehen

    ![Einmaliges Anmelden konfigurieren][23]

    ein. Wählen Sie als **SSO-Typ** **WSFederation**.

    b. Hochladen der heruntergeladenen Azure Metadatendatei **Durchsuchen**, navigieren Sie zur Datei, beenden Sie klicken Sie auf **Hochladen Metadaten**.

    c. Geben Sie in das Textfeld **E-Mail** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    d. Im Textfeld **Vorname** **Typ http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. Im Textfeld **Nachname** **Typ http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    f. Klicken Sie in der **Aktionsleiste**auf **Speichern**.







6. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Was ist Azure AD verbinden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Was ist Azure AD verbinden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  
Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 
 
4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte: 
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) 
 
    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) 
 
8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) 
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

  
 
### <a name="creating-a-quickhelp-test-user"></a>Erstellen einen Testbenutzer Schnellhilfe

Dieser Abschnitt soll Benutzer Britta Simon in Schnellhilfe erstellen.
Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück Schnellhilfe Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer Schnellhilfe hergestellt werden.

Schnellhilfe unterstützt Just-in-Time-Bereitstellung. Dies bedeutet, wenn erforderlich, automatisch ein Benutzerkonto in Schnellhilfe erstellt wird und das Konto Azure AD-Konto verknüpft ist.

Es ist keine Aufgabe für Sie in diesem Abschnitt.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Schnellhilfe Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon Schnellhilfe zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Schnellhilfe**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf Schnellhilfe Kachel in der Sie sollte automatisch zur Anwendung Schnellhilfe angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png




