<properties 
    pageTitle="Lernprogramm: Arbeitstag für eingehende Synchronisierung konfigurieren | Microsoft Azure" 
    description="Erfahren Sie, wie mit eingehende Synchronisierung mit Active Directory Azure-auf automatisierte Bereitstellung und mehr!" 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Lernprogramm: Konfigurieren der Arbeitstag für eingehende Synchronisierung
>[AZURE.NOTE]Azure Active Directory (AD) Premium ist verfügbar für Kunden in China weltweite Instanz von Azure AD.    
Azure AD Premium ist in den Dienst Microsoft Azure von 21Vianet in China derzeit nicht unterstützt.    

Das Ziel dieses Lernprogramms ist die Schritte zeigen Arbeitstag und Microsoft Azure AD Personen von Arbeitstag an Microsoft Azure AD ausführen müssen.    
 In diesem Lernprogramm beschriebenen Szenario wird davon ausgegangen, dass Sie bereits folgende Elemente:  

-   Ein gültiges Azure-Abonnement  
-   Ein Mieter Arbeitstag  

In diesem Lernprogramm beschriebenen Szenario umfasst die folgenden Bausteine erforderlich:  

1.  Aktivieren der Anwendungsintegration Arbeitstag  
2.  Erstellen eines Benutzers mit System integration  
3.  Erstellen einer Sicherheitsgruppe  
4.  Zuweisen der Integration System der Sicherheitsgruppe  
5.  Konfigurieren der Sicherheitsgruppe  
6.  Änderung der Sicherheitsrichtlinien aktivieren  
7.  Konfigurieren von Benutzerimport in Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>Aktivieren der Anwendungsintegration Arbeitstag

Dieser Abschnitt soll beschreiben die Anwendungsintegration Salesforce aktivieren.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration Arbeitstag:

1.  Klicken Sie in der Azure-Verwaltungsportal im linken Navigationsbereich auf **Active Directory**.    

    ![Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Active Directory")  

2.  Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.    

3.  Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .    

    ![Applikationen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Applikationen")  

4.  Öffnen Sie die **Anwendung Galerie**auf **Einer Anwendung hinzufügen**und dann auf **eine Anwendung für meine Organisation hinzufügen**.    

    ![Was möchten Sie tun?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Was möchten Sie tun?")  

5.  Geben Sie im **Suchfeld** **Arbeitstag**.    

    ![Arbeitstag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "Arbeitstag")  

6.  Klicken Sie im Ergebnisbereich wählen Sie **Arbeitstag aus**und dann auf **vollständig** die Anwendung hinzufügen.    

    ![Arbeitstag] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "Arbeitstag")  

##<a name="creating-an-integration-system-user"></a>Erstellen eines Benutzers mit System integration

1.  **Arbeitstag Workbench** **Erstellen Sie Benutzer** in das Suchfeld Geben Sie ein und klicken Sie dann auf den Link **Integration System-Benutzer erstellen**.     

    ![Benutzer erstellen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "Benutzer erstellen")  

2.  Aufgabe erstellen Integrationsbenutzer System mit einen Benutzernamen und ein Kennwort für eine neue Integrationssystembenutzer.  Lassen Sie erfordern neue Kennwort am nächsten Anmeldung Option deaktiviert ist, da dieser Benutzer programmgesteuert anmelden    
    Lassen Sie Minuten der Sitzung mit dem Standardwert 0 des Benutzers Sessions Timeout vorzeitig verhindern.    

    ![Integration System erstellen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Integration System erstellen")  

##<a name="creating-a-security-group"></a>Erstellen einer Sicherheitsgruppe

In diesem Lernprogramm beschriebenen Szenario müssen Sie eine uneingeschränkte Integration System Sicherheitsgruppe erstellen und Benutzer zuweisen.    

1.  Geben Sie Sicherheitsgruppe erstellen, in das Suchfeld, und klicken Sie auf Gruppe erstellen.     

    ![CreateSecurity Gruppe] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "CreateSecurity Gruppe")  

2.  Abschließen der Aufgabe Sicherheitsgruppe erstellen.  Wählen Sie Integration System Security Group – ist in der Dropdownliste Typ des vermietete Sicherheitsgruppe kein Hindernis für eine Sicherheitsgruppe erstellen, Mitglieder explizit hinzugefügt.     

    ![CreateSecurity Gruppe] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "CreateSecurity Gruppe")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>Zuweisen der Integration System der Sicherheitsgruppe

1.  Geben Sie Sicherheitsgruppe bearbeiten in das Suchfeld ein, und klicken Sie auf den Link **Gruppe bearbeiten**.     

    ![Gruppe bearbeiten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Gruppe bearbeiten")  

2.  Suchen Sie und wählen Sie die neue Sicherheitsgruppe Integration nach Namen    

    ![Gruppe bearbeiten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Gruppe bearbeiten")  

3.  Die neue Sicherheitsgruppe neue integrierte Systembenutzer hinzufügen.       

    ![System-Sicherheitsgruppe] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "System-Sicherheitsgruppe")  

##<a name="configuring-security-group-options"></a>Konfigurieren der Sicherheitsgruppe

In diesem Schritt gewähren Sie der neuen Gruppe-Sicherheitsberechtigungen für Get und Put-Vorgänge für die Objekte durch Sicherheitsrichtlinien von Domäne gesichert:  

-   Externe Bereitstellung  
-   : Arbeitskraft öffentliche Arbeitskraft Berichte  
-   Arbeitskräftedaten: Alle Positionen  
-   Worker Daten: Personal Information aktuell  
-   Arbeitskräftedaten: Titel für dieses Profil  

&nbsp;  

1.  Geben Sie Sicherheitsrichtlinien für Domänen in das Suchfeld ein, und klicken Sie auf den Link Domäne Sicherheitsrichtlinien für Funktionsbereich.     

    ![Sicherheitsrichtlinien für Domänen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Sicherheitsrichtlinien für Domänen")  

2.  System suchen Sie und wählen Sie des Funktionsbereichs System aus.  Klicken Sie auf die Schaltfläche, OK.     

    ![Sicherheitsrichtlinien für Domänen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Sicherheitsrichtlinien für Domänen")  

3.  In der Liste der Sicherheitsrichtlinien für den Funktionsbereich System erweitern Sie Sicherheitsadministration und wählen Sie der Domänensicherheitsrichtlinie externe Bereitstellung.     

    ![Sicherheitsrichtlinien für Domänen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Sicherheitsrichtlinien für Domänen")  

4.  Klicken Sie auf Berechtigungen bearbeiten und dann auf dem Bildschirm Berechtigungen bearbeiten der Liste der Sicherheitsgruppen mit Get und Put Integration Berechtigungen fügen Sie die neuen Sicherheitsgruppe hinzu.     

    ![Berechtigung bearbeiten] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Berechtigung bearbeiten")  

5.  Wiederholen Sie Schritt 1 oben auf dem Bildschirm für die Auswahl von Funktionsbereichen zurückgegeben und diesmal suchen Personal, wählen Sie den Funktionsbereich Personal und klicken Sie auf die Schaltfläche, OK.    

    ![Sicherheitsrichtlinien für Domänen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Sicherheitsrichtlinien für Domänen")  

6.  Erweitern Sie in der Liste der Sicherheitsrichtlinien für den Funktionsbereich Personal Arbeitskräftedaten: Personal, und wiederholen Sie Schritt 4 für jeden verbleibenden Sicherheitsrichtlinien:    

    -   : Arbeitskraft öffentliche Arbeitskraft Berichte  
    -   Arbeitskräftedaten: Alle Positionen  
    -   Worker Daten: Personal Information aktuell  
    -   Arbeitskräftedaten: Titel für dieses Profil    

    ![Sicherheitsrichtlinien für Domänen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Sicherheitsrichtlinien für Domänen")  

##<a name="activating-security-policy-changes"></a>Änderung der Sicherheitsrichtlinien aktivieren

1.  Geben Sie im Suchfeld aktivieren und klicken Sie dann auf den Link aktivieren Sie ausstehende Änderung der Sicherheitsrichtlinien.    

    ![Aktivieren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktivieren")  

2.   Beginn der Aktivierung ausstehende Änderung der Sicherheitsrichtlinien für Überwachungszwecke einen Kommentar eingeben und auf die Schaltfläche, OK klicken.      

    ![Ausstehende Sicherheit aktivieren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Ausstehende Sicherheit aktivieren")  

3.  Aufgabe auf der nächsten Seite durch Aktivieren des Kontrollkästchens bestätigen bzw. auf die Schaltfläche, OK.     

    ![Ausstehende Sicherheit aktivieren] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Ausstehende Sicherheit aktivieren")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Konfigurieren von Benutzerimport in Microsoft Azure AD

Dieser Abschnitt soll Microsoft Azure AD Personen von Arbeitstag importieren konfigurieren gliedern.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Benutzer importieren um in Microsoft Azure AD konfigurieren, die folgenden Schritte:

1.  Klicken Sie auf Anwendungsseite Integration **Arbeitstag** zum Dialogfeld **Bereitstellung konfigurieren** **Konfigurieren Benutzer importieren** .    

2.  Führen Sie auf der Seite **Einstellungen und Administratoranmeldeinformationen** die folgenden Schritte aus und klicken Sie auf Weiter:    

    ![Einstellung und Administratoranmeldeinformationen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Einstellung und Administratoranmeldeinformationen")    

    1.  Geben Sie den Namen des Benutzers im Abschnitt [Erstellen einer Integration System](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) erstellte Textfeld **Arbeitstag Admin-Benutzernamen** .    
    2.  Geben Sie das Kennwort des Benutzers im Abschnitt [Erstellen einer Integration System](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) erstellte Textfeld **Administratorkennwort Arbeitstag** .    
    3.  Geben Sie im Textfeld **Arbeitstag Mieter URL** die URL oder Ihrem Mandanten Arbeitstag.    

3.  Auf der Seite **Verbindung testen** auf Konnektivität bestätigen **Test starten,** und klicken Sie auf **Weiter**.    

    ![Verbindung testen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Verbindung testen")  

4.  Klicken Sie auf der Seite " **Bereitstellung"** auf **Weiter**.    

    ![Optionen bereitstellen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Optionen bereitstellen")  

5.  Klicken Sie im Dialogfeld **Bereitstellen** auf **abgeschlossen**.    

    ![Bereitstellen] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Bereitstellen")  

Jetzt können Sie die **Benutzer** Abschnitt und überprüfen Sie, ob Ihr Arbeitstag Benutzer importiert wurde.    
