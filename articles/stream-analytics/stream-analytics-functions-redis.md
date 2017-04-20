<properties
    pageTitle="Ein Azure Redis Cache mit Azure Funktionen von Azure Stream Analytics Ausgabe | Microsoft Azure"
    description="Erfahren Sie, wie eine Azure-Funktion verbunden ein Service Bus-Warteschlange um Azure Redis Cache aus der Ausgabe eines Auftrags Stream Analytics füllen."
    keywords="Datenstrom Redis Cache Service Bus-Warteschlange"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Wie in einem Azure Redis Cache Azure Funktionen von Azure Stream Analytics Datenspeicher

Azure Stream Analytics ermöglicht die schnelle Entwicklung und Bereitstellung von kostengünstigen Lösungen von Geräten, Sensoren, Infrastruktur und Applikationen oder jeder Datenstrom in Echtzeit Einblick. Sie können verschiedene Anwendungsfälle wie Echtzeit-Management und Überwachung, Befehl und Steuerung, Betrugsversuche, verbundenen Fahrzeuge. In vielen dieser Szenarien möchten Sie speichern Daten in verteilten Datenspeicher wie ein Azure Redis Cache von Azure Stream Analytics ausgegeben.

Angenommen Sie, ein Teil sind. SIM-Betrug erkennen, wo mehrere Aufrufe von derselben Identität gleichzeitig, aber andere geografisch Zeit, versuchen Speicherorte. Sie sollen alle möglichen betrügerischen Anrufe in Azure Redis-Cache gespeichert. In diesem Blog bieten wir Anleitung auf, wie einfach die Aufgabe abschließen können. 

## <a name="prerequisites"></a>Erforderliche Komponenten
Führen Sie die [Erkennung in Echtzeit] [ fraud-detection] schrittweise asa

## <a name="architecture-overview"></a>Übersicht über die Architektur
![Screenshot der Architektur](./media/stream-analytics-functions-redis/architecture-overview.png)

Wie in der vorherigen Abbildung gezeigt, können Stream Analytics streaming eingegebenen Daten abgefragt und Ausgabe gesendet. Anhand der Ausgabe können Azure Funktionen ein Ereignis auslösen. 

In diesem Blog konzentrieren wir uns auf Azure Funktionen Teil dieser Pipeline und insbesondere das Auslösen eines Ereignisses, die betrügerische Daten im Cache gespeichert.
Nach Abschluss der [Erkennung in Echtzeit] [ fraud-detection] Lernprogramm haben Sie Eingabe (Ereignis-Hub), eine Abfrage und eine Ausgabe (BLOB-Speicher) bereits konfiguriert und ausgeführt. In diesem Blog ändern wir die Ausgabe Service Bus-Warteschlange verwenden. Danach verbinden wir Azure-Funktion in diese Warteschlange. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Erstellen Sie und verbinden Sie eine Ausgabe Service Bus-Warteschlange
Ein Service Bus-Warteschlange erstellen Sie Schritte 1 und 2 des Abschnitts .NET in [Erste Schritte mit Service Bus Warteschlangen][servicebus-getstarted].
Jetzt verbinden wir die Warteschlange Stream Analytics-Projekt, das in der exemplarischen Vorgehensweise Erkennung von früheren Betrug erstellt wurde.



1. In Azure-Portal zur Blade **Ausgaben** des Projekts, und wählen Sie am oberen Rand der Seite **Hinzufügen** .

    ![Ausgaben hinzufügen](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Wählen Sie **Service Bus-Warteschlange** als **Auffangen** und Anleitung auf dem Bildschirm. Den Namespace des Service Bus-Warteschlange wählen Sie in [Erste Schritte mit Service Bus Warteschlangen]erstellt[servicebus-getstarted]. Klicken Sie "rechts" Wenn Sie fertig sind.
3. Geben Sie die folgenden Werte:
    - **Ereignis Serialisierungsprogramm Format**: JSON
    - **Codierung**: UTF8
    - **FORMAT**: Zeile getrennt

4. Klicken Sie **Erstellen** diese Quelle hinzufügen und überprüfen, ob der Stream Analytics das Speicherkonto herstellen können.

5. Ersetzen Sie auf der Registerkarte **Abfrage** die aktuelle Abfrage folgende. Ersetzen Sie *[IHR SERVICE BUS NAME]* durch den Ausgabenamen in Schritt 3 erstellten. 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Erstellen einer Azure Redis
Erstellen einen Azure Redis Cache Abschnitt .NET unter [Verwendung Azure Redis Cache] [ use-rediscache] bis Abschnitt ***Konfigurieren Sie die cacheclients***aufgerufen.
Danach müssen Sie einen neuen Redis Cache. Wählen Sie unter **Alle** **Zugriffstasten** und notieren Sie sich die ***primären Verbindungszeichenfolge***.

![Screenshot der Architektur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Erstellen Sie eine Azure-Funktion
Führen Sie [erstellen Ihre erste Azure-Funktion] [ functions-getstarted] Lernprogramm Einstieg in Azure-Funktionen. Haben Sie bereits eine Azure-Funktion verwenden möchten, fahren Sie fort mit [Redis Cache Schreiben](#Writing-to-Redis-Cache)

1. Im Portal Anwendungsdienste in der linken Navigationsleiste, klicken Sie auf Namen Azure-Funktion die Funktion app-Website zu.
    ![Screenshot der Anwendungsliste Services-Funktion](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Klicken Sie auf **neue Funktion > ServiceBusQueueTrigger C#-**. Anleitung für die folgenden Felder:
    - **Name der Warteschlange**: denselben Namen wie beim Erstellen der Warteschlange in [Erste Schritte mit Service Bus Warteschlangen] eingegebene Name[ servicebus-getstarted] (nicht den Namen des Servicebus). Stellen Sie sicher, dass die Warteschlange verwenden, die mit der Ausgabe-Stream Analytics verbunden.
    - **Service Bus Verbindung**: Hinzufügen **einer Verbindungszeichenfolge**. Um die Verbindungszeichenfolge zu suchen, besuchen Sie das Verwaltungsportal, wählen Sie **Service Bus**, Servicebus erstellten und **Informationen** am unteren Bildschirmrand. Stellen Sie sicher, dass Sie auf dem Hauptbildschirm auf dieser Seite. Kopieren Sie und fügen Sie die Verbindungszeichenfolge. Jede Verbindung eingeben können.
    
        ![Screenshot der Bus-Verbindung](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: Wählen Sie **Verwalten**


3. Klicken Sie auf **Erstellen**

## <a name="writing-to-redis-cache"></a>Redis Cache Schreiben
Wir haben jetzt eine Azure-Funktion erstellt, die aus einem Service Bus-Warteschlange liest. Müssen lediglich unsere Funktion Redis Cache diese Daten schreiben. 

1. Wählen Sie die neu erstellte **ServiceBusQueueTrigger**und klicken Sie **Appeinstellungen** in der oberen rechten Ecke. Wählen Sie **App Service Einstellungen > Settings > Einstellungen**

2. Erstellen Sie im Abschnitt Connection Strings im Abschnitt **Name** einen Namen. Fügen Sie primäre Verbindungszeichenfolge **Erstellen Redis Cache** Schritt in Abschnitt **Wert** gefunden. Wählen Sie **Benutzerdefiniert** , **SQL Datenbank laut**.

3. Klicken Sie auf **Speichern** .

    ![Screenshot der Bus-Verbindung](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Nun zurück zur App Service Settings und wählen **Tools > App Service-Editor (Vorschau) > auf > Gehen Sie**.

    ![Screenshot der Bus-Verbindung](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Erstellen Sie in einem Editor Ihrer Wahl eine JSON-Datei namens **project.json** mit folgenden und auf Ihrer lokalen Festplatte speichern.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Hochladen Sie in das Stammverzeichnis Ihrer Funktion (nicht WWWROOT). Sie sollten sehen eine Datei namens **project.lock.json** automatisch angezeigt, die Nuget-Pakete "StackExchange.Redis" und "Newtonsoft.Json" wurden importiert.

7. Ersetzen Sie in der Datei **run.csx** bereits generierten Code durch den folgenden Code. Ersetzen Sie in der Funktion LazyConnection "CONN NAME" durch den Namen in Schritt 2 der **Daten in den Cache Redis**erstellten.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Stream Analytics-Auftrag starten

1. Starten Sie die Anwendung telcodatagen.exe. Die Syntax lautet wie folgt:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Stream Analytics Auftrag Blatt im Portal **Starten** am oberen Rand der Seite klicken.

    ![Screenshot der Auftrag starten](./media/stream-analytics-functions-redis/starting-job.png)

3. Das Blatt **Auftrag starten** wählen Sie **jetzt** , und klicken Sie auf die Schaltfläche **Start** am unteren Bildschirmrand. Job Status ändert ab und nach einigen Änderungen zu ausgeführt.
 
    ![Screenshot der Start jobauswahl](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Lösung und Überprüfen der Ergebnisse
Zurück zur Seite **ServiceBusQueueTrigger** , sollte Sie Log-Anweisungen angezeigt werden. Diese Protokolle anzeigen, dass Sie etwas aus der Service Bus-Warteschlange in der Datenbank speichern und mit der Zeit als Schlüssel abgerufen.

Um sicherzustellen, dass Ihre Daten im Cache Redis, gehen auf Ihrer Seite Redis Cache in das neue Portal (siehe [Erstellen einer Azure Redis Cache](#Create-an-Azure-Redis-Cache) Schritt), und wählen Sie Konsole.

Sie können jetzt Redis Befehle zu bestätigen, dass Daten tatsächlich in den Cache schreiben.

![Screenshot der Konsole Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Nächste Schritte
Wir freuen uns über die neuen Leistungsmerkmale Azure Funktionen Stream Analytics kann zusammen und hoffen, dass dieses neue Sie entsperrt. Haben Sie Feedback auf Weiter zögern der [Azure-UserVoice-Website](https://feedback.azure.com/forums/270577-stream-analytics).

Wenn Sie neue Microsoft Azure sind, laden wir Sie ausprobieren, indem Sie sich für ein [kostenloses Testabo Azure](https://azure.microsoft.com/pricing/free-trial/). Wenn Sie Stream Analytics vertraut sind, dann laden wir Sie an [Ihrem ersten Stream Analytics-Auftrag erstellen](stream-analytics-create-a-job.md).

Wenn Sie benötigen Hilfe oder Fragen haben, Buchen auf [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) oder [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) Foren. 

Finden Sie unter den folgenden Ressourcen:

- [Azure Funktionen-Entwicklerreferenz](../azure-functions/functions-reference.md)
- [Azure Funktionen C#-Entwicklerreferenz](../azure-functions/functions-reference-csharp.md)
- [Azure Funktionen F#-Entwicklerreferenz](../azure-functions/functions-reference-fsharp.md)
- [Azure Funktionen NodeJS-Entwicklerreferenz](../azure-functions/functions-reference.md)
- [Azure Funktionen Trigger und Bindung](../azure-functions/functions-triggers-bindings.md)
- [Azure Redis Cache überwachen](../redis-cache/cache-how-to-monitor.md)

Führen Sie auf dem laufenden über die neuesten News und Features bleiben [@AzureStreaming](https://twitter.com/AzureStreaming) auf Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
