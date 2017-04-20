<properties
  pageTitle="Client und Server SDK Versionskontrolle in Mobile Apps und Mobile Dienste | Azure App Service"
  description="Liste der Client-SDKs und Kompatibilität mit Serverversionen für Mobile Services und Azure Mobile Apps SDK"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Client- und Versionskontrolle in Mobile Apps und Mobile Dienste

Die neueste Version von Azure Mobile Services ist das **Mobile Apps** -Feature von Azure App Service.

Die Mobile Apps Client- und SDKs basieren ursprünglich auf Mobile Dienste, aber sie sind miteinander *nicht* kompatibel.
Also müssen Sie *Apps Mobile* Client SDK *Mobile Apps* Server SDK und ebenso für *Mobile Dienste*verwenden. Dieser Vertrag wird erzwungen durch einen speziellen Wert von Client und Server SDKs, `ZUMO-API-VERSION`.

Hinweis: Wenn Backend *Mobile Dienste* dieses Dokument bezieht, wird es nicht unbedingt auf Mobile Dienste gehostet werden. Kann jetzt migrieren Mobilfunkanbieter App-Dienst ohne Code ausgeführt, aber der Dienst würde weiterhin *Mobile Dienste* SDK-Versionen verwenden.

Erfahren Sie mehr über das Migrieren zu App Service ohne Code finden Sie im Artikel [Migrieren Mobile Service Azure App Service].

## <a name="header-specification"></a>Header-Angabe

Der Schlüssel `ZUMO-API-VERSION` im HTTP-Header oder der Abfragezeichenfolge angegeben werden kann. Der Wert ist eine Zeichenfolge in der Form **x.y.z**.

Zum Beispiel:

Https://service.azurewebsites.net/tables/TodoItem abrufen

HEADER: ZUMO-API-VERSION: 2.0.0

Https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0 buchen

## <a name="opting-out-of-version-checking"></a>Deaktivieren von Version überprüfen

Sie können Version Einchecken Wert **true** für die app-Einstellung **MS_SkipVersionCheck**deaktivieren. Geben Sie diesen in web.config oder im Abschnitt Einstellungen der Azure-Portal.

> [AZURE.NOTE] Gibt es eine Anzahl Verhalten Unterschiede zwischen Mobile Dienste und Mobile Apps insbesondere offline synchronisieren, Authentifizierung und Pushbenachrichtigungen. Sie sollten nur deaktivieren Version nach umfassende Tests, um sicherzustellen, dass diese Verhalten ändert nicht Ihre app Funktionalität beeinträchtigen.

## <a name="summary-of-compatibility-for-all-versions"></a>Zusammenfassung der Kompatibilität für alle Versionen

Das folgende Diagramm zeigt die Kompatibilität zwischen Client und Server. Backend eingestuft als Mobile **Dienste** oder mobiler **Apps** Server SDK basiert, der verwendet wird.

|                           | **Mobile Dienste** Node.js oder .NET | **Mobiler Apps** Node.js oder .NET |
| ----------                | -----------------------             |   ----------------              |
| [Mobile Services-clients] | Okay                                  | Fehler\*                         |
| [Apps für Mobile clients]     | Fehler\*                             | Okay                              |

\*Dies kann durch Angabe von **MS_SkipVersionCheck**gesteuert werden.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobile Clients und server

Der Client-SDKs in der folgenden Tabelle sind **Mobile Dienste**mit.

Hinweis: der Mobile-Client SDKs *nicht* senden einen Headerwert für `ZUMO-API-VERSION`. Der Dienst erhält diese Kopf- oder Abfragezeichenfolgenwert, ein Fehler zurückgegeben, wenn Sie explizit wie oben beschrieben entschieden haben.

### <a name="MobileServicesClients"></a>Mobile *Services* Client SDKs

| Client-Plattform                   | Version                                                                   | Version-Headerwert |
| -------------------               | ------------------------                                                  | -------------------  |
| Verwaltete Client (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/a                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/a                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/a                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/a     |

### <a name="mobile-services-server-sdks"></a>Mobile *Services* Server SDKs

| Server-Plattform  | Version                                                                                                        | Akzeptierte Versionsheader |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* Version 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Keine Versionsheader** |
| Node.js          | (in Kürze verfügbar)                        | **Keine Versionsheader** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Verhalten von Mobile Dienste Downloadzeit

| ZUMO-API-VERSION | Wert des MS_SkipVersionCheck | Antwort |
| ---------------- | ---------------------------- | -------- |
| Nicht angegeben    | Alle                          | 200 - OK |
| Beliebiger Wert        | True                         | 200 - OK |
| Beliebiger Wert        | False oder nicht angegeben          | 400 - Ungültige Anforderung |

## <a name="2.0.0"></a>Azure Mobile Apps Client und server

### <a name="MobileAppsClients"></a>Mobiler *Apps* Client SDKs

Überprüfen der Version wurde beginnend mit den folgenden Versionen von Client-SDK für **Azure Mobile Apps**:

| Client-Plattform                   | Version                   | Version-Headerwert |
| -------------------               | ------------------------  | -----------------    |
| Verwaltete Client (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobiler *Apps* Server SDKs

Überprüfen der Version ist im folgenden Server SDK-Versionen enthalten:

| Server-Plattform  | SDK                                                                                                        | Akzeptierte Versionsheader |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure-Mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Verhalten von Mobile Apps Downloadzeit

| ZUMO-API-VERSION | Wert des MS_SkipVersionCheck | Antwort |
| ---------------- | ---------------------------- | -------- |
| x.y.z oder Null    | True                         | 200 - OK |
| NULL             | False oder nicht angegeben          | 400 - Ungültige Anforderung |
| 1.x.y            | False oder nicht angegeben          | 400 - Ungültige Anforderung |
| 2.0.0-2.x.y      | False oder nicht angegeben          | 200 - OK |
| 3.0.0-3.x.y      | False oder nicht angegeben          | 400 - Ungültige Anforderung |


## <a name="next-steps"></a>Nächste Schritte

- [Mobilen Service Azure App Service migrieren]


[Mobile Services-clients]: #MobileServicesClients
[Apps für Mobile clients]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Mobilen Service Azure App Service migrieren]: app-service-mobile-migrating-from-mobile-services.md

