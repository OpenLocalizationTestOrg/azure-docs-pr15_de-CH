<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Thoughtworks mischen | Microsoft Azure" 
    description="Erfahren Sie, wie mit Thoughtworks mischen Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Lernprogramm: Azure Active Directory-Integration mit Thoughtworks mischen
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Thoughtworks mischen anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Mieter Thoughtworks mischen
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Thoughtworks mischen
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Szenario")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Die Application Integration für Thoughtworks mischen
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für Thoughtworks mischen aktivieren.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Thoughtworks Mischen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Thoughtworks mischen**.

    ![Anwendung Galerie] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Thoughtworks mischen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![ThoughtWorks mischen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "ThoughtWorks mischen")

##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Thoughtworks mischen mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Im Rahmen dieses Verfahrens müssen Sie ein Zertifikat um Thoughtworks hochladen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der **Thoughtworks mischen **Application Integrationsseite auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden zu Thoughtworks mischen** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Thoughtworks mischen Mieter URL** den URL dem folgenden Muster "*http://company.mingle.thoughtworks.com*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am Thoughtworks statt** auf Download Metadaten und auf dem Computer speichern.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Einmaliges Anmelden für konfigurieren")

5.  Melden Sie sich Ihr Unternehmensstandort **Thoughtworks mischen** als Administrator an.

6.  Klicken Sie auf die Registerkarte " **Admin** " und klicken Sie **SSO-Config**.

    ![SSO-Konfiguration] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO-Konfiguration")

7.  **SSO** -Konfigurationsabschnitt folgendermaßen Sie fest:

    ![SSO-Konfiguration] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO-Konfiguration")

    1.  Klicken Sie zum Hochladen der Metadatendatei auf **Datei auswählen**.
    2.  Klicken Sie auf **Speichern**.

8.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Einmaliges Anmelden für konfigurieren")

##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
AAD Benutzer anmelden müssen sie die Thoughtworks mischen Anwendung ihre Benutzernamen Azure Active Directory bereitgestellt werden.  
Bei Thoughtworks mischen ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Melden Sie sich Ihr Unternehmensstandort Thoughtworks mischen als Administrator an.

2.  Klicken Sie auf **Profil**.

    ![Das erste Projekt] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Das erste Projekt")

3.  Klicken Sie auf die Registerkarte " **Admin** ", und klicken Sie dann auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Benutzer")

4.  Klicken Sie auf **Neuer Benutzer**.

    ![Neuer Benutzer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Neuer Benutzer")

5.  Folgendermaßen Sie auf der Seite **Neuer Benutzer** Dialogfeld an:

    ![Neuer Benutzer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Neuer Benutzer")

    1.  Geben Sie **Benutzername**, **Anzeigename**, **Wählen Sie Kennwort**, **Kennwort bestätigen** eines gültigen Kontos AAD in verknüpfte Textfelder bereitstellen möchten.
    2.  Wählen Sie als **Benutzertyp** **Benutzer**.
    3.  Klicken Sie auf **dieses Profil**.

>[AZURE.NOTE] Können Sie alle anderen Thoughtworks mischen Benutzer Konto Erstellungstools oder APIs von Thoughtworks statt Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu Thoughtworks Mischen:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Die **Thoughtworks mischen** Application Integration klicken Sie auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)
