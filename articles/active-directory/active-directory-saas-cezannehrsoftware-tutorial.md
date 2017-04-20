<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Cezanne HR Software | Microsoft Azure"
    description="Erfahren Sie, wie einmaliges Anmelden zwischen Azure Active Directory und Cezanne HR-Software konfigurieren."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Lernprogramm: Azure Active Directory-Integration mit Cezanne h

Das Ziel dieses Lernprogramms ist zeigen Cezanne HR Azure Active Directory (Azure AD) integrieren.

Azure AD Cezanne HR Software integrieren bietet folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf Cezanne HR-Software
- Sie können die Benutzer automatisch Cezanne HR Software (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Cezanne HR Software benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cezanne HR Software Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Cezanne HR Software aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR Software aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Cezanne HR Software in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Cezanne HR Software aus der Galerie hinzufügen.

**Um Cezanne HR Software aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .
    
    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Cezanne HR-Software**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. Wählen Sie im Bedienfeld "Ergebnisse" **Cezanne HR Software**, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Wählen die Anwendung in der Galerie](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Cezanne HR Software basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Gegenstück Benutzer in HR-Software Cezanne an einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer in HR-Software Cezanne hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Cezanne HR-Software.

Konfigurieren und Azure AD einmaliges Cezanne HR Software testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer Cezanne HR Software testen Benutzer](#creating-a-cezanne-hr-software-test-user)** - Gegenstück Britta Simon Cezanne HR-Software enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in Ihrer Anwendung Cezanne HR-Software konfigurieren.

**Konfigurieren Azure AD einmaliges Cezanne HR-Software die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Cezanne HR Software** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden Cezanne HR Software** **Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Führen Sie auf der **App-Einstellungen konfigurieren** die folgenden Schritte aus, und **Klicken Sie auf**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    ein. Geben Sie im Textfeld **Anmelde-URL** eine URL dem folgenden Muster: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Geben Sie im Feld **Antwort-URL** eine URL dem folgenden Muster: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Klicken Sie auf **Weiter**

    > [AZURE.NOTE] Beachten Sie, dass Sie diese Werte mit den tatsächlichen Anmelde-URL und Antwort-URL aktualisieren. Diese Werte zu erhalten, wenden Sie Cezanne HR Software über <mailto:info@cezannehr.com>.

4. Führen Sie auf der Seite **Konfigurieren einmaliges Cezanne HR Software** die folgenden Schritte aus und klicken Sie auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

5. Anmelden Sie in anderen Webbrowserfenster Ihrem Mandanten Cezanne HR Software als Administrator.

6. Klicken Sie im linken Navigationsbereich auf **System-Setup**. Fahren Sie mit **Sicherheit**. Navigieren Sie zu **Konfiguration für einzelne Zeichen**.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. **Benutzer können den Dienst für einmaliges Anmelden (SSO) anmelden** Bereich **SAML 2.0** das Kontrollkästchen und die Option **Advanced Configuration** daneben.

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Klicken Sie auf **Hinzufügen** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. Die folgenden Schritte auf **SAML 2.0 IDENTITÄTSANBIETER** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    ein. Geben Sie den Namen der Identitätsanbieter als **Anzeigename**.

    b. **Instanzkennzeichner** setzen Textbox **Entitäts-ID** von Azure AD-Anwendung-Konfigurationsassistenten.

    c. Ändern der **SAML Bindung** in 'POST'.

    d. Im **Security Token Service Endpunkt** setzen Textbox **Einzelne Sign-On Service URL** von Azure AD-Anwendung-Konfigurationsassistenten.

    e. Geben Sie in **Benutzername ID-Attribut**'http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name'.

    f. Klicken Sie auf **Hochladen** heruntergeladene Zertifikat aus Azure hochladen.

    g. Klicken Sie auf die Schaltfläche **Ok** . 

10. Klicken Sie auf **Speichern** .

    ![Konfigurieren von Single Sign-On auf App-Seite](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

12. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Geben Sie im Textfeld **Nachname** **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Erstellen eines Benutzers Cezanne HR Software testen

Um Azure AD Benutzer Cezanne HR Software anmelden können, müssen sie in Cezanne HR Software bereitgestellt werden. Bei Cezanne HR-Software ist die Bereitstellung einer manuellen Aufgabe.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Um ein Benutzerkonto bereitstellen, führen Sie die folgenden Schritte:

1.  Ihr Unternehmensstandort Cezanne HR Software als Administrator anmelden.

2.  Klicken Sie im linken Navigationsbereich auf **System-Setup**. Gehen Sie zum **Verwalten von Benutzern**. Navigieren Sie zum **Neuen Benutzer hinzufügen**.

    ![Neuer Benutzer] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Neuer Benutzer")

3.  Führen Sie auf **Angaben zur Person** folgende Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Neuer Benutzer")

    ein. Festlegen Sie **interne Benutzer** als OFF.

    b. Geben Sie im Textfeld **Vorname** **Britta**.  

    c. Geben Sie im Textfeld **Nachname** **Simon**.

    d. Geben Sie in das Textfeld **E-mail** die e-Mail-Adresse des Kontos Britta Simon.

4.  Führen Sie unter **Kontoinformationen** folgende Schritte aus:

    ![Neuer Benutzer] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Neuer Benutzer")

    ein. Geben Sie im Textfeld **Benutzername** die e-Mail-Adresse von Britta Simon.

    b. Geben Sie im Textfeld **Kennwort** das Kennwort des Kontos Britta Simon.

    c. Wählen Sie als **Rolle** **HR Professional** .

    d. Klicken Sie auf **OK**.

5. **Auf** der Registerkarte navigieren Sie, und wählen Sie **Neuer** **SAML 2.0-Bezeichner** .

    ![Benutzer] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Benutzer")

6. Auswählen der Identitätsanbieter **Identitätsanbieter** und im Feld **Benutzer-ID**, die e-Mail-Adresse des Kontos Britta Simon.

    ![Benutzer] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Benutzer")
    
7. Klicken Sie auf **Speichern** .

    ![Benutzer] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Benutzer")


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff Cezanne HR-Software aktivieren.
    
![Benutzer zuweisen][200]

**Britta Simon Cezanne HR Software zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Cezanne HR Software**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Anklicken der Kachel Cezanne HR-Software in der Sie sollte automatisch zur Anwendung Cezanne HR Software angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
