<properties
    pageTitle="Azure AD 2.0 Bereiche, Berechtigungen und Zustimmung | Microsoft Azure"
    description="Eine Beschreibung der Autorisierung in den Azure AD v2. 0 Endpunkt Bereiche, Berechtigungen und Zustimmung."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Bereiche, Berechtigungen und Zustimmung in V2. 0-Endpunkt

Apps, die in Azure AD integriert folgen bestimmten Autorisierungsmodell, das können Sie festlegen, wie eine Anwendung ihre Daten zugreifen kann.  2.0 Implementierung dieses Modells Autorisierung wurde aktualisiert, ändern, wie eine Anwendung in Azure AD interagieren muss.  Dieses Thema behandelt die grundlegenden Konzepte dieser Autorisierungsmodell, einschließlich der Bereiche, Berechtigungen und Zustimmung.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Bereiche & Berechtigungen

Azure AD implementiert eine Methode ermöglicht einer Anwendung 3. webgestützte Zugriff auf Ressourcen im Auftrag eines Benutzers [OAuth 2.0](active-directory-v2-protocols.md) -Protokoll Autorisierung.  Alle Web gehosteten Ressourcen, die in Azure AD integriert haben einen Ressourcenbezeichner oder **App-ID-URI**.  Beispielsweise Microsoft Web gehosteten Ressourcen:

- Office 365 Unified Mail API:`https://outlook.office.com`
- Azure AD Graph-API:`https://graph.windows.net`
- Microsoft Graph:`https://graph.microsoft.com`

Dies gilt auch für 3rd Party Ressourcen, die in Azure AD integriert wurde.  Diese Ressourcen können auch einen Satz von Berechtigungen definieren, mit der die Funktionen der Ressource in kleinere Einheiten aufteilen.  Beispielsweise hat Microsoft Graph einige Berechtigungen definiert:

- Kalender des Benutzers lesen
- Kalender des Benutzers schreiben
- E-Mail als Benutzer
- [+ mehr](https://graph.microsoft.io)

Diese Berechtigungen definieren können die Ressource eine genauere Kontrolle über die Daten und wie der Außenwelt verfügbar gemacht wird.  Eine 3rd Party app kann dann diese Berechtigungen von einem Endbenutzer - anfordern und der Endbenutzer muss die Berechtigungen genehmigen, bevor die app handeln kann.  Durch Segmentierung der Ressource Funktionen in kleinere Berechtigungssätze, werden 3. Partei apps erstellt, nur die Berechtigungen anfordern, die sie benötigen, um ihre Aufgabe durchzuführen.  Sie können auch Endbenutzer wissen genau wie eine Anwendung die Daten verwendet, sind sie sicher, dass die Anwendung nicht mit böswilliger Absicht verhält.

In Azure AD OAuth, diese Berechtigungen werden als **Bereiche**bezeichnet.  Sie können auch als **oAuth2Permissions**angezeigt.  Ein Bereich wird in Azure AD als Zeichenfolgenwert dargestellt.  Die Microsoft Graph Beispiel ist der Bereichswert für jede Berechtigung:

- Kalender des Benutzers zu lesen:`Calendar.Read`
- Kalender des Benutzers schreiben:`Mail.ReadWrite`
- Senden Sie als Benutzer:`Mail.Send`

Eine Anwendung kann diese Berechtigungen anfordern, indem Bereiche in Anfragen an den Endpunkt v2. 0 angeben wie unten beschrieben.

## <a name="openid-connect-scopes"></a>OpenId verbinden Bereiche

2.0 Implementierung von OpenID Connect hat einige definierten Bereiche nicht für eine bestimmte Ressource gelten - `openid`, `email`, `profile`, und `offline_access`.

#### <a name="openid"></a>OpenId

Führt eine Anwendung anmelden mit [OpenID herstellen](active-directory-v2-protocols.md#openid-connect-sign-in-flow), müssen anfordern der `openid` Bereich.  Die `openid` Bereich erscheint im Fenster Zustimmung Arbeit Konto die Berechtigung "Anmeldung" und in der persönlichen Konto zustimmungsbildschirm Microsoft die Berechtigung "Ihr Profil anzeigen und Verbinden mit apps und Dienste mit Ihrem Microsoft-Konto".  Diese Berechtigung ermöglicht eine Anwendung einen eindeutigen Bezeichner für den Benutzer in Form von empfangen die `sub` anfordern.  Es bietet auch Anwendung auf Benutzer-Info-Endpunkt.  Die `openid` Bereich auch dienen token v2. 0-Endpunkt zu ID-Token, die sichere HTTP-Aufrufe zwischen verschiedenen Komponenten einer Anwendung verwendet werden kann.

#### <a name="email"></a>E-Mail

Die `email` Bereich lassen sich mit den `openid` Bereich und andere.  Ermöglicht den Anwendung Zugriff auf e-Mail-Adresse des Benutzers aus der `email` anfordern.  Die `email` Forderung wird in Token nur enthalten, wenn eine e-Mail-Adresse das Benutzerkonto zugeordnet ist, ist nicht immer der Fall.  Verwenden der `email` Bereich Ihrer app bereit sein, bei der Behandlung der `email` Forderung in das Token nicht vorhanden.

#### <a name="profile"></a>Profil

Die `profile` Bereich lassen sich mit den `openid` Bereich und andere.  Es bietet den Anwendung Zugriff auf eine Fülle von Informationen über den Benutzer.  Dies umfasst, jedoch nicht beschränkt auf des Benutzers Vorname, Nachname, bevorzugter Benutzername, Objekt-ID usw..  Finden Sie eine vollständige Liste der für einen bestimmten Benutzer ID-Token für Profil-Anträge [Tokenverweis v2. 0](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

Die [ `offline_access` Bereich](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) Ihrer Anwendung Zugriff auf die Ressourcen für den Benutzer längere Zeit ermöglicht.  Arbeit Konto Zustimmung Bildschirm wird dieser Bereich der Berechtigung "Daten jederzeit Zugriff" angezeigt.  Im persönlichen Microsoft-Konto zustimmungsbildschirm erscheint es als die Berechtigung "Ihre Informationen jederzeit zugreifen".  Wenn ein Benutzer genehmigt die `offline_access` Bereich Ihrer app zu Aktualisierungstoken aus dem token v2. 0-Endpunkt aktiviert.  Aktualisieren Sie sind langlebig und können Ihre app zu neuen Zugriffstoken älteren ablaufen.

Wenn Ihre Anwendung nicht wünscht die `offline_access` Bereich, erhalten keine Aktualisierungstoken.  Dies bedeutet, dass beim einlösen Authorization_code [OAuth 2.0 Authorization Code Datenfluss](active-directory-v2-protocols.md#oauth2-authorization-code-flow)nur wieder ein Zugriffstoken aus Sie erhalten die `/token` Endpunkt.  Dieses Zugriffstoken kurzer (in der Regel eine Stunde) gültig bleiben, aber schließlich ablaufen.  Zeitpunkt Ihrer app muss Benutzer umleiten zu sichern die `/authorize` Endpunkt eine neue Authorization_code abrufen.  Während diese Umleitung der Benutzer oder nicht müssen ihre Anmeldeinformationen oder Berechtigungen erneut Einverständnis abhängig von den Anwendungstyp.

Weitere Informationen und aktualisieren Sie Token, finden Sie in [Version 2.0 Protokoll Verweis](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Zustimmung des einzelnen Benutzers anfordern

Eine Authentifizierungsanfrage [OpenID oder OAuth 2.0](active-directory-v2-protocols.md) kann eine Anwendung mit erforderlichen Berechtigungen anfordern der `scope` Parameter Abfragen.  Beispielsweise würde meldet ein Benutzer in einer Anwendung die Anwendung eine Anforderung wie folgt (Zeilenwechsel Lesbarkeit) senden:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Die `scope` Parameter ist eine durch Leerzeichen getrennte Liste von Bereichen, die die Anwendung anfordert.  Jeder einzelne Bereich wird durch Anhängen der Bereichswert auf Ressourcenbezeichner (App-ID URI).  Anforderung gibt an, dass die app den Kalender des Benutzers lesen und Senden als Benutzer.

Nachdem die Benutzer ihre Anmeldeinformationen überprüft v2. 0-Endpunkt einen übereinstimmenden Datensatz des **Benutzers zustimmen**.  Wenn der Benutzer keine angeforderten Berechtigungen in der Vergangenheit zugestimmt hat, fragt v2. 0-Endpunkt die Benutzer die erforderlichen Berechtigungen gewähren.  

![Arbeit Konto Zustimmung Screenshot](../media/active-directory-v2-flows/work_account_consent.png)

Wenn der Benutzer die Berechtigung genehmigt, werden damit der Benutzer nicht erneut auf nachfolgenden Anmeldungen Zustimmung Zustimmung aufgezeichnet.

## <a name="requesting-consent-for-an-entire-tenant"></a>Einholung der Zustimmung für eine gesamte Mandanten

Häufig erwirbt ein Unternehmen eine Lizenz oder ein Abonnement für eine Anwendung, möchten sie vollständig für ihre Mitarbeiter bereitstellen.  Als Teil dieses Prozesses kann ein Unternehmensadministrator Zustimmung für die Anwendung im Namen jeder Mitarbeiter gewähren.  Mit Einwilligung für eine gesamte Mandanten erleben Mitarbeiter der Organisation nicht den zustimmungsbildschirm für die Anwendung.

Um Zustimmung für alle Benutzer in einem Mandanten zu beantragen, können Ihre Anwendung die **Admin Zustimmung Endpunkt**beschrieben.

## <a name="admin-restricted-scopes"></a>Admin eingeschränkte Bereiche

Bestimmte hohe Berechtigungen Berechtigungen im Microsoft-Ökosystem können als **Administrator beschränkt**gekennzeichnet.  Beispiele für solche Bereiche:

- Ein Organizaion Verzeichnisdaten werden gelesen:`Directory.Read`
- Schreiben von Daten in einer Organisation Verzeichnis:`Directory.ReadWrite`
- Lesen Sie Sicherheitsgruppen im Verzeichnis der Organisation:`Groups.Read.All`

Während Verbraucher Benutzer eine Anwendungszugriff gewähren kann, dürfen Organisationseinheit Benutzer Zugriff auf vertrauliche Daten.  Wenn die Anwendung Zugriff auf eine der folgenden Berechtigungen von einem organisatorischen anfordert, erhält der Benutzer eine Fehlermeldung, dass unbefugte Ihre app Berechtigungen stimmen sind.

Wenn Ihre app diese Admin eingeschränkte Bereiche für Unternehmen erforderlich ist, sollten Sie sie direkt ein Unternehmensadministrator auch mithilfe der **Admin Zustimmung Endpunkt**beschrieben beantragen.

Administrator erteilt diese Berechtigungen über die Admin Zustimmung Endpunkt Zustimmung für alle Benutzer in der Mieter erhält wie oben beschrieben.

## <a name="using-the-admin-consent-endpoint"></a>Admin Zustimmung Endpunkt verwenden

Anhand dieser Schritte können Ihre app Berechtigungen für alle Benutzer in einem bestimmten Mandanten einschließlich Admin eingeschränkte Bereiche zu.  Finden Sie ein Codebeispiel finden, die die vorangehenden Schritte unten implementiert [Admin eingeschränkte Bereiche Beispiel](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Anfordern von Berechtigungen im Portal Registrierung app

- Navigieren Sie in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder [Erstellen Sie eine Anwendung](active-directory-v2-app-registration.md) für die Anwendung, falls noch nicht.
- Suchen Sie im **Microsoft Graph-Berechtigungen** und Berechtigungen Sie die, die Ihre Anwendung erfordert.
- Stellen Sie sicher **zu** app-Registrierung

#### <a name="recommended-sign-the-user-into-your-app"></a>Empfohlen an: Ihre Anwendung melden Sie den Benutzer

Normalerweise beim Erstellen einer Anwendung, die der Administrator bekommen Endpunkt, benötigen die Anwendung eine Seitenansicht, mit dem der Administrator die Anwendung Berechtigungen genehmigen können.  Diese Seite kann die app Registrierungsprozesses, Teil der Einstellungen für die Anwendung oder einen dedizierten Datenfluss "Verbinden".  In vielen Fällen ist es sinnvoll, die für die Anwendung zeigen "Ansicht verbinden" nur, wenn ein Benutzer mit einer Arbeit oder Schule Microsoft-Konto angemeldet hat.

Signieren des Benutzers in der Anwendung können Sie die Verbandes identifizieren, der Administrator gehört vor bitten, die erforderlichen Berechtigungen zu genehmigen.  Zwar nicht unbedingt erforderlich, hilft es mehr intuitiv für die Organisationseinheit Benutzer erstellen.  Anmeldung den Benutzer in folgen Sie unserer [2.0 Protokoll Lernprogramme](active-directory-v2-protocols.md)

#### <a name="request-the-permissions-from-a-directory-admin"></a>Verzeichnis-Administrator Berechtigungen anfordern

Um Berechtigungen anzufordern aus der Unternehmensadministrator nun, können Sie Benutzer v2. 0- **Endpunkt Zustimmung Admin**umleiten.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Erforderlich | Sie die Erlaubnis möchten Directory-Pächter.  Kann im Guid oder Anzeigename bereitgestellt werden. |
| client_id | Erforderlich | Die Anwendung Id registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre Anwendung zugewiesen. |
| redirect_uri | Erforderlich | Die Redirect_uri auf Ihre App Verarbeiten gesendet werden soll.  Es muss genau eines der Redirect_uris entsprechen, die im Portal registriert. |
| Zustand | empfohlen | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Der Status wird zum Codieren Informationen der Benutzer in der app vor der Authentifizierung angefordert wurde oder die Seite anzeigen, die auf. |

An diesem Punkt wird Azure AD erzwungen, dass ein Tenant-Administrator anmelden kann, um die Anforderung abzuschließen.  Der Administrator aufgefordert, alle Berechtigungen zu genehmigen, die Sie für Ihre Anwendung im registrierungsportal angefordert haben. 

##### <a name="successful-response"></a>Erfolgreiche Antwort
Wenn der Administrator die Berechtigungen für die Anwendung genehmigt, werden erfolgreiche Antwort:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mieter | Verzeichnis Mandanten, die Ihre Anwendung Berechtigungen gewünscht, im Guid-Format. |
| Zustand | Ein Wert in der Anforderung, die auch auf token zurückgegeben werden.  Eine Zeichenfolge von Inhalten möglich, die Sie wünschen.  Der Status wird zum Codieren Informationen der Benutzer in der app vor der Authentifizierung angefordert wurde oder die Seite anzeigen, die auf. |
| admin_consent | Wird `True`. |



##### <a name="error-response"></a>Fehlermeldung
Wenn der Administrator die Berechtigungen für die Anwendung nicht genehmigen, werden fehlerhafte Antwort:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden, um Fehler zu klassifizieren, die auftreten, und wird auf Fehler reagieren. |
| error_description | Eine Fehlermeldung, mit denen Entwickler die Ursache eines Fehlers ermitteln kann.  |

Nachdem Sie eine erfolgreiche Antwort vom Endpunkt Admin Zustimmung erhalten haben, sammelte Ihre app Berechtigungen angeforderte.  Sie können nun auf ein Token für die gewünschte Ressource anfordert, wie unten beschrieben.

## <a name="using-permissions"></a>Mithilfe von Berechtigungen

Nachdem Benutzer Berechtigungen für Ihre Anwendung dies erwerben Ihre app Zugriffstoken, die Ihre Anwendung Zugriff auf eine Ressource in eine darstellen.  Ein Zugriffstoken für den angegebenen kann ausschließlich für eine einzelne Ressource darin codiert werden jedoch jede Berechtigung, die Ihre Anwendung für diese Ressource erteilt wurde.  Erwerben Sie ein Zugriffstoken kann Ihre app token v2. 0-Endpunkt beantragen:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Das resultierende Zugriffstoken kann in HTTP-Anfragen an die Ressource verwendet werden - es zuverlässig zeigt der Ressource hat die Anwendung die entsprechende Berechtigung zum Ausführen einer bestimmten Aufgabe.  

Ausführlicher OAuth 2.0-Protokoll und zum Zugriffstoken erhalten finden Sie unter [Protokoll Endpunktverweis v2. 0](active-directory-v2-protocols.md).
