<properties
    pageTitle="Authentifizierung und Autorisierung in Azure mobiler Apps | Microsoft Azure"
    description="Grundlegende Referenz und Übersicht über die Authentifizierung / Autorisierung feature für Azure Mobile Apps"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Authentifizierung und Autorisierung in Azure mobiler Apps

## <a name="what-is-app-service-authentication--authorization"></a>Was ist App-Authentifizierung / Autorisierung?

> [AZURE.NOTE] In diesem Thema Migration zu einem konsolidierten [App-Authentifizierung / Autorisierung](../app-service/app-service-authentication-overview.md) Thema umfasst Web, Mobile und API-Apps.

App-Authentifizierung Autorisierung ist ein Feature, das Benutzer mit keine Änderungen auf dem app-Backend anmelden kann. Es bietet eine einfache Möglichkeit zu Ihrer Anwendung mit Daten arbeiten.

App Service verwendet Identitätsverbund 3rd Party **Identitätsprovider** ("IDP") speichert Konten und authentifiziert Benutzer und die Anwendung diese Identität anstelle ihrer eigenen. App-Dienst unterstützt fünf Identitätsanbieter aus Feld: _Azure Active Directory_, _Facebook_, _Google_, _Microsoft-Konto_und _Twitter_. Erweitern Sie diese Unterstützung, um Ihre Apps durch einen anderen Identitätsanbieter oder Ihre eigene benutzerdefinierte Identität-Lösung integrieren.

Ihre app kann eine beliebige Anzahl dieser Identitätsanbieter nutzen, damit Sie Ihre Endbenutzer, Optionen bereitstellen können für wie der Anmeldung.

Wenn Sie sofort beginnen möchten, finden Sie die folgenden Lernprogramme:

- [IOS-app Authentifizierung hinzufügen]
- [Authentifizierung Ihrer Anwendung Xamarin.iOS hinzufügen]
- [Authentifizierung Ihrer Anwendung Xamarin.Android hinzufügen]
- [Authentifizierung für Windows-Anwendung hinzufügen]

## <a name="how-authentication-works"></a>Funktionsweise der Authentifizierung

Die Authentifizierung mit einem Identitätsanbieter müssen Sie Identitätsanbieter wissen über die Anwendung konfigurieren. Der Identitätsanbieter wird Ihnen dann mit IDs und Kennwörter, die Sie für die Anwendung bereitstellen. Dies schließt die Vertrauensstellung und App Service bereitgestellte Identität überprüfen.

Diese Schritte sind in den folgenden Themen aufgeführt:

- [So konfigurieren Sie Ihre app Azure Active Directory-Benutzernamen verwenden]
- [So konfigurieren Sie Ihre Anwendung auf Facebook anmelden]
- [So konfigurieren Sie Ihre app Google anmelden]
- [So konfigurieren Sie die Anwendung Microsoft Account anmelden]
- [So konfigurieren Sie Ihre Anwendung auf Twitter Login verwenden]

Alles auf dem Back-End konfiguriert ist, können Sie Ihren Client anmelden ändern. Es gibt hier zwei Ansätze:

- Mit einer Codezeile, können Sie Mobile Apps Client SDK Benutzer anmelden.
- Nutzen Sie eine SDK von bestimmten Identitätsanbieter Identität und dann auf App-Dienst veröffentlicht.

>[AZURE.TIP] Meisten verwenden Anbieter SDK eine weitere systemeigene Gefühl Login Erfahrung und aktualisieren-Unterstützung und andere anbieterspezifische Vorteile nutzen.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Funktionsweise der Authentifizierung ohne einen Anbieter SDK

Wenn Sie keinen Anbieter SDK einrichten möchten, können Sie Mobile Apps für Sie die Anmeldung durchführen. Mobile Apps Client SDK eine Webansicht an den Anbieter Ihrer Wahl öffnen und die Anmeldung abzuschließen. Gelegentlich auf Blogs und Foren Sie dies als finden "Server Bewegung" oder "Server gerichtete Bewegung" seit die Anmeldung verwaltet und das Client-SDK nie anbietertoken empfängt.

Der Code für die diesem Datenstrom starten fällt im Lernprogramm Authentifizierung für jede Plattform. Am Ende des Flusses Client SDK ist einen App Service-Token und das Token wird automatisch an alle Anfragen an die Back-End.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Funktionsweise der Authentifizierung mit einem Anbieter SDK

Arbeiten mit einem Anbieter kann SDK anmelden Erfahrung enger mit der OS-Plattform interagieren, die Anwendung ausgeführt wird. Außerdem bietet Ihnen ein anbietertoken und einige Informationen auf dem Client erleichtert Graph APIs und Anpassen der Benutzeroberfläche. Gelegentlich in Blogs und Foren sehen Sie dies als "Client Bewegung" bezeichnet "Client gerichtete Bewegung" seit Code auf dem Client wird der Benutzername behandeln und Clientcode verfügt über ein anbietertoken.

Gewonnene anbietertoken muss App Service zur Überprüfung gesendet werden. Am Ende des Flusses Client SDK ist einen App Service-Token und das Token wird automatisch an alle Anfragen an die Back-End. Entwickler kann auch einen Verweis auf das anbietertoken beibehalten, wenn gewünscht.

## <a name="how-authorization-works"></a>Funktionsweise der Autorisierung

App-Authentifizierung / Autorisierung stellt mehrere Optionen für die **Aktion Anforderung nicht authentifiziert ist**. Bevor der Code eine bestimmte Anforderung erhält, haben Sie App Service überprüfen, ob die Anforderung authentifiziert und wenn nicht ablehnen und versuchen, den Benutzer anmelden, bevor Sie erneut versuchen.

Eine Möglichkeit ist nicht authentifizierte Anfragen umleiten auf einen Identitätsanbieter haben. In einem Webbrowser wäre dies tatsächlich den Benutzer zu einer neuen Seite. Allerdings Ihre mobile Client kann nicht auf diese Weise umgeleitet und nicht authentifizierte Antworten erhalten eine HTTP _401 Unauthorized_ Antwort. Sich sollte die erste Anforderung macht Ihr Client immer Login-Endpunkt, und dann können Sie Aufrufe von anderen APIs. Wenn Sie versuchen, eine andere API aufrufen, vor dem anmelden, wird Ihr Client eine Fehlermeldung.

Wollen Sie genauer steuern, über welche Endpunkte Authentifizierung erforderlich ist, wählen Sie auch "keine Aktion (Anforderung zulassen)" für nicht authentifizierte Anfragen. In diesem Fall werden alle Beschlüsse der Authentifizierung für den Anwendungscode zurückgestellt. Dadurch können Sie den Zugriff auf bestimmte Benutzer benutzerdefinierte Autorisierungsregeln.

## <a name="documentation"></a>Dokumentation

Die folgenden Lernprogramme anzeigen wie Ihre mobile Clients mithilfe von App Service Authentifizierung hinzugefügt:

- [IOS-app Authentifizierung hinzufügen]
- [Authentifizierung Ihrer Anwendung Xamarin.iOS hinzufügen]
- [Authentifizierung Ihrer Anwendung Xamarin.Android hinzufügen]
- [Authentifizierung für Windows-Anwendung hinzufügen]

Die folgenden Lernprogramme anzeigen App Service nutzen verschiedene Authentifizierungsanbieter konfigurieren:

- [So konfigurieren Sie Ihre app Azure Active Directory-Benutzernamen verwenden]
- [So konfigurieren Sie Ihre Anwendung auf Facebook anmelden]
- [So konfigurieren Sie Ihre app Google anmelden]
- [So konfigurieren Sie die Anwendung Microsoft Account anmelden]
- [So konfigurieren Sie Ihre Anwendung auf Twitter Login verwenden]

Wenn eine Identitätssystem als den hier verwenden möchten, können Sie auch [benutzerdefinierte Authentifizierung unterstützt .NET Server SDK Vorschau](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth)nutzen.

[IOS-app Authentifizierung hinzufügen]: app-service-mobile-ios-get-started-users.md
[Authentifizierung Ihrer Anwendung Xamarin.iOS hinzufügen]: app-service-mobile-xamarin-ios-get-started-users.md
[Authentifizierung Ihrer Anwendung Xamarin.Android hinzufügen]: app-service-mobile-xamarin-android-get-started-users.md
[Authentifizierung für Windows-Anwendung hinzufügen]: app-service-mobile-windows-store-dotnet-get-started-users.md

[So konfigurieren Sie Ihre app Azure Active Directory-Benutzernamen verwenden]: app-service-mobile-how-to-configure-active-directory-authentication.md
[So konfigurieren Sie Ihre Anwendung auf Facebook anmelden]: app-service-mobile-how-to-configure-facebook-authentication.md
[So konfigurieren Sie Ihre app Google anmelden]: app-service-mobile-how-to-configure-google-authentication.md
[So konfigurieren Sie die Anwendung Microsoft Account anmelden]: app-service-mobile-how-to-configure-microsoft-authentication.md
[So konfigurieren Sie Ihre Anwendung auf Twitter Login verwenden]: app-service-mobile-how-to-configure-twitter-authentication.md
