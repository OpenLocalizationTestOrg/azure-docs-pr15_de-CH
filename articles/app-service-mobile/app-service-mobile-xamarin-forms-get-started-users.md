<properties
    pageTitle="Erste Schritte mit Authentifizierung für Mobile Apps in Xamarin.Forms app | Microsoft Azure"
    description="Erfahren Sie, wie Mobile Apps zum Authentifizieren von Benutzern der Anwendung Xamarin Forms mithilfe verschiedener Identitätsanbieter wie AAD, Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Authentifizierung Ihrer Anwendung Xamarin.Forms hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Übersicht

In diesem Thema veranschaulicht die Authentifizierung von Benutzern einer Anwendung Service Mobile App in der Clientanwendung. In diesem Lernprogramm wird das Xamarin.Forms Quickstart Projekt Identitätsanbieter App Service unterstützt Authentifizierung hinzufügen. Authentifiziert und autorisiert die Mobile-App erfolgreich, wird der Benutzer-ID-Wert angezeigt, und Sie werden auf eingeschränkte Daten zugreifen.

##<a name="prerequisites"></a>Erforderliche Komponenten

Das beste Ergebnis in diesem Lernprogramm wird empfohlen, des Lernprogramms [Xamarin.Forms app erstellen ausführen](app-service-mobile-xamarin-forms-get-started.md) . Nach Abschluss des Lernprogramms haben Sie ein Xamarin.Forms Projekt, das eine Multiplattform Aufgabenliste App.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, müssen Sie das Erweiterung Authentifizierungspaket zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendungsdienste

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Portable Klassenbibliothek Authentifizierung hinzufügen

Mobile Apps verwendet die Erweiterungsmethode [LoginAsync] auf [MobileServiceClient] Benutzer mit App-Authentifizierung anmelden. Dieses Beispiel verwendet ein Authentifizierungsablauf Server verwaltet, die der Provider-Schnittstelle in der Anwendung angezeigt. Weitere Informationen finden Sie unter [Authentifizierung Server verwaltet](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Um Benutzerfunktionalität in Ihrer produktionsanwendung bereitstellen, sollten Sie stattdessen mithilfe der [verwalteten Client-Authentifizierung](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Die Authentifizierung mit einem Xamarin.Forms Projekt definieren Sie eine Schnittstelle **"IAuthenticate"** in Portable Klassenbibliothek für die Anwendung. Sie aktualisieren Portable Klassenbibliothek eine Schaltfläche **Anmelden** das Hinzufügen der Benutzer zu Authentifizierung definierte Benutzeroberfläche. Nach erfolgreicher Authentifizierung werden Daten aus Backend mobile Anwendung geladen.

Implementieren Sie die Schnittstelle **"IAuthenticate"** für jede Plattform, die von der Anwendung unterstützt.


1. In Visual Studio oder Xamarin Studio öffnen App.cs aus dem Projekt mit **tragbaren** im Namen Portable Klassenbibliothek-Projekt ist, fügen Sie die folgenden `using` Anweisung:

        using System.Threading.Tasks;

2. Fügen Sie folgende App.cs, `IAuthenticate` Schnittstellendefinition unmittelbar vor der `App` Klassendefinition.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Der **App** -Klasse, die Schnittstelle mit einer bestimmten Plattform-Implementierung nicht initialisieren fügen Sie statischen Member hinzu.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. TodoList.xaml Portable Klassenbibliothek-Projekt öffnen, **Button** -Element im Layout-Element *ButtonsPanel* nach der vorhandenen Schaltfläche hinzufügen: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Diese Schaltfläche löst eine Authentifizierung mit der mobilen Anwendung Back-End-Server verwaltet.

5. TodoList.xaml.cs Portable Klassenbibliothek-Projekt öffnen, und fügen Sie das folgende Feld, das `TodoList` Klasse:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Ersetzen Sie die **OnAppearing** -Methode durch folgenden Code:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Dadurch wird sichergestellt, dass Daten nur vom Dienst aktualisiert werden, nachdem der Benutzer authentifiziert wurde.

7. **Aufgabenliste** -Klasse den folgenden Ereignishandler für das Ereignis **Clicked** hinzufügen:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Speichern Sie und neu erstellen Sie Portable Klassenbibliothek-Projekt ohne Fehler überprüfen.


##<a name="add-authentication-to-the-android-app"></a>Die Android Authentifizierung hinzufügen

Dieser Abschnitt veranschaulicht das Implementieren der Schnittstelle **"IAuthenticate"** im Android-Projekt. Überspringen Sie diesen Abschnitt, wenn Android-Geräte unterstützt werden.

1. Klicken Sie in Visual Studio oder Xamarin Studio **Droid** Projekt **als Startprojekt festlegen**.

2. Drücken Sie F5, um das Projekt im Debugger starten und überprüfen, ob eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird. Dies geschieht, da der Zugriff auf die Back-End auf autorisierte Benutzer beschränkt ist.

3. Öffnen Sie MainActivity.cs im Android-Projekt und fügen Sie den folgenden `using` Aussagen:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualisieren Sie die **MainActivity** -Klasse zum Implementieren der Schnittstelle **"IAuthenticate"** wie folgt:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Aktualisieren der **MainActivity** -Klasse ein **MobileServiceUser** -Feld und einem **Authenticate** -Methode der Schnittstelle **"IAuthenticate"** wie folgt erforderlich ist:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Verwenden Sie Identitätsanbieter als Facebook, wählen Sie einen anderen Wert für [MobileServiceAuthenticationProvider].

6. Fügen Sie folgenden Code der **OnCreate** -Methode der **MainActivity** -Klasse vor dem Aufruf von `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Dies stellt sicher, dass der Authentifikator vor dem Laden der Anwendung initialisiert wird.

7. Erstellen Sie die Anwendung neu, ausführen und mit den Authentifizierungsanbieter Sie und stellen Sie sicher, dass Daten als authentifizierter Benutzer zugreifen können.

##<a name="add-authentication-to-the-ios-app"></a>IOS-app Authentifizierung hinzufügen

Dieser Abschnitt veranschaulicht das Implementieren der Schnittstelle **"IAuthenticate"** in iOS-app-Projekt. Überspringen Sie diesen Abschnitt, wenn iOS-Geräte unterstützt werden.

1. Klicken Sie in Visual Studio oder Xamarin Studio das **iOS** -Projekt **als Startprojekt festlegen**.

2. Drücken Sie F5, um das Projekt im Debugger starten und überprüfen, ob eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird. Dies geschieht, da der Zugriff auf die Back-End auf autorisierte Benutzer beschränkt ist.

4. Öffnen Sie AppDelegate.cs in iOS-Projekt und fügen Sie den folgenden `using` Aussagen:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualisieren Sie die **AppDelegate** -Klasse zum Implementieren der Schnittstelle **"IAuthenticate"** wie folgt:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Aktualisieren der **AppDelegate** -Klasse ein **MobileServiceUser** -Feld und einem **Authenticate** -Methode der Schnittstelle **"IAuthenticate"** wie folgt erforderlich ist:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Verwenden Sie Identitätsanbieter als Facebook, wählen Sie einen anderen Wert für [MobileServiceAuthenticationProvider].

6. Fügen Sie folgende Codezeile vor dem Aufruf der **FinishedLaunching** -Methode `LoadApplication()`: 

        App.Init(this);

    Dies stellt sicher, dass der Authentifikator initialisiert wird, bevor die Anwendung geladen wird.

7. Erstellen Sie die Anwendung neu, ausführen und mit den Authentifizierungsanbieter Sie und stellen Sie sicher, dass Daten als authentifizierter Benutzer zugreifen können.


##<a name="add-authentication-to-windows-app-projects"></a>Authentifizierung für Windows app-Projekten hinzufügen

Dieser Abschnitt veranschaulicht das Implementieren der Schnittstelle **"IAuthenticate"** Windows 8.1 und Windows Phone 8.1 app Projekte. Die gleichen Schritte gelten für universelle Windows-Plattform (UWP) Projekte. Überspringen Sie diesen Abschnitt, wenn Sie nicht Windows-Geräte unterstützen.

1. Klicken Sie in Visual Studio **WinApp** oder das **WinPhone81** -Projekt **als Startprojekt festlegen**.

2. Drücken Sie F5, um das Projekt im Debugger starten und überprüfen, ob eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird. Dies geschieht, da der Zugriff auf die Back-End auf autorisierte Benutzer beschränkt ist.

3. Öffnen Sie MainPage.xaml.cs für das Windows-Anwendungsprojekt und fügen Sie den folgenden `using` Aussagen:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Ersetzen Sie `<your_Portable_Class_Library_namespace>` mit dem Namespace für portable Klassenbibliothek.

4. Aktualisieren der **MainPage** -Klasse zum Implementieren der Schnittstelle **"IAuthenticate"** wie folgt:

        public sealed partial class MainPage : IAuthenticate


5. Aktualisieren der **MainPage** -Klasse ein **MobileServiceUser** Feld und **eine Authentifizierungsmethode der Schnittstelle **"IAuthenticate"** , wie folgt erforderlich ist** :

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Verwenden Sie Identitätsanbieter als Facebook, wählen Sie einen anderen Wert für [MobileServiceAuthenticationProvider].

6. Fügen Sie folgenden Code im Konstruktor für **MainPage** -Klasse vor dem Aufruf von `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Ersetzen Sie `<your_Portable_Class_Library_namespace>` mit dem Namespace für portable Klassenbibliothek.  
    Ist dies das WinApp-Projekt, können Sie zu Schritt 8 überspringen. Der nächste Schritt gilt nur für WinPhone81 Projekt sollten Rückruf Anmeldung abzuschließen.

7. (Optional) **WinPhone81** -app-Projekt öffnen Sie App.xaml.cs, und fügen Sie den folgenden `using` Aussagen:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Ersetzen Sie `<your_Portable_Class_Library_namespace>` mit dem Namespace für portable Klassenbibliothek.

8.  **OnActivated** -Methode überschreiben der **App** -Klasse hinzufügen:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Beim Überschreiben der Methode bereits vorhanden ist, nur aus den oben stehenden Ausschnitt bedingten Code hinzufügen.

7. Erstellen Sie die Anwendung neu, ausführen und mit den Authentifizierungsanbieter Sie und stellen Sie sicher, dass Daten als authentifizierter Benutzer zugreifen können.

##<a name="next-steps"></a>Nächste Schritte

Abschluss dieser praktischen Einführung Standardauthentifizierung sollten Sie auf eine der folgenden Lernprogramme:

+ [Push-Benachrichtigung für Ihre Anwendung hinzufügen](app-service-mobile-xamarin-forms-get-started-push.md)  
  Informationen Sie zum Push Notifications für Ihre Anwendung unterstützen und Mobile App Backend um Azure Notification Hubs Pushbenachrichtigungen senden konfigurieren hinzufügen.

+ [Offline-Synchronisierung für Ihre Anwendung aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

