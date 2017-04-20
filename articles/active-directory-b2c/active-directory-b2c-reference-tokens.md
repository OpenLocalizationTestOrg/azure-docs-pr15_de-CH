<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Die Typen von Azure Active Directory B2C ausgestellten Token."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Token Verweis

Azure Active Directory (Azure AD) B2C gibt verschiedene Arten von Sicherheitstokens jeder [Authentifizierungsablauf](active-directory-b2c-apps.md)verarbeitet. Dieses Dokument beschreibt das Format Sicherheitsmerkmale und Inhalt jedes Token.

## <a name="types-of-tokens"></a>Typen von Token

Azure AD B2C unterstützt das [OAuth 2.0 Authorization-Protokoll](active-directory-b2c-reference-protocols.md), welches Zugriffstoken und Aktualisierungstoken verwendet. Unterstützt Authentifizierung und Anmeldung über [OpenID verbinden](active-directory-b2c-reference-protocols.md), einen dritten Typ von Token vorgestellt: das ID-Token. Jede dieser Token wird als trägertoken dargestellt.

Ein trägertoken ist ein einfaches Sicherheitstoken, den "Inhaber" den Zugriff auf eine geschützte Ressource gewährt. Der Inhaber ist eine Partei, die das Token darstellen kann. Azure AD muss zuerst eine Partei authentifizieren, bevor ein trägertoken erhalten. Wenn die erforderlichen Schritte zum Sichern des Tokens in der Übertragung und Speicherung nicht stammen, kann jedoch werden abgefangen und durch einen unerwünschten Dritten verwendet. Einige Sicherheitstoken verfügen über einen integrierten Mechanismus verhindern, dass unbefugte Verwendung jedoch trägertoken verfügen über diesen Mechanismus. Sie müssen in einem sicheren Kanal wie Transport Layer Security (HTTPS) übertragen.

Bei der Übertragung eines Träger Tokens außerhalb eines sicheren Kanals können böswillige Dritte einen Man-in-the-Middle-Angriff das Token und Zugriff auf eine geschützte Ressource zu verwenden. Sicherheitsgrundsätze gelten beim trägertoken gespeichert oder für die spätere Verwendung zwischengespeichert. Immer sicherstellen Sie, dass Ihre app überträgt und trägertoken sicher speichert.

Weitere Sicherheitsaspekte zu produzierende Token finden Sie in [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der Probleme von Azure AD B2C Token werden als JSON Web Token (JWTs) implementiert. Ein JWT ist ein kompaktes, URL-sichere Übertragung von Daten zwischen zwei Parteien. JWTs enthalten Informationen, die Ansprüche genannt. Diese sind Assertionen Informationen Träger der des Tokens. Die Ansprüche in JWTs sind JSON-Objekten, die codiert und für die Übertragung serialisiert. Da Azure AD B2C ausgestellten JWTs signiert jedoch nicht verschlüsselt, können Sie einfach den Inhalt des JWT zum Debuggen überprüfen. Verschiedene Tools zur Verfügung, die dazu, einschließlich [calebb.net](http://calebb.net). Weitere Informationen zu JWTs finden Sie in [JWT-Spezifikationen](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-Token

Ein ID-Token ist ein Sicherheitstoken, das Ihre app von Azure AD B2C empfängt `authorize` und `token` Endpunkte. ID-Token als [JWTs](#types-of-tokens)dargestellt, und sie enthalten Ansprüche, mit denen Sie Benutzer in Ihrer Anwendung zu identifizieren. Wenn ID-Token gewonnenen sind die `authorize` Endpunkt häufig werden sich Benutzer von ASP.NET-Webanwendungen. Wenn ID-Token gewonnenen sind die `token` Endpunkt, sie können gesendet werden in HTTP-Anfragen während der Kommunikation zwischen zwei Komponenten einer Anwendung oder Dienst. Die Ansprüche können in ein ID-Token je nach Bedarf. Sie werden häufig um Kontoinformationen anzuzeigen oder zu Access Control in einer Anwendung verwendet.  

ID-Token signiert, aber sie werden nicht verschlüsselt. Wenn Ihre app oder API einen ID-Token erhält, muss [die Signatur überprüfen](#token-validation) , um nachzuweisen, dass das Token authentisch ist. Ihre app oder -API müssen Sie einige Ansprüche im Token nachweislich gültig ist auch überprüfen. Je nach Szenario einer App überprüft Ansprüche können variieren, aber Ihre app müssen einige [häufig Behauptung Prüfung](#token-validation) in jedem Szenario.

#### <a name="sample-id-token"></a>Beispiel-ID-token
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Zugriffstoken

Ein Zugriffstoken ist auch ein Sicherheitstoken, das Ihre app von Azure AD B2C empfängt `authorize` und `token` Endpunkte. Zugriffstoken werden auch als [JWTs](#types-of-tokens)dargestellt und enthalten Angaben, mit denen Sie Benutzer in Ihrem WebServices und APIs.

Zugangs-Token signiert, aber sie sind nicht zu diesem Zeitpunkt - verschlüsselte und Id-Token sehr ähnlich.  Zugriffstoken sollte Zugriff auf Webdienste und APIs zum Identifizieren und Authentifizieren des Benutzers in dieser Dienste verwendet werden.  Sie bieten jedoch keine Geltendmachung der Berechtigung auf diese Dienste.  Das heißt, die `scp` Anspruch im Zugriffstoken nicht einschränken oder anderweitig Zugriff, der das Thema des Tokens erteilt wurde darstellen.

API Zugriffstoken erhält, muss [die Signatur überprüfen](#token-validation) , um nachzuweisen, dass das Token authentisch ist. Ihre API muss auch überprüfen einige Ansprüche im Token nachweislich gültig ist. Je nach Szenario einer App überprüft Ansprüche können variieren, aber Ihre app müssen einige [häufig Behauptung Prüfung](#token-validation) in jedem Szenario.

### <a name="claims-in-id--access-tokens"></a>Ansprüche im ID & Zugriffstoken

Bei Verwendung von Azure AD B2C haben Sie eine genauere Kontrolle über den Inhalt des Token. Sie können [Richtlinien](active-directory-b2c-reference-policies.md) für bestimmte Datensätze Ansprüche zu senden, die Ihre Anwendung für den Betrieb benötigt. Diese Ansprüche können Standardeigenschaften wie des Benutzers gehören `displayName` und `emailAddress`. Sie können auch [benutzerdefinierte](active-directory-b2c-reference-custom-attr.md) Benutzerattribute, die im Verzeichnis B2C definieren können. Jede ID & Access Token Sie erhalten enthalten eine Reihe von Sicherheits-Ansprüche. Diese Ansprüche können Ihre Anwendung sicher authentifiziert Benutzer und Anfragen.

Beachten Sie, dass die Ansprüche im Token ID nicht in einer bestimmten Reihenfolge zurückgegeben werden. Darüber hinaus können neue Ansprüche jederzeit in ID-Token eingeführt. Ihre Anwendung sollte nicht unterbrochen neuer Anträge eingeführt werden. Hier sind die Ansprüche, die erwarten, ID und von Azure AD B2C ausgestellten Token vorhanden. Richtlinien werden zusätzliche Ansprüche bestimmt. Übung versuchen Sie überprüft die Ansprüche im Token-ID Beispiel von [calebb.net](http://calebb.net)eingefügt. Weitere Informationen finden in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html).

| Name | Forderung | Beispiel für einen Wert | Beschreibung |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Zielgruppe | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Ein Anspruch Zielgruppe identifiziert den Empfänger des Tokens. Für Azure AD B2C ist das Publikum Ihre app Anwendung ID Ihrer app im Portal Registrierung app zugewiesen wurde. Ihre Anwendung sollte diesen Wert überprüfen und Token ablehnen, wenn es nicht übereinstimmt. |
| Aussteller | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Dies gibt den Sicherheitstokendienst (STS), der erstellt und gibt das Token zurück. Es gibt auch Azure AD-Verzeichnis, in dem der Benutzer authentifiziert wurde. Ihre app sollte Aussteller Anspruch darauf kam das Token vom Endpunkt v2. 0 überprüft werden. |
| Ausgestellt am | `iat` | `1438535543` | Dies ist die Zeit mit Token ausgestellt wurde, rechtzeitig Epoche dargestellt. |
| Ablaufzeit | `exp` | `1438539443` | Die Ablaufzeit, die Behauptung ist, das Token ungültig ist, wird, dargestellt in Epoche Zeit. Ihre app verwenden dafür die Gültigkeitsdauer des Tokens Gültigkeit.  |
| Nicht vor | `nbf` | `1438535543` | Dies ist die Zeit, die das Token ungültig, Epoche Zeit dargestellt wird. Dies entspricht in der Regel gleichzeitig das Token ausgestellt wurde. Ihre app verwenden dafür die Gültigkeitsdauer des Tokens Gültigkeit.  |
| Version | `ver` | `1.0` | Die Version der Token-ID ist von Azure AD definiert |
| Code-hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Ein Hash Code gehört ein ID-Token nur, wenn das Token mit einem Autorisierungscode OAuth 2.0 ausgegeben wird. Ein Hash Code kann verwendet werden, die Authentizität einer Autorisierungscode überprüft. [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) für weitere Details zu dieser Überprüfung anzeigen |
| Access token hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Ein token Zugriff Hash gehört ein ID-Token nur, wenn das Token mit einem Zugriffstoken OAuth 2.0 ausgegeben wird. Ein token Access-Hash kann verwendet werden, die Authentizität eines Zugriffstokens überprüft. [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) für weitere Details zu dieser Überprüfung anzeigen |
| Nonce | `nonce` | `12345` | Eine Nonce ist eine Strategie zur token Replay-Angriffe. Ihre app können Nonce in eine Authentifizierungsanfrage mithilfe der `nonce` Parameter Abfragen. Der Wert in der Anforderung ausgegeben, die unverändert das `nonce` Anspruch ein ID-Token. Dadurch wird Ihre app überprüfen den Wert mit dem Wert in der Anforderung angegebene das app Sitzung mit einer bestimmten ID verknüpft. Ihre Anwendung sollte diese Überprüfung während der ID-token Validierung ausführen. |
| Betreff | `sub` | `Not supported currently. Use oid claim.` | Dies ist ein Prinzipal über dem Token Informationen wie der Benutzer einer Anwendung bestimmt. Dieser Wert ist unveränderlich und kann nicht neu zugewiesen oder wiederverwendet werden. Hiermit können Sie Autorisierung überprüft werden soll, wie wenn das Token Zugriff auf eine Ressource verwendet wird. In Azure AD B2C ist Thema Anspruch jedoch noch nicht implementiert. Konfigurieren Sie die Richtlinien auf die Objekt-ID `oid` anfordern und dessen Wert identifizieren Benutzer anstelle Betreff Anspruch für die Autorisierung. |
| Authentifizierung Kontextreferenz-Klasse | `acr` | `b2c_1_sign_in` | Dies ist der Name der Richtlinie, die verwendet wurde, um die Token-ID erhalten.  |
| Authentifizierung | `auth_time` | `1438535543` | Dieser Antrag ist Zeit mit einer letzten eingegebenen Benutzeranmeldeinformationen Epoche Zeit. |


### <a name="refresh-tokens"></a>Aktualisieren von Token

Aktualisieren Token sind Sicherheitstoken, die Ihre Anwendung neue ID-Tokens und Tokens in einen Fluss OAuth 2.0 zugreifen kann. Sie bieten Ihre app langfristigen Zugriff auf Ressourcen für Benutzer ohne Interaktion mit Benutzern.

Eine Aktualisierung empfangen token token auf Ihre app muss Anfordern der `offline_acesss` Bereich. Weitere Informationen zu den `offline_access` Bereich, finden Sie in [Azure AD B2C-Protokoll Verweis](active-directory-b2c-reference-protocols.md).

Token aktualisiert werden und werden immer, Ihre Anwendung transparent. Sie erhalten von Azure AD überprüft und von Azure AD interpretiert. Sie sind langlebig, aber Ihre app sollte nicht mit der Erwartung Aktualisierungstoken für einen bestimmten Zeitraum dauert geschrieben werden. Aktualisierungstoken können jederzeit aus verschiedenen Gründen ungültig sein. Die einzige Möglichkeit für Ihre Anwendung wissen, ob eine Aktualisierungstoken gültig ist ist der Versuch durch ein token Anforderung Azure AD einlösen.

Beim einlösen Aktualisierungstoken für ein neues Token (und wenn Ihre app gewährt wurden die `offline_access` Bereich), erhalten Sie ein neues Aktualisierungstoken token auf. Speichern Sie das Aktualisierungstoken neu. Sie ersetzen Aktualisierungstoken, das Sie zuvor in der Anforderung verwendet. Dies stellt sicher, dass Ihr Aktualisierungstoken so lange gültig.

## <a name="token-validation"></a>Token Validierung

Überprüfen ein Tokens Prüfen Ihrer Anwendung die Signatur und den Ansprüchen des Tokens.

Viele open-Source-Bibliotheken stehen für JWTs, je nach Ihrer bevorzugten Sprache überprüfen. Wir empfehlen, dass Sie diese Optionen, anstatt eine eigene Validierungslogik implementieren. Die Informationen in diesem Handbuch helfen bei der Verwendung ordnungsgemäß diese Bibliotheken.

### <a name="validate-the-signature"></a>Die Signatur überprüfen
Ein JWT enthält drei Segmente getrennt von den `.` Zeichen. Das erste Segment ist der **Header**, die zweite ist der **Text**und der dritte ist die **Signatur**. Segment Signatur kann verwendet werden, die Authentizität des Tokens überprüft, damit Ihre Anwendung vertrauen können.

Mit Industriestandard Asymmetrische Verschlüsselungsalgorithmen wie RSA 256 sind Azure AD B2C-Token signiert. Der Header des Tokens enthält Informationen der Schlüssel und Verschlüsselung zum Signieren von Token verwendet:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Die `alg` Anspruch gibt den Algorithmus, der verwendet wurde, das Token. Die `kid` Anspruch gibt den bestimmten öffentlichen Schlüssel, mit dem das Token signiert wurde.

Zu jedem Zeitpunkt kann Azure AD einen Token mithilfe einer Reihe von öffentlich-Private Schlüsselpaare signieren. Azure AD dreht kann mehrere Schlüssel in regelmäßigen Abständen Ihre app automatisch mit den geänderten geschrieben werden soll. Eine angemessene Häufigkeit nach Updates auf die öffentlichen Schlüssel von Azure AD verwendet wird innerhalb von 24 Stunden.

Azure AD B2C hat einen Metadaten-Endpunkt OpenID verbinden. Dadurch können apps Informationen Azure AD B2C zur Laufzeit abrufen. Hierzu zählen Endpunkte, token Inhalt und Token Signaturschlüssel. Das B2C-Verzeichnis enthält ein JSON-Metadatendokument für jede Richtlinie. Beispielsweise das Metadatendokument für die `b2c_1_sign_in` Politik in der `fabrikamb2c.onmicrosoft.com` befindet sich unter:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`wird verwendet, um die Benutzerauthentifizierung B2C-Verzeichnis und `b2c_1_sign_in` Richtlinie zu Token verwendet. Um zu bestimmen, welche Richtlinie verwendet wurde, einen Token und wo die Metadaten abgerufen haben Sie zwei Optionen. Der Richtlinienname, befindet sich auf der `acr` Anspruch im Token. Analysieren Ansprüche aus den JWT von Base64-Decodierung Text und deserialisiert die JSON-Zeichenfolge führt. Die `acr` Anspruchs werden der Name der Richtlinie, die verwendet wurde, um das Token ausstellen.  Eine andere Möglichkeit ist zum Codieren der Richtlinie in den Wert der `state` Parameter, wenn die Anforderung und decodieren, um zu ermitteln, welche Richtlinie verwendet wurde. Bei beiden Methoden ist gültig.

Das Metadatendokument ist ein JSON-Objekt, das mehrere nützliche Informationen enthält. Dazu gehören die Position der Endpunkte verbinden OpenID Authentifizierung erforderlich. Auch `jwks_uri`, die dem Speicherort der Gruppe öffentlicher Schlüssel gibt Token signiert wird. Standort wird hier bereitgestellt, dass es empfiehlt sich, den Speicherort abrufen dynamisch durch das Metadatendokument und Analysieren `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

JSON-Dokument unter diesem URL enthält alle Informationen zum öffentlichen Schlüssel gerade verwendet. Ihre Anwendung kann die `kid` Anspruch im JWT-Header auf den öffentlichen Schlüssel in das JSON-Dokument auswählen, die ein bestimmtes Token signiert. Sie können dann die Überprüfung der Signatur den richtigen öffentlichen Schlüssel mit dem angegebenen Algorithmus ausführen.

Außerhalb dieses Dokument wird beschrieben, wie die Validierung der Signatur. Zahlreiche open-Source-Bibliotheken stehen Ihnen dabei helfen, wenn Sie sie benötigen.

### <a name="validate-the-claims"></a>Überprüfen der Ansprüche
Erhält Ihre app oder API-ID-Token, sollte es auch mehrere gegen die Ansprüche im Token-ID durchführen. Diese sind jedoch nicht auf:

- **Zielgruppe** Anspruch: sicherstellen, dass das Token-ID für Ihre Anwendung zugewiesen werden soll.
- Die Ansprüche **vor** und **Ablaufzeit** : diese überprüfen, ob das Token-ID nicht abgelaufen ist.
- Der **Aussteller** Anspruch: sicherstellen, dass das Token an Ihre app Azure AD ausgestellt wurde.
- Die **Nonce**: eine Strategie für token Replay-Angriffen.

Eine vollständige Liste der Validierung ausführen Ihrer app sollte finden Sie [Spezifikation OpenID verbinden](https://openid.net). Details zu den erwarteten Werten für diese Ansprüche sind im vorherigen [Abschnitt token](#types-of-tokens)enthalten.  

## <a name="token-lifetimes"></a>Token Lebensdauer

Die folgenden token Lebensdauer werden bereitgestellt, um Ihre Kenntnisse zu erweitern. Sie helfen Ihnen beim Entwickeln und Debuggen von apps. Beachten Sie, dass Ihre apps auf diese Lebensdauer konstant erwarten nicht geschrieben werden soll. Sie werden ändern können und.  Erfahren Sie mehr über die Anpassung von token Lebensdauer in Azure AD B2C [hier](active-directory-b2c-token-session-sso.md).

| Token | Lebensdauer | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| ID-Token | Eine Stunde | ID-Token gelten in der Regel eine Stunde. Ihrer Anwendung können diese Lebensdauer zu eigenen Sessions mit Benutzern (empfohlen). Sie können auch eine andere Gültigkeitsdauer. Ihre Anwendung benötigt eine neue ID token, ist einfach zu einer neuen Anforderung-in Azure AD. Wenn ein Benutzer eine gültige Browsersitzung mit Azure hat Benutzer nicht müssen Anmeldedaten erneut eingeben. |
| Aktualisieren von Token | Bis zu 14 Tage | Ein einzelnes Aktualisierungstoken ist für maximal 14 Tage gültig. Ein Aktualisierungstoken kann jedoch verschiedene Gründe jederzeit ungültig werden. Ihre app sollte weiterhin versuchen, eine Aktualisierungstoken verwenden, bis die Anforderung fehlschlägt oder Ihre app Aktualisierungstoken durch einen neuen ersetzt.  Ein Aktualisierungstoken kann auch ungültig, wenn 90 Tage vergangen, seit der Benutzer zuletzt eingegebenen Anmeldeinformationen. |
| Autorisierungscodes | Fünf Minuten | Autorisierungscodes sind absichtlich kurzlebig. Sie sollten sofort eingelöst werden Zugriffstoken, ID-Token oder Aktualisierungstoken beim Empfang. |
