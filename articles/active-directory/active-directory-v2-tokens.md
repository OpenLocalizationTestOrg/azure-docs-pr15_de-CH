<properties
    pageTitle="Azure AD v2. 0 Tokenverweis | Microsoft Azure"
    description="Typen von v2. 0-Endpunkt ausgegebenen Tokens und"
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="v20-token-reference"></a>Tokenverweis v2. 0

V2. 0-Endpunkt gibt verschiedene Arten von Sicherheitstoken in der Verarbeitung jeder [Authentifizierungsablauf](active-directory-v2-flows.md). Dieses Dokument beschreibt das Format Sicherheitsmerkmale und Inhalt jedes Token.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Typen von Token

V2. 0-Endpunkt unterstützt das [OAuth 2.0 Authorization Protocol](active-directory-v2-protocols.md), Access_tokens und Aktualisierungstoken nutzt.  Es unterstützt auch Authentifizierung und Anmeldung über [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow), einen dritten Typ von Token, die ID-Token vorgestellt.  Jede dieser Token wird als "trägertoken" dargestellt.

Ein trägertoken ist ein einfaches Sicherheitstoken, den "Inhaber" den Zugriff auf eine geschützte Ressource gewährt. In diesem Sinne ist der "" eine Partei, die das Token darstellen kann. Wenn eine Partei erforderlichen Schritte zum Sichern des Tokens in der Übertragung und Speicherung nicht getroffen sind zunächst mit Azure trägertoken zu authentifizieren muss können sie abgefangen und durch einen unerwünschten Dritten verwendet. Zwar einige Sicherheitstoken einen integrierten Mechanismus zum verhindern, dass unbefugte Verwendung trägertoken verfügen über diesen Mechanismus und einen sicheren Kanal wie Transport Layer Security (HTTPS) transportiert werden müssen. Ein trägertoken im Klartext übertragen, kann ein Man-in der mittleren Angriff verwendet werden bösartige Angriffe Token und für nicht autorisierten Zugriff auf eine geschützte Ressource verwenden. Sicherheitsgrundsätze gelten beim Speichern trägertoken zur späteren Verwendung zwischenspeichern. Immer sicherstellen Sie, dass Ihre app überträgt und trägertoken sicher speichert. Weitere Sicherheitsaspekte zu produzierende Token finden Sie in [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der von v2. 0-Endpunkt ausgestellten Token werden als Json Web Token oder JWTs implementiert.  Ein JWT ist ein kompaktes, URL-sichere Übertragung von Daten zwischen zwei Parteien.  In JWTs enthaltene Informationen werden als "Ansprüche" oder Assertionen von Informationen über den Inhaber und Betreff des Tokens bezeichnet.  Die Ansprüche in JWTs sind JSON-Objekten codiert und für die Übertragung serialisiert.  Da v2. 0-Endpunkt ausgestellten JWTs signiert, aber nicht verschlüsselt, können Sie problemlos den Inhalt einer JWT für Debugzwecke überprüfen. Weitere Informationen zu JWTs finden Sie in der [JWT-Spezifikation](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>ID-Token

ID-Token bilden anmelden Sicherheitstoken, das Ihre Anwendung erhält bei Authentifizierung mit [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Sie werden als [JWTs](#types-of-tokens)dargestellt und enthalten Angaben, mit denen Sie für Ihre Anwendung den Benutzer anmelden.  Die Ansprüche können in ein ID-Token wie gewünscht: sie werden häufig für Kontoinformationen anzeigen oder Entscheidungen Access Control in einer Anwendung verwendet.  V2. 0-Endpunkt stellt nur eine Art von ID-Token, die einen konsistenten Satz von Ansprüchen unabhängig von der Benutzer angemeldet hat.  Das bedeutet, Sie werden Format und Inhalt der ID-Token für beide Microsoft Account Privatanwender und Geschäfts-oder.

ID-Token signiert jedoch nicht verschlüsselt.  Wenn Ihre Anwendung ein ID-Token erhält, muss [die Signatur überprüfen,](#validating-tokens) um das Token Authentizität zu belegen und einige Ansprüche im Token sich seine Gültigkeit zu überprüfen.  Die Ansprüche einer App überprüft variieren je nach Szenario, aber es gibt einige [häufig Behauptung Prüfung](#validating-tokens) , die Ihre Anwendung in jedem Szenario ausführen müssen.

Einzelheiten der Ansprüche im ID-Token erhalten, sowie ein Beispiel-ID-Token.  Beachten Sie, dass die Ansprüche im ID-Token nicht in einer bestimmten Reihenfolge zurückgegeben werden.  Darüber hinaus neue Ansprüche in ID-Token zu beliebigen Zeitpunkt entstehen - app sollte nicht unterbrechen, neuer Anträge eingeführt werden.  Die folgende Liste enthält die Ansprüche, die Ihre Anwendung zum Zeitpunkt der Erstellung dieses Dokuments zuverlässig interpretieren kann.  Ggf. mehr Details in der [Spezifikation OpenID Verbindung](http://openid.net/specs/openid-connect-core-1_0.html)finden.

#### <a name="sample-idtoken"></a>Beispiel-ID-Token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Übung versuchen Sie überprüfen der Ansprüche in der Beispiel-ID-Token in [calebb.net](https://calebb.net)eingefügt.

#### <a name="claims-in-idtokens"></a>Ansprüche im ID-Token
| Name | Forderung | Beispiel für einen Wert | Beschreibung |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Zielgruppe | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Identifiziert den Empfänger des Tokens.  Im ID-Token ist das Publikum Ihre app Anwendung Id Ihrer app im Portal Registrierung app zugewiesen wurde.  Ihre Anwendung sollte diesen Wert überprüfen und Token ablehnen, wenn es nicht übereinstimmt. |
| Aussteller | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Identifiziert den Sicherheitstokendienst (STS), der erstellt und das Token sowie die Azure AD-Mandanten, in dem der Benutzer authentifiziert wurde.  Ihre app sollte Aussteller Anspruch darauf kam das Token vom Endpunkt v2. 0 überprüft werden.  Es sollte auch der Guid-Anteil der Forderung Mieter beschränken die Anwendung anmelden dürfen.  Die Guid, die den Benutzer ist ein Benutzer Consumer von Microsoft-Konto ist `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Ausgestellt am | `iat` | `1452285331` | Gleichzeitig mit das Token ausgestellt wurde, dargestellt rechtzeitig Epoche. |
| Ablaufzeit | `exp` | `1452289231` | Die Zeit, die das Token ungültig ist, wird, dargestellt in Epoche Zeit.  Ihre app verwenden dafür die Gültigkeitsdauer des Tokens Gültigkeit.  |
| Nicht vor | `nbf` | `1452285331` |  Die Zeit, die das Token gültig ist, wird, dargestellt in Epoche Zeit. Es ist normalerweise gleichzeitig Ausgabe identisch.  Ihre app verwenden dafür die Gültigkeitsdauer des Tokens Gültigkeit.  |
| Version | `ver` | `2.0` | Die Version der ID-Token, von Azure AD definiert.  Für den Endpunkt v2. 0 der Wert werden `2.0`. |
| Mandanten-Id | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Eine Guid für Azure Anzeige Mieter ist der Benutzer aus.  Arbeits- und Konten werden die Guid unveränderlich Mandanten-ID der Organisation, zu der der Benutzer gehört.  Für Persönliche Konten werden der Wert `9188040d-6c67-4c5b-b112-36a304b66dad`.  Die `profile` Bereich dieser Anspruch erforderlich. |
| Code-Hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Hash Code ist nur bei der ID-Token neben einen Autorisierungscode OAuth 2.0 im ID-Token enthalten.  Hiermit können Sie die Echtheit einer Autorisierungscode überprüfen.  [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) für Weitere Informationen über die Durchführung dieser Überprüfung anzeigen |
| Access Token Hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Access token Hash ist nur bei der ID-Token neben Zugriffstoken OAuth 2.0 im ID-Token enthalten.  Hiermit können Sie die Authentizität eines Zugriffstokens überprüft.  [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) für Weitere Informationen über die Durchführung dieser Überprüfung anzeigen |
| Nonce | `nonce` | `12345` | Die Nonce ist eine Strategie zur Minderung token Replay-Angriffe.  Ihre app können Nonce in eine Authentifizierungsanfrage mithilfe der `nonce` Parameter Abfragen.  Der Wert in der Anforderung in der ID-Token ausgegebenen `nonce` Anspruch unverändert.  Dadurch wird Ihre app überprüfen den Wert mit dem Wert in der Anforderung angegebene angegebene ID-Token der app-Sitzung zuordnet.  Ihre Anwendung sollte diese Validierung während der Validierung ID-Token. |
| Name | `name` | `Babe Ruth` | Der Anspruch stellt einen Menschen lesbaren Wert, der Gegenstand der Token identifiziert. Dieser Wert wird nicht garantiert eindeutig, änderbar und dient nur zu Anzeigezwecken verwendet.  Die `profile` Bereich dieser Anspruch erforderlich. |
| E-Mail | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Die e-Mail-Adresse das Benutzerkonto zugeordnet, sofern vorhanden.  Der Wert ist veränderbar und möglicherweise ändern für einen bestimmten Benutzer.  Die `email` Bereich dieser Anspruch erforderlich. |
| Bevorzugte Benutzername | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Der primäre Benutzername, der für den Benutzer in V2. 0-Endpunkt verwendet wird.  Es wäre eine e-Mail-Adresse, Telefonnummer oder einen generischen Benutzernamen ohne angegebenen Format.  Der Wert ist veränderbar und möglicherweise ändern für einen bestimmten Benutzer.  Die `profile` Bereich dieser Anspruch erforderlich. |
| Betreff | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Der Prinzipal über die Token Informationen wie der Benutzer einer Anwendung bestimmt. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet, so verwendet werden Autorisierung überprüft ausführen, wenn das Token verwendet wird, auf eine Ressource zuzugreifen. Da das Thema immer im Token Azure AD Probleme vorhanden ist, sollten mithilfe dieses Werts in eine allgemeine Autorisierungssystem. |
| ObjectId | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Die Objekt-Id des Kontos Arbeits- oder Schulcomputer in Azure AD-System.  Dies erfolgt nicht für persönlichen Microsoft-Konten.  Die `profile` Bereich dieser Anspruch erforderlich. |


## <a name="access-tokens"></a>Zugriffstoken

V2. 0-Endpunkt ausgestellte Zugriffstoken sind zu diesem Zeitpunkt nur von Microsoft Services.  Apps müssen keine Validierung oder Überprüfung von Zugriffstoken für alle derzeit unterstützten Szenarien führen.  Zugriffstoken als transparent behandeln - nur Zeichenfolgen Ihre app in HTTP-Anfragen an Microsoft übergeben können.

V2. 0-Endpunkt wird in naher Zukunft die Möglichkeit Ihre app Zugriffstoken von anderen Clients empfangen einführen.  Zu diesem Zeitpunkt werden diese Informationen mit den Informationen aktualisiert Ihre Anwendung Access token Validierung und ähnlichen Aufgaben durchführen muss.

Beim Anfordern eines Zugriffstokens v2. 0-Endpunkt gibt v2. 0-Endpunkt auch einige Metadaten über das Zugriffstoken für Ihre app Verbrauch.  Diese Informationen umfassen die Ablaufzeit eines Zugriffstoken und Bereiche für die gültig ist.  Dadurch wird Ihre Anwendung ausführen, intelligenten caching Zugriffstoken ohne Öffnen Analysieren Zugriffstoken selbst.

## <a name="refresh-tokens"></a>Aktualisieren von Token

Aktualisieren von Token sind Sicherheitstoken der app neu erwerben können Token in einer fortlaufenden OAuth 2.0 zugreifen.  Sie können Ihre app zu langfristigen Zugriff auf Ressourcen im Auftrag eines Benutzers ohne Benutzereingriffe.

Aktualisierungstoken sind mehrere Ressourcen.  Das heißt werden, während ein token Anforderung für eine Ressource aktualisieren Zeichen kann eingelöst Zugriffstoken zu einer völlig anderen Ressource.

Um eine Aktualisierung ein token erhalten, Ihre app müssen und gewährt den `offline_acesss` Bereich.   Weitere Informationen zu den `offline_access` Bereich, [Zustimmung & Bereiche Artikel](active-directory-v2-scopes.md)verweisen.

Token aktualisiert werden und werden immer, Ihre Anwendung transparent.  Sie können Azure AD v2. 0-Endpunkt ausgestellten und nur geprüft und v2. 0-Endpunkt interpretiert werden.  Sie sind langlebig, aber Ihre app sollte nicht erwarten, dass ein Aktualisierungstoken längere Zeit dauern geschrieben werden.  Aktualisierungstoken können jederzeit eine Vielzahl von Gründen ungültig sein.  Die einzige Möglichkeit für Ihre Anwendung wissen, ob eine Aktualisierungstoken gültig ist ist der Versuch durch ein token Anforderung an v2. 0-Endpunkt einlösen.

Beim einlösen Aktualisierungstoken für ein neues Zugriffstoken (und Ihre app Gewährung der `offline_access` Bereich), erhalten Sie ein neues Aktualisierungstoken token auf.  Speichern Sie das Aktualisierungstoken neu ersetzt, die in der Anforderung verwendet.  Garantiert, dass Ihr Aktualisierungstoken so lange gültig.

## <a name="validating-tokens"></a>Überprüfen von Token

Zu diesem Zeitpunkt ist nur Überprüfung müssen Ihre apps ausführen ID-Token überprüft.  Um ein ID-Token zu überprüfen, sollten Ihre app das ID-Token-Signatur und die Ansprüche in der ID-Token überprüfen.

<!-- TODO: Link -->
Wir bieten Bibliotheken und Codebeispiele, die veranschaulichen, wie token Prüfung – einfache Handhabung der Informationen ist einfach unten für den zugrunde liegenden Prozess verstehen wollen.  Gibt es auch mehrere 3rd Party open Source-Bibliotheken für die JWT Validierung – gibt es mindestens eine Option für fast jede Plattform und Sprache gibt.

#### <a name="validating-the-signature"></a>Überprüfen der Signatur
Ein JWT enthält drei Segmente von getrennt den `.` Zeichen.  Das erste Segment wird der zweite als **Text**und als **Signatur**dritten als **Kopfzeile**bezeichnet.  Segment Signatur kann verwendet werden, die Echtheit der ID-Token überprüft, damit Ihre Anwendung vertrauen können.

ID-Token sind mit Industry standard Asymmetrische Verschlüsselungsalgorithmen wie RSA 256 signiert. Der Header der ID-Token enthält Informationen der Schlüssel und Verschlüsselung zum Signieren von Token verwendet:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Die `alg` Anspruch gibt den Algorithmus, der verwendet wurde, die token While die `kid` Anspruch gibt den bestimmten öffentlichen Schlüssel, mit dem das Token signiert wurde.

V2. 0-Endpunkt kann jederzeit Zeit ein ID-Token mit einer Reihe von öffentlich-Private Schlüsselpaare anmelden.  V2. 0-Endpunkt dreht kann mehrere Schlüssel in regelmäßigen Abständen Ihre app automatisch mit den geänderten geschrieben werden soll.  Eine angemessene Häufigkeit nach Updates auf die öffentlichen Schlüssel von v2. 0-Endpunkt verwendet ist 24 Stunden.

Überprüfung die Signatur mit der OpenID verbinden Metadatendokument erforderlich Signieren Schlüsseldaten erwerben:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Versuchen Sie diese URL in einem Browser!

Dieses Metadatendokument ist ein JSON-Objekt mit mehreren nützliche Informationen wie die Position der verschiedenen Endpunkte verbinden OpenID Authentifizierung erforderlich.  

Auch ein `jwks_uri`, wodurch Position festgelegten öffentlichen Schlüssel zum Signieren von Token verwendet.  Das JSON-Dokument befindet sich an der `jwks_uri` enthält alle Informationen zum öffentlichen Schlüssel verwendet, bestimmten Zeitpunkt.  Ihre Anwendung kann die `kid` Anspruch im JWT-Header auf die öffentlicher Schlüssel in diesem Dokument verwendet wurde, um ein bestimmtes Token signiert.  Die Überprüfung der Signatur den richtigen öffentlichen Schlüssel mit dem angegebenen Algorithmus können Sie ausführen.

Die Überprüfung der Signatur ist außerhalb des Bereichs dieses Dokuments - stehen zahlreiche open-Source-Bibliotheken helfen Sie bei Bedarf.

#### <a name="validating-the-claims"></a>Überprüfen der Ansprüche
Erhält Ihre app ein ID-Token auf Benutzer anmelden, sollte es auch einige der Ansprüche in der ID-Token durchführen.  Dazu gehören unter anderem sind:

- Die **Zielgruppe** Anspruch - sicherstellen, dass das ID-Token für Ihre Anwendung zugewiesen werden soll.
- Die **Nicht vor** und **Ablaufzeit** Ansprüche - sicherstellen, dass das ID-Token nicht abgelaufen ist.
- Der **Aussteller** Anspruch - überprüfen, ob Ihre App von v2. 0-Endpunkt tatsächlich Token ausgestellt wurde.
- Die **Nonce** - als token Replay-Angriffen.
- und vieles mehr...

Eine vollständige Liste der Anspruch Prüfung ausführen Ihrer app sollte finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details zu den erwarteten Werten für diese Ansprüche sind oben im [Abschnitt ID-Token](#id_tokens)enthalten.


## <a name="token-lifetimes"></a>Token Lebensdauer

Die folgenden token Lebensdauer dienen ausschließlich für Ihr Verständnis hilft entwickeln und Debuggen von apps.  Apps sollten nicht in diese Lebensdauer konstant erwarten geschrieben werden – sie können und Uhrzeit jederzeit ändern.

| Token | Lebensdauer | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| ID-Token (Arbeits- oder Schulcomputer Konten) | 1 Stunde | ID-Token gelten in der Regel eine Stunde.  Ihrer Anwendung verwenden diese dieselbe Lebensdauer dabei eigene Sitzung mit dem Benutzer (empfohlen), oder wählen Sie eine anderes Sitzungsdauer.  Ihre app muss eine neue ID-Token zu erhalten, muss sie einfach neue Anmelden die v2. 0 Endpunkt autorisieren anfordern.  Verfügt der Benutzer über eine gültige Browser-Sitzung mit der Version 2.0, können sie ihre Anmeldeinformationen erneut eingeben nicht erforderlich. |
| ID-Token (Persönliche Konten) | 24 Stunden | ID-Token für Persönliche Konten gelten normalerweise 24 Stunden.  Ihrer Anwendung verwenden diese dieselbe Lebensdauer dabei eigene Sitzung mit dem Benutzer (empfohlen), oder wählen Sie eine anderes Sitzungsdauer.  Ihre app muss eine neue ID-Token zu erhalten, muss sie einfach neue Anmelden die v2. 0 Endpunkt autorisieren anfordern.  Verfügt der Benutzer über eine gültige Browser-Sitzung mit der Version 2.0, können sie ihre Anmeldeinformationen erneut eingeben nicht erforderlich. |
| Zugriffstoken (Arbeits- oder Schulcomputer Konten) | 1 Stunde | Token Antworten als token Metadaten angegeben. |
| Zugriffstoken (Persönliche Konten) | 1 Stunde | Token Antworten als token Metadaten angegeben.  Access_tokens im Namen persönlicher Konten können für unterschiedliche Lebensdauer konfiguriert eine Stunde ist normalerweise der Fall |
| Aktualisieren von Token (Arbeits- oder Schulcomputer Konto) | Bis zu 14 Tage | Ein einzelnes Aktualisierungstoken ist für maximal 14 Tage gültig.  Jedoch werden Aktualisierungstoken jederzeit für eine beliebige Anzahl von Gründen ungültig, weiterhin Ihre app zu Aktualisierungstoken verwenden, bis sie fehlschlägt oder Ihre Anwendung mit einer neuen Aktualisierung ersetzt.  Ein Aktualisierungstoken werden auch ungültig, wenn es 90 Tage seit der Benutzer ihre Anmeldeinformationen eingegeben hat. |
| Aktualisieren von Token (Persönliche Konten) | Bis zu 1 Jahr | Ein einzelnes Aktualisierungstoken ist für maximal 1 Jahr gültig.  Jedoch werden Aktualisierungstoken jederzeit für eine beliebige Anzahl von Gründen ungültig, weiterhin Ihre app zu Aktualisierungstoken verwenden, bis sie fehlschlägt. |
| Autorisierungscodes (Arbeits- oder Schulcomputer Konten) | 10 Minuten | Autorisierungscodes gezielt kurzlebig und sollte sofort für Access_tokens und Aktualisierungstoken eingelöst werden, wenn sie eingehen. |
| Autorisierungscodes (Persönliche Konten) | 5 Minuten | Autorisierungscodes gezielt kurzlebig und sollte sofort für Access_tokens und Aktualisierungstoken eingelöst werden, wenn sie eingehen.  Autorisierungscodes im Namen persönlicher Konten sind auch einmal. |
