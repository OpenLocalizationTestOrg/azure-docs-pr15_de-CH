<properties
    pageTitle="Offline-Synchronisierung für Ihre Azure Mobile (Cordova) aktivieren | Microsoft Azure"
    description="Erfahren Sie, wie App Service Mobile App Cache und Synchronisation offline-Daten in der Cordova-Anwendung verwenden"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Offline-Synchronisierung für Ihre mobilen Cordova aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Dieses Lernprogramm führt die offline Synchronisierungsfunktion Azure Mobile Apps für Cordova. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem remote-Dienst synchronisiert.

Dieses Lernprogramm basiert auf Cordova Schnellstart Lösung für Mobile Apps, die Sie beim Ausführen des Lernprogramms [Apache Cordova quick start]erstellen. Aktualisieren Sie in diesem Lernprogramm Schnellstart Lösung offline Funktionen von Azure Mobile Apps hinzufügen. Offline-spezifischen Code in der Anwendung wird auch hervorzuheben.

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps]. Details der API-Nutzung finden Sie in [der Infodatei im Plug-in] .

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Der Schnellstart-Lösung offline synchronisieren hinzufügen

Die Anwendung muss offline synchronisieren Code hinzugefügt. Offline synchronisieren erfordert Cordova Sqlite Storage Plugin-Plugin Azure Mobile Apps im Projekt enthalten ist auf Ihre Anwendung automatisch hinzugefügt wird. Das Schnellstart-Projekt enthält beide dieser Plugins.

1. Öffnen Sie im Projektmappen-Explorer von Visual Studio index.js, und Ersetzen Sie den folgenden code

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    mit diesem Code:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Als Nächstes ersetzen Sie folgenden Code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    mit diesem Code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Vorhergehenden Code Hinzufügen lokaler Informationsspeicher und definieren eine lokale Tabelle, die Spaltenwerte verwendet entspricht in der Azure-back-End. (Sie müssen alle Spaltenwerte in diesem Code enthalten.)

    Abrufen eines Verweises auf den Synchronisierungskontext durch Aufrufen von **GetSyncContext**. Der Synchronisierungskontext hilft Beziehungen und drücken ändert alle Tabellen eine Clientanwendung geändert hat, beim **Push** wird beibehalten.

3. Aktualisieren Sie die URL Ihrer Anwendung URL Mobile-Anwendung.

4. Als Nächstes ersetzen Sie diesen Code:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    mit diesem Code:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Dieser Code initialisiert den Synchronisierungskontext und ruft dann einen Verweis auf die lokale Tabelle zu GetSyncTable (statt getTable).

    Dieser Code verwendet die lokale Datenbank für alle erstellen, lesen, aktualisieren und löschen (CRUD) Tabelle.

    Dieses Beispiel führt Fehler auf Synchronisierungskonflikte. Eine echte Anwendung würden Fehler Netzwerkprobleme und Server Konflikte behandeln. Codebeispiele finden Sie im [Beispiel offline synchronisieren].

5. Fügen Sie diese Funktion, um die eigentliche Synchronisierung durchführen.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Sie entscheiden, wann müssen Änderungen Backend Mobile-Anwendung durch **Push** vom Client verwendete **SyncContext** -Objekt aufrufen. Beispielsweise ein-Ereignishandler in der Anwendung wie eine Synchronisierungsschaltfläche konnte einen Aufruf von **SyncBackend** hinzufügen oder hinzufügen kann des Aufrufs der Funktion **AddItemHandler** synchronisieren, wenn ein neues Element hinzugefügt wird.

##<a name="offline-sync-considerations"></a>Aspekte der offline synchronisieren

Im Beispiel wird **die Push **SyncContext** ** nur für app-Start in der Rückruffunktion für Anmeldung aufgerufen.  In einer realen Anwendung können Sie auch dies Synchronisierung manuell ausgelöst oder wenn der Netzwerkstatus geändert wird.

Wenn ein für eine Tabelle, die ausstehende lokale Updates nachverfolgt wurde vom Kontext ausgeführt wird, löst, Pull-Vorgang automatisch einen vorherigen Kontext Push. Beim Aktualisieren, können hinzufügen und Elemente in diesem Beispiel ausfüllen Sie explizite **Push** -Aufruf weggelassen werden, da redundante (erste Kontrollkästchen Feature Stand der [Readme-Datei] ) werden kann.

In den bereitgestellten Code alle Datensätze in der Tabelle TodoItem remote abgefragt, aber kann auch zum Filtern von Datensätzen durch eine Abfrage-Id und die Abfrage **zu**übergeben. Weitere Informationen finden Sie im Abschnitt *Inkrementelle Synchronisierung* [Offline Daten]synchron in Azure Mobile Apps.

## <a name="optional-disable-authentication"></a>(Optional) Deaktivieren der Authentifizierung

Wenn nicht bereits Authentifizierung festgelegt und möchten Authentifizierung vor dem Testen offline synchronisieren, auskommentieren Rückruffunktion für Benutzernamen, aber lassen Sie den Code nicht kommentiert die Callback-Funktion.

Der Code sollte wie folgt aussehen, nach Anmeldung Zeilen auskommentieren.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Die Anwendung, mit der Azure-Backend synchronisiert beim Ausführen der Anwendung statt beim Anmelden werden.

## <a name="run-the-client-app"></a>Führen Sie die Clientanwendung

Offline Sync jetzt aktiviert können Sie nun die Client-Anwendung mindestens einmal auf jeder Plattform die lokale Datenbank aufgefüllt ausführen. Sie können offline Szenario simulieren und Daten im lokalen Speicher ändern, während die Anwendung offline ist.

## <a name="optional-test-the-sync-behavior"></a>(Optional) Testen Sie das Synchronisierungsverhalten

In diesem Abschnitt ändern Sie das Clientprojekt simulieren offline Szenario mithilfe einer URL ungültige Anwendung für die Back-End. Beim Hinzufügen oder Ändern von Daten werden geändert im lokalen Speicher gespeichert, aber nicht mit dem Back-End-Datenspeicher synchronisiert, bis die Verbindung wiederhergestellt ist.

1. Öffnen Sie im Projektmappen-Explorer die Projektdatei index.js und ändern Sie die URL auf eine ungültige URL wie folgt:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Aktualisieren Sie index.html CSP `<meta>` mit derselben URL ungültig.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Erstellen Sie und führen Sie die Clientanwendung, und beachten Sie, dass eine Ausnahme in der Konsole angemeldet ist, wenn die Anwendung versucht, Backend nach Anmeldung synchronisieren. Alle neuen Elemente, die Sie hinzufügen werden nur im lokalen Speicher vorhanden, bis sie mobile Backend abgelegt werden können. Die Clientanwendung verhält sich wie der Back-End-Unterstützung alle erstellen, lesen, aktualisieren, Löschvorgänge (CRUD) verbunden ist.

4. Schließen Sie die Anwendung und neu starten Sie, um sicherzustellen, dass neue Elemente erstellten lokalen Speicher beibehalten werden.

5. (Optional) Verwenden Sie Visual Studio an der Tabelle Azure SQL-Datenbank, um anzuzeigen, dass die Daten in der Back-End-Datenbank nicht geändert wurde.

    Öffnen Sie **Server-Explorer**in Visual Studio. Navigieren Sie zu der Datenbank in **Azure**->**SQL-Datenbanken**. Ihre Datenbank Maustaste und wählen **im SQL Server-Objekt-Explorer öffnen**. Jetzt können Sie Ihre SQL-Datenbanktabelle und dessen Inhalt durchsuchen.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optional) Testen Sie die Wiederherstellung auf mobilen backend

In diesem Abschnitt wird die app mobile Back-End-Verbindung die Anwendung wieder in den Online-Zustand simuliert. Beim Anmelden, werden Daten auf mobilen Backend synchronisiert.

1. Öffnen Sie index.js, und korrigieren Sie die URL auf den richtigen URL.

2. Index.html öffnen und korrigieren Sie die URL im CSP `<meta>` Element.

3. Erstellen Sie und führen Sie die Clientanwendung. Die Anwendung versucht, mobile-app Backend nach Anmeldung synchronisieren. Stellen Sie sicher, dass keine Ausnahmen in der Debugkonsole angemeldet sind.

4. (Optional) Anzeigen der aktualisierten Daten mit SQL Server-Objekt-Explorer oder einem anderen Tool wie Fiddler. Beachten Sie, dass die Daten zwischen Back-End-Datenbank und den lokalen Speicher synchronisiert wurde.

    Die Daten zwischen der Datenbank und den lokalen Speicher synchronisiert und enthält die Elemente, die hinzugefügt, während Ihre Anwendung getrennt wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure mobiler Apps]

* [Bewölkung: Offline synchronisieren in Azure Mobile Dienste] \(Hinweis: Videos auf Mobile Services ist jedoch offline synchronisieren funktioniert ähnlich wie in Azure Mobile Apps\)

* [Visual Studio-Tools für Apache Cordova]

## <a name="next-steps"></a>Nächste Schritte

* Erweiterte offline Synchronisierungsfunktionen wie Konfliktbehebung [offline synchronisieren Beispiel] betrachten
* Betrachten Sie offline Sync-API-Referenz in der [Readme-Datei]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova-Schnellstart]: app-service-mobile-cordova-get-started.md
[README-DATEI]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Offline Sync-Beispiel]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Offline-Daten synchronisieren in Azure mobiler Apps]: app-service-mobile-offline-data-sync.md
[Bewölkung: Offline synchronisieren in Azure Mobile Dienste]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
