<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit ServiceNow und ServiceNow Express | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory, ServiceNow und ServiceNow Express."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Lernprogramm: Azure Active Directory-Integration mit ServiceNow und ServiceNow Express.


In diesem Lernprogramm erfahren Sie, wie ServiceNow und ServiceNow Express in Azure Active Directory (Azure AD) integriert.

Integration von ServiceNow und ServiceNow Express in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern Zugriff auf ServiceNow und ServiceNow Express
- Sie können die Benutzer automatisch ServiceNow und ServiceNow Express (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit ServiceNow und ServiceNow Express benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Für ServiceNow, Instanz oder Mieter Calgary Version ServiceNow oder höher
- ServiceNow Express eine Instanz von ServiceNow Express Helsinki Version oder höher
- ServiceNow Mieter müssen [Mehrere Anbieter einzelne Zeichen auf Plugin](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) aktiviert. Dies erfolgt durch eine Serviceanfrage an <https://hi.service-now.com/> senden 


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. ServiceNow aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden für ServiceNow oder ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow aus der Galerie hinzufügen
Konfigurieren Sie die Integration von ServiceNow oder ServiceNow Express in Azure AD müssen Sie der Liste der verwalteten SaaS-apps ServiceNow aus dem Katalog hinzufügen. 

**Um ServiceNow aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **ServiceNow**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **ServiceNow aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Test Azure AD einmaliges Anmelden mit ServiceNow oder ServiceNow Express basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in ServiceNow an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in ServiceNow hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in ServiceNow. Konfigurieren und Testen Azure AD einmaliges Anmelden mit ServiceNow müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On für ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - der Benutzer dieses Feature verwenden können.
2. **[Konfigurieren von Azure AD Single Sign-On für ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - Benutzer diese Funktion aktivieren.
3. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer ServiceNow testen Benutzer](#creating-a-servicenow-test-user)** - Gegenstück Britta Simon ServiceNow enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
6. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

> [AZURE.NOTE] Möchten Sie ServiceNow konfigurieren überspringen Sie Schritt 2. Ebenso möchten Sie konfigurieren ServiceNow Express überspringen Sie Schritt 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfiguration von Azure AD einmaliges Anmelden für ServiceNow

1.  Klicken Sie im klassischen Azure AD-Portal auf Anwendungsseite Integration **ServiceNow** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ServiceNow** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Einmaliges Anmelden für konfigurieren")

3.  Auf der Seite **Konfigurieren App Settings** Schritte:

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Konfigurieren von app-URL")

    ein. Geben Sie im Textfeld **ServiceNow Zeichen URL** den URL für Ihre Benutzer anmelden, der nach dem Muster ServiceNow Anwendung: `https://<instance-name>.service-now.com`.

    b. Geben Sie im Textfeld **ID** den URL, die nach dem Muster ServiceNow Anwendung anmelden von Benutzern verwendet: `https://<instance-name>.service-now.com`.

    c. Klicken Sie auf **Weiter**

4.  Um Azure AD automatisch ServiceNow für SAML-Authentifizierung konfigurieren, geben Sie Ihre ServiceNow Instanznamen-Benutzername und Administratorkennwort im Formular **automatisch konfigurieren Sie einmaliges Anmelden** und auf *Konfigurieren*. Beachten Sie, dass der Benutzername Admin müssen **Security_admin** Rolle im ServiceNow für diese arbeiten. Andernfalls um ServiceNow Verwendung von Azure AD als Identitätsanbieter ein SAML manuell zu konfigurieren, klicken Sie auf **manuell konfigurieren die Anwendung für einmaliges Anmelden**, klicken Sie auf **Weiter** und gehen.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Konfigurieren von app-URL")



5.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am ServiceNow** **Download Zertifikat**speichern Zertifikatsdatei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Einmaliges Anmelden für konfigurieren")

1. Anmeldung bei ServiceNow Anwendung als Administrator.
2. Aktivieren Sie _mehrerer Anbieter Single Sign-On Installer-Integration -_ Plugin die nächsten Schritte:

    ein. Klicken Sie im Navigationsbereich auf der linken Seite **Systemdefinition** Abschnitt und dann auf **Plugins**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Plug-Ins aktivieren")
    
    b. _Integration – mehrere Anbieter anmelden Installationsprogramm_suchen.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Plug-Ins aktivieren")

    c. Wählen Sie das Plug-in. Rechts klicken und wählen **Aktivieren-Aktualisierung**.
    
    d. Klicken Sie auf die Schaltfläche **Aktivieren** .

2. Klicken Sie im Navigationsbereich auf der linken Seite klicken Sie auf **Eigenschaften**.  

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Konfigurieren von app-URL")


3. Gehen Sie im Dialogfeld **Mehrere SSO-Eigenschaften** :

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Konfigurieren von app-URL")

    ein. Als **mehrere Anbieter SSO zu aktivieren**wählen Sie **Ja**aus.

    b. **Debug-Protokollierung mehrere Anbieter SSO-Integration hat aktivieren**die Option **Ja**aus.

    c. Geben Sie im **Feld Benutzer...-Tabelle** **Benutzername**.

    d. Klicken Sie auf **Speichern**.



1. Klicken Sie im Navigationsbereich auf der linken Seite auf **X509 Zertifikate**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Einmaliges Anmelden für konfigurieren")


1. Klicken Sie im Dialogfeld **X. 509-Zertifikate** auf **neu**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Einmaliges Anmelden für konfigurieren")


1. Gehen Sie im Dialogfeld **X. 509-Zertifikate** :

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Einmaliges Anmelden für konfigurieren")

    ein. **Klicken Sie auf.**

    b. Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration (z. B.: **TestSAML2.0**).

    c. Wählen Sie **aktiv**.

    d. Wählen Sie als **Format** **PEM**.

    e. Wählen Sie als **Typ** **Speicher Zertifikat vertrauen**.
    
    f. Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in das **PEM Zertifikat** Textbox einfügen

    g. Klicken Sie auf **Aktualisieren**.


1. Klicken Sie im Navigationsbereich auf der linken Seite auf **Identitätsanbieter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Einmaliges Anmelden für konfigurieren")

1. Klicken Sie im Dialogfeld **Identitätsprovider** auf **neu**:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Einmaliges Anmelden für konfigurieren")


1. Klicken Sie im Dialogfeld **Identitätsprovider** auf **SAML2 Update1?**:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Einmaliges Anmelden für konfigurieren")


1. Gehen Sie im Dialogfeld SAML2 Update1 Eigenschaften:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Einmaliges Anmelden für konfigurieren")


    ein. Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration (z. B.: **SAML 2.0**).

    b. Im **Feld Benutzer** Textbox, **e-Mail** oder **User_id**, je nach Bereich mit Benutzern in Ihrer Bereitstellung ServiceNow eindeutig. 
    
    > [AZURE.NOTE] Kann Configue Azure AD um die Azure AD-Benutzer-ID (User principal Name) oder die e-Mail-Adresse als eindeutigen Bezeichner im SAML-Token ausgeben, indem Sie auf die **ServiceNow > Attribute > Single Sign-On** Abschnitt klassischen Azure-Portal und das gewünschte Feld Attribut **Nameidentifier** . In Azure AD (z. B. User principal Name) für das ausgewählte Attribut gespeicherte Wert muss den Wert für das Feld (z. B. User_id) in ServiceNow gespeichert

    c. Azure AD-Verwaltungsportal kopieren Sie **Identität Anbieter-ID** -Wert, und fügen Sie ihn in das Textfeld **Identität Provider-URL** .

    d. Kopieren Sie in Azure AD-Verwaltungsportal den **Authentifizierung anfordern URL** -Wert, und fügen Sie ihn in das Textfeld **der Identitätsanbieter AuthnRequest** .

    e. Azure AD-Verwaltungsportal kopieren Sie **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **der Identitätsanbieter SingleLogoutRequest** .

    f. Geben Sie im Textfeld **ServiceNow-Homepage** den URL der Startseite Instanz ServiceNow.

    > [AZURE.NOTE] ServiceNow Instanz Homepage ist eine Verkettung der **Tenant-URL ServieNow** und **/navpage.do** (z.B.:`https://fabrikam.service-now.com/navpage.do`).
 

    g. In der **Entitäts-ID / Aussteller** Textfeld Geben Sie die URL von Ihrem Mandanten ServiceNow.

    h. Geben Sie die URL von Ihrem Mandanten ServiceNow im Textfeld **URL Publikum** . 

    Ich. Geben Sie im Textfeld **Protokoll binden die IDP SingleLogoutRequest** **Urn: Oasis: Namen: Tc: SAML:2.0:bindings:HTTP-Umleitung**.

    j. Geben Sie im Textfeld NameID Richtlinie **Urn: Oasis: Namen: Tc: SAML:1.1:nameid-Format: nicht angegebener**.

    k. Deaktivieren Sie **ein AuthnContextClass erstellen**.

    l. Geben Sie im **AuthnContextClassRef-Methode** `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Dies ist nur erforderlich, wenn Sie Cloud nur Organisation. Wenn Sie lokal ADFS oder MFA für die Authentifizierung verwenden sollten Sie diesem Wert nicht konfigurieren. 

    m. Geben Sie im Textfeld **Clock Skew** **60**.

    n. Wählen Sie als **Einzelnes Zeichen Skript** **MultiSSO_SAML2_Update1**.

    o. Als **X509 Zertifikat**, wählen Sie das Zertifikat im vorherigen Schritt erstellt haben.

    p. Klicken Sie auf **Senden**. 



6. Azure AD-Verwaltungsportal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Einmaliges Anmelden für konfigurieren")

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.
 
    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Einmaliges Anmelden für konfigurieren")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfiguration von Azure AD einmaliges Anmelden für ServiceNow Express

1.  Klicken Sie im klassischen Azure AD-Portal auf Anwendungsseite Integration **ServiceNow** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ServiceNow** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Einmaliges Anmelden für konfigurieren")

3.  Auf der Seite **Konfigurieren App Settings** Schritte:

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Konfigurieren von app-URL")

    ein. Geben Sie im Textfeld **ServiceNow Zeichen URL** den URL für Ihre Benutzer anmelden, der nach dem Muster ServiceNow Anwendung: `https://<instance-name>.service-now.com`.

    b. Geben Sie im Textfeld **Aussteller URL** den URL der ServiceNow-Anwendung nach dem Muster Anmelden von Benutzern verwendet `https://<instance-name>.service-now.com`.

    c. Klicken Sie auf **Weiter**

4.  Klicken Sie auf **manuell konfigurieren die Anwendung für einmaliges Anmelden**, klicken Sie auf **Weiter** und gehen.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Konfigurieren von app-URL")

5.  Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am ServiceNow** auf **Zertifikat herunterladen**lokal auf Ihrem Computer gespeichert und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Einmaliges Anmelden für konfigurieren")

6. Anmeldung ServiceNow Express Anwendung als Administrator.

7. Klicken Sie im Navigationsbereich auf der linken Seite **Einmaliges Anmelden**.  

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Konfigurieren von app-URL")


8. Klicken Sie auf Konfiguration, oben rechts im Dialogfeld **Single Sign-On** und Eigenschaften Sie die folgenden:

    ![Konfigurieren von app-URL] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Konfigurieren von app-URL")

    ein. Umschalten Sie **können mehrere Anbieter SSO** rechts

    b. Rechts umschalten Sie **Debugprotokollierung für mehrere Anbieter SSO-Integration aktivieren** .

    c. Geben Sie im **Feld Benutzer...-Tabelle** **Benutzername**.


9. Klicken Sie im Dialogfeld **Single Sign-On** auf **Neues Zertifikat hinzufügen**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Einmaliges Anmelden für konfigurieren")



10. Gehen Sie im Dialogfeld **X. 509-Zertifikate** :

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Einmaliges Anmelden für konfigurieren")

    ein. Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration (z. B.: **TestSAML2.0**).

    b. Wählen Sie **aktiv**.

    c. Wählen Sie als **Format** **PEM**.

    d. Wählen Sie als **Typ** **Speicher Zertifikat vertrauen**.

    e. Erstellen Sie eine Base64-codierte Datei heruntergeladene Zertifikat.
   
    > [AZURE.NOTE] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Das Base64-codierte Zertifikat in Editor öffnen, den Inhalt in die Zwischenablage kopieren und in das **PEM Zertifikat** Textbox einfügen

    g. Klicken Sie auf **Aktualisieren**.


11. Klicken Sie im Dialogfeld **Single Sign-On** auf **Neue IdP hinzufügen**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Einmaliges Anmelden für konfigurieren")


12. Gehen Sie im Dialogfeld **Neue Identitätsprovider hinzufügen** unter **Identitätsanbieter konfigurieren**:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Einmaliges Anmelden für konfigurieren")


    ein. Geben Sie in das Textfeld **Name** einen Namen für die Konfiguration (z. B.: **SAML 2.0**).

    b. Azure AD-Verwaltungsportal kopieren Sie **Identität Anbieter-ID** -Wert, und fügen Sie ihn in das Textfeld **Identität Provider-URL** .

    c. Kopieren Sie in Azure AD-Verwaltungsportal den **Authentifizierung anfordern URL** -Wert, und fügen Sie ihn in das Textfeld **der Identitätsanbieter AuthnRequest** .

    d. Azure AD-Verwaltungsportal kopieren Sie **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **der Identitätsanbieter SingleLogoutRequest** .

    e. Als **Identität Anbieter Zertifikat**wählen Sie das Zertifikat aus, das Sie im vorherigen Schritt erstellt haben.


13. Klicken Sie auf **Erweiterte Einstellungen**und unter **Zusätzliche Anbieter Identitätseigenschaften**folgendermaßen:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Einmaliges Anmelden für konfigurieren")

    ein. Geben Sie im Textfeld **Protokoll binden die IDP SingleLogoutRequest** **Urn: Oasis: Namen: Tc: SAML:2.0:bindings:HTTP-Umleitung**.

    b. Geben Sie im Textfeld **NameID Richtlinie** **Urn: Oasis: Namen: Tc: SAML:1.1:nameid-Format: nicht angegebener**.    

    c. Geben Sie die **AuthnContextClassRef-Methode** **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. Deaktivieren Sie **ein AuthnContextClass erstellen**.

14. Gehen Sie unter **Zusätzliche Eigenschaften**:

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Einmaliges Anmelden für konfigurieren")

    ein. Geben Sie im Textfeld **ServiceNow-Homepage** den URL der Startseite Instanz ServiceNow.

    > [AZURE.NOTE] ServiceNow Instanz Homepage ist eine Verkettung der **Tenant-URL ServieNow** und **/navpage.do** (z.B.: `https://fabrikam.service-now.com/navpage.do`).

    b. In der **Entitäts-ID / Aussteller** Textfeld Geben Sie die URL von Ihrem Mandanten ServiceNow.

    c. Geben Sie die URL von Ihrem Mandanten ServiceNow im Textfeld **Publikum URI** . 

    d. Geben Sie im Textfeld **Clock Skew** **60**.

    e. Im **Feld Benutzer** Textbox, **e-Mail** oder **User_id**, je nach Bereich mit Benutzern in Ihrer Bereitstellung ServiceNow eindeutig.
    
    > [AZURE.NOTE] Kann Configue Azure AD um die Azure AD-Benutzer-ID (User principal Name) oder die e-Mail-Adresse als eindeutigen Bezeichner im SAML-Token ausgeben, indem Sie auf die **ServiceNow > Attribute > Single Sign-On** Abschnitt klassischen Azure-Portal und das gewünschte Feld Attribut **Nameidentifier** . In Azure AD (z. B. User principal Name) für das ausgewählte Attribut gespeicherte Wert muss den Wert für das Feld (z. B. User_id) in ServiceNow gespeichert

    f. Klicken Sie auf **Speichern**. 


15. Azure AD-Verwaltungsportal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**. 

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Einmaliges Anmelden für konfigurieren")

16. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.
 
    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Dieser Abschnitt soll aktivieren benutzerbereitstellung von Active Directory-Benutzerkonten ServiceNow beschreiben.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1. Klicken Sie im klassischen Azure Management-Portal auf der Seite **ServiceNow** Application Integration **Konfigurieren Benutzer bereitstellen**. 

    ![Benutzer bereitstellen] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Benutzer bereitstellen")


2. Auf der Seite **Geben Sie Ihre ServiceNow Anmeldeinformationen ermöglichen automatisiertes provisioning** bieten folgende Einstellungen: Benutzerbereitstellung konfigurieren 

     ein. Geben Sie im Textfeld **ServiceNow Instanzname** Instanzname ServiceNow.

     b. Geben Sie den Namen des ServiceNow Administratorkonto im Textfeld **ServiceNow Admin-Benutzernamen** .

     c. Geben Sie im Textfeld **ServiceNow Admin-Kennwort** das Kennwort für dieses Konto ein.

     d. Klicken Sie auf **Überprüfen** , um Ihre Konfiguration zu überprüfen.

     e. Klicken Sie auf **Weiter** , um die Seite **Weiter** zu öffnen.

     f. Wählen Sie alle Benutzer dieser Anwendung bereitgestellt werden soll, "**alle Benutzerkonten im Verzeichnis dieser Anwendung automatisch bereitstellen**". 

    ![Nächste Schritte] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Nächste Schritte")

     g. Klicken Sie auf **Weiter** auf **abgeschlossen** , um die Konfiguration zu speichern.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   


### <a name="creating-a-servicenow-test-user"></a>Erstellen einen Testbenutzer ServiceNow

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in ServiceNow. In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in ServiceNow. Wenn Sie nicht wissen wie zum Hinzufügen eines Benutzers in der ServiceNow oder ServiceNow Express, wenden Sie ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges ServiceNow Zugang gewährt.

![Benutzer zuweisen][200] 

**Britta Simon ServiceNow zuweisen, die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **ServiceNow**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste alle Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Anklicken der Kachel ServiceNow in der Sie sollte automatisch zur Anwendung ServiceNow angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
