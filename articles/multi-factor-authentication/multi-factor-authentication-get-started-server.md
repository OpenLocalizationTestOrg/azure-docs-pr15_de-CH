<properties 
    pageTitle="Erste Schritte mit Azure mehrstufige Authentifizierungsserver"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die Einstieg in Azure MFA-Server beschrieben."
    services="multi-factor-authentication"
    keywords="Authentifizierung, Azure Multi Faktor Authentifizierung app Aktivierungsseite Server Authentication Server herunterladen"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Erste Schritte mit Azure mehrstufige Authentifizierungsserver




<center>![Cloud](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Da wir festgestellt haben, ob mit mehrstufigen Authentifizierung lokal, wir loslegen. Diese Seite umfasst eine Neuinstallation des Servers und mit lokalen Active Directory. Wenn Sie bereits die PhoneFactor installiert Server und zum Aktualisieren finden Sie unter [Upgrading to Server mehrstufige Azure](multi-factor-authentication-get-started-server-upgrade.md) oder suchen Informationen zur Installation von nur der Webdienst finden Sie unter [Azure mehrstufige Authentifizierung Server Mobile App Web Service bereitstellen](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Server herunterladen



Es gibt zwei Arten, Azure mehrstufige Authentifizierungsserver herunterzuladen. Beide erfolgt über das Azure-Portal. Die erste ist Mehrfaktor-Authentifizierungsanbieter direkt verwalten. Das zweite ist über die Einstellungen. Die zweite Option ist eine mehrstufige Authentifizierungsanbieter oder eine Azure MFA, Azure AD Premium oder Enterprise Mobility Suite-Lizenz erforderlich.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Azure Multi-Factor Authentication Server von Azure-Portal herunterladen
--------------------------------------------------------------------------------

1. Azure-Portal als Administrator anmelden.
2. Wählen Sie auf der linken Seite Active Directory.
3. Klicken Sie auf der Seite Active Directory oben **Mehrstufige Authentifizierung Anbieter**
4. Klicken Sie unten auf **Verwalten**
5. Dadurch wird eine neue Seite geöffnet.  Klicken Sie auf **Downloads.** 
 ![Herunterladen](./media/multi-factor-authentication-sdk/download.png)
6. Über **Aktivierung Anmeldeinformationen zu generieren**, klicken Sie auf **herunterladen.** 
 ![Herunterladen](./media/multi-factor-authentication-get-started-server/download4.png)
7. Speichern Sie den Download.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Azure mehrstufige Authentifizierungsserver über die Einstellungen herunterladen


1. Azure-Portal als Administrator anmelden.
2. Wählen Sie auf der linken Seite Active Directory.
3. Doppelklicken Sie auf die Instanz von Azure AD.
4. Klicken Sie oben auf **Konfigurieren**
![herunterladen](./media/multi-factor-authentication-sdk/download2.png)
5. Wählen Sie unter mehrstufige Authentifizierung **Einstellungen verwalten**
6. Auf der Seite Services Settings am unteren Bildschirmrand klicken Sie auf **das Portal**.
![Herunterladen](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Dadurch wird eine neue Seite geöffnet. Klicken Sie auf **Downloads.**
8. Über **Aktivierung Anmeldeinformationen zu generieren**, klicken Sie auf **herunterladen.**
9. Speichern Sie den Download.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Server installieren und konfigurieren
Da Sie Server heruntergeladen haben, installieren und konfigurieren.  Achten Sie auf installierten Server die erfüllt:



Azure mehrstufige Authentifizierungsanforderungen|Beschreibung|
:------------- | :------------- |
Hardware|<li>200 MB freier Festplattenspeicher</li><li>kann X32 oder X64-Prozessor</li><li>1 GB oder mehr RAM</li>
Software|<li>WindowsServer 2008 oder höher ist der Host ein Server-Betriebssystem</li><li>Windows 7 oder höher ist der Host ein Client-Betriebssystem</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 oder höher, wenn User Portal oder Web Service SDK</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure mehrstufige Authentifizierung firewallanforderungen
--------------------------------------------------------------------------------
Jeder MFA-Server muss Port 443 abgehend folgende kommunizieren:

- https://Pfd.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Wenn ausgehende Firewalls auf Port 443 eingeschränkt sind, müssen IP-Adressbereiche geöffnet werden:

IP-Subnetz|Netzmaske|IP-Bereich
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Wenn Sie nicht Azure mehrstufige Authentifizierung Ereignis Bestätigung verwenden und wenn Benutzer nicht werden authentifizieren mit mehrstufigen Authentifizierung mobiler apps von Geräten in diesem Netzwerk die IP-Adresse Bereiche wie folgt reduziert:


IP-Subnetz|Netzmaske|IP-Bereich
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Installieren und Konfigurieren von Azure Multi-Factor Authentication server
--------------------------------------------------------------------------------


1. Doppelklicken Sie auf die ausführbare Datei. Dadurch wird die Installation beginnen.
2. Auf dem Bildschirm Installationsordner auswählen sicher, dass der Ordner korrekt ist und klicken Sie auf Weiter.
3. Sobald die Installation abgeschlossen ist, klicken Sie auf Fertig stellen.  Der Konfigurations-Assistent wird gestartet.
4. Willkommenseite auf den Konfigurations-Assistenten **Überspringen mithilfe des Assistenten Authentifizierung** aktivieren Sie dieses Kontrollkästchen und klicken Sie auf **Weiter**.  Dies wird den Assistenten zu schließen und den Server starten.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Klicken Sie auf der Seite, der wir den Server heruntergeladen haben auf **Generieren Aktivierung Anmeldeinformationen** .  Kopieren Sie diese Informationen in Azure MFA-Server in die Textfelder, und klicken Sie auf **Aktivieren**.


Express-Setup mit dem Assistenten anzeigen die Schritte  Im Menü Extras auf dem Server auswählen können Sie den Authentifizierung-Assistenten erneut ausführen.



##<a name="import-users-from-active-directory"></a>Benutzer aus Active Directory importieren

Server installiert und konfiguriert ist, können Sie Benutzer schnell in Azure MFA-Server importieren.

### <a name="to-import-users-from-active-directory"></a>So importieren Sie Benutzer aus Active Directory
--------------------------------------------------------------------------------


1. Wählen Sie **Benutzer**in Azure MFA-Server auf der linken Seite.
2. Wählen Sie unten **aus Active Directory importieren**.
3. Jetzt suchen nach einzelnen Benutzern oder Organisationseinheiten für Benutzer werden AD-Verzeichnis suchen.  In diesem Fall geben wir die Organisationseinheit Benutzer.
4. Markieren Sie alle Benutzer auf der rechten Seite und klicken Sie auf **Importieren**.  Sie erhalten ein Popup angezeigt, dass Sie erfolgreich waren.  Schließen Sie das Fenster importieren.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Senden Sie den Benutzern eine e-Mail
Nun, dass die Benutzer in Azure Multi-Factor Authentication Server importiert haben, wird empfohlen, dass Sie den Benutzern eine e-Mail, die sie darüber informiert, dass sie mehrstufige Authentifizierung registriert wurden.

Azure mehrstufige Authentifizierung Server werden verschiedene Verwendungsmöglichkeiten für mehrstufige Authentifizierung Benutzer konfigurieren.  Beispielsweise wird Wissen der Benutzer Telefonnummern oder wurden die Telefonnummern in Azure mehrstufige Authentifizierungsserver aus ihrem Unternehmen Verzeichnis importieren e-Mail sollten Benutzer wissen, dass sie Azure Mehrfaktor-Authentifizierung verwenden, einige Azure mehrstufige Authentifizierung bereitgestellt und die Benutzer erhalten sie ihre Authentifizierungen auf Telefonnummer konfiguriert wurden.  

Der Inhalt der e-Mail variieren je nach Authentifizierungsmethode, die für den Benutzer (z. B. Telefonanruf, SMS, mobile-app) festlegen.  Z. B. wenn der Benutzer eine PIN verwenden, wenn sie sich authentifizieren muss, informiert e-Mail sie was seine anfängliche PIN festgelegt wurde.  Benutzer müssen in der Regel während der ersten Authentifizierung ihre PIN zu ändern.

Wenn Benutzer Telefonnummern nicht konfiguriert oder Azure mehrstufige Authentifizierungsserver importiert oder Benutzer vorkonfiguriert sind, die app für die Authentifizierung verwenden, können Sie eine e-Mail senden, die wissen sie Azure mehrstufige Authentifizierung konfiguriert wurden, leitet sie die Konto-Registrierung über Azure mehrstufige Authentifizierung User Portal abgeschlossen.  Ein Hyperlink werden, der Benutzer klickt auf User Portal zugreifen. Klickt der Benutzer auf den Hyperlink wird Webbrowser geöffnet und sie ihrem Unternehmen Azure mehrstufige Authentifizierung User Portal.   


### <a name="configuring-email-and-email-templates"></a>Konfigurieren von e-Mail und e-Mail-Vorlagen

Indem Sie auf das e-Mail-Symbol auf der linken Seite können Sie die Standardeinstellungen für diese e-Mails einrichten.  Hier wird die SMTP-Informationen Ihres Mail-Servers eingeben und können eine große Decke e-Mail senden ein Häkchen hinzu Kontrollkästchen Benutzer e-Mails senden.

![E-Mail-Einstellung](./media/multi-factor-authentication-get-started-server/email1.png)

Auf der Registerkarte e-Mail-Inhalte sehen Sie alle verschiedenen e-Mail-Vorlagen, die verfügbar sind.  Je nach Konfiguration Benutzer mehrstufige Authentifizierung müssen Sie die Vorlage auswählen können passt, die am besten Ihnen.

![E-Mail-Vorlagen](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Behandlung von Azure mehrstufige Authentifizierungsserver Benutzerdaten

Bei Verwendung die lokalen mehrstufige Authentifizierung (MFA) Server werden ein Benutzer Daten in lokalen Server gespeichert. Keine permanenten Benutzerdaten in der Cloud gespeichert. Wenn der Benutzer eine zweistufige Authentifizierung, sendet der Server MFA Daten zum Azure MFA-Cloud-Dienst zum Durchführen der Authentifizierung. Wenn diese Authentifizierungsanfragen zum Cloud-Dienst gesendet werden, sind die folgenden Felder in der Anforderung und Protokolle gesendet, damit verfügbar Kundenberichten Authentifizierung-Verwendung. Einige Felder sind optional, sie können aktiviert oder die mehrstufige Authentifizierung Server deaktiviert. Die Kommunikation zwischen dem Server MFA MFA-Clouddienst verwendet SSL/TLS über Port 443 abgehend. Diese Felder sind:

- Eindeutige ID - Benutzernamen oder interne MFA Server ID
- Erste und letzte Name - optional
- E-Mail-Adresse: optional
- Telefonnummer - Wenn ein Anruf oder SMS-Authentifizierung
- Gerätetoken - bei mobilen app-Authentifizierung
- Authentifizierungsmodus
- Ergebnis der Authentifizierung
- MFA-Servernamen
- MFA Server IP
- Client IP – falls verfügbar



Neben den Feldern ist Authentifizierungsergebnis (Erfolg/Ablehnung) und Grund für jede verweigerte auch die Authentifizierungsdaten gespeichert und über die Authentifizierung/Verwendungsberichte verfügbar.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Erweiterte Konfigurationen Azure mehrstufige Authentifizierung
Weitere Informationen zur erweiterten Installation und Konfiguration anhand der folgenden Tabelle.

-Methode|Beschreibung
:------------- | :------------- |
[User Portal](multi-factor-authentication-get-started-portal.md)|  Informationen zum Einrichten und Konfigurieren des Bereitstellung sowie Benutzer User Portals.
[Active Directory-Verbunddienst](multi-factor-authentication-get-started-adfs.md)|Informationen zum Einrichten von Azure mehrstufige Authentifizierung mit AD FS.
[RADIUS-Authentifizierung](multi-factor-authentication-get-started-server-radius.md)|  Informationen zur Installation und Konfiguration von Azure MFA-Server mit RADIUS.
[IIS-Authentifizierung](multi-factor-authentication-get-started-server-iis.md)|Informationen zur Installation und Konfiguration von Azure MFA-Server mit IIS.
[Windows-Authentifizierung](multi-factor-authentication-get-started-server-windows.md)|  Informationen über Setup und Konfiguration von Azure MFA-Server mit Windows-Authentifizierung.
[LDAP-Authentifizierung](multi-factor-authentication-get-started-server-ldap.md)|Informationen über Setup und Konfiguration von Azure MFA-Server mit LDAP-Authentifizierung.
[Remote Desktop Gateway und Azure mehrstufige Authentifizierungsserver mittels RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informationen zum Einrichten und Konfigurieren von Azure MFA-Server mit Remotedesktopgateway RADIUS verwenden.
[Mit Windows Server Active Directory synchronisieren](multi-factor-authentication-get-started-server-dirint.md)|Informationen zum Einrichten und Konfigurieren der Synchronisierung zwischen Active Directory und Azure MFA-Server.
[Den Serverdienst Mobile Web App Azure mehrstufige Authentifizierung bereitstellen](multi-factor-authentication-get-started-server-webservice.md)|Informationen über Setup und Konfiguration von Azure MFA-Serverwebdienst.
