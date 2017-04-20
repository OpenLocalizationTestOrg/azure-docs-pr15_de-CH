<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Jive | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Jive."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Lernprogramm: Azure Active Directory-Integration mit Jive

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) Jive integrieren.

Azure AD Jive Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Jive Zugriff
- Sie können die Benutzer automatisch angemeldet-Jive (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Jive benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Jive Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Jive aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-jive-from-the-gallery"></a>Jive aus der Galerie hinzufügen
Konfigurieren Sie die Integration von Jive in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Jive aus der Galerie hinzufügen.

**Um Jive aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Jive**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. Wählen Sie im Ergebnisbereich **Jive aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Jive basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück im Jive einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Jive hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** im Jive.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit Jive müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen ein Jive Benutzer testen](#creating-a-jive-test-user)** - Gegenstück Britta Simon Jive enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Benutzerbereitstellung konfigurieren](#configuring-user-provisioning)** - benutzerbereitstellung von Active Directory-Benutzer aktivieren Gliedern Konten Jive.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
6. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Jive.

**Konfigurieren Azure AD einmaliges Anmelden mit Jive die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Jive** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Jive** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** die URL die Benutzer anmelden, der Jive-Anwendung mit dem folgenden Muster: **https://\<Customer Name\>. jivecustom.com**.
    
    b. Klicken Sie auf **Weiter**
 
4. Auf der Seite **Konfigurieren einmaliges Anmelden am Jive** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Anmeldung bei Ihrem Mandanten Jive als Administrator.

6. Klicken Sie im Menü oben auf "**Saml**".

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    ein. Wählen Sie auf der Registerkarte **Allgemein** **aktiviert** .

    b. Klicken Sie auf die Schaltfläche "**Alle Saml speichern**".

7. Navigieren Sie zur Registerkarte "**Idp Metadaten**".

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    ein. Kopieren Sie den Inhalt der heruntergeladenen Metadaten-XML-Datei, und fügen Sie ihn in das Textfeld **Identität Anbieter (IDP) Metadaten** .

    b. Klicken Sie auf die Schaltfläche "**Alle Saml speichern**". 

8. Wechseln Sie zur Registerkarte "**Benutzer Attribut zuordnen**".

    ![Einmaliges Anmelden auf App konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    ein. Kopieren Sie im Textfeld **E-Mail** und fügen Sie der Attributname **Mail** -Wert ein.

    b. Kopieren Sie im Textfeld **Vorname** und fügen Sie der Attributname **"givenName"** -Wert ein.

    c. Kopieren Sie im Textfeld **Nachname** und fügen Sie der Attributname der **Name** -Wert ein.
    
9. Wählen Sie im Portal Azure AD Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **Weiter**.
![Azure AD einmaliges Anmelden][10]

10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
  ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf das **Benutzerprofil** , gehen Sie folgendermaßen vor: ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



###<a name="creating-a-jive-test-user"></a>Erstellen einen Testbenutzer Jive

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Jive. Arbeiten Sie mit Jive Unterstützung der Benutzer in der Jive-Plattform hinzufügen.


###<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Dieser Abschnitt soll aktivieren benutzerbereitstellung von Active Directory-Benutzerkonten, Jive beschreiben.  
Im Rahmen dieses Verfahrens müssen Sie ein Benutzersicherheitstoken bieten von Jive.com anfordern.
  
Der folgende Screenshot zeigt ein Beispiel für das entsprechende Dialogfeld in Azure AD:

![Konfigurieren Sie Benutzer bereitstellen] (./media/active-directory-saas-jive-tutorial/IC698794.png "Konfigurieren Sie Benutzer bereitstellen")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Klicken Sie in Azure-Verwaltungsportal auf Anwendungsseite Integration **Jive** Dialogfeld **Benutzerbereitstellung konfigurieren** **Konfigurieren Benutzer bereitstellen** .

2.  Bieten Sie auf der Seite **Geben Sie Ihre Jive Anmeldeinformationen ermöglichen automatisiertes provisioning** folgende Einstellungen:

    1.  Geben Sie im Textfeld **Jive Admin-Benutzernamen** einen Jive Kontonamen, das **System Administrator** -Profil in Jive.com zugewiesen.

    2.  Geben Sie im Textfeld **Jive Admin-Kennwort** das Kennwort für dieses Konto.

    3.  Geben Sie im Textfeld **Jive Mieter URL** URL Mieter Jive.

        >[AZURE.NOTE]Die Jive Mieter ist URL Ihrer Organisation mit Jive anmelden.  
        Die URL hat normalerweise Folgendes Format: **Www.\< Organisation\>. jive.com**.

    4.  Klicken Sie auf **Überprüfen** , um Ihre Konfiguration zu überprüfen.

    5.  Klicken Sie auf **Weiter** , um **die Bestätigungsseite** zu öffnen.

3.  Klicken Sie auf **der Bestätigungsseite** auf das Häkchen, um die Konfiguration zu speichern.
  
Sie können jetzt erstellen Sie ein Testkonto 10 Minuten warten und überprüfen, ob das Konto mit Jive.com synchronisiert wurde.




### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Jive Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon Jive zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Jive**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Anklicken der Kachel Jive in der Sie sollte automatisch zur Anwendung Jive angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
