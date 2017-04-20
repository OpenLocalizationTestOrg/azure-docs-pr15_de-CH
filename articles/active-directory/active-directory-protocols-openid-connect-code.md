<properties
    pageTitle="Übersicht über Azure AD .NET Protokoll | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Web-Applikationen und Web-APIs in Ihrem Mandanten mit Azure Active Directory OpenID verbinden."
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


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Autorisieren des Zugriffs auf ASP.NET-Webanwendungen mit OpenID verbinden Azure Active Directory

[OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) stellt eine einfache Identität auf OAuth 2.0-Protokoll erstellt. OAuth 2.0 Mechanismen Verwendung von **Zugriffstoken** Zugriff auf geschützte Ressourcen definiert, aber sie Standardmethoden Identität Informationen nicht definiert. OpenID verbinden implementiert Authentifizierung als Erweiterung des Autorisierungsvorgangs OAuth 2.0 Informationen der Benutzer aus einer `id_token` , die die Identität des Benutzers überprüft sowie grundlegende Informationen über den Benutzer.

OpenID Connect ist unsere Empfehlung, wenn Sie eine Anwendung erstellen, die auf einem Server gehostet und Zugriff über einen Browser.

## <a name="authentication-flow-using-openid-connect"></a>Authentifizierungsablauf mit OpenID verbinden

Der grundlegende Fluss-in umfasst die folgenden Schritte – jeweils im folgenden ausführlich beschrieben.

![OpenId Authentifizierungsablauf verbinden](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Senden der Anforderung

Bei die Webanwendung den Benutzer zu authentifizieren muss, müssen sie Benutzer direkt die `/authorize` Endpunkt. Diese Anforderung ähnelt den ersten Teil der [OAuth 2.0 Authorization Code fließen](active-directory-protocols-oauth-code.md), bestehen einige wichtige Unterschiede:

- Die Anforderung muss den Bereich einschließen `openid` in der `scope` Parameter.
- Die `response_type` Parameter sind `id_token`.
- Die Anforderung muss enthalten die `nonce` Parameter.

Damit eine Beispiel für eine Anforderung wie folgt aussieht:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Die `{tenant}` Wert im Pfad der Anforderung lässt sich steuern, wer die Anwendung anmelden kann.  Zulässige Werte sind Mieter Bezeichner, z. B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` oder `common` für unabhängige Mieter Token |
| client_id | Erforderlich | Ihre Anwendung beim Registrieren in Azure AD zugewiesene Anwendung-ID. Sie finden diese in Azure-Verwaltungsportal. Klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung und klicken Sie dann auf **Konfigurieren** |
| Antworttyp | Erforderlich | Muss `id_token` für Verbindung OpenID anmelden.  Es kann auch andere Response_types wie `code`. |
| Gültigkeitsbereich | Erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen.  OpenID verbunden sind es den Bereich `openid`, die Berechtigung "Anmelden" in der Zustimmung Benutzeroberfläche entspricht.  Sie können auch andere Bereiche in dieser Anforderung für die Einholung der Zustimmung enthalten. |
| Nonce | Erforderlich | Ein Wert in der Anforderung durch die app in der berücksichtigten generiert `id_token` als Anspruch.  Die Anwendung kann dann diesen Wert, um token Replay-Angriffe überprüfen.  Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge oder GUID verwendet werden kann, um den Ursprung der Anforderung zu identifizieren.  |
| redirect_uri | empfohlen | Die Redirect_uri der Anwendung, wo Authentifizierungsantworten gesendet und von der Anwendung empfangen.  Sie müssen eines der Redirect_uris exakt im Portal registriert außer Url codiert werden müssen. |
| response_mode | empfohlen | Gibt die Methode an die resultierenden Authorization_code für Ihre Anwendung verwendet werden soll.  Unterstützte Werte sind `form_post` für *http-Formular senden* oder `fragment` für *URL-Fragment*.  Für ASP.NET-Webanwendungen empfehlen wir `response_mode=form_post` um die sicherste Übertragung von Token der Anwendung sicherzustellen.  
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen [siteübergreifende Anforderungsfälschungsangriffe](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen des Benutzers in die app codieren, vor der die Authentifizierungsanfrage oder die Seite auf Ansicht, verwendet. |
| Aufforderung | Optionale | Gibt den Typ von Benutzerinteraktion erforderlich ist.  Zu diesem Zeitpunkt die einzigen gültigen Werte sind 'Anmeldung', 'none' und ''.  `prompt=login`Zwingt den Benutzer zur Eingabe ihrer Anmeldeinformationen beantragt, negieren Single Sign-On.  `prompt=none`ist das Gegenteil - stellt sicher, dass der Benutzer jede interaktive Aufforderung überhaupt nicht angezeigt werden.  Wenn die Anforderung über Single Sign-On im Hintergrund ausgeführt werden kann, wird der Endpunkt einen Fehler zurück.  `prompt=consent`Löst Zustimmungsdialogfeld OAuth nach Benutzer, den Benutzer auffordert meldet, der Anwendung Berechtigungen. |
| login_hint | Optionale | Dienen zum Feld Benutzername/e-Mail-Adresse der Seite für den Benutzer vorab füllen sollten Sie ihren Benutzernamen voraus.  Häufig apps mithilfe dieses Parameters während der erneuten Authentifizierung müssen der Benutzername bereits aus einem früheren Anmeldung mit extrahiert die `preferred_username` anfordern. |


Zu diesem Zeitpunkt wird der Benutzer seine Anmeldeinformationen eingeben und die Authentifizierung aufgefordert.

### <a name="sample-response"></a>Beispiel für eine Antwort

Beispielantwort, könnte nachdem der Benutzer authentifiziert wurde, wie folgt aussehen:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| ID-Token | Die `id_token` , die die Anwendung angefordert. Sie können die `id_token` die Identität des Benutzers überprüfen und beginnen mit dem Benutzer.  |
| Zustand | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden. Nach dem Zufallsprinzip generierter eindeutiger Wert wird in der Regel gegen [siteübergreifende Anforderungsfälschungsangriffe](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen des Benutzers in die app codieren, vor der die Authentifizierungsanfrage oder die Seite auf Ansicht, verwendet. |

### <a name="error-response"></a>Fehlermeldung
Fehlerantworten auch zugesandt werden die `redirect_uri` , die Anwendung sie angemessen behandelt werden kann:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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

## <a name="validate-the-idtoken"></a>Überprüfen Sie das ID-Token

Empfang einer `id_token` reicht für die Benutzerauthentifizierung; muss die Signatur überprüfen und Überprüfen der Ansprüche in der `id_token` app Anforderungen. Azure AD-Endpunkt verwendet JSON Web Token (JWTs) und öffentlichen Schlüsseln Token signieren und überprüfen, ob sie gültig sind.

Sie können überprüfen die `id_token` Client Code jedoch üblich ist, senden die `id_token` mit einem Back-End-Server und führen Sie die Überprüfung. Nachdem Sie die Signatur überprüft haben die `id_token`, gibt es einige Ansprüche überprüfen müssen.

Sie können auch zusätzliche je nach Szenario zu überprüfen. Einige allgemeine Prüfung gehören:

- Sicherstellen der Benutzerorganisation wurde für die Anwendung registriert.
- Sicherstellen des Benutzers verfügt ausreichende Autorisierung-Privilegien
- Sicherstellung einer bestimmten Stärke der Authentifizierung, wie mehrstufige Authentifizierung aufgetreten.

Einmal vollständig überprüft die `id_token`, beginnen mit dem Benutzer und verwenden die Ansprüche in der `id_token` Informationen über die Benutzer in Ihrer Anwendung. Diese Informationen kann verwendet werden, anzeigen, Datensätze, Berechtigungen. Lesen Sie weitere Informationen über die Tokentypen und Ansprüche [unterstützt und Anspruchstypen](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Zeichen Sie ein-Anforderung

Der Benutzer von der app möchten, reicht es nicht aus Ihrer Anwendung Cookies oder anderweitig am Ende der Sitzung mit dem Benutzer deaktivieren.  Sie müssen Benutzer umleiten der `end_session_endpoint` für die Abmeldung.  Wenn Sie dies nicht tun, werden die Ihre app erneut authentifizieren ohne ihre Anmeldeinformationen erneut, da eine gültige Anmeldung Sitzung mit der Azure AD haben.

Sie können einfach den Benutzer umleiten der `end_session_endpoint` OpenID verbinden Metadatendokument aufgeführt:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | empfohlen | Der URL, die der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll.  Nicht enthalten, wird der Benutzer eine allgemeine Meldung angezeigt.  |

## <a name="token-acquisition"></a>Token Anschaffung

Viele webapps müssen jedoch nicht nur anmelden des Benutzers auch Zugriff auf einen Webdienst für diese Benutzer OAuth. Dieses Szenario für die Benutzerauthentifizierung gleichzeitig gleichzeitig kombiniert OpenID Herstellen einer `authorization_code` nutzbar zu `access_tokens` mit OAuth Authorization Code übertragen.

## <a name="get-access-tokens"></a>Zugriffstoken abrufen

Zugriffstoken zu erhalten, müssen Sie die Anforderung von oben zu ändern:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Berechtigungsbereiche in der Anforderung und Verwendung von `response_type=code+id_token`, `authorize` Endpunkt wird sichergestellt, dass der Benutzer die Berechtigungen gemäß zugestimmt hat die `scope` Parameter Abfragen und Rückgabecode Ihre app eine Autorisierung für ein Zugriffstoken exchange.

### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche Antwort mit `response_mode=form_post` aussieht:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| ID-Token | Die `id_token` , die die Anwendung angefordert. Sie können die `id_token` die Identität des Benutzers überprüfen und beginnen mit dem Benutzer. |
| Code | Die Authorization_code, die die Anwendung angefordert. Die app können den Autorisierungscode ein Zugriffstoken für die Ressource anfordern. Authorization_codes sind sehr kurzlebige, i. d. r. nach etwa 10 Minuten ablaufen. |
| Zustand | Wenn Parameters in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt. Die Anwendung überprüfen, dass der Zustand der Anforderung und Antwort identisch sind. |

### <a name="error-response"></a>Fehlermeldung

Fehlerantworten auch zugesandt werden die `redirect_uri` , die Anwendung sie angemessen behandelt werden kann:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache in einem Authentifizierungsfehler ermitteln kann.  |

Eine Beschreibung der möglichen Fehlercodes und ihre empfohlene Client finden Sie unter [Fehlercodes für Endpunkt Autorisierungsfehlern](#error-codes-for-authorization-endpoint-errors).

Sobald Sie eine Zulassung erhalten haben `code` und `id_token`, Sie können Benutzer anmelden und Zugriffstoken in ihrem Namen.  Anmeldung den Benutzer überprüfen Sie die `id_token` wie oben beschrieben. Zu Zugriffstoken können Sie die [OAuth Protokoll Dokumentation](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token)beschriebenen Schritte.
