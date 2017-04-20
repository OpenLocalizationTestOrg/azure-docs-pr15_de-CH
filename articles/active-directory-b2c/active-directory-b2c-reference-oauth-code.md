<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Erstellen von ASP.NET-Webanwendungen mit Azure Active Directory-Implementierung des Authentifizierungsprotokolls OpenID verbinden."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 Authorization Codefluss

OAuth 2.0 Authorization Code gewähren kann in apps verwendet werden, die auf einem Gerät Zugriff auf geschützte Ressourcen wie APIs installiert sind. Mithilfe von Azure Active Directory (Azure AD) B2C-Implementierung von OAuth 2.0 können Sie Ihre mobilen und desktop-apps Identität Anmeldung anmelden und andere Aufgaben hinzufügen. Dieses Handbuch ist sprachunabhängig. Es beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne unsere Open-Source-Bibliotheken.

<!-- TODO: Need link to libraries -->

OAuth 2.0 Authorization Code Flow wird in [Abschnitt 4.1 OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben. Sie können in den meisten app-Typen, einschließlich [Web-apps](active-directory-b2c-apps.md#web-apps) und [Systemeigen installierten apps](active-directory-b2c-apps.md#mobile-and-native-apps)Authentifizierung und Autorisierung durchzuführen. Sie können apps zu sicheren **Access_tokens**, die verwendet werden kann, auf Ressourcen zuzugreifen, die eine [autorisierungsserver](active-directory-b2c-reference-protocols.md#the-basics)gesichert werden.

Dieses Handbuch konzentriert sich auf einen bestimmten Geschmack OAuth 2.0 Authorization Code Flow -**Öffentliche Auftraggeber**. Ein öffentlicher Client ist jeder Clientanwendung sichere Integrität eine geheime vertraut werden kann. Dazu gehören mobiler apps, desktop-apps und ziemlich jeder Anwendung, die auf einem Gerät ausgeführt wird und zu Access_tokens. Wenn Sie mithilfe von Azure AD B2C Identitätsmanagement Web app hinzufügen möchten, verwenden Sie [OpenID verbinden](active-directory-b2c-reference-oidc.md) als OAuth 2.0.

Azure AD B2C erweitert den Standard-OAuth 2.0 fließt um mehr als einfache Authentifizierung und Autorisierung. Führt der [**Richtlinienparameter**](active-directory-b2c-reference-policies.md)OAuth 2.0 hinzuzufügende Benutzer Ihrer Anwendung wie ermöglicht Anmeldung anmelden und Verwaltung von Profilen. Hier zeigen wir wie OAuth 2.0 und Richtlinien verwenden, um jede dies in einer systemeigenen Anwendung implementieren. Wir zeigen auch das Access_tokens für den Zugriff auf Web-APIs abrufen.

Die folgenden Beispiel HTTP-Anfragen verwendet unser Beispiel B2C-Verzeichnis, **fabrikamb2c.onmicrosoft.com**und unser Beispiel sowie Richtlinien. Sie können die Anfragen selbst versuchen, diese Werte oder durch Ihren eigenen ersetzen.
Erfahren Sie, wie man [Eigene B2C-Verzeichnis, Anwendung und Richtlinien](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. einen Autorisierungscode abrufen
Authorization Code Flow beginnt mit dem Client Weiterleiten des Benutzers die `/authorize` Endpunkt. Ist der interaktiven Teil der Fluss, führt der Benutzer tatsächlich Aktion In dieser Anforderung gibt der Client die Berechtigungen, die vom Benutzer in erwerben muss die `scope` Parameter und Richtlinien zum Ausführen in der `p` Parameter. Drei Beispiele unten (Zeilenumbrüche Lesbarkeit) mit einer anderen Richtlinie erhalten.

#### <a name="use-a-sign-in-policy"></a>Verwenden Sie eine Richtlinie

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Verwenden Sie eine Registrierungsrichtlinie

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Verwenden Sie eine Richtlinie Profil bearbeiten

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Erforderlich | Die-ID, die Ihre app [Azure-Portal](https://portal.azure.com) zugewiesen. |
| Antworttyp | Erforderlich | Der Antworttyp, einschließlich `code` für die Autorisierung Code Flow. |
| redirect_uri | Erforderlich | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen. Es muss exakt eine Redirect_uris, die im Portal registriert, URL-codiert werden. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen. Bereich gibt an beiden Azure AD Berechtigungen angefordert werden. Verwenden die Client-ID als Bereich gibt an, dass Ihre Anwendung ein **Token zuzugreifen** , das gegen Service oder Web-API verwendet werden kann, dargestellt durch dieselbe Client-ID  Die `offline_access` Bereich gibt an, dass Ihre Anwendung ein **Aktualisierungstoken** für langlebige Ressourcen benötigen.  Sie können auch die `openid` Bereich ein **ID-Token** von Azure AD B2C anfordern. |
| response_mode | Empfohlen | Die Methode, die verwendet werden soll, die resultierenden Authorization_code für Ihre Anwendung zu senden. 'Abfrage', 'Form_post' oder 'fragmentieren' möglich.
| Zustand | Empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden. Eine Zeichenfolge von Inhalten möglich, die Sie möchten. Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen siteübergreifende Anforderungsfälschungsangriffe verwendet. Der Status wird auch zum Codieren von Informationen der Benutzer in der app vor die Authentifizierung wird die Richtlinie ausgeführt wird oder die Seite auf, verwendet. |
| p | Erforderlich | Die Richtlinie ausgeführt wird. Es ist der Name einer Richtlinie, die in Ihrem B2C-Verzeichnis erstellt wird. Der Richtlinienwert Namen sollte mit beginnen "b2c\_1\_". Erfahren Sie mehr über Richtlinien in [Extensible Politik](active-directory-b2c-reference-policies.md). |
| Aufforderung | Optionale | Der Typ von Benutzerinteraktion erforderlich ist. Zu diesem Zeitpunkt der einzige gültige Wert ist 'Anmeldung', die dem Benutzer die Anforderung ihre Anmeldeinformationen eingeben. Einmaliges Anmelden wird wirksam. |

Zu diesem Zeitpunkt wird der Benutzer aufgefordert die Richtlinie Workflows. Dies kann den Benutzer ihren Benutzernamen und Kennwort anzumelden sozialen Identität anmelden für das Verzeichnis oder eine beliebige Anzahl Schritte je nach die Richtlinie Definition.

Nach Abschluss der Benutzer die Richtlinie zurück Azure AD auf Ihre Anwendung auf die `redirect_uri`, mit der Methode der `response_mode` Parameter. Die Antwort wird genau für jeden der oben genannten Fälle, unabhängig von der Richtlinie übereinstimmen, die ausgeführt wurde.

Eine erfolgreiche Antwort verwendet `response_mode=query` aussieht:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Code | Die Authorization_code, die die Anwendung angefordert. Die app können den Autorisierungscode ein Zugriffstoken für eine Ressource anfordern. Authorization_codes sind sehr kurzlebig. In der Regel laufen sie nach etwa 10 Minuten. |
| Zustand | Siehe vollständige Beschreibung in der vorherigen Tabelle. Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |

Fehlerantworten auch zugesandt werden die `redirect_uri` , damit die Anwendung diese angemessen behandelt werden kann:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die dienen zum Klassifizieren der Arten von Fehlern, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, die einen Entwickler die Ursache in einem Authentifizierungsfehler zu helfen. |
| Zustand | Siehe vollständige Beschreibung in der ersten Tabelle in diesem Abschnitt. Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |


## <a name="2-get-a-token"></a>2. einen Token abrufen
Damit eine Authorization_code erworben haben, lösen Sie die `code` für ein Token für die gewünschte Ressource senden ein `POST` Anfordern der `/token` Endpunkt. In Azure AD B2C ist die einzige Ressource, der einen Token anfordern kann Ihre app eigene Back-End-Web-API. Übereinkommen, mit denen sich einen Token angefordert ist Ihre app Client-ID als Bereich:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| p | Erforderlich | Die Richtlinie, die zu den Autorisierungscode verwendet wurde. Eine andere Richtlinie können keine in dieser Anforderung. Beachten Sie, dass dieser Parameter in der *Abfragezeichenfolge*nicht in den POST-Text hinzufügen. |
| client_id | Erforderlich | Die-ID, die Ihre app [Azure-Portal](https://portal.azure.com) zugewiesen. |
| grant_type | Erforderlich | Den Typ des Zuschusses, die `authorization_code` für die Autorisierung Code Flow. |
| Gültigkeitsbereich | Empfohlen | Eine durch Leerzeichen getrennte Liste von Bereichen. Bereich gibt an beiden Azure AD Berechtigungen angefordert werden. Verwenden die Client-ID als Bereich gibt an, dass Ihre Anwendung ein **Token zuzugreifen** , das gegen Service oder Web-API verwendet werden kann, dargestellt durch dieselbe Client-ID  Die `offline_access` Bereich gibt an, dass Ihre Anwendung ein **Aktualisierungstoken** für langlebige Ressourcen benötigen.  Sie können auch die `openid` Bereich ein **ID-Token** von Azure AD B2C anfordern. |
| Code | Erforderlich | Die Authorization_code, die Sie in die erste Etappe des Flusses erworben. |
| redirect_uri | Erforderlich | Die Redirect_uri die Authorization_code erhielt Anwendung. |

Eine erfolgreiche token Antwort sieht wie:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| not_before | Der Zeitpunkt, an dem Token, Epoche Zeitpunkt gültig ist. |
| token_type | Der Wert des Tokentyps. Der einzige Typ Azure AD unterstützt ist Träger. |
| Zugriffstoken | Die angeforderte signierte JSON Web Token (JWT)-Token. |
| Gültigkeitsbereich | Die Bereiche, denen das Token gültig ist, ist die Token zur späteren Verwendung Zwischenspeichern verwendet werden kann. |
| expires_in | Die Zeitspanne (in Sekunden) das Token gilt. |
| Aktualisierungstoken | Ein Aktualisierungstoken OAuth 2.0. Dieses Token können die Anwendung weitere Tokens nach Ablauf des aktuellen Tokens bekommen. Aktualisierungstoken lange lebten und können verwendet werden, um Zugriff auf Ressourcen für längere Zeit beibehalten. Weitere Details finden Sie unter [B2C Tokenverweis](active-directory-b2c-reference-tokens.md). |

Fehlerantworten aussehen werden:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die dienen zum Klassifizieren der Arten von Fehlern, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, die einen Entwickler die Ursache in einem Authentifizierungsfehler zu helfen. |

## <a name="3-use-the-token"></a>3. verwenden Sie das token
Nun, dass Sie bereits erfolgreich gekauft haben eine `access_token`, können Sie das Token in Anfragen an die Back-End-web-APIs durch Einschließen in die `Authorization` Header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. das Token aktualisieren
Access & ID-Token sind kurzlebige. Sie aktualisieren müssen, nachdem ablaufen, um weiterhin Zugriff auf Ressourcen. Dazu Übermitteln einer anderen `POST` anfordern, die `/token` Endpunkt. Dieses Mal die `refresh_token` statt der `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | -------- |
| p | Erforderlich | Die Richtlinie, die zu der ursprünglichen Aktualisierungstoken verwendet wurde. Eine andere Richtlinie können keine in dieser Anforderung. Beachten Sie, dass dieser Parameter in der *Abfragezeichenfolge*nicht in den POST-Text hinzufügen. |
| client_id | Empfohlen | Die-ID, die Ihre app [Azure-Portal](https://portal.azure.com) zugewiesen. |
| grant_type | Erforderlich | Den Typ des Zuschusses, die `refresh_token` für dieses Abschnitts Authorization Code Flow. |
| Gültigkeitsbereich | Empfohlen | Eine durch Leerzeichen getrennte Liste von Bereichen. Bereich gibt an beiden Azure AD Berechtigungen angefordert werden. Verwenden die Client-ID als Bereich gibt an, dass Ihre Anwendung ein **Token zuzugreifen** , das gegen Service oder Web-API verwendet werden kann, dargestellt durch dieselbe Client-ID  Die `offline_access` Bereich gibt an, dass Ihre Anwendung ein **Aktualisierungstoken** für langlebige Ressourcen benötigen.  Sie können auch die `openid` Bereich ein **ID-Token** von Azure AD B2C anfordern. |
| redirect_uri | Optionale | Die Redirect_uri die Authorization_code erhielt Anwendung. |
| Aktualisierungstoken | Erforderlich | Die ursprüngliche Aktualisierungstoken, die in der des Flusses erworben. |

Eine erfolgreiche token Antwort sieht wie:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| not_before | Der Zeitpunkt, an dem Token, Epoche Zeitpunkt gültig ist. |
| token_type | Der Wert des Tokentyps. Der einzige Typ Azure AD unterstützt ist Träger. |
| Zugriffstoken | Die angeforderte signierte JSON Web Token (JWT)-Token. |
| Gültigkeitsbereich | Die Bereiche, denen das Token gültig ist, ist die Token zur späteren Verwendung Zwischenspeichern verwendet werden kann. |
| expires_in | Die Zeitspanne (in Sekunden) das Token gilt. |
| Aktualisierungstoken | Ein Aktualisierungstoken OAuth 2.0. Dieses Token können die Anwendung weitere Tokens nach Ablauf des aktuellen Tokens bekommen. Aktualisierungstoken lange lebten und können verwendet werden, um Zugriff auf Ressourcen für längere Zeit beibehalten. Weitere Details finden Sie unter [B2C Tokenverweis](active-directory-b2c-reference-tokens.md). |

Fehlerantworten aussehen werden:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann. |

## <a name="use-your-own-b2c-directory"></a>Eigene B2C-Verzeichnis verwenden

Wenn Sie diese Anfragen selbst ausprobieren möchten, müssen zuerst gehen drei und Beispielwerte oben durch Ihren eigenen ersetzen:

- [Erstellen ein B2C-Verzeichnis](active-directory-b2c-get-started.md) und den Namen des Verzeichnisses der Anfragen verwenden.
- [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) eine ID und einen Redirect_uri. Sie möchten **native Client** in Ihre Anwendung aufnehmen.
- [Erstellen Sie Ihre Richtlinien](active-directory-b2c-reference-policies.md) zu den Richtliniennamen.
