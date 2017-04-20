<properties
   pageTitle="Azure Active Directory-Authentifizierungsbibliotheken | Microsoft Azure"
   description="Azure AD Authentifizierung Library (ADAL) Client ermöglicht Anwendungsentwicklern einfach in die cloud Benutzerauthentifizierung oder lokale Active Directory (AD) und dann Zugriffstoken für das Sichern von API-aufrufen."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory-Authentifizierungsbibliotheken

Azure AD-Authentifizierung Library (ADAL) ermöglicht es Anwendungsentwicklern, einfach in die cloud Benutzerauthentifizierung Client lokal Active Directory (AD) und Zugriffstoken für API-Aufrufe Absichern von zu erhalten. ADAL verfügt über viele Funktionen, stellen Authentifizierung einfacher für Entwickler, wie asynchrone Unterstützung einer konfigurierbaren token Cache, das Zugriffstoken und Aktualisierung Tokens, automatische token aktualisieren Zugriffstoken abläuft und Aktualisierungstoken verfügbar ist, speichert und vieles mehr. Behandelt die meisten der Komplexität, ADAL können sich Entwickler auf die Geschäftslogik in der Anwendung und sichere Ressourcen problemlos ohne ein Experte auf Sicherheit.

ADAL steht für verschiedene Plattformen.

|Plattform|Paketname|Client-Server|Herunterladen|Quellcode|Dokumentation und Beispiele|
|---|---|---|---|---|---|
|.NET Client, Windows Store UWP Xamarin IOS- und Android|Active Directory-Authentifizierung Library (ADAL) für .NET v3 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL für .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Dokumentation](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET Client Windows Store Windows Phone 8.1 |Active Directory-Authentifizierung Library (ADAL) für .NET v2 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL für .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Dokumentation](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Active Directory-Authentifizierung Library (ADAL) für JavaScript|Client|[ADAL für JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL für JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Beispiel: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X, iOS|Active Directory-Authentifizierung Library (ADAL) für Objective-C|Client|[ADAL für Objective-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL für Objective-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Beispiel: [NativeClient iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Active Directory-Authentifizierung Library (ADAL) für Android|Client|[ADAL für Android (das zentrale Repository)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL für Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Beispiel: [NativeClient Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Active Directory-Authentifizierung Library (ADAL) für Node.js|Client|[ADAL für Node.js (Npm)](https://www.npmjs.com/package/adal-node)|[ADAL für Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Beispiel: [WebAPI Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport-Authentifizierung Middleware für Knoten|Client|[Azure Active Directory Passport Node.js (Npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Active Directory-Authentifizierung Library (ADAL) für Java|Client|[ADAL für Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL für Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Identität Protocol Extensions für Microsoft.NET Framework 4.5|Server|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD Identität Modell Extensions for .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Web-Tokenhandler von JSON für Microsoft .net Framework 4.5|Server|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD Identität Modell Extensions for .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Owin-Middleware, die eine Anwendung von Microsoft-Technologie für die Authentifizierung verwenden kann.|Server|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|Owin-Middleware, die OpenIDConnect für die Authentifizierung verwenden kann.|Server|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Beispiel: [WebApp OpenIDConnecty DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|Owin-Middleware, die eine Anwendung WS-Federation für die Authentifizierung verwenden kann.|Server|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Beispiel: [WebApp WSFederation DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Szenarien

Hier sind drei Szenarien, in denen ADAL für die Authentifizierung verwendet werden kann.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Authentifizieren der Benutzer einer Clientanwendung mit einer

In diesem Szenario wurde ein Entwickler einen Client wie eine WPF-Anwendung, die eine remote von Azure AD wie Web API gesicherte Ressource zuzugreifen. Er besitzt ein Azure-Abonnement, das downstream Web API Aufrufen kennt und weiß Azure AD-Mandanten, die Web-API verwendet. ADAL können er daher um Authentifizierung bei Azure AD oder vollständig Delegieren der Authentifizierung Erfahrung ADAL explizit behandeln Benutzeranmeldeinformationen zu erleichtern. ADAL macht fordert den Benutzer authentifizieren, erhalten ein Zugriffstoken und Aktualisierungstoken von Azure AD und verwenden Sie das Zugriffstoken zu Web-API.

Ein Codebeispiel, das diese Authentifizierung Azure AD Szenario veranschaulicht, finden Sie unter [Systemeigene WPF-Clientanwendung Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Eine Server-Anwendung mit einer Authentifizierung

In diesem Szenario wurde ein Entwickler eine Anwendung auf einem Server, die eine remote von Azure AD wie Web API gesicherte Ressource zuzugreifen. Er besitzt ein Azure-Abonnement den Downstreamdienst aufrufen kann und weiß Azure AD-Mandanten Web-API verwendet. ADAL können er daher explizit behandeln die Anwendung Anmeldeinformationen Authentifizierung mit Azure erleichtern. ADAL macht fordert einen Token von Azure AD mithilfe der Anwendung Clientanmeldeinformationen abrufen und verwenden Sie Tokens zu Web-API. ADAL übernimmt auch die Verwaltung der Lebensdauer des Zugriffstokens Zwischenspeichern und ggf. erneuern. Ein Codebeispiel, die dieses Szenario veranschaulicht, finden Sie unter [Console Application Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Eine Server-Anwendung für einen Benutzer auf einer Authentifizierung

In diesem Szenario wurde ein Entwickler eine Anwendung auf einem Server, die eine remote von Azure AD wie Web API gesicherte Ressource zuzugreifen. Die Anforderung auch im Namen eines Benutzers in Azure AD erstellt werden soll. Er besitzt ein Azure-Abonnement, weiß, wie die downstream-API-aufrufen und Azure AD verwendet den Mieter kennt. Sobald der Benutzer die Anwendung authentifiziert wurde, kann die Anwendung einen Autorisierungscode für den Benutzer von Azure AD erhalten. Die Anwendung können dann ADAL Zugriffstoken und Token für einen Benutzer mit der Anwendung von Azure AD und Servercode Anmeldeinformationen aktualisieren. Nachdem die Anwendung im Besitz des Zugriffstokens wird nennen es Web API Token abgelaufen ist. Ablauf des Tokens können die Anwendung ADAL zu einem Zugriffstoken mit der Aktualisierung token bereits empfangen wurde.


## <a name="see-also"></a>Siehe auch

[Azure Active Directory developer's guide](active-directory-developers-guide.md)

[Authentifizierungsszenarien Azure Active Directory](active-directory-authentication-scenarios.md)

[Azure Active Directory-Beispiele](active-directory-code-samples.md)
