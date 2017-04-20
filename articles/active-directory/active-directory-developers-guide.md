<properties
   pageTitle="Azure Active Directory-Entwicklerhandbuch | Microsoft Azure"
   description="Dieser Artikel bietet eine umfassende Anleitung für entwicklerorientierte Ressourcen für Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory developer's guide

## <a name="overview"></a>Übersicht
Ein Identity Management als Serviceplattform (IDMaaS) bietet Azure Active Directory (AD) Entwicklern effektiv Identitätsmanagement in ihre Anwendung integrieren. Die folgenden Artikel bieten Übersichten für Implementierung und die wichtigsten Features von Azure AD. Sie sollten in der Reihenfolge gelesen oder springen, [Erste Schritte](#getting-started) , wenn Sie beschäftigen möchten.


1. [Die Vorteile von Azure AD-Integration](./develop/active-directory-how-to-integrate.md): Entdecken Sie, warum Integration in Azure AD die beste Lösung für die sichere Anmeldung und Autorisierung bietet.

1. [Azure AD Authentifizierungsszenarien](active-directory-authentication-scenarios.md): vereinfachte Authentifizierung in Azure AD Anmeldung der Anwendung nutzen.

1. [Integration mit Azure AD](active-directory-integrating-applications.md): Informationen zum Hinzufügen, aktualisieren und Entfernen von Clientanwendungen von Azure AD und der Richtlinien für integrierte apps.

1. [Azure AD Graph-API](active-directory-graph-api.md): Azure AD Graph-API verwenden, um programmgesteuert Azure AD über Endpunkte REST-API zugreifen. Azure AD Graph ist auch über [Microsoft Graph](https://graph.microsoft.io/). Microsoft Graph bietet eine einheitliche API, die Zugriff auf mehrere Microsoft Cloud-Dienst-APIs über einen einzelnen Endpunkt REST-API und mit einem einzigen Token ermöglicht.

1. [Azure AD-authentifizierungsbibliotheken](active-directory-authentication-libraries.md): einfache Benutzerauthentifizierung zu Zugriffstoken mit Azure AD-authentifizierungsbibliotheken für .NET JavaScript Objective-C und Android.


## <a name="getting-started"></a>Erste Schritte

Diese Lernprogramme für mehrere Plattformen zugeschnitten und können Sie schnell starten mit Azure Active Directory. Voraussetzung ist müssen Sie [einen Mandanten Azure Active Directory erhalten](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Mobile und PC-Anwendung Schnellstart-Handbücher

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Universal Windows](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Universal Windows](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Direkte Integration mit OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Web Application Schnellstart-Handbücher

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID verbinden](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Direkte Integration mit OpenID verbinden](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Web-API Schnellstart-Handbücher

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Abfragen des Directory-Schnellstart-Handbuchs

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph-API](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>Gewusst wie

Diese Artikel beschreiben, wie bestimmte Aufgaben mithilfe von Azure Active Directory:

- [Abrufen von Azure AD-Mandanten](active-directory-howto-tenant.md)
- [Jede Azure AD-Benutzerobjekt mit der Multi-Tenant-Anwendung anmelden](active-directory-devhowto-multi-tenant-overview.md)
- Aktivieren Sie Cross-app SSO verwenden ADAL, [Android](active-directory-sso-android.md) und [iOS](active-directory-sso-ios.md) -Geräte
- [Die Anwendung Elemente verwenden zertifiziert für Azure AD](active-directory-devhowto-appsource-certified.md)
- [Ihre Anwendung in der Galerie Azure AD](active-directory-app-gallery-listing.md)
- [Senden Sie Web-apps für Office 365 Verkäufer-Cockpit](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Kennen Sie Azure Active Directory Anwendungsmanifest](active-directory-application-manifest.md)
- [Verstehen der Richtlinien für die Übernahme Schaltflächen anmelden und app in der Clientanwendung](active-directory-branding-guidelines.md)
- [Vorschau: Wie apps erstellen, die Benutzer mit sowohl persönliche & Geschäfts-oder signieren](active-directory-appmodel-v2-overview.md)
- [Vorschau: Wie apps, die Verbraucher anmelden Anmelden](../active-directory-b2c/active-directory-b2c-overview.md)
- [Vorschau: Konfigurieren von token Lebensdauer in Azure AD](active-directory-configurable-token-lifetimes.md) mithilfe von PowerShell. Anzeigen Sie [Operationen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) und [Policy-Entität](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) Einzelheiten zur Konfiguration von Azure AD Graph API

## <a name="reference"></a>Referenz

Dieser Artikel enthält eine Grundlage für REST Authentifizierungsbibliothek APIs, Protokolle, Fehler, Codebeispiele und Endpunkte.  

###  <a name="support"></a>Unterstützung
- [Markierter Fragen](http://stackoverflow.com/questions/tagged/azure-active-directory): Ermitteln von Azure Active Directory Lösungen Stapelüberlauf Tags [Azure Active Directory](http://stackoverflow.com/questions/tagged/azure-active-directory) suchen und [adal](http://stackoverflow.com/questions/tagged/adal).
- Siehe [Azure AD Entwickler Glossar](active-directory-dev-glossary.md) Definitionen von häufig verwendeten Begriffen Anwendungsentwicklung und Integration.

### <a name="code"></a>Code

- [Azure Active Directory Open Source - Bibliotheken](http://github.com/AzureAD): zu einer Quelle am einfachsten mithilfe unserer [Liste](active-directory-authentication-libraries.md).

- [Azure Active Directory Beispiele](https://github.com/azure-samples?query=active-directory): die einfachste Methode zum Durchsuchen der Liste von Beispielen ist mit dem [Index der Codebeispiele](active-directory-code-samples.md).

- [Active Directory Authentifizierung Library (ADAL) für .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - Dokumentation ist für [die letzte Hauptversion](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) und die [vorherige Hauptversion](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph-API

- [Graph-API-Referenz](https://msdn.microsoft.com/library/azure/hh974476.aspx): REST Referenz für Azure Active Directory Graph API. [Graph-API Reference interaktiv anzuzeigen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Graph-API berechtigungsbereiche](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0 berechtigungsbereiche, die den Zugriff steuern, die eine Anwendung Verzeichnisdaten in einem Mandanten verwendet werden.

### <a name="authentication-and-authorization-protocols"></a>Authentifizierung und Autorisierung Protokolle

- [Signatur Schlüssel Rollover in Azure AD](active-directory-signing-key-rollover.md): erfahren Sie mehr über Azure AD Signieren Key Rollover Cadence und den Schlüssel für die häufigsten Anwendungsszenarien aktualisieren.

- [Protokoll OAuth 2.0: Grant Code Autorisierung mit](active-directory-protocols-oauth-code.md): können OAuth 2.0 Protokoll Authorization Code gewähren Zugriff auf Web Applications autorisieren und Web-APIs in Azure Active Directory Mieter.

- [Protokoll OAuth 2.0: verstehen die implizite Gewährung](active-directory-dev-understanding-oauth2-implicit-grant.md): erfahren Sie mehr über implizite Berechtigung gewähren und für Ihre Anwendung.

- [Protokoll OAuth 2.0: Dienst aufrufen Verwendung Clientanmeldeinformationen](active-directory-protocols-oauth-service-to-service.md): das OAuth 2.0 Clientanmeldeinformationen Grant Funktion kann ein Webdienst (vertrauliche Client) mit eigenen Anmeldeinformationen authentifizieren beim Aufruf eines anderen Web Service statt einen Benutzer imitiert. In diesem Szenario ist der Client in der Regel ein Middle-Tier-Webdienst, Daemon-Dienst oder Website.

- [Protokoll OpenID verbinden 1.0: Anmeldung und Authentifizierung](active-directory-protocols-openid-connect-code.md): der OpenID verbinden 1.0-Protokoll erweitert OAuth 2.0 als Authentifizierungsprotokoll verwendet. Eine Clientanwendung kann wird ein ID-Token um den Vorgang zu verwalten oder erweitern den Fluss Authorization Code um ein ID-Token und Autorisierungscode erhalten.

- [SAML 2.0 Protokoll Verweis](active-directory-saml-protocol-reference.md): das SAML 2.0-Protokoll kann Anwendungsprogramme einzelne Anmelden ihren Benutzern zur Verfügung.

- [WS-Federation 1.2-Protokoll](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory unterstützt WS-Federation 1.2 gemäß der Spezifikation Web Services Federation Version 1.2. Weitere Informationen über das Verbundmetadaten-Dokument finden Sie unter [Verbundmetadaten](active-directory-federation-metadata.md).

- [Unterstützte Typen von Token und Anspruch](active-directory-token-and-claims.md): Sie können dieses Handbuch verstehen und bewerten die Ansprüche im Token SAML 2.0 und JSON Web Token (JWT).

## <a name="videos"></a>Videos

### <a name="build"></a>Erstellen

Diese Präsentationen zur Entwicklung von apps mithilfe von Azure Active Directory Feature Lautsprecher an das engineering-Team arbeiten. Die Präsentationen Themen grundlegende, einschließlich IDMaaS, Authentifizierung, Identitätsverbund und einmaliges Anmelden.

- [Microsoft Identity: Zustand der Union und die Zukunft](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Identity-Management als Service für moderne](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Moderne Web-Anwendungsentwicklung Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Anwendungsentwicklung moderne systemeigene Azure Active Directory](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure Freitag
[Azure Freitag](https://azure.microsoft.com/documentation/videos/azure-friday/) ist ein sich wiederholendes Freitag mit Experten auf einer Vielzahl von Azure Themen video 1:1-Serie, der sich, kurze (10 bis 15 Minuten Interviews).  Verwenden der Services-Filter auf der Seite alle Azure Active Directory Videos anzeigen.

- [Azure Identität 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure Identität 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure Identität 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Soziale

- [Active Directory-Teamblog](http://blogs.technet.com/b/ad/): die neuesten Entwicklungen in der Welt von Azure Active Directory.

- [Azure Active Directory Graph-Teamblog](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory Informationen der Graph-API.

- [Cloud-Identität](http://www.cloudidentity.net): Gedanken Identity Management als Service von principal Azure Active Directory PM.  

- [Azure Active Directory auf Twitter](https://twitter.com/azuread): Azure Active Directory Ankündigungen in maximal 140 Zeichen.

## <a name="windows-server-on-premises-development"></a>Windows Server lokale Entwicklung
Mit der Entwicklung von Windows Server und Active Directory Federation Services (ADFS) Siehe:

- [AD FS Szenarien für Entwickler](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): Überblick AD FS-Komponenten und Informationen über unterstützte Authentifizierung/Autorisierung Szenarien funktioniert es.
- [AD FS Exemplarische Vorgehensweisen](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): eine Liste der Artikel schrittweise Anleitung zu zugehörigen Authentifizierung/Autorisierung Flows implementieren.
