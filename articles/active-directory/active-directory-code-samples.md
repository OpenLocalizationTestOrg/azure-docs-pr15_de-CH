<properties
   pageTitle="Codebeispiele Azure Active Directory | Microsoft Azure"
   description="Ein Index der Azure Active Directory Codebeispiele Szenario geordnet."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Azure Active Directory-Beispiele

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Microsoft Azure Active Directory (Azure AD) können Sie Ihre Webapplikationen und Web-APIs Authentifizierung und Autorisierung hinzufügen. In diesem Abschnitt finden Sie links Codebeispiele, die zeigen, wie dies funktioniert und Codeausschnitte, die Sie in Ihrer Anwendung verwenden können. Der Codebeispielseite finden Sie detaillierte Read-me Hilfethemen, Installation und Einrichtung. Und der Code wird zu der kritischen Abschnitte.

Das grundlegende Szenario für jeden Beispiel finden Sie unter Authentifizierungsszenarien Azure AD.

Zu unseren Beispielen auf GitHub beitragen: [Microsoft Azure Active Directory Beispiele und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webbrowser Anwendung

Diese Beispiele veranschaulichen, wie eine Anwendung schreiben, die der Browser des Benutzers in Azure AD anmelden leitet.



| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C# / .NET | [WebApp OpenIDConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Verwenden Sie zum Authentifizieren von Benutzern von Azure AD-Mandanten OpenID verbinden (ASP.Net OpenID verbinden owin-Middleware).
| C# / .NET | [WebApp mehrere-OpenIdConnect DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Ein Multi-Tenant .NET MVC Webanwendungsprojekt, die OpenID verbinden (ASP.Net OpenID verbinden owin-Middleware) authentifiziert Benutzer mithilfe von mehreren Azure AD-Mandanten.
| C# / .NET | [WebApp WSFederation DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Verwenden Sie WS-Federation (ASP.Net WS-Federation owin-Middleware) zum Authentifizieren von Benutzern von Azure AD-Mandanten.






## <a name="single-page-application-spa"></a>Einzelne Seite Anwendung (SPA)

Dieses Beispiel veranschaulicht, wie einer Seite Anwendung in Azure AD gesichert.  

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| JavaScript, C# / .NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | ADAL für JavaScript und Azure AD zum Sichern einer Seite AngularJS-basierte Anwendung implementiert mit einem ASP.NET Web API-Back-End verwenden.


## <a name="native-application-to-web-api"></a>Systemeigene Anwendung Web API

Diese Codebeispiele zeigen, wie systemeigene Clientanwendungen erstellen, die Web-APIs aufrufen, die von Azure AD gesichert sind. Verwenden sie [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md) und [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Verwenden Sie ADAL-Plugin für Apache Cordova eine Apache Cordova Anwendung erstellen, die ruft Web-API und Azure AD für die Authentifizierung verwendet.
| C# / .NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Eine .NET WPF-Anwendung, die eine Web-API aufgerufen wird mithilfe von Azure AD gesichert.
| C# / .NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Windows Store-Anwendung, die eine Web-API aufgerufen wird mit Azure gesichert.
| C# / .NET | [NativeClient WebAPI-mehrere WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Eine Windows Store-Anwendung Multi-Tenant-Web-API, die gesichert wurden mit Azure aufrufen.
| C# / .NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Eine systemeigene Clientanwendung, die eine Web-API aufruft, ruft einen Token für den ursprünglichen Benutzer fungieren, verwendet das Token dann einem anderen Web-API aufrufen.
| C# / .NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Azure AD sichern eine Windows Store-Anwendung für Windows Phone 8.1, die Web-API aufruft.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | Eine iOS-Anwendung, die eine Web-API aufruft, erfordert Azure AD für Authentifizierung.
| C# / .NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Eine native Clientanwendung, die Logik zum Verarbeiten JWT-Token in einem Web-API anstatt owin-Middleware.
| C# / Xamarin | [NativeClient Xamarin Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Eine Xamarin Bindung an das systemeigene Azure AD Authentifizierung Library (ADAL) für Android-Bibliothek.
| C# / Xamarin | [NativeClient Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Eine Xamarin Bindung an das systemeigene Azure AD Authentifizierung Library (ADAL) für iOS.
| C# / Xamarin | [NativeClient MultiTarget DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Ein Xamarin-Projekt, das auf fünf Plattformen und Web-API aufruft, wird von Azure AD gesichert.
| C# / .NET | [NativeClient Headless DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Systemeigene Anwendung, die nicht interaktive Authentifizierung und Web-API aufruft, wird von Azure AD gesichert.



## <a name="web-application-to-web-api"></a>Web Application to Web-API

Diese Codebeispiele anzeigen wie [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) zu Aufrufen von ASP.NET-Webanwendungen verwenden web-APIs von Azure AD geschützt sind.

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C# / .NET | [WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Rufen Sie Web-API mit Berechtigungen des angemeldeten Benutzers.
|  C# / .NET | [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Web-API mit Berechtigungen der Anwendung aufrufen.
| C# / .NET | [WebApp-WebAPI-OAuth2-Benutzeridentität-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Fügen Sie [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) Autorisierung einer vorhandenen Anwendung Web-API aufgerufen werden kann hinzu.
| JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Richten Sie ein REST API-Dienst, der in Azure AD Schutz API integriert ist. Enthält einen Node.js-Server mit einer Web-API.
| C# / .NET | [WebApp-WebAPI-mehrere-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Multi-Tenant MVC web-Anwendung, die OpenID verbinden (ASP.Net OpenID verbinden owin-Middleware) authentifiziert Benutzer mithilfe von Azure AD-Mandanten. Verwendet einen Autorisierungscode Graph-API aufrufen.

## <a name="server-or-daemon-application-to-web-api"></a>Server- oder Dämon Web API

Diese Codebeispiele zeigen, wie eine Daemon oder Server-Anwendung erstellen, die Ressourcen aus einem Web API ruft mithilfe von [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md) und [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C# / .NET | [DotNet-Daemon](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Eine ruft Web-API. Die Client-Anmeldeinformationen ist ein Kennwort.
| C# / .NET | [Daemon CertificateCredential DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Eine Konsolenanwendung, die eine Web-API aufruft. Die Client-Anmeldeinformationen ist ein Zertifikat.


## <a name="calling-azure-ad-graph-api"></a>Azure AD Graph-API aufrufen

Diese Beispiel zeigen, wie Clientanwendungen erstellen, die die Azure AD Graph-API zum Lesen und schreiben Daten aufrufen.

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| Java | [WebApp GraphAPI Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Eine Anwendung, die die Graph-API auf Verzeichnisdaten Azure AD verwendet.
| PHP | [WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Eine Anwendung, die die Graph-API auf Verzeichnisdaten Azure AD verwendet.
| C# / .NET | [WebApp GraphAPI DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Eine Anwendung, die die Graph-API auf Verzeichnisdaten Azure AD verwendet.
| C# / .NET | [ConsoleApp GraphAPI DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Diese Konsole app häufige lesen und schreiben die Graph-API aufgerufen, und veranschaulicht Lizenz von Benutzerrechten ausführen und Aktualisieren des Benutzers Vorschaubild und Links.
| C# / .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Eine Konsolenanwendung, die differenzielle Abfrage Azure AD-Mandanten zu periodischen Änderungen Benutzerobjekte in der Graph-API verwendet.
| C# / .NET | [WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Eine MVC-Anwendung verwendet ein einfaches Unternehmen Organigramm zu Graph-API Abfragen.
| PHP | [WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Eine PHP-Anwendung, die das Diagramm API, um eine Erweiterung zu registrieren und dann lesen, aktualisieren und löschen Sie Werte in das Extension-Attribut.


## <a name="authorization"></a>Autorisierung

Diese Codebeispiele zeigen, wie Azure AD für die Autorisierung verwenden.

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C# / .NET | [WebApp GroupClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Führen Sie rollenbasierte Zugriffskontrolle (RBAC) mit Gruppenansprüche Azure Active Directory in einer Anwendung in Azure AD integriert ist.
| C# / .NET | [WebApp RoleClaims DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Führen Sie rollenbasierte Zugriffskontrolle (RBAC) mit Anwendungsrollen Azure Active Directory in einer Anwendung in Azure AD integriert ist.


## <a name="legacy-walkthroughs"></a>Ältere Exemplarische Vorgehensweisen

Diese exemplarischen Vorgehensweisen ältere Technologie jedoch von Interesse sein könnten.

| Sprache-Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C# / .NET | [Rollen und ACL-basierte Autorisierung in einer Anwendung Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=331694) | Führen Sie rollenbasierte Autorisierung (RBAC) und ACL-Autorisierung in einer Anwendung in Azure AD integriert ist.
| C# / .NET |  [AAL - Windows Store-app mit anderen Dienst - Authentifizierung](http://go.microsoft.com/fwlink/?LinkId=330605) |  Verwenden Sie [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md) (früher AAL) für Windows Store Windows Store-app Authentifizierungsfunktionen Benutzer hinzu.
| C# / .NET | [ADAL - systemeigene Anwendung REST Service - Authentifizierung mit AAD über Browser](http://go.microsoft.com/fwlink/?LinkId=259814) |  Verwenden Sie [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md) einen WPF-Client Authentifizierung Benutzerberechtigungen hinzufügen.
| C# / .NET | [ADAL - systemeigene Anwendung REST Service - Authentifizierung mit ACS über Browser](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Verwenden Sie [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md) und [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) einen WPF-Client Authentifizierung Benutzerberechtigungen hinzufügen.
| C# / .NET | [ADAL - Server-zu-Server-Authentifizierung](http://go.microsoft.com/fwlink/?LinkId=259816) | Verwenden Sie [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md) zum Sichern von Service-Aufrufe aus eine serverseitige Prozess ein MVC4 Web API REST-Dienst.
| C# / .NET | [Ihre Webanwendung in Azure AD anmelden hinzufügen](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Konfigurieren einer Anwendung .NET Web einmaliges gegen Ihre Azure AD Unternehmensverzeichnis ausführen.
| C# / .NET | [Entwicklung Multi-Tenant ASP.NET-Webanwendungen mit Azure](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Verwenden Sie Azure AD einmaliges und Directory-Zugriffsfunktionen von einem .NET Anwendung in mehreren Organisationen hinzu.
JAVA | [Java-Beispiel-App für Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkId=263969) | Mithilfe der Graph-API Zugriff auf Verzeichnisdaten von Azure AD.
PHP | [PHP-Beispiel-App für Azure AD Graph API](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Mithilfe der Graph-API Zugriff auf Verzeichnisdaten von Azure AD.
| C# / .NET | [Beispiel-App für Azure AD Graph API](http://go.microsoft.com/fwlink/?LinkID=262648) | Mithilfe der Graph-API Zugriff auf Verzeichnisdaten von Azure AD.
| C# / .NET | [Beispiel-App für differenzielle Abfrage Azure AD-Diagramm](http://go.microsoft.com/fwlink/?LinkId=275398) | Die differenzielle Abfrage in der Graph-API periodische Änderungen Benutzerobjekte in Azure AD-Mandanten zu verwenden.
| C# / .NET | [Beispiel-App für die Integration von Multi-Tenant Cloudanwendung Azure AD](http://go.microsoft.com/fwlink/?LinkId=275397) | Multi-Tenant-Anwendung in Azure AD integriert.
| C# / .NET | [Eine Windows Store-Anwendung und REST-Webdienst mithilfe von Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Erstellen einer einfachen Web-API-Ressource und eine Windows Store-Clientanwendung Azure AD- und [Azure AD Authentifizierung Library (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [Verwenden von Graph API Azure AD Abfragen](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Konfigurieren einer Anwendung Microsoft .NET Azure AD Graph-API verwenden, um Daten aus einem Mandanten Azure AD-Verzeichnis zugreifen.

## <a name="see-also"></a>Siehe auch

##### <a name="other-resources"></a>Andere Ressourcen

[Azure Active Directory Developer's Guide](active-directory-developers-guide.md)

[Azure AD Graph API konzeptionellen und Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Hilfsprogrammbibliothek für Azure AD Graph-API](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

