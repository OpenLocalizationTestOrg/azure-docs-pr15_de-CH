<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Rallye-Software | Microsoft Azure" 
    description="Erfahren Sie, wie mit Rallye-Software mit Active Directory Azure-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Lernprogramm: Azure Active Directory Integration Rallye-Software
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Rallye-Software anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Rallye-Software
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Rallye-Software
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Szenario")
##<a name="enabling-the-application-integration-for-rally-software"></a>Die Application Integration für Rallye-Software
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Rallye-Software aktivieren.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Rallye-Software:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Rallye**.

    ![Anwendung Galerie] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Rallye Software**, und klicken Sie auf **vollständig** die Anwendung hinzufügen.

    ![Rallye-Software] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rallye-Software")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Software Rallye Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Zertifikat für Rallye-Software hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf Anwendungsseite Integration **Rallye Software **auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Benutzer anmelden Rallye** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Microsoft Azure AD einmaliges Anmelden] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD einmaliges Anmelden")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Rallye Software Mieter URL** den URL dem folgenden Muster "*https://\<Name Mieters\>. rally.com*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden bei** auf Metadaten herunterladen und auf Ihrem Computer speichern.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie sich bei Ihrem Mandanten **Rallye-Software** .

6.  Klicken Sie in der oberen Symbolleiste auf **Setup**, und wählen Sie **Abonnements**.

    ![Abonnement] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Abonnement")

7.  Klicken Sie auf die **interaktive** Schaltfläche auf der Symbolleiste oben rechts und wählen Sie dann die **Anmeldung bearbeiten**.

8.  Führen Sie auf das **Abonnement** die folgenden Schritte aus, und klicken Sie auf **Speichern und schließen**:

    ![Authentifizierung] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Authentifizierung")

    1.  Authentifizierung Dropdown **Rallye oder SSO-Authentifizierung** auswählen
    2.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Rallye Software** **Identity-Provider-ID** -Wert und fügen Sie ihn in das Textfeld **Identität Provider-URL**
    3.  Kopieren Sie im klassischen Azure-Portal, auf der Seite **Konfigurieren einmaliges Rallye Software** **Remote Logout URL** -Wert.

9.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
AAD Benutzer anmelden müssen sie ihre Azure Active Directory-Benutzernamen mit Software Rallye-Anwendung bereitgestellt werden.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich bei Ihrem Mandanten Rallye-Software.

2.  Zur **Installation \> Benutzer**, und klicken Sie dann auf **+ Neu hinzufügen**.

    ![Benutzer] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Benutzer")

3.  Geben Sie den Namen in das Textfeld neuen Benutzer und dann auf **Details hinzufügen**.

4.  Folgendermaßen Sie im Abschnitt **Erstellen Benutzer** an:

    ![Benutzer erstellen] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Benutzer erstellen")

    1.  Geben Sie im Textfeld **Benutzername** den Namen des Azure AD-Benutzer, den Sie bereitstellen möchten.
    2.  Geben Sie in das Textfeld **E-Mail-Adresse** die e-Mail-Adresse des Benutzers Azure AD bereitstellen möchten.
    3.  Klicken Sie auf **Speichern und schließen**.

>[AZURE.NOTE]Können Sie alle anderen Rallye Benutzer Konto erstellen Softwaretools oder APIs von Software Rallye Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Rallye-Software:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **Rallye Software** Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)




