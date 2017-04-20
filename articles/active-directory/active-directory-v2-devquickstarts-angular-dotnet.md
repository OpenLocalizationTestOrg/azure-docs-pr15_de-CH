<properties
    pageTitle="Azure AD v2. 0 AngularJS erste Schritte | Microsoft Azure"
    description="Wie eine Winkel JS Seite Anwendung erstellen, die Benutzer mit Konten sowohl persönlichen Microsoft signiert und Geschäfts-oder."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>Hinzufügen einer AngularJS Seite app - .NET anmelden

In diesem Artikel fügen wir Azure Active Directory v2. 0-Endpunkt mit AngularJS App mit Microsoft powered Konten anmelden.  Endpunkt v2. 0 ermöglicht eine Integration in Ihre Anwendung und Benutzerauthentifizierung mit persönlichen und Arbeit oder Schule.

Dieses Beispiel ist eine einfache Aufgabenliste Seite Anwendung, in der Aufgaben im Back-End-REST-API mit .NET 4.5 MVC-Frameworks geschrieben und gesicherte OAuth trägertoken von Azure AD gespeichert.  AngularJS-app verwenden unsere Opensource JavaScript Authentifizierung Bibliothek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) behandelt die gesamte Anmeldung und Token für die REST-API aufrufen.  Dasselbe Muster kann angewendet werden, um andere REST-APIs wie [Microsoft Graph](https://graph.microsoft.com)zu authentifizieren.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="download"></a>Herunterladen

Zunächst müssen Sie herunterladen und Installieren von Visual Studio.  Anschließend klonen können oder Skelett app [herunterzuladen](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) :

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

Skelette app enthält den Standardcode für eine einfache AngularJS app aber alle identitätsbezogene fehlen.  Wenn Sie nachvollziehen möchten, können Sie stattdessen Klonen oder vollständiges Beispiel [herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) .

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Registrieren einer Anwendung

Zunächst erstellen Sie eine Anwendung im [App Registrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)oder befolgen Sie diese [Anleitung](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- **Web** Plattform für Ihre Anwendung hinzufügen.
- Geben Sie den entsprechenden **URI umleiten**. Der Standardwert für dieses Beispiel ist `https://localhost:44326/`.
- Lassen Sie das Kontrollkästchen **Zulassen implizite fließen** aktiviert. 

Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist, Sie benötigen diese kurz. 

## <a name="install-adaljs"></a>Adal.js installieren
Zum Starten von Ihnen heruntergeladenen Projekt navigieren und adal.js installieren.  [Bower](http://bower.io/) installiert haben, führen Sie einfach diesen Befehl.  Wählen Sie für jede Abhängigkeit Versionskonflikte einfach die höhere Version.
```
bower install adal-angular#experimental
```

Alternativ können Sie [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) und [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js)manuell herunterladen.  Fügen Sie beide Dateien, die `app/lib/adal-angular-experimental/dist` Verzeichnis von der `TodoSPA` Projekt.

Nun öffnen Sie das Projekt in Visual Studio und Laden Sie adal.js am Ende des Haupttextes:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Richten Sie die REST-API

Während wir Dinge einrichten, erfahren Sie im Back-End-REST-API arbeiten.  Öffnen Sie im Stammverzeichnis des Projekts `web.config` und die `audience` Wert.  Die REST-API verwendet diesen Wert Token aus Winkel app AJAX Anfragen zu bestätigen.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>
    
...
```

Das ist die Zeit, die wir wie die REST-API gesagt werden.  Gerne im Code anzeigen, aber wenn Sie weitere Informationen zum Sichern von Web-APIs in Azure AD überprüfen [diesen](active-directory-v2-devquickstarts-dotnet-api.md)Artikel. 

## <a name="sign-users-in"></a>Benutzer anmelden
Zeit Identitätscode schreiben.  Sie können bereits bemerkt haben, adal.js AngularJS-Anbieter enthält, die gut mit Winkel Routingmechanismen spielt.  Starten Sie die Anwendung adal Modul hinzufügen:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Sie können jetzt initialisieren die `adalProvider` mit der ID der Anwendung:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Besitzt jetzt adal.js die Informationen zum Sichern Ihrer app und Benutzer anmelden muss.  Anmelden für einen bestimmten Arbeitsplan in die Anwendung erzwingen, alles ist eine Codezeile:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Wenn nun ein Benutzer klickt der `TodoList` Link adal.js automatisch umleitet Azure AD-Anmeldung bei Bedarf.  Sie können auch explizit anmelden und Abmelden Anfragen durch den Aufruf von adal.js in Ihre Domänencontroller:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Anzeigen von Benutzerinformationen
Da der Benutzer angemeldet ist, müssen Sie wahrscheinlich Authentifizierungsdaten des angemeldeten Benutzers in Ihrer Anwendung zugreifen.  Adal.js macht diese Informationen für Sie in der `userInfo` Objekt.  Zugriff auf dieses Objekt in einer Ansicht zuerst fügen Sie adal.js zum Stammbereich entsprechenden Controller hinzu:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Dann direkt adressieren können die `userInfo` Objekt in der Ansicht: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Sie können auch die `userInfo` Objekt, um festzustellen, ob der Benutzer oder nicht angemeldet ist.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Rufen Sie die REST-API
Schließlich ist es Zeit, einige Token und rufen Sie die REST-API zum Erstellen, lesen, aktualisieren und Löschen von Aufgaben.  Tja, was?  Sie müssen nicht *tun*.  Adal.js kümmern automatisch, Zwischenspeichern und Aktualisieren von Token.  Es wird auch kümmern ausgehenden AJAX-Anfragen, die an die REST-API diese Tokens zuordnen.  

Funktioniert wie das? Ist Magic [AngularJS Interceptors](https://docs.angularjs.org/api/ng/service/$http), adal.js zum Transformieren von eingehenden und ausgehenden HTTP-Nachrichten ermöglicht.  Außerdem geht adal.js Anfragen zu derselben Domäne senden Fenster Token zur derselben Anwendung ID AngularJS App sollte.  Deshalb verwendet die gleichen-ID in der Winkel app und NodeJS REST-API.  Natürlich können Sie dieses Verhalten überschreiben und adal.js zu Token für andere REST-APIs bei Bedarf - für dieses einfache Szenario die Standardeinstellungen ist jedoch der Fall.

Hier ist ein Ausschnitt, der zeigt, wie einfach mit trägertoken von Azure AD senden:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Herzlichen Glückwunsch!  Ihre Azure AD integrierte Seite Anwendung ist jetzt abgeschlossen.  Los, nehmen Sie einen Bogen.  Benutzer authentifizieren kann, sicher Aufrufen der Back-End-REST-API mit OpenID herstellen und grundlegende Informationen über den Benutzer.  Standardmäßig unterstützt alle Benutzer mit persönlichen Microsoft-Account oder ein Konto Arbeit-Schule von Azure AD.  Führen Sie die Anwendung, und navigieren Sie im Browser zu `https://localhost:44326/`.  Mit persönlichen Microsoft-Konto oder eine Arbeit-Schule Konto anmelden.  Aufgabenliste des Benutzers Aufgaben hinzu, und melden Sie sich ab.  Versuchen Sie, die andere Art von Konto anzumelden. Benötigen Sie einen Azure AD-Mandanten arbeiten Schule Benutzer erstellen [erfahren Sie, wie man hier](active-directory-howto-tenant.md) (kostenfrei).

Weiter Kennenlernen der v2. 0-Endpunkt zurück unsere [v2. 0-Entwicklerhandbuch](active-directory-appmodel-v2-overview.md).  Weitere Ressourcen zum Auschecken:

- [Azure-Beispielen auf GitHub >>](https://github.com/Azure-Samples)
- [Azure AD Stapelüberlauf >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD Dokumentation [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
