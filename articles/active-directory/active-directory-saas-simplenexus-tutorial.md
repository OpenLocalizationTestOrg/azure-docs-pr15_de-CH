<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit SimpleNexus | Microsoft Azure" 
    description="Erfahren Sie, wie mit SimpleNexus in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Lernprogramm: Azure Active Directory-Integration mit SimpleNexus
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und SimpleNexus anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   Ein SimpleNexus einmaliges Anmelden aktiviert Abonnement
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der SimpleNexus Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die SimpleNexus zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für SimpleNexus
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Szenario")
##<a name="enabling-the-application-integration-for-simplenexus"></a>Die Application Integration für SimpleNexus
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für SimpleNexus aktivieren.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SimpleNexus:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **einfach Nexus**.

    ![Anwendung Galerie] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **SimpleNexus aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Einfache Nexus] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Einfache Nexus")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer SimpleNexus mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **SimpleNexus** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei SimpleNexus** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren App-URL** in das Textfeld **SimpleNexus Zeichen In URL** den URL dem folgenden Muster "*https://simplenexus.com/CompanyName\_Anmeldung*", und klicken Sie dann auf **Weiter**.

    ![Konfigurieren von App-URL] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Konfigurieren von App-URL")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am SimpleNexus** auf **Metadaten**und Weiterleiten der Metadatendatei zu SimpleNexus.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Einmaliges Anmelden konfigurieren")

    >[AZURE.NOTE] Einmaliges Anmelden muss aktiviert werden, indem SimpleNexus Support-Team.

5.  Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD SimpleNexus anmelden können, müssen sie in SimpleNexus bereitgestellt werden.  
Bei SimpleNexus ist die Bereitstellung einer manuellen Aufgabe der Tenant-Administrator ausgeführt.

>[AZURE.NOTE] Können Sie alle anderen SimpleNexus Benutzer Konto Erstellungstools oder APIs von SimpleNexus Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu SimpleNexus:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **SimpleNexus **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)