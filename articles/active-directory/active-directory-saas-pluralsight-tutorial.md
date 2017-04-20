<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Pluralsight | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Pluralsight."
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


# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Lernprogramm: Azure Active Directory Integration Pluralsight

Dieses Lernprogramm soll veranschaulichen Pluralsight Azure Active Directory (Azure AD) integrieren.

Integrieren von Azure AD Pluralsight bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer auf Pluralsight Zugriff
- Sie können die Benutzer automatisch angemeldet-Pluralsight (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten


Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Pluralsight benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Pluralsight einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Pluralsight aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-pluralsight-from-the-gallery"></a>Pluralsight aus der Galerie hinzufügen
Zum Konfigurieren der Integration von Pluralsight in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Pluralsight aus dem Katalog hinzufügen.

**Um Pluralsight aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
 
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Pluralsight**.
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Pluralsight aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Anmelden bei Pluralsight basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Konfigurieren und Azure AD einmaliges Anmelden bei Pluralsight testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Pluralsight test](#creating-a-pluralsight-test-user)** - Gegenstück Britta Simon Pluralsight enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Pluralsight.

Die Anwendung Pluralsight erwartet SAML-Assertionen in einem bestimmten Format hinzufügen benutzerdefiniertes Attribut Mappings der SAML-token Attribute Konfiguration erfordern. Der folgende Screenshot zeigt ein Beispiel dafür. 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_02.png) 

Sie können das Attribut **"Einmalige Nr."** mit dem entsprechenden Wert EmployeeID oder etwas passt auch für Ihre Organisation hinzufügen. Beachten Sie, dass dies nicht das erforderliche Attribut. Sie können jedoch, um die eindeutige Identifikation des Benutzers hinzufügen. 

**Konfigurieren Azure AD einmaliges Anmelden bei Pluralsight Schritte:**

1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **Pluralsight** -Anwendung im Menü oben.**

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_81.png) 



1. So entfernen Sie die redundante **SAML-token-Attribute**: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/2829.png) 


    ein. Für jedes Benutzerattribut in den roten Rahmen der Tabelle zeigen auf das Attribut und dann auf Löschen. 




1. Fügen Sie die erforderlichen **SAML-token Attribute**für jede Zeile in der Tabelle unten aufgeführten Schritte:

  	| Attributname | Attributwert |
  	| --- | --- |    
  	| Vorname| User.GivenName |
  	| Nachname  | User.Surname |
  	| E-Mail | User.Mail |

    ein. Klicken Sie auf **Hinzufügen Benutzer Attribure** Dialogfeld **Attribut hinzufügen** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_82.png) 


    b. Geben Sie im Textfeld **Attrubute** den Attributnamen für diese Zeile angezeigt.

    c. Der **Attributwert** Liste Selsect den Attributwert für die Zeile angezeigt.

    d. Klicken Sie auf **abgeschlossen**.  
    


1. Klicken Sie auf **Übernehmen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/3232.png)  



1. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_83.png)  









1. Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Pluralsight** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Pluralsight** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_03.png) 

3. Auf der **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_04.png) 


    ein. Anmelde-URL-Textfeld Geben Sie den URL verwendet Ihre Benutzer anmelden, der Pluralsight-Anwendung mit dem folgenden Muster:`https://<instance name>.pluralsight.com/sso/<comapny name>`

    b. Klicken Sie auf **Weiter**.


4. Auf der Seite **Konfigurieren einmaliges Anmelden bei Pluralsight** folgendermaßen:  ![konfigurieren Sie einmaliges Anmelden](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_05.png) 

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Um SSO für die Anwendung konfigurierten, kontaktieren Sie Pluralsight [Professional Services](mailTo:professionalservices@pluralsight.com) -Team und geben Sie den heruntergeladenen Metadaten.


6. Wählen Sie im klassischen Azure-Portal die Konfiguration für einzelne Zeichen Bestätigung und klicken Sie dann auf **Weiter**.
  
    ![Azure AD einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

Wählen Sie in der Liste Benutzer **Britta Simon**.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-pluralsight-test-user"></a>Erstellen einen Testbenutzer Pluralsight

Dieser Abschnitt soll Benutzer Britta Simon in Pluralsight erstellen. Arbeiten Sie mit Pluralsight-Supportteam Benutzer Pluralsight-Konto hinzufügen. 


> [AZURE.NOTE]Benötigen Sie einen Benutzer manuell erstellen, müssen Sie den Pluralsight-Support wenden.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Pluralsight Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon Pluralsight zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Pluralsight**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_50.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Anklicken von Pluralsight Kachel in der Sie sollte automatisch zur Anwendung Pluralsight angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_205.png
