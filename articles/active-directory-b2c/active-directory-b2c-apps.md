<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Erstellen in Azure Active Directory B2C Anwendungstypen."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Anwendungstypen

Azure Active Directory (Azure AD) B2C unterstützt die Authentifizierung für eine Vielzahl der modernen app-Architekturen. Alle basieren auf dem branchenüblichen Protokollen [OAuth 2.0](active-directory-b2c-reference-protocols.md) oder [OpenID verbinden](active-directory-b2c-reference-protocols.md). Beschreibt dieses Dokument kurz die Anwendungstypen, die Sie erstellen können, unabhängig von der Sprache oder Plattform Sie bevorzugen. Darüber hinaus unterstützt die allgemeinen Szenarien vor [start erstellen](active-directory-b2c-overview.md#getting-started)zu verstehen.

## <a name="the-basics"></a>Die Grundlagen
Jede Anwendung, die mithilfe von Azure AD B2C muss im [B2C-Verzeichnis](active-directory-b2c-get-started.md) über das [Azure-Portal](https://portal.azure.com/)registriert werden. App-Registrierung erfasst und weist einige Werte für Ihre Anwendung:

- **ID der Anwendung** , die Ihre Anwendung eindeutig.
- Ein **URI umleiten** , die Antworten auf Ihre Anwendung direkt verwendet werden können.
- Andere szenariospezifische Werte. Ausführliche Informationen zum [Registrieren einer Anwendung](active-directory-b2c-app-registration.md).

Nach die Anwendung registriert ist, kommuniziert sie mit Azure AD Senden von Anfragen an den Endpunkt Azure AD v2. 0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Jede Anforderung an Azure AD B2C gibt eine **Richtlinie**. Eine Richtlinie steuert das Verhalten von Azure AD. Diese Endpunkte können Sie anpassbare Benutzeroberflächen erstellen. Allgemeine Richtlinien umfassen Anmeldung, anmelden und Richtlinien Profil bearbeiten. Wenn Sie nicht mit vertraut sind, sollten Sie über Azure AD B2C [extensible Richtlinienframework](active-directory-b2c-reference-policies.md) lesen, bevor Sie fortfahren.

Die Interaktion von jeder Anwendung einen Endpunkt v2. 0 folgt einem ähnlichen allgemeinen Muster:

1. Die Anwendung weist Benutzer v2. 0-Endpunkt, eine [Richtlinie](active-directory-b2c-reference-policies.md)auszuführen.
2. Der Benutzer schließt die Richtlinie gemäß der Richtliniendefinition.
4. Die Anwendung erhält eine Art von Sicherheitstoken vom Endpunkt v2. 0.
5. Die Anwendung verwendet das Sicherheitstoken, geschützte Informationen oder eine geschützte Ressource zugreifen.
6. Ressourcenserver überprüft das Sicherheitstoken zu überprüfen, ob Zugriff gewährt werden kann.
7. Die Anwendung aktualisiert regelmäßig das Sicherheitstoken.

<!-- TODO: Need a page for libraries to link to -->
Diese Schritte sind leicht basierend auf dem Typ der Anwendung, die Sie erstellen. Open-Source-Bibliotheken können Details Adresse.

## <a name="web-apps"></a>Webapps
Web apps (einschließlich .NET, PHP, Java, Ruby, Python und Node.js), die auf einem Server gehostet und Zugriff über einen Browser unterstützt Azure AD B2C [OpenID verbinden](active-directory-b2c-reference-protocols.md) für alle Benutzererlebnis. Dazu gehören anmelden, Anmeldung und Verwaltung von Profilen. In der Implementierung Azure AD B2C OpenID Connect initiiert Ihrer Anwendung diese Benutzeroberflächen durch Authentifizierungsanfragen an Azure AD. Das Ergebnis der Anforderung ist ein `id_token`. Dieses Sicherheitstoken stellt die Identität des Benutzers. Es bietet auch Informationen über den Benutzer in Form von Ansprüchen:

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

Erfahren Sie mehr über die Typen von Token und Ansprüche auf eine app [B2C-Token zuzugreifen](active-directory-b2c-reference-tokens.md).

In einer Web-app hat jede Ausführung einer [Richtlinie](active-directory-b2c-reference-policies.md) diesen allgemeinen Schritten:

![Web App Verantwortlichkeitsbereiche Bild](./media/active-directory-b2c-apps/webapp.png)

Überprüfung der `id_token` mit ein öffentlicher Signaturschlüssel, die von Azure AD erhalten reicht zum Überprüfen der Identität des Benutzers. Dadurch wird außerdem ein Sitzungscookie mit nachfolgenden Seite fordert den Benutzer identifizieren.

Um dieses Szenario in Aktion zu sehen, führen Sie eine Web app-in Codebeispielen in unseren [Abschnitt Schritte](active-directory-b2c-overview.md#getting-started).

Erleichtert einfach anmelden müssen eine Web-app auch einen Backend-Webdienst zuzugreifen. In diesem Fall kann geringfügig [Flow verbinden OpenID](active-directory-b2c-reference-oidc.md) Web app und Token mit Autorisierungscodes erwerben und Ausführen Token zu aktualisieren. Dieses Szenario wird im folgenden [Abschnitt Web-APIs](#web-apis)dargestellt.

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Web-APIs
Azure AD B2C können Webdienste wie Ihre app RESTful API. Web-APIs können OAuth 2.0 ihre Daten sichern, indem Sie eingehende HTTP-Anfragen mit Token authentifizieren. Der Aufrufer einer Web-API Fügt ein Token in der Authorization-Header einer HTTP-Anforderung:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web-API können Sie das Token dem API-Aufrufer Identität und Informationen zum Anrufer aus extrahieren, die im Token codiert sind. Erfahren Sie mehr über die Typen von Token und Ansprüche auf einer Anwendung in [Azure AD B2C-Token zuzugreifen](active-directory-b2c-reference-tokens.md).

> [AZURE.NOTE]
    Azure AD B2C unterstützt derzeit nur Web APIs eigene bekannte Clients zugreifen. Beispielsweise gehören die vollständige Anwendung iOS-app, Android app und Back-End-Web-API. Diese Architektur wird vollständig unterstützt. Ermöglicht einen Client Partner wie einem anderen iOS-app auf dasselbe Web API derzeit nicht unterstützt wird. Alle Komponenten der vollständigen app muss eine einzelne Anwendung ID freigeben.

Web-API erhalten Token aus vielen Arten von Clients, einschließlich webapps, Desktop und mobilen apps Seite apps, serverseitige Daemons und anderen Web-APIs. Hier ist ein Beispiel für den gesamten Ablauf für eine Webanwendung, die eine Web-API aufruft:

![Web App Web API Verantwortlichkeitsbereiche Bild](./media/active-directory-b2c-apps/webapi.png)

Lesen Sie weitere Autorisierungscodes Aktualisierungstoken und die Schritte Token [OAuth 2.0-Protokoll](active-directory-b2c-reference-oauth-code.md).

Informationen zum Sichern einer Webs-API mit Azure AD B2C checken Sie Web API-Lernprogramme in unsere [Erste Schritte-Abschnitt aus](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile und systemeigenen apps
Auf Geräten wie mobile und desktop-apps installierten Apps müssen häufig Zugriff auf Back-End-Services oder Web-APIs für Benutzer. Mithilfe von Azure AD B2C und [OAuth 2.0 Authorization Code Flow](active-directory-b2c-reference-oauth-code.md)können Sie benutzerdefinierte Identität Erfahrungen zu systemeigenen apps und sichere Call Back-End-Dienste hinzufügen.  

Dieser Fluss der Anwendung führt [Richtlinien](active-directory-b2c-reference-policies.md) und empfängt ein `authorization_code` von Azure AD der Benutzer die Richtlinie abgeschlossen hat. Die `authorization_code` stellt die app-Berechtigung auf den Back-End für den Benutzer, die angemeldet ist. Die Anwendung kann dann exchange die `authorization_code` im Hintergrund für ein `id_token` und `refresh_token`.  Die Anwendung kann die `id_token` Authentifizieren einer Back-End-Web-API in HTTP-Anfragen. Können Sie auch die `refresh_token` zu einer neuen `id_token` Ablauf eine ältere.

> [AZURE.NOTE]
    Azure AD B2C unterstützt derzeit nur Token, die auf einen der app Backend-Web Service verwendet werden. Beispielsweise gehören die vollständige Anwendung iOS-app, Android app und Back-End-Web-API. Diese Architektur wird vollständig unterstützt. IOS-app auf Partner Web API mit Zugriffstoken OAuth 2.0 ermöglicht wird derzeit nicht unterstützt. Alle Komponenten der vollständigen app muss eine einzelne Anwendung ID freigeben.

![Systemeigene Anwendung Verantwortlichkeitsbereiche Bild](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuelle Grenzen
Azure AD B2C unterstützt derzeit nicht die folgenden Anwendungstypen, sind sie auf die roadmapy. Zusätzliche Einschränkungen und Azure AD B2C Einschränkungen werden in [Einschränkungen](active-directory-b2c-limitations.md)beschrieben.

### <a name="single-page-apps-javascript"></a>Einzelne Seite apps (JavaScript)
Viele moderne apps haben eine einseitige app-Frontend hauptsächlich in JavaScript geschrieben. Sie verwenden häufig ein Framework wie AngularJS, Ember.js oder Durandal. Allgemein verfügbare Azure Active Directory unterstützt diese apps mithilfe des impliziten OAuth 2.0-Fluss. Allerdings ist dieser Fluss noch nicht in Azure AD B2C verfügbar.

### <a name="daemonsserver-side-apps"></a>Daemons, Server-Side apps
Apps, lang andauernde Prozesse bzw. bearbeitet werden ohne Benutzer, benötigen ebenfalls eine Möglichkeit auf sichere Ressourcen wie Web-APIs. Diese apps können authentifizieren und Token mithilfe des app Identität (anstelle der delegierten Benutzeridentität) und OAuth 2.0 Client Anmeldeinformationen Fluss.

Dieser Fluss ist von Azure AD B2C derzeit nicht unterstützt. Diese apps erhalten Token erst ein interaktiven Ablauf aufgetreten ist.

### <a name="web-api-chains-on-behalf-of-flow"></a>Web-API Ketten (im Auftrag von Fluss)
Viele Architekturen enthalten eine Web-API, die einem anderen downstream Web-API aufrufen, in denen sowohl von Azure AD B2C gesichert. Dieses Szenario wird häufig in systemeigenen Clients mit einem Back-End-Web-API. Dies ruft ein Microsoft online Service wie Azure AD Graph-API.

Dieses Szenario verketteten Web API kann mit OAuth 2.0 JWT Träger Anmeldeinformationen gewähren, auch die im Auftrag von Flow unterstützt werden.  Allerdings wird der Datenfluss im Auftrag von nicht in Azure AD B2C implementiert.
