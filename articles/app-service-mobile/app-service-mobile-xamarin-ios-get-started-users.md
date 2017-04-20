<properties
    pageTitle="Erste Schritte mit Authentifizierung für Mobile Apps in Xamarin iOS"
    description="Erfahren Sie, wie Mobile Apps mit Benutzerauthentifizierung die Xamarin iOS-App mit verschiedenen Identitätsanbieter wie AAD, Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Authentifizierung Ihrer Anwendung Xamarin.iOS hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema veranschaulicht die Authentifizierung von Benutzern einer Anwendung Service Mobile App in der Clientanwendung. In diesem Lernprogramm wird das Xamarin.iOS Quickstart Projekt Identitätsanbieter App Service unterstützt Authentifizierung hinzufügen. Authentifiziert und autorisiert die Mobile-App erfolgreich, wird der Benutzer-ID-Wert und werden auf eingeschränkte Daten.

Sie müssen zuerst das Lernprogramm [Xamarin.iOS app erstellen]. Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie das Erweiterung Authentifizierungspaket zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendungsdienste

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. in Visual Studio oder Xamarin Studio führen Sie das Clientprojekt auf einem Gerät oder Emulator. Stellen Sie sicher, dass eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird. Der Fehler wird in der Konsole des Debuggers protokolliert. Daher sollte in Visual Studio den Fehler im Ausgabefenster angezeigt werden.

&nbsp;&nbsp;Nicht autorisierte Fehler geschieht, da die Anwendung versucht, Mobile App Backend als nicht authentifizierter Benutzer zugreifen. Die Tabelle *TodoItem* benötigt nun Authentifizierung.

Danach aktualisieren Sie die Clientanwendung auf Anforderung Ressourcen vom Back-End Mobile-Anwendung mit einem authentifizierten Benutzer.

##<a name="add-authentication-to-the-app"></a>Authentifizierung für die Anwendung hinzufügen

In diesem Abschnitt ändern Sie die app Login-Bildschirm vor dem Anzeigen von Daten. Beim Starten der Anwendung wird nicht mit Ihren App-Dienst nicht verbunden und keine Daten angezeigt. Nach der erstmaligen führt der Benutzer die Aktion Aktualisieren der Anmeldebildschirm erscheint; nach erfolgreicher Anmeldung wird die Liste der Aufgaben angezeigt.

1. Im Clientprojekt, öffnen Sie die Datei **QSTodoService.cs** , und fügen Sie die folgende Anweisung verwenden und `MobileServiceUser` mit Accessor für die QSTodoService-Klasse:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Fügen Sie neue Methode namens **authentifizieren** , **QSTodoService** mit folgender Definition:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Wenn ein Identitätsprovider als Facebook ändern **LoginAsync** , eine der folgenden übergebenen Wert: _MicrosoftAccount_, _Twitter_, _Google_oder _WindowsAzureActiveDirectory_.

3. Öffnen Sie **QSTodoListViewController.cs**. Ändern Sie die Methodendefinition **Spiele** den Aufruf von **RefreshAsync()** Ende entfernen:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Ändern Sie die Methode **RefreshAsync** authentifizieren, wenn die **User** -Eigenschaft null ist. Fügen Sie den folgenden Code am Anfang der Methodendefinition:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. In Visual Studio oder Xamarin Studio verbunden Xamarin Erstellen von Host auf Ihrem Mac ausführen auf einem Gerät oder Emulator Clientprojekt. Stellen Sie sicher, dass die Anwendung keine Daten angezeigt.

    Dieser Bewegung aktualisieren ziehen Sie in der Liste der Elemente, die den Anmeldebildschirm angezeigt wird. Sobald Sie erfolgreich gültige Anmeldeinformationen eingegeben haben, zeigt die Anwendung die Liste der Aufgaben und können Sie Updates auf die Daten.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Erstellen einer Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md
