
Das vorherige Beispiel zeigte Standard Anmeldung erfordert der Client an der Identitätsanbieter und Back-End-Azure Service jedem, der die Anwendung startet. Diese Methode ist nicht nur ineffizient, Verwendung Probleme auftreten können viele app gleichzeitig beginnen sollten. Ein besserer Ansatz ist zu cache zurückgegebene Azure Service Autorisierungstoken zuerst bevor verwenden ein anbieterbasierter anmelden. 

>[AZURE.NOTE]Sie können vom Back-End-Azure Service unabhängig davon, ob Sie verwalteten Client oder Dienst verwaltet Authentifizierung verwenden herausgegebenes Token Zwischenspeichern. In diesem Lernprogramm verwendet Authentifizierung Dienst verwaltet.


1. Öffnen Sie die Datei ToDoActivity.java, und fügen Sie die folgende Import-Anweisung:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Hinzufügen der folgenden Mitgliedern der `ToDoActivity` Klasse.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Fügen Sie in der Datei ToDoActivity.java die folgende Definition für die `cacheUserToken` Methode.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Diese Methode speichert die Id und das Token in eine Voreinstellung als privat gekennzeichnet. Dies sollte Zugriff auf den Cache schützen, so dass andere apps auf dem Gerät ist die Einstellung für die app Sandbox keinen Zugriff auf das Token haben. Allerdings erhält ein Benutzer Zugriff auf das Gerät, kann sie auf token Cache auf andere Weise zugreifen können. 

    >[AZURE.NOTE]Das Token mit Verschlüsselung geschützt token Zugriff auf Ihre Daten ist streng vertrauliche und jemand kann auf das Gerät zugreifen. Eine vollständig sichere Lösung ist jedoch im Rahmen dieses Lernprogramms und Ihrer Sicherheitserfordernisse abhängig.


4. Fügen Sie in der Datei ToDoActivity.java die folgende Definition für die `loadUserTokenCache` Methode.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Ersetzen Sie in der Datei *ToDoActivity.java* die `authenticate` Methode mit der folgenden Methode token Cache verwendet. Ändern Sie den Anmeldeanbieter Google Konto verwendet werden soll.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Erstellen Sie die Anwendung und Test Authentifizierung mit einem gültigen Konto. Mindestens zweimal ausgeführt. Bei der ersten Ausführung erhalten Sie eine Aufforderung zum Anmelden und zum Erstellen von token Caches. Danach sollten versucht alle token Cache für Authentifizierung laden und Sie nicht anmelden.



