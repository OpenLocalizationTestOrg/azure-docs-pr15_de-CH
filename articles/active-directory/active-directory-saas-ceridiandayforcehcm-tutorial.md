<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Ceridians Dayforce HCM | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Ceridians Dayforce HCM."
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


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Lernprogramm: Azure Active Directory-Integration mit Ceridians Dayforce HCM

Das Ziel dieses Lernprogramms ist, wie Ceridians Dayforce HCM in Azure Active Directory (Azure AD) integriert werden.  
Azure AD Ceridians Dayforce HCM Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Ceridians Dayforce HCM Zugriff
- Sie können die Benutzer automatisch auf Ceridians Dayforce HCM (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten


Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Ceridians Dayforce HCM benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Ceridians Dayforce HCM Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Ceridians Dayforce HCM aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Ceridians Dayforce HCM aus der Galerie hinzufügen
Zum Konfigurieren der Integration Ceridians Dayforce hcm in Azure AD müssen Sie die Liste der verwalteten SaaS-apps Ceridians Dayforce HCM aus dem Katalog hinzufügen.

**Um Ceridians Dayforce HCM aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Ceridians Dayforce HCM**.

    ![Applikationen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Wählen Sie im Ergebnisbereich **Ceridians Dayforce HCM aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Applikationen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD SSO-Ceridians Dayforce HCM basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Ceridians Dayforce HCM einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Ceridians Dayforce HCM hergestellt werden.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit Ceridians Dayforce HCM müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Ceridians Dayforce HCM Benutzer testen](#creating-a-ceridian-dayforce-hcm-test-user)** - Gegenstück Britta Simon Ceridians Dayforce HCM enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Ceridians Dayforce HCM.

Die Anwendung Ceridians Dayforce HCM erwartet SAML-Assertionen in einem bestimmten Format. Arbeiten Sie mit Dayforce HCM zuerst die richtige Benutzer-ID identifiziert. Microsoft empfiehlt das Attribut **"Name"** als Benutzer-ID verwenden. Sie können den Wert dieses Attributs im Dialogfeld **"Polysaccharid"** verwalten. Der folgende Screenshot zeigt ein Beispiel dafür. 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Konfigurieren Azure AD einmaliges Anmelden mit Ceridians Dayforce HCM die folgenden Schritte:**




1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **Ceridians Dayforce HCM** Anwendung im Menü oben.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. Wählen Sie in der Attributliste **Saml-token Attribute** das Name-Attribut aus und dann auf **Bearbeiten**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. Gehen Sie im Dialogfeld **Attribut bearbeiten** :
 
    ein. Wählen Sie Liste **Attributwert** Benutzerattribut Implementierung verwenden möchten.  
    Beispielsweise wenn EmployeeID als eindeutige Benutzer-ID verwenden möchten, und den Wert des Attributs in der ExtensionAttribute2 gespeichert haben, wählen Sie **user.extensionattribute2**. 

    b. Klicken Sie auf **abgeschlossen**.  
    



1. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Klicken Sie im klassischen Azure-Portal, auf der Seite **Ceridians Dayforce HCM** Application Integration **Konfigurieren einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer Ceridians Dayforce HCM Anmeldung** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Auf der **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    ein. Geben Sie im Textfeld **Anmelde-URL** die URL von den Benutzern der Anwendung Ceridians Dayforce HCM anmelden. Verwenden Sie für produktionsumgebung das folgende URL-Format:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Aus Gründen der Übersichtlichkeit ersetzen Sie DayforcehcmNamespace durch den Namespace Ihrer Umgebung oder Firmen ID; Zum Beispiel:`https://sso.dayforcehcm.com/contoso`
    
    Verwenden Sie für testumgebung das folgende URL-Format:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. Geben Sie im Feld **Antwort-URL** den URL zur Azure AD die Antwort.  
    Produktionsumgebung verwenden:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Testumgebung verwenden:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Auf der Seite **Konfigurieren einmaliges Anmelden an Ceridians Dayforce HCM** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. SSO für die Anwendung konfigurierten erhalten, wenden Sie Ihre Ceridians Dayforce HCM per e-Mail und bieten ihnen Folgendes:

    - Die heruntergeladene Datei
    - **Aussteller-URL**
    - **SAML SSO-URL** 
    - **Einmaliges Service URL** 


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.

    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Erstellen einen Testbenutzer Ceridians Dayforce HCM

Dieser Abschnitt soll Benutzer Britta Simon in Ceridians Dayforce HCM zu erstellen. 

Arbeiten Sie an das Supportteam Ceridians Dayforce HCM Benutzer zu der in der Anwendung Ceridians Dayforce HCM. 





### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure SSO-Ceridians Dayforce HCM Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon Ceridians Dayforce HCM zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Ceridians Dayforce HCM**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Klicken auf die Ceridians Dayforce HCM Kachel in der Sie sollte automatisch zur Anwendung Ceridians Dayforce HCM angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
