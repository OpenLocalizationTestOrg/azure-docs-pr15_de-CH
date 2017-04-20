<properties 
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit NetDocuments | Microsoft Azure" 
    description="Erfahren Sie, wie mit NetDocuments in Azure Active Directory-auf automatisierte Bereitstellung und mehr!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Lernprogramm: Azure Active Directory-Integration mit NetDocuments
  
Das Ziel dieses Lernprogramms ist die Integration von Azure und NetDocuments anzeigen.  
In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:

-   Ein gültiges Azure-Abonnement
-   NetDocuments Mieter
  
Am Ende dieses Lernprogramms werden können einzelne Zeichen in der Anwendung auf der NetDocuments Website (Service Provider initiiert anmelden) oder mit der [Einführung der](active-directory-saas-access-panel-introduction.md)Azure AD-Benutzer, die NetDocuments zugeordnet haben.
  
In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:

1.  Die Application Integration für NetDocuments
2.  Konfigurieren des einmaligen Anmeldens
3.  Konfigurieren der Benutzer bereitstellen
4.  Zuweisen von Benutzern

![Szenario] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Szenario")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Die Application Integration für NetDocuments
  
Dieser Abschnitt soll beschreiben die Anwendungsintegration für NetDocuments aktivieren.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für NetDocuments:

1.  Klicken Sie im klassischen Azure-Portal, klicken Sie im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Applikationen")

4.  Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Anwendung hinzufügen] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Anwendung hinzufügen")

5.  Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Hinzufügen einer Anwendung gallerry] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Hinzufügen einer Anwendung gallerry")

6.  Geben Sie im **Suchfeld** **NetDocuments**.

    ![Anwendung Galerie] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Anwendung Galerie")

7.  Wählen Sie im Ergebnisbereich **NetDocuments aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens
  
Dieser Abschnitt soll Gliedern wie Benutzer NetDocuments mit seinem Konto in Azure AD Zusammenschlüsse anhand der SAML-Protokolls authentifizieren können.  
Konfigurieren einmaliges Anmelden für NetDocuments erfordert Fingerabdruck eines Zertifikats Abrufen eines.  
Wenn Sie nicht mit diesen Verfahren vertraut sind, finden Sie unter [Fingerabdruckwert ein Zertifikat abrufen](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren einmaliges Anmelden:

1.  Klicken Sie im klassischen Azure-Portal, auf der Seite Application Integration **NetDocuments** auf **Konfigurieren einmaliges Anmelden** **Konfigurieren Einmalanmeldung** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Einmaliges Anmelden konfigurieren")

2.  Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei NetDocuments** **Microsoft Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Einmaliges Anmelden konfigurieren")

3.  Folgendermaßen Sie auf der Seite **Konfigurieren App URL** an:

    ![Konfigurieren von App-URL] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Konfigurieren von App-URL")

    1.  Geben Sie im Textfeld **Anmelde-URL** den URL zur NetDocuments Anwendung anmelden von Benutzern verwendet (z. B.: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  Geben Sie im Textfeld **NetDocuments Antwort-URL** in eingegebene denselben Wert der **Anmelde-URL** -Textfeld.  

        >[AZURE.NOTE]Finden Sie den richtigen Wert am Ende des Dialogfelds **Federated Identity** (Siehe Screenshot für Schritt 9).

    3.  Klicken Sie auf **Weiter**

4.  Herunterladen Sie auf der Seite **Konfigurieren einmaliges Anmelden am NetDocuments** das Zertifikat auf **Zertifikat herunterladen**und dann lokal auf Ihrem Computer gespeichert.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Einmaliges Anmelden konfigurieren")

5.  Melden Sie in verschiedenen Webbrowserfenster als Administrator der Website Ihres Unternehmens NetDocuments.

6.  Fahren Sie mit **Admin**.

7.  Klicken Sie auf **Hinzufügen und Entfernen von Benutzern und Gruppen**.

    ![Repository] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

8.  Klicken Sie auf **Konfigurieren erweiterter Optionen**.

    ![Konfigurieren erweiterter Optionen] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Konfigurieren erweiterter Optionen")

9.  **Federated Identity** -Dialogfeld die folgenden Schritte:

    ![Verbundene Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Verbundene Identitty")

    1.  **Federated Identity Servertyp**wählen Sie **Active Directory Federation Services aus**.
    2.  Klicken Sie auf **Datei auswählen**, die heruntergeladenen Metadatendatei laden.
    3.  Klicken Sie auf **OK**.

10. Der Azure-Verwaltungsportal wählen Sie Bestätigung Konfiguration für einzelne Zeichen aus und dann auf **vollständig** **Konfigurieren Single Sign-On** -Dialogfeld zu schließen.

    ![Einmaliges Anmelden konfigurieren] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Einmaliges Anmelden konfigurieren")
##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzer bereitstellen
  
Um Benutzer Azure AD NetDocuments anmelden können, müssen sie in NetDocuments bereitgestellt werden.  
Bei NetDocuments ist die Bereitstellung einer manuellen Aufgabe.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurieren der benutzerbereitstellung folgendermaßen Sie an:

1.  Singen Sie als Administrator auf der Website Ihres Unternehmens **NetDocuments** .

2.  Klicken Sie im Menü oben auf **Administration**.

    ![Admin] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Admin")

3.  Klicken Sie auf **Hinzufügen und Entfernen von Benutzern und Gruppen**.

    ![Repository] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Repository")

4.  Geben Sie in das Textfeld **E-Mail-Adresse** die e-Mail-Adresse eines gültigen Azure Active Directory-Kontos bereitstellen möchten, und klicken Sie dann auf **Benutzer hinzufügen**.

    ![E-Mail-Adresse] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-Mail-Adresse")

    >[AZURE.NOTE]Kontoinhaber Azure Active Directory erhalten eine e-Mail, die enthält einen Link, um das Konto bestätigen, bevor es aktiviert wird.

>[AZURE.NOTE]Können Sie alle anderen NetDocuments Benutzer Konto Erstellungstools oder APIs von NetDocuments Bestimmung AAD Benutzerkonten.

##<a name="assigning-users"></a>Zuweisen von Benutzern
  
Testen Ihrer Konfiguration müssen Sie Azure AD-Benutzern gewähren, ermöglichen die Anwendung auf zuweisen möchten.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Gehen Sie zum Zuweisen von Benutzern zu NetDocuments:

1.  Erstellen Sie ein Testkonto in der Azure-Verwaltungsportal.

2.  Klicken Sie auf der Seite **NetDocuments **Application Integration auf **Benutzer zuweisen**.

    ![Benutzer zuweisen] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Benutzer zuweisen")

3.  Wählen Sie Ihre Testbenutzer, klicken Sie auf **zuweisen**und klicken Sie auf **Ja,** um die Aufgabe zu bestätigen.

    ![Ja] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Ja")
  
So testen Sie die einzelnen Einstellungen anmelden, Öffnen der. Weitere Informationen über die anzeigen Sie [Einführung der](active-directory-saas-access-panel-introduction.md)