<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Facebook am Arbeitsplatz | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Facebook am Arbeitsplatz."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Lernprogramm: Azure Active Directory-Integration mit Facebook am Arbeitsplatz

Das Ziel dieses Lernprogramms ist, wie Facebook arbeitet in Azure Active Directory (Azure AD) integriert werden.

Integration von Facebook bei der Arbeit mit Azure bietet die folgenden Vorteile: 

- Sie können in Azure AD steuern am Arbeitsplatz hat Zugriff auf Facebook 
- Sie können automatisch Konto für Benutzer bereitstellen, die Facebook am Arbeitsplatz Zugriff wurde
- Sie können die Benutzer automatisch auf Facebook am Arbeitsplatz (einmaliges Anmelden) mit ihren Azure AD angemeldete abrufen
- Sie können Ihre Konten an einem zentralen Ort verwalten. 

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Erforderliche Komponenten 

Zum Konfigurieren von Azure AD-Integration mit CS benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Facebook mit einmaliges Anmelden aktiviert

Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen. 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Hinzufügen von Facebook aus der Galerie
Zum Konfigurieren der Integration von Facebook am Arbeitsplatz in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Facebook am Arbeitsplatz aus der Galerie hinzufügen.

**Um Facebook am Arbeitsplatz aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .
    
    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Facebook am Arbeitsplatz**.

7. Wählen Sie im Ergebnisbereich **Facebook am Arbeitsplatz aus**und dann auf **vollständig** die Anwendung hinzufügen.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Dieser Abschnitt beschreibt, wie Benutzer Facebook bei der Arbeit mit ihrem Konto in Azure Active Directory Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

**Um Azure AD einmaliges Anmelden mit Facebook am Arbeitsplatz konfigurieren, führen Sie die folgenden Schritte:**

1.  Klicken Sie nach Facebook am Arbeitsplatz im klassischen Azure-Portal hinzufügen auf **Konfigurieren Sie einmaliges Anmelden**.

2.  Geben Sie den URL Bildschirm **App-URL konfigurieren** , werden Benutzer Facebook Arbeit Anwendung anmelden. Dies ist Facebook Arbeit Mieter URL (Beispiel: https://example.facebook.com/). Wenn abgeschlossen, klicken Sie auf **Weiter**.

3.  In anderen Webbrowserfenster die bei Facebook melden Sie am Standort Arbeit als Administrator an.

4. Gehen Sie unter der folgenden URL im Büro Azure AD als Identitätsanbieter Facebook konfigurieren: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Abgeschlossen, zurück zum Browser Windows Azure-Verwaltungsportal angezeigt, das Kontrollkästchen, um zu bestätigen, dass Sie den Vorgang abgeschlossen haben, klicken Sie **Weiter** und **abgeschlossen**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Bereitstellung automatisch Benutzer Facebook am Arbeitsplatz

Azure AD unterstützt die Möglichkeit, automatisch synchronisieren an zugewiesenen Benutzer Facebook am Arbeitsplatz. Diese automatische Synchronisierung ermöglicht Facebook am Arbeitsplatz zum Abrufen der Daten Benutzer vor ihnen die Anmeldung zum ersten Mal versucht auf Autorisieren muss. Sie Vorschriften heben auch Benutzer von Facebook am Arbeitsplatz beim Zugriff in Azure AD gesperrt wurde.

Automatische Bereitstellung kann eingerichtet werden durch Klicken auf in Azure klassischen Portal Fenster **Bereitstellung konfigurieren** .

Weitere Informationen zum Konfigurieren der automatischen Bereitstellung finden Sie unter [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Zuweisen von Benutzern zu Facebook am Arbeitsplatz

Bereitgestellte AAD Benutzer Facebook am Arbeitsplatz auf die Abdeckung zu sehen müssen sie Zugang klassischen Azure-Portal zugeordnet.

**Facebook arbeitet Benutzer zuweisen:**

1.  Klicken Sie auf der Startseite für Facebook am Arbeitsplatz im klassischen Azure-Portal auf **Konten zuweisen**.

2.  Wählen Sie im Menü **Anzeigen** möchten Facebook am Arbeitsplatz einen Benutzer oder eine Gruppe zuweisen, und klicken Sie auf das Kontrollkästchen.

3.  Wählen Sie in der Liste der Benutzer oder Gruppen, denen Sie Facebook am Arbeitsplatz zuweisen möchten.

4.  Klicken Sie in der Fußzeile **zuweisen** .


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




