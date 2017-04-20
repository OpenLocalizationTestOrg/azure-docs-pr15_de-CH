<properties
    pageTitle="Azure AD AngularJS erste Schritte | Microsoft Azure"
    description="Wie eine Winkel JS Seite Anwendung erstellen, die Azure AD Anmelden integriert und ruft Azure AD, geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>AngularJS einzelne Seite Apps mit Azure sichern

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD vereinfacht erleichert Add-in anmelden, Abmelden und OAuth-API-Aufrufe Ihrer Seite apps.  Sie können Ihre Anwendung zur Benutzerauthentifizierung mit ihren Active Directory-Konten und Web API geschützt von Azure AD Azure-API oder die Office 365-APIs verwenden.

Javascript-Anwendung in einem Browser ausgeführt bietet Azure AD Active Directory Authentifizierungsbibliothek oder adal.js.  Zweck des Adal.js im Leben ist Ihre app Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier einer Anwendung AngularJS Liste, die wir:

- Meldet den Benutzer in der app mit Azure AD als Identitätsanbieter.
- Zeigt Informationen über den Benutzer.
- Sicher API aufgerufen, der app zu tun Liste trägertoken aus AAD verwenden.
- Meldet den Benutzer von der app.

Um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihre Anwendung in Azure AD.
3. ADAL installieren und Konfigurieren von SPA.
5. Verwenden Sie ADAL, um Seiten im SPA.

Um zu beginnen, [das Skelett app](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip)oder vollständiges Beispiel.  Sie benötigen auch einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

## <a name="1-register-the-directorysearcher-application"></a>*1. registrieren Sie die DirectorySearcher-Anwendung*
Um Ihre app Benutzerauthentifizierung und Token zu aktivieren, müssen Sie zuerst registrieren in Azure AD-Mandanten:

-   Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten in der Anwendung registriert.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen Sie, und erstellen Sie eine neue **Anwendung oder WebAPI**.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   Der **Uri umleiten** befindet, AAD Token zurück.  Der Standardspeicherort für dieses Beispiel ist`https://localhost:44326/`
-   Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige **Client-ID**.  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Adal.js verwendet Kommunikation mit Azure AD impliziten OAuth-Fluss.  Sie müssen den impliziten Fluss für die Anwendung:
    - Herunterladen des Anwendungsmanifests **Manifest verwalten**.
    - Das Manifest zu öffnen, und suchen Sie die `oauth2AllowImplicitFlow` Eigenschaft. Wert `true`.
    - Speichern Sie und Hochladen Sie Anwendungsmanifest erneut auf **Manifest verwalten** .

## <a name="2-install-adal--configure-the-spa"></a>*2 ADAL installieren und Konfigurieren von SPA*
Jetzt haben Sie eine Anwendung in Azure AD können Sie adal.js installieren und identitätsbezogene Code schreiben.

-   Zunächst adal.js TodoSPA Projekt mithilfe der Paket-Manager-Konsole:
  - [Adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) herunterladen und fügen sie die `App/Scripts/` Projektverzeichnis.
  - [Adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) herunterladen und fügen sie die `App/Scripts/` Projektverzeichnis.
  - Laden Sie jedes Skript vor dem Ende der `</body>` in `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Für SPA Back-End zu tun Liste API Token vom Browser akzeptiert benötigt der Back-End-Konfigurationsinformationen über die app-Registrierung. Öffnen Sie im Projekt TodoSPA `web.config`.  Ersetzen Sie die Werte der Elemente in der `<appSettings>` zu den Werten in der Azure-Portal eingegeben.  Code verweist diese Werte, wenn ADAL verwendet.
    -   Die `ida:Tenant` ist die Domäne der Azure AD-Mandanten, z.B. contoso.onmicrosoft.com
    -   Die `ida:Audience` muss die **Client-ID** der Anwendung kopiert vom Portal aus.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. Verwenden von ADAL auf Seiten im SPA*
Adal.js baut Integration mit AngularJS Arbeitsplan und http, wodurch Sie einzelne Ansichten SPA sichern.

- In `App/Scripts/app.js`, im Modul adal.js bringen:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Können jetzt initialisieren die `adalProvider` mit der Konfiguration Ihrer Anwendung Registrierung in `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Jetzt zum Sichern der `TodoList` anzeigen in der Anwendung nur eine Zeile Code - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Sie haben jetzt eine sichere Einzelseite Anwendung die Möglichkeit, Benutzer anmelden und Träger token geschützte Anfragen an die Back-End-API.  Wenn ein Benutzer klickt der `TodoList` Link adal.js automatisch umleitet Azure AD Anmelden bei Bedarf.  Darüber hinaus werden adal.js automatisch ein Zugriffstoken Ajax Anfragen zuordnen, die Back-End der Anwendung an.  Ist das notwendige minimum zu SPA mit adal.js -, aber es gibt eine Reihe von anderen Funktionen SPA nützlich sind:

- Explizit Problem anmelden und Abmelden Anfragen können Sie Funktionen in Ihre definieren, die adal.js aufrufen.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Sie sollten außerdem Benutzerinformationen in der app-Benutzeroberfläche vorhanden.  Adal Service bereits hinzugefügt wurde das `userDataCtrl` Controller zugreifen zu können die `userInfo` Objekt in der zugeordneten Ansicht `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Es gibt auch viele Szenarios, in denen möchten wissen, ob der Benutzer oder nicht angemeldet ist.  Sie können auch die `userInfo` Objekt diese Informationen.  Beispielsweise `index.html` "Login" oder "Logout" auf der Grundlage des Authentifizierungsstatus anzeigen:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Herzlichen Glückwunsch! Azure AD integriert einzelne Seite Anwendung ist jetzt abgeschlossen.  Benutzer authentifizieren kann, sicher Aufrufen der Back-End-OAuth 2.0 verwenden und grundlegende Informationen über den Benutzer.  Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt.  Führen Sie zu tun Liste SPA und dieser Benutzer anmelden.  Hinzufügen von Tasks für die Benutzer auflisten, Abmelden und wieder anmelden.

Adal.js erleichtert diese gemeinsamen Identität Features in Ihre Anwendung integrieren.  Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Sie können zusätzliche Szenarien nun.  Möglicherweise möchten versuchen:

[Rufen Sie CORS Web-API aus einer >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
