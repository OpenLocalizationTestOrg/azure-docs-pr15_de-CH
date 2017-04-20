<properties
    pageTitle="Authentifizierung und Autorisierung für API-Apps in Azure App Service | Microsoft Azure"
    description="Erfahren Sie mehr über die Authentifizierung und Autorisierung Dienste von Azure App Service API-Apps."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Authentifizierung und Autorisierung für API-Apps in Azure App Service

## <a name="overview"></a>Übersicht 

> [AZURE.NOTE] In diesem Thema Migration zu einem konsolidierten [App-Authentifizierung / Autorisierung](../app-service/app-service-authentication-overview.md) Thema umfasst Web, Mobile und API-Apps.

Azure App Service bietet integrierte Authentifizierung und Autorisierung Dienste, [die OAuth 2.0](#oauth) und [OpenID verbinden](#oauth)implementieren. Dieser Artikel beschreibt die Dienste und die verfügbaren Optionen für API-Apps in Azure App Service.

Das folgende Diagramm veranschaulicht die wichtigsten Eigenschaften von App-Authentifizierung:

* Es vorverarbeitet API-Anfragen, d. h. funktioniert mit allen Sprachen oder Rahmen vom App-Dienst unterstützt.
* Es gibt Ihnen verschiedene Optionen für wie viel Authentifizierung arbeiten Sie in Ihrem eigenen Code durchführen möchten.
* Für Endbenutzer und Dienstauthentifizierung funktioniert. 
* Unterstützt fünf Identitätsanbieter: Azure Active Directory, Facebook, Google, Twitter und Microsoft Account.
* Es funktioniert genauso für API-Apps, Web Apps und Mobile Apps.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Sprache unabhängig

Verarbeitung der App Service-Authentifizierung erfolgt, bevor Anfragen Ihre app API bedeutet erreichen, dass die Authentifizierungsfunktionen für API-apps in beliebiger Sprache oder Rahmen.  Ihre API kann ASP.NET, Java, Node.js oder App unterstützt Frameworks basieren.

App Service JSON Web Token (JWT) in der Authorization-Header einer HTTP-Anforderung übergeben und in allen Sprachen oder Framework geschriebenen Code erhalten die Informationen aus dem Token. Darüber hinaus ermöglicht App Service einfacher Zugriff auf die am häufigsten verwendeten Ansprüche durch einige spezielle Header wie folgt festlegen:

* X-MS-CLIENT-PRINZIPAL-NAME
* X-MS-CLIENT-PRINZIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
In einer .NET API können Sie die `Authorize` -Attribut und für eine fein abgestufte Autorisierung können Sie mühelos schreiben code basierend auf Ansprüchen, da Daten in Klassen gefüllt ist.

## <a name="multiple-protection-options"></a>Mehrere Optionen

App Service kann verhindern, dass anonyme HTTP-Anfragen erreichen Ihre app API übergeben alle Requests und Überprüfen von Token für Anfragen, die sie enthalten kann oder sie können über alle Anfragen ohne auf:

1. Nur authentifizierte Anfragen Ihre API-Anwendung zu ermöglichen.

    Wenn eine anonyme Anforderung vom Browser empfangen wird, App Service eine Umleitung auf eine Anmeldeseite für Authentifizierung (Azure AD, Google, Twitter, etc.), die Sie auswählen. 

    Mit dieser Option müssen alle Authentifizierungscode in Ihrer Anwendung schreiben und Autorisierungscode wird vereinfacht, da die wichtigsten Ansprüche in HTTP-Headern bereitgestellt werden.

2. Können Sie alle Anfragen Ihre app API jedoch authentifizierte Anfragen überprüfen und Authentifizierungsinformationen in der HTTP-Header übergeben.

    Dadurch mehr Flexibilität mit anonyme Anfragen, aber Sie müssen Code schreiben, möchten Sie verhindern, dass anonyme Benutzer mithilfe der API. Da die am häufigsten verwendeten Ansprüche im Header der HTTP-Anfragen übergeben werden, ist Autorisierungscode relativ einfach.
    
3. Lassen Sie alle Anfragen, erreichen Ihre API keine Aktion Authentifizierungsinformationen in Anfragen zu.

    Diese Option bewirkt, dass die Aufgaben der Authentifizierung und Autorisierung Anwendungscode überlassen.

In [Azure-Portal](https://portal.azure.com/)die Option auf der **Authentifizierung / Autorisierung** Blade.

![](./media/app-service-api-authentication/authblade.png)

Option 1 und 2 aktivieren Sie **App-Authentifizierung**, und wählen Sie in der Liste **Aktion Anforderung nicht authentifiziert** **Anmelden** oder **Anforderung zulassen (keine Aktion)**.  Wählen Sie **Anmelden**haben Sie Authentifizierungsanbieter auswählen und diesen Anbieter konfigurieren.

![](./media/app-service-api-authentication/actiontotake.png)

Ausführliche Informationen zum Konfigurieren der Authentifizierung finden Sie unter [Ihre App mit Azure Active Directory-Konto konfigurieren](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Der Artikel bezieht sich auf API-apps sowie mobiler apps und links zu anderen Artikeln für die anderen Authentifizierungsanbieter.
 
## <a id="internal"></a>Dienstauthentifizierung

App-Authentifizierung für interne Szenarien wie für den Aufruf einer API-App von einer anderen API-Anwendung funktioniert. In diesem Szenario erhalten Sie einen Token mit Anmeldeinformationen für ein Dienstkonto nicht Endbenutzer. Ein Dienstkonto ist auch ein *Service principal* in Azure Active Directory und Authentifizierung mit einem Konto ist auch ein Dienst-Szenario. 

Dienst-Szenarien schützen Sie aufgerufene API-Anwendung mithilfe von Azure Active Directory und geben Sie AAD Service principal Authentifizierungstoken beim Aufrufen der API-app. Sie erhalten einen Token durch die Client-ID und Client von AAD-Anwendung. Keine spezieller Azure-Code ist erforderlich wie für Mobile Dienste Zumo Token behandeln. Ein Beispiel für dieses Szenario mit ASP.NET API-apps wird durch das Lernprogramm [principal Authentifizierung für API-Apps](app-service-api-dotnet-service-principal-auth.md)abgedeckt.

Wenn ein Dienst-Szenario ohne App-Authentifizierung behandelt werden soll, können Sie Clientzertifikate oder Standardauthentifizierung. Informationen über Clientzertifikate in Azure finden Sie [Wie auf TLS gegenseitige Authentifizierung konfigurieren Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informationen zur Standardauthentifizierung in ASP.NET finden Sie in [Authentifizierungsfilter in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Service Kontoauthentifizierung von einer App Service Logik app API-app ist ein Sonderfall, der [mit Ihrer benutzerdefinierte API App Service Logik Apps gehostet](../app-service-logic/app-service-logic-custom-hosted-api.md)erläutert.

## <a name="mobile-client-authentication"></a>Mobile Client-Authentifizierung

Informationen zur Authentifizierung von mobilen Clients zu behandeln, finden in der [Dokumentation zur Authentifizierung für mobile apps](../app-service-mobile/app-service-mobile-ios-get-started-users.md). App-Authentifizierung funktioniert genauso für mobile und API-apps.
  
## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zur Authentifizierung und Autorisierung in Azure App Service finden Sie in folgenden Ressourcen:

* [Erweiterbare App-Authentifizierung / Autorisierung](/blog/announcing-app-service-authentication-authorization/)
* [Wie Sie Ihre App mit Azure Active Directory-Konto konfigurieren](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Enthält Links für anderen Authentifizierungsanbieter am oberen Rand der Seite.) 

Weitere Informationen zu OAuth 2.0 OpenID verbinden und JSON Web Token (JWT) finden Sie in folgenden Ressourcen.

* [Erste Schritte mit OAuth 2.0] (http://shop.oreilly.com/product/0636920021810.do "Erste Schritte mit OAuth 2.0") 
* [Einführung in OAuth2 OpenID verbinden und JSON Web Token (JWT) - PluralSight-Kurses](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Erstellen und Sichern einer RESTful API für mehrere Clients in ASP.NET - PluralSight-Kurses](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Weitere Informationen zu Active Directory Azure finden Sie unter den folgenden Ressourcen.

* [Azure AD-Szenarien](http://aka.ms/aadscenarios)
* [Azure AD Entwicklerhandbuch](http://aka.ms/aaddev)
* [Azure AD-Beispiele](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel erklärt die Verwendung für API-apps Authentifizierung und Autorisierung Features der App Service. Nächste Lernprogramm immer gestartet Serie veranschaulicht [Benutzerauthentifizierung in App Service API-Apps](app-service-api-dotnet-user-principal-auth.md).
