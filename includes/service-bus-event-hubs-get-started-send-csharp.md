## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Event Hubs

In diesem Abschnitt werden Sie eine Windows Konsolenanwendung schreiben, die Ereignisse mit der Ereignis-Hub sendet.

1. Erstellen Sie in Visual Studio ein neues Visual C# Desktop-App-Projekt mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **Absender**.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. Im Projektmappen-Explorer mit der rechten Maustaste der Projektmappe und dann auf **NuGet-Pakete verwalten, Lösung**. 

3. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Sicherstellen Sie, dass im Feld **Version** der Namen (**Absender**) angegeben ist. Klicken Sie auf **Installieren**und akzeptieren Sie die Vereinbarung. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio heruntergeladen, installiert und ein Verweis auf das [Azure Service Bus Bibliothek NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Fügen Sie den folgenden `using` -Anweisung am Anfang der Datei **"Program.cs"** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Die Klasse **Program** , ersetzen die Platzhalterwerte durch den Namen des im vorherigen Abschnitt erstellte Ereignis-Hub und die Verbindungszeichenfolge auf Namespaceebene zuvor gespeicherte fügen Sie folgende Felder hinzu.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Der **Program** -Klasse die folgende Methode hinzugefügt:

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Diese Methode sendet Ereignisse ständig an der Ereignis-Hub mit einer Verzögerung von 200 ms.

7. Die **Main** -Methode schließlich folgenden Zeilen hinzufügen:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
