<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Proofpoint bei Bedarf | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Proofpoint bei Bedarf."
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
    ms.date="10/05/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Lernprogramm: Azure Active Directory-Integration mit Proofpoint bei Bedarf

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) Proofpoint bei Bedarf integrieren.

Integration von Proofpoint bei Bedarf in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, die bei Bedarf auf Proofpoint zugreifen.
- Sie können Benutzer automatisch zu Proofpoint bei Bedarf (einmaliges Anmelden oder SSO) mit ihren Azure AD-Konten angemeldet zu erhalten.
- Sie können Ihre Konten an einem zentralen Ort, klassischen Azure-Portal verwalten.

Weitere Informationen zur Integration von SaaS Anwendung in Azure AD möchten, finden Sie unter [Neuigkeiten Zugriff und einmaliges Anmelden mit Active Directory Azure?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Proofpoint bei Bedarf benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Proofpoint auf Anforderung anmelden Abonnement


Um die Schritte in diesem Lernprogramm zu testen, führen Sie diese Empfehlung:

- Verwenden Sie Ihre produktionsumgebung, wenn dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie [einen Monat testen](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Proofpoint bei Bedarf aus der Galerie hinzufügen
2. Konfigurieren Sie und Testen Sie Azure AD einmaliges Anmelden.


## <a name="add-proofpoint-on-demand-from-the-gallery"></a>Proofpoint bei Bedarf aus der Galerie hinzufügen
Konfigurieren die Integration von Proofpoint bei Bedarf in Azure AD müssen Sie der Liste der verwalteten SaaS-apps aus dem Proofpoint bei Bedarf hinzufügen.

1. Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory-Symbol][1]
2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im Menü oben auf **APPLIKATIONEN** .

    ![Menüelement APPLIKATIONEN][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Schaltfläche hinzufügen][3]

5. Klicken Sie in das Feld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Auswahl eine Anwendung aus der Galerie hinzufügen][4]

6. Geben Sie im Suchfeld **Proofpoint bei Bedarf**.

    ![Geben Sie "Proofpoint on Demand" ein Feld](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_01.png)

7. Klicken Sie im Ergebnisbereich aktivieren Sie **Proofpoint bei Bedarf**und dann auf **vollständig** die Anwendung hinzufügen.



##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren Sie und Testen Sie der Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Azure AD einmaliges Anmelden mit Proofpoint auf Nachfrage Testbenutzer namens Britta Simon testen.

Für einmaliges Anmelden zu arbeiten muss Azure AD Benutzer Gegenstück im Proofpoint bei Bedarf an einen Benutzer in Azure AD ist. Sie müssen also eine Verknüpfung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Proofpoint bei Bedarf Beziehung.

Dieser Beziehung wird hergestellt, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** im Proofpoint bei Bedarf.

Konfigurieren und Azure AD einmaliges Anmelden mit Proofpoint bei Bedarf zu testen, führen Sie die folgenden Schritte:

1. [Konfigurieren Azure AD einmaliges Anmelden](#configuring-azure-ad-single-sign-on), die Benutzer dieses Feature verwenden.
2. [Erstellen Sie einen Testbenutzer Azure AD](#creating-an-azure-ad-test-user)Azure AD einmaliges Anmelden mit Britta Simon testen.
3. [Proofpoint auf Anforderung Testbenutzer erstellen](#creating-a-proofpoint-ondemand-test-user), damit ein Gegenstück Britta Simon Proofpoint bei Bedarf, die in Azure AD Darstellung Ihres verknüpft ist.
4. [Weisen Sie Testbenutzer Azure AD](#assigning-the-azure-ad-test-user)Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. [Test einmaliges Anmelden](#testing-single-sign-on), um sicherzustellen, dass die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD einmaliges Anmelden konfigurieren

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in Ihrem Proofpoint on Demand-Anwendung konfigurieren.

1. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Proofpoint bei Bedarf** **Konfigurieren einmaliges** öffnen im Dialogfeld **Konfigurieren Sie einmaliges Anmelden** .

    ![Schaltfläche "Einmaliges Anmelden konfigurieren"][6]

2. Wählen Sie auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Proofpoint bei Bedarf** **Microsoft Azure AD einmaliges Anmelden**und klicken Sie auf **Weiter**.

    ![Optionsfeld "Microsoft Azure AD einmaliges Anmelden"](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_03.png)

3. Auf der Seite **Konfigurieren App Settings** Schritte:

    ![Seite "Einstellungen App" Felder ausgefüllt](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_04.png)

    ein. Geben Sie im Feld **Zeichen auf URL** den URL, in dem Benutzer Ihre Proofpoint bei Bedarf Anwendung anmelden. Verwenden Sie das folgende Muster: **https://\<Hostnamen\>.pphosted.com/ppssamlsp_hostname**

    b. Geben Sie die URL im **BEZEICHNER** mit dem folgenden Muster: **https://\<Hostname / >.pphosted.com/ppssamlsp**

    c. Geben Sie im Feld **Antwort-URL** den URL dem folgenden Muster: **https://\<Hostname / >.pphosted.com:portnumber/v1/samlauth/samlconsumer**

    d. Klicken Sie auf **Weiter**.

4. Auf der Seite **Konfigurieren einmaliges Anmelden am Proofpoint bei Bedarf** die folgenden Schritte:

    ![Seite "Herunterladen des Zertifikats" Schaltfläche "Einmaliges Anmelden am Proofpoint bei Bedarf konfigurieren"](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_05.png)

    ein. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

5. SSO für die Anwendung konfigurierten, Proofpoint bei Bedarf Support-Team kontaktieren und bieten Folgendes:

    • Das heruntergeladene Zertifikat

    • Der Element-ID

    • SAML SSO-URL

6. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.

    ![Kontrollkästchen, das bestätigt, dass einmaliges Anmelden konfiguriert haben][10]

7. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  

    ![Bestätigungsseite][11]


### <a name="create-an-azure-ad-test-user"></a>Erstellen Sie einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer Britta Simon im klassischen Portal Namens.


![Der Test-Benutzer in der Benutzerliste][20]

1. Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory-Symbol](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_09.png)

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Um die Liste der Benutzer im Menü oben anzuzeigen, klicken Sie auf **Benutzer**.

    ![Menüelement für Benutzer](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png)

4. Klicken Sie zum Öffnen in der Symbolleiste am unteren Rand des Dialogfelds **Benutzer hinzufügen** auf **Benutzer hinzufügen**.

    ![Schaltfläche "Benutzer hinzufügen"](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png)

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  !["Erzählen Benutzer" Seite Felder ausgefüllt](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_05.png)

    ein. Wählen Sie im Feld **Typ des Benutzers** **neue Benutzer in Ihrer Organisation**.

    b. Geben Sie im Feld **Benutzername** **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf der Seite **Benutzerprofil** folgendermaßen: ![Seite "Benutzerprofil" Felder ausgefüllt](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_06.png)

    ein. Geben Sie im Feld **Vorname** **Britta**.  

    b. Geben Sie im Feld **Nachname** **Simon**.

    c. Geben Sie im Feld **ANZEIGENAME** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Das **temporäres Kennwort abrufen** klicken Sie auf **Erstellen**.

    ![Schaltfläche zum Erstellen eines temporären Kennworts](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_07.png)

8. Führen Sie die folgenden Schritte auf **Passwort zu erhalten** :

    ![Seite "Passwort abrufen" Kennwörter](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_08.png)

    ein. Notieren Sie sich den Wert in das Feld **Neues Kennwort** .

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="create-a-proofpoint-on-demand-test-user"></a>Ein Proofpoint auf Anforderung Testbenutzer erstellen

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Proofpoint bei Bedarf. Arbeiten Sie mit Proofpoint, bei Bedarf Unterstützung Team Benutzer Proofpoint auf Anforderung Plattform hinzufügen.


### <a name="assign-the-azure-ad-test-user"></a>Azure AD-Testbenutzer zuweisen

In diesem Abschnitt können Sie Britta Simon mit Azure SSO-Proofpoint bei Bedarf Zugang gewähren.

![Benutzerinformationen mit Zugriff über direkte Methode][200]

1. Im klassischen Portal in der Verzeichnisansicht **Anwendung** dem Menü oben in der Anwendungsansicht geöffnet.

    ![Menüelement APPLIKATIONEN][201]

2. Wählen Sie in der Liste der Programme **Proofpoint bei Bedarf**.

    ![Liste mit Proofpoint bei Bedarf aktiviert](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_50.png)

3. **Klicken Sie auf das Menü oben.**

    ![Menüelement für Benutzer][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**.

5. Klicken Sie auf der Symbolleiste am unteren **zuweisen**.

    ![Taste zuweisen][205]


### <a name="test-single-sign-on"></a>Testen Sie einmaliges Anmelden

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mithilfe der.

Beim Anklicken der Kachel **Proofpoint bei Bedarf** der sollten Sie automatisch Ihre Proofpoint bei Bedarf Anwendung unterzeichnet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_205.png
