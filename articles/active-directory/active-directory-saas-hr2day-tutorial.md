<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit HR2day von Merces | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und HR2day von Merces."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Lernprogramm: Azure Active Directory-Integration mit HR2day von Merces

Das Ziel dieses Lernprogramms ist, wie HR2day von Merces in Azure Active Directory (Azure AD) integriert werden.  
Integration von HR2day von Merces in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer auf HR2day von Merces Zugriff
- Sie können die Benutzer automatisch angemeldet-HR2day von Merces (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit HR2day von Merces benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- HR2day von Merces Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. HR2day von Merces aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>HR2day von Merces aus der Galerie hinzufügen
Konfigurieren Sie die Integration von HR2day von Merces in Azure AD müssen Sie der Liste der verwalteten SaaS-apps HR2day von Merces aus dem Katalog hinzufügen.

**Um HR2day von Merces aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
 
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **HR2day von Merces**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. Wählen Sie im Ergebnisbereich **HR2day von Merces aus**und dann auf **vollständig** die Anwendung hinzufügen.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen der Azure AD einmaliges Anmelden mit HR2day von Merces basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in HR2day von Merces einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer in HR2day von Merces hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in HR2day von Merces.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit HR2day von Merces müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer HR2day durch Merces testen Benutzer](#creating-a-hr2day-by-merces-test-user)** - Gegenstück Britta Simon HR2day von Merces enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Dieser Abschnitt soll Gliedern wie Benutzer HR2day von Merces Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

Die HR2day Merces Anwendung erwartet SAML-Assertionen in einem bestimmten Format hinzufügen benutzerdefiniertes Attribut Mappings der SAML-token Attribute Konfiguration erfordern. Der folgende Screenshot zeigt ein Beispiel dafür. 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Die SAML-Assertion konfigurieren zu können, müssen Ihre HR2day Kundenservice über [servicedesk@merces.nl](mailto:servicedesk@merces.nl) und den Wert des Attributs Kennung für Ihren Mandanten. Sie benötigen diesen Wert, um die Schritte im nächsten Abschnitt.


**Konfigurieren Azure AD einmaliges Anmelden mit HR2day von Merces die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf Integration **HR2day von Merces** Anwendung im Menü oben auf **Attribute** **SAML-Token-Eigenschaften** -Dialogfeld öffnen. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Erforderliches Attribut Zuordnung hinzufügen, gehen Sie, gehen Sie folgendermaßen vor: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    ein. Klicken Sie auf **Attribut hinzufügen**.

    b. Geben Sie im Textfeld **Attribut** **"ATTR_LOGINCLAIM"**.

    c. Wählen Sie aus der Liste **Attributwert** **Join()**. 

    d. Wählen Sie aus der Liste **Zeichenfolge1** **User.mail**. 

    e. Geben Sie im Textfeld **String2** vom HR2day-Team bereitgestellten **Bezeichner** . 

    f. Geben Sie im Textfeld **Trennzeichen** **@**.

    g. Klicken Sie auf **abgeschlossen**.

  
3. Klicken Sie auf **Übernehmen**.


1. Klicken Sie im Menü oben auf **Quick Start** , um **das Dialogfeld** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Klicken Sie auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer HR2day von Merces anmelden** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    ein. Geben Sie im Textfeld Anmelde-URL die Benutzer anmelden, um Ihre HR2day von Merces-Anwendung das folgende Muster verwendete URL: **"https://\<Mieter Name\>.force.com/\<Instanznamen\>"**.

    b. Klicken Sie auf **Weiter**.

4. Auf der Seite **Konfigurieren einmaliges Anmelden am HR2day von Merces** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Zu SSO für die Anwendung konfigurierten wenden Sie sich an die HR2day von Merces Support-Team über [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) und Ihre e-Mail-Adresse die heruntergeladenen Datei zuordnen. Außerdem Bitte geben Sie SAML SSO-URL, Zeichen, URL und Aussteller URL, damit sie für SSO-Integration konfiguriert werden können.


> [AZURE.NOTE] Bitte Erwähnen Sie Merces Team, dass diese Integration Entitäts-ID mit diesem Muster **https://hr2day.force.com/INSTANCENAME**



6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>HR2day von Merces Testbenutzer erstellen

Dieser Abschnitt soll Benutzer Britta Simon in HR2day von Merces erstellen. Arbeiten Sie mit HR2day, Merces Support Team Benutzer Konto HR2day hinzufügen. 


> [AZURE.NOTE]Möchten Sie einen Benutzer manuell erstellen, müssen Sie die HR2day Merces Support-Team kontaktieren.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure SSO-HR2day von Merces keinen Zugriff erteilen.

![Benutzer zuweisen][200] 

**Um Britta Simon HR2day von Merces zuzuweisen, führen Sie die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **HR2day von Merces**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Anklicken der HR2day von Merces untereinander der Sie sollte automatisch an die HR2day Merces Anwendung angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png