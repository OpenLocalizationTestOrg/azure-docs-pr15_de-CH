
1. In der Projektmappe anzeigen (oder **Projektmappen-Explorer** in Visual Studio) Maustaste Ordner **Komponenten** **Erhalten mehr Komponenten...**, **Google Cloud Messaging-Client** -Komponente suchen und auf dem Projekt hinzufügen.

2. Öffnen Sie die Projektdatei ToDoActivity.cs und fügen Sie die folgende using-Anweisung hinzu:

        using Gcm.Client;

3. Fügen Sie in der **ToDoActivity** -Klasse den folgenden Code hinzu: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Dadurch werden der Dienstprozess Push-Handler-Instanz des mobilen Clients zugreifen.

4.  Fügen Sie folgenden Code **OnCreate** -Methode, nachdem der **MobileServiceClient** erstellt wurde:

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Die **ToDoActivity** ist nun zum Hinzufügen von Pushbenachrichtigungen bereit.