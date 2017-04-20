
<properties
    pageTitle="Azure AD v2. 0 OAuth Client Anmeldeinformationen Fluss | Microsoft Azure"
    description="Erstellen von ASP.NET-Webanwendungen mit Azure AD-Implementierung des Authentifizierungsprotokolls OAuth 2.0."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>v2. 0 Protokolle - OAuth 2.0 Clientanmeldeinformationen fließen

Die [Clientanmeldeinformationen OAuth 2.0 gewähren](http://tools.ietf.org/html/rfc6749#section-4.4), manchmal auch als "zweibeinige OAuth" kann Zugriff auf Web gehosteten Ressourcen anhand der Identität einer Anwendung verwendet werden.  Es wird häufig für Server-zu-Server-Aktivitäten verwendet, die im Hintergrund ohne sofortigen Precense der Endbenutzer ausführen muss.  Diese Anwendungstypen werden häufig als **Daemons** oder **Dienstkonten**bezeichnet.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

In der häufiger drei "dreibeinige OAuth," erhält eine Anwendung Zugriff auf eine Ressource für einen bestimmten Benutzer.  Die Berechtigung ist für die Anwendung in der Regel bei der [Zustimmung](active-directory-v2-scopes.md) vom Benutzer **delegiert** .  Jedoch sind im Fluss Anmeldeinformationen Client **direkt** für die Anwendung selbst Berechtigungen.  Bei die Anwendung ein Token für eine Ressource, erzwingt die Ressource hat die app Authorization - nicht eine bestimmte Aktion ausgeführt, dass einige Benutzer eine Berechtigung besitzt.

## <a name="protocol-diagram"></a>Protokoll-Diagramm
Gesamten Anmeldeinformationen Datenfluss sieht - Schritte werden im folgenden ausführlich beschrieben.

![Client-Anmeldeinformationen Fluss](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Erhalten direkten Autorisierung 
Es gibt zwei Methoden, um eine Anwendung in der Regel direkte Berechtigung Zugriff auf eine Ressource empfängt: über eine Zugriffssteuerungsliste der Ressource oder Anwendung Berechtigungen zugewiesen in Azure AD.  Gibt es verschiedene Arten, die eine Ressource Kunden autorisieren kann, und jede Ressourcenserver können die Methode, die für ihre Anwendung am sinnvollsten ist.  Diese beiden Methoden sind die gängigsten in Azure AD und empfohlen für Clients und Ressourcen, die den Fluss der Client-Anmeldeinformationen ausführen.

### <a name="access-control-lists"></a>Zugriffssteuerungslisten
Ein bestimmte Ressourcenanbieter möglicherweise eine Autorisierung Überprüfung anhand einer Liste von IDs Anwendung kennt und eine bestimmte Zugriffsebene gewährt erzwingen.  Erhält die Ressource einen Token vom Endpunkt v2. 0, decodieren das Token und kann extrahiert die ID des Clients aus der `appid` und `iss` Ansprüche.  Sie können dann, dass vor einigen Zugriffssteuerungsliste (ACL) verwaltet.  Die Genauigkeit und die Zugriffssteuerungsliste können von Ressource zu Ressource erheblich variieren.

Allgemeine Anwendungsfall für diese ACLs test Runner für eine Anwendung oder web-api.  Das Web kann api nur eine Teilmenge der vollständigen Berechtigungen verschiedene Clients gewähren.  Aber zum Ausführen von Tests Ende zur api ein Testclient erstellt, erhält Tokens von v2. 0-Endpunkt und sendet diese an die api.  Die api kann ACL Test ID des Clients Anwendung für vollständigen Zugriff auf die gesamte Funktionalität der api.  Beachten Sie, dass haben Sie eine Liste für den Dienst nicht nur des Aufrufers überprüft werden `appid`, aber auch überprüfen, die `iss` des Tokens ist vertrauenswürdig.

Diese Berechtigung ist für Daemons und Dienstkonten, die Daten von Privatanwender mit persönlichen Microsoft-Konten zugreifen.  Daten von Unternehmen wird empfohlen, dass Sie die Vollmacht über Anwendung Perimssions erwerben.

### <a name="application-permissions"></a>Berechtigungen
Anstatt ACLs können APIs Satz **Berechtigungen** verfügen, die eine Anwendung gewährt werden können.  Eine Anwendung die Berechtigung erhält eine Anwendung von einem Administrator eines Unternehmens und kann nur auf Daten, Organisation und ihrer Mitarbeiter verwendet werden.  Microsoft Graph enthält beispielsweise verschiedene Berechtigungen:

- Lesen von Nachrichten in allen mailboxes
- Lesen und Schreiben von e-Mail-Nachrichten in allen mailboxes
- Ein Benutzer senden
- Daten lesen
- [+ mehr](https://graph.microsoft.io)

Um diese Berechtigungen in Ihrer Anwendung zu erhalten, führen Sie die folgenden Schritte aus.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Anfordern von Berechtigungen im Portal Registrierung app

- Navigieren Sie in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder [Erstellen Sie eine Anwendung](active-directory-v2-app-registration.md) für die Anwendung, falls noch nicht.  Sie müssen sicherstellen, dass Ihre Anwendung mindestens einen geheimen Schlüssel erstellt hat.
- Suchen Sie im **Direkten Berechtigungen** und Berechtigungen Sie die, die Ihre Anwendung erfordert.
- Stellen Sie sicher **zu** app-Registrierung

#### <a name="recommended-sign-the-user-into-your-app"></a>Empfohlen an: Ihre Anwendung melden Sie den Benutzer

In der Regel Berechtigungen Erstellen einer Anwendung verwendet werden, muss die app zu einer Seitenansicht, mit der der Administrator die Anwendung Berechtigungen genehmigen können.  Diese Seite kann die app Registrierungsprozesses, Teil der Einstellungen für die Anwendung oder einen dedizierten Datenfluss "Verbinden".  In vielen Fällen ist es sinnvoll, die für die Anwendung zeigen "Ansicht verbinden" nur, wenn ein Benutzer mit einer Arbeit oder Schule Microsoft-Konto angemeldet hat.

Signieren des Benutzers in der app bietet, die Verbandes, der Benutzer gehört, bevor Sie sich die Berechtigungen der Anwendung genehmigen.  Zwar nicht unbedingt erforderlich, hilft es mehr intuitiv für die Organisationseinheit Benutzer erstellen.  Anmeldung den Benutzer in folgen Sie unserer [2.0 Protokoll Lernprogramme](active-directory-v2-protocols.md)

#### <a name="request-the-permissions-from-a-directory-admin"></a>Verzeichnis-Administrator Berechtigungen anfordern

Um Berechtigungen anzufordern aus der Unternehmensadministrator nun, können Sie Benutzer v2. 0- **Endpunkt Zustimmung Admin**umleiten.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Sie die Erlaubnis möchten Directory-Pächter.  Kann im Guid oder Anzeigename bereitgestellt werden.  Wenn Sie nicht wissen, welche Mieter der Benutzer angehört und alle Mieter anmelden möchte, verwenden Sie `common`. |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| redirect_uri | Erforderlich | Die Redirect_uri auf Ihre App Verarbeiten gesendet werden soll.  Sie müssen eines der Redirect_uris exakt im Portal registriert Url codiert werden müssen und zusätzliche Pfadsegmente lassen. |
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Der Status wird zum Codieren Informationen der Benutzer in der app vor der Authentifizierung angefordert wurde oder die Seite anzeigen, die auf. |

An diesem Punkt wird Azure AD erzwungen, dass ein Tenant-Administrator anmelden kann, um die Anforderung abzuschließen.  Der Administrator aufgefordert, alle Berechtigungen direkte Anwendung genehmigen, die für Ihre Anwendung im registrierungsportal angefordert haben. 

##### <a name="successful-response"></a>Erfolgreiche Antwort
Wenn der Administrator die Berechtigungen für die Anwendung genehmigt, werden erfolgreiche Antwort:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Verzeichnis Mandanten, die Ihre Anwendung Berechtigungen gewünscht, im Guid-Format. |
| Zustand | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Der Status wird zum Codieren Informationen der Benutzer in der app vor der Authentifizierung angefordert wurde oder die Seite anzeigen, die auf. |
| admin_consent | Wird `True`. |


##### <a name="error-response"></a>Fehlermeldung
Wenn der Administrator die Berechtigungen für die Anwendung nicht genehmigen, werden fehlerhafte Antwort:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache eines Fehlers ermitteln kann.  |

Nach Erhalt eine erfolgreiche Antwort bereitstellen Endpunkt App sammelte Ihre app Berechtigungen direkte Anwendung angeforderte.  Sie können nun auf ein Token für die gewünschte Ressource anfordert.

## <a name="get-a-token"></a>Einen Token abrufen
Nachdem Sie die erforderliche Berechtigung für die Anwendung erworben haben, können Sie fortfahren mit dem Zugriffstoken für APIs setzen.  Um ein Token mit dem Client Anmeldeinformationen Grant erhalten, senden Sie eine POST-Anforderung der `/token` v2. 0-Endpunkt:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| Gültigkeitsbereich | Erforderlich | Der Wert für die `scope` in dieser Anforderung liegen den Resource Identifier (URI App-ID) der gewünschten Ressource angebracht mit den `.default` Suffix.  So z.B. Microsoft Graph gegeben sein sollten `https://graph.microsoft.com/.default`.  Dieser Wert informiert v2. 0-Endpunkt, daß alle direkten Anwendung Berechtigungen für Ihre Anwendung konfigurierten, es einen Token für die gewünschte Ressource für. |
| client_secret | Erforderlich | Die geheimen Schlüssel, den in der registrierungsportal für Ihre Anwendung generiert. |
| grant_type | Erforderlich | Muss `client_credentials`. | 

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort dauert das Formular:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Zugriffstoken | Die angeforderte Zugriffstoken. Dieses Token können die app auf geschützte Ressourcen, z. B. eine Web-API authentifizieren. |
| token_type | Gibt den Tokentyp Wert. Der einzige Typ, Azure AD unterstützt `Bearer`.  |
| expires_in | Wie lange das Zugriffstoken (in Sekunden) gültig ist. |

#### <a name="error-response"></a>Fehlermeldung
Eine Fehlerantwort dauert das Formular:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen.  |
| Zeitstempel | Der Zeitpunkt des Fehlers. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die Diagnose Komponenten helfen. |

## <a name="use-a-token"></a>Einen Token verwenden
Damit einen Token erworben haben, können Sie Tokens, die Ressource anzufordern.  Ablauf das Token, wiederholen Sie die Anforderung an die `/token` Endpunkt zu frischem Zugriffstoken.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Codebeispiel
Finden Sie ein Beispiel für eine Anwendung finden, implementiert die Client_credentials gewährt der Administrator mit Endpoint bekommen unsere [v2. 0-Daemon-Codebeispiel](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
