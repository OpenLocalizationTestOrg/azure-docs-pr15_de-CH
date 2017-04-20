<properties
    pageTitle="Übersicht über Azure AD .NET Protokoll | Microsoft Azure"
    description="Dieser Artikel beschreibt die Verwendung von HTTP-Nachrichten zum Autorisieren des Zugriffs auf Web-Applikationen und Web-APIs in Ihrem Mandanten Azure Active Directory mit OAuth 2.0."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Autorisieren des Zugriffs auf Web Applications OAuth 2.0 und Azure Active Directory verwenden

Azure Active Directory (Azure AD) verwendet OAuth 2.0 können zum Autorisieren des Zugriffs auf Web-Applikationen und Web-APIs in Azure AD-Mandanten. Dieses Handbuch ist sprachenunabhängig und beschreibt die zum Senden und Empfangen von HTTP-Nachrichten ohne unsere Open-Source-Bibliotheken.

OAuth 2.0 Authorization Code Flow wird in [Abschnitt 4.1 OAuth 2.0-Spezifikation](https://tools.ietf.org/html/rfc6749#section-4.1) beschrieben. Hiermit wird in den meisten app-Typen, einschließlich Web-apps und systemeigen installierten apps Authentifizierung und Autorisierung durchzuführen.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 Autorisierung Fluss

Auf einer hohen Ebene sieht der Fluss gesamte Autorisierung für eine Anwendung etwas:

![OAuth Autorisierungscode Fluss](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Anforderung einen Autorisierungscode

Authorization Code Flow beginnt mit dem Client Weiterleiten des Benutzers die `/authorize` Endpunkt. Der Client gibt in dieser Anforderung zu vom Benutzer benötigten Berechtigungen. Sie erhalten die Endpunkte OAuth 2.0 über Ihre Anwendung in Azure-Verwaltungsportal Schaltfläche **Ansicht Endpunkte** in der unteren Schublade.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind Mieter Bezeichner, z. B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` oder `common` für unabhängige Mieter Token |
| client_id | Erforderlich | Ihre Anwendung beim Registrieren in Azure AD zugewiesene Anwendung-ID. Sie finden diese in Azure-Verwaltungsportal. Klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung und klicken Sie dann auf **Konfigurieren** |
| Antworttyp | Erforderlich | Muss `code` für die Autorisierung Code Flow. |
| redirect_uri | empfohlen | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen.  Sie müssen eines der Redirect_uris exakt im Portal registriert außer Url codiert werden müssen.  Für systemeigenen und mobile apps verwenden Sie den Standardwert `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | empfohlen | Gibt die Methode das resultierende token zurück an Ihre app verwendet werden soll.  Kann `query` oder `form_post`.  |
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden. Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen [siteübergreifende Anforderungsfälschungsangriffe](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen des Benutzers in die app codieren, vor der die Authentifizierungsanfrage oder die Seite auf Ansicht, verwendet. |
| Ressource | Optionale | App-ID URI Web-API (gesicherte Ressource). Um App-ID-URI der Web-API in Azure-Verwaltungsportal zu suchen, klicken Sie auf **Active Directory**klicken Sie auf das Verzeichnis klicken Sie auf die Anwendung, klicken Sie auf **Konfigurieren**. |
| Aufforderung | Optionale |  Geben Sie den Benutzereingriff erforderlich ist.<p> Gültige Werte sind: <p> *Anmeldung*: sollte der Benutzer aufgefordert, erneut authentifizieren. <p> *Zustimmung*: Zustimmung erteilt, jedoch muss aktualisiert werden. Der Benutzer sollte Zustimmung aufgefordert. <p> *Admin_consent*: Administrator sollte für alle Benutzer in ihrer Organisation Zustimmung aufgefordert |
| login_hint | Optionale | Dienen zum Feld Benutzername/e-Mail-Adresse der Seite für den Benutzer vorab füllen sollten Sie ihren Benutzernamen voraus.  Häufig apps mithilfe dieses Parameters während der erneuten Authentifizierung müssen der Benutzername bereits aus einem früheren Anmeldung mit extrahiert die `preferred_username` anfordern. |
| domain_hint | Optionale | Enthält einen Hinweis auf den Mieter oder die Domäne, mit denen Benutzer anmelden muss. Der Wert der Domain_hint ist eine registrierte Domäne für den Mandanten. Wenn der Mieter in einem lokalen Verzeichnis verbunden ist, leitet AAD angegebenen Mieter Verbundserver. |

> [AZURE.NOTE] Wenn der Benutzer einer Organisation gehört, kann Administrator der Organisation Zustimmung ablehnen im Auftrag des Benutzers oder der Benutzer Zustimmung. Der Benutzer erhält die Option nur, wenn der Administrator erlaubt zuzustimmen.

An dieser Stelle der Benutzer wird aufgefordert, ihre Anmeldeinformationen und Berechtigungen gemäß stimmen die `scope` Parameter Abfragen. Sobald der Benutzer authentifiziert und Zustimmung erteilt, sendet Azure AD auf Ihre app die `redirect_uri` Adresse in der Anforderung.

### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche Antwort könnte wie folgt aussehen:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschreibung |
| -----------------------| --------------- |
| admin_consent | Der Wert ist True, wenn ein Administrator eine Zustimmung Anforderung Aufforderung zugestimmt.|
| Code | Der Autorisierungscode, den die Anwendung angefordert. Die Anwendung können den Autorisierungscode ein Zugriffstoken für die Ressource anfordern. |
| session_state | Ein eindeutiger Wert, der die Sitzung identifiziert. Dieser Wert ist eine GUID jedoch behandelt werden, als ein nicht transparenter Wert ohne Prüfung übergeben wird. |
| Zustand | Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Es empfiehlt sich für die Anwendung überprüfen, ob die Statuswerte in Anforderung und Antwort identisch sind, bevor die Antwort mit. Dadurch [Cross-Site Request Fälschung (CSRF) Angriffe](https://tools.ietf.org/html/rfc6749#section-10.12) gegen den Kunden erkennen.  

### <a name="error-response"></a>Fehlermeldung

Fehlerantworten auch zugesandt werden die `redirect_uri` , damit die Anwendung diese angemessen behandelt werden kann.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
|-----------|-------------|
| Fehler | In Abschnitt 5.2 [OAuth 2.0 Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749)definierten Wert ein Fehlercodes. Der folgenden Tabelle werden die Fehlercodes Azure AD gibt. |
| error_description | Eine detailliertere Beschreibung des Fehlers. Diese Meldung sollte nicht Endbenutzer geeignet. |
| Zustand | Der Wert ist ein zufällig generierten Wert nicht wiederverwendet, der in der Anforderung gesendet und in der Antwort zu websiteübergreifenden (CSRF) Anforderungsfälschungsangriffe zurückgegeben. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Verwenden Sie den Autorisierungscode ein Zugriffstoken anfordern

Einen Autorisierungscode erworben haben und besitzen die Berechtigung vom Benutzer können Sie den Code für ein Zugriffstoken auf die gewünschte Ressource einlösen, durch Senden einer POST-Anforderung der `/token` Endpunkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| Mieter | Erforderlich |  Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind Mieter Bezeichner, z. B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` oder `common` für unabhängige Mieter Token |
| client_id | Erforderlich | Ihre Anwendung beim Registrieren in Azure AD zugewiesene Anwendung-ID. Sie finden diese in Azure-Verwaltungsportal. Klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung und klicken Sie dann auf **Konfigurieren** |
| grant_type | Erforderlich | Muss `authorization_code` für die Autorisierung Code Flow. |
| Code | Erforderlich | Die `authorization_code` , die Sie im vorherigen Abschnitt erworben   |
| redirect_uri | Erforderlich | Dasselbe `redirect_uri` Wert, der verwendet wurde, zu den `authorization_code`. |
| client_secret | Web Apps erforderlich | Die geheimen Schlüssel, den im Portal Registrierung app für Ihre Anwendung erstellt.  Er sollte nicht in eine systemeigene Anwendung verwendet werden, da Client_secrets zuverlässig auf Geräten gespeichert werden kann.  Wird für Web-apps und Web-APIs, die die Fähigkeit zum Speichern der `client_secret` auf der Serverseite. |
| Ressource | erforderlich, wenn im Code Identifizierungsprozess, ansonsten optional angegeben | App-ID URI Web-API (gesicherte Ressource).
Um URI-ID App in Azure-Verwaltungsportal zu suchen, klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis klicken Sie auf die Anwendung und klicken Sie auf **Konfigurieren**.

### <a name="successful-response"></a>Erfolgreiche Antwort

Azure AD gibt ein Zugriffstoken auf eine erfolgreiche Antwort. Netzwerkaufrufe aus der Client-Anwendung und ihre zugeordneten Wartezeit zu minimieren, sollte die Clientanwendung Zugriffstoken für die Gültigkeitsdauer des Tokens zwischenspeichern, die auf OAuth 2.0 angegeben ist. Um die Gültigkeitsdauer des Tokens zu bestimmen, verwenden Sie die `expires_in` oder `expires_on` Parameterwerte.

Wenn eine Web API-Ressource gibt eine `invalid_token` Fehlercode möglicherweise, dass die Ressource festgestellt hat, dass das Token abgelaufen ist. Sind die Uhrzeiten Client und Ressource anderen (auch bekannt als ein "Zeitversatz"), sollten die Ressource Token abgelaufen sein, bevor das Token aus dem Clientcache gelöscht wird. In diesem Fall deaktivieren Sie das Token aus dem Cache wird es weiterhin in der berechnete Lebensdauer.

Eine erfolgreiche Antwort könnte wie folgt aussehen:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Zugriffstoken | Die angeforderte Zugriffstoken. Dieses Token können die app auf geschützte Ressourcen, z. B. eine Web-API authentifizieren. |
| token_type | Gibt den Tokentyp Wert. Der einzige Typ Azure AD unterstützt ist Träger. Weitere Informationen zu trägertoken finden Sie unter [OAuth2.0 Autorisierungsframeworks: Träger Token Verwendung (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| expires_on | Die Zeit des Zugriffstokens Ablauf. Das Datum wird als die Anzahl der Sekunden zwischen 1970 dargestellt-01-01T0:0:0Z UTC Zeitpunkt ablaufen. Dieser Wert wird verwendet, um die Gültigkeitsdauer des zwischengespeicherten Tokens zu bestimmen. |
| Ressource | App-ID URI Web-API (gesicherte Ressource).|
| Gültigkeitsbereich | Identitätswechsel Berechtigungen an die Clientanwendung. Die Standardberechtigung `user_impersonation`. Der Besitzer der gesicherten Ressource kann zusätzliche Werte in Azure AD registrieren.|
| Aktualisierungstoken |  Ein Token OAuth 2.0 aktualisieren. Dieses Token können die Anwendung weitere Zugriffstoken abrufen nach Ablauf der aktuellen Zugriffstoken.  Aktualisieren Token sind langlebig und können verwendet werden, um Zugriff auf Ressourcen für längere Zeit beibehalten. |
| ID-Token | Eine nicht signierte JSON Web Token (JWT). Anwendung kann base64Url decodieren Segmente dieses Token zum Abrufen von Informationen über Benutzer angemeldet. Die Anwendung kann die Werte zwischengespeichert und anzeigen sollten, aber sie nicht diese Autorisierung oder Sicherheitsgrenzen. |

### <a name="jwt-token-claims"></a>JWT Token Ansprüche
JWT-Token in den Wert der `id_token` Parameter kann in die folgenden Ansprüche decodiert werden:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Die `id_token` Parameter enthält die folgenden Anspruchstypen. Weitere Informationen zu JSON Web Token finden Sie in [IETF JWT-Spezifikation](http://go.microsoft.com/fwlink/?LinkId=392344). Weitere Informationen über die Tokentypen und Ansprüche lesen Sie [unterstützt und Ansprüche](active-directory-token-and-claims.md)

| Anspruchstyp | Beschreibung |
|------------|-------------|
| AUD | Zielgruppe des Tokens. Wenn das Token einer Clientanwendung ausgegeben wird, ist das Publikum die `client_id` des Clients.
| EXP | Ablaufzeit. Die Zeit, wenn das Sicherheitstoken abläuft. Für das Token gültig ist, das aktuelle(s) Datum/Uhrzeit muss kleiner oder gleich der `exp` Wert. Die Zeit wird als die Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt das Token ausgestellt wurde. |
| family_name | Benutzers letzte Name oder Name. Die Anwendung kann diesen Wert anzeigen. |
| angegebener_Name | Vorname des Benutzers. Die Anwendung kann diesen Wert anzeigen. |
| IAT | Zeitpunkt ausgegeben. Die Ausgabezeit JWT. Die Zeit wird als die Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt das Token ausgestellt wurde. |
| ISS | Identifiziert den Herausgeber des token |
| NBF | Erst mal. Die Zeit wirksam das Token wird. Für das Token gültig ist muss die aktuelle Zeit größer oder gleich dem Nbf-Wert sein. Die Zeit wird als die Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt das Token ausgestellt wurde. |
| OID | Objektbezeichner (ID) des Benutzerobjekts in Azure AD. |
| Sub | Token Antragstellerbezeichners. Dies ist eine beständige und unveränderliche Bezeichner des Benutzers, der das Token beschreibt. Verwenden Sie diesen Wert im Cache Logik. |
| TID | Mieter Kennung (ID) Azure AD-Mandanten, die das Token ausgestellt hat. |
| unique_name | Ein eindeutiger Bezeichner für diese kann dem Benutzer angezeigt werden. Dies ist normalerweise ein Benutzerprinzipalname (UPN). |
| UPN | User principal Name des Benutzers. |
| Ver | Version. Die Version des JWT-Tokens normalerweise 1.0. |

### <a name="error-response"></a>Fehlermeldung

Tokenausgabe Endpunkt Fehler sind HTTP-Fehlercodes Client token ausstellen Endpunkt direkt aufruft. Neben den HTTP-Statuscode gibt Azure AD Tokenausgabe Endpunkt ein JSON-Dokument mit Objekten, die den Fehler beschreiben.

Eine Fehlerantwort Beispiel könnte wie folgt aussehen:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen. |
| Zeitstempel | Der Zeitpunkt des Fehlers. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die Diagnose Komponenten helfen.|

#### <a name="http-status-codes"></a>HTTP-Statuscodes

Die folgende Tabelle enthält HTTP-Statuscodes, die Endpunkt token Ausgabe zurückgibt. In einigen Fällen der Fehlercode lautet die Antwort Beschreibung aber bei Fehlern müssen Begleitdokuments JSON analysiert und den Fehlercode überprüfen.

| HTTP-Code | Beschreibung |
|-----------|-------------|
| 400       | HTTP-Standardcode. In den meisten Fällen verwendet und ist normalerweise durch eine fehlerhafte Anforderung. Korrigieren und die Anforderung erneut übermitteln. |
| 401       | Authentifizierung ist fehlgeschlagen. Beispielsweise fehlt die Anforderung den Client_secret-Parameter.|
| 403       | Autorisierung fehlgeschlagen. Benutzer haben z. B. keine Zugriffsberechtigung für die Ressource. |
| 500       | Interner Fehler im Dienst. Wiederholen Sie die Anforderung. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>Verwenden Sie das Zugriffstoken für die Ressource

Da Sie bereits erfolgreich gekauft haben eine `access_token`, können Sie das Token in Anfragen an Web-APIs durch Einschließen in die `Authorization` Header. Die Spezifikation [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) erläutert trägertoken in HTTP-Anfragen auf geschützte Ressourcen zugreifen.

### <a name="sample-request"></a>Beispiel für eine Anforderung

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Fehlermeldung

Gesicherte Ressourcen, die RFC 6750 Problem HTTP-Statuscodes zu implementieren. Wenn die Anforderung keine Anmeldeinformationen enthalten, das Token fehlt die Antwort enthält eine `WWW-Authenticate` Header. Wenn eine Anforderung fehlschlägt, antwortet der Ressourcenserver mit den HTTP-Statuscode und einen Fehlercode.

Nachfolgend ein Beispiel für eine nicht erfolgreiche Antwort bei die Clientanforderung kein trägertoken enthalten:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Fehlerparameter

| Parameter | Beschreibung |
|-----------|-------------|
| authorization_uri | Der URI (physischen Endpunkt) des autorisierungsservers. Dieser Wert wird auch als Suchschlüssel verwendet, über den Server von einem Endpunkt Entdeckung informieren. <p><p> Der Client muss überprüfen, ob der autorisierungsserver vertrauenswürdig ist. Die Ressource von Azure AD geschützt, genügt zu überprüfen, ob der URL beginnt mit https://login.windows.net oder einen anderen Hostnamen Azure AD unterstützt. Eine mandantenspezifische Ressource sollte immer eine mandantenspezifische Autorisierung URI zurückgeben. |
| Fehler | In Abschnitt 5.2 [OAuth 2.0 Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749)definierten Wert ein Fehlercodes.|
| error_description | Eine detailliertere Beschreibung des Fehlers. Diese Meldung sollte nicht Endbenutzer geeignet.|
| resource_id | Gibt die eindeutige Kennung der Ressource. Die Client-Anwendung kann diese Kennung verwenden, als Wert der `resource` Parameter einen Token für die Ressource anfordert. <p><p> Es ist sehr wichtig für die Client-Anwendung um diesen Wert zu überprüfen, andernfalls ein böswilliger Dienst möglicherweise einen **Erhöhung von Berechtigungen** Angriff auslösen <p><p> Die empfohlene Strategie gegen einen Angriff ist zu überprüfen, ob die `resource_id` entspricht die Basis des Web API URL zugegriffen wird. Wenn https://service.contoso.com/data zugegriffen wird, z. B. die `resource_id` htttps://service.contoso.com/ werden können. Die Clientanwendung muss ablehnen einer `resource_id` , die beginnt nicht mit der Basis-URL ohne eine alternative zuverlässig die Geprüftes Mitglied. |

#### <a name="bearer-scheme-error-codes"></a>Träger Schema-Fehlercodes

Die RFC 6750-Spezifikation definiert die folgenden Fehler für Ressourcen, die mit der WWW-Authenticate-Header Träger Schema in der Antwort.

| HTTP-Statuscode | Fehlercode | Beschreibung | Clientaktion |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Die Anforderung ist nicht wohlgeformt. Z. B. kann es einen Parameter fehlt oder denselben Parameter verwenden. | Beheben Sie den Fehler, und wiederholen Sie die Anforderung. Dieser Fehlertyp tritt nur während der Entwicklung und erste Tests erkannt werden. |
| 401 | invalid_token   | Das Zugriffstoken fehlt, ungültig oder widerrufen. Der Wert des Parameters Error_description enthält weitere Details. |  Ein neues Token von autorisierungsserver anfordern. Schlägt das neue Token ist ein unerwarteter Fehler aufgetreten. Senden Sie eine Fehlermeldung an den Benutzer und wiederholen nach zufällige Verzögerung. |
| 403 | insufficient_scope | Das Zugriffstoken enthält keinen Identitätswechsel Berechtigungen auf die Ressource zuzugreifen. | Fordern Sie neue Autorisierung Autorisierung Endpunkt. Enthält die Antwort der Bereichsparameter verwenden Sie Bereichswert in der Anforderung für die Ressource. |
| 403 | insufficient_access | Betreff des Tokens muss nicht die Berechtigungen, die Zugriff auf die Ressource erforderlich sind. | Fordert den Benutzer auf ein anderes Konto verwenden oder Berechtigungen in die angegebene Ressource. |

## <a name="refreshing-the-access-tokens"></a>Die Zugriffstoken aktualisieren

Zugriffstoken kurzlebig sind und müssen aktualisiert werden, nachdem diese abgelaufen, um Zugriff auf Ressourcen. Aktualisieren Sie die `access_token` mit anderen `POST` anfordern, die `/token` Endpunkt, aber dieses Mal die der `refresh_token` statt der `code`.

Aktualisieren Token keinen angegebenen Gültigkeitsdauer. Die Lebensdauer der Aktualisierungstoken sind normalerweise relativ lange. Jedoch in einigen Fällen Aktualisierungstoken abgelaufen, widerrufen oder verfügen nicht über ausreichende Berechtigungen für die gewünschte Aktion. Die Anwendung benötigt und Fehler zurückgegebene token ausstellen Endpunkt ordnungsgemäß behandeln.

Beim Empfangen einer Antwort mit einem token Fehler aktualisieren aktuelle Aktualisierungstoken und einen neuen Autorisierungscode anfordern oder verwerfen Token zuzugreifen. Insbesondere wenn ein aktualisieren token im Fluss Authorization Code gewähren erhält eine Antwort mit der `interaction_required` oder `invalid_grant` Fehlercodes Aktualisierungstoken verwerfen und einen neuen Autorisierungscode anfordern.

Ein Beispiel fordern **mandantenspezifische** Endpunkt (Sie können auch den **gemeinsamen** Endpunkt) zu einer neuen Access token mithilfe Aktualisierungstoken wie folgt aussehen:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parameter | Beschreibung |
|-----------|-------------|
| Zugriffstoken | Das Zugriffstoken, das angefordert wurde.|
| expires_in   | Gültigkeitsdauer des Tokens in Sekunden. Ein Standardwert ist 3600 (eine Stunde). |
| expires_on   | Datum und Uhrzeit, an dem das Token abläuft. Das Datum wird als die Anzahl der Sekunden zwischen 1970 dargestellt-01-01T0:0:0Z UTC Zeitpunkt ablaufen. |
| Aktualisierungstoken | Eine neue OAuth 2.0-Aktualisierungstoken, mit der neue Zugriffstoken Ablauf in dieser Antwort anfordern. |
| Ressource     | Identifiziert die gesicherte Ressource, die das Zugriffstoken Zugriff verwendet. |
| Gültigkeitsbereich        | Identitätswechsel Berechtigungen native Client-Anwendung. Die Standardberechtigung ist **User_impersonation**. Der Besitzer der Ressource kann alternative Werte in Azure AD registrieren. |
| token_type   | Der Tokentyp. Der einzige unterstützte Wert ist **Träger**. |

### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche token Antwort sieht wie:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Fehlermeldung

Eine Fehlerantwort Beispiel könnte wie folgt aussehen:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen. |
| Zeitstempel | Der Zeitpunkt des Fehlers. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die Diagnose Komponenten helfen.|

Eine Beschreibung des Fehlercodes und der empfohlene Clientaktion finden Sie unter [Fehlercodes für token Endpunkt Fehler](#error-codes-for-token-endpoint-errors).
