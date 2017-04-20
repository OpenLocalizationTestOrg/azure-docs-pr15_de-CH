<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Optimizely | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Optimizely."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Lernprogramm: Azure Active Directory-Integration mit Optimizely

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) Optimizely integrieren.

Azure AD Optimizely Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Optimizely Zugriff
- Sie können die Benutzer automatisch angemeldet-Optimizely (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Optimizely benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein **Optimizely** Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Optimizely aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-optimizely-from-the-gallery"></a>Optimizely aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Optimizely in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Optimizely aus dem Katalog hinzufügen.

**Um Optimizely aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Optimizely**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Optimizely aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Optimizely basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück im Optimizely einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Optimizely hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Optimizely.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit Optimizely müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Optimizely testen](#creating-an-optimizely-test-user)** - Gegenstück Britta Simon Optimizely enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Optimizely.

Optimizely erwartet SAML-Assertionen enthalten ein Attribut namens "e-Mail". Der Wert "e-Mail" sollte e-Mail Optimizely erkannt, die von Azure AD authentifiziert abrufen können. Konfigurieren Sie den Anspruch "e-Mail" für diese Anwendung. Sie können die Werte dieser Attribute auf der Registerkarte **"Atrributes"** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel dafür. 


![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Konfigurieren Azure AD einmaliges Anmelden mit Optimizely die folgenden Schritte:**

1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **Optimizely** Anwendung im Menü oben.**
     
    ![Einmaliges Anmelden konfigurieren][5]

2. Fügen Sie im Dialogfeld SAML token Attribute "e-Mail" Attribut.

    ein. Klicken Sie auf **Attribut hinzufügen** , um das Dialogfeld **Attribut hinzufügen** . 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. Geben Sie im Textfeld **Attribut** das Attribut "e-Mail".

    c. Wählen Sie aus **Attributwert** Attribut Wert "Userprincipalname" oder jeder Wert, der eine von Azure AD erkannt e-Mail und Optimizely.

    d. Klicken Sie auf **abgeschlossen**.
3. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren][6]
4. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Optimizely** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][7] 

5. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Optimizely** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    ein. Geben Sie in das Textfeld **Anmelde-URL** :`https://app.optimizely.net/contoso`

    b. Geben Sie im Textfeld **ID**`urn:auth0:optimizely:contoso`

    c. Klicken Sie auf **Weiter**. 


    > [AZURE.NOTE] Die Werte für die **Anmelde-URL** und **Bezeichner** sind nur Platzhalter für den tatsächlichen Werten. Sie finden Anleitung zum Erwerb der tatsächlichen Werte von Optimizely später in diesem Lernprogramm.

7. Auf der Seite **Konfigurieren einmaliges Anmelden am Optimizely** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Kopieren Sie die **URL des Diensts für einmaliges Anmelden**.

8. Um für die Anwendung konfigurierten SSO, wenden Sie sich an Ihren Kundenbetreuer Optimizely und Angaben Sie folgende:

    - Das heruntergeladene Zertifikat 
    - Der Dienst für einmaliges Anmelden-URL
 
    Auf Ihre e-Mail-Adresse bietet Optimizely die Anmelde-URL (SP initiiertes SSO) und die Kennung Service Provider Entität Werte.

9. **Configure App Settings** Dialog Seite zurück, und führen Sie die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** die **SP initiiertes SSO-URL** Optimizely.

    b. Geben Sie im Textfeld **ID** die **Service Provider Entitäts-ID** von Optimizely.

    c. Klicken Sie auf **Weiter**.

10. Auf der Seite **Konfigurieren einmaliges Anmelden am Optimizely** Schritte:
    
    ![Azure AD einmaliges Anmelden][10]

    ein. Wählen Sie die Konfiguration für einzelne Zeichen Bestätigung.

    b. Klicken Sie auf **Weiter**.

11. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]

12. In einem anderen Browserfenster Anmeldung der Anwendung Optimizely.
13. Klicken Sie in der oberen rechten Ecke und **Konto**Konto.

    ![Azure AD einmaliges Anmelden](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Auf der Registerkarte Konto das Kontrollkästchen Sie **SSO aktivieren** unter einmaliges Anmelden im Abschnitt " **Übersicht** ".

    ![Azure AD einmaliges Anmelden](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.
Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-optimizely-test-user"></a>Erstellen eines Benutzers Optimizely test

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Optimizely.

1. Wählen Sie auf der Homepage **Mitarbeiter** Registerkarte
2. Klicken Sie auf **Neuer Mitarbeiter** dem Projekt einen neuen Mitarbeiter hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Geben Sie die e-Mail-Adresse und weisen Sie eine Rolle zu. Klicken Sie auf **Laden**.


    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Sie erhalten eine Einladung per e-Mail. Verwenden die e-Mail-Adresse. Sie müssen sich in Optimizely.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Optimizely Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon Optimizely zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Optimizely**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste alle Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf die Optimizely Fläche in der Sie sollte automatisch zur Anwendung Optimizely angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
