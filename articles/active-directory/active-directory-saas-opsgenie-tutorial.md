<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit OpsGenie | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und OpsGenie."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Lernprogramm: Azure Active Directory-Integration mit OpsGenie

Das Ziel dieses Lernprogramms ist, wie OpsGenie in Azure Active Directory (Azure AD) integriert werden.

Azure AD OpsGenie Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf OpsGenie Zugriff
- Sie können die Benutzer automatisch angemeldet-OpsGenie (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit OpsGenie benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein OpsGenie SSO-Abonnement aktiviert


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. OpsGenie aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie aus der Galerie hinzufügen
Konfigurieren Sie die Integration von OpsGenie in Azure AD müssen Sie der Liste der verwalteten SaaS-apps OpsGenie aus dem Katalog hinzufügen.

**Um OpsGenie aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **OpsGenie**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. Wählen Sie im Ergebnisbereich **OpsGenie aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-OpsGenie basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden funktioniert benötigt Azure AD Benutzer Gegenstück in OpsGenie an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in OpsGenie hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in OpsGenie.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit OpsGenie müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer OpsGenie testen Benutzer](#creating-a-opsgenie-test-user)** - Gegenstück Britta Simon OpsGenie enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden in Azure-Verwaltungsportal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung OpsGenie.



**Konfigurieren Azure AD einmaliges Anmelden mit OpsGenie die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **OpsGenie** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei OpsGenie** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    ein. Geben Sie im Textfeld Anmelde-URL die URL die Benutzer anmelden, der OpsGenie-Anwendung mit dem folgenden Muster: **"https://app.opsgenie.com/auth/login"**.

    > [AZURE.NOTE] Wenden Sie die [OpsGenie-Supportteam](mailto:support@opsgenie.com) benötigen Sie Ihre Anmelde-URL.

    b. Klicken Sie auf **Weiter**.


4. Auf der Seite **Konfigurieren einmaliges Anmelden am OpsGenie** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer. Wir benötigen dieses Zertifikat und Metadaten-URLs (Entity ID SSO-Zeichen In URL und Zeichen, URL) SSO auf OpsGenie festgelegt.

    b. Klicken Sie auf **Weiter**.


5. Eine weitere Browserinstanz geöffnet und dann melden Sie sich als Administrator OpsGenie.

6. **Klicken Sie**und klicken Sie dann auf die Registerkarte **Single Sign-On** .
 
    ![OpsGenie einmaliges Anmelden](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. Wählen Sie zum Aktivieren von SSO **aktiviert**.

    ![OpsGenie Optionen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. **Im Abschnitt** klicken Sie auf die Registerkarte **Azure Active Directory** .

    ![OpsGenie Optionen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Auf der Dialogseite Azure Active Directory die folgenden Schritte:
 
    ![OpsGenie Optionen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    ein. Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am OpsGenie** den **Einzelnen Zeichen Service URL** -Wert, und fügen Sie ihn in das Textfeld **SAML 2.0-Endpunkt** .

    b. Erstellen Sie eine Base64-codierte Datei heruntergeladene Zertifikat.      
    
    > [AZURE.NOTE] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be).

    c. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **X. 500-Zertifikat** .

    d. Klicken Sie auf **Speichern**.


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.



![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-opsgenie-test-user"></a>Erstellen einen Testbenutzer OpsGenie

Dieser Abschnitt soll Benutzer Britta Simon in OpsGenie erstellen. 

1.  Ein Webbrowserfenster in Ihrem Mandanten OpsGenie als Administrator anmelden.

2.  Navigieren Sie zu Liste **Benutzer** im linken Bereich.
   
    ![OpsGenie Optionen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Klicken Sie auf "**Benutzer hinzufügen**".

3.  Gehen Sie im Dialogfeld **Benutzer hinzufügen** :

    ![OpsGenie Optionen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    ein. Geben Sie in das Textfeld **E-Mail** Adresse Brittas Azure Active Directory.

    b. Geben Sie im Textfeld **Vorname** **Britta Simon**.

    c. Klicken Sie auf **Speichern**. 

Britta erhalten eine e-Mail mit ihrem Profil einrichten.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon mit Azure einmaliges ihr Zugriff auf OpsGenie aktivieren.

![Benutzer zuweisen][200] 

**Britta Simon OpsGenie zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
 
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **OpsGenie**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Klicken auf die OpsGenie Fläche in der Sie sollte automatisch zur Anwendung OpsGenie angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
