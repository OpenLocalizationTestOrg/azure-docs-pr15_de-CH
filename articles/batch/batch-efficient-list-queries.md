<properties
    pageTitle="Effiziente Liste Abfragen in Azure Batch | Microsoft Azure"
    description="Leistungssteigerung durch Filtern Abfragen beim Anfordern von Informationen zur Stapelverarbeitung Ressourcen-Pools, Projekte, Aufgaben und compute-Knoten."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="query-the-azure-batch-service-efficiently"></a>Azure Batch-Dienst effizient Abfragen

Hier erfahren Sie wie Azure Batch die Leistung der Anwendung erhöhen, indem weniger Daten vom Dienst zurückgegeben werden, wenn Sie Projekte, Aufgaben und-Knoten mit [Batch .NET Compute] [ api_net] Bibliothek.

Fast alle Stapel Anträge müssen einige Überwachung oder andere ausführen, mit dem den Batch-Dienst häufig in regelmäßigen Abständen abgefragt. Beispielsweise müssen Sie bestimmen, ob die Warteschlange Aufgaben verbleiben in einem Auftrag, Daten für jede Aufgabe im Auftrag abrufen. Ermitteln Sie den Status der Knoten im Pool muss auf jedem Knoten im Pool Daten abgerufen werden. Erläutert, wie solche Abfragen auf effizienteste Weise ausführen.

## <a name="meet-the-detaillevel"></a>Die DetailLevel entsprechen

Wie Projekte, Aufgaben und Compute-Knoten können Batch Anwendung Produktion Tausenden nummerieren. Beim Anfordern von Informationen über diese Ressourcen muss eine große Datenmenge "die Leitung von Batch-Dienst Ihrer Anwendung jede Abfrage schneiden". Beschränken Sie die Anzahl der Elemente und die Art der Informationen, die von einer Abfrage zurückgegeben wird, können Sie die Geschwindigkeit der Abfragen und damit die Leistung der Anwendung erhöhen.

Diese [Stapelverarbeitung .NET] [ api_net] API-Codeausschnitt listet *jede* Aufgabe, die ein Projekt und *Alle* Eigenschaften der einzelnen Aufgaben zugeordnet ist:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Sie können effizienter Listenabfrage jedoch Anwendung "Detail" der Abfrage ausführen. Hierzu geben einen [ODATADetailLevel] [ odata] gegen [JobOperations.ListTasks] [ net_list_tasks] Methode. Dieser Ausschnitt gibt nur ID, Befehlszeile und Compute Knoteneigenschaften Informationen der abgeschlossenen Aufgaben:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

In diesem Beispiel gibt es Tausende von Aufgaben im Auftrag, die Ergebnisse der zweiten Abfrage normalerweise kehren viel schneller als das erste. Weitere Informationen über ODATADetailLevel bei Artikeln mit der Stapelverarbeitung .NET API ist enthalten [unten](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Wir empfehlen, dass Sie *immer* ein ODATADetailLevel-Objekt auf die .NET API Liste Aufrufe maximale Effizienz und Leistung der Anwendung bereitstellen. Eine Detailebene angeben, können Sie Batch Service Reaktionszeiten verringern, die Netzwerklast und Minimieren der Speicherverwendung von Clientanwendungen.

## <a name="filter-select-and-expand"></a>Filtern, wählen und erweitern

[Batch.NET] [ api_net] und [Batch REST] [ api_rest] APIs bieten die Möglichkeit, die Anzahl der Elemente in einer Liste zurückgegeben werden, sowie die Menge für jeden zurückgegebenen Informationen. Dazu beim Liste Abfragen **Filtern**, **Wählen**und **Erweitern Sie Zeichenfolgen** angeben.

### <a name="filter"></a>Filter
Die Filterzeichenfolge ist ein Ausdruck, der die Anzahl der Elemente verringert, die zurückgegeben werden. Z. B. ausgeführten Aufgaben für ein Projekt aus, oder Listen Sie nur Computeknoten Aufgaben ausgeführt werden.

- Die Filterzeichenfolge besteht aus einem oder mehreren Ausdrücken mit einem Ausdruck, der einen Eigenschaftennamen, einem Operator und einem Wert besteht. Die Eigenschaften, die angegeben werden können sind für jeden Entitätstyp Abfragen, wie Operatoren, die für jede Eigenschaft unterstützt werden.
- Mehrere Ausdrücke können mit logischen Operatoren kombiniert `and` und `or`.
- In diesem Beispiel Filterlisten Zeichenfolge nur die Ausführung "render" Aufgaben: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Wählen Sie
Die ausgewählte Zeichenfolge begrenzt die Eigenschaftenwerte für jedes Element zurückgegeben werden. Geben Sie eine Liste von Eigenschaftennamen und nur die Eigenschaftswerte für die Elemente in den Abfrageergebnissen zurückgegeben werden.

- Wählen Sie Zeichenfolge besteht aus einer durch Trennzeichen getrennte Liste von Eigenschaftennamen. Sie können die Eigenschaften für den Entitätstyp werden angeben.
- Dieses Beispiel select Zeichenfolge gibt an, dass nur drei Werte für jede Aufgabe zurückgegeben werden soll: `id, state, stateTransitionTime`.

### <a name="expand"></a>Erweitern Sie
Wert reduziert die Anzahl der API-Aufrufe, die erforderlich sind, um bestimmte Informationen zu erhalten. Wenn Sie einen Wert verwenden, kann Informationen über jedes Element abgerufen werden mit einer einzigen API-Aufruf. Als vorher die Liste der Entitäten, fordert Informationen für jedes Element in der Liste können Sie einen Wert der gleichen Informationen in einem einzigen API-Aufruf. Weniger API-Aufrufe bedeutet eine bessere Leistung.

- Wie die Option Zeichenfolge Wert steuert, ob bestimmte Daten in Abfrageergebnissen Liste enthalten ist.
- Wert wird nur unterstützt, wenn sie in der Liste Aufträge, Projektpläne, Aufgaben und Pools verwendet wird. Derzeit unterstützt nur statistische Informationen.
- Alle Eigenschaften müssen keine select Zeichenfolge angegeben, der erweitern Zeichenfolge *muss* verwendet werden, statistische Informationen. Wenn mit einer Teilmenge von Eigenschaften, dann erhalten `stats` die Option Zeichenfolge angegeben werden und die Erweiterungszeichenfolge muss nicht angegeben werden.
- In diesem Beispiel erweitern Zeichenfolge gibt an, dass statistische Informationen für jedes Element in der Liste zurückgegeben werden soll: `stats`.

> [AZURE.NOTE] Beim Erstellen eines der drei Abfragetypen Zeichenfolge filtern, auswählen, und erweitern Sie Sie müssen sicherstellen, dass die Eigenschaftsnamen und Fall ihrer Gegenstücke REST API Element entsprechen. Beispielsweise müssen bei .NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) Klasse **Status** **Status**soll, obwohl die Eigenschaft .NET [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state)ist. Finden Sie in den folgenden Tabellen eigenschaftenzuordnungen zwischen .NET und REST-APIs.

### <a name="rules-for-filter-select-and-expand-strings"></a>Regeln für Filter, wählen und erweitern Sie Zeichenfolgen

- Eigenschaften Namen Filter wählen und erweitern Sie Zeichenfolgen erscheinen wie im [Batch VERBLEIBENDEN] [ api_rest] API - auch wenn [Batch.NET] verwenden[ api_net] oder Batch SDKs.
- Alle Eigenschaft Groß-und Kleinschreibung, aber Werte sind Groß-und Kleinschreibung.
- Datum/Uhrzeit-Zeichenfolgen kann eines von zwei Formaten, und muss mit `DateTime`.

  - Beispiel für W3C DTF-Format:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - RFC 1123 Format-Beispiel:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Boolesche Zeichenfolgen sind entweder `true` oder `false`.
- Wenn eine ungültige Eigenschaft oder einen Operator angegeben ist, eine `400 (Bad Request)` Fehler.

## <a name="efficient-querying-in-batch-net"></a>Effiziente Abfragen in Batch .NET

In der [Stapelverarbeitung .NET] [ api_net] -API, [ODATADetailLevel] [ odata] Klasse zum Filter angeben, wählen und erweitern Sie Zeichenfolgen auf Vorgänge auflisten. Die ODataDetailLevel-Klasse hat drei öffentliche Eigenschaften, die im Konstruktor angegeben oder direkt für das Objekt festgelegt werden können. Übergeben dann das ODataDetailLevel-Objekt als Parameter an die verschiedenen Liste Vorgänge wie [ListPools][net_list_pools], [ListJobs][net_list_jobs], und [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Die Anzahl der zurückgegebenen Elemente.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Geben Sie die Eigenschaftswerte jedes Element zurückgegeben werden.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Abrufen von Daten für alle Elemente in einer einzigen API anstatt separate Ruft für jedes Element aufrufen.

Im folgenden Codeausschnitt werden Stapel .NET API effiziente Batch-Dienst für Statistiken für eine bestimmte Gruppe von Pools abgefragt. In diesem Fall hat der Benutzer Batch Test- und Pools. Testpool IDs "Test" vorangestellt und Produktionspool IDs "Prod" vorangestellt. Im Codeausschnitt ist *MyBatchClient* eine ordnungsgemäß initialisierte Instanz der Klasse [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) .

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Eine Instanz von [ODATADetailLevel] [ odata] Option konfiguriert ist und Sie Klauseln können auch an entsprechende Get-Methoden wie [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx)beschränkt die Datenmenge, die zurückgegeben wird.

## <a name="batch-rest-to-net-api-mappings"></a>Batch REST in .NET API-Zuordnung

Eigenschaftennamen Filter wählen und erweitern Sie Zeichenfolgen *müssen* entsprechend ihren Gegenstücken REST API-Namen und Fall. Die Tabelle bieten Zuordnungen zwischen .NET und REST API Gegenstücke.

### <a name="mappings-for-filter-strings"></a>Mappings für Filtern von Zeichenfolgen

- **.NET Methoden**: .NET API-Methoden in dieser Spalte akzeptiert ein [ODATADetailLevel] [ odata] -Objekt als Parameter.
- **REST Liste Anfragen**: in dieser Spalte verknüpft jede REST API Seite enthält eine Tabelle, die die Eigenschaften und die zulässigen Operationen in *Filterzeichenfolgen* angibt. Verwenden Sie diesen Namen und Operationen beim Erstellen einer [ODATADetailLevel.FilterClause] [ odata_filter] Zeichenfolge.

| .NET Methoden | REST Liste Anfragen |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Liste der Zertifikate in einem Konto][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Liste der Dateien, die einem Vorgang zugeordnet][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Den Status der Vorbereitung und Veröffentlichung der Aufgaben für ein Projekt][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Liste der Aufträge in einem Konto][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Listet die Dateien auf einem Knoten][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Liste der Aufgaben eines Auftrags][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Liste der Auftragszeitpläne in einem Konto][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Liste ein Projekt Zeitplan zugeordneten jobs][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Liste der Compute-Knoten in einem pool][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Listen Sie die Pools in einem Konto][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Zuordnung für select-Zeichenfolgen

- **Batch .NET Datentypen**: Batch .NET API-Typen.
- **REST-API-Elemente**: jede Seite in dieser Spalte enthält eine oder mehrere Tabellen mit einer die REST API Eigenschaftennamen für den Typ Liste. Diese Namen werden verwendet, wenn *Wählen Sie* Zeichenfolgen erstellen. Diese denselben Eigenschaftennamen verwenden Sie beim Erstellen einer [ODATADetailLevel.SelectClause] [ odata_select] Zeichenfolge.

| Stapeltypen für .NET | REST-API Entitäten |
|---|---|
| [Zertifikat][net_cert] | [Informationen Sie zu einem Zertifikat][rest_get_cert] |
| [CloudJob][net_job] | [Informationen Sie zu einem Auftrag][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Informationen Sie über einen Auftragszeitplan][rest_get_schedule] |
| [ComputeNode][net_node] | [Informationen Sie zu einem Knoten][rest_get_node] |
| [CloudPool][net_pool] | [Informationen Sie zu einem pool][rest_get_pool] |
| [CloudTask][net_task] | [Informationen Sie zu einem Vorgang][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Beispiel: Erstellen einer Zeichenfolge

Beim Erstellen einer Filterzeichenfolge für [ODATADetailLevel.FilterClause][odata_filter], finden Sie in der Tabelle oben unter "Mappings setzen", suchen die REST-API-Dokumentationsseite in der Liste, die Sie ausführen möchten. Sie finden filterbaren Eigenschaften und die unterstützten Operatoren in der ersten multirow Tabelle auf dieser Seite. Möchten Sie alle Vorgänge, deren Exitcode ungleich NULL ist, wurde z. B. diese Zeile auf [Liste der Vorgänge mit einem Auftrag] abrufen[ rest_list_tasks] gibt den entsprechenden Eigenschaftenzeichenfolge und zulässigen Operatoren:

| Eigenschaft | Zulässige Operationen | Typ |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Daher wäre die Filterzeichenfolge für alle Aufgaben mit einem Exitcode:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Beispiel: Erstellen einer

[ODATADetailLevel.SelectClause]erstellen[odata_select]finden Sie in der Tabelle oben unter "Mappings für Zeichenfolgen auswählen" und navigieren Sie zur Seite REST-API, die den Typ der Entität entspricht, die Sie auflisten. Sie finden auswählbaren Eigenschaften und ihre unterstützten Operatoren in der ersten multirow Tabelle auf dieser Seite. Wenn Sie nur die ID und die Befehlszeile für jede Aufgabe in einer Liste abrufen möchten, z. B. finden Sie diese Zeilen in der entsprechenden Tabelle [Informationen über eine Aufgabe][rest_get_task]:

| Eigenschaft | Typ | Notizen |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Wählen Sie Zeichenfolge auch nur die ID und die Befehlszeile Tasks wäre:

`id, commandLine`

## <a name="code-samples"></a>Code-Beispiele

### <a name="efficient-list-queries-code-sample"></a>Beispielcode für effiziente Liste Abfragen

Auschecken [EfficientListQueries] [ efficient_query_sample] -Beispielprojekt auf GitHub wie effizient Liste Abfragen finden Sie unter Leistung in einer Anwendung beeinträchtigen kann. Dieser C#-Konsolenanwendungsprojekt erstellt und ein Projekt eine große Anzahl von Aufgaben hinzugefügt. Dann führt mehrere Aufrufe an [JobOperations.ListTasks] [ net_list_tasks] -Methode auf und übergibt [ODATADetailLevel] [ odata] Objekte unterschiedliche Eigenschaftenwerte Daten variieren konfiguriert sind. Diese Ausgabe ähnelt der folgenden:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Siehe die verstrichene Zeit, können Sie Antwortzeiten erheblich senken, beschränken Sie die Eigenschaften und die Anzahl der zurückgegebenen Elemente. Finden Sie diese und andere Beispielprojekte [Azure-Batch-Beispiele] [ github_samples] GitHub-Repository.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics-Bibliothek und Code-Beispiel

Finden Sie neben obigen Codebeispiel EfficientListQueries [BatchMetrics] [ batch_metrics] Projekt [Azure-Batch-Beispiele] [ github_samples] GitHub-Repository. Das Beispielprojekt BatchMetrics veranschaulicht, wie effizient Azure Batch Job mit der Batch-API überwachen.

[BatchMetrics] [ batch_metrics] Beispiel enthält ein .NET Klassenbibliotheksprojekt die in eigene Projekte und ein einfaches Befehlszeilenprogramm und veranschaulichen die Verwendung der Bibliothek integrieren können.

Beispielanwendung innerhalb des Projekts veranschaulicht die folgenden Vorgänge:

1. Bestimmte Attribute auswählen, um nur die Eigenschaften herunterladen
2. Filtern nach Status Übergangszeiten nur seit der letzten Abfrage herunterladen

Beispielsweise wird die folgende Methode in der BatchMetrics-Bibliothek. Es gibt eine ODATADetailLevel, die nur angibt, die `id` und `state` Eigenschaften erhalten für die Entitäten abgefragt werden. Es gibt außerdem an, dass nur Elemente, deren Status sich, seit der angegebenen geändert hat `DateTime` Parameter zurückgegeben werden soll.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Nächste Schritte

### <a name="parallel-node-tasks"></a>Aufgaben parallel Knoten

[Maximieren Azure Batch berechnen Ressourcenverbrauch bei gleichzeitiger Knoten Aufgaben](batch-parallel-node-tasks.md) ist einem anderen Artikel über Batch Anwendungsperformance. Einige Arten von Arbeitslasten profitieren auf parallele Aufgaben ausführen größere - aber weniger--compute-Knoten. Das [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) in der Artikel in einem solchen Szenario anzeigen

### <a name="batch-forum"></a>Batch-Forum

[Azure Batch Forum] [ forum] auf MSDN eignet sich Batch und Fragen zu dem Dienst. Gehen Sie für hilfreiche Beiträge "dauerhaft", und stellen Sie Ihre Fragen, wie sie beim Erstellen von Batch-Projektmappen auftreten.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
