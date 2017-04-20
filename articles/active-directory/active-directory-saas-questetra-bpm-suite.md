<properties
    pageTitle="Lernprogramm: Azure Active Directory Integration BPM-Suite Questetra | Microsoft Aure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Questetra BPM-Suite."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Lernprogramm: Azure Active Directory Integration Questetra BPM-Suite

Dieses Lernprogramm soll zeigen, wie Sie Azure Active Directory (Azure AD) Questetra BPM-Suite integriert.  
Integrieren von Azure AD Questetra BPM-Suite bietet die folgenden Vorteile: 

- Sie können in Azure AD steuern Zugriff auf Questetra BPM-Suite 
- Sie können die Benutzer automatisch Questetra BPM-Suite (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten 

Konfiguration von Azure AD-Integration mit Questetra BPM-Suite benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein [Questetra BPM-Suite](https://senbon-imadegawa-988.questetra.net/) Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Das Ziel dieses Lernprogramms ist Azure AD einmaliges Anmelden in einer Umgebung testen können.  
In diesem Lernprogramm beschriebenen Szenario besteht aus drei wesentlichen Bausteine:

1. Questetra BPM-Suite aus der Galerie hinzufügen 
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM-Suite aus der Galerie hinzufügen
Konfigurieren Sie die Integration der BPM-Suite Questetra in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Questetra BPM-Suite aus dem Katalog hinzufügen.

**Um Questetra BPM-Suite aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Questetra BPM-Suite**.

    ![Applikationen][5]

7. Wählen Sie im Ergebnisbereich **BPM-Suite Questetra aus**und dann auf **vollständig** die Anwendung hinzufügen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
Dieser Abschnitt soll wie Sie konfigurieren und Testen Azure AD einmaliges Questetra BPM-Suite basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden funktioniert wissen, was der Benutzer Gegenstück in Questetra BPM-Suite einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und dem entsprechenden Benutzer in Questetra BPM-Suite hergestellt werden.  
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** Questetra BPM-Suite.
 
Konfigurieren und Azure AD einmaliges Questetra BPM-Suite testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Questetra BPM-Suite testen Benutzer](#creating-a-questetra-bpm-suite-test-user)** - Gegenstück Britta Simon Questetra BPM-Suite enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und konfigurieren einmaliges Anmelden in der Anwendung Questetra BPM-Suite.

**Um Azure AD einmaliges Anmelden mit Questetra BPM-Suite zu konfigurieren, führen Sie die folgenden Schritte:**

1. Klicken Sie im klassischen Azure-Portal, auf der Seite **Questetra BPM-Suite** Application Integration **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][8]

2. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Questetra BPM-Suite** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Azure AD einmaliges Anmelden][9]


3. Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens **Questetra BPM-Suite** .

4. Klicken Sie im Menü oben auf **Systemeinstellungen**. 

    ![Azure AD einmaliges Anmelden][10]

5. Klicken Sie zum Öffnen der Seite **SingleSignOnSAML** auf **SSO (SAML)**. 

    ![Azure AD einmaliges Anmelden][11]


6. Der Azure-Verwaltungsportal auf der **App-Einstellungen konfigurieren** , führen Sie die folgenden Schritte aus: 

    ![Anwendung konfigurieren][13]
 
    ein. Auf Sie **Questetra BPM-Suite** Standort, Abschnitt SP **ACS URL**kopieren Sie und fügen Sie ihn in das Textfeld **Anmelde-URL** .

    b. Auf Sie **Questetra BPM-Suite** Standort, Abschnitt SP **Entitäts-ID**kopieren Sie und fügen Sie ihn in das Textfeld **URL Aussteller** .

    c. Auf Sie **Questetra BPM-Suite** Standort, Abschnitt SP **ACS URL**kopieren Sie und fügen Sie ihn in das Textfeld **Antwort-URL** .

    d. Klicken Sie auf **Weiter**.

 
7. Auf der Seite **Konfigurieren einmaliges Questetra BPM-Suite** auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren][14]


8. Sie **Questetra BPM-Suite** Unternehmensstandort führen Sie die folgenden Schritte: 

    ![Einmaliges Anmelden konfigurieren][15]

    ein. Aktivieren Sie **Einmaliges Anmelden**.
     
    b. Der Azure-Verwaltungsportal kopieren Sie den **Aussteller URL** -Wert, und fügen Sie ihn in das Textfeld **Entitäts-ID** .

    c. Der Azure-Verwaltungsportal kopieren Sie **Einzelne Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Anmeldung Seiten-URL** .

    d. Der Azure-Verwaltungsportal kopieren Sie den Wert der **Einzelnen Abmelde Service URL** , und fügen Sie ihn in das Textfeld **URL der Abmeldeseite** .

    e. Geben Sie im Textfeld **NameID Format** **Urn: Oasis: Namen: Tc: SAML:1.1:nameid-Format: EmailAddress**.


    f. Erstellen Sie eine Base64-codierte Datei heruntergeladene Zertifikat. 

    >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

    g. Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und dann das **Zertifikat Überprüfung** Textfeld einfügen 

    h. Klicken Sie auf **Speichern**.


9. Klassischen Azure-Portal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Was ist Azure AD verbinden][17]


10. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Was ist Azure AD verbinden][18]




### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
Dieser Abschnitt soll im klassischen Azure-Portal namens Britta Simon Testbenutzer erstellen.

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Azure AD Testbenutzer erstellen][100] 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Azure AD Testbenutzer erstellen][101] 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**. 

    ![Azure AD Testbenutzer erstellen][102] 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:

    ![Azure AD Testbenutzer erstellen][103]
 
    ein. Wählen Sie als **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.
  
    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf Weiter.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus: 

    ![Azure AD Testbenutzer erstellen][104] 
  
    ein. Geben Sie im Textfeld **Vorname** **Britta**. 
 
    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Azure AD Testbenutzer erstellen][105]  

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Azure AD Testbenutzer erstellen][106]   
  
    ein. Notieren Sie den Wert für das **Neue Kennwort**.
  
    b. Klicken Sie auf **abgeschlossen**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Erstellen einen Testbenutzer Questetra BPM-Suite

Dieser Abschnitt soll Benutzer Britta Simon Questetra BPM-Suite erstellen.

**Gehen Sie zum Erstellen eines Benutzers bezeichnet Britta Simon Questetra BPM-Suite:**

1.  Anmeldung bei der Website Ihres Unternehmens Questetra BPM-Suite als Administrator.
2.  Gehen Sie zu **Systemeinstellungen > Benutzerliste > neuer Benutzer**. 
3.  Gehen Sie im Dialogfeld Neuer Benutzer: 

    ![Test erstellen][300] 

    ein. Geben Sie in das Textfeld **Name** Brittas Benutzernamen in Azure AD.

    b. Geben Sie in das Textfeld **E-Mail** Brittas Benutzernamen in Azure AD.

    c. Geben Sie im Textfeld **Kennwort** ein Kennwort ein.

4.  Klicken Sie auf **neuen Benutzer hinzufügen**.



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

Dieser Abschnitt soll Britta Simon Questetra BPM-Suite Zugang gewähren Azure einmaliges Anmelden verwenden aktivieren.

![Was ist Azure AD verbinden][200]

**Gehen Sie zum Zuweisen von Britta Simon Questetra BPM-Suite:**

1. Der Azure-Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Was ist Azure AD verbinden][201]

2. Wählen Sie in der Anwendungsliste **Questetra BPM-Suite**.

    ![Was ist Azure AD verbinden][205]

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Was ist Azure AD verbinden][202]

1. Wählen Sie in der Liste Benutzer **Britta Simon**.

    ![Was ist Azure AD verbinden][203]

2. Klicken Sie auf unten auf **zuweisen**.

    ![Was ist Azure AD verbinden][204]



### <a name="testing-single-sign-on"></a>Testen von Single Sign-On

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.  
Beim Anklicken der Kachel Questetra BPM-Suite in der Sie sollte automatisch zur Anwendung Questetra BPM-Suite angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 