<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit PolicyStat | Microsoft Azure" 
    description="Erfahren Sie, wie mit PolicyStat in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Lernprogramm: Azure Active Directory-Integration mit PolicyStat
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und PolicyStat anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   PolicyStat Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der PolicyStat Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die PolicyStat zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für PolicyStat
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Szenario")
##<a name="enabling-the-application-integration-for-policystat"></a>Die Application Integration für PolicyStat
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für PolicyStat aktivieren.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für PolicyStat:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **PolicyStat**.

    ![Anwendung Galerie] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **PolicyStat aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer PolicyStat mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Die PolicyStat-Anwendung erwartet SAML-Assertionen in einem bestimmten Format, das benutzerdefinierte Attribut Zuordnung zu Ihrer Konfiguration **Saml-token Attribute** hinzufügen muss.  
Der folgende Screenshot zeigt ein Beispiel dafür.

![Attribute] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Attribute")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **PolicyStat** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei PolicyStat** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Einmaliges Anmelden konfigurieren")

3.  Geben Sie auf der Seite **Konfigurieren Appeinstellungen** im Textfeld **Anmelde-URL** die URL von den Benutzern Ihrer Anwendung URL PolicyStat anmelden (z. B.: *"https://demo-azure.policystat.com"*), und klicken Sie dann auf **Weiter**.

    ![Anwendung konfigurieren] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Anwendung konfigurieren")

4.  Auf der Seite **Konfigurieren einmaliges Anmelden am PolicyStat** auf **Metadaten**und speichern Sie Metadaten-Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens PolicyStat.

6.  Klicken Sie auf die Registerkarte " **Admin** " und klicken Sie dann im linken Navigationsbereich auf **Konfiguration für einzelne Zeichen** .

    ![Menü "Administrator"] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Menü "Administrator"")

7.  Wählen Sie im Abschnitt **Einrichten** **einzigen aktivieren anmelden Integration**.

    ![Konfiguration für einzelne Zeichen] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Konfiguration für einzelne Zeichen")

8.  Klicken Sie auf **Attribute konfigurieren**und anschließend im Abschnitt **Attribute konfigurieren** folgendermaßen:

    ![Konfiguration für einzelne Zeichen] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguration für einzelne Zeichen")

    1.  Geben Sie im Textfeld **Benutzername Attribut** **Uid**.
    2.  Geben Sie im Textfeld **Vorname Attribut** **Vorname**.
    3.  Geben Sie im Textfeld **Last Name-Attribut** **Lastname**.
    4.  Geben Sie in das Textfeld **E-Mail-Attribut** **Emailaddress**.
    5.  Klicken Sie auf **Speichern**.

9.  Auf **Der IDP-Metadaten**und klicken Sie dann im Abschnitt **Der IDP Metadaten** folgendermaßen:

    ![Konfiguration für einzelne Zeichen] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguration für einzelne Zeichen")

    1.  Öffnen Sie die Metadatendatei heruntergeladen, kopieren Sie den Inhalt und fügen Sie ihn in das Textfeld **Ihre Identität Metadaten**
    2.  Klicken Sie auf **Speichern**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Einmaliges Anmelden konfigurieren")

11. 12. Klicken Sie im Menü oben auf **Attribute** um das **SAML-Token-Eigenschaften** -Dialogfeld öffnen.

    ![Attribute] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Attribute")

13. Fügen Sie erforderliche Attribut Mappings Schritte:

    ![Attribute] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Attribute")

    1.  Klicken Sie auf **Attribut hinzufügen**.
    2.  Geben Sie im Textfeld **Attributname** **Uid**.
    3.  Wählen Sie im Feld **Attributwert** **ExtractMailPrefix()**.
    4.  Wählen Sie aus **der Liste** **User.mail**.
    5.  Klicken Sie auf **abgeschlossen**.
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD PolicyStat anmelden können, müssen sie in PolicyStat bereitgestellt werden.  
PolicyStat unterstützt nur Zeit Benutzer bereitstellen. Dies bedeutet, Sie brauchen Benutzer PolicyStat manuell hinzufügen.  
Der Benutzer werden automatisch bei ihrer ersten Anmeldung durch einmaliges auf hinzugefügt.

>[AZURE.NOTE]Können Sie alle anderen PolicyStat Benutzer Konto Erstellungstools oder APIs von PolicyStat Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu PolicyStat:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **PolicyStat **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)