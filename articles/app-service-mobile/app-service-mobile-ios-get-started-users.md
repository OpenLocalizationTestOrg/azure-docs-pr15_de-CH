<properties
    pageTitle="Authentifizierung auf iOS Azure Mobile Apps hinzufügen"
    description="Erfahren Sie, wie Azure Mobile Apps mit Benutzerauthentifizierung iOS-App mit verschiedenen Identitätsanbieter wie AAD, Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>IOS-app Authentifizierung hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Lernprogramm wird das [iOS quick start] Projekt unterstützten Identitätsanbieter Authentifizierung hinzufügen. In diesem Lernprogramm [iOS quick start] -Lernprogramm beruht auf zuerst ausführen müssen.

##<a name="register"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendung

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Drücken Sie in Xcode starten Sie die Anwendung **Ausführen** . Die Anwendung versucht, auf dem Back-End als nicht authentifizierter Benutzer, weil _TodoItem_ Tabelle benötigt nun Authentifizierung wird eine Ausnahme ausgelöst.

##<a name="add-authentication"></a>App Authentifizierung hinzufügen

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS-Schnellstart]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
