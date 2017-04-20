
1. Fügen Sie in der Projektdatei MainPage.xaml.cs Folgendes **verwenden** :

        using System.Linq;      
        using Windows.Security.Credentials;

2. Ersetzen Sie die **AuthenticateAsync** -Methode durch folgenden Code:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    Die Anwendung versucht, in dieser Version von **AuthenticateAsync**in **PasswordVault** gespeicherte Anmeldeinformationen verwenden, um auf den Dienst zugreifen. Reguläre Anmeldung erfolgt auch wird keine gespeicherten Anmeldeinformationen.

    >[AZURE.NOTE]Ein zwischengespeichertes Token abgelaufen ist und Ablauf des Tokens kann auch bei die Anwendung wird nach der Authentifizierung auftreten. Informationen zum bestimmen, ob ein Token abgelaufen ist, sehen Sie [für abgelaufenen Authentifizierungstokens](http://aka.ms/jww5vp). Eine Lösung zum Behandeln von Autorisierungsfehlern ablaufende Token beziehen finden Sie im Eintrag [Zwischenspeichern und Behandlung abgelaufene Token in Azure Mobile Services SDK verwaltet](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Starten Sie die Anwendung zweimal.

    Beachten Sie, dass beim ersten Start mit den Anbieter erneut erforderlich ist. Jedoch beim zweiten Neustart werden die zwischengespeicherten Anmeldeinformationen verwendet und Anmelden umgangen. 
