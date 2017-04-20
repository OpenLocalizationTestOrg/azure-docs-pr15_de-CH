<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit ClickTime | Microsoft Azure" 
    description="Erfahren Sie, wie mit ClickTime in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Lernprogramm: Azure Active Directory-Integration mit ClickTime

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) ClickTime integrieren.

Azure AD ClickTime Integration bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer auf ClickTime Zugriff
- Sie können die Benutzer automatisch angemeldet-ClickTime (einmaliges Anmelden) mit ihren Azure AD-Konten auf Abrufen
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit ClickTime benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ClickTime Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. ClickTime aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden

##<a name="adding-clicktime-from-the-gallery"></a>ClickTime aus der Galerie hinzufügen

Dieser Abschnitt soll beschreiben die Anwendungsintegration für ClickTime aktivieren.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ClickTime:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **ClickTime**.

    ![Anwendung Galerie] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **ClickTime aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit ClickTime basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in ClickTime an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in ClickTime hergestellt werden.

Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in ClickTime.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit ClickTime müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer ClickTime testen Benutzer](#creating-a-clicktime-test-user)** - Gegenstück Britta Simon ClickTime enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Dieser Abschnitt soll Gliedern wie Benutzer ClickTime mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  


>[AZURE.IMPORTANT] Um einmaliges Anmelden auf Ihrem Mandanten ClickTime konfigurieren können, müssen Sie zuerst ClickTime technischen Support dieser Funktion zu erhalten.

**Konfigurieren Azure AD einmaliges Anmelden mit ClickTime die folgenden Schritte:**

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **ClickTime** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden aktivieren] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Einmaliges Anmelden aktivieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei ClickTime** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Einmaliges Anmelden für konfigurieren")

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    ein. Geben Sie im Textfeld **IdentifierL** die URL dem folgenden Muster: **https://app.clicktime.com/sp/**
    
    b. Geben Sie im Feld **Antwort-URL** den URL dem folgenden Muster: **https://app.clicktime.com/Login/**

    c. Klicken Sie auf **Weiter**

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am ClickTime** das Zertifikat auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Einmaliges Anmelden für konfigurieren")

4.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens ClickTime.

5.  Klicken Sie in der oberen Symbolleiste auf **Voreinstellungen**und dann auf **Sicherheit**.

6.  Im Konfigurationsabschnitt **Voreinstellungen für einzelne Zeichen** folgendermaßen:

    ![Sicherheit] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Sicherheit")

    ein.  Wählen Sie **Zulassen** aus anmelden **Azure AD**mit Single Sign-On (SSO).
    
    b.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am ClickTime** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieterendpunkt** .

    c.  Öffnen Sie Base64-codierte Zertifikat in **Editor**, kopieren Sie den Inhalt und fügen Sie ihn in das Textfeld **X. 509-Zertifikat** .
    
    d.  Klicken Sie auf **Speichern**.

7.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer ClickTime anmelden können, müssen sie in ClickTime bereitgestellt werden.  
Bei ClickTime ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich bei Ihrem Mandanten **ClickTime** .

2.  **Klicken Sie in der oberen Symbolleiste**und klicken Sie dann auf **Benutzer**.

    ![Personen] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Personen")

3.  Klicken Sie auf **Person hinzufügen**.

    ![Person hinzufügen] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Person hinzufügen")

4.  Folgendermaßen Sie im Abschnitt neue Person an:

    ![Personen] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Personen")

    ein.  Geben Sie in das Textfeld **e-Mail-Adresse** die e-Mail-Adresse Ihres Kontos Azure AD.
    
    b.  Geben Sie im Feld **Vollständiger Name** den Namen des Azure AD-Konto.  

    >[AZURE.NOTE] Wenn Sie möchten, können Sie zusätzliche Eigenschaften das Person-Objekt festlegen.

    c.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen ClickTime Benutzer Konto Erstellungstools oder APIs von ClickTime Bereitstellung Azure AD-Benutzerkonten.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges ClickTime Zugang gewährt.

![Benutzer zuweisen][200]

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

**Um Britta Simon ClickTime zuzuweisen, gehen Sie folgendermaßen vor**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **ClickTime**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]

## <a name="testing-single-sign-on"></a>Einmaliges testen
In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Klicken auf die ClickTime Fläche in der Sie sollte automatisch zur Anwendung ClickTime angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png