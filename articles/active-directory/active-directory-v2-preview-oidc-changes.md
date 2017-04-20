<properties
    pageTitle="Ändert den Endpunkt Azure AD v2. 0 | Microsoft Azure"
    description="Eine Beschreibung der Änderungen an die app-Modell v2. 0 public Preview-Protokolle."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Wichtige Updates Authentifizierungsprotokolle v2. 0
Achtung-Entwickler! In den nächsten zwei Wochen werden wir einige Updates unserer Authentifizierungsprotokolle v2. 0 vornehmen, die grundlegend geändert für apps während unserer Probezeit erstellten bedeuten kann.  

## <a name="who-does-this-affect"></a>Die wirkt sich?
Apps, die mit der Version 2.0 geschrieben wurden konvergiert Authentifizierung Endpunkt,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Weitere Informationen für den Endpunkt v2. 0 finden [hier](active-directory-appmodel-v2-overview.md).

Wenn Sie eine Anwendung mithilfe der Codierung an v2. 0-Protokoll v2. 0-Endpunkt erstellt haben, sollte eines unserer OpenID verbinden oder OAuth Web Middlewares oder mit anderen Bibliotheken 3. authentifizieren Sie vorbereitet werden Projekte testen und ändern ggf..

## <a name="who-doesnt-this-affect"></a>Wer ist dies?
Apps, die für die Produktion Azure AD-Authentifizierung Endpunkt geschrieben wurden,

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Dieses Protokoll unveränderlich und werden keine Änderungen auftreten.

Wenn Ihre Anwendung **nur** ADAL Bibliothek Authentifizierung verwendet, müssen Sie außerdem ändern.  ADAL hat die app aus dem abgeschirmt.  

## <a name="what-are-the-changes"></a>Was sind die Änderungen?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Den Wert x5t entfernt aus JWT
V2. 0-Endpunkt verwendet JWT Token, Parameter Kopfzeilenbereich mit relevanten Metadaten über das Token enthalten.  Wenn den Header eines unserer aktuellen JWTs decodieren finden Sie etwa:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Wo die "x5t" und "Kind" Eigenschaften den öffentlichen Schlüssel identifizieren, der verwendet das Token Signatur überprüfen von Metadatenendpunkt OpenID Verbindung abgerufen werden soll.

Machen wir hier ändern werden die Eigenschaft "x5t" entfernt.  Sie sollten können weiterhin denselben Mechanismen Token überprüft, aber nur "Kind"-Eigenschaft, um den richtigen öffentlichen Schlüssel gemäß Protokoll OpenID Verbindung abzurufen. 

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre Anwendung hängt nicht auf den Wert x5t.**

### <a name="removing-profileinfo"></a>Profile_info entfernen
Zuvor v2. 0-Endpunkt hat wurde ein base64-codierte JSON-Objekt zurückzugeben token Antworten aufgerufen `profile_info`.  Beim Anfordern von Zugriffstoken von v2. 0-Endpunkt durch Senden einer Anforderung an:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Die Antwort sieht wie das JSON-Objekt:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Die `profile_info` enthalten Informationen über Benutzer in der app - ihren Anzeigenamen Vorname, Nachname, e-Mail-Adresse, ID, und signiert.  Vor allem die `profile_info` wurde für das Zwischenspeichern von token verwendet und Zwecke anzeigen.

Wir entfernt jetzt die `profile_info` Wert – aber keine Sorge, wir werden weiterhin diese Informationen für Entwickler geringfügig an.  Anstelle von `profile_info`, v2. 0-Endpunkt liefert jetzt eine `id_token` jedes token auf:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Sie decodieren und Analysieren der ID-Token Abrufen von Profile_info erhalten dieselbe Informationen.  Das ID-Token ist ein JSON Web Token (JWT), Inhalt laut OpenID verbinden.  Code dafür sollte daher sehr ähnlich – müssen Sie lediglich das mittlere Marktsegment (Text) das ID-Token zu extrahieren und decodiert base64 in JSON-Objekt zugreifen.

In den nächsten zwei Wochen sollten Sie Ihre Anwendung zum Abrufen von Benutzerinformationen aus code der `id_token` oder `profile_info`; Je nachdem, was vorhanden ist.  So bei der Änderung Ihrer app kann den Übergang nahtlos behandeln `profile_info` , `id_token` ohne Unterbrechung.

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre Anwendung hängt nicht auf die `profile_info` Wert.**

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in entfernen
Ähnlich wie `profile_info`, entfernen wir auch die `id_token_expires_in` -Parameter Antworten.  V2. 0-Endpunkt liefern zuvor einen Wert für `id_token_expires_in` mit jeder Antwort ID-Token beispielsweise auf ein autorisieren:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Oder ein token:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Die `id_token_expires_in` bewertet die Anzahl Sekunden wäre das ID-Token für gültig.  Jetzt entfernen wir die `id_token_expires_in` Wert vollständig.  Sie können stattdessen Standard OpenID verbinden `nbf` und `exp` überprüfen Sie die Gültigkeit einer ID-Token ausgibt.  Diese Ansprüche [Tokenverweis v2. 0](active-directory-v2-tokens.md) Weitere Informationen entnehmen.

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre Anwendung hängt nicht auf die `id_token_expires_in` Wert.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Ändern der zurückgegebene Bereich Ansprüche = Openid
Diese Änderung wird am wichtigsten – tatsächlich, wirkt fast jede Anwendung, die v2. 0-Endpunkt verwendet.  Vielen Anfragen v2. 0-Endpunkt mit der `openid` Bereich, wie:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Heute, wenn der Benutzer Zustimmung erteilt die `openid` Bereich Ihrer Anwendung empfängt eine Fülle von Informationen über den Benutzer in der resultierenden ID-Token.  Diese Ansprüche können ihren Namen, bevorzugte Benutzername, e-Mail-Adresse, Objekt-ID und mehr enthalten.

In diesem Update wir die Informationen ändern, die `openid` Bereich bietet Ihre app auf, bessere Comform Spezifikation OpenID verbinden.  Die `openid` Bereich nur können sich Benutzer in Ihrer app und erhalten einen anwendungsspezifischen Bezeichner für den Benutzer in der `sub` Anspruch auf das ID-Token.  Die Ansprüche in ein ID-Token nur die `openid` Bereich gewährt werden persönlichen Informationen.  Beispiel-ID-Token Ansprüche sind:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Wenn Sie personenbezogene Informationen (PII) über den Benutzer in der app abrufen möchten, müssen Ihre app Benutzer weitere Berechtigungen anfordern.  Wir stellen Unterstützung für zwei neue Bereiche von OpenID verbinden Spec – der `email` und `profile` Bereiche – tun können.

- Die `email` ist sehr einfach – ermöglicht den Anwendung Zugriff auf e-Mail-Adresse des Benutzers über die `email` des Anspruchs der ID-Token.  Beachten Sie, dass die `email` Anspruch nicht immer vorhanden in ID-Token – nur enthalten ggf. im Profil des Benutzers.
- Die `profile` Bereich bietet Ihre app auf andere grundlegende Informationen über den Benutzer-Namen, bevorzugte Benutzername, Objekt-ID usw..

Dadurch können Sie Ihre Anwendung in einer minimale Offenlegung Weise code – bitten Sie den Benutzer für nur diejenigen Informationen, die Ihre Anwendung erfordert zu.  Möchten Sie weiterhin immer den vollständigen Satz der Benutzerinformationen, die Ihre Anwendung derzeit erhalten, ist Ihre Autorisierungsanfragen sollen alle drei Bereiche:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Ihre app kann beginnen Senden der `email` und `profile` Bereiche sofort, v2. 0-Endpunkt akzeptiert diese beiden Bereiche und Anfordern von Berechtigungen von Benutzern nach Bedarf beginnen.  Jedoch die Änderung bei der Interpretation von den `openid` Bereich nicht wirksam für Wochen.

> [AZURE.IMPORTANT] **Ihre Aufgabe: Hinzufügen der `profile` und `email` erfordert die Anwendung Informationen über den Benutzer festlegt.**  Beachten Sie, dass ADAL beide Berechtigungen Anfragen standardmäßig enthalten sein sollen. 

### <a name="removing-the-issuer-trailing-slash"></a>Entfernen des Ausstellers nachgestellten Schrägstrich.
Früher wurde der Aussteller Wert in Token vom Endpunkt v2. 0

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Die Guid TenantId Azure AD-Mandanten war das Token ausgestellt.  Diese Änderung wird der Issuer-Wert

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

Beide Token und im Discoverydokument OpenID verbinden.

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre app nimmt den Wert Aussteller mit und ohne nachstehenden Schrägstrich während der Validierung des Emittenten.**

## <a name="why-change"></a>Warum ändern?
Die primäre Motivation für diese Änderung ist mit der Standardspezifikation OpenID verbinden.  Wir hoffen, OpenID Verbindung kompatibel Unterschiede zwischen Integration Identity von Microsoft und anderen Identitätsdienste im Bereich minimieren.  Wir möchten Entwickler mithilfe ihrer bevorzugten open-Source-authentifizierungsbibliotheken ohne ändern die Bibliotheken Microsoft Unterschiede.

## <a name="what-can-you-do"></a>Was können Sie tun?
Heute können Sie alle oben beschriebenen Änderung beginnen.  Sie sollten sofort:

1.  **Entfernen von Abhängigkeiten auf die `x5t` Header-Parameter.**
2.  **Beheben Sie den Übergang von `profile_info` , `id_token` token Antworten.**
3.  **Entfernen von Abhängigkeiten auf die `id_token_expires_in` Antwort Parameter.**
3.  **Hinzufügen der `profile` und `email` für Ihre Anwendung festgelegt werden, wenn Ihre app grundlegende Benutzerinformationen.**
4.  **Akzeptieren Sie Aussteller-Werten in Token mit und ohne nachstehenden Schrägstrich.**

[Dokumentation-Protokoll v2. 0](active-directory-v2-protocols.md) wurde bereits aktualisiert diese Änderungen angepasst, so dass Sie als Referenz bei der Code aktualisieren können.

Haben Sie Fragen hinsichtlich der ändert sich gerne erreichen, um uns auf Twitter unter @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Wie oft treten Protokoll ändern?
Wir sehen nicht weiter brechen Änderungen an die Authentifizierungsprotokolle.  Wir bündeln absichtlich diese Änderungen in einer Version, damit Sie nicht durch diese Art der Aktualisierung wieder schnell gehen.  Natürlich sollte werden wir den Authentifizierungsdienst konvergierte v2. 0 Features hinzufügen, die Sie nutzen können, diese Änderungen jedoch Zusatzstoff und nicht vorhandenen Code unterbricht.

Schließlich möchten wir bedanken für ausprobieren während der Probezeit.  Einblicke und Erfahrungen unserer Erstanwendern haben bisher von unschätzbarem Wert, und wir hoffen, dass Sie weiterhin Ihre Meinung und Ideen.

Viel Spaß beim Codieren!

Microsoft Identity Division
