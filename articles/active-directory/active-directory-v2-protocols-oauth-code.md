

<properties
    pageTitle="Azure AD v2. 0 OAuth Autorisierung Codefluss | Microsoft Azure"
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2. 0 Protokolle - OAuth 2.0 Autorisierungscode fließen

OAuth 2.0 Authorization Code gewähren kann in apps verwendet werden, die auf einem Gerät Zugriff auf geschützte Ressourcen wie APIs installiert sind.  App Model 2.0 Implementierung OAuth 2.0 können Sie hinzufügen anmelden und API-Zugriff auf Ihre mobilen und desktop-apps.  Dieses Handbuch ist sprachunabhängig und beschreibt die zum Senden und Empfangen von HTTP-Nachrichten ohne unsere Open-Source-Bibliotheken.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

OAuth 2.0 Authorization Code Flow ist in [Abschnitt 4.1 OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben.  Hiermit wird in den meisten app-Typen, einschließlich [Web-apps](active-directory-v2-flows.md#web-apps) und [Systemeigen installierten apps](active-directory-v2-flows.md#mobile-and-native-apps)Authentifizierung und Autorisierung durchzuführen.  Sie können apps sicher verwendet werden kann, Zugriff auf Ressourcen, die v2. 0-Endpunkt gesichert sind Access_tokens erwerben.  

## <a name="protocol-diagram"></a>Protokoll-Diagramm
Auf einer hohen Ebene sieht der gesamte Authentifizierungsablauf für systemeigene/Anwendung etwas:

![OAuth Autorisierungscode Fluss](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Anforderung einen Autorisierungscode
Authorization Code Flow beginnt mit dem Client Weiterleiten des Benutzers die `/authorize` Endpunkt.  Der Client gibt in dieser Anforderung zu vom Benutzer benötigten Berechtigungen:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Klicken Sie auf den Link unten, um diese Anforderung auszuführen. Nach der Anmeldung Ihr Browser sollte weitergeleitet `https://localhost/myapp/` mit einem `code` in der Adressleiste.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/Common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind `common`, `organizations`, `consumers`, und Mieter Bezeichner.  Weitere Informationen finden Sie unter [Protokoll Grundlagen](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| Antworttyp | Erforderlich | Muss `code` für die Autorisierung Code Flow. |
| redirect_uri | empfohlen | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen.  Sie müssen eines der Redirect_uris exakt im Portal registriert außer Url codiert werden müssen.  Für systemeigenen und mobile apps verwenden Sie den Standardwert `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von [Bereichen](active-directory-v2-scopes.md) Benutzer Zustimmung soll.  |
| response_mode | empfohlen | Gibt die Methode das resultierende token zurück an Ihre app verwendet werden soll.  Kann `query` oder `form_post`.  |
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen [siteübergreifende Anforderungsfälschungsangriffe](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen des Benutzers in die app codieren, vor der die Authentifizierungsanfrage oder die Seite auf Ansicht, verwendet. |
| Aufforderung | Optionale | Gibt den Typ von Benutzerinteraktion erforderlich ist.  Zu diesem Zeitpunkt die einzigen gültigen Werte sind 'Anmeldung', 'none' und ''.  `prompt=login`Zwingt den Benutzer zur Eingabe ihrer Anmeldeinformationen beantragt, negieren Single Sign-On.  `prompt=none`ist das Gegenteil - stellt sicher, dass der Benutzer jede interaktive Aufforderung überhaupt nicht angezeigt werden.  Wenn die Anforderung über Single Sign-On im Hintergrund ausgeführt werden kann, wird der v2. 0-Endpunkt einen Fehler zurück.  `prompt=consent`Löst Zustimmungsdialogfeld OAuth nach Benutzer, den Benutzer auffordert meldet, der Anwendung Berechtigungen. |
| login_hint | Optionale | Dienen zum Feld Benutzername/e-Mail-Adresse der Seite für den Benutzer vorab füllen sollten Sie ihren Benutzernamen voraus.  Häufig apps mithilfe dieses Parameters während der erneuten Authentifizierung müssen der Benutzername bereits aus einem früheren Anmeldung mit extrahiert die `preferred_username` anfordern. |
| domain_hint | Optionale | Kann eine der `consumers` oder `organizations`.  Wenn enthalten, überspringen Sie den e-Mail-basierte Erkennungsprozess Benutzer durchläuft auf V2. 0 anmelden, führt zu einer etwas optimierte Benutzeroberfläche.  Häufig apps verwendet dieser Parameter während der erneuten Authentifizierung durch Extrahieren der `tid` aus vorherigen anmelden.  Wenn die `tid` Wert ist `9188040d-6c67-4c5b-b112-36a304b66dad`, verwenden Sie `domain_hint=consumers`.  Andernfalls `domain_hint=organizations`. |

Zu diesem Zeitpunkt wird der Benutzer seine Anmeldeinformationen eingeben und die Authentifizierung aufgefordert.  V2. 0-Endpunkt wird auch sichergestellt, dass der Benutzer im angegebenen Berechtigungen zugestimmt hat die `scope` Parameter Abfragen.  Wenn der Benutzer alle Berechtigungen nicht zugestimmt hat, werden den Benutzer die erforderlichen Berechtigungen Zustimmung gefragt.  [Berechtigungen, Zustimmung und Multi-Tenant-apps dienen hier](active-directory-v2-scopes.md)Details.

Nachdem der Benutzer authentifiziert und Zustimmung erteilt, zurückgeben v2. 0-Endpunkt auf Ihre Anwendung auf die `redirect_uri`, mit der Methode der `response_mode` Parameter.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=query` aussieht:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Code | Die Authorization_code, die die Anwendung angefordert. Die app können den Autorisierungscode ein Zugriffstoken für die Ressource anfordern.  Authorization_codes sind sehr kurzlebige, i. d. r. nach etwa 10 Minuten ablaufen. |
| Zustand | Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |

#### <a name="error-response"></a>Fehlermeldung
Fehlerantworten auch zugesandt werden die `redirect_uri` , die Anwendung sie angemessen behandelt werden kann:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Fehlercodes für Endpunkt Autorisierungsfehlern

Die folgende Tabelle beschreibt die verschiedenen Fehlercodes in zurückgegeben werden können die `error` Parameter Fehlerreaktion.

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid_request | Fehler, wie fehlende erforderliche parameter | Korrigieren und die Anforderung erneut übermitteln. Das ist ein Fehler in der Regel während der ersten Testphase abgefangen.|
| unauthorized_client | Die Client-Anwendung ist nicht zulässig, einen Autorisierungscode anfordern. | Das Problem tritt gewöhnlich auf, wenn die Clientanwendung nicht in Azure AD registriert oder nicht Azure AD-Mandanten des Benutzers hinzugefügt. Die Anwendung kann den Benutzer für die Installation und Azure AD hinzufügen auffordern. |
| ACCESS_DENIED | Ressourcenbesitzer Zustimmung verweigert | Die Clientanwendung kann den Benutzer benachrichtigen, den fortfahren kann, wenn der Benutzer. |
| unsupported_response_type | Autorisierungsserver unterstützt nicht den Antworttyp in der Anforderung. | Korrigieren und die Anforderung erneut übermitteln. Das ist ein Fehler in der Regel während der ersten Testphase abgefangen.|
|server_error | Ein unerwarteten Fehler aufgetreten. | Wiederholen Sie die Anforderung. Dieser Fehler können aus temporären führen. Die Clientanwendung kann dem Benutzer erklären, dass aufgrund eines temporären Fehlers die Antwort verzögert. |
| temporarily_unavailable | Der Server ist vorübergehend ausgelastet und kann die Anforderung verarbeiten. | Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass aufgrund einer temporären Bedingung die Antwort verzögert. |
| invalid_resource |Die Ressource ist ungültig, da es nicht, Azure AD nicht finden oder nicht richtig konfiguriert.| Dies bedeutet, dass die Ressource existiert sie nicht in den Mandanten konfiguriert wurde. Die Anwendung kann den Benutzer für die Installation und Azure AD hinzufügen auffordern. |

## <a name="request-an-access-token"></a>Ein Zugriffstoken anfordern
Ein Authorization_code erworben haben und besitzen die Berechtigung vom Benutzer können Sie einlösen der `code` für eine `access_token` auf die gewünschte Ressource senden ein `POST` anfordern, die `/token` Endpunkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Versuchen Sie diese Anforderung in Briefträger! (Vergessen Sie nicht, ersetzen Sie die `code`)     [ ![Postbote ausführen](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind `common`, `organizations`, `consumers`, und Mieter Bezeichner.  Weitere Informationen finden Sie unter [Protokoll Grundlagen](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| grant_type | Erforderlich | Muss `authorization_code` für die Autorisierung Code Flow. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen.  In diesem Abschnitt erforderlichen Bereiche muss entspricht oder eine Teilmenge der Bereiche in der angefordert.  Wenn diese Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, kehren v2. 0-Endpunkt einen Token für die Ressource im ersten Bereich angegeben.  Eine ausführlichere Erklärung der Bereiche finden Sie unter [Berechtigungen, Zustimmung](active-directory-v2-scopes.md)und Bereiche.  |
| Code | Erforderlich | Die Authorization_code, die Sie in die erste Etappe des Flusses erworben.   |
| redirect_uri | Erforderlich | Die gleichen Redirect_uri-Wert, der verwendet wurde, um die Authorization_code zu erhalten. |
| client_secret | Web Apps erforderlich | Die geheimen Schlüssel, den im Portal Registrierung app für Ihre Anwendung erstellt.  Er sollte nicht in eine systemeigene Anwendung verwendet werden, da Client_secrets zuverlässig auf Geräten gespeichert werden kann.  Er ist erforderlich für Web-apps und Web-APIs, die die Client_secret sicher auf dem Server speichern. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche token Antwort sieht wie:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Zugriffstoken | Die angeforderte Zugriffstoken. Dieses Token können die app auf geschützte Ressourcen, z. B. eine Web-API authentifizieren. |
| token_type | Gibt den Tokentyp Wert. Ist der einzige Typ, Azure AD unterstützt  |
| expires_in | Wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Gültigkeitsbereich | Die Bereiche, denen das Zugriffstoken für gültig ist. |
| Aktualisierungstoken |  Ein Token OAuth 2.0 aktualisieren. Die Anwendung kann dieses Tokens Abrufen zusätzlicher Zugriffstoken nach Ablauf der aktuellen Zugriffstoken.  Aktualisierungstoken sind langlebig und können verwendet werden, um Zugriff auf Ressourcen für längere Zeit beibehalten.  Weitere Informationen finden Sie unter [Tokenverweis v2. 0](active-directory-v2-tokens.md).  |
| ID-Token | Eine nicht signierte JSON Web Token (JWT). Anwendung kann base64Url decodieren Segmente dieses Token zum Abrufen von Informationen über Benutzer angemeldet. Die Anwendung kann die Werte zwischengespeichert und anzeigen sollten, aber sie nicht diese Autorisierung oder Sicherheitsgrenzen.  Finden Sie weitere Informationen zu ID-Token [token Endpunktverweis v2. 0](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Fehlermeldung
Fehlerantworten aussehen werden:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

#### <a name="error-codes-for-token-endpoint-errors"></a>Fehlercodes für token Endpunkt-Fehler

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid_request | Fehler, wie fehlende erforderliche parameter | Korrigieren und die Anforderung erneut übermitteln |
| invalid_grant | Der Autorisierungscode ist ungültig oder abgelaufen. | Versuchen Sie, eine neue Anforderung an die `/authorize` Endpunkt |
| unauthorized_client | Der authentifizierte Client ist nicht berechtigt, mit diesem Zuschusstyp Autorisierung. | Das Problem tritt gewöhnlich auf, wenn die Clientanwendung nicht in Azure AD registriert oder nicht Azure AD-Mandanten des Benutzers hinzugefügt. Die Anwendung kann den Benutzer für die Installation und Azure AD hinzufügen auffordern. |
| invalid_client | Clientauthentifizierung fehlgeschlagen. | Die Clientanmeldeinformationen sind ungültig. Beheben, aktualisiert der Anwendungsadministrator die Anmeldeinformationen. |
| unsupported_grant_type | Autorisierungsserver unterstützt der Autorisierungstyp gewähren. | Ändert den Zuschuss in der Anforderung. Dieser Fehler tritt nur während der Entwicklung und während der ersten Testphase erkannt. |
| invalid_resource | Die Ressource ist ungültig, da es nicht, Azure AD nicht finden oder nicht richtig konfiguriert. | Dies bedeutet, dass die Ressource existiert sie nicht in den Mandanten konfiguriert wurde. Die Anwendung kann den Benutzer für die Installation und Azure AD hinzufügen auffordern. |
| interaction_required | Die Anforderung muss der Benutzer eingreifen. Z. B. geht eine zusätzliche Authentifizierung erforderlich. | Wiederholen Sie die Anforderung mit dem gleichen Ressourcen. |
| temporarily_unavailable | Der Server ist vorübergehend ausgelastet und kann die Anforderung verarbeiten. | Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass aufgrund einer temporären Bedingung die Antwort verzögert.|

## <a name="use-the-access-token"></a>Verwenden Sie das Zugriffstoken
Da Sie bereits erfolgreich gekauft haben eine `access_token`, können Sie das Token in Anfragen an Web-APIs durch Einschließen in die `Authorization` Header:

> [AZURE.TIP] Führen Sie diese Anforderung in Briefträger! (Ersetzen Sie die `Authorization` Kopfzeile erste)     [ ![Postbote ausführen](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Das Zugriffstoken aktualisieren
Access_tokens sind kurz lebten und muss aktualisiert werden, werden nach dem ablaufen, um weiterhin auf die Ressourcen zugreifen.  Dazu Übermitteln einer anderen `POST` anfordern, die `/token` Endpunkt, dieses Mal die der `refresh_token` statt der `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Versuchen Sie diese Anforderung in Briefträger! (Vergessen Sie nicht, ersetzen Sie die `refresh_token`)     [ ![Postbote ausführen](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | -------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind `common`, `organizations`, `consumers`, und Mieter Bezeichner.  Weitere Informationen finden Sie unter [Protokoll Grundlagen](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| grant_type | Erforderlich | Muss `refresh_token` für dieses Abschnitts Authorization Code Flow. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen.  In diesem Abschnitt erforderlichen Bereiche müssen entspricht oder eine Teilmenge der Bereiche in der ursprünglichen Authorization_code Anforderung angefordert werden.  Wenn diese Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, kehren v2. 0-Endpunkt einen Token für die Ressource im ersten Bereich angegeben.  Eine ausführlichere Erklärung der Bereiche finden Sie unter [Berechtigungen, Zustimmung](active-directory-v2-scopes.md)und Bereiche.  |
| Aktualisierungstoken | Erforderlich | Die Aktualisierungstoken, die in der des Flusses erworben.   |
| redirect_uri | Erforderlich | Die gleichen Redirect_uri-Wert, der verwendet wurde, um die Authorization_code zu erhalten. |
| client_secret | Web Apps erforderlich | Die geheimen Schlüssel, den im Portal Registrierung app für Ihre Anwendung erstellt.  Er sollte nicht in eine systemeigene Anwendung verwendet werden, da Client_secrets zuverlässig auf Geräten gespeichert werden kann.  Er ist erforderlich für Web-apps und Web-APIs, die die Client_secret sicher auf dem Server speichern. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche token Antwort sieht wie:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Zugriffstoken | Die angeforderte Zugriffstoken. Dieses Token können die app auf geschützte Ressourcen, z. B. eine Web-API authentifizieren. |
| token_type | Gibt den Tokentyp Wert. Ist der einzige Typ, Azure AD unterstützt  |
| expires_in | Wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Gültigkeitsbereich | Die Bereiche, denen das Zugriffstoken für gültig ist. |
| Aktualisierungstoken |  Ein neues Token für OAuth 2.0 aktualisieren. Ersetzen Sie das alte Aktualisierungstoken mit diesem Aktualisierungstoken neu erworbenen, um sicherzustellen, dass Ihr Aktualisierungstoken so lange gültig.  |
| ID-Token | Eine nicht signierte JSON Web Token (JWT). Anwendung kann base64Url decodieren Segmente dieses Token zum Abrufen von Informationen über Benutzer angemeldet. Die Anwendung kann die Werte zwischengespeichert und anzeigen sollten, aber sie nicht diese Autorisierung oder Sicherheitsgrenzen.  Finden Sie weitere Informationen zu ID-Token [token Endpunktverweis v2. 0](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Fehlermeldung
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

Eine Beschreibung des Fehlercodes und der empfohlene Clientaktion finden Sie unter [Fehlercodes für token Endpunkt Fehler](#error-codes-for-token-endpoint-errors).
