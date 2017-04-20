<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit AnswerHub | Microsoft Azure" 
    description="Erfahren Sie, wie mit AnswerHub in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Lernprogramm: Azure Active Directory-Integration mit AnswerHub

Das Ziel dieses Lernprogramms ist die Integration von Azure und [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software)anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) SSO-Abonnement aktiviert

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der AnswerHub Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die AnswerHub zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für AnswerHub
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Szenario")

##<a name="enabling-the-application-integration-for-answerhub"></a>Die Application Integration für AnswerHub

Dieser Abschnitt soll beschreiben die Anwendungsintegration für AnswerHub aktivieren.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für AnswerHub:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **AnswerHub**.

    ![Anwendung Galerie] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **AnswerHub aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer AnswerHub mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **AnswerHub** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei AnswerHub** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App URL** in das Textfeld **AnswerHub Zeichen In URL** den URL dem folgenden Muster "*https://company.answerhub.com*" und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Konfigurieren von App-URL")

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am AnswerHub** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens AnswerHub.
    >[AZURE.NOTE] Benötigen Sie Informationen zur Konfiguration von AnswerHub, wenden Sie [die AnswerHub](mailto:success@answerhub.com. ).








6.  Wechseln Sie zu **Administration**.

7.  Klicken Sie auf die Registerkarte **Benutzer und Gruppe** .

8.  Klicken Sie im Navigationsbereich auf der linken Seite in **Sozialen** Einstellungen **SAML einrichten**.

9.  Klicken Sie **IDP Config** .

10. Auf der Registerkarte **IDP Config** Schritte:

    ![SAML-Setup] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "SAML-Setup")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am AnswerHub** Wert **Remote Login-URL** und fügen Sie ihn in das Textfeld **IDP Anmelde-URL** .
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am AnswerHub** Wert **Remote Logout-URL** und fügen Sie ihn in das Textfeld **IDP Logout URL** .
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am AnswerHub** den Wert **Name Bezeichner** , und fügen Sie ihn in das Textfeld **IDP Bezeichner Namensformat** .
    4.  Klicken Sie auf **Schlüssel und Zertifikate**.

11. Auf der Registerkarte Schlüssel und Zertifikate die folgenden Schritte:

    ![Schlüssel und Zertifikate] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Schlüssel und Zertifikate")

    1.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    2.  Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **IDP öffentlichen Schlüssel (x 509-Format)** .
    3.  Klicken Sie auf **Speichern**.

12. Klicken Sie auf der Registerkarte **IDP-Konfiguration** auf **Speichern**.

13. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Benutzer Azure AD AnswerHub anmelden können, müssen sie in AnswerHub bereitgestellt werden.  
Bei AnswerHub ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihre Site **AnswerHub** Unternehmen als Administrator an.

2.  Wechseln Sie zu **Administration**.

3.  Klicken Sie auf der Registerkarte **Benutzer und Gruppen** .

4.  Klicken Sie im Navigationsbereich auf der linken Seite im Abschnitt **Benutzer verwalten** auf **Benutzer erstellen oder importieren**.

    ![Benutzer und Gruppen] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Benutzer und Gruppen")

5.  Geben Sie **e-Mail-Adresse**, **Benutzername** und **Kennwort** eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten, und klicken Sie dann auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen AnswerHub Benutzer Konto Erstellungstools oder APIs von AnswerHub Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu AnswerHub:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **AnswerHub **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
