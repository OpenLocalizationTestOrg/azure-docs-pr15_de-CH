<properties
   pageTitle="Azure Active Directory 2.0 authentifizierungsbibliotheken | Microsoft Azure"
   description="Kompatible Clientbibliotheken und Middleware Serverbibliotheken verknüpfte Bibliothek, Quelle und Samples Links für Azure Active Directory v2. 0-Endpunkt."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory 2.0 authentifizierungsbibliotheken
Azure Active Directory (Azure AD) v2. 0-Endpunkt unterstützt die OAuth 2.0 und OpenID verbinden 1.0 Standardprotokolle. Verschiedene Bibliotheken von Microsoft und anderen Organisationen können Sie mit der Version 2.0.

Beim Erstellen einer Anwendung, den Endpunkt v2. 0 verwendet, empfiehlt Sie Bibliotheken, die Experten Protokoll geschrieben werden, die eine Methode (Security Development Lifecycle, SDL) wie [die Microsoft folgen]folgen[Microsoft-SDL]. Beschließen Hand Code Unterstützung für Protokolle sollten folgen Methodik von SDL und achten Sie besonders auf die Sicherheitsaspekte in den Standards Spezifikationen für jedes Protokoll.

## <a name="types-of-libraries"></a>Arten von Bibliotheken

Azure AD 2.0 arbeitet mit zwei Arten von Bibliotheken:

- **Client-Bibliotheken**. Systemeigene Clients und Server verwenden Clientbibliotheken zu Zugriffstoken für eine Ressource, z. B. Microsoft Graph aufrufen.
- **Server-Middleware-Bibliotheken**. Webapps verwenden Server Middleware Bibliotheken für Benutzer anmelden. Web-APIs verwenden Server Middleware Bibliotheken Token zu bestätigen, die systemeigene Clients oder anderen Servern gesendet werden.

## <a name="library-support"></a>Unterstützung
Da alle Standard-Bibliothek der v2. 0-Endpunkt verwenden stehen, ist es wichtig zu wissen, wo Sie Unterstützung. Probleme und Wünsche im Bibliothekscode erhalten Sie Bibliotheksbesitzer. Probleme und Wünsche Implementierung dienstseitigen-wenden Sie sich an Microsoft.

Bibliotheken sind in zwei Support-Kategorien:

- **Microsoft unterstützt**. Microsoft stellt für diese Bibliotheken, und hat SDL Sorgfalt auf diese Bibliotheken.
- **Kompatibel**. Microsoft hat diese Bibliotheken in grundlegenden Szenarien getestet und bestätigt mit v2. 0-Endpunkt. Microsoft bietet keine Updates für diese Bibliotheken und hat keine Überprüfung dieser Bibliotheken. Probleme und Wünsche der Bibliothek Open Source-Projekt richten.

Eine Liste der Bibliotheken, die mit der Version 2.0 arbeiten, finden Sie in den nächsten Abschnitten in diesem Artikel.

## <a name="microsoft-supported-client-libraries"></a>Microsoft unterstützt Clientbibliotheken
| Plattform| Bibliotheksnamen| Herunterladen | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| Xamarin .NET Windows Store | Microsoft-Authentifizierung Library (MSAL) für .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL für .NET (GitHub)][ClientLib-NET-Repo] | [Beispiel für Windows desktop native client][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js-Plug-in | [Passport-Azure-AD (Npm)][ClientLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ClientLib-Node-Repo] | Demnächst |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft unterstützt Server-Middleware-Bibliotheken
| Plattform| Bibliotheksnamen| Herunterladen | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | Owin-OpenID Connect Middleware für ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana-Projekt (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Beispiel für Web app][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | Owin-OAuth Träger Middleware für ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana-Projekt (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Web-API-Beispiel][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET core | Owin-OpenID Middleware für .NET Core verbinden | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Beispiel für Web app][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET core | Owin-OAuth Träger Middleware für .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET Security (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Demnächst |
| Node.js | Microsoft Azure Active Directory Passport.js-Plug-in | [Passport-Azure-AD (Npm)][ServerLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ServerLib-Node-Repo] | [Beispiel für Web app][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Kompatible Clientbibliotheken
| Plattform| Bibliotheksnamen | Getestete version | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Systemeigene app-Beispiel](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Systemeigene app-Beispiel](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Demnächst |
| Python-Kolben | [Kolben-OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Kolben-OAuthlib](https://github.com/lepture/flask-oauthlib) | Demnächst |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>Omniauth-Oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Demnächst |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Kompatible Server Middleware-Bibliotheken
Demnächst

## <a name="related-content"></a>Verwandte Inhalte
Weitere Informationen zu Azure AD v2. 0-Endpunkt finden Sie unter [Azure AD app 2.0 Übersicht][AAD-App-Model-V2-Overview].

Um optimieren und unseren Inhalt, bitte Funktion Disqus Kommentare am Ende dieses Artikels, um Feedback zu geben.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
