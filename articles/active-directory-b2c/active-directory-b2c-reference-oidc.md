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

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web Anmelden mit OpenID Connect

OpenID Connect ist ein Authentifizierungsprotokoll OAuth 2.0 mit ASP.NET-Webanwendungen Benutzer sicher anmelden aufbaut.  Mithilfe von Azure Active Directory (Azure AD) B2C-Implementierung von OpenID Connect können Sie auslagern, Anmeldung, anmelden und andere Identitätsmanagement in einer Anwendung in Azure AD auftritt. Dabei wird eine sprachunabhängige Weise veranschaulicht. Es wird beschrieben, wie senden und Empfangen von HTTP-Nachrichten ohne unsere Open-Source-Bibliotheken.

[OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) erweitert das OAuth 2.0 *Authorization* -Protokoll als *Authentifizierungsprotokoll* verwendet. So können Sie mithilfe von OAuth einmaliges ausführen. Es wird ein `id_token`, ist ein Sicherheitstoken, über die der Client die Identität des Benutzers und grundlegende Informationen über den Benutzer.

Da sie OAuth 2.0 erweitert ermöglicht es apps sicher **Access_tokens**erwerben. Access_tokens auf Ressourcen zugreifen können, die eine [autorisierungsserver](active-directory-b2c-reference-protocols.md#the-basics)gesichert werden. Wir empfehlen OpenID verbinden Sie eine Anwendung erstellen, die auf einem Server gehostet und Zugriff über einen Browser. Wenn Sie mithilfe von Azure AD B2C Identitätsmanagement Mobil oder desktop-Anwendung hinzufügen möchten, verwenden Sie [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) , sondern OpenID.

Azure AD B2C erweitert das OpenID verbinden Standardprotokoll mehr als einfache Authentifizierung und Autorisierung. Führt der [**Richtlinienparameter**](active-directory-b2c-reference-policies.md)OpenID verbinden hinzuzufügende Benutzer Ihrer app - wie ermöglicht Anmeldung anmelden und Verwaltung von Profilen. Hier zeigen wir Ihnen wie OpenID verbinden und Richtlinien verwenden, um jede dieser Funktionen in einer Anwendung implementiert. Wir werden auch das Access_tokens für den Zugriff auf Web-APIs abrufen angezeigt.

Die folgenden Beispiel HTTP-Anfragen verwendet unser Beispiel B2C-Verzeichnis, **fabrikamb2c.onmicrosoft.com**sowie unser Beispiel Anwendung **https://aadb2cplayground.azurewebsites.net** und Richtlinien. Sie können die Anfragen selbst ausprobieren mithilfe dieser Werte oder durch Ihren eigenen ersetzen.
Erfahren Sie, wie Sie [Ihre eigenen B2C Mieter, Anwendung und Richtlinien](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Authentifizierung senden
Ihrer Anwendung muss den Benutzer authentifiziert und Ausführen einer Richtlinie, leiten sie Benutzer der `/authorize` Endpunkt. Ist der interaktive Teil der Fluss, in dem der Benutzer tatsächlich Aktion die Richtlinie dauert

In dieser Anforderung gibt der Client die Berechtigungen, die vom Benutzer in erwerben muss die `scope` Parameter und Richtlinien zum Ausführen in der `p` Parameter. Drei Beispiele unten (Zeilenumbrüche Lesbarkeit) mit einer anderen Richtlinie erhalten. Um ein Gefühl für jede Anforderung Funktionsweise einfügen die Anforderung in einem Browser, und starten es.

#### <a name="use-a-sign-in-policy"></a>Verwenden Sie eine Richtlinie

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Verwenden Sie eine Registrierungsrichtlinie

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Verwenden Sie eine Richtlinie Profil bearbeiten

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Erforderlich | Die-ID, die Ihre app [Azure-Portal](https://portal.azure.com/) zugewiesen. |
| Antworttyp | Erforderlich | Der Antworttyp, einschließlich `id_token` OpenID verbunden. Wenn Ihrer Anwendung auch Token wird für eine Web-API aufrufen, können Sie `code+id_token`, wie wir es getan haben. |
| redirect_uri | Empfohlen | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen. Es muss exakt eine Redirect_uris, die im Portal registriert, URL-codiert werden. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen. Bereich gibt an beiden Azure AD Berechtigungen angefordert werden. Die `openid` Bereich gibt eine Berechtigung Anmelden des Benutzers und Daten über die Benutzer **ID-Token** (mehr dazu später in diesem Artikel stammen). Die `offline_access` ist optional für Web-apps. Es gibt an, dass Ihre Anwendung ein **Aktualisierungstoken** für langlebige Ressourcen benötigen. |
| response_mode | Empfohlen | Die Methode, die verwendet werden soll, die resultierenden Authorization_code für Ihre Anwendung zu senden. 'Abfrage', 'Form_post' oder 'fragmentieren' möglich.  'Form_post' wird für eine optimale Sicherheit empfohlen. |
| Zustand | Empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden. Eine Zeichenfolge von Inhalten möglich, die Sie möchten. Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen siteübergreifende Anforderungsfälschungsangriffe verwendet. Der Status wird auch zum Codieren von Informationen der Benutzer in der app vor die Authentifizierung wird, wie die Seite, auf verwendet. |
| Nonce | Erforderlich | Ein Wert in der Anforderung (generiert durch die app), die Bestandteil der resultierenden ID-Token als Anspruch enthalten. Die Anwendung kann dann diesen Wert, um token Replay-Angriffe überprüfen. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die die Herkunft der Anforderung verwendet werden kann. |
| p | Erforderlich | Die Richtlinie ausgeführt wird. Es ist der Name einer Richtlinie, die in Ihrem Mandanten B2C erstellt wird. Der Richtlinienwert Namen sollte mit beginnen "b2c\_1\_". Erfahren Sie mehr über Richtlinien in [Extensible Politik](active-directory-b2c-reference-policies.md). |
| Aufforderung | Optionale | Der Typ von Benutzerinteraktion erforderlich ist. Zu diesem Zeitpunkt der einzige gültige Wert ist 'Anmeldung', die dem Benutzer die Anforderung ihre Anmeldeinformationen eingeben. Einmaliges Anmelden wird wirksam. |

Zu diesem Zeitpunkt wird der Benutzer aufgefordert die Richtlinie Workflows. Dies kann den Benutzer ihren Benutzernamen und Kennwort anzumelden sozialen Identität anmelden für das Verzeichnis oder eine beliebige Anzahl Schritte je nach die Richtlinie Definition.

Nach Abschluss der Benutzer die Richtlinie zurück Azure AD auf Ihre Anwendung auf die `redirect_uri`, mit der Methode, die in der `response_mode` Parameter. Die Antwort wird genau für jeden der oben genannten Fälle, unabhängig von der Richtlinie übereinstimmen, die ausgeführt wurde.

Eine erfolgreiche Antwort mit `response_mode=fragment` würde wie folgt aussehen:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| ID-Token | Das ID-Token, das die Anwendung angefordert. Das ID-Token können Sie die Identität des Benutzers überprüfen und beginnen mit dem Benutzer. Weitere Informationen zu ID-Token und deren Inhalte sind in [Azure AD B2C Tokenverweis](active-directory-b2c-reference-tokens.md)enthalten. |
| Code | Authorization_code, die die Anwendung angefordert hat, wenn Sie verwendet, `response_type=code+id_token`. Die app können den Autorisierungscode ein Zugriffstoken für eine Ressource anfordern. Authorization_codes sind sehr kurzlebig. In der Regel laufen sie nach etwa 10 Minuten. |
| Zustand | Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |

Fehlerantworten an gesendet werden die `redirect_uri` , damit die Anwendung diese angemessen behandelt werden kann:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann. |
| Zustand | Siehe vollständige Beschreibung in der vorherigen Tabelle. Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |


## <a name="validate-the-idtoken"></a>Überprüfen Sie das ID-Token
Empfang ein ID-Token ist nicht genug, um die Benutzerauthentifizierung – müssen das ID-Token Signatur überprüfen und Überprüfen der Ansprüche im Token nach Ihrer Anwendung wünschen. Azure AD B2C verwendet [JSON Web Token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und öffentlichen Schlüsseln Token signieren und überprüfen, ob sie gültig sind.

Es gibt zahlreiche Open-Source-Bibliotheken zur Überprüfung JWTs, je nach der Sprache. Wir empfehlen diese Optionen, anstatt eigene Überprüfungslogik zu implementieren. Die Informationen werden herauszufinden, wie gut diese Bibliotheken.

Azure AD B2C hat OpenID verbinden Metadatenendpunkt eine Anwendung zum Abrufen von Informationen über Azure AD B2C zur Laufzeit ermöglicht. Hierzu zählen Endpunkte, token Inhalt und Token Signaturschlüssel. Es ist ein JSON-Metadatendokument für jede Richtlinie in Ihrem B2C-Mandanten. Beispielsweise das Metadatendokument für die `b2c_1_sign_in` Politik in der `fabrikamb2c.onmicrosoft.com` befindet sich unter:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Eine der Eigenschaften dieses Konfiguration ist die `jwks_uri`, deren Wert für dieselbe Richtlinie wäre:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Um zu ermitteln, welche Richtlinie zum Signieren einer ID-Token wurde (und wo Metadaten abgerufen), haben Sie zwei Optionen. Der Richtlinienname, befindet sich auf der `acr` Anspruch in der ID-Token. Informationen zum Analysieren der Ansprüche ein ID-Token finden Sie in [Azure AD B2C Tokenverweis](active-directory-b2c-reference-tokens.md). Eine andere Möglichkeit ist zum Codieren der Richtlinie in den Wert der `state` Parameter, wenn die Anforderung und decodieren, um zu ermitteln, welche Richtlinie verwendet wurde. Bei beiden Methoden ist uneingeschränkt.

Nachdem Sie das Metadatendokument vom Endpunkt Metadaten OpenID Verbindung erworben haben, können Sie die öffentlichen Schlüssel RSA 256 (die an diesem Endpunkt) zum Überprüfen der Signatur der ID-Token. Möglicherweise mehrere Schlüssel an diesem Endpunkt jederzeit rechtzeitig aufgeführt, jeweils anhand einer `kid`. Der Header der ID-Token enthält auch eine `kid` behaupten, wodurch die diese Schlüssel verwendet wurde, das ID-Token. [Azure AD B2C Tokenverweis](active-directory-b2c-reference-tokens.md) Informationen einschließlich [Token überprüfen](active-directory-b2c-reference-tokens.md#validating-tokens) und [wichtige Informationen zum Signieren der Schlüssel Rollover](active-directory-b2c-reference-tokens.md#validating-tokens)anzeigen
<!--TODO: Improve the information on this-->

Nachdem Sie die Signatur der ID-Token überprüft haben, sind mehrere Ansprüche beispielsweise überprüfen müssen:

- Sie sollten überprüfen die `nonce` Anspruch auf token Replay-Angriffe verhindert. Der Wert sollte Angabe in der Anforderung.
- Sie sollten überprüfen die `aud` behaupten, sicherzustellen, dass das ID-Token für Ihre ausgestellt wurde. Der Wert sollte die-ID der Anwendung.
- Sie sollten überprüfen die `iat` und `exp` soll sicherstellen, dass das ID-Token nicht abgelaufen ist.

Es gibt auch einige weitere Prüfung, die Sie, [OpenID verbinden Kern-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html)beschrieben durchführen sollten.  Sie möchten auch weitere, je nach Szenario zu überprüfen. Einige allgemeine Prüfung gehören:

- Sicherstellen, dass die Benutzerorganisation für die Anwendung signiert wurde.
- Sicherstellen, dass der Benutzer ordnungsgemäße Autorisierung-Berechtigungen verfügt.
- Sicherstellen, dass eine bestimmte Stärke von Authentifizierung wie Azure mehrstufige Authentifizierung aufgetreten ist.

Weitere Informationen zu den Ansprüchen ein ID-Token finden Sie in [Azure AD B2C Tokenverweis](active-directory-b2c-reference-tokens.md).

Nachdem das ID-Token vollständig überprüft haben, können Sie beginnen mit dem Benutzer und Ansprüche in der ID-Token zu Informationen über die Benutzer in Ihrer Anwendung verwenden. Diese Informationen kann für die Anzeige, Datensätze, Autorisierung usw. verwendet.

## <a name="get-a-token"></a>Einen Token abrufen
Ist, Ihr Web app muss Ausführung, überspringen Sie die nächsten Abschnitten. Diese Abschnitte sind nur anwendbar auf Web apps müssen authentifizierte Aufrufe einer Web-API und auch von Azure AD B2C geschützt.

Lösen Sie die Authorization_code, die Sie erworben (mit `response_type=code+id_token`) für ein Token für die gewünschte Ressource durch Senden einer `POST` anfordern, die `/token` Endpunkt. Derzeit ist die einzige Ressource, der einen Token anfordern kann Ihre app eigene Back-End-Web-API. Die Konvention für ein Token sich anfordert ist Ihre app Client-ID als Bereich:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| p | Erforderlich | Die Richtlinie, die zu den Autorisierungscode verwendet wurde. Eine andere Richtlinie können keine in dieser Anforderung. Beachten Sie, dass dieser Parameter in der **Abfragezeichenfolge**nicht in den POST-Text hinzufügen. |
| client_id | Erforderlich | Die-ID, die Ihre app [Azure-Portal](https://portal.azure.com/) zugewiesen. |
| grant_type | Erforderlich | Den Typ des Zuschusses, die `authorization_code` für die Autorisierung Code Flow. |
| Gültigkeitsbereich | Empfohlen | Eine durch Leerzeichen getrennte Liste von Bereichen. Bereich gibt an beiden Azure AD Berechtigungen angefordert werden. Die `openid` Bereich gibt eine Berechtigung Anmelden des Benutzers und Daten über die Benutzer **ID-Token**. Es kann zu vertreten durch die gleichen-ID als Client-Token, die der app Back-End-Web-API verwendet werden. Die `offline_access` Bereich gibt an, dass Ihre Anwendung ein **Aktualisierungstoken** für langlebige Ressourcen benötigen. |
| Code | Erforderlich | Die Authorization_code, die Sie in die erste Etappe des Flusses erworben. |
| redirect_uri | Erforderlich | Die Redirect_uri die Authorization_code erhielt Anwendung. |
| client_secret | Erforderlich | Die geheimen Schlüssel, den in [Azure-Portal](https://portal.azure.com/)generiert. Dieser geheimen Schlüssel ist ein wichtiges Sicherheitsfeature Artefakt. Sie sollten es sicher auf dem Server speichern. Außerdem Vorsicht dieser geheimen regelmäßig drehen. |

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
| Zugriffstoken | Die angeforderte signierte JWT-Token. |
| Gültigkeitsbereich | Die Bereiche, denen das Token gültig ist, ist die Token zur späteren Verwendung Zwischenspeichern verwendet werden kann. |
| expires_in | Die Zeitspanne (in Sekunden) das Zugriffstoken gilt. |
| Aktualisierungstoken | Ein Aktualisierungstoken OAuth 2.0. Dieses Token können die Anwendung weitere Tokens nach Ablauf des aktuellen Tokens bekommen.  Aktualisierungstoken lange lebten und können verwendet werden, um Zugriff auf Ressourcen für längere Zeit beibehalten. Weitere Details finden Sie unter [B2C Tokenverweis](active-directory-b2c-reference-tokens.md). Beachten Sie, dass Sie den Bereich verwendet haben, müssen `offline_access` die Autorisierung und token Anfragen um ein Aktualisierungstoken zu erhalten. |

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

## <a name="use-the-token"></a>Verwenden Sie das token
Nun, dass Sie bereits erfolgreich gekauft haben eine `access_token`, können Sie das Token in Anfragen an die Back-End-web-APIs durch Einschließen in die `Authorization` Header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Das Token aktualisieren
Die ID-Token sind kurzlebige. Sie aktualisieren müssen, nachdem ablaufen, um weiterhin Zugriff auf Ressourcen. Dazu Übermitteln einer anderen `POST` anfordern, die `/token` Endpunkt. Dieses Mal, die `refresh_token` statt der `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Erforderlich | Beschreibung |
| ----------------------- | ------------------------------- | -------- |
| p | Erforderlich | Die Richtlinie, die zu der ursprünglichen Aktualisierungstoken verwendet wurde. Eine andere Richtlinie können keine in dieser Anforderung. Beachten Sie, dass dieser Parameter in der **Abfragezeichenfolge**nicht in den POST-Text hinzufügen. |
| client_id | Erforderlich | Die-ID, die Ihre app [Azure-Portal](https://portal.azure.com/) zugewiesen. |
| grant_type | Erforderlich | Den Typ des Zuschusses, die `refresh_token` für dieses Abschnitts Authorization Code Flow. |
| Gültigkeitsbereich | Empfohlen | Eine durch Leerzeichen getrennte Liste von Bereichen. Bereich gibt an beiden Azure AD Berechtigungen angefordert werden. Die `openid` Bereich gibt eine Berechtigung Anmelden des Benutzers und Daten über die Benutzer **ID-Token**. Es kann zu vertreten durch die gleichen-ID als Client-Token, die der app Back-End-Web-API verwendet werden. Die `offline_access` Bereich gibt an, dass Ihre Anwendung ein **Aktualisierungstoken** für langlebige Ressourcen benötigen. |
| redirect_uri | Empfohlen | Die Redirect_uri die Authorization_code erhielt Anwendung. |
| Aktualisierungstoken | Erforderlich | Die ursprüngliche Aktualisierungstoken, die in der des Flusses erworben. Beachten Sie, dass Sie den Bereich verwendet haben, müssen `offline_access` die Autorisierung und token Anfragen um ein Aktualisierungstoken zu erhalten. |
| client_secret | Erforderlich | Die geheimen Schlüssel, den in [Azure-Portal](https://portal.azure.com/)generiert. Dieser geheimen Schlüssel ist ein wichtiges Sicherheitsfeature Artefakt. Sie sollten es sicher auf dem Server speichern. Außerdem Vorsicht dieser geheimen regelmäßig drehen. |

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
| Zugriffstoken | Die angeforderte signierte JWT-Token. |
| Gültigkeitsbereich | Die Bereiche, denen das Token gültig ist, ist die Token zur späteren Verwendung Zwischenspeichern verwendet werden kann. |
| expires_in | Die Zeitspanne (in Sekunden) das Zugriffstoken gilt. |
| Aktualisierungstoken | Ein Aktualisierungstoken OAuth 2.0. Dieses Token können die Anwendung weitere Tokens nach Ablauf des aktuellen Tokens bekommen.  Aktualisierungstoken lange lebten und können verwendet werden, um Zugriff auf Ressourcen für längere Zeit beibehalten. Weitere Details finden Sie unter [B2C Tokenverweis](active-directory-b2c-reference-tokens.md). |

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


## <a name="send-a-sign-out-request"></a>Eine Abmelde Anforderung senden

Wenn Sie den Benutzer aus der Anwendung signieren möchten, reicht es nicht Ihre app Cookies oder anderweitig am Ende der Sitzung für den Benutzer löschen. Sie müssen auch Azure AD Abmelden den Benutzer umleiten. Wenn Sie dies nicht tun, kann der Benutzer für Ihre Anwendung zu authentifizieren, ohne ihre Anmeldeinformationen erneut eingeben können. Ist eine gültige Anmeldung Sitzung mit Azure haben.

Sie können einfach den Benutzer umleiten der `end_session_endpoint` im gleichen OpenID verbinden Metadaten beschriebenen Dokuments bereits im Abschnitt "Überprüfen der ID-Token" aufgeführt ist:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| p | Erforderlich | Die Richtlinie, die die Benutzer Ihrer Anwendung verwenden möchten. |
| post_logout_redirect_uri | Empfohlen | Die URL, die nach der Benutzer umgeleitet werden soll erfolgreich abmelden. Wenn nicht angegeben ist, wird der Benutzer eine allgemeine Meldung von Azure AD B2C angezeigt.  |

> [AZURE.NOTE]
    Beim Umleiten des Benutzers die `end_session_endpoint` Deaktivieren einer einzigen Anmeldung Benutzerstatus mit Azure AD B2C, wird den Benutzer aus der Benutzer Identität (IDP) Sitzung nicht signieren. Wählt der Benutzer die gleiche IDP während nachfolgende anmelden, werden sie erneut authentifiziert werden ohne Anmeldeinformationen. Will ein Benutzer aus der B2C-Anwendung signieren, bedeutet dies nicht notwendigerweise möchten vollständig aus ihren Facebook-Konto anmelden. Allerdings wird bei lokalen Konten die Sitzung des Benutzers ordnungsgemäß beendet.

## <a name="use-your-own-b2c-tenant"></a>Verwenden Sie eigene B2C Mieter

Wenn Sie diese Anfragen selbst ausprobieren möchten, müssen zuerst gehen drei und Beispielwerte oben durch Ihren eigenen ersetzen:

- [B2C Mieter erstellen](active-directory-b2c-get-started.md)und verwenden die von Ihrem Mandanten in Anfragen.
- [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) eine ID und einen Redirect_uri. Werden sollen ein **web-app-Web api** in Ihrer Anwendung und optional einen **geheimen Schlüssel**erstellen.
- [Erstellen Sie Ihre Richtlinien](active-directory-b2c-reference-policies.md) zu den Richtliniennamen.
