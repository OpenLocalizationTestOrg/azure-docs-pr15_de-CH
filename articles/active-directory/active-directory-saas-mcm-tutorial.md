<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration MCM | Microsoft Azure" 
    description="Erfahren Sie, wie mit MCM Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Lernprogramm: Azure Active Directory Integration MCM
  
Dieses Lernprogramm soll veranschaulichen MCM Azure Active Directory (Azure AD) integrieren.

Integrieren von Azure AD MCM bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer auf MCM Zugriff
- Sie können die Benutzer automatisch angemeldet-MCM (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit MCM benötigen Sie Folgendes:

- Ein gültiges Azure-Abonnement
- MCM einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.

## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. MCM aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden

## <a name="adding-mcm-from-the-gallery"></a>MCM aus der Galerie hinzufügen
Konfigurieren die Integration MCM Azure AD müssen Sie der Liste der verwalteten SaaS-apps MCM aus dem Katalog hinzufügen.

**Um MCM aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **MCM**.

    ![Anwendung Galerie] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **MCM aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden mit MCM basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück in MCM an einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer MCM hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in MCM.

Konfigurieren und Azure AD einmaliges Anmelden mit MCM testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Benutzer erstellen eine MCM test](#creating-a-mcm-test-user)** - Gegenstück Britta Simon MCM enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden
  
In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der MCM-Anwendung konfigurieren.

**Konfigurieren Azure AD einmaliges Anmelden mit MCM Schritte:**

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **MCM** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden MCM** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD einmaliges Anmelden")

3.  Führen Sie auf der App-Einstellungen zu konfigurieren die folgenden Schritte:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Konfigurieren von App-URL")

    ein. Geben Sie im Textfeld **Anmelde-URL** : `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Klicken Sie auf **Weiter**

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am MCM** **Metadaten**auf und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Einmaliges Anmelden konfigurieren")

5. Um SSO für die Anwendung konfigurierten zu erhalten, wenden Sie die MCM. Anfügen der heruntergeladenen Metadatendatei und gemeinsam mit MCM SSO auf Seite einrichten.

6.  Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Einmaliges Anmelden konfigurieren")

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Einmaliges Anmelden konfigurieren")


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD

Dieser Abschnitt soll im klassischen Portal namens Britta Simon Testbenutzer erstellen.

![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   

###<a name="creating-a-mcm-test-user"></a>Erstellen einen Testbenutzer MCM
  
In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in MCM. Arbeiten Sie mit MCM-Supportteam MCM Plattform Benutzer hinzufügen.

>[AZURE.NOTE]Alle anderen MCM Benutzer Konto Erstellungstools verwenden oder APIs von MCM Bereitstellung AAD Benutzerkonten.


###<a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer
  
Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges MCM Zugang gewähren.
    
![Benutzer zuweisen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Benutzer zuweisen")

**Britta Simon MCM zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.
    
    ![Benutzer zuweisen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Benutzer zuweisen")

2. Wählen Sie in der Anwendungsliste **MCM**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. Klicken Sie im Menü oben auf **Benutzer**.
    
    ![Benutzer zuweisen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Benutzer zuweisen")

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.
    
    ![Benutzer zuweisen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Benutzer zuweisen")


### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.
 
Beim Anklicken der Kachel MCM in der Sie sollte automatisch zur Anwendung MCM angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)