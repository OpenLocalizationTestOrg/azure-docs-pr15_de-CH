<properties
    pageTitle="Authentifizierung für Ihre app universelle Windows-Plattform (UWP) hinzufügen | Azure mobiler Apps"
    description="Lernen Azure App Service Mobile Apps zum Authentifizieren von Benutzern der Anwendung Universal Windows Plattform (UWP) mit verschiedenen Identitätsprovider einschließlich: AAD, Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Authentifizierung für Windows-Anwendung hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema veranschaulicht die Cloud-basierte Authentifizierung der mobilen Anwendung hinzufügen. In diesem Lernprogramm fügen Sie Authentifizierung universelle Windows-Plattform (UWP) Schnellstart-Projekt für Mobile Apps mit Identitätsanbieter Azure App Service unterstützt. Authentifiziert und autorisiert durch Mobile App Backend erfolgreich, wird der Benutzer-ID-Wert angezeigt.

Dieses Lernprogramm basiert auf Mobile Apps Schnellstart. Sie müssen zuerst das Lernprogramm [Erste Schritte mit Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).

##<a name="register"></a>Registrieren Sie Ihre Anwendung für die Authentifizierung und konfigurieren Anwendung

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Schränken Sie die Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nun können Sie überprüfen, dass anonymer Zugriff auf Ihre Back-End-deaktiviert wurde. Bereitstellen Sie der UWP-app-Projekt als Startprojekt festlegen und führen Sie die Anwendung. Stellen Sie sicher, dass eine nicht behandelte Ausnahme mit dem Statuscode 401 (nicht autorisiert) nach dem Starten der Anwendung ausgelöst wird. Dies geschieht, da die Anwendung versucht, die Mobile Anwendungscode als nicht authentifizierter Benutzer zugreifen, aber die *TodoItem* jetzt Authentifizierung benötigt.

Anschließend aktualisieren Sie die Anwendung um Benutzer zu authentifizieren, bevor Ressourcen von App Service anfordern.

##<a name="add-authentication"></a>Authentifizierung für die Anwendung hinzufügen

1. In der UWP app Projektdatei MainPage.cs und der MainPage-Klasse den folgenden Codeausschnitt hinzuzufügen:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Dieser Code authentifiziert den Benutzer mit Facebook anmelden. Wenn Identitätsanbieter als Facebook ändern Sie den Wert der **MobileServiceAuthenticationProvider** über dem Wert für den Anbieter.

3. Auskommentieren oder Löschen des Aufrufs der Methode **ButtonRefresh_Click** (oder **InitLocalStoreAsync** Methode) vorhandenen **OnNavigatedTo** -Methode überschreiben. Dadurch wird verhindert, dass die Daten geladen werden, bevor der Benutzer authentifiziert ist. Als Nächstes fügen Sie eine Schaltfläche **Anmelden** an die Anwendung, die Authentifizierung auslöst.

4. Im folgenden Codeausschnitt der MainPage-Klasse hinzufügen:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Öffnen Sie "MainPage.xaml" Projektdatei, suchen Sie das Element, das die Schaltfläche **Speichern** definiert und durch folgenden Code ersetzen:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Drücken Sie auf F5, führen Sie die Anwendung, klicken Sie auf **Anmelden** und die Anwendung mit der gewählten Identitätsanbieter anmelden. Wenn die Anmeldung erfolgreich ist, die Anwendung ohne Fehler ausgeführt, und Sie können Abfragen Backend und Updates Daten.


##<a name="tokens"></a>Speichern Sie das Authentifizierungstoken auf dem client

Das vorherige Beispiel zeigte einen Standard-Anmeldung, die muss der Client Identitätsanbieter und App-Dienst bei jedem Kontakt, der die Anwendung startet. Diese Methode ist nicht nur ineffizient, führen Sie in Verwendung-betrifft Probleme sollten viele Anwendung gleichzeitig zu starten. Ein besserer Ansatz ist zu Ihren App-Dienst zurückgegebene Autorisierungstoken cache zuerst bevor verwenden ein anbieterbasierter anmelden.

>[AZURE.NOTE]Sie können das Token ausgestellt von App unabhängig davon, ob Sie verwalteten Client oder Dienst verwaltet Authentifizierung verwenden Zwischenspeichern. In diesem Lernprogramm verwendet Authentifizierung Dienst verwaltet.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Nächste Schritte

Abschluss dieser praktischen Einführung Standardauthentifizierung sollten Sie auf eine der folgenden Lernprogramme:

+ [Push-Benachrichtigung für Ihre Anwendung hinzufügen](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informationen Sie zum Push Notifications für Ihre Anwendung unterstützen und Mobile App Backend um Azure Notification Hubs Pushbenachrichtigungen senden konfigurieren hinzufügen.

+ [Offline-Synchronisierung für Ihre Anwendung aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

