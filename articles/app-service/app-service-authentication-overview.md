<properties
    pageTitle="Authentifizierung und Autorisierung in Azure App Service | Microsoft Azure"
    description="Grundlegende Referenz und Übersicht über die Authentifizierung / Autorisierung für Azure App Service bieten"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Authentifizierung und Autorisierung in Azure App Service

## <a name="what-is-app-service-authentication--authorization"></a>Was ist App-Authentifizierung / Autorisierung?

App-Authentifizierung Autorisierung ist eine Funktion, eine Möglichkeit für die Anwendung Benutzer anmelden, damit Sie nicht Code auf das app-Backend ändern. Es bietet eine einfache Möglichkeit zu Ihrer Anwendung mit Daten arbeiten.

App-Dienst verwendet Identitätsverbund Drittanbietern Identitätsanbieter speichert Konten und Benutzer authentifiziert. Die Anwendung hängt von Identitätsinformationen des Anbieters, damit die Anwendung diese Information selbst hat. App-Dienst unterstützt fünf Identitätsanbieter aus Feld: Azure Active Directory, Facebook, Google, Microsoft Account und Twitter. Ihre app können eine beliebige Anzahl dieser Identitätsanbieter den Benutzern Optionen für wie sie sich anmelden. Erweitern Sie die integrierte Unterstützung können integriert anderen Identitätsanbieter oder [Ihre eigene benutzerdefinierte identitätslösung][custom-auth].

Wenn Sie sofort beginnen möchten, finden Sie die folgenden Lernprogramme:

- [IOS-app Authentifizierung hinzufügen] [ iOS] (oder [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]oder [Cordova])
- [Benutzerauthentifizierung für API-Apps in Azure App Service][apia-user]
- [Erste Schritte mit Azure App Service - Teil2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Funktionsweise der Authentifizierung in App Service

Die Authentifizierung mithilfe der Identitätsanbieter müssen Sie Identitätsanbieter wissen über die Anwendung konfigurieren. Der Identitätsanbieter liefern-IDs und Kennwörter App Service bieten. Dies schließt die Vertrauensstellung, damit Benutzer Assertionen wie Authentifizierungstoken vom Identitätsanbieter App Service überprüft werden kann.

Um einen Benutzer mit einem Anbieter anmelden muss der Benutzer für einen Endpunkt umgeleitet, die Benutzer für diesen Anbieter signiert. Wenn Kunden einen Web-Browser verwenden, haben Sie die App Service automatisch alle nicht authentifizierten Benutzer auf den Endpunkt weiterleiten, der Benutzern signiert. Andernfalls müssen an Ihre Kunden, `{your App Service base URL}/.auth/login/<provider>`, wobei `<provider>` ist einer der folgenden Werte: Aad, Facebook, Google, Microsoft oder Twitter. Mobile und API-Szenarien werden in den Abschnitten weiter unten in diesem Artikel erläutert.

Benutzer, die Interaktion mit der Anwendung über einen Webbrowser haben ein Cookie festgelegt, sodass authentifizierte bleiben können sie Ihre Anwendung durchsuchen. Für andere Clients wie Mobile ein JSON Web Token (JWT), vorgelegt werden der `X-ZUMO-AUTH` -Header an den Client ausgestellt. Mobile Apps Client SDKs behandelt dies für Sie. Alternativ ein Identitätstoken Azure Active Directory oder einem Zugriffstoken möglicherweise direkt in enthalten die `Authorization` Header als [trägertoken](https://tools.ietf.org/html/rfc6750).

App Service überprüft Cookie oder Token, die Ihre Anwendung ausgibt, um Benutzer zu authentifizieren. Um zu beschränken, die die Anwendung zugreifen können, finden Sie in Abschnitt [Authorization](#authorization) .

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobile-Authentifizierung mit einem Anbieter SDK

Nachdem alles im Back-End konfiguriert ist, können Sie mobile Clients App Service anmelden ändern. Es gibt hier zwei Ansätze:

- Verwenden Sie eine SDK angegebenen Identitätsanbieter Identität und dann auf App-Dienst veröffentlicht.
- Verwenden Sie eine einzelne Codezeile, sodass Benutzer Apps Mobile Client SDK anmelden kann.

>[AZURE.TIP] Die meisten verwenden Anbieter SDK auf eine konsistente Umgebung Benutzer anmelden aktualisieren-Unterstützung und andere Anbieter gibt nutzen.

Wenn Sie ein SDK verwenden, können Benutzer Erfahrung anmelden, die enger in das Betriebssystem integriert, der die Anwendung ausgeführt wird. Außerdem bietet Ihnen ein anbietertoken und einige Informationen auf dem Client erleichtert Graph APIs und Anpassen der Benutzeroberfläche. Gelegentlich werden Sie in Blogs und Foren sehen dies genannt "Client Bewegung" oder "Client weitergeleitet" da Code auf dem Client in Benutzer und Clientcode meldet Ablauf auf ein anbietertoken.

Nachdem ein Token Provider abgerufen, muss App Service zur Überprüfung gesendet werden. Nach dem App-Dienst das Token überprüft, erstellt App-Dienst eines neuen App Service-Tokens, das an den Client zurückgegeben wird. Mobile Apps Client SDK hat Hilfsmethoden dies verwalten und automatisch das Token alle Anfragen an die Anwendung Backend. Entwickler können auch einen Verweis auf das anbietertoken beibehalten, wenn gewünscht.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobile-Authentifizierung ohne einen Anbieter SDK

Wenn Sie keinen Anbieter SDK einrichten möchten, können Sie Mobile Apps Feature von Azure App Service Sie anmelden. Mobile Apps Client SDK eine Webansicht an den Anbieter Ihrer Wahl öffnen und Benutzer anmelden. Gelegentlich in Blogs und Foren sehen Sie dies als bezeichnet "Server Bewegung" oder "Server gerichtete Bewegung", da der Server den Prozess verwaltet, der Benutzern signiert und das Client-SDK nie empfängt anbietertoken.

Code zu diesem Datenstrom ist im Lernprogramm Authentifizierung für jede Plattform enthalten. Am Ende des Flusses Client SDK ist einen App Service-Token und das Token wird automatisch an alle Anfragen an die Anwendung Back-End.

### <a name="service-to-service-authentication"></a>Dienst-Authentifizierung

Obwohl Sie Benutzer auf die Anwendung zugreifen können, können Sie auch eigene API-Aufruf in einer anderen Anwendung vertrauen. Beispielsweise könnte eine WebApp Aufruf einer API in einer anderen Webanwendung haben. In diesem Szenario verwenden Sie Anmeldeinformationen für ein Dienstkonto nicht Benutzer, ein Token abzurufen. Ein Dienstkonto ist auch *Service principal* in Azure Active Directory Begriffen und Authentifizierung, die ein Konto ist auch ein Dienst-Szenario.

>[AZURE.IMPORTANT] Da mobiler apps auf Kunden-Geräten ausgeführt werden, werden Dienste _nicht_ vertrauenswürdige Anwendung und sollte nicht verwenden einen Service principal Count. Stattdessen sollten sie einen Fluss Benutzer verwenden, der oben erläuterte.

Dienst-Szenarien können App Service die Anwendung mithilfe von Azure Active Directory schützen. Die aufrufende Anwendung muss nur Azure Active Directory Service principal Authentifizierungstoken bereitzustellen, die durch die Client-ID und Client von Azure Active Directory abgerufen werden. Ein Beispiel für dieses Szenario ASP.NET API-apps Lernprogramm [principal Authentifizierung für API-Apps]erklärt[apia-service].

Ggf. App-Authentifizierung verwenden, um einen Dienst-Szenario können Sie Clientzertifikate oder Standardauthentifizierung. Informationen über Clientzertifikate in Azure finden Sie [Wie auf TLS gegenseitige Authentifizierung konfigurieren Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informationen zur Standardauthentifizierung in ASP.NET finden Sie in [Authentifizierungsfilter in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Service Kontoauthentifizierung von einer App Service Logik app API-app ist ein Sonderfall, der detailliert [mit App Service Logik Apps Ihre benutzerdefinierte API gehostet](../app-service-logic/app-service-logic-custom-hosted-api.md)wird.

## <a name="authorization"></a>Funktionsweise von Autorisierung in App Service

Sie haben vollständige Kontrolle über die Anfragen, die die Anwendung zugreifen können. App-Authentifizierung / Autorisierung kann mit einem der folgenden Verhalten konfiguriert werden:

- Nur authentifizierte Anfragen Ihrer Anwendung zu ermöglichen.

    Wenn ein Browser eine anonyme Anforderung erhält, leiten App Service zu einer Seite für Identitätsanbieter, die Sie auswählen, dass Benutzer sich anmelden können. Wenn die Anforderung von einem mobilen Gerät ist die zurückgegebene Antwort eine HTTP _401 Unauthorized_ .

    Mit dieser Option müssen Sie Code für die Authentifizierung in Ihrer Anwendung schreiben. Benötigen Sie genauere Autorisierung steht Code Informationen über den Benutzer.

- Können Sie alle Anfragen erreicht die Anwendung aber authentifizierte Anfragen überprüfen und Authentifizierungsinformationen in HTTP-Headern weitergeben.

    Diese Option richtet Autorisierung Beschlüsse Anwendungscode. Bietet mehr Flexibilität mit anonyme Anfragen, aber Sie müssen Code schreiben.

- Alle Anfragen zu Ihrer Anwendung zulassen und Authentifizierungsinformationen in Anfragen keine Aktion ausgeführt.

    In diesem Fall die Authentifizierung / Autorisierungsfeature ist deaktiviert. Authentifizierung und Autorisierung Vorgänge Anwendungscode überlassen.

Die früheren Verhalten werden durch die Option **Aktion nicht authentifizierte Anforderung** im Azure-Portal gesteuert. Wählt *Anbieternamen *Anmelden mit ** **, alle Anträge müssen authentifiziert werden.** Anforderung (keine Aktion) ermöglichen ** überträgt die Entscheidung über die Genehmigung der Code jedoch weiterhin Authentifizierungsinformationen. Ggf. den Code alles behandeln können, deaktivieren Sie die Authentifizierung / Autorisierungsfeature.

## <a name="working-with-user-identities-in-your-application"></a>Arbeiten mit Benutzeridentitäten in der Anwendung

App-Dienst übergibt einige Benutzerinformationen der Anwendung mit Spezial-Header. Externe Anfragen verhindern diese Header und wird nur vorhanden If von App-Authentifizierung / Autorisierung. Einige Beispiel-Header umfassen:

* X-MS-CLIENT-PRINZIPAL-NAME
* X-MS-CLIENT-PRINZIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Sprache oder Framework geschriebener Code erhalten die erforderlichen Informationen aus diesen. Für ASP.NET 4.6 apps wird **ClaimsPrincipal** automatisch durch die entsprechenden Werte festgelegt.

Die Anwendung erhalten weitere Informationen über eine HTTP-GET auf der `/.auth/me` Endpunkt der Anwendung. Ein gültiges Token mit der Anforderung enthalten gibt eine JSON-Nutzlast mit Details zu den verwendeten Anbieter zugrunde liegenden anbietertoken und einige weitere Informationen zurück. Mobile Apps Server SDKs bieten Hilfsmethoden mit diesen Daten arbeiten. Weitere Informationen finden Sie unter [Azure Mobile Apps Node.js SDK verwenden](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)und [mit der Back-End-Server SDK für Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentation und zusätzliche Ressourcen

### <a name="identity-providers"></a>Identitätsanbieter
Die folgenden Lernprogramme zeigen, wie App-Dienst verwenden verschiedene Authentifizierungsanbieter konfigurieren:

- [So konfigurieren Sie Ihre app Azure Active Directory-Benutzernamen verwenden][AAD]
- [So konfigurieren Sie Ihre Anwendung auf Facebook anmelden][Facebook]
- [So konfigurieren Sie Ihre app Google anmelden][Google]
- [So konfigurieren Sie die Anwendung Microsoft Account anmelden][MSA]
- [So konfigurieren Sie Ihre Anwendung auf Twitter Login verwenden][Twitter]

Ggf. an einem Identitätssystem als den Sie können auch [benutzerdefinierte Authentifizierung unterstützt Mobile Apps .NET Server SDK Vorschau][custom-auth], die im webapps, mobiler apps oder API-apps verwendet werden können.

### <a name="web-applications"></a>ASP.NET-Webanwendungen
Die folgenden Lernprogramme zeigen, wie eine Anwendung Authentifizierung hinzugefügt:

- [Erste Schritte mit Azure App Service - Teil2][web-getstarted]

### <a name="mobile-applications"></a>Windows-Dienste
Die folgenden Lernprogramme zeigen, wie mit der Server-gesteuerte mobile Clients Authentifizierung hinzu:

- [IOS-app Authentifizierung hinzufügen][iOS]
- [Authentifizierung auf Ihrem Android hinzufügen] [Android]
- [Authentifizierung für Ihre Windows-Anwendung hinzufügen] [Windows]
- [Authentifizierung Ihrer Anwendung Xamarin.iOS hinzufügen] [Xamarin.iOS]
- [Authentifizierung Ihrer Anwendung Xamarin.Android hinzufügen] [Xamarin.Android]
- [Authentifizierung Ihrer Anwendung Xamarin.Forms hinzufügen] [Xamarin.Forms]
- [Authentifizierung für Ihre Anwendung Cordova hinzufügen] [Cordova]

Verwenden Sie die folgenden Ressourcen, wenn Client weitergeleitet Fluss Azure Active Directory angezeigt werden soll:

- [Verwenden Sie Active Directory-Authentifizierung Library für iOS][ADAL-iOS]
- [Verwenden Sie Active Directory-Authentifizierungsbibliothek für Android][ADAL-Android]
- [Verwenden Sie Active Directory-Authentifizierung-Bibliothek für Windows und Xamarin][ADAL-dotnet]

Verwenden Sie die folgenden Ressourcen Client weitergeleitet Fluss für Facebook verwendet werden soll:

- [Verwenden Sie die Facebook-SDK für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Verwenden Sie Folgendes, wenn Client weitergeleitet Fluss Twitter angezeigt werden soll:

- [Mit Twitter Fabric für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Verwenden Sie Folgendes, wenn Client weitergeleitet Fluss Google angezeigt werden soll:

- [Verwenden Sie die Google-SDK für iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-Applikationen
Die folgenden Lernprogramme zeigen, wie die API-apps zu:

- [Benutzerauthentifizierung für API-Apps in Azure App Service][apia-user]
- [Authentifizierung für API-Apps in Azure App Service principal][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
