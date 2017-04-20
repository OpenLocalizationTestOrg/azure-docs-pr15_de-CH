<properties
    pageTitle="Azure AD Connect: Beheben von Konnektivitätsproblemen | Microsoft Azure"
    description="Erläutert die Behandlung von Verbindungsproblemen mit Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Beheben von Konnektivitätsproblemen mit Azure AD verbinden
Dieser Artikel beschreibt die Funktionsweise der Konnektivität zwischen Azure AD Connect Azure und Verbindungsproblemen. Diese Probleme werden wahrscheinlich in einer Umgebung mit einem Proxy-Server angezeigt werden.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Beheben von Konnektivitätsproblemen im Installations-Assistenten
Azure AD Connect verwendet moderne Authentifizierung (mit ADAL Bibliothek) für die Authentifizierung. Der Installations-Assistent und dem richtigen Synchronisierungsmodul erfordern machine.config .NET Applications sind ordnungsgemäß konfiguriert werden.

In diesem Artikel zeigen wir, wie Fabrikam mit Azure AD über dessen Proxy verbunden. Proxyserver heißt Fabrikamproxy und Anschluss 8080.

Zunächst müssen wir sicherstellen, dass [**"Machine.config"**](active-directory-aadconnect-prerequisites.md#connectivity) richtig konfiguriert ist.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
In einigen Blogs nicht Microsoft dokumentiert miiserver.exe.config stattdessen geändert werden soll. Diese Datei ist jedoch bei jeder Aktualisierung auch wenn funktioniert während der Erstinstallation des Systems unterbrechen wird zuerst aktualisieren überschrieben. Aus diesem Grund empfiehlt es sich, stattdessen machine.config aktualisieren.

Der Proxyserver muss auch die erforderlichen URLs geöffnet. Die offizielle Liste ist in [Office 365 URLs und IP-Adressbereiche ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)dokumentiert.

Dieser ist in der folgenden Tabelle das absolute Minimum zu Azure AD zu verbinden. Diese Liste enthält keine optionalen Funktionen wie Kennwort Rückschreiben oder Azure AD Connect Health. Es ist hier dokumentiert, um Hilfe bei der Problembehandlung für die Erstkonfiguration.

URL | Anschluss | Beschreibung
---- | ---- | ----
mscrl.Microsoft.com | HTTP-80 | Zum Downloaden von Zertifikatsperrlisten Listen verwendet.
\*. verisign.com | HTTP-80 | Zum Downloaden von Zertifikatsperrlisten Listen verwendet.
\*. entrust.com | HTTP-80 | Zum Downloaden von Zertifikatsperrlisten Listen für MFA verwendet.
\*. Windows | HTTPS-443 | Anmelden bei Azure AD verwendet.
Secure.aadcdn.microsoftonline p.com | HTTPS-443 | Für MFA verwendet.
\*. microsoftonline.com | HTTPS-443 | Zur Konfiguration von Azure AD-Verzeichnis und Importieren/Exportieren von Daten verwendet.

## <a name="errors-in-the-wizard"></a>Fehler im Assistenten
Der Installationsassistent wird zwei Sicherheitskontexte verwendet. Auf der Seite **Verbindung mit Azure AD** wird momentan angemeldeten Benutzer verwendet. Auf der Seite **Konfigurieren** ändert sie das [Konto der Dienst für das Synchronisierungsmodul ausgeführt](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts)sich. Wir stellen Proxykonfigurationen global sind auf dem Computer ist ein Problem, das Problem wird wahrscheinlich bereits **mit Azure AD** Seite im Assistenten angezeigt.

Dies sind die häufigsten Fehler finden Sie im Installations-Assistenten.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Der Installations-Assistent wurde nicht richtig konfiguriert
Dieser Fehler wird angezeigt, wenn der Assistent den Proxy nicht erreichen kann.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Wenn Sie sehen, überprüfen Sie, ob die [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) ordnungsgemäß konfiguriert wurde.
- Wenn die Ordnung der Schritte in [Proxy-Verbindung überprüfen](#verify-proxy-connectivity) , ob das Problem außerhalb der Assistenten vorhanden ist.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA-Endpunkt kann nicht erreicht werden
Dieser Fehler wird angezeigt, wenn Endpunkt **https://secure.aadcdn.microsoftonline-p.com** nicht erreicht werden kann und des globalen Administrator MFA aktiviert.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Wenn Sie sehen, überprüfen Sie, ob der Proxy die Endpunkt-secure.aadcdn.microsoftonline-p.com hinzugefügt wurde.

### <a name="the-password-cannot-be-verified"></a>Das Kennwort kann nicht bestätigt werden
Der Installations-Assistent erfolgreich mit Azure AD jedoch nicht das Kennwort selbst sehen dies überprüft werden: ![den Text Badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Das Kennwort ist ein temporäres Kennwort und muss geändert werden? Ist es tatsächlich das Kennwort? Versuchen Sie, https://login.microsoftonline.com (auf einem anderen Computer als dem Azure AD Connect Server) anmelden und überprüfen, ob das Konto verwendet werden kann.

### <a name="verify-proxy-connectivity"></a>Überprüfen Sie die Proxy-Verbindung
Zum Überprüfen, ob Azure AD Connect Server tatsächliche Verbindung mit dem Proxy und Internet nutzen wir einige PowerShell um festzustellen, ob der Proxy Anfragen zugelassen wird. Eine Aufforderung PowerShell ausführen `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Technisch der erste Aufruf ist https://login.microsoftonline.com und dies funktioniert auch, aber der URI ist schneller reagieren.)

PowerShell verwendet die Konfiguration in der Datei machine.config der Proxy an. Die Einstellungen in Winhttp-Netsh sollte keinen Einfluss auf diese Cmdlets.

Wenn der Proxy korrekt konfiguriert ist, erhalten Sie eine Erfolgsmeldung: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Erhält **mit dem remote-Server kann nicht hergestellt** dann PowerShell versucht, einen direkten Aufruf ohne Proxy oder DNS nicht ordnungsgemäß konfiguriert. Stellen Sie sicher, dass die Datei **machine.config** ordnungsgemäß konfiguriert ist.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Wenn der Proxy nicht ordnungsgemäß konfiguriert ist, erhalten wir eine Fehlermeldung: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Fehler | Fehlertext | Kommentar
---- | ---- | ---- |
403 | Verboten | Der Proxy wurde nicht für die angeforderte URL geöffnet. Die Proxy-Konfiguration erneut und sicherstellen, dass [URLs](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) geöffnet worden sein.
407 | Proxyauthentifizierung erforderlich | Der Proxyserver erforderlich anmelden und keine gab. Wenn der Proxyserver Authentifizierung erfordert, müssen Sie dies in der Datei machine.config konfiguriert. Stellen Sie außerdem sicher, dass Sie Domänenkonten für die Benutzer den Assistenten für das Dienstkonto verwenden.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Das Kommunikationsmuster zwischen Azure AD Connect Azure
Wenn Sie alle befolgt haben, diese Schritte oben und noch keine Verbindung herstellen können, können Sie jetzt beginnen am Netzwerk anmeldet. Dieser Abschnitt ist normal und erfolgreiche Verbindung Muster dokumentieren. Es ist auch allgemeine Ablenkungsmanöver auflisten, die ignoriert werden können, wenn Sie Netzwerk-Protokolle lesen.

- Aufrufe von https://dc.services.visualstudio.com werden. Es ist nicht erforderlich, dieses öffnen im Proxy für die Installation erfolgreich und ignoriert werden können.
- Sie sehen, dass DNS-Auflösung tatsächliche Hosts in der DNS-Name Space-nsatc.net und andere Namespaces unter microsoftonline.com aufgeführt. Allerdings werden alle Web Service-Requests auf den tatsächlichen Servernamen und nicht auf den Proxy hinzufügen müssen.
- Endpunkte Adminwebservice und Provisioningapi (siehe unten in den Protokollen) und Discovery Endpunkte verwendet den aktuellen Endpunkt zu verwenden und können je nach Region unterschiedlich.

### <a name="reference-proxy-logs"></a>Referenz-Proxy-Protokolle
Hier ist ein Abbild von einem tatsächlichen Proxy-Protokoll und der Seite des Installationsassistenten, an denen sie entnommen wurde (um dieselbe doppelte Einträge wurden entfernt). Dies kann als Referenz für eigene Proxy und Netzwerk-Protokolle verwendet werden. Die tatsächliche Endpunkte abweichen in Ihrer Umgebung (insbesondere in *Kursivschrift*).

**Verbinden mit Azure AD**

Zeit | URL
--- | ---
11 1 2016 8:31 | Connect://Login.microsoftonline.com:443
11 1 2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
11 1 2016 8:32 | Verbinden: / /*bba800-Anker*. microsoftonline.com:443
11 1 2016 8:32 | Connect://Login.microsoftonline.com:443
11 1 2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
11 1 2016 8:33 | Verbinden: / /*bwsc02-Relay*. microsoftonline.com:443

**Konfigurieren**

Zeit | URL
--- | ---
11 1 2016 8:43 | Connect://Login.microsoftonline.com:443
11 1 2016 8:43 | Verbinden: / /*bba800-Anker*. microsoftonline.com:443
11 1 2016 8:43 | Connect://Login.microsoftonline.com:443
11 1 2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
11 1 2016 8:44 | Verbinden: / /*bba900-Anker*. microsoftonline.com:443
11 1 2016 8:44 | Connect://Login.microsoftonline.com:443
11 1 2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
11 1 2016 8:44 | Verbinden: / /*bba800-Anker*. microsoftonline.com:443
11 1 2016 8:44 | Connect://Login.microsoftonline.com:443
11 1 2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
11 1 2016 8:46 | Verbinden: / /*bwsc02-Relay*. microsoftonline.com:443

**Erste Synchronisierung**

Zeit | URL
--- | ---
11 1 2016 8:48 | Connect://Login.Windows.NET:443
11 1 2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
11 1 2016 8:49 | Verbinden: / /*bba900-Anker*. microsoftonline.com:443
11 1 2016 8:49 | Verbinden: / /*bba800-Anker*. microsoftonline.com:443

## <a name="authentication-errors"></a>Authentifizierungsfehler
Dieser Abschnitt behandelt Fehler ADAL (Authentifizierung Library von Azure AD Connect verwendet) und PowerShell zurückgegeben werden können. Fehler erklärt sollten Ihnen in Ihre nächsten Schritte helfen.

### <a name="invalid-grant"></a>Ungültige gewähren
Benutzername oder Kennwort ungültig. Weitere Informationen finden Sie unter [Kennwort nicht überprüft werden](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Unbekannte Benutzertyp
Azure AD-Verzeichnis kann nicht gefunden oder behoben werden. Versuchen Sie, sich mit dem Benutzernamen in einer Domäne nicht überprüft?

### <a name="user-realm-discovery-failed"></a>Benutzer Realm Discovery fehlgeschlagen
Netzwerk oder Proxy Konfigurationsprobleme. Das Netzwerk nicht erreicht, finden Sie unter [Problembehandlung bei Verbindungsproblemen im Installations-Assistenten](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Kennwort abgelaufen
Die Anmeldeinformationen sind abgelaufen. Ändern Sie Ihr Kennwort ein.

### <a name="authorizationfailure"></a>AuthorizationFailure
Unbekanntes Problem.

### <a name="authentication-cancelled"></a>Authentifizierung wurde abgebrochen
Die kombinierte Authentifizierung (MFA) Herausforderung wurde abgebrochen.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Die Authentifizierung war erfolgreich, aber Azure AD PowerShell hat ein Authentifizierungsproblem hin.

### <a name="azurerolemissing"></a>AzureRoleMissing
Die Authentifizierung war erfolgreich. Sie sind kein globaler Administrator angemeldet.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Die Authentifizierung war erfolgreich. Privilegierte Identitätsmanagement wurde aktiviert, und Sie sind derzeit kein globaler Administrator angemeldet. Weitere Informationen finden Sie unter [Berechtigungen Identity Management](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Die Authentifizierung war erfolgreich. Informationen konnte von Azure AD nicht abgerufen werden.

### <a name="retrievedomains"></a>RetrieveDomains
Die Authentifizierung war erfolgreich. Domäneninformationen konnte von Azure AD nicht abgerufen werden.

## <a name="troubleshooting-steps-for-previous-releases"></a>Schritte zur Fehlerbehebung für frühere Versionen.
Versionen mit Build-Nummer 1.1.105.0 (veröffentlicht Februar 2016) wurde der Assistenten eingestellt. In diesem Abschnitt und die Konfiguration werden als Verweis nicht mehr benötigt werden

Die Single-Anmelde-Assistent funktioniert muss Winhttp konfiguriert werden. Dies kann mit [**Netsh**](active-directory-aadconnect-prerequisites.md#connectivity)erfolgen.  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Der Assistenten wurde nicht richtig konfiguriert
Dieser Fehler angezeigt, wenn der Assistenten den Proxy nicht erreichen oder der Proxy ist nicht der Anforderung.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Wenn der Proxy-Konfiguration in [Netsh](active-directory-aadconnect-prerequisites.md#connectivity) und überprüfen Sie es angezeigt.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Wenn die Ordnung der Schritte in [Proxy-Verbindung überprüfen](#verify-proxy-connectivity) , ob das Problem außerhalb der Assistenten vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
