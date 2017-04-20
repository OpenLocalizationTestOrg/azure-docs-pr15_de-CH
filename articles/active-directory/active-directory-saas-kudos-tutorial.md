<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit LOB | Microsoft Azure" 
    description="Erfahren Sie, wie mit LOB Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Lernprogramm: Azure Active Directory-Integration mit LOB
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Kudos anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Kudos Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung die Kudos Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzern, Kudos zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für LOB
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Szenario")
##<a name="enabling-the-application-integration-for-kudos"></a>Die Application Integration für LOB
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für LOB aktivieren.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration ansehen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-kudos-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **ansehen**.

    ![Anwendung Galerie] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Kudos aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Ansehen] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Ansehen")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Kudos Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Integration **LOB** -Anwendung auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Kudos** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL Kudos** den URL dem folgenden Muster "*https://company.kudosnow.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-kudos-tutorial/IC787804.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Kudos** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens Kudos.

6.  Klicken Sie im Menü oben auf **Settings**.

    ![Standardeinstellungen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Standardeinstellungen")

7.  Klicken Sie auf **Integration \> SSO**.

8.  Im Abschnitt **SSO** Schritte:

    ![SSO] (./media/active-directory-saas-kudos-tutorial/IC787807.png "SSO")

    1.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Kudos** **Einzelne Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **URL anmelden** .
    2.  Erstellen Sie eine **Base64 - codierte** Datei heruntergeladene Zertifikat.  

        >[AZURE.TIP]
        Weitere Informationen finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o)

    3.  Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt in die Zwischenablage und fügen Sie ihn in das Textfeld **x. 509-Zertifikat**
    4.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Kudos** **Einzelne Abmelde Service URL** -Wert, und fügen Sie ihn in das Textfeld **URL Abmelden** .
    5.  Geben Sie im Textfeld **Ihre Kudos-URL** den Firmennamen.
    6.  Klicken Sie auf **Speichern**.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD-Benutzer Kudos anmelden können, müssen sie in Kudos bereitgestellt werden.  
Bei LOB ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Sich der **Kudos** -Unternehmenswebsite als Administrator an.

2.  Klicken Sie im Menü oben auf **Settings**.

    ![Standardeinstellungen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Standardeinstellungen")

3.  Klicken Sie auf **Benutzer Admin**.

4.  Klicken Sie auf die Registerkarte **Benutzer** , und klicken Sie dann auf **Benutzer hinzufügen**.

    ![Benutzer Admin] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Benutzer Admin")

5.  Führen Sie im Abschnitt **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Hinzufügen eines Benutzers")

    1.  Geben Sie den **Vornamen**, **Nachnamen**, **E-Mail** und andere Details eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Benutzer erstellen**.

>[AZURE.NOTE]Alle anderen Kudos Benutzer Konto Erstellungstools verwenden oder APIs von Kudos Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Um Kudos Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Kudos **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)