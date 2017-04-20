<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration Halogenlampen Software"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Halogenlampen Software."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Lernprogramm: Azure Active Directory Integration Halogenlampen Software

Dieses Lernprogramm soll veranschaulichen Halogenlampen Azure Active Directory (Azure AD) integrieren.

Integrieren von Azure AD Halogenlampen Software bietet folgende Vorteile: 

- Sie können in Azure AD steuern Zugriff auf Halogenlampen Software 
- Sie können die Benutzer automatisch Halogenlampen Software (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit Halogenlampen Software benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Halogenlampen Software Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können. 

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Hinzufügen von Software Halogenlampen aus der Galerie 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-halogen-software-from-the-gallery"></a>Hinzufügen von Software Halogenlampen aus der Galerie
Zum Konfigurieren der Integration Halogenlampen Software in Azure AD müssen Sie die Liste der verwalteten SaaS-apps Halogenlampen Software aus dem Katalog hinzufügen.

**Um Software Halogenlampen aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** . 

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Halogenlampen Software**.

    ![Applikationen][5]

7. Klicken Sie im Ergebnisbereich wählen Sie **Halogenlampen Software aus**und dann auf **vollständig** die Anwendung hinzufügen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Halogenlampen Software basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück Halogenlampen Software für Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer Halogenlampen Software hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Halogenlampen Software.
 
Konfigurieren und Testen Azure AD einmaliges Halogenlampen Software müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD einzelne Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Benutzer erstellen eine Halogenlampen Software testen](#creating-a-halogen-software-test-user)** - Gegenstück Britta Simon Halogenlampen Software enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfiguration von Azure AD-einem Single Sign-On

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Halogenlampen Software.


**Konfigurieren Azure AD einmaliges Halogenlampen Software die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Halogenlampen Software** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][8]

2. Auf der Seite **Wie möchten Sie Benutzer anmelden Halogenlampen Software** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][9]

3. Auf der **App-Einstellungen konfigurieren** , gehen Sie folgendermaßen vor:  ![App-Einstellungen konfigurieren][10]
 
     ein. Geben Sie im Textfeld **Anmelde-URL** den URL der Halogenlampen Software-Anwendung mit dem folgenden Muster Anmelden von Benutzern verwendet: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Konfigurieren einmaliges Halogenlampen Software** auf **Metadaten**und speichern Sie Metadaten-Datei lokal auf Ihrem Computer.
    
    ![Was ist Azure AD verbinden][11]

5. In einem anderen Browserfenster Anmeldung **Halogenlampen Software** Anwendung als Administrator.

6. Klicken Sie auf die Registerkarte **Optionen** . 

    ![Was ist Azure AD verbinden][12]


7. Klicken Sie im linken Navigationsbereich auf **SAML-Konfiguration**. 

    ![Was ist Azure AD verbinden][13]

8. Auf der Konfigurationsseite **SAML** folgendermaßen:  ![was Azure AD Connect ist][14]

    ein. Wählen Sie als **Eindeutigen Bezeichner** **NameID**.

    b. Wählen Sie als **Eindeutigen Bezeichner Karten auf** **Benutzernamen**.

    c. Klicken Sie zum Hochladen der heruntergeladenen Metadatendatei auf **Durchsuchen** , um die Datei auszuwählen und dann auf **Datei hochladen**.

    d. Klicken Sie auf **Test ausführen**, um die Konfiguration zu testen. 

    > [AZURE.NOTE] Sie müssen warten für die Meldung "*der SAML-Test ist abgeschlossen. Schließen Sie das Fenster*". Schließen Sie das geöffneten Browserfenster. Das **SAML aktivieren** Kontrollkästchen ist nur aktiviert, wenn der Test abgeschlossen wurde.

    e. Aktivieren Sie **SAML**.
    
    f. Klicken Sie auf **Speichern**. 


9. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen. 

    ![Was ist Azure AD verbinden][15]

10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Was ist Azure AD verbinden][16]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Was ist Azure AD verbinden][100] 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Was ist Azure AD verbinden][101] 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Was ist Azure AD verbinden][102] 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Was ist Azure AD verbinden][103] 
 
    ein. Wählen Sie als **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf Weiter.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Was ist Azure AD verbinden][104] 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. In den **Namen** Txtbox, Typ, **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Was ist Azure AD verbinden][105]  

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Was ist Azure AD verbinden][106]   

    ein. Notieren Sie den Wert für das **Neue Kennwort**.
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Erstellen einen Testbenutzer Halogenlampen Software

Dieser Abschnitt soll Benutzer Britta Simon Halogenlampen Software erstellen.

**Gehen Sie zum Erstellen eines Benutzers Britta Simon Halogenlampen Software aufgerufen:**

1. Melden Sie sich auf **Halogenlampen Software** Anwendung als Administrator an.

2. Klicken Sie **Hier** , und klicken Sie auf **Benutzer erstellen**.

    ![Was ist Azure AD verbinden][300]  

3. Folgendermaßen Sie auf der Seite **Neuer Benutzer** Dialogfeld an:

    ![Was ist Azure AD verbinden][301]

    ein. Geben Sie im Textfeld **Vorname** **Britta**. 
  
    b. Geben Sie im Textfeld **Nachname** **Simon**.
  
    c. Geben Sie im Textfeld **Benutzername** **Brita Simon Benutzernamen im klassischen Azure-Portal**.
  
    d. Geben Sie im Textfeld **Kennwort** ein Kennwort für Britta.
  
    e. Klicken Sie auf **Speichern**.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll aktivieren Britta Simon mit Azure einmaliges Halogenlampen Software keinen Zugriff erteilen.

![Was ist Azure AD verbinden][200]

**Britta Simon Halogenlampen Software zuweisen, die folgenden Schritte:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Was ist Azure AD verbinden][201]

2. Wählen Sie in der Anwendungsliste **Halogenlampen Software**.

    ![Was ist Azure AD verbinden][202]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Was ist Azure AD verbinden][203]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

    ![Was ist Azure AD verbinden][204]

2. Klicken Sie auf unten auf **zuweisen**.

    ![Was ist Azure AD verbinden][205]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Anklicken der Kachel Halogenlampen Software in der Sie sollte automatisch zur Anwendung Halogenlampen Software angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png