<properties
    pageTitle="Offline-Synchronisierung für Ihre Azure Mobile (Xamarin.Forms) aktivieren | Microsoft Azure"
    description="Erfahren Sie, wie App Service Mobile App Cache und Synchronisation offline Daten in Ihrer Anwendung Xamarin.Forms"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Offline-Synchronisierung für Ihre mobilen Xamarin.Forms aktivieren

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Übersicht
Dieses Lernprogramm führt die offline Synchronisierungsfunktion Azure Mobile Apps für Xamarin.Forms. Offline Sync ermöglicht Endbenutzern, mit einer app - anzeigen, hinzufügen oder Ändern von Daten, selbst wenn keine Verbindung zum Netzwerk zu interagieren. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem remote-Dienst synchronisiert.

Dieses Lernprogramm basiert auf Xamarin.Forms Schnellstart Lösung für Mobile Apps, die Sie beim Ausführen des Lernprogramms [erstellen eine Xamarin iOS-app] erstellen. Die Schnellstart-Lösung für Xamarin.Forms enthält Code zum unterstützen offline synchronisieren muss aktiviert sein. In diesem Lernprogramm aktualisieren Sie die Schnellstart-Lösung offline Funktionen von Azure Mobile Apps aktivieren. Wir markieren Sie offline-spezifischen Code in der Anwendung. Wenn Sie nicht die heruntergeladenen Quickstart-Lösung verwenden, müssen Sie Data Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server-Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps][1].

Erfahren Sie mehr über die offline Synchronisierungsfunktion finden Sie [Offline Daten-Synchronisierung in Azure Mobile Apps][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Offline synchronisieren den Funktionsumfang Schnellstart aktivieren

Code offline synchronisieren ist mit C# Präprozessordirektiven im Projekt enthalten. Wenn der **OFFLINE\_SYNC\_aktiviert** Symbol definiert, werden diese Pfade im Build enthalten. Für Windows-apps müssen auch SQLite Plattform installieren.

1. Klicken Sie in Visual Studio die Projektmappe > **NuGet-Pakete verwalten, für die Lösung**dann suchen und installieren **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket für alle Projekte in der Projektmappe.

2. Entfernen Sie im Projektmappen-Explorer TodoItemManager.cs aus dem Projekt den Namen mit **tragbaren** öffnen ist Portable Klassenbibliothek-Projekt dann nach Präprozessordirektive:

        #define OFFLINE_SYNC_ENABLED

4. (Optional) Unterstützung von Windows-Geräten installieren eines SQLite Runtime Pakete:

    * **Windows 8.1 Runtime:** [SQLite für Windows 8.1]installieren[3].
    * **Windows Phone 8.1:** [SQLite für Windows Phone 8.1]installieren[4].
    * **Universelle Windows-Plattform** [SQLite universellen universelle Windows]installieren[5].

    Obwohl Schnellstart nicht universelle Windows-Projekt enthält, wird die universelle Windows-Plattform mit Xamarin unterstützt.

5. (Optional) Klicken Sie in jedem Windows-app-Projekt **Verweise** > **Verweis hinzufügen**, erweitern Sie den Ordner **Windows** > **Extensions**.
    Aktivieren Sie das entsprechende **SQLite für Windows** SDK mit **Visual C++ 2013 Runtime für Windows** SDK.
    SQLite SDK Namen abweichen mit jeder Windows-Plattform.

## <a name="review-the-client-sync-code"></a>Überprüfen Sie den Clientcode synchronisieren

Hier wird eine kurze Übersicht über bereits der Code des Lernprogramms im Lieferumfang der `#if OFFLINE_SYNC_ENABLED` Richtlinien. Offline Sync-Funktionalität ist in der TodoItemManager.cs-Projektdatei im Portable Klassenbibliothek-Projekt. Eine grundlegende Übersicht über die Funktion finden Sie unter [Offline Daten synchronisieren in Azure Mobile Apps][2].

* Bevor Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher initialisiert. Die lokale Datenbank wird durch den folgenden Code im **TodoItemManager** -Klassenkonstruktor initialisiert:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Dieser Code erstellt eine neue lokale SQLite Datenbank mithilfe der **MobileServiceSQLiteStore** -Klasse.

    **DefineTable** -Methode erstellt eine Tabelle in den lokalen Speicher, der die Felder in den bereitgestellten Typ entspricht.  Der Typ muss alle Spalten enthalten, die in der entfernten Datenbank. Es ist möglich, eine Teilmenge der Spalten speichern.

* Feld **TodoTable** **TodoItemManager** ist ein **IMobileServiceSyncTable** -Typ statt **IMobileServiceTable**. Diese Klasse verwendet die lokale Datenbank für alle erstellen, lesen, aktualisieren und löschen (CRUD) Tabelle. Sie entscheiden, wann diese Änderungen Mobile App Back-End-Aufruf von **PushAsync** für die **IMobileServiceSyncContext**gesendet werden. Der Synchronisierungskontext hilft beibehalten Beziehungen verfolgen und Änderungen in allen Tabellen, die eine Clientanwendung geändert hat, wenn **PushAsync** aufgerufen wird.

    Die folgende **SyncAsync** -Methode aufgerufen mit Backend Mobile-Anwendung synchronisieren:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Einfachen Fehlerbehandlung mit dem Standardhandler Synchronisierung verwendet. Eine reale Anwendung würde eine benutzerdefinierte **IMobileServiceSyncHandler** -Implementierung mit Fehler Netzwerkprobleme wie Server Konflikte behandeln.

## <a name="offline-sync-considerations"></a>Aspekte der offline synchronisieren

Im Beispiel heißt die Methode **SyncAsync** nur einschalten und Synchronisierung angefordert wird.  Ziehen Sie zum Initiieren der Synchronisierung in Android oder iOS-app in der Liste nach unten; Verwenden Sie unter Windows die **Sync** -Taste. In einer realen Anwendung können Sie auch Trigger Sync bei Netzwerkstatus geänderten.

Wenn ein für eine Tabelle ausgeführt wird, die ausstehende verfolgt lokale Updates vom Kontext, der Vorgang automatisch Trigger ziehen einen vorherigen Kontext Push. Aktualisieren, hinzufügen und Elemente in diesem Beispiel abschließen, können Sie den expliziten Aufruf **PushAsync** auslassen.

In den bereitgestellten Code alle Datensätze in der entfernten Tabelle TodoItem abgefragt, aber kann auch zum Filtern von Datensätzen durch eine Abfrage-Id und die Abfrage an **PushAsync**übergeben. Weitere Informationen finden Sie im Abschnitt *Inkrementelle Synchronisierung* [Offline Daten]synchron in Azure Mobile Apps[2].

## <a name="run-the-client-app"></a>Führen Sie die Clientanwendung

Die Client-Anwendung offline Sync jetzt aktiviert auf jeder Plattform die lokale Datenbank aufgefüllt mindestens einmal ausgeführt. Später simulieren Sie offline Szenario und ändern Sie die Daten im lokalen Speicher, während die Anwendung offline ist.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Aktualisieren Sie das Synchronisierungsverhalten für die Clientanwendung

In diesem Abschnitt ändern Sie das Clientprojekt simulieren offline Szenario mithilfe einer URL ungültige Anwendung für die Back-End. Alternativ können Sie Netzwerkschnittstellen deaktivieren indem Ihr Gerät auf "Flugmodus".  Beim Hinzufügen oder Ändern von Daten geändert statt im lokalen Speicher jedoch nicht an den Back-End-Datenspeicher synchronisiert, bis die Verbindung wiederhergestellt ist.

1. Im Projektmappen-Explorer öffnen Sie die Projektdatei Constants.cs aus dem **tragbaren** Projekt, und ändern Sie den Wert der `ApplicationURL` auf eine ungültige URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Öffnen Sie die Datei TodoItemManager.cs aus dem **tragbaren** Projekt und **try...catch** -Block in **SyncAsync** **abfangen** der **Ausnahme** Basisklasse hinzufügen. Dieser **catch** -Block schreibt die Ausnahmemeldung auf der Konsole:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Erstellen Sie und führen Sie die Clientanwendung.  Einige neue Elemente hinzuzufügen. Beachten Sie, dass eine Ausnahme in der Konsole bei jedem Versuch mit dem Back-End angemeldet ist. Diese neuen Elemente vorhanden im lokalen Speicher, bis sie mobile Backend abgelegt werden können. Die Clientanwendung verhält sich, als ob es der Back-End-Unterstützung alle erstellen, lesen, aktualisieren, Löschvorgänge (CRUD) verbunden ist.

4. Schließen Sie die Anwendung und neu starten Sie, um sicherzustellen, dass neue Elemente erstellten lokalen Speicher beibehalten werden.

5. (Optional) Verwenden Sie Visual Studio an der Tabelle Azure SQL-Datenbank, um anzuzeigen, dass die Daten in der Back-End-Datenbank nicht geändert wurde.

    Öffnen Sie **Server-Explorer**in Visual Studio. Navigieren Sie zu der Datenbank in **Azure**->**SQL-Datenbanken**. Ihre Datenbank Maustaste und wählen **im SQL Server-Objekt-Explorer öffnen**. Jetzt können Sie Ihre SQL-Datenbanktabelle und dessen Inhalt durchsuchen.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Aktualisieren der Clientanwendung Ihre mobile Back-End-Verbindung

Schließen Sie in diesem Abschnitt die app mobile Backend, das Anwendung wieder in den Online-Zustand simuliert. Beim Ausführen der Aktion aktualisieren Daten auf mobilen Backend synchronisiert.

1. Öffnen Sie Constants.cs. Korrigieren der `applicationURL` den richtigen URL auf.

2. Erstellen Sie und führen Sie die Clientanwendung. Die Anwendung versucht, Backend mobile Anwendung nach dem Starten von synchronisiert. Stellen Sie sicher, dass keine Ausnahmen in der Debugkonsole angemeldet sind.

3. (Optional) Anzeigen die aktualisierten Daten mit SQL Server-Objekt-Explorer oder anderen Tools wie Fiddler oder [Postbote][6]. Beachten Sie, dass die Daten zwischen Back-End-Datenbank und den lokalen Speicher synchronisiert wurde.

    Die Daten zwischen der Datenbank und den lokalen Speicher synchronisiert und enthält die Elemente, die hinzugefügt, während Ihre Anwendung getrennt wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure mobiler Apps][2]
* [Azure mobiler Apps .NET SDK HOWTO][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md