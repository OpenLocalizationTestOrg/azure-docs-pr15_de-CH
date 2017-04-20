<properties 
    pageTitle="Erste Schritte MFA Server Mobile App-Webdienst"
    description="Azure mehrstufige Authentifizierung App bietet optional zusätzliche Out-of-Band-Authentifizierung.  Es ermöglicht den MFA Server Push-Benachrichtigung an Benutzer."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>Erste Schritte MFA Server Mobile App-Webdienst

Azure mehrstufige Authentifizierung App bietet optional zusätzliche Out-of-Band-Authentifizierung. Anstatt einen automatisierten Anruf oder SMS an den Benutzer bei der Anmeldung, legt Azure mehrstufige Authentifizierung eine Benachrichtigung auf Ihrem Smartphone oder Tablet Azure mehrstufige Authentifizierung App. Der Benutzer tippt einfach "Authentifizierung" (oder eine PIN eingibt und tippt "Authentifizierung") in der Anwendung anmelden.

Nutzung der Azure mehrstufige Authentifizierung App sind erforderlich, damit die Anwendung erfolgreich mit Mobile App-Webdienst kommunizieren kann:

- Finden Sie Hardware und Software für Hardware und software
- Sie müssen verwenden V6. 0 oder höher von Azure mehrstufige Authentifizierungsserver
- Mobile App-Webdienst installiert auf einem Internetzugriff Webserver mit Microsoft® Internet Information Services (IIS) IIS 7.x oder höher.  Weitere Informationen zu IIS finden Sie unter [IIS.NET](http://www.iis.net/).
- Sicherstellen von ASP.NET v4.0.30319 ist installiert, registriert und zugelassen
- Erforderliche Rollendienste sind ASP.NET und IIS 6-Metabase-Kompatibilität
- Mobile App-Webdienst muss über einen öffentlichen URL sein.
- Mobile App-Webdienst muss ein SSL-Zertifikat geschützt werden.
- Azure mehrstufige Authentifizierung Web Service SDK installiert in IIS 7.x oder höher auf dem Server, Azure mehrstufige Authentifizierungsserver
- Azure mehrstufige Authentifizierung Web Service SDK muss ein SSL-Zertifikat geschützt werden.
- Mobile App-Webdienst muss Azure mehrstufige Authentifizierung Web Service SDK über SSL herstellen können
- Mobile App-Webdienst dürfen zur Authentifizierung von Azure mehrstufige Authentifizierung Web Service SDK mit den Anmeldeinformationen eines Dienstkontos, das Mitglied der Sicherheitsgruppe "PhoneFactor-Admins" ist. Dieses Dienstkonto und Gruppe vorhanden in Active Directory Wenn Azure mehrstufige Authentifizierungsserver auf einem Server Domäne ausgeführt wird. Dieses Dienstkonto und Gruppe vorhanden lokal auf dem Azure mehrstufige Authentifizierungsserver, wenn er nicht Mitglied einer Domäne ist.


Installieren des Portals Benutzer auf einem Server als Azure mehrstufige Authentifizierungsserver muss die folgenden drei Schritte:

1. Den Webdienst SDK installieren
2. Mobile-app-Webdienst installieren
3. Konfigurieren Sie in Azure mehrstufige Authentifizierungsserver mobile Anwendung
4. Aktivieren von Azure mehrstufige Authentifizierung App für Endbenutzer

## <a name="install-the-web-service-sdk"></a>Den Webdienst SDK installieren

Wenn Azure mehrstufige Authentifizierung Web Service SDK auf Azure mehrstufige Authentifizierungsserver nicht installiert ist, fahren Sie mit dem Server und öffnen Sie Azure mehrstufige Authentifizierungsserver. Klicken Sie auf das Symbol Web Service SDK, installieren Sie Web Service SDK... Befolgen Sie die Anweisungen und klicken. Ein SSL-Zertifikat muss Web Service SDK geschützt werden. Ein selbstsigniertes Zertifikat ist für diesen Zweck gut Speicher "Vertrauenswürdige Stammzertifizierungsstellen" des lokalen Computerkontos auf dem Webserver User Portal importiert werden, damit sie dieses Zertifikat vertrauen Wenn die SSL-Verbindung initiieren muss.

<center>![Setup](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>Mobile-app-Webdienst installieren
Werden Sie vor der Installation des mobilen Anwendung Webdienst sollten Sie Folgendes beachten:

- Wenn Azure mehrstufige Authentifizierung User Portal auf den Internetzugriff Server installiert ist, kann der Benutzername, das Kennwort und die URL, die Web Service SDK User Portal web.config-Datei kopiert.
- Es ist hilfreich, einen Webbrowser für den Internetzugriff Webserver öffnen und navigieren Sie zu der URL des Web Service SDK, das in der Datei web.config eingetragen wurde. Wenn der Browser erfolgreich an den Webdienst erhalten, sollten Sie Anmeldeinformationen aufgefordert. Geben Sie den Benutzernamen und das Kennwort in der Datei web.config eingegeben wurden, genau wie in der Datei angezeigt. Stellen Sie sicher, dass kein Zertifikat Warn- oder Fehlermeldungen angezeigt werden.
- Wenn ein reverse-Proxyserver oder Firewall vor dem Webserver Mobile App-Webdienst sitzt SSL-Abladung, Sie können die Datei web.config Mobile App-Webdienst bearbeiten und fügen Sie den folgenden Schlüssel, die <appSettings> Abschnitt, damit Mobile App-Webdienst http statt Https verwenden können. SSL ist jedoch aus der Mobile-Anwendung auf der Firewall bzw. des Reverseproxys erforderlich. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>Mobile-app-Webdienst installieren

<ol>
<li>Öffnen Sie Windows Explorer auf Azure mehrstufige Authentifizierungsserver und den Ordner dem Azure mehrstufige Authentifizierungsserver installiert ist (z.B. C:\Program Files\Azure mehrstufige Authentifizierung). Wählen Sie die 32-Bit- oder 64-Bit-Version der Azure mehrstufige AuthenticationPhoneAppWebServiceSetup Installationsdatei entsprechend der Server, der Mobile Anwendung Webdienst installiert wird. Kopieren Sie die Installationsdatei auf den Internetzugriff Server.</li>

<li>Die Setupdatei muss auf dem Webserver Internetzugriff mit Administratorrechten ausgeführt werden. Die einfachste Möglichkeit hierzu ist, öffnen Sie ein Eingabeaufforderungsfenster als Administrator und navigieren Sie zum Speicherort, in dem die Datei kopiert wurde.</li>  

<li>Führen Sie die Installationsdatei mehrstufige AuthenticationMobileAppWebServiceSetup, Ändern der Website ggf. und das virtuelle Verzeichnis für einen kurzen Namen wie "PA". Kurze virtuellen Verzeichnisnamen wird empfohlen, da Benutzer bei Aktivierung die Mobile Anwendung Webdienst-URL in das mobile Gerät eingeben müssen.</li>

<li>Nach Abschluss der Installation von Azure mehrstufige AuthenticationMobileAppWebServiceSetup, C:\inetpub\wwwroot\PA (oder entsprechenden Verzeichnis auf den Namen des virtuellen Verzeichnisses basiert) suchen und die Datei web.config bearbeiten.</li>  

<li>Suchen Sie den Schlüssel WEB_SERVICE_SDK_AUTHENTICATION_USERNAME und WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD und legen die Werte auf den Benutzernamen und das Kennwort für das Dienstkonto, das die Sicherheit PhoneFactor Admins angehört (siehe Abschnitt Vorschriften). Möglicherweise das gleiche Konto wird die Identität des Azure mehrstufige Authentifizierung User Portal, die zuvor installiert wurden. Geben Sie den Benutzernamen und das Kennwort zwischen den Anführungszeichen am Ende der Zeile ein (Wert = "" / >). Es wird empfohlen, einen vollqualifizierten Benutzernamen (Domäne\Benutzername oder Computer\Benutzername) verwenden.</li>  

<li>Suchen Sie PfMobile App Web Service_pfwssdk_PfWsSdk Einstellung und ändern Sie den Wert von "Http://localhost:4898/PfWsSdk.asmx" in der URL des Web Service SDK, das auf den Azure mehrstufige Authentifizierungsserver (z.B. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) ausgeführt wird. Da für diese Verbindung SSL verwendet wird, müssen Sie das SSL-Zertifikat wird für den Server ausgestellt wurde und die URL muss der Name auf dem Zertifikat Web Service SDK nach Servername und keine IP-Adresse verweisen. Wenn der Servername nicht in eine IP-Adresse vom Server Internetzugriff auflöst, fügen Sie einen Eintrag zur Hosts-Datei auf dem Server, den Namen des Servers Azure mehrstufige Authentifizierung seiner IP-Adresse zuzuordnen. Speichern Sie die Datei web.config nach wurde geändert.</li>  

<li>Wenn Website Mobile App Web Service unter (z. B. Standardwebsite) installiert nicht bereits eine öffentlich signierte Zertifikat, installieren Sie das Zertifikat auf dem Server installiert, öffnen Sie IIS-Manager und binden Sie das Zertifikat an die Website.</li>  

<li>Öffnen Sie einen Webbrowser von jedem Computer aus, und navigieren Sie zu der URL dem Mobile App-Webdienst installiert wurde (z.B. https://www.publicwebsite.com/PA). Stellen Sie sicher, dass kein Zertifikat Warn- oder Fehlermeldungen angezeigt werden.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Konfigurieren Sie in Azure mehrstufige Authentifizierungsserver mobile Anwendung
Mobile-app-Webdienst installiert ist, müssen Sie Azure mehrstufige Authentifizierungsserver das Portal zu konfigurieren.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Um die mobile Anwendung in Azure mehrstufige Authentifizierungsserver konfigurieren

1. Klicken Sie auf User Portal in Azure mehrstufige Authentifizierungsserver. Wenn Benutzer ihre Authentifizierungsmethoden steuern, überprüfen Sie auf der Registerkarte Einstellungen unter Benutzern erlauben, Methode, Mobile-Anwendung. Ohne diese Funktion Endbenutzer müssen an Ihrem Helpdesk für die Mobile Anwendung aktivieren.
2. Überprüfen Sie Benutzer ein Mobile-Anwendung aktivieren.
3. Das Kontrollkästchen Sie Benutzerregistrierung zulassen.
4. Klicken Sie auf das Symbol Mobile-Anwendung.
5. Geben Sie die URL mit dem virtuellen Verzeichnis Azure mehrstufige AuthenticationMobileAppWebServiceSetup Installation erstellt wurde. Einen Kontonamen können in das Feld eingegeben werden. Dieser Firmenname wird in der mobilen Anwendung angezeigt. Wenn leer, wird der Name des Anbieters mehrstufige Authentifizierung in Azure-Verwaltungsportal erstellt angezeigt.



<center>![Setup](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
