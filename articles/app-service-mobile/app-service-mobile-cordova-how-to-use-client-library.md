<properties
    pageTitle="Verwendung von Apache Cordova Plugin für Azure mobiler Apps"
    description="Verwendung von Apache Cordova Plugin für Azure mobiler Apps"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Verwendung von Apache Cordova-Clientbibliothek für Azure mobiler Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieses Handbuch erklärt Szenarien mit den neuesten [Apache Cordova Plugin für Azure Mobile Apps]ausführen. Sind Sie neu in Azure Mobile Apps, vollständige [Azure Mobile Apps Quick Start] eine Backend erstellt eine Tabelle erstellen und Herunterladen eines Apache Cordova vorgefertigte. In diesem Handbuch konzentrieren wir uns auf clientseitige Apache Cordova Plugin.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Apache Cordova v6.0.0 und später iOS, Android und Windows SDK unterstützt Geräte.  Plattform-Support lautet wie folgt:

* Android-API (KitKat durch Nougat) 19-24
* iOS-Version 8.0 und höher.
* Windows Phone 8.0
* Windows Phone 8.1
* Universelle Windows-Plattform

##<a name="Setup"></a>Setup und Komponenten

Dieses Handbuch setzt voraus, dass eine Back-End-Tabellen erstellt haben. Dieses Handbuch setzt voraus, dass die Tabelle das gleiche Schema wie die Tabellen in diesen Lernprogrammen. Dabei wird vorausgesetzt, dass Sie Code Apache Cordova Plugin hinzugefügt haben.  Wenn Sie dies nicht getan haben, können Sie die Apache-Cordova-Plug-in auf der Befehlszeile hinzufügen:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Weitere Informationen zum Erstellen [Ihrer ersten Apache Cordova Anwendung]finden Sie in ihrer Dokumentation.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Gewusst wie: Benutzer authentifizieren

Authentifizieren und Autorisieren von app Benutzer verschiedene externe Identitätsanbieter Azure App Service unterstützt: Facebook, Google, Microsoft Account und Twitter. Sie können Berechtigungen für Tabellen für bestimmte Operationen nur authentifizierten Benutzern Zugriff festlegen. Die Identität des authentifizierten Benutzern können Sie Autorisierungsregeln in Serverskripts implementieren. Weitere Informationen finden Sie im Lernprogramm [beginnen mit der Authentifizierung] .

Bei Verwendung der Authentifizierung in einer Apache Cordova müssen die folgenden Cordova-Plugins verfügbar sein:

* [Cordova-Plugin-Gerät]
* [Cordova-Plugin-inappbrowser]

Zwei Authentifizierung Abläufe werden unterstützt: einem Server und einem Client.  Fluss Server bietet die einfachste Authentifizierung wie der Provider Weboberfläche Authentifizierung abhängig. Der Client ermöglicht tiefere Integration gerätespezifische Funktionen wie Single Sign-On als anbieterspezifische gerätespezifische SDKs beruht.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Gewusst wie: Konfigurieren des Mobilfunkbereichs App für externe Umleitung URLs.

Mehrere Anwendungstypen Apache Cordova verwenden eine Loopback-Funktion OAuth UI Flows behandeln.  OAuth UI Datenflüsse auf Localhost Probleme seit der Authentifizierungsdienst nur Ihren Dienst standardmäßig verwenden kann.  Beispiele für problematische OAuth UI Abläufe:

- Wellen-Emulator.
- Leben Sie laden mit ionischen.
- Mobile Backend lokal ausgeführt
- Mobile Back-End-in einem anderen Azure App-Dienst als eine die Authentifizierung ausgeführt.

Die lokale Einstellung der Konfiguration hinzufügen, gehen Sie wie folgt vor:

1. [Azure-Portal] anmelden
2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen der Mobile-Anwendung.
3. Klicken Sie auf **Extras**
4. **Ressourcen-Explorer** im Menü "BEOBACHTEN" klicken Sie auf **wechseln**.  Öffnet ein neues Fenster oder Tab.
5. Erweitern Sie **Konfiguration**, **Authsettings** -Knoten für die Website in der linken Navigationsleiste.
6. Klicken Sie auf **Bearbeiten**
7. Suchen Sie das Element "AllowedExternalRedirectUrls".  Sie können Null oder ein Array von Werten festgelegt werden.  Ändern Sie den Wert auf den folgenden Wert:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    URLs mit den URLs Ihres Dienstes ersetzen.  Beispiele: "http://localhost: 3000" (für den Beispieldienst Node.js) oder "Http://localhost:4400" (für die Wellen-Dienst).  Diese URLs sind jedoch Beispiele - Situation für die Dienste, die in den Beispielen genannten abweichen.
8. Klicken Sie auf **Schreibgeschützt** , in der rechten Ecke des Bildschirms.
9. Klicken Sie auf die grüne Schaltfläche **PLATZIEREN** .

Nun werden gespeichert.  Schließen Sie das Browserfenster nicht, bis die Einstellungen wurden gespeichert.
Auch diese URLs Loopback an der CORS App Service hinzufügen:

1. [Azure-Portal] anmelden
2. **Alle Ressourcen** oder **Anwendungsdienste** klicken Sie auf den Namen der Mobile-Anwendung.
3. Settings-Blade wird automatisch geöffnet.  Wenn nicht, klicken Sie auf **Alle**.
4. Klicken Sie im Menü API auf **CORS** .
5. Geben Sie die URL in das Feld hinzuzufügende bereitgestellt und drücken Sie die EINGABETASTE.
6. Geben Sie ggf. zusätzliche URLs.
7. Klicken Sie auf **Speichern** , um die Einstellung zu speichern.

Es dauert ungefähr 10 bis 15 Sekunden für die neue Einstellung wirksam wird.

##<a name="register-for-push"></a>Gewusst wie: Registrieren für Pushbenachrichtigungen

Installieren Sie [Phonegap-Plugin-Push] Push-Benachrichtigung zu behandeln.  Dieses Plug-in kann problemlos mit hinzugefügt werden die `cordova plugin add` Befehl in der Befehlszeile oder über das Installationsprogramm Git-Plug-in in Visual Studio.  Der folgende Code in Ihrer Anwendung Apache Cordova registriert Geräts Push-Benachrichtigung:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Verwenden Sie Notification Hubs SDK Push-Benachrichtigung vom Server zu senden.  Senden Sie niemals Pushbenachrichtigungen direkt von Clients. Dies kann verwendet werden, ein Dienstverweigerungsangriff Benachrichtigungshubs oder PNS ausgelöst.  PNS konnte den Verkehr durch solche Angriffe blockieren.

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com
[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md
[Erste Schritte mit Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova Plugin für Azure mobiler Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[Ihre erste Apache Cordova Anwendung]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[PhoneGap-Plugin-push]: https://www.npmjs.com/package/phonegap-plugin-push
[Cordova-Plugin-Gerät]: https://www.npmjs.com/package/cordova-plugin-device
[Cordova-Plugin-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
