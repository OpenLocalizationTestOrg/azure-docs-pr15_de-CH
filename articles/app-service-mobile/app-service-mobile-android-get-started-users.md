<properties
    pageTitle="Authentifizierung auf Android Mobile Apps hinzufügen | Azure App Service"
    description="Erfahren Sie, wie Mobile Apps in Azure App Service mit Ihrem Android mithilfe verschiedener Identitätsanbieter wie Google, Facebook, Twitter und Microsoft Benutzerauthentifizierung."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Ihre Android Authentifizierung hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wird dem Projekt Aufgabenliste Schnelleinstieg Android mit unterstützten Identitätsanbieter Authentifizierung hinzufügen. In diesem Lernprogramm Lernprogramm [Erste Schritte mit Mobile Apps] beruht auf zuerst ausführen müssen.

##<a name="register"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendung

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Öffnen Sie in Android Studio geplanten abgeschlossenen Projekts mit dem Lernprogramm [beginnen mit Mobile Apps]. Im Menü **Ausführen** auf **Anwendung ausführen** und überprüfen Sie, ob eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird.

     Diese Ausnahme passiert die Anwendung auf dem Back-End als nicht authentifizierter Benutzer versucht, weil die Tabelle _TodoItem_ benötigt nun Authentifizierung.

Danach aktualisieren Sie zum Authentifizieren von Benutzern vor dem Anfordern von Ressourcen von Mobile App Back-End-Anwendung.

## <a name="add-authentication-to-the-app"></a>Authentifizierung für die Anwendung hinzufügen

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Authentifizierungstokens Cache auf dem client

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Nächste Schritte

Abschluss dieser praktischen Einführung Standardauthentifizierung sollten Sie auf eine der folgenden Lernprogramme:

+ [Push-Benachrichtigung zu Ihrem Android hinzufügen](app-service-mobile-android-get-started-push.md) Informationen Sie zum Konfigurieren von Mobile-Anwendung Backend um Azure Notification Hubs Pushbenachrichtigungen zu senden.

+ [Offline-Synchronisierung für die Android aktivieren](app-service-mobile-android-get-started-offline-data.md) Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Benutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Erste Schritte mit Mobile Apps]: app-service-mobile-android-get-started.md
