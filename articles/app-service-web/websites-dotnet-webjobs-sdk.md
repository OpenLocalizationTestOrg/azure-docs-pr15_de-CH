<properties 
    pageTitle="Was ist der Azure Webaufträge SDK" 
    description="Eine Einführung in Azure Webaufträge SDK. Erläutert das SDK, normalen Szenarien eignet sich für und code-Beispiele." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Was ist der Azure Webaufträge SDK

## <a id="overview"></a>Übersicht

Dieser Artikel erläutert das WebJobs SDK, überprüft einige häufige Szenarien eignet sich für und gibt einen Überblick darüber, wie Sie es im Code verwenden.

[Webaufträge](websites-webjobs-resources.md) ist ein Feature von Azure App Service, der Sie ein Programm oder Skript in demselben Kontext als WebApp, API-app oder mobile Anwendung ausführen können. [Webaufträge SDK](websites-webjobs-resources.md) soll die Code für häufige Aufgaben schreiben, ein Webauftrag können, wie Bild-, Verarbeitung in die Warteschlange, RSS-Aggregation, Wartung und e-Mails zu vereinfachen. Das WebJobs SDK enthält Funktionen für die Arbeit mit Azure Service Bus für Planungsaufgaben und Fehlerbehandlung und viele andere Szenarien. Darüber hinaus soll es erweiterbar. [Webaufträge SDK ist open Source](https://github.com/Azure/azure-webjobs-sdk/), und [Öffnen Sie Quell-Repository für Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Das WebJobs SDK enthält die folgenden Komponenten:

* **NuGet-Pakete**. Einer Visual Studio-Konsolenanwendungsprojekt hinzufügen NuGet-Pakete bieten einen Rahmen, den Code durch das ergänzen der Verfahren WebJobs SDK verwendet.
  
* **Dashboard**. WebJobs SDK in Azure App Service enthalten und bietet umfassende Überwachung und Diagnose für Programme, die NuGet-Pakete verwenden. Sie müssen Code schreiben, um diese Überwachung und Diagnose-Funktionen verwenden.

## <a id="scenarios"></a>Szenarien

Hier sind einige normalen Szenarien leichter Azure Webaufträge SDK verarbeitet werden können:

* Image-Verarbeitung oder andere CPU-Intensive Vorgänge. Ein gemeinsames Merkmal von Websites ist die Möglichkeit, Bilder oder Videos. Oft möchten Sie den Inhalt bearbeiten, nachdem sie hochgeladen, aber der Benutzer warten Sie dies möchten.

* Verarbeitung von Arbeitswarteschlangen. Eine gängige Methode für Web-Frontend für die Kommunikation mit einem Back-End-Dienst werden Warteschlangen verwendet. Die Website muss Arbeit, legt eine Nachricht in einer Warteschlange. Back-End-Dienst Nachrichten aus der Warteschlange abruft und funktioniert. Sie können Warteschlangen für Image-Verarbeitung: z. B. nachdem der Benutzer eine Reihe von Dateien hochgeladen, setzen die Dateinamen in einer warteschlangennachricht vom Back-End-Verarbeitung entnommen werden. Oder Sie können Warteschlangen um Website Reaktionsfähigkeit zu verbessern. Z. B. anstatt direkt in einer SQL-Datenbank, Schreiben an eine Warteschlange Raten Sie fertig sind, und lassen Sie die Back-End-Dienst Handle Wartezeiten relationale Datenbank arbeiten. Beispielsweise der Warteschlange mit Bild finden Sie im [Lernprogramm Webaufträge SDK beginnen](websites-dotnet-webjobs-sdk-get-started.md).

* RSS-Aggregation. Haben Sie eine Website, die eine Liste von RSS-Feeds verwaltet, können Sie in allen Artikeln von Feeds im Hintergrund ziehen.

* Wartung oder Aggregieren Protokolldateien bereinigt.  Möglicherweise Protokolldateien erstellt werden, indem mehrere Standorte oder für separate Zeitspannen um Sie Analysis-Aufträge ausgeführt werden sollen. Oder Sie möchten alte Logdateien bereinigen wöchentliche Ausführung ein Tasks planen.

* Eindringen in Azure-Tabellen. Sie können Dateien und Blobs und möchten sie analysieren und die Daten werden in Tabellen gespeichert. Die Funktion Eindringen konnte viele Zeilen (in einigen Fällen Millionen) schreiben und WebJobs SDK ermöglicht dies problemlos implementieren. Das SDK enthält auch die Echtzeit-Überwachung von Indikatoren wie die Anzahl der Zeilen in der Tabelle geschrieben.

* Andere lang andauernde Vorgänge in einem Hintergrundthread, wie [e-Mails](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure)ausgeführt. 

* Alle Aufgaben, die nach einem Zeitplan wie Vorgang einer Sicherung täglich ausgeführt werden soll.

In vielen Szenarien möchten Sie skalieren eine Web app, auf mehrere VMs, die mehrere Webaufträge gleichzeitig ausführen möchten. In einigen Szenarien könnte dies dieselben Daten mehrmals verarbeitet, aber dies ist kein Problem, wenn Sie integrierte Warteschlange, Blob und Service Bus Trigger WebJobs SDK verwenden. Das SDK wird sichergestellt, dass Funktionen nur einmal für jede Nachricht oder Blob verarbeitet werden.

Das WebJobs SDK vereinfacht auch allgemeine Szenarien für die Fehlerbehandlung behandeln. Sie können Alarme einrichten, Benachrichtigungen senden, wenn eine Funktion fehlschlägt und Timeouts festgelegt werden, kann sodass eine Funktion automatisch abgebrochen wird, wenn er nicht innerhalb einer bestimmten Frist abgeschlossen.

## <a id="code"></a>Code-Beispiele

Der Code für normale Aufgaben, die mit Azure-Speicher ist einfach. In der Konsolenanwendung `Main` -Methode erstellen eine `JobHost` Objekt, das Koordinaten der Aufrufe von Methoden schreiben. Das WebJobs SDK-Framework weiß beim Aufrufen die Methoden und welche Parameterwerte verwendet auf WebJobs SDK Attribute Sie verwenden. Das SDK enthält *Trigger* , die angeben, welche Zustände aufzurufende Funktion und *Ordner* , die angeben, wie Informationen in und aus Parameter abgerufen.

[QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) -Attribut wird z. B. eine Funktion, die aufgerufen werden, wenn eine in einer Warteschlange Meldung und ist das Nachrichtenformat JSON ein Bytearray oder einen benutzerdefinierten Typ, die Meldung automatisch deserialisiert. Das [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) -Attribut löst einen Prozess aus, wenn ein neues Blob in Azure Storage-Konto erstellt wird.

Hier ist ein einfaches Programm, das eine Warteschlange abruft und erstellt ein Blob für jede empfangene warteschlangennachricht in:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

Die `JobHost` Objekt ist ein Container für eine Gruppe von Hintergrundfunktionen. Die `JobHost` -Objekt überwacht die Funktionen überwacht Ereignisse, die ausgelöst werden, und führt die Funktionen Wenn Ereignisse auftreten. Sie rufen eine `JobHost` Methode an, ob den Container Prozess auf den aktuellen Thread oder einem Hintergrundthread ausführen soll. Im Beispiel die `RunAndBlock` -Methode führt den Prozess ständig auf dem aktuellen Thread.

Da die `ProcessQueueMessage` -Methode in diesem Beispiel ist ein `QueueTrigger` -Attribut den Trigger für die Funktion einer neuen warteschlangennachricht ist. Die `JobHost` Objekt überwacht neue Warteschlange Nachrichten in der angegebenen Warteschlange (in diesem Beispiel "Webjobsqueue") und wenn eine gefunden wird, ruft `ProcessQueueMessage`. 

Die `QueueTrigger` Attribut bindet die `inputText` Parameter an den Wert der warteschlangennachricht. Und die `Blob` Attribut bindet ein `TextWriter` Objekt ein Blob mit dem Namen "Blobname" in einem Container mit dem Namen "Containername".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Die Funktion verwendet dann diese Parameter auf den Wert von Message Queue in das Blob schreiben:

        writer.WriteLine(inputText);

Trigger und Binder Funktionen des WebJobs SDK vereinfachen erheblich Code schreiben müssen. Die Stücklistenebene erforderliche Warteschlangen, Blobs oder Dateien verarbeiten oder geplante Tasks starten Sie WebJobs SDK Rahmen erfolgt. Beispielsweise die Grundstruktur Warteschlangen noch öffnet nicht der Warteschlange Nachrichten die Warteschlange liest, löscht die Warteschlange Nachrichten Wenn Verarbeitung abgeschlossen ist, erstellt der Blob-Container nicht vorhanden, an Blobs geschrieben.

Das folgende Codebeispiel zeigt eine Vielzahl von Triggern in einer Webauftrags: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, und `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Planung

Die `TimerTrigger` Attribut können Sie Trigger-Funktionen nach einem Zeitplan ausgeführt. Planen einer Webauftrags Azure oder Zeitplan einzelner Funktionen ein Webauftrag mit WebJobs SDK insgesamt `TimerTrigger`. Hier ist ein Codebeispiel.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Weitere Codebeispiele finden Sie unter [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) im Repository Azure Webaufträge Sdk Erweiterungen auf GitHub.com.

## <a name="extensibility"></a>Erweiterbarkeit

Auf integrierte Funktionen – du nicht können WebJobs SDK Sie benutzerdefinierte Trigger und Binder.  Beispielsweise können Sie Trigger für Cacheereignisse und regelmäßige Termine schreiben. [Öffnen Sie Quell-Repository](https://github.com/Azure/azure-webjobs-sdk-extensions) enthält ein [ausführlicher Leitfaden Webaufträge SDK Erweiterbarkeit](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) und Beispiel helfen Sie eigene Trigger und Ordner geschrieben.

## <a id="workerrole"></a>Mithilfe des SDK Webaufträge außerhalb Webaufträge

Ein Programm, das WebJobs SDK ist eine standard-Konsolenanwendung und überall ausführen – es muss ein Webauftrag ausgeführt. Sie können das Programm lokal auf dem Entwicklungscomputer testen und Produktion ausführen in einem Clouddienst Worker-Rolle oder einen Windows-Dienst eines Umgebungen bevorzugen. 

Das Dashboard ist jedoch nur als Erweiterung einer Azure App Service Web App. Ggf. außerhalb einer Webauftrags und verwenden weiterhin das Dashboard können Sie konfigurieren eine Web app verwenden das gleiche Speicherkonto, das auf die Verbindungszeichenfolge Webaufträge SDK Dashboard und Web app Webaufträge Dashboard zeigt Daten über die funktionsausführung aus Ihrem Programm an anderer Stelle ausgeführt wird. URL https://*{Webappname}*.scm.azurewebsites.net/azurejobs/#/functions können auf das Dashboard zugreifen. Für Weitere Informationen sehen [ein Dashboard für lokale Entwicklung Webaufträge SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx)im Blogbeitrag einen alte Namen für die Verbindungszeichenfolge angezeigt. 

## <a id="nostorage"></a>Dashboard-Funktionen

Das WebJobs SDK bietet verschiedene Vorteile, auch wenn Sie WebJobs SDK Trigger oder Ordner verwenden:

* Sie können das Dashboard-Funktionen aufrufen.
* Sie können Funktionen im Dashboard wiedergeben.
* Protokolle im Dashboard zu bestimmten Webauftrags (Anwendungsprotokolle, mithilfe von Console.Out Console.Error verfolgen, usw.) oder mit bestimmten Funktionsaufruf, die sie erzeugt verknüpft anzeigen (Protokollen mithilfe einer `TextWriter` -Objekt, das SDK als Parameter an die Funktion übergeben). 

Weitere Informationen finden Sie unter [Funktion manuell aufrufen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) und [Protokolle schreiben](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Nächste Schritte

Weitere Informationen zum WebJobs-SDK finden Sie unter [Azure Webaufträge empfohlene Ressourcen](http://go.microsoft.com/fwlink/?linkid=390226).

Informationen zu den neuesten verbesserte WebJobs SDK finden Sie unter [Release Notes](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
