<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration AirWatch | Microsoft Azure" 
    description="Erfahren Sie, wie mit AirWatch Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Lernprogramm: Azure Active Directory-Integration mit AirWatch

Das Ziel dieses Lernprogramms ist die Integration von Azure und AirWatch anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein AirWatch SSO-Abonnement aktiviert

Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der AirWatch Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Sie AirWatch zugewiesen haben.

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für AirWatch
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>Die Application Integration für AirWatch

Dieser Abschnitt soll beschreiben die Anwendungsintegration für AirWatch aktivieren.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für AirWatch:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **AirWatch**.

    ![Anwendung Galerie] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Anwendung Galerie")

7.  Klicken Sie im Ergebnisbereich wählen Sie **AirWatch aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens

Dieser Abschnitt soll Gliedern wie Benutzer AirWatch mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Als Teil dieses Vorgangs müssen Sie eine Base64-codierte Zertifikat erstellen.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [ein binäres Zertifikat in eine Textdatei konvertieren](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **AirWatch** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei AirWatch** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **AirWatch Zeichen URL** den URL der Anwendung AirWatch Anmelden von Benutzern verwendet (z.B.: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am AirWatch** auf **Zertifikat herunterladen**und dann auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in anderen Webbrowserfenster als Administrator der Website Ihres Unternehmens AirWatch.

6.  Klicken Sie im linken Navigationsbereich auf **Konten**und dann auf **Administratoren**.

    ![Administratoren] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administratoren")

7.  Erweitern Sie das Menü **Optionen** und dann auf **Verzeichnisdienste**.

    ![Standardeinstellungen] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Standardeinstellungen")

8.  Klicken Sie auf der Registerkarte **Benutzer** in das **Basis-DN** , geben Sie den Domänennamen ein und klicken Sie dann auf **Speichern**.

    ![Benutzer] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Benutzer")

9.  Klicken Sie auf die Registerkarte **Server** .

    ![Server] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. Führen Sie die folgenden Schritte aus:

    ![Hochladen] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Hochladen")

    1.  **Verzeichnis**wählen Sie **keine**aus.
    2.  Wählen **Sie SAML für die Authentifizierung**.
    3.  Klicken Sie auf **Hochladen**, um die heruntergeladene Zertifikat laden.

11. Folgendermaßen Sie im Abschnitt **Anforderung** an:

    ![Anfordern] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Anfordern")

    1.  Wählen Sie als **Bindung Anforderungstyp** **Bereitstellen**.
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Anmelden am Airwatch** den **Einzelnen Sign-On Service URL** -Wert, und fügen Sie ihn in das Textfeld **Identität Anbieter einzelne Anmelde-URL** .
    3.  **NameID Format**wählen Sie **E-Mail-Adresse aus**
    4.  Klicken Sie auf **Speichern**.

12. Klicken Sie erneut auf " **Benutzer** ".

    ![Benutzer] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Benutzer")

13. Im Abschnitt **Attribute** die folgenden Schritte:

    ![Attribut] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribut")

    1.  Geben Sie im Feld **Objekt-ID** **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  Geben Sie im Textfeld **Benutzername** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  Geben Sie im Textfeld **Angezeigter Name** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  Geben Sie im Textfeld **Vorname** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  Geben Sie im Textfeld **Nachname** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  Geben Sie in das Textfeld **E-Mail** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Klicken Sie auf **Speichern**.

14. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen

Um Azure AD-Benutzer AirWatch anmelden können, müssen sie in AirWatch bereitgestellt werden.  
Bei AirWatch ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre Site **AirWatch** Unternehmen als Administrator an.

2.  Klicken Sie im Navigationsbereich auf der linken Seite auf **Konten**und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Benutzer")

3.  Klicken Sie im Menü **Benutzer** auf **Ansicht**und dann auf **Hinzufügen \> Benutzer hinzufügen**.

    ![Benutzer hinzufügen] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Benutzer hinzufügen")

4.  Gehen Sie im Dialogfeld **hinzufügen / bearbeiten** :

    ![Benutzer hinzufügen] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Benutzer hinzufügen")

    1.  Geben Sie **Benutzername**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname**, **E-Mail-Adresse** eines gültigen Azure Active Directory-Kontos in verknüpfte Textfelder bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.

>[AZURE.NOTE] Können Sie alle anderen AirWatch Benutzer Konto Erstellungstools oder APIs von AirWatch Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern

Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>AirWatch Benutzern zuweisen, die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **AirWatch **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Ja")

So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
