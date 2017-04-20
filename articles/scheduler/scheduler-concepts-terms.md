<properties
 pageTitle="Planer Konzepte und Begriffe Entitäten | Microsoft Azure"
 description="Azure Scheduler Konzepte, Terminologie und Entitätshierarchie Aufträge und auftragssammlungen.  Zeigt ein umfassendes Beispiel eines geplanten Auftrags."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Taskplaner (Konzepte), Terminologie + Entitätshierarchie

## <a name="scheduler-entity-hierarchy"></a>Planer Entitätshierarchie

Die folgende Tabelle beschreibt die wichtigsten Ressourcen verfügbar gemacht oder von der Planer-API verwendet:

|Ressource | Beschreibung |
|---|---|
|**Job-Auflistung**|Job-Auflistung enthält eine Gruppe von Aufträgen und verwaltet Einstellungen, Quoten und drosseln, die Projekte in der Auflistung verwendet werden. Job-Auflistung wird erstellt, indem ein Abonnementbesitzer und Gruppen gemeinsam an Verwendung oder Anwendung auf. Es ist auf einen Bereich eingeschränkt. Sie können auch die Durchsetzung von Kontingenten beschränkt die Verwendung aller Projekte in der Auflistung. Kontingente sind MaxJobs und MaxRecurrence.|
|**Auftrag**|Ein Auftrag definiert eine wiederkehrende Aktion, einfache oder komplexe Strategien für die Ausführung. Aktionen können HTTP, Speicherwarteschlange, Service Bus-Warteschlange oder Serviceanfragen Bus Thema enthalten.|
|**Historie**|Eine Historie stellt Informationen zur Ausführung eines Auftrags. Erfolg oder Fehler sowie alle Details der Antwort enthält.|

## <a name="scheduler-entity-management"></a>Planer Entität management

Auf einer hohen Ebene stellen der Planer und Servicemanagement-API die folgenden Ressourcen:

|Funktion|Beschreibung und die URI-Adresse|
|---|---|
|**Projektmanagement-Auflistung**|GET, PUT und DELETE Unterstützung zum Erstellen und Ändern von Job-Sammlungen und die enthaltenen Projekte. Job-Auflistung ist ein Container für Aufträge und Karten Kontingente und freigegebenen. Beispiele für Kontingente beschriebenen, sind maximal Aufträge und kleinsten Serienintervall. <p>Einfügen und löschen:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>Erhalten:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Projektmanagement**|GET, PUT, POST PATCH und Unterstützung zum Erstellen und bearbeiten Aufträge löschen. Alle Aufträge müssen eine Projekt-Auflistung, die bereits vorhanden ist, gehören, es gibt also keine implizites erstellen. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Projektmanagement-Verlauf**|Support für 60 Tage Ausführungsverlauf Auftrag Auftrag verstrichene Zeit und Arbeit Ergebnisse abrufen. Unterstützt Query String Parameter Filtern nach und Status. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Stellentypen

Es gibt mehrere Arten von Einzelvorgängen: http-Aufträge (auch HTTPS, die SSL unterstützen), Warteschlangenaufträge Storage Service Bus Warteschlangenaufträge und Service Bus Thema Aufträge. HTTP-Projekte sind ideal, wenn Sie einen Endpunkt einer vorhandenen Arbeitslast oder Service. Warteschlangenaufträge Speicher können Sie Nachrichten an speicherwarteschlangen die Aufträge sind also ideal für Arbeitslasten, die speicherwarteschlangen verwenden. Ebenso stellen Service Bus für Arbeitslasten, die Service Bus Warteschlangen und Themen verwenden.

## <a name="the-job-entity-in-detail"></a>Die Entität "Job" im detail

Auf der Basisebene hat ein geplanter Auftrag mehrere Teile:

- Die Aktion bei Zeitgeber Auftrag  

- (Optional) Die Zeit zum Ausführen des Auftrags  

- (Optional) Wann und wie oft den Auftrag wiederholt  

- (Optional) Eine Aktion auslöst, wenn die primäre Aktion fehlschlägt  

Intern enthält ein geplanter Auftrag auch vom System bereitgestellten Daten wie die nächste geplante Ausführungszeit.

Der folgende Code bietet ein umfassendes Beispiel eines geplanten Auftrags. Angaben in den folgenden Abschnitten.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Wie im obigen Beispiel Objektaufrufplaner, hat eine Auftragsdefinition mehreren Teilen:

- Startzeit ("StartTime")  

- Aktion (""), einschließlich Fehleraktion ("ErrorAction")

- Wiederholung ("Wiederholung")  

- Zustand ("")  

- Status ("Status")  

- Wiederholen Sie Richtlinien ("RetryPolicy")  

Betrachten Sie jede dieser im Detail:

## <a name="starttime"></a>Startzeit

"StartTime" die Startzeit und ermöglicht es dem Aufrufer einen Zeitzonenoffset in der Leitung im [ISO 8601-Format](http://en.wikipedia.org/wiki/ISO_8601)an.

## <a name="action-and-erroraction"></a>Aktion und errorAction

"Aktion" wird bei jedem Vorkommen aufgerufen und beschreibt einen Dienstaufruf. Die Aktion ist was angegebenen Zeitplan ausgeführt wird. Scheduler unterstützt HTTP, Speicherwarteschlange Service Bus-Topic und Service Bus Warteschlange Aktionen.

Die Aktion im obigen Beispiel ist eine HTTP-Aktion. Im folgenden ist ein Beispiel einer Storage Queue-Aktion:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Es folgt ein Beispiel einer Aktion Service Bus Thema.

  "Aktion": {"Typ": "ServiceBusTopic", "ServiceBusTopicMessage": {"TopicPath": "t1"  
      "Namespace": "MySBNamespace", "TransportType": "NetMessaging" / / NetMessaging oder AMQP "Authentifizierung": {"SasKeyName": "QPolicy", "Typ": "SharedAccessKey"}, "Meldung": "Eine Nachricht", "BrokeredMessageProperties": {}, "CustomMessageProperties": {"Appname": "FromScheduler"}},}

Es folgt ein Beispiel einer Aktion Service Bus Warteschlange:


  "Aktion": {"ServiceBusQueueMessage": {"QueueName": "q1"  
      "Namespace": "MySBNamespace", "TransportType": "NetMessaging" / / NetMessaging oder AMQP "Authentifizierung": {  
        "SasKeyName": "QPolicy", "Typ": "SharedAccessKey"}, "Meldung": "Eine Nachricht"  
      "BrokeredMessageProperties": {}, "CustomMessageProperties": {"Appname": "FromScheduler"}}, "Typ": "ServiceBusQueue"}

"ErrorAction" ist der Fehlerhandler die Aktion wird aufgerufen, wenn die primäre Aktion fehlschlägt. Verwenden Sie diese Variable anliegenden Fehlerbehandlung oder senden eine Benachrichtigung. Erreicht einen sekundären Endpunkt im Fall, dass die primäre nicht verfügbar (z.B. bei einer Katastrophe am Standort des Endpunkts) verwendet werden, oder benachrichtigen Sie eine Fehlerbehandlung Endpunkt verwendet werden. Wie die primäre Aktion kann die Fehleraktion einfachen oder zusammengesetzten Logik auf andere Aktionen. Informationen zum Erstellen eines SAS-Tokens finden Sie unter [Erstellen](https://msdn.microsoft.com/library/azure/jj721951.aspx)und Verwenden einer SAS.

## <a name="recurrence"></a>Serie

Serie besteht aus mehreren Teilen:

- Häufigkeit: Eines Minute, Stunde, Tag, Woche, Monat, Jahr  

- Intervall: Intervall mit der angegebenen Häufigkeit der Serie  

- Vorgegebene Zeitplan: Minuten, Stunden, Wochentage, Monate und Monthdays der Serie angeben  

- Anzahl: Anzahl Vorkommen  

- Ende: führt keine Aufträge nach der angegebenen Endzeit  

Ein wiederkehrendes ist ein wiederkehrendes Objekt in der JSON-Definition angegeben hat. Anzahl und Endzeit angegeben werden, wird die Abschluss-Regel, die zuerst eintritt berücksichtigt.

## <a name="state"></a>Zustand

Der Status des Auftrags ist einer von vier Werten: aktiviert, deaktiviert, abgeschlossen oder fehlgeschlagen. Sie stellen oder PATCH um den aktivierten oder deaktivierten Status aktualisieren. Wenn ein Auftrag abgeschlossen oder fehlerhaft, d. h. einen endgültigen Status aktualisiert werden kann (obwohl noch gelöscht werden können). Ein Beispiel für die State-Eigenschaft lautet wie folgt:


        "state": "disabled", // enabled, disabled, completed, or faulted
Abgeschlossene und fehlerhafte Aufträge werden nach 60 Tagen gelöscht.

## <a name="status"></a>Status

Sobald ein Job Scheduler gestartet wurde, werden Informationen über den aktuellen Status des Auftrags zurückgegeben. Dieses Objekt kann nicht vom Benutzer festgelegt werden, wird vom System festgelegt. Es ist im Auftragsobjekt (anstatt eine separate verknüpfte Ressource) enthalten, so dass man leicht den Status eines Auftrags erhalten kann.

Auftragsstatus enthält der vorherigen Ausführung (falls vorhanden) und die Uhrzeit der nächsten geplanten Ausführung (in Bearbeitung befindlichen Aufträge) die Ausführungsanzahl des Auftrags.

## <a name="retrypolicy"></a>retryPolicy

Schlägt ein Job Scheduler kann an eine wiederholungsrichtlinie, um zu bestimmen, ob und wie die Aktion wiederholt. Dies wird vom **RetryType** -Objekt bestimmt – es soll **keine** ist keine wiederholungsrichtlinie wie oben dargestellt. Festlegen Sie **, die** eine Wiederholung Politik.

Eine Wiederholung Richtlinie zwei zusätzliche Optionen angegeben werden: ein Wiederholungsintervall (**RetryInterval**) und die Anzahl der Wiederholungsversuche (**RetryCount**).

Mit dem Objekt **RetryInterval** angegebene Wiederholungsintervall ist das Intervall zwischen Wiederholungsversuchen. Der Standardwert ist 30 Sekunden und der Maximalwert beträgt 18 Monate konfigurierbare sein Mindestwert beträgt 15 Sekunden. Aufträge in freien Stelle Sammlungen haben einen konfigurierbaren Mindestwert 1 Stunde.  Es wird im ISO 8601-Format definiert. Ebenso wird der Wert die Anzahl der Wiederholungsversuche mit dem **RetryCount** Objekt angegeben. Es ist die Anzahl der Wiederholung des Vorgangs. Der Standardwert ist 4, und der Maximalwert beträgt 20\. beide **RetryInterval** und **RetryCount** sind optional. Sie erhalten Ihre setzen **RetryType** **festgelegt** wird und keine Werte explizit angegeben werden.

## <a name="see-also"></a>Siehe auch

 [Was ist der Taskplaner?](scheduler-intro.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Wie Sie komplexe Abfolgepläne und erweiterte Serie mit Azure Scheduler erstellen](scheduler-advanced-complexity.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
