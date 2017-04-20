<properties
    pageTitle="Verwendung von JavaScript-SDK für Azure mobiler Apps"
    description="Verwendung V Azure Mobile Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Verwendung die JavaScript-Clientbibliothek für Azure mobiler Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieses Handbuch erklärt Szenarien mit dem neuesten [JavaScript SDK für Azure Mobile Apps]ausführen. Wenn Sie neue Azure Mobile Apps sind, zuerst führen Sie [Azure Mobile Apps Quick Start] , um ein Back-End- und Erstellen einer Tabelle. In diesem Handbuch konzentrieren wir uns auf mobilen Backend in HTML/JavaScript Web Applications.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Wir beschränken Browserunterstützung für aktuellen und vergangenen Versionen von gängigen Browsern: Google Chrome, Microsoft Edge, Microsoft Internet Explorer und Mozilla Firefox.  Wir erwarten, dass das SDK mit relativ moderne Browser.

Verteilung des Pakets wird als universelle JavaScript-Modul unterstützt Global, AMD und CommonJS formatiert.

##<a name="Setup"></a>Setup und Komponenten

Dieses Handbuch setzt voraus, dass eine Back-End-Tabellen erstellt haben. Dieses Handbuch setzt voraus, dass die Tabelle das gleiche Schema wie die Tabellen in diesen Lernprogrammen.

Installieren von Azure Mobile Apps JavaScript SDK erfolgen über den `npm` Befehl:

```
npm install azure-mobile-apps-client --save
```

Nach der Installation befindet sich die Bibliothek im `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Kopieren Sie diese Datei in Ihrem Web-Bereich.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Die Bibliothek kann auch als ein ES2015 Modul in CommonJS Umgebung wie Browserify und Webpaketdatei und AMD-Bibliothek verwendet.  Zum Beispiel:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Gewusst wie: Benutzer authentifizieren

Authentifizieren und Autorisieren von app Benutzer verschiedene externe Identitätsanbieter Azure App Service unterstützt: Facebook, Google, Microsoft Account und Twitter. Sie können Berechtigungen für Tabellen für bestimmte Operationen nur authentifizierten Benutzern Zugriff festlegen. Die Identität des authentifizierten Benutzern können Sie Autorisierungsregeln in Serverskripts implementieren. Weitere Informationen finden Sie im Lernprogramm [beginnen mit der Authentifizierung] .

Zwei Authentifizierung Abläufe werden unterstützt: einem Server und einem Client.  Fluss Server bietet die einfachste Authentifizierung wie der Provider Weboberfläche Authentifizierung abhängig. Der Client ermöglicht tiefere Integration gerätespezifische Funktionen wie Single Sign-On als anbieterspezifische SDKs beruht.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Gewusst wie: Konfigurieren des Mobilfunkbereichs App für externe Umleitung URLs.

Mehrere Anwendungstypen JavaScript verwenden eine Loopback-Funktion OAuth UI Flows behandeln.  Diese Funktionen umfassen:

* Den Dienst lokal ausgeführt
* Verwenden Live Reload ionischen Framework
* Umleiten von App Service für die Authentifizierung. 

Lokal kann Probleme verursachen, da standardmäßig App-Authentifizierung nur konfiguriert ist Backend Mobile-Anwendung zugreifen. Gehen Sie folgendermaßen vor, Ändern der App Service um Authentifizierung zu aktivieren, wenn der Server lokal ausgeführt:

1. [Azure-Portal] anmelden
2. Navigieren Sie zum Backend Mobile-Anwendung.
3. Wählen Sie **Ressource Explorer** im **ENTWICKLUNGSTOOLS** .
4. Klicken Sie in einer neuen Registerkarte oder Fenster öffnen für die Mobile Anwendung Back-End-Ressourcen-Explorer **wechseln** .
5. Erweitern Sie **Konfiguration** > **Authsettings** Knoten für Ihre Anwendung.
6. Klicken Sie auf die Schaltfläche **Bearbeiten** , um die Ressource bearbeiten aktivieren.
7. Suchen Sie das **AllowedExternalRedirectUrls** -Element, das null sein soll. Fügen Sie die URLs in einem Array:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Ersetzen von URLs im Array mit den URLs der Service in diesem Beispiel `http://localhost:3000` für die Dienstleistung Node.js-Beispiel. Sie können `http://localhost:4400` für Welle Service oder anderen URL je nach Ihrer Anwendung Konfiguration.

8. Klicken Sie oben auf der Seite **Schreibgeschützt**klicken **setzen** , um Ihre Updates speichern.

Sie müssen dieselben Loopback-URLs auf den weißen CORS hinzufügen:

1. Navigieren Sie zu der [Azure-Portal].
2. Navigieren Sie zum Backend Mobile-Anwendung.
3. Klicken Sie im Menü **API** auf **CORS** .
4. Geben Sie jede URL im Textfeld leer **Ursprünge zulässig** .  Ein neues Textfeld erstellt.
5. Klicken Sie auf **Speichern**
    
Nach der Aktualisierung der Back-End-werden neue Loopback-URLs in Ihrer Anwendung verwenden.

<!-- URLs. -->
[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Erste Schritte mit Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure-portal]: https://portal.azure.com/
[JavaScript-SDK für Azure mobiler Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

