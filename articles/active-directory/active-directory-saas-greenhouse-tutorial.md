<properties 
    pageTitle="Lernprogramm: Azure Active Directory Integration Treibhausgasen | Microsoft Azure" 
    description="Erfahren Sie, wie mit Treibhausgasemissionen Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Lernprogramm: Azure Active Directory-Integration mit Treibhausgasemissionen
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und Treibhausgasemissionen anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein Gewächshaus einzelnes anmelden Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung in Ihre Treibhausgasemissionen Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die Treibhausgasemissionen zugewiesen haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für Treibhausgasemissionen
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Szenario")
##<a name="enabling-the-application-integration-for-greenhouse"></a>Die Application Integration für Treibhausgasemissionen
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration Treibhausgasen aktivieren.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Treibhausgasen:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **Treibhausgasemissionen**.

    ![Anwendung Galerie] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **Treibhausgasen aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Treibhausgasemissionen] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Treibhausgasemissionen")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer Treibhausgasen mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **Treibhausgasen** **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Einmaliges Anmelden für konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer anmelden Treibhausgasen** **Microsoft Azure AD einmaliges**wählen Sie aus und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Einmaliges Anmelden für konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** im Textfeld **Anmelde-URL** den URL dem folgenden Muster "*https://company.greenhouse.io*" und klicken Sie auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden an** auf **Metadaten**und speichern Sie Metadatendatei lokal auf Ihrem Computer.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Einmaliges Anmelden für konfigurieren")

5.  Der Metadatendatei Treibhausgasen Support-Team weitergeleitet.

    >[AZURE.NOTE] Einmaliges Anmelden muss von Treibhausgasen Supportteam aktiviert werden.

6.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden für konfigurieren] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Einmaliges Anmelden für konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Azure AD Benutzer Treibhausgasen anmelden können, müssen sie in Gewächshaus bereitgestellt werden.  
Bei Gewächshaus ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Um Benutzerkonten bereitzustellen, führen Sie die folgenden Schritte:

1.  Melden Sie sich Ihre **Treibhausgasemissionen** Unternehmensstandort als Administrator an.

2.  Im Menü oben auf **Konfigurieren**, und klicken Sie auf **Benutzer**.

    ![Benutzer] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Benutzer")

3.  Klicken Sie auf **Neuer Benutzer**.

    ![Neuer Benutzer] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Neuer Benutzer")

4.  Abschnitt **Hinzufügen neuer Benutzer** folgendermaßen Sie fest:

    ![Neuen Benutzer hinzufügen] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Neuen Benutzer hinzufügen")

    1.  Geben Sie im Textfeld **geben Benutzer e-Mails** die e-Mail-Adresse ein gültiges Azure Active Directory-Konto, das Sie bereitstellen möchten.
    2.  Klicken Sie auf **Speichern**.
        
        >[AZURE.NOTE] Kontoinhaber Azure Active Directory erhalten eine e-Mail mit einem Link das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE] Alle anderen Treibhausgasen Benutzer Konto Erstellungstools verwenden oder APIs von Treibhausgasen Bereitstellung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Um Treibhausgasen Benutzer zuzuweisen, führen Sie die folgenden Schritte:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf Anwendungsseite Integration **Treibhausgasen **auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)