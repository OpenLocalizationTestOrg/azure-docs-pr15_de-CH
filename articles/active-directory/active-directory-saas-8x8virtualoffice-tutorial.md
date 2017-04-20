<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit 8 x 8 virtuellen | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und 8 x 8 virtuellen."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Lernprogramm: Azure Active Directory-Integration mit 8 x 8 Virtual Office

Dieses Lernprogramm soll zeigen, wie Sie Azure Active Directory (Azure AD) 8 x 8 Virtual Office integrieren.

Integration von 8 x 8 bietet Virtual Office mit Azure folgende Vorteile:

- Sie können in Azure AD steuern mit Zugriff auf 8 x 8 Virtual Office
- Sie können die Benutzer automatisch auf 8 x 8 Virtual Office (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit 8 x 8 Virtual Office benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein 8 x 8 Virtual Office Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Microsoft Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. 8 x 8 Virtual Office hinzufügen aus der Galerie
2. Konfigurieren und Testen von Microsoft Azure AD einmaliges Anmelden


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 Virtual Office hinzufügen aus der Galerie
Zum Konfigurieren der Integration von 8 x 8 Virtual Office in Azure AD müssen Sie die Liste der verwalteten SaaS-apps 8 x 8 Virtual Office aus dem Katalog hinzufügen.

**Um 8 x 8 Virtual Office aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **8 x 8 Virtual Office**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. Wählen Sie im Ergebnisbereich **8 x 8 Virtual Office aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Microsoft Azure AD einmaliges Anmelden
Ziel dieses Abschnitts wird erläutert, wie Sie konfigurieren und Testen der Microsoft Azure AD einmaliges Anmelden mit Namen "Britta Simon" Testbenutzer Virtual Office auf 8 x 8.

Für einmaliges Anmelden zu arbeiten muss Azure AD virtuelle Benutzer in Azure AD ist den Gegenstück in 8 x 8 Benutzer kennen. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in 8 x 8 Virtual Office hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in 8 x 8 Virtual Office.

Konfigurieren und Testen Sie Microsoft Azure AD einmaliges Anmelden mit 8 x 8 Virtual Office müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Microsoft Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Microsoft Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[8 x 8 Virtual Office Testbenutzer erstellen](#creating-a-8x8-virtual-office-test-user)** - Gegenstück Britta Simon 8 x 8 Virtual Office enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Microsoft Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD einmaliges Anmelden konfigurieren

In diesem Abschnitt Microsoft Azure AD einmaliges Anmelden im klassischen Portal aktivieren und konfigurieren im Anwendungscode Büro 8 x 8.

**Um mit 8 x 8 virtuellen Microsoft Azure AD Single Sign-On konfigurieren, führen Sie die folgenden Schritte:**

1. Klicken Sie im klassischen Portal auf der Seite **8 x 8 Virtual Office** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer sich auf 8 x 8 Virtual Office** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Führen Sie auf der **App-Einstellungen konfigurieren** die folgenden Schritte aus, und **Klicken Sie auf**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    ein. Geben Sie im Feld **Antwort-URL**`https://sso.8x8.com/saml2`

    b. Klicken Sie auf **Weiter**

4. Gehen Sie auf der Seite **Konfigurieren Sie einmaliges Anmelden 8 x 8 virtuellen Büro** und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

5. Anmeldung bei Ihrem Büro 8 x 8 Mandanten als Administrator.
6. Wählen Sie **Virtual Office Account Manager** auf Anwendung.

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Wählen Sie **Geschäftskonto verwalten, und klicken Sie auf **Anmelden** ** .

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Klicken Sie in der Liste **Konten** .

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Klicken Sie in der Liste der Konten **Single Sign-On** .

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. **Einzelnes anmelden** unter Authentifizierungsmethode wählen Sie aus und auf **SAML**.

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Kopieren von **SAML SSO-URL** **einem singen Dienst-URL** und **Aussteller URL** von Azure AD **URL anmelden**, **Abmelden URL** und **Aussteller URL** im Büro 8 x 8 

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Anwendung auf Konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Klicken Sie auf **Schaltfläche zum Hochladen des Zertifikats die von Azure AD heruntergeladen** .

13. Klicken Sie auf die Schaltfläche **Speichern** .

14. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

15. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.
    
![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>8 x 8 Virtual Office Testbenutzer erstellen

Dieser Abschnitt soll Benutzer Britta Simon in 8 x 8 Virtual Office erstellen. 8 x 8 unterstützt Virtual Office Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Es ist keine Aufgabe für Sie in diesem Abschnitt. Ein neuer Benutzer wird bei dem Versuch, 8 x 8 Virtual Office access noch nicht erstellt. 

> [AZURE.NOTE] Möchten Sie einen Benutzer manuell erstellen, müssen Sie das 8 x 8 Virtual Office Support-Team kontaktieren.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges Anmelden ihren Zugriff auf 8 x 8 Virtual Office aktivieren.
    
![Benutzer zuweisen][200]

**Um 8 x 8 Virtual Office Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **8 x 8 Virtual Office**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Ihre Microsoft Azure AD-auf Konfiguration mit der testen.

Beim Anklicken von 8 x 8 Virtual Office nebeneinander in der Sie sollte automatisch anwendungsspezifische Büro 8 x 8 angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
