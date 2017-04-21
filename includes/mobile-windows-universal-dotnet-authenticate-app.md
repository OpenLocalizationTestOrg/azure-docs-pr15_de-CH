
1. Öffnen Sie die Projektdatei MainPage.cs und der MainPage-Klasse hinzuzufügen Sie den folgenden Codeausschnitt:
    
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

3. Auskommentieren oder löschen Sie den Aufruf der **RefreshTodoItems** -Methode in vorhandenen **OnNavigatedTo** -Methode überschreiben.

    Dadurch wird verhindert, dass die Daten geladen werden, bevor der Benutzer authentifiziert ist. Als Nächstes fügen Sie eine Schaltfläche **Anmelden** an die Anwendung, die Authentifizierung auslöst.

4. Im folgenden Codeausschnitt der MainPage-Klasse hinzufügen:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Windows Store-app-Projekt öffnen der Datei MainPage.xaml Projekt und fügen folgende **Button** -Element vor dem Element, das die Schaltfläche **Speichern** definiert:

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. Fügen Sie in Windows Phone Store-app-Projekt folgende **Button** -Element in **ContentPanel**nach dem **TextBox** -Element:

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Öffnen Sie die freigegebene App.xaml.cs Projektdatei, und fügen Sie folgenden Code:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Wenn die **OnActivated** -Methode bereits vorhanden ist, fügen Sie einfach die `#if...#endif` Codeblock.

9. Drücken Sie F5, um Windows Store-app ausführen, klicken Sie auf **Anmelden** und die Anwendung mit der gewählten Identitätsanbieter anmelden. 

    Wenn Sie erfolgreich angemeldet sind, sollte die Anwendung ohne Fehler ausgeführt, und Sie sollen Ihre Back-End-Abfragen und Updates auf Daten.

10. Mit der rechten Maustaste Windows Phone Store-app-Projekt, klicken Sie auf **als Startprojekt festlegen**, und wiederholen Sie dem vorherigen Schritt zu überprüfen, ob Windows Phone Speicher app auch korrekt ausgeführt wird.  

 