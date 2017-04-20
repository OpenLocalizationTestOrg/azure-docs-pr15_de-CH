
1. Öffnen Sie im **Projekt-Explorer** Android Studio die Datei ToDoActivity.java und fügen Sie Import Folgendes hinzu.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Fügen Sie die folgende Methode zur Klasse **ToDoActivity** : 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Dies erstellt eine neue Methode zum Behandeln des Authentifizierungsprozesses. Google Anmeldung wird der Benutzer authentifiziert. Ein Dialogfeld wird angezeigt, die ID des authentifizierten Benutzers angezeigt. Ohne eine positive Authentifizierung kann nicht fortgesetzt werden.

    > [AZURE.NOTE] Wenn ein Identitätsprovider als Google ändern **Anmeldemethode oben auf eine der folgenden** übergebenen Wert: _MicrosoftAccount_, _Facebook_, _Twitter_oder _Windowsazureactivedirectory_.

3. Im **OnCreate** -Methode fügen Sie die folgende Zeile Code nach dem Code instanziiert die `MobileServiceClient` Objekt.

        authenticate();

    Dieser Aufruf startet den Authentifizierungsprozess.

4. Verschieben Sie den übrigen Code nach `authenticate();` in der **OnCreate** -Methode einer neuen **CreateTable** -Methode die sieht folgendermaßen aus:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Menü **Ausführen** klicken Sie auf die Anwendung und Ihre ausgewählten Identitätsanbieter anmelden **Anwendung ausführen** . 

    Wenn Sie sich erfolgreich angemeldet haben, sollte die Anwendung ohne Fehler ausgeführt und sollte zu den Back-End-Dienst Abfragen und Updates an Daten.