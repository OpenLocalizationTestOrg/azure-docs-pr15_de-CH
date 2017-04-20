<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit SAP Business ByDesign | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und SAP Business ByDesign."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Lernprogramm: Azure Active Directory-Integration mit SAP Business ByDesign

In diesem Lernprogramm lernen Sie SAP Business ByDesign in Azure Active Directory (Azure AD) integriert.

Integrieren von SAP Business ByDesign in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer auf SAP Business ByDesign Zugriff
- Sie können die Benutzer automatisch auf SAP Business ByDesign (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit SAP Business ByDesign benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine SAP Business ByDesign Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. SAP Business ByDesign aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP Business ByDesign aus der Galerie hinzufügen
Zum Konfigurieren der Integration des SAP Business ByDesign in Azure AD müssen Sie der Liste der verwalteten SaaS-apps SAP Business ByDesign aus dem Katalog hinzufügen.

**Um SAP Business ByDesign aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.


3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **SAP Business ByDesign**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **SAP Business ByDesign aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit SAP Business ByDesign basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Gegenstück Benutzer in SAP Business ByDesign an einen Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen einem Azure AD-Benutzer und die zugehörigen Benutzer in SAP Business ByDesign hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in SAP Business ByDesign.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit SAP Business ByDesign müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen von SAP Business ByDesign Benutzer testen](#creating-an-sap-business-bydesign-test-user)** - Gegenstück Britta Simon SAP Business ByDesign enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in Ihrer SAP Business ByDesign-Anwendung konfigurieren.

SAP erwartet Business ByDesign SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie folgenden Angaben für diese Anwendung. Sie können die Werte dieser Attribute auf der Registerkarte **"Polysaccharid"** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel dafür. 


**Konfigurieren Azure AD einmaliges Anmelden mit SAP Business ByDesign die folgenden Schritte:**


1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **SAP Business ByDesign** -Anwendung im Menü oben.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Wählen Sie in der Liste der Attribute SAML-token Attribute das Attribut Nameidentifier und klicken Sie dann auf **Bearbeiten**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. Gehen Sie im Dialogfeld Attribut bearbeiten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    ein. Wählen Sie aus der Liste Attribut **ExtractMailPrefix()** Funktion

    b. Wählen Sie aus der Liste Benutzer-Attribut für die Implementierung verwenden möchten. 
    Beispielsweise wenn EmployeeID als eindeutige Benutzer-ID verwenden möchten, und den Wert des Attributs in der ExtensionAttribute2 gespeichert haben, wählen Sie **user.extensionattribute2**. 

    c. Klicken Sie auf **abgeschlossen**. 
    

4. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **SAP Business ByDesign** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

5. Auf der Seite **Wie möchten Sie Benutzern, SAP Business ByDesign anmelden** **Azure AD einmaliges Anmelden**wählen Sie aus und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    ein. **Anmelde-URL** -Textfeld Geben Sie den URL verwendet Ihre Benutzer anmelden, der SAP Business ByDesign-Anwendung mit dem folgenden Muster:`https://<servername>.sapbydesign.com`
    
    b. Klicken Sie auf **Weiter**
 
7. Auf der Seite **Konfigurieren einmaliges Anmelden am SAP Business ByDesign** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


8. Zu SSO für die Anwendung konfigurierten die folgenden Schritte:

    ein. Melden Sie sich auf Ihr SAP Business ByDesign Portal mit Administratorrechten an.

    b. Navigieren Sie zur **Anwendung und allgemeine Verwaltungsaufgabe Benutzer** und klicken Sie auf der Registerkarte **Identitätsanbieter** .

    c. Klicken Sie auf **Neue Identitätsprovider** und wählen Sie Metadaten, die von klassischen Azure-Portal heruntergeladen haben. Importieren der Metadaten, lädt das System automatisch erforderliche Signaturzertifikat und Verschlüsselungszertifikat.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Um die **Assertion Consumer Service URL** in SAML-Anforderung enthalten **Sind Assertion Consumer Service URL**auswählen

    e. Klicken Sie auf **Single Sign-On aktivieren**.

    f. Speichern.

    g. Klicken Sie auf die Registerkarte **Mein System** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. **SSO-URL** kopieren und in **Azure AD anmelden URL** -Textfeld einzufügen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    Ich. Geben Sie an, ob der Mitarbeiter manuell zwischen **Manuellen Identität Provider-Auswahl**auswählen mit Benutzer-ID und Kennwort oder SSO anmelden kann.

    j. Geben Sie im Abschnitt **SSO-URL** die URL, die der Mitarbeiter für die Anmeldung an das System verwendet werden soll. 
    In der URL gesendet Mitarbeiter Dropdown-Liste können Sie zwischen folgenden Optionen auswählen:
    
    **Nicht-SSO-URL**
 
    Das System sendet nur die normalen URL für den Mitarbeiter. Der Mitarbeiter kann nicht über SSO anmelden muss Kennwort verwenden und stattdessen Zertifikat.

    **SSO-URL** 

    Das System sendet nur die SSO-URL für den Mitarbeiter. Der Mitarbeiter kann über SSO anmelden. Authentifizierungsanfrage wird durch die IdP umgeleitet.

    **Automatische Auswahl**
 
    Wenn SSO nicht aktiviert ist, sendet das System den normalen URL für den Mitarbeiter. Wenn SSO aktiviert ist, überprüft das System, ob der Mitarbeiter ein. Ist ein Kennwort verfügbar, SSO-URL und nicht-SSO-URL an den Mitarbeiter gesendet. Wenn der Mitarbeiter kein Kennwort hat, wird nur die SSO-URL an den Mitarbeiter gesendet.

    k. Speichern.

9. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Erstellen eines Benutzers mit SAP Business ByDesign test

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in SAP Business ByDesign. Arbeiten Sie mit SAP Business ByDesign Support-Team in die Plattform SAP Business ByDesign Benutzer hinzu. 

> [AZURE.NOTE] Stellen Sie sicher, dass Feld Username Plattform SAP Business ByDesign NameID Wert entsprechen muss.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges SAP Business ByDesign Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon SAP Business ByDesign zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SAP Business ByDesign**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Anklicken der Kachel SAP Business ByDesign in der Sie sollte automatisch zur Anwendung SAP Business ByDesign angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
