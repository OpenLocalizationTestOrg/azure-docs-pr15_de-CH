<properties
    pageTitle="Implizite Fluss von Azure AD v2. 0 | Microsoft Azure"
    description="Building Web Applications Azure AD v2. 0-Implementierung des impliziten Flusses Seite Apps verwenden."
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

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>Protokolle - SPA mit der impliziten v2. 0
Mit dem Endpunkt v2. 0 können Sie Benutzer in Ihrer Seite apps mit persönlichen und Arbeit-Schule von Microsoft anmelden.  Seite und andere JavaScript-apps ausführen, vor allem vor einem Browser einige interessante Herausforderung bei Authentifizierung:

- Die Sicherheitseigenschaften dieser Apps sind erheblich von herkömmlichen serverbasierten Web Applications.
- Viele autorisierungsserver und Identitätsanbieter unterstützt CORS Anfragen nicht.
- Ganze seitenbrowser leitet invasiv insbesondere die Benutzerfunktionalität App.

Diese Programme (vorstellen: AngularJS, Ember.js, React.js, usw.) Azure AD die implizite Gewährung von OAuth 2.0 unterstützt.  Implizite Fluss beschreibt [OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749#section-4.2).  Der Hauptvorteil ist, dass es die app zu Token von Azure AD ohne einen Backend-Server Austausch von Anmeldeinformationen.  Dadurch wird die Anwendung den Benutzer anmelden, Sitzung beibehalten, und Token andere Web APIs innerhalb der Client JavaScript-Code.  Es gibt einige wichtige Sicherheitsaspekte zu berücksichtigen, wenn implizite Flow - speziell auf [Client](http://tools.ietf.org/html/rfc6749#section-10.3) und [Benutzeridentitätswechsel](http://tools.ietf.org/html/rfc6749#section-10.3)verwenden.

Möchten Sie implizite Fluss und Azure AD verwenden, um Ihre JavaScript-Anwendung Authentifizierung hinzugefügt, empfehlen wir unsere open Source JavaScript-Bibliothek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js)verwenden.  Es gibt einige AngularJS-Tutorials [hier](active-directory-appmodel-v2-overview.md#getting-started) auf der Einstieg.  

Allerdings möchten Sie nicht zu einer Bibliothek in Ihrer app Seite Nachrichten zu senden, gehen Sie Allgemein.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Protokoll-Diagramm
Gesamte implizite Anmeldung im Fluss sieht - Schritte werden im folgenden ausführlich beschrieben.

![OpenId Verantwortlichkeitsbereiche verbinden](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Senden der Anforderung

Um Ihre app zunächst den Benutzer anmelden, können Sie senden eine Authentifizierungsanfrage [OpenID verbinden](active-directory-v2-protocols-oidc.md) und erhalten eine `id_token` vom Endpunkt v2. 0:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Klicken Sie auf den Link unten, um diese Anforderung auszuführen. Nach der Anmeldung Ihr Browser sollte weitergeleitet `https://localhost/myapp/` mit einem `id_token` in der Adressleiste.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/Common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind `common`, `organizations`, `consumers`, und Mieter Bezeichner.  Weitere Informationen finden Sie unter [Protokoll Grundlagen](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| Antworttyp | Erforderlich | Muss `id_token` für Verbindung OpenID anmelden.  Enthält auch den Antworttyp `token`. Mit `token` hier Ihre app Zugriffstoken vom Endpunkt autorisieren umgehend ohne eine zweite Anforderung an den Endpunkt autorisieren können.  Verwenden Sie die `token` Antworttyp, der `scope` Parameter muss einen Bereich, der angibt, welche Ressource für Token ausstellen. |
| redirect_uri | empfohlen | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen.  Sie müssen eines der Redirect_uris exakt im Portal registriert außer Url codiert werden müssen. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen.  OpenID verbunden sind es den Bereich `openid`, die Berechtigung "Anmelden" in der Zustimmung Benutzeroberfläche entspricht.  Optional kann auch sollen die `email` oder `profile` [Bereiche](active-directory-v2-scopes.md) für den Zugriff auf zusätzliche Daten.  Sie können auch andere Bereiche in dieser Anforderung für die Einholung der Zustimmung, die verschiedenen Ressourcen enthalten.  |
| response_mode | empfohlen | Gibt die Methode das resultierende token zurück an Ihre app verwendet werden soll.  Sollte `fragment` für den impliziten Flow.  |
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen [siteübergreifende Anforderungsfälschungsangriffe](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen des Benutzers in die app codieren, vor der die Authentifizierungsanfrage oder die Seite auf Ansicht, verwendet. |
| Nonce | Erforderlich | Ein Wert in der Anforderung der Anwendung, die Bestandteil der resultierenden ID-Token als Anspruch generiert.  Die Anwendung kann dann diesen Wert, um token Replay-Angriffe überprüfen.  Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die die Herkunft der Anforderung verwendet werden kann.  |
| Aufforderung | Optionale | Gibt den Typ von Benutzerinteraktion erforderlich ist.  Zu diesem Zeitpunkt die einzigen gültigen Werte sind 'Anmeldung', 'none' und ''.  `prompt=login`Zwingt den Benutzer zur Eingabe ihrer Anmeldeinformationen beantragt, negieren Single Sign-On.  `prompt=none`ist das Gegenteil - stellt sicher, dass der Benutzer jede interaktive Aufforderung überhaupt nicht angezeigt werden.  Wenn die Anforderung über Single Sign-On im Hintergrund ausgeführt werden kann, wird der v2. 0-Endpunkt einen Fehler zurück.  `prompt=consent`Löst Zustimmungsdialogfeld OAuth nach Benutzer, den Benutzer auffordert meldet, der Anwendung Berechtigungen. |
| login_hint | Optionale | Dienen zum Feld Benutzername/e-Mail-Adresse der Seite für den Benutzer vorab füllen sollten Sie ihren Benutzernamen voraus.  Häufig apps mithilfe dieses Parameters während der erneuten Authentifizierung müssen der Benutzername bereits aus einem früheren Anmeldung mit extrahiert die `preferred_username` anfordern. |
| domain_hint | Optionale | Kann eine der `consumers` oder `organizations`.  Wenn enthalten, überspringen Sie den e-Mail-basierte Erkennungsprozess Benutzer durchläuft auf V2. 0 anmelden, führt zu einer etwas optimierte Benutzeroberfläche.  Häufig apps verwendet dieser Parameter während der erneuten Authentifizierung durch Extrahieren der `tid` aus dem ID-Token anfordern.  Wenn die `tid` Wert ist `9188040d-6c67-4c5b-b112-36a304b66dad`, verwenden Sie `domain_hint=consumers`.  Andernfalls `domain_hint=organizations`. |

Zu diesem Zeitpunkt wird der Benutzer seine Anmeldeinformationen eingeben und die Authentifizierung aufgefordert.  V2. 0-Endpunkt wird auch sichergestellt, dass der Benutzer im angegebenen Berechtigungen zugestimmt hat die `scope` Parameter Abfragen.  Wenn der Benutzer alle Berechtigungen nicht zugestimmt hat, werden den Benutzer die erforderlichen Berechtigungen Zustimmung gefragt.  [Berechtigungen, Zustimmung und Multi-Tenant-apps dienen hier](active-directory-v2-scopes.md)Details.

Nachdem der Benutzer authentifiziert und Zustimmung erteilt, zurückgeben v2. 0-Endpunkt auf Ihre Anwendung auf die `redirect_uri`, mit der Methode der `response_mode` Parameter.

#### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche Antwort mit `response_mode=fragment` und `response_type=id_token+token` folgende Zeilenumbrüche für Leserlichkeit aussieht:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Zugriffstoken | Hinzugefügt, wenn `response_type` umfasst `token`. Das Zugriffstoken, das die Anwendung, in diesem Fall Microsoft Graph angefordert.  Das Zugriffstoken nicht decodiert oder andernfalls überprüft, als undurchsichtige Zeichenfolge behandelt werden. |
| token_type | Hinzugefügt, wenn `response_type` umfasst `token`.  Immer `Bearer`. |
| expires_in | Hinzugefügt, wenn `response_type` umfasst `token`.  Gibt die Anzahl der Sekunden, die das Token Zwischenspeichern gültig ist. |
| Gültigkeitsbereich | Hinzugefügt, wenn `response_type` umfasst `token`.  Zeigt die Bereiche, für die das Zugriffstoken gültig sein. |
| ID-Token | Das ID-Token, das die Anwendung angefordert. Das ID-Token können Sie die Identität des Benutzers überprüfen und beginnen mit dem Benutzer.  Weitere Informationen zu ID-Token und deren Inhalt ist im [token Endpunktverweis v2. 0](active-directory-v2-tokens.md)enthalten.  |
| Zustand | Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |


#### <a name="error-response"></a>Fehlermeldung
Fehlerantworten auch zugesandt werden die `redirect_uri` , die Anwendung sie angemessen behandelt werden kann:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |

## <a name="validate-the-idtoken"></a>Überprüfen Sie das ID-Token
Empfang ein ID-Token ist nicht ausreichend, um den Benutzer zu authentifizieren. die ID-Token Signatur überprüfen müssen und Überprüfen der Ansprüche im Token app Anforderungen.  V2. 0-Endpunkt verwendet [JSON Web Token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und öffentlichen Schlüsseln Token signieren und überprüfen, ob sie gültig sind.

Sie können überprüfen die `id_token` Client Code jedoch üblich ist, senden die `id_token` mit einem Back-End-Server und führen Sie die Überprüfung.  Nachdem Sie die Signatur der ID-Token überprüft haben, sind einige Ansprüche müssen Sie werden überprüfen.  [V2. 0 Tokenverweis](active-directory-v2-tokens.md) Informationen einschließlich [Token überprüfen](active-directory-v2-tokens.md#validating-tokens) und [Wichtige Informationen zum Signieren von Schlüssel Rollover](active-directory-v2-tokens.md#validating-tokens)anzeigen  Nutzung einer Bibliothek für Analyse und Überprüfung Token - gibt mindestens für die meisten Sprachen und Plattformen empfohlen.
<!--TODO: Improve the information on this-->

Sie können auch zusätzliche je nach Szenario zu überprüfen.  Einige allgemeine Prüfung gehören:

- Sicherstellen der Benutzerorganisation wurde für die Anwendung registriert.
- Sicherstellen des Benutzers verfügt ausreichende Autorisierung-Privilegien
- Sicherstellung einer bestimmten Stärke der Authentifizierung, wie mehrstufige Authentifizierung aufgetreten.

Weitere Informationen zu den Ansprüchen ein ID-Token finden Sie unter [token Endpunktverweis v2. 0](active-directory-v2-tokens.md).

Nach der ID-Token vollständig überprüft haben, können Sie beginnen mit dem Benutzer und Ansprüche in der ID-Token zu Informationen über die Benutzer in Ihrer Anwendung verwenden.  Diese Informationen kann verwendet werden, anzeigen, Datensätze, Berechtigungen.

## <a name="get-access-tokens"></a>Zugriffstoken abrufen

Damit Ihre app Seite Benutzer angemeldet haben, erhalten Sie Zugriffstoken für aufrufende Web APIs von Azure AD wie [Microsoft Graph](https://graph.microsoft.io)gesichert.  Auch wenn Sie bereits ein token mit dem `token` Antworttyp, können diese Methode zu Token zu zusätzlichen Ressourcen, ohne den Benutzer wieder anmelden.

Im normalen Fluss OpenID verbinden/OAuth führen Sie dies durch eine Anforderung der v2. 0 `/token` Endpunkt.  Jedoch unterstützt v2. 0-Endpunkt CORS Anfragen keine AJAX-Aufrufe zum Abrufen und Aktualisieren von Token ausgeschlossen ist.  Stattdessen können impliziten Fluss in versteckten Iframe neue Token für andere Web APIs abrufen: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Versuchen Sie es kopieren und Einfügen der Anfrage in einer Registerkarte! (Vergessen Sie nicht, ersetzen Sie die `domain_hint` und `login_hint` Werte mit den richtigen Werten für den Benutzer)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind `common`, `organizations`, `consumers`, und Mieter Bezeichner.  Weitere Informationen finden Sie unter [Protokoll Grundlagen](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| Antworttyp | Erforderlich | Muss `id_token` für Verbindung OpenID anmelden.  Es kann auch andere Response_types wie `code`. |
| redirect_uri | empfohlen | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen.  Sie müssen eines der Redirect_uris exakt im Portal registriert außer Url codiert werden müssen. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen.  Beim Abrufen des Tokens enthalten Sie alle [Bereiche](active-directory-v2-scopes.md) für die Ressource an benötigen.  |
| response_mode | empfohlen | Gibt die Methode das resultierende token zurück an Ihre app verwendet werden soll.  Kann eine der `query`, `form_post`, oder `fragment`.  |
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen siteübergreifende Anforderungsfälschungsangriffe verwendet.  Der Status wird auch Informationen des Benutzers in die app codieren, vor der die Authentifizierungsanfrage oder die Seite auf Ansicht, verwendet. |
| Nonce | Erforderlich | Ein Wert in der Anforderung der Anwendung, die Bestandteil der resultierenden ID-Token als Anspruch generiert.  Die Anwendung kann dann diesen Wert, um token Replay-Angriffe überprüfen.  Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die die Herkunft der Anforderung verwendet werden kann.  |
| Aufforderung | Erforderlich | Aktualisieren und Abrufen von Token in versteckten Iframe verwenden Sie `prompt=none` , dass Iframe hängt nicht auf V2. 0-Anmeldeseite und kehrt sofort zurück. |
| login_hint | Erforderlich | Aktualisieren & erste Token in einer versteckten Iframe, müssen Sie den Benutzernamen des Benutzers in diesem Hinweis einschließen, um mehrere Sitzungen unterscheiden haben der Benutzer zu einem bestimmten Zeitpunkt. Extrahieren Sie Benutzernamen aus einer früheren Anmeldung mit den `preferred_username` anfordern. |
| domain_hint | Erforderlich | Kann eine der `consumers` oder `organizations`.  Aktualisieren & erste Token in einer versteckten Iframe, müssen Sie die Domain_hint in der Anforderung enthalten.  Extrahieren Sie die `tid` Anfordern von ID-Token eines vorherigen Zeichen um den zu verwendenden Wert zu ermitteln.  Wenn die `tid` Wert ist `9188040d-6c67-4c5b-b112-36a304b66dad`, verwenden Sie `domain_hint=consumers`.  Andernfalls `domain_hint=organizations`. |

Dank der `prompt=none` Parameter dieser Anforderung entweder erfolgreich oder Fehler sofort und an die Anwendung zurück.  Eine erfolgreiche Antwort erhalten Ihre Anwendung auf die `redirect_uri`, mit der Methode der `response_mode` Parameter.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=fragment` aussieht:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Zugriffstoken | Das Token, das die Anwendung angefordert. |
| token_type | Immer `Bearer`. |
| Zustand | Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |
| expires_in | Wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Gültigkeitsbereich | Die Bereiche, denen das Zugriffstoken für gültig ist. |

#### <a name="error-response"></a>Fehlermeldung
Fehlerantworten auch zugesandt werden die `redirect_uri` , die Anwendung sie angemessen behandelt werden kann.  Im Fall von `prompt=none`, Fehler Erwarteter werden:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |

Wenn Sie diese Fehlermeldung in der Iframe-Anforderung muss der Benutzer interaktiv anmelden ein neues Token abrufen.  Sie können hier wie für Ihre Anwendung sinnvoll behandeln.

## <a name="refreshing-tokens"></a>Aktualisieren von Token

Beide `id_token`s und `access_token`s laufen nach kurzer Zeit, also Ihre app muss diese aktualisieren regelmäßig Token.  Um beide Typen von Token aktualisieren können derselben versteckten Iframe Anforderung von oben mit dem `prompt=none` Parameter Azure AD Verhalten steuern.  Möchten Sie einen neuen `id_token`, verwenden `response_type=id_token` und `scope=openid`, als `nonce` Parameter.


## <a name="send-a-sign-out-request"></a>Zeichen Sie ein-Anforderung

Die OpenIdConnect `end_session_endpoint` wird derzeit nicht vom Endpunkt v2. 0 unterstützt. Dies bedeutet, dass Ihre app v2. 0-Endpunkt des Benutzers beenden und deaktivieren Sie Cookies von v2. 0-Endpunkt eine Anforderung senden kann.
Benutzer, Anmeldung Ihre app einfach eigene beenden mit dem Benutzer, und lassen Sie die Sitzung des Benutzers mit der v2. 0-Endpunkt Takt.  Der Benutzer versucht, sich das nächste Mal sehen eine Seite "Wählen Sie Konto" mit ihren Konten aktiv angemeldet aufgeführt sie.
Auf dieser Seite können Benutzer ein Konto Beenden der Sitzung mit der Version 2.0 abmelden.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->