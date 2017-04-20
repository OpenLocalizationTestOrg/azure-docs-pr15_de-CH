<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit HR | Microsoft Azure" 
    description="Erfahren Sie, wie mit Bambus HR Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Lernprogramm: Azure Active Directory-Integration mit HR

Das Ziel dieses Lernprogramms ist die Integration von Azure und BambooHR anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein BambooHR einmaliges Anmelden aktiviert Abonnement

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der BambooHR Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die BambooHR zugeordnet haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für BambooHR
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Szenario")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Die Application Integration für BambooHR

Dieser Abschnitt soll beschreiben die Anwendungsintegration für BambooHR aktivieren.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für BambooHR:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **BambooHR**.

    ![Anwendung Galerie] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **BambooHR aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer BambooHR mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **BambooHR** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Szenario] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Szenario")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei BambooHR** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **BambooHR Zeichen URL** den URL der Anwendung BambooHR Anmelden von Benutzern verwendet (z.B.: https://company.bamboohr.com), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von app-URL] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Konfigurieren von app-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am BambooHR** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens BambooHR.

6.  Auf der Homepage die folgenden Schritte:

    ![Einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Einmaliges Anmelden")

    1.  Klicken Sie auf **Apps**.
    2.  Klicken Sie im Menü apps auf der linken Seite **Einmaliges Anmelden**.
    3.  Klicken Sie auf **SAML einmaliges Anmelden**.

7.  Im Abschnitt **SAML einmaliges** Schritte:

    ![SAML einmaliges Anmelden] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML einmaliges Anmelden")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am BambooHR** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **SSO Anmelde-URL** .
    2.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP] Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    3.  Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **X. 509-Zertifikat**
    4.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Benutzer Azure AD BambooHR anmelden können, müssen sie in BambooHR bereitgestellt werden.  
Bei BambooHR ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie auf der **BambooHR** Website als Administrator an.

2.  **Klicken Sie auf der Symbolleiste im oberen Bereich.**

    ![Festlegen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Festlegen")

3.  Klicken Sie auf **Übersicht**.

4.  Gehen Sie im linken Navigationsbereich auf **Security \> Benutzer**.

5.  Geben Sie die Benutzer Namen, Kennwort und e-Mail-Adresse eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.

6.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen BambooHR Benutzer Konto Erstellungstools oder APIs von BambooHR Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu BambooHR:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **BambooHR **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
