<properties 
    pageTitle="User Portal bereitstellen für Azure mehrstufige Authentifizierungsserver"
    description="Dies ist der Azure mehrstufige Authentifizierungsseite, die Azure MFA-Benutzerportal Schritte beschreibt."
    services="multi-factor-authentication"
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

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>User Portal bereitstellen für Azure mehrstufige Authentifizierungsserver

User Portal kann der Administrator zur Installation und Konfiguration von Azure mehrstufige Authentifizierung User Portal. User Portal ist eine IIS-Website der Benutzer Azure mehrstufige Authentifizierung und Konten. Benutzer kann ihre Telefonnummer, Ändern ihrer PIN oder Azure mehrstufige Authentifizierung bei ihrer nächsten Anmeldung zu umgehen.

Benutzer werden User Portal mit ihren normalen Benutzernamen und Kennwort anmelden und Abschließen ein Aufrufs Azure mehrstufige Authentifizierung oder Sicherheit Fragen, um die Authentifizierung abzuschließen. Wenn Benutzeranmeldung zulässig ist, kann Benutzer ihre Telefonnummer und PIN erstmals konfigurieren User Portal anmelden.

Benutzeradministratoren Portal kann einrichten und die Berechtigung zum Hinzufügen neuer Benutzer und Aktualisieren vorhandener Benutzer.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>User Portal auf dem gleichen Server wie Azure mehrstufige Authentifizierungsserver bereitstellen

Die folgenden erforderlichen Komponenten sind für das Portal Benutzer auf demselben Server wie der Azure mehrstufige Authentifizierungsserver installieren:

- IIS muss asp.net und IIS 6 Meta Basis Kompatibilität (IIS 7 oder höher) installiert werden
- Angemeldete Benutzer muss Administratorrechte für den Computer und Domäne, falls vorhanden.  Dies ist das Konto benötigt Berechtigungen zum Erstellen von Active Directory-Sicherheitsgruppen.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>User Portal für Azure mehrstufige Authentifizierungsserver bereitstellen

1. Azure mehrstufige Authentifizierung Server: User Portal klicken Sie im linken Menü, klicken Sie auf User Portal installieren.
1. Klicken Sie auf Weiter.
1. Klicken Sie auf Weiter.
1. Wenn der Computer einer Domäne Mitglied und Active Directory-Konfiguration zur Absicherung der Kommunikation zwischen User Portal und Azure mehrstufige Authentifizierung unvollständig ist, wird Active Directory Schritt angezeigt. Klicken Sie auf Weiter, um diese Konfiguration automatisch abgeschlossen.
1. Klicken Sie auf Weiter.
1. Klicken Sie auf Weiter.
1. Klicken Sie auf Schließen.
1. Öffnen Sie einen Webbrowser von jedem Computer aus, und navigieren Sie zu der URL dem User Portal installiert wurde (z.B. https://www.publicwebsite.com/MultiFactorAuth). Stellen Sie sicher, dass kein Zertifikat Warn- oder Fehlermeldungen angezeigt werden.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Bereitstellen von Azure Multi-Factor Authentication Server User Portal auf einem anderen Server

Nutzung der Azure mehrstufige Authentifizierung App sind erforderlich, damit die Anwendung erfolgreich mit User Portal kommunizieren kann:

Finden Sie Hardware und Software für Hardware und Software:

- Sie müssen verwenden V6. 0 oder höher von Azure mehrstufige Authentifizierungsserver.
- User Portal installiert mit Microsoft® Internet Information Services (IIS) auf einem Internet verbundenen Webserver IIS 6.x 7.x oder höher.
- Bei Verwendung von IIS 6.x gewährleisten ASP.NET v2.0.50727 ist installiert, registriert und zugelassen.
- Erforderliche Rollendienste Verwendung IIS 7.x oder höher ASP.NET und IIS 6-Metabase-Kompatibilität.
- User Portal sollte ein SSL-Zertifikat gesichert werden.
- Azure mehrstufige Authentifizierung Web Service SDK installiert in IIS 6.x IIS 7.x oder höher auf dem Server, Azure mehrstufige Authentifizierungsserver installiert ist.
- Azure mehrstufige Authentifizierung Web Service SDK muss ein SSL-Zertifikat geschützt werden.
- User Portal muss Azure mehrstufige Authentifizierung Web Service SDK über SSL herstellen können.
- User Portal dürfen zur Authentifizierung von Azure mehrstufige Authentifizierung Web Service SDK mit den Anmeldeinformationen eines Dienstkontos, das Mitglied der Sicherheitsgruppe "PhoneFactor-Admins" ist. Dieses Dienstkonto und Gruppe vorhanden in Active Directory Wenn Azure mehrstufige Authentifizierungsserver auf einem Server Domäne ausgeführt wird. Dieses Dienstkonto und Gruppe vorhanden lokal auf dem Azure mehrstufige Authentifizierungsserver, wenn er nicht Mitglied einer Domäne ist.

Installieren des Portals Benutzer auf einem Server als Azure mehrstufige Authentifizierungsserver muss die folgenden drei Schritte:

1. Den Webdienst SDK installieren
2. Installieren Sie das Benutzerportal
3. Konfigurieren der Benutzer Portal Server Azure mehrstufige Authentifizierung


### <a name="install-the-web-service-sdk"></a>Den Webdienst SDK installieren

Wenn Azure mehrstufige Authentifizierung Web Service SDK auf Azure mehrstufige Authentifizierungsserver nicht installiert ist, fahren Sie mit dem Server und öffnen Sie Azure mehrstufige Authentifizierungsserver. Klicken Sie auf das Symbol Web Service SDK, installieren Sie Web Service SDK... Befolgen Sie die Anweisungen und klicken. Ein SSL-Zertifikat muss Web Service SDK geschützt werden. Ein selbstsigniertes Zertifikat ist für diesen Zweck gut Speicher "Vertrauenswürdige Stammzertifizierungsstellen" des lokalen Computerkontos auf dem Webserver User Portal importiert werden, damit sie dieses Zertifikat vertrauen Wenn die SSL-Verbindung initiieren muss.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Installieren Sie das Benutzerportal

Vor der Installation werden User Portal auf einem separaten Server sollten Sie Folgendes beachten:

- Es ist hilfreich, einen Webbrowser für den Internetzugriff Webserver öffnen und navigieren Sie zu der URL des Web Service SDK, das in der Datei web.config eingetragen wurde. Wenn der Browser erfolgreich an den Webdienst erhalten, sollten Sie Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort in der Datei web.config eingegeben wurden, genau wie in der Datei angezeigt. Stellen Sie sicher, dass kein Zertifikat Warn- oder Fehlermeldungen angezeigt werden.
- Wenn ein reverse-Proxyserver oder Firewall vor dem User Portal-Webserver sitzt SSL-Abladung, Sie können die Datei web.config User Portal bearbeiten und fügen Sie den folgenden Schlüssel, die <appSettings> Abschnitt User Portal http statt Https verwenden können. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>User Portal installieren

1. Öffnen Sie Windows Explorer auf dem Server von Azure mehrstufige Authentifizierung und den Ordner dem Azure mehrstufige Authentifizierungsserver installiert ist (z.B. C:\Program Files\Multi-Factor Authentication Server). Wählen Sie die 32-Bit- oder 64-Bit-Version der MultiFactorAuthenticationUserPortalSetup Installationsdatei entsprechend der Server, der auf User Portal installiert werden. Kopieren Sie die Installationsdatei auf den Internetzugriff Server.
2. Die Setupdatei muss auf dem Webserver Internetzugriff mit Administratorrechten ausgeführt werden. Die einfachste Möglichkeit hierzu ist, öffnen Sie ein Eingabeaufforderungsfenster als Administrator und navigieren Sie zum Speicherort, in dem die Datei kopiert wurde.
3. Führen Sie die Datei MultiFactorAuthenticationUserPortalSetup64 installieren, ändern Sie Website und virtuelles Verzeichnis Namen bei Bedarf.
4. Nach Abschluss der Installation User Portal, C:\inetpub\wwwroot\MultiFactorAuth (oder entsprechenden Verzeichnis auf den Namen des virtuellen Verzeichnisses basiert) suchen und die Datei web.config bearbeiten.
5. Suchen Sie den Schlüssel USE_WEB_SERVICE_SDK, und ändern Sie den Wert von False auf True. Suchen Sie den Schlüssel WEB_SERVICE_SDK_AUTHENTICATION_USERNAME und WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD und legen die Werte auf den Benutzernamen und das Kennwort für das Dienstkonto, das die Sicherheit PhoneFactor Admins angehört (siehe Abschnitt Vorschriften). Geben Sie den Benutzernamen und das Kennwort zwischen den Anführungszeichen am Ende der Zeile ein (Wert = "" / >). Es wird empfohlen, einen vollqualifizierten Benutzernamen (Domäne\Benutzername oder Computer\Benutzername) verwenden
6. Suchen Sie die Pfup_pfwssdk_PfWsSdk-Einstellung, und ändern Sie den Wert von "Http://localhost:4898/PfWsSdk.asmx" auf den URL des Web Service SDK, das auf den Azure mehrstufige Authentifizierungsserver (z.B. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) ausgeführt wird. Da für diese Verbindung SSL verwendet wird, müssen Sie das SSL-Zertifikat wird für den Server ausgestellt wurde und die URL muss der Name auf dem Zertifikat Web Service SDK nach Servername und keine IP-Adresse verweisen. Wenn der Servername nicht in eine IP-Adresse vom Server Internetzugriff auflöst, fügen Sie einen Eintrag zur Hosts-Datei auf dem Server, den Namen des Servers Azure mehrstufige Authentifizierung seiner IP-Adresse zuzuordnen. Speichern Sie die Datei web.config nach wurde geändert.
7. Wenn die Website User Portal unter (z. B. Standardwebsite) installiert nicht bereits eine öffentlich signierte Zertifikat, installieren Sie das Zertifikat auf dem Server installiert, öffnen Sie IIS-Manager und binden Sie das Zertifikat an die Website.
8. Öffnen Sie einen Webbrowser von jedem Computer aus, und navigieren Sie zu der URL dem User Portal installiert wurde (z.B. https://www.publicwebsite.com/MultiFactorAuth). Stellen Sie sicher, dass kein Zertifikat Warn- oder Fehlermeldungen angezeigt werden.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren der User Portal in Azure mehrstufige Authentifizierungsserver
Das Portal installiert ist, müssen Sie Azure mehrstufige Authentifizierungsserver das Portal zu konfigurieren.

Azure Multi-Factor Authentication Server bietet verschiedene Optionen für das Benutzerportal.  Die folgende Tabelle enthält eine Liste dieser Optionen und eine Erklärung der deren Verwendung.

Webportal User Settings|Beschreibung|
:------------- | :------------- |
User Portal-URL| Geben den URL, das Portal gehostet wird ermöglicht.
Authentifizierung| Ermöglicht die Authentifizierung bei der Anmeldung auf das Portal geben.  Windows, Radius oder LDAP-Authentifizierung.
Benutzer anmelden|Geben Sie Benutzernamen und Kennwort auf der Anmeldeseite für User Portal ermöglicht.  Wenn diese Option nicht aktiviert ist, werden die Felder abgeblendet.
Benutzeranmeldung zulassen|Anwender zu mehrstufige Authentifizierung durch zu Setup-Bildschirm, der Weitere Informationen wie Telefonnummer aufgefordert werden.  Aufforderung für Ersatzrufnummer Benutzer eine zweite Rufnummer angeben kann.  Aufforderung für Drittanbieter-Anwender EID Token 3rd Party EID Zeichen angeben.
Einmalige umgehen starten zulassen| Dies ermöglicht eine einmalige umgehen initiieren.  Wenn Benutzer diesen nehmen Sie es das nächste Mal beeinflussen meldet den Benutzer.  Fragen zur Umgehung Sekunden bietet dem Benutzer ein Standardwert von 300 Sekunden ändern können.  Andernfalls ist nur einmal umgehen für 300 Sekunden.
Benutzer auswählen| Ermöglicht Benutzern, ihre primäre Kontaktmethode angeben.  Dies ist Telefonanruf, SMS, mobile app oder EID Token.
Benutzer können Sprache|  Ermöglicht dem Benutzer die Sprache ändern, die für den Anruf, SMS, mobile app oder EID-Token verwendet wird.
Benutzer können mobile Anwendung aktivieren| Ermöglicht den Benutzern generiert einen Aktivierungscode um die mobile Anwendung Aktivierung abzuschließen, die mit dem Server verwendet wird.  Sie können auch die Anzahl der Geräte festlegen, denen sie diese aktivieren können.  Zwischen 1 und 10.
Fragen zur Sicherheit für Fallback verwenden|Können Sie mit Sicherheitsfragen bei mehrstufige Authentifizierung fehlschlägt.  Sie können die Anzahl der Fragen, die erfolgreich beantwortet werden müssen.
Benutzer können von Drittanbietern EID Token zuordnen| Ermöglicht ein Drittanbieter-EID Token an.
Verwenden Sie EID Token für Ersatz|Ermöglicht die Verwendung eines EID Token, mehrstufige Authentifizierung nicht erfolgreich ist.  Sie können auch das Sitzungstimeout in Minuten angeben.
Aktivieren der Protokollierung|Aktiviert die Protokollierung der User Portal.  Die Protokolldateien befinden sich: C:\Program Files\Multi-Faktor-Authentifizierung Server\Logs.

Die meisten Optionen sind sichtbar, nachdem sie aktiviert sind und der Benutzer Zeichen in User Portal.

![Portal Einstellungen](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>An die User Portal Azure mehrstufige Authentifizierungsserver konfigurieren




1. Klicken Sie auf User Portal in Azure mehrstufige Authentifizierungsserver. Geben Sie die URL auf der Registerkarte User Portal im Textfeld Benutzer Portal-URL. Dieser URL wird in e-Mails eingefügt werden, die an Benutzer gesendet werden, wenn sie in Azure mehrstufige Authentifizierungsserver importiert werden, wenn das e-Mail-aktiviert.
2. Einstellungen Sie im User Portal verwenden möchten. Wenn Benutzer ihre Authentifizierungsmethoden steuern, dass Benutzern erlauben, Methode werden zusammen mit den Methoden können sie z. B. aus auswählen.
3. Klicken Sie auf den Link Hilfe in der oberen rechten Ecke für Hilfe Einstellungen angezeigt.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Administratoren-Registerkarte
Auf dieser Registerkarte können einfach Benutzer hinzufügen, die über Administratorrechte verfügen.  Wenn Sie Administrator hinzufügen, können Sie die Berechtigungen Feinabstimmung, die sie erhalten.  Auf diese Weise kann dem Administrator nur die benötigten Berechtigungen gewähren dabei.  Klicken Sie einfach auf die Schaltfläche hinzufügen und dann Benutzer und ihre Berechtigungen, und klicken Sie auf Hinzufügen.

![Webportal Benutzeradministratoren](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Fragen zur Sicherheit
Auf dieser Registerkarte können Sie Fragen zur Sicherheit an, denen Benutzer benötigen zur Beantwortung mit Sicherheitsfragen fallback-Option ausgewählt wurde.  Azure Mehrfaktor-Authentifizierung Server enthält Standard-Fragen, die Sie verwenden können.  Sie können auch die Reihenfolge ändern oder eigene Fragen hinzufügen.  Beim Hinzufügen von Fragen können Sie die Sprache angeben, diese Frage in angezeigt werden soll.

![Portal Benutzerfragen](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Übergebene Sessions

## <a name="saml"></a>SAML
Können Sie das User Portal Ansprüche vom Identitätsanbieter mit SAML einrichten.  Sie können Session Timeout festlegen, Verifizierungszertifikat sowie die URL umleiten.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen
Auf dieser Registerkarte können Sie einzelne IP-Adressen oder IP-Adressbereiche, die hinzugefügt werden können, wenn ein Benutzer eines dieser IP-Adressen anmelden, mehrstufige Authentifizierung umgangen wird angeben.

![User Portal vertrauenswürdigen IP-Adressen](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Self-Service-Registrierung
Wenn der Benutzer zum Anmelden und registrieren wählen Sie Benutzer für die Anmeldung in und Registrierungsoptionen Benutzer zulassen. Beachten Sie, dass die gewählte Einstellung anmelden Benutzeroberfläche auswirken.

Z. B. wenn ein Benutzer User Portal anmelden und auf die Schaltfläche Anmelden klickt, sie dann Azure mehrstufige Authentifizierung Benutzer Setup-Seite aufgerufen.  Je nach Konfiguration Azure mehrstufige Authentifizierung haben kann Benutzer ihre Authentifizierungsmethode auswählen können.  

Wenn sie wählen Sie die Authentifizierungsmethode Telefongespräch oder vorkonfigurierte diese verwenden, fordert die Seite den Benutzer zur Eingabe ihrer Telefonnummer und Erweiterung, falls zutreffend.  Sie können auch eine Ersatzrufnummer eingeben dürfen.  

![User Portal vertrauenswürdigen IP-Adressen](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Wenn der Benutzer eine PIN verwenden, wenn sie sich authentifizieren muss, wird die Seite auch eine PIN aufgefordert.  Geben Sie ihre Telefonnummer(n) und PIN (falls zutreffend), klickt der Benutzer die rufen jetzt authentifizieren Schaltfläche.  Azure mehrstufige Authentifizierung führen eine Telefongespräch Authentifizierung primäre Telefonnummer des Benutzers.  Der Benutzer muss den Anruf zu beantworten und PIN (falls zutreffend) und drücken Sie # weiter zum nächsten Schritt des Prozesses Registrierung selbst.   

Der Benutzer wählt die Authentifizierungsmethode SMS-Text oder bereits vorkonfiguriert, diese Methode zu verwenden, fordert die Seite Benutzer für ihre Mobiltelefonnummer.  Wenn der Benutzer eine PIN verwenden, wenn sie sich authentifizieren muss, wird die Seite auch eine PIN aufgefordert.  Geben Sie ihre Telefonnummer und PIN-Nummer (falls zutreffend), klickt der Benutzer den Text jetzt authentifizieren Schaltfläche.  Azure mehrstufige Authentifizierung führt eine Authentifizierung SMS an das Mobiltelefon des Benutzers.  Der Benutzer muss SMS enthält eine One Time Code (OTP) und Antwort auf die Nachricht mit diesem OTP PIN ggf. empfangen) weiter zum nächsten Schritt des Prozesses Registrierung selbst.

![User Portal SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Wenn der Benutzer die Mobile Anwendung Authentifizierungsmethode wählt oder vorkonfigurierte diese verwenden, fordert die Seite den Benutzer zu app Azure mehrstufige Authentifizierung auf ihrem Gerät generiert einen Aktivierungscode.  Nach der Installation der app Azure mehrstufige Authentifizierung klickt der Benutzer die Aktivierungscode generieren.    

>[AZURE.NOTE]Anwendung der Azure mehrstufige Authentifizierung muss Benutzer Pushbenachrichtigungen für ihr Gerät aktivieren.

Die Seite zeigt dann einen Aktivierungscode und eine URL mit einem Barcode Bild.  Wenn der Benutzer eine PIN verwenden, wenn sie sich authentifizieren muss, wird die Seite auch eine PIN aufgefordert.  Der Benutzer gibt den Aktivierungscode und die URL in Azure mehrstufige Authentifizierung app oder Barcode-Scanner zum Scannen Barcode-Bild verwendet und klickt auf die Schaltfläche aktivieren.    

Nachdem die Aktivierung abgeschlossen ist, klickt der Benutzer die Schaltfläche jetzt authentifizieren.  Azure mehrstufige Authentifizierung führt eine Authentifizierung der Benutzer mobilen Anwendung.  Der Benutzer muss Geben Sie ihre PIN-Nummer (falls zutreffend) und Schaltfläche in ihrer mobilen Anwendung weiter zum nächsten Schritt des Prozesses Registrierung selbst authentifizieren.  


Wenn Administratoren Azure mehrstufige Authentifizierungsserver sammeln Fragen und Antworten konfiguriert haben, wird der Benutzer dann Fragen zur Sicherheit übernommen.  Benutzer wählen vier Fragen und Antworten auf ihre ausgewählten Fragen.    

![Portal Benutzerfragen](./media/multi-factor-authentication-get-started-portal/secq.png)  

Die benutzerregistrierung selbst ist jetzt vollständig, und der Benutzer an die User Portal angemeldet ist.  Benutzer können User Portal zu einem beliebigen Zeitpunkt in der Zukunft, ihre Telefonnummern, Stifte, Authentifizierungsmethoden und Sicherheitsfragen, wenn von den Administratoren anmelden.
