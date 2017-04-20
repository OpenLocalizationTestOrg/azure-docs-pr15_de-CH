<properties
    pageTitle="Authentifizierung auf Apache Cordova Mobile Apps hinzufügen | Azure App Service"
    description="Erfahren Sie mit Mobile Apps in Azure App Service zum Authentifizieren von Benutzern der Anwendung Apache Cordova mithilfe verschiedener Identitätsanbieter wie Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Apache Cordova app Authentifizierung hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wird der Aufgabenliste Quickstart Projekt unterstützten Identitätsanbieter mit Apache Cordova Authentifizierung hinzufügen. In diesem Lernprogramm Lernprogramm [Erste Schritte mit Mobile Apps] beruht auf zuerst ausführen müssen.

##<a name="register"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendung

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[In diesem Video zeigt ähnliche Schritte](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nun können Sie überprüfen, dass anonymer Zugriff auf Ihre Back-End-deaktiviert wurde. Öffnen Sie das Projekt erstellt, wenn Sie das Lernprogramm [Erste Schritte mit Mobile Apps], abgeschlossen in Visual Studio führen Sie die Anwendung im **Google Android-Emulator** und überprüfen Sie, ob ein unerwarteter Fehler bei Verbindung angezeigt wird, nachdem die Anwendung gestartet wird.

Danach aktualisieren Sie zum Authentifizieren von Benutzern vor dem Anfordern von Ressourcen von Mobile App Back-End-Anwendung.

##<a name="add-authentication"></a>Authentifizierung für die Anwendung hinzufügen

1. Öffnen Sie das Projekt in **Visual Studio**, und öffnen Sie die `www/index.html` Datei zur Bearbeitung.

2. Suchen Sie die `Content-Security-Policy` Meta-Tag im Head-Bereich.  Sie müssen OAuth Host zur Liste der zulässigen Quellen hinzufügen.

  	| Anbieter               | SDK-Anbietername | OAuth-Host                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | AAD               | https://Login.Windows.NET   |
  	| Facebook               | Facebook          | https://www.Facebook.com    |
  	| Google                 | Google            | https://Accounts.Google.com |
  	| Microsoft              | MicrosoftAccount  | https://Login.Live.com      |
  	| Twitter                | Twitter           | https://API.twitter.com     |

    Ein Beispiel für Content-Security-Policy (Azure Active Directory implementiert) lautet wie folgt:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Ersetzen Sie `https://login.windows.net` mit OAuth Host aus der oben stehenden Tabelle.  Weitere Informationen über diese Meta-Tag finden Sie in [Inhalt Sicherheitsrichtlinie Dokumentation] .

    Beachten Sie, dass einige Authentifizierungsanbieter Inhalt Sicherheitsrichtlinie Änderungen auf entsprechende Mobilgeräte nicht benötigen.  Beispielsweise müssen keine Inhalte Sicherheitsrichtlinie ändert bei Google-Authentifizierung auf einem Android-Gerät.

3. Öffnen der `www/js/index.js` Datei bearbeiten, suchen Sie die `onDeviceReady()` -Methode und unter Erstellung Client folgenden Code hinzufügen:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Beachten Sie, dass dieser Code den Code ersetzen, der die Referenz erstellt und die Benutzeroberfläche aktualisiert.

    Die login()-Methode startet Authentifizierung mit dem Anbieter. Login()-Methode ist ein asynchroner Funktion, die JavaScript Promise zurückgibt.  Der Rest der Initialisierung wird in Versprechen Reaktion platziert, damit es nicht ausgeführt wird, bis die login() Methode.

4. Ersetzen Sie in dem soeben hinzugefügten Code `SDK_Provider_Name` mit dem Namen des Anbieters anmelden. Azure Active Directory, z. B. `client.login('aad')`.

4. Führen Sie das Projekt.  Nach Abschluss des Projekts initialisieren zeigen die Anwendung für den ausgewählten Authentifizierungsanbieter OAuth-Anmeldeseite.

##<a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr [Über Authentifizierung] mit Azure App Service.
* Fortsetzen des Lernprogramms Apache Cordova app [Pushbenachrichtigungen] hinzufügen.

Dazu verwenden Sie die SDKs.

* [Apache Cordova SDK]
* [ASP.NET Server SDK]
* [Node.js Server SDK]

<!-- URLs. -->
[Erste Schritte mit Mobile Apps]: app-service-mobile-cordova-get-started.md
[Inhalt der Sicherheitsrichtlinie Dokumentation]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Push-Benachrichtigung]: app-service-mobile-cordova-get-started-push.md
[Informationen zur Authentifizierung]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
