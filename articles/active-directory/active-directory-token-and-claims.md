 <properties
   pageTitle="Azure AD-Token Verweis | Microsoft Azure"
   description="Ein Handbuch für verstehen und Bewerten von Ansprüche von Azure Active Directory (AAD) ausgestellten Token SAML 2.0 und JSON Web Token (JWT)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD Tokenverweis

Azure Active Directory (Azure AD) gibt mehrere Arten von Sicherheitstokens bei der Verarbeitung der einzelnen Authentifizierungsablauf. Dieses Dokument beschreibt das Format Sicherheitsmerkmale und Inhalt jedes Token.

## <a name="types-of-tokens"></a>Typen von Token

Azure AD unterstützt das [OAuth 2.0 Authorization Protocol](active-directory-protocols-oauth-code.md), Access_tokens und Aktualisierungstoken nutzt.  Es unterstützt auch Authentifizierung und Anmeldung über [OpenID verbinden](active-directory-protocols-openid-connect-code.md), einen dritten Typ von Token, die ID-Token vorgestellt.  Jede dieser Token wird als "trägertoken" dargestellt.

Ein trägertoken ist ein einfaches Sicherheitstoken, den "Inhaber" den Zugriff auf eine geschützte Ressource gewährt. In diesem Sinne ist der "" eine Partei, die das Token darstellen kann. Wenn Authentifizierung mit Azure trägertoken erhalten erforderlich ist, müssen Schritte zu secure Token zum Abfangen einer unbeabsichtigten Partei zu verhindern. Da trägertoken keine integrierte Methode, um zu verhindern, dass unbefugte Verwendung verfügen, müssen sie in einem sicheren Kanal wie Transport Layer Security (HTTPS) transportiert werden. Ein trägertoken im Klartext übertragen, kann ein Man-in der mittleren Angriff Token und unberechtigten Zugriff auf eine geschützte Ressource verwendet werden. Sicherheitsgrundsätze gelten beim Speichern trägertoken zur späteren Verwendung zwischenspeichern. Immer sicherstellen Sie, dass Ihre app überträgt und trägertoken sicher speichert. Weitere Sicherheitsaspekte zu produzierende Token finden Sie in [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der von Azure AD ausgestellten Token werden als JSON Web Token oder JWTs implementiert.  Ein JWT ist ein kompaktes, URL-sichere Übertragung von Daten zwischen zwei Parteien.  In JWTs enthaltene Informationen werden als "Ansprüche" oder Assertionen von Informationen über den Inhaber und Betreff des Tokens bezeichnet.  Die Ansprüche in JWTs sind JSON-Objekten codiert und für die Übertragung serialisiert.  Da JWTs ausgestellt von Azure AD signiert, aber nicht verschlüsselt, können Sie problemlos den Inhalt einer JWT für Debugzwecke überprüfen.  Es gibt mehrere Tools für, wie [jwt.calebb.net](http://jwt.calebb.net). Weitere Informationen zu JWTs finden Sie in der [JWT-Spezifikation](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>ID-Token

ID-Token bilden anmelden Sicherheitstoken, das Ihre Anwendung erhält bei Authentifizierung mit [OpenID verbinden](active-directory-protocols-openid-connect-code.md).  Sie werden als [JWTs](#types-of-tokens)dargestellt und enthalten Angaben, mit denen Sie für Ihre Anwendung den Benutzer anmelden.  Die Ansprüche können in ein ID-Token wie gewünscht: sie werden häufig für Kontoinformationen anzeigen oder Entscheidungen Access Control in einer Anwendung verwendet.

ID-Token signiert jedoch nicht verschlüsselt.  Wenn Ihre Anwendung ein ID-Token erhält, muss [die Signatur überprüfen,](#validating-tokens) um das Token Authentizität zu belegen und einige Ansprüche im Token sich seine Gültigkeit zu überprüfen.  Die Ansprüche einer App überprüft variieren je nach Szenario, aber es gibt einige [häufig Behauptung Prüfung](#validating-tokens) , die Ihre Anwendung in jedem Szenario ausführen müssen.

Finden Sie die folgenden Informationen auf ID-Token, sowie ein Beispiel-ID-Token.  Beachten Sie, dass die Ansprüche im ID-Token nicht in einer bestimmten Reihenfolge zurückgegeben werden.  Darüber hinaus neue Ansprüche in ID-Token zu beliebigen Zeitpunkt entstehen - app sollte nicht unterbrechen, neuer Anträge eingeführt werden.  Die folgende Liste enthält die Ansprüche, die Ihre Anwendung zum Zeitpunkt der Erstellung dieses Dokuments zuverlässig interpretieren kann.  Ggf. mehr Details in der [Spezifikation OpenID Verbindung](http://openid.net/specs/openid-connect-core-1_0.html)finden.

#### <a name="sample-idtoken"></a>Beispiel-ID-Token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Übung versuchen Sie überprüfen der Ansprüche in der Beispiel-ID-Token in [calebb.net](http://jwt.calebb.net)eingefügt.

#### <a name="claims-in-idtokens"></a>Ansprüche im ID-Token

| JWT Antrag | Name | Beschreibung |
|-----------|------|-------------|
| `appid`| ID der Anwendung | Identifiziert die Anwendung, die das Token auf eine Ressource zuzugreifen. Die Anwendung kann selbst oder im Namen eines Benutzers agieren. Die ID in der Regel ein Application-Objekt darstellt, aber ein Service principal-Objekt kann auch in Azure AD darstellen. <br><br> **Beispiel JWT Wert**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Zielgruppe | Der Empfänger des Tokens. Die Anwendung, die das Token erhalten muss überprüfen Publikum Wert stimmt und alle Token für ein anderes Publikum bestimmt ablehnen. <br><br> **SAML Beispielwert**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Beispiel JWT Wert**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Anwendung Authentifizierung Kontextreferenz-Klasse | Gibt an, wie der Client authentifiziert wurde. Öffentliche Kunden ist der Wert 0. Client-ID und geheimen verwendet, ist der Wert 1. <br><br> **Beispiel JWT Wert**: <br> `"appidacr": "0"`|
| `acr`| Authentifizierung Kontextreferenz-Klasse | Gibt an, wie der Betreff Client Anwendung Authentifizierung Klasse Kontextreferenz Forderung authentifiziert wurde. "0" gibt an, dass die Endbenutzer Authentifizierung nicht die von ISO/IEC 29115 Anforderungen. <br><br> **Beispiel JWT Wert**: <br> `"acr": "0"`|
| | Instant Authentifizierung | Zeichnet Datum und Uhrzeit der Authentifizierung auftrat. <br><br> **SAML Beispielwert**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Authentifizierungsmethode | Gibt an, wie der Betreff des Tokens authentifiziert wurde. <br><br> **SAML Beispielwert**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Beispiel JWT Wert**:`“amr”: ["pwd"]` |
| `given_name`| Vorname | Stellt die erste oder "" Name des Benutzers, auf Azure AD-Benutzerobjekt. <br><br> **SAML Beispielwert**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `"given_name": "Frank"` |
| `groups`| Gruppen | Enthält die Objekt-IDs, die Gruppenmitgliedschaften des Antragstellers darstellen. Diese Werte sind eindeutig (Siehe Objekt-ID) und zur Verwaltung von, wie erzwingt die Autorisierung zum Zugriff auf eine Ressource sicher verwendet werden können. Die Gruppen in Gruppen Anspruch werden jeweils pro Anwendung über die Eigenschaft "GroupMembershipClaims" des Anwendungsmanifests konfiguriert. Ein Wert von Null werden alle Gruppen ausgeschlossen, enthält der Wert "SecurityGroup" nur Active Directory-Sicherheitsgruppen-Mitgliedschaft und den Wert "alle" Sicherheitsgruppen und Verteilerlisten von Office 365 enthalten. <br><br> **SAML Beispielwert**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Identitätsanbieter | Zeichnet den Identitätsanbieter, der den Betreff des Tokens authentifiziert. Dieser Wert entspricht der Wert des Anspruchs Aussteller, sofern das Benutzerkonto in einem anderen Mandanten als den Aussteller ist. <br><br> **SAML Beispielwert**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Speichert den Zeitpunkt, an dem das Token ausgestellt wurde. Es wird häufig verwendet, um token frische messen. <br><br> **SAML Beispielwert**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Beispiel JWT Wert**: <br> `"iat": 1390234181` |
| `iss` | Aussteller | Identifiziert den Sicherheitstokendienst (STS), der erstellt und gibt das Token zurück. Der Aussteller ist in Azure AD gibt Token, sts.windows.net. Die GUID in der Anspruchswert Aussteller ist die Mandanten-ID Azure AD-Verzeichnis. Die Mandanten-ID ist ein Bezeichner unveränderlich und zuverlässige des Verzeichnisses. <br><br> **SAML Beispielwert**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Beispiel JWT Wert**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Nachname | Bietet letzten Namen, Nachnamen oder Familienname des Benutzers in Azure AD-Benutzerobjekt definiert. <br><br> **SAML Beispielwert**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `"family_name": "Miller"` |
| `unique_name`| Name | Stellt einen Menschen lesbaren Wert, der Gegenstand der Token identifiziert. Dieser Wert ist nicht unbedingt im Mandant eindeutig und dient nur zu Anzeigezwecken verwendet. <br><br> **SAML Beispielwert**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Objekt-ID | Enthält einen eindeutigen Bezeichner eines Objekts in Azure AD. Dieser Wert ist unveränderlich und kann nicht neu zugewiesen oder wiederverwendet werden. Verwenden Sie die Objekt-ID zum Identifizieren eines Objekts in Azure AD-Abfragen. <br><br> **SAML Beispielwert**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Rollen | Stellt alle Anwendungsrollen, die der Betreff erteilt direkt und indirekt über Gruppenmitgliedschaft wird rollenbasierte Zugriffskontrolle erzwingen. Anwendungsrollen sind jeweils pro Anwendung definiert, durch die `appRoles` -Eigenschaft des Anwendungsmanifests. Die `value` -Eigenschaft jeder Anwendungsrolle ist der Wert der Forderung Rollen. <br><br> **SAML Beispielwert**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Gültigkeitsbereich | Gibt die Identitätswechsel Berechtigungen an die Clientanwendung. Die Standardberechtigung `user_impersonation`. Der Besitzer der gesicherten Ressource kann zusätzliche Werte in Azure AD registrieren. <br><br> **Beispiel JWT Wert**: <br> `"scp": "user_impersonation"`|
| `sub` |Betreff| Gibt den Prinzipal über dem Token Informationen wie der Benutzer einer Anwendung bestimmt. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet, so verwendet werden, sichere Autorisierung überprüft werden soll. Da das Thema immer im Token Azure AD Probleme vorhanden ist, sollten mithilfe dieses Werts in eine allgemeine Autorisierungssystem. <br> `SubjectConfirmation`ist kein Anspruch. Es beschreibt, wie der Betreff des Tokens überprüft wird. `Bearer`Gibt an, dass deren Besitz des Tokens Betreff bestätigt. <br><br> **SAML Beispielwert**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Beispiel JWT Wert**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Mandanten-ID | Ein unveränderlich, Einweg Bezeichner, der Pächter Verzeichnis identifiziert, die das Token ausgestellt hat. Diesen Wert können Sie in einer mehrinstanzenfähigen Anwendung mandantenspezifische Verzeichnis zugreifen. Diesen Wert können Sie Mieter in einem Aufruf an die Graph-API identifizieren. <br><br> **SAML Beispielwert**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Beispiel JWT Wert**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Gültigkeitsdauer des Tokens | Definiert das Zeitintervall, in dem ein Token gültig ist. Der Dienst, der das Token überprüft sollten überprüfen, ob das aktuelle Datum innerhalb der Gültigkeitsdauer des Tokens, sonst wird das Token abzulehnen. Der Dienst können bis zu fünf Minuten außerhalb Tokengültigkeitsdauer alle Unterschiede zwischen in Systemzeit ("Abweichung") zwischen Azure und den Dienst. <br><br> **SAML Beispielwert**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Beispiel JWT Wert**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Benutzerprinzipalnamen | Speichert den Namen des Benutzerprinzipals.<br><br> **Beispiel JWT Wert**: <br> `"upn": frankm@contoso.com`|
| `ver`| Version | Speichert die Versionsnummer des Tokens. <br><br> **Beispiel JWT Wert**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Zugriffstoken

Zugriffstoken werden zu diesem Zeitpunkt nur von Microsoft Services.  Apps müssen keine Validierung oder Überprüfung von Zugriffstoken für alle derzeit unterstützten Szenarien führen.  Zugriffstoken als transparent behandeln - nur Zeichenfolgen Ihre app in HTTP-Anfragen an Microsoft übergeben können.

Beim Anfordern eines Zugriffstokens gibt Azure AD auch einige Metadaten über das Zugriffstoken für Ihre app Verbrauch.  Diese Informationen umfassen die Ablaufzeit eines Zugriffstoken und Bereiche für die gültig ist.  Dadurch wird Ihre Anwendung ausführen, intelligenten caching Zugriffstoken ohne Öffnen Analysieren Zugriffstoken selbst.

## <a name="refresh-tokens"></a>Aktualisieren von Token

Aktualisieren Token sind Sicherheitstoken Ihrer app zu neuen Zugriffstoken in einen Fluss OAuth 2.0 verwenden kann.  Sie können Ihre app zu langfristigen Zugriff auf Ressourcen im Auftrag eines Benutzers ohne Benutzereingriffe.

Aktualisierungstoken sind mehrere Ressourcen sie möglicherweise während einer token-Anforderung für eine Ressource erhalten, aber Zugriffstoken zu einer völlig anderen Ressource eingelöst. Um mehreren Ressourcen anzugeben, legen die `resource` Parameter in der Anforderung an die Zielressource.

Aktualisierungstoken sind transparent für Ihre Anwendung. Sie sind langlebig, aber Ihre app sollte nicht erwarten, dass ein Aktualisierungstoken längere Zeit dauern geschrieben werden.  Aktualisierungstoken können jederzeit eine Vielzahl von Gründen ungültig sein.  Die einzige Möglichkeit für Ihre Anwendung wissen, ob eine Aktualisierungstoken gültig ist ist der Versuch durch ein token Anforderung Azure AD token Endpunkt einlösen.

Beim einlösen Aktualisierungstoken für ein neues Zugriffstoken erhalten Sie ein neues Aktualisierungstoken token auf.  Speichern Sie das Aktualisierungstoken neu ersetzt, die in der Anforderung verwendet.  Garantiert, dass Ihr Aktualisierungstoken so lange gültig.

## <a name="validating-tokens"></a>Überprüfen von Token

Zu diesem Zeitpunkt ist nur Prüfung müssen die Clientanwendung ausführen ID-Token überprüft.  Um ein ID-Token zu überprüfen, sollten Ihre app das ID-Token-Signatur und die Ansprüche in der ID-Token überprüfen.

Wir bieten Bibliotheken und Codebeispiele, die problemlos token Validierung veranschaulichen möchten Sie den zugrunde liegenden Prozess verstehen.  Es stehen verschiedene Drittanbieter-open-Source-Bibliotheken für JWT Prüfung, mindestens eine Option für fast jede Plattform und Sprache. Weitere Informationen zu Azure AD-Authentifizierung Codebeispiele finden Sie in [Azure AD-authentifizierungsbibliotheken](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Überprüfen der Signatur

Ein JWT enthält drei Segmente von getrennt den `.` Zeichen.  Das erste Segment wird der zweite als **Text**und als **Signatur**dritten als **Kopfzeile**bezeichnet.  Segment Signatur kann verwendet werden, die Echtheit der ID-Token überprüft, damit Ihre Anwendung vertrauen können.

ID-Token sind mit Industry standard Asymmetrische Verschlüsselungsalgorithmen wie RSA 256 signiert. Der Header der ID-Token enthält Informationen der Schlüssel und Verschlüsselung zum Signieren von Token verwendet:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Die `alg` Anspruch gibt den Algorithmus, der verwendet wurde, die token While die `x5t` Anspruch gibt den bestimmten öffentlichen Schlüssel, mit dem das Token signiert wurde.

Azure AD kann jederzeit Zeit ein ID-Token mit einer Reihe von öffentlich-Private Schlüsselpaare anmelden. Azure AD dreht kann mehrere Schlüssel in regelmäßigen Abständen Ihre app automatisch mit den geänderten geschrieben werden soll.  Eine angemessene Häufigkeit nach Updates auf die öffentlichen Schlüssel von Azure AD verwendet wird innerhalb von 24 Stunden.

Überprüfung die Signatur mit der OpenID verbinden Metadatendokument erforderlich Signieren Schlüsseldaten erwerben:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Versuchen Sie diese URL in einem Browser!

Dieses Metadatendokument ist ein JSON-Objekt mit mehreren nützliche Informationen wie die Position der verschiedenen Endpunkte verbinden OpenID Authentifizierung erforderlich.  

Auch ein `jwks_uri`, wodurch Position festgelegten öffentlichen Schlüssel zum Signieren von Token verwendet.  Das JSON-Dokument befindet sich an der `jwks_uri` enthält alle Informationen zum öffentlichen Schlüssel verwendet, bestimmten Zeitpunkt.  Ihre Anwendung kann die `kid` Anspruch im JWT-Header auf die öffentlicher Schlüssel in diesem Dokument verwendet wurde, um ein bestimmtes Token signiert.  Die Überprüfung der Signatur den richtigen öffentlichen Schlüssel mit dem angegebenen Algorithmus können Sie ausführen.

Die Überprüfung der Signatur ist außerhalb des Bereichs dieses Dokuments - stehen zahlreiche open-Source-Bibliotheken helfen Sie bei Bedarf.

#### <a name="validating-the-claims"></a>Überprüfen der Ansprüche

Erhält Ihre app ein ID-Token auf Benutzer anmelden, sollte es auch einige der Ansprüche in der ID-Token durchführen.  Dazu gehören unter anderem sind:

  - Die **Zielgruppe** Anspruch - sicherstellen, dass das ID-Token für Ihre Anwendung zugewiesen werden soll.
  - Die **Nicht vor** und **Ablaufzeit** Ansprüche - sicherstellen, dass das ID-Token nicht abgelaufen ist.
  - Der **Aussteller** Anspruch - überprüfen, ob Ihre App von Azure AD tatsächlich das Token ausgestellt wurde.
  - Die **Nonce** - um token Replay-Angriff zu verringern.
  - und vieles mehr...

Eine vollständige Liste der Anspruch Prüfung ausführen Ihrer app sollte finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details zu den erwarteten Werten für diese Ansprüche sind in der vorhergehenden Abschnitt [ID-Token](#id-tokens) enthalten.

## <a name="sample-tokens"></a>Beispiel-Token

Dieser Abschnitt enthält Beispiele von SAML und JWT-Token, die Azure AD zurückgibt. In diesen Beispielen können Sie die Ansprüche im Kontext sehen.
SAML-Token

Dies ist ein Beispiel für eine normale SAML-Token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT-Token - Benutzeridentitätswechsel

Dies ist ein Beispiel für eine normale JSON Web Token (JWT) eine Zulassung erteilen Codefluss verwendet.
Das Token enthält sowie eine **Ver** und **Appidacr**, die Authentifizierung Klasse Kontextreferenz, die angibt, wie der Client authentifiziert wurde. Öffentliche Kunden ist der Wert 0. Wenn eine Client-ID oder geheimen verwendet wurde, ist der Wert 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Verwandte Inhalte
- Finden Sie weitere Informationen zum Verwalten von token Lebensdauerrichtlinien API Azure AD Graph Azure AD Graph [Operationen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) und [Policy-Entität](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity).
- Weitere Informationen und Beispiele zum Verwalten von Richtlinien über PowerShell-Cmdlets, sowie Beispiele finden Sie unter [konfigurierbare token Lebensdauer in Azure AD](active-directory-configurable-token-lifetimes.md). 
