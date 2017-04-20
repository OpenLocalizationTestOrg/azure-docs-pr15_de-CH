<properties
    pageTitle="Typen der Endpunkte v2. 0 | Microsoft Azure"
    description="Die Typen von apps und Azure AD v2. 0-Endpunkt unterstützt."
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

# <a name="types-of-apps-for-the-v20-endpoint"></a>Anwendungstypen für den Endpunkt v2. 0
V2. 0-Endpunkt unterstützt die Authentifizierung für eine Vielzahl der modernen app Architekturen, die die Protokolle nach Industriestandard [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) bzw. [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow)auf.  Dokument die Anwendungstypen beschreibt erstellen können, unabhängig von der Sprache oder Plattform Sie bevorzugen.  Es hilft Ihnen hohen Szenarien vor [direkt in den Code](active-directory-appmodel-v2-overview.md#getting-started)zu verstehen.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Die Grundlagen
Jede Anwendung, die v2. 0-Endpunkt verwendet [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)registriert werden müssen.  App-Registrierung Abhol- und Ihre app einige Werte zuweisen:

- **Id der Anwendung** , die Ihre Anwendung eindeutig
- Ein **URI umleiten** , die verwendet werden können, Antworten auf Ihre app
- Einige andere Szenario Werte.  Ausführliche Informationen zum [Registrieren einer Anwendung](active-directory-v2-app-registration.md).

Nach der Registrierung kommuniziert Senden von Anfragen an Active Directory Azure v2. 0-Endpunkt die Anwendung in Azure AD.  Wir bieten open-Source-Frameworks und Bibliotheken, die die Details der Anfragen kümmern oder der Authentifizierungslogik selbst implementieren von Anfragen an diese Endpunkte erstellen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Webapps
Web Apps (.NET, PHP, Java, Ruby, Python, Knoten usw.), die über einen Webbrowser zugegriffen wird, können Benutzer mit [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  OpenID Connect Web app empfängt ein `id_token`, ein Sicherheitstoken, das die Identität des Benutzers überprüft und Informationen über den Benutzer in Form von Ansprüchen:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Sie können alle Token und Ansprüche auf eine app [Tokenverweis v2. 0](active-directory-v2-tokens.md)erfahren.

In Web-apps hat anmelden Authentifizierungsablauf diese Schritte:

![Web App Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Die Überprüfung des ID-Token mit einer öffentlichen Signaturschlüssel empfangen von v2. 0-Endpunkt reicht die Identität des Benutzers und legen ein Sitzungscookie mit nachfolgenden Seite fordert den Benutzer identifizieren.

Um dieses Szenario in Aktion sehen möchten, versuchen Sie eine Web app-in Codebeispielen im Bereich [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) .

Neben einfachen anmelden müssen eine Web-app auch einige andere Webdienst wie eine REST-API zugreifen.  In diesem Fall kann die Web-app in einem kombinierten OpenID Verbinden & OAuth 2.0, mit [OAuth 2.0 Autorisierungscode Fluss](active-directory-v2-protocols.md#oauth2-authorization-code-flow)teilnehmen Dieses Szenario wird im [Thema WebApp WebAPI Einstieg](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)nachfolgend erläutert.

## <a name="web-apis"></a>Web-APIs
V2. 0-Endpunkt können Sie, wie Ihre app RESTful Web API Webdienste.  ID-Token und Sitzungscookies anstelle von APIs OAuth 2.0 Access_tokens ihre Daten sichern und Anfragen zu authentifizieren.  Der Aufrufer eine Web-API Fügt ein Zugriffstoken Authorization-Header einer HTTP-Anforderung:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Die Web-API können das Zugriffstoken Identität dem API-Aufrufer und Extrahieren von Informationen zum Anrufer aus, die das Zugriffstoken codiert sind.  Sie können alle Token und Ansprüche auf eine app [Tokenverweis v2. 0](active-directory-v2-tokens.md)erfahren.

Eine Web-API kann zu opt-in/opt-Out bestimmte Funktions-oder Benutzern mithilfe Berechtigungen [Bereiche](active-directory-v2-scopes.md)genannt.  Der Benutzer muss für eine aufrufende Anwendung Berechtigung für einen Bereich zu in einen Bereich zustimmen.  V2. 0-Endpunkt kümmern Berechtigung Bestätigung und Berechtigungen in allen Access_tokens Web-API empfängt aufzeichnen.  Die Web-API kümmern muss überprüft bei jedem Aufruf erhält Access_tokens und die entsprechende Autorisierung überprüft.

Eine Web-API können alle Anwendungstypen, einschließlich Web Server apps, Desktop und mobilen apps Seite apps, Seite Dämonprozesse und sogar andere Web-APIs Access_tokens erhalten.  Hohe Fluss für Web api-Authentifizierung lautet wie folgt:

![Web-API Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Um weitere Informationen zu Authorization_codes, Aktualisierungstoken und Access_tokens immer ausführliche Informationen Sie zu [OAuth 2.0-Protokoll](active-directory-v2-protocols-oauth-code.md).

Informationen zum Sichern einer Webs-api mit OAuth2 Access_tokens Auschecken Web api-Codebeispiele im [Abschnitt Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile und systemeigenen apps
Auf einem Gerät wie mobile und desktop-apps installierten Apps müssen häufig Zugriff auf Back-End-Services oder Web-APIs, die verschiedene Funktionen für einen Benutzer speichern.  Diese apps können Anmeldung und Autorisierung [OAuth 2.0 Autorisierungscode Flow](active-directory-v2-protocols-oauth-code.md)mit Back-End-Dienste hinzufügen.  

Dieser Fluss einer app erhält eine Authorization_code von v2. 0-Endpunkt auf Benutzer anmelden, die app Berechtigung auf den Back-End für den derzeit angemeldeten Benutzer darstellt.  Die Anwendung kann die Authoriztion_code im Hintergrund ein Zugriffstoken OAuth 2.0 und ein Aktualisierungstoken austauschen.  Die app kann das Zugriffstoken Web-APIs in HTTP-Anfragen authentifiziert und kann die Aktualisierungstoken auf neue Access_tokens älteren ablaufen.

![Systemeigene Anwendung Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Einzelne Seite apps (Javascript)
Viele moderne apps müssen eine einzelne Seite App (SPA) Front-End-hauptsächlich in Javascript geschrieben und oft mit Frameworks wie AngularJS, Ember.js, Durandal.  Azure AD v2. 0-Endpunkt unterstützt diese apps mit [OAuth 2.0 implizite fließen](active-directory-v2-protocols-implicit.md).

Dieser Fluss die Anwendung empfängt Token v2. 0 autorisieren Endpunkt direkt, ohne alle Back-End-Server zu Server ausgetauscht.  Dies ermöglicht allen Authentifizierungslogik und Sitzung Behandlung zu vollständig in Javascript-Client ohne zusätzliche Seite umgeleitet.

![Implizite Fluss Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Um dieses Szenario in Aktion zu sehen, versuchen Sie eine Seite app-Codebeispiele im Bereich [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Dämonen-Server Seite apps
Apps mit langer Vorgänge oder ohne Benutzer bearbeitet werden benötigen auch gesicherte Ressourcen wie Web-APIs zugreifen.  Diese apps können authentifizieren und Token mithilfe des app Identität (anstelle der delegierten Benutzeridentität) OAuth 2.0-Client Anmeldeinformationen Fluss.

In diesem Datenstrom erhält die app Token durch direkte Interaktion mit den `/token` Endpunkt:

![Daemon App Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Zum Erstellen einer app Daemon finden Sie unter Client Anmeldeinformationen Documeenation im Bereich [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) oder [.NET Beispiel](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)App.

## <a name="current-limitations"></a>Aktuelle Grenzen
Diese Anwendungstypen werden derzeit nicht von v2. 0-Endpunkt unterstützt, jedoch sind.  Zusätzliche Einschränkungen und die Einschränkung für den Endpunkt v2. 0 werden in [v2. 0 Grenzen Artikel](active-directory-v2-limitations.md)beschrieben.

### <a name="chained-web-apis-on-behalf-of"></a>Verkettete Web-APIs (im Auftrag von)
Viele Architekturen enthalten eine Web-API, die anderen downstream Web API sowohl gesicherte v2. 0-Endpunkt aufrufen.  Dieses Szenario wird häufig in systemeigenen Clients mit Backend Web-API, die wiederum einen Microsoft Online Service Office 365 oder Graph-API aufruft.

Diesem Web API verketteten kann mit OAuth 2.0 Jwt Träger Anmeldeinformationen gewähren, bekannt als [Flow On-Behalf-Of](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)unterstützt werden.  Jedoch ist auf Namen der Fluss nicht in V2. 0-Endpunkt implementiert.  Um diesem Datenstrom erhältlich Azure AD funktioniert service, [im Auftrag von Codebeispiel auf GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)prüfen.
