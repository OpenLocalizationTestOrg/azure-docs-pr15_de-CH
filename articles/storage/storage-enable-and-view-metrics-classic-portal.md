<properties 
    pageTitle="Speichermetriken in Azure-Portal aktivieren | Microsoft Azure" 
    description="Speichermetriken Blob, Warteschlange, Tabelle und Datei aktivieren" 
    services="storage" 
    documentationCenter="" 
    authors="robinsh" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/03/2016" 
    ms.author="robinsh"/>

# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Aktivieren speichermetriken und Metrikdaten

[AZURE.INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Übersicht

Standardmäßig ist Speichermetriken für Storage Services nicht aktiviert. Sie können mit dem [Azure-Verwaltungsportal](https://manage.windowsazure.com), Windows PowerShell oder programmgesteuert über Speicher-API.

Beim Speichermetriken aktivieren müssen, wählen Sie einen Aufbewahrungszeitraum für Daten: Dieser Zeitraum bestimmt, für wie lange Speicher hält Service Metriken und Gebühren für den Speicherplatz erfordern sie. Normalerweise verwenden Sie eine kürzere Aufbewahrungsdauer für Minute Metriken als stündliche Metriken wegen erheblichen Platzbedarf für Minute Metriken. Wählen Sie einen Aufbewahrungszeitraum, so dass Sie ausreichend Zeit zum Analysieren der Daten und downloaden alle Metriken für Offline-Analyse oder zur Berichterstattung beibehalten möchten. Beachten Sie, dass Sie auch zum Download Metrikdaten Ihres Speicherkontos berechnet.

## <a name="how-to-enable-storage-metrics-using-the-azure-classic-portal"></a>Speicher-Metriken anhand der Azure-Verwaltungsportal aktivieren

In [Azure-Verwaltungsportal](https://manage.windowsazure.com)verwenden Sie die Seite Konfigurieren für ein Speicherkonto Steuerelement Speichermetriken. Für die Überwachung können Sie eine Ebene und eine Aufbewahrungsdauer in Tagen für Blobs, Tabellen und Warteschlangen festlegen. In jedem Fall ist die Ebene eines der folgenden:

- Aus – Keine Metriken gesammelt werden.

- Minimal: Storage Metriken erfasst einen grundlegenden Satz an Messgrößen Ingress-Ausgang, Verfügbarkeit, Latenz und Erfolg Prozentwerte Blob, Tabelle und Warteschlange zusammengefasst werden.

- Verbose-Speichermetriken erfasst eine ganze Reihe von Metriken, die die gleichen Metriken für jeden Speicher API-Vorgang zusätzlich die Service Level-Metriken enthält. Ausführliche Kriterien ermöglichen näher Analyse der Probleme bei Anwendung.

Beachten Sie, dass Azure-Verwaltungsportal nicht gerade in das Speicherkonto Minute Metriken konfigurieren können. Aktivieren Sie minutenmetriken mithilfe von PowerShell oder programmgesteuert.


## <a name="how-to-enable-storage-metrics-using-powershell"></a>Speicher-Metriken mit PowerShell aktivieren

PowerShell können auf dem lokalen Computer Sie Speichermetriken in das Speicherkonto mit Azure PowerShell-Cmdlet Get-AzureStorageServiceMetricsProperty zum Abrufen von aktuellen und Set-AzureStorageServiceMetricsProperty-Cmdlet die aktuellen Einstellungen konfigurieren.

Die Cmdlets, die Speichermetriken steuern verwenden die folgenden Parameter:

- MetricsType mögliche Werte sind Stunden und Minuten.

- ServiceType mögliche Werte sind BLOBs, Warteschlangen und Tabelle.

- MetricsLevel mögliche Werte sind keine (entspricht der Azure-Verwaltungsportal Off) Service (entspricht dem in der Azure-Verwaltungsportal) und ServiceAndApi (Verbose in Azure-Verwaltungsportal entspricht).

Z. B. switches Folgendes Minute Kennzahlen für BLOB-Dienst in Ihrem Standard-Speicherkonto die Aufbewahrungsdauer auf fünf Tage:

`Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`

Der folgende Befehl ruft die aktuelle stündlich Metriken Ebene und Aufbewahrung Tage für BLOB-Dienst in Ihrem Standard-Speicherkonto:

`Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob`

Informationen zum Konfigurieren der Azure-PowerShell-Cmdlets von Azure-Abonnement und Auswählen von Speicher Standardkonto verwenden, finden Sie unter: [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Speicher-Metriken programmgesteuert aktivieren

Der folgende C#-Codeausschnitt zeigt, wie Metriken und Protokollierung für den BLOB-Dienst mit der Speicher-Clientbibliothek für .NET:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Enable Storage Analytics logging and set retention policy to 10 days. 
    ServiceProperties properties = new ServiceProperties();
    properties.Logging.LoggingOperations = LoggingOperations.All;
    properties.Logging.RetentionDays = 10;
    properties.Logging.Version = "1.0";

    // Configure service properties for metrics. Both metrics and logging must be set at the same time.
    properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.HourMetrics.RetentionDays = 10;
    properties.HourMetrics.Version = "1.0";

    properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
    properties.MinuteMetrics.RetentionDays = 10;
    properties.MinuteMetrics.Version = "1.0";

    // Set the default service version to be used for anonymous requests.
    properties.DefaultServiceVersion = "2015-04-05";

    // Set the service properties.
    blobClient.SetServiceProperties(properties);
    
## <a name="viewing-storage-metrics"></a>Speichermetriken anzeigen

Wenn Speichermetriken Überwachen Ihres Speicherkontos konfiguriert haben, werden die Metriken in einer bekannten Tabellen in das Speicherkonto zeichnet. Können die Seite Monitor für das Speicherkonto in Azure-Verwaltungsportal in einem Diagramm werden stündlichen Metriken anzeigen. Auf dieser Seite in Azure-Verwaltungsportal können Sie:

- Wählen Sie die Kriterien im Diagramm zeichnen (die Auswahl der verfügbaren Metriken hängt, ob gewählte ausführliche oder minimale Überwachung auf der Seite konfigurieren).


- Wählen Sie den Zeitraum für die Metriken im Diagramm angezeigt.


- Wählen Sie eine absolute oder relative Skala aus, um die Metriken zu zeichnen.


- Konfigurieren Sie e-Mail benachrichtigen eine bestimmte Metrik einen bestimmten Wert erreicht.


Wenn Sie Metriken für die langfristige Speicherung herunterladen oder lokal analysieren möchten, müssen Sie ein Tool verwenden oder Code schreiben, um die Tabellen zu lesen. Sie müssen die Minuten Metriken zur Analyse herunterladen. Die Tabellen werden nicht angezeigt, wenn das Speicherkonto Liste aller Tabellen, können sie direkt über den Namen zugreifen. Viele Speicher durchsuchen Drittanbietertools kennen diese Tabellen und können direkt anzeigen (finden Sie im Blogbeitrag [Microsoft Azure Storage Explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) eine Liste der verfügbaren Tools).

### <a name="hourly-metrics"></a>Stündliche Metriken
- $MetricsHourPrimaryTransactionsBlob
- $MetricsHourPrimaryTransactionsTable
- $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minute Metriken
- $MetricsMinutePrimaryTransactionsBlob
- $MetricsMinutePrimaryTransactionsTable
- $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapazität
- $MetricsCapacityBlob

Vollständige finden Schemas für diese Tabellen [Analytics Metriken Tabelle Speicherschema](https://msdn.microsoft.com/library/azure/hh343264.aspx)Sie. Beispielzeilen unten zeigen eine Teilmenge der verfügbaren Spalten jedoch veranschaulichen einige wichtigen Funktionen wie Storage Metriken diese Metriken gespeichert:

| PartitionKey  |       RowKey       |                    Zeitstempel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Verfügbarkeit | AverageE2ELatency | AverageServerLatency | PercentSuccess |
|---------------|:------------------:|-----------------------------:|---------------|-----------------------|--------------|-------------|--------------|-------------------|----------------------|----------------|
| 20140522T1100 |      Benutzer. Alle      | 2014-05-22T11:01:16.7650250Z | 7             | 7                     | 4003         | 46801       | 100          | 104.4286          | 6.857143             | 100            |
| 20140522T1100 | Benutzer. QueryEntities | 2014-05-22T11:01:16.7640250Z | 5             | 5                     | 2694         | 45951       | 100          | 143.8             | 7.8                  | 100            |
| 20140522T1100 |  Benutzer. QueryEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 538          | 633         | 100          | 3                 | 3                    | 100            |
| 20140522T1100 | Benutzer. UpdateEntity  | 2014-05-22T11:01:16.7650250Z | 1             | 1                     | 771          | 217         | 100          | 9                 | 6                    | 100               |

In diesem Beispiel Minute Metrikdaten verwendet der Partitionsschlüssel Zeit Min. Auflösung. Der Zeilenschlüssel identifiziert die Art der Informationen, die in der Zeile gespeichert und besteht aus zwei Informationen, den Zugriffstyp und den Anforderungstyp:

- Der Zugriffstyp ist Benutzer oder System, wo Benutzer bezieht sich auf alle Speicher-Service und System auf Anfragen Speicheranalyse.

- Der Anforderungstyp ist in dem Fall ist eine Zusammenfassung oder bestimmte API wie QueryEntity oder UpdateEntity identifiziert.


Die oben aufgeführten Beispieldaten werden alle Datensätze für eine Minute (ab 11:00 Uhr), damit die Anzahl QueryEntities plus der Anzahl der QueryEntity sowie die Anzahl der Anfragen UpdateEntity Anfragen hinzufügen bis zu sieben insgesamt ist auf Benutzer: alle Zeile angezeigt. Auch die durchschnittliche Wartezeit 104.4286 Ende-Benutzer: alle Zeile berechnen ableiten ((143.8 * 5) + 3 + 9) / 7.

Einrichten von Alerts in Azure-Verwaltungsportal auf der Seite empfiehlt Speichermetriken automatisch wichtige Änderungen im Verhalten von Speicher-Services informieren können. Verwenden Sie ein Tool Storage Explorer diese Metriken Daten in ein Format mit Trennzeichen, können Microsoft Excel Sie Daten analysieren. Finden Sie im Blogbeitrag [Microsoft Azure Storage Explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) eine Liste der verfügbaren Speicher-Explorer-Tools.



## <a name="accessing-metrics-data-programmatically"></a>Metriken programmgesteuert Datenzugriff

Das folgende Listing zeigt C# Beispielcode, greift auf die Minuten Metriken für einen Bereich von Minuten zeigt die Ergebnisse in einer Konsole Fenster. Azure Storage Library Version 4 mit der CloudAnalyticsClient-Klasse, die vereinfacht Zugriff auf die metriktabellen Speicher verwendet.

    private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
    {
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    
    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
    Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
    var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
    var t = analyticsClient.GetMinuteMetricsTable(service);
    var opContext = new OperationContext();
    var query =
    from entity in metricsQuery
    // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
    // because they are calculated fields in the MetricsEntity class.
    // The PartitionKey identifies the DataTime of the metrics.
    where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
    select entity;
    
    // Filter on "user" transactions after fetching the metrics from Table Storage.
    // (StartsWith is not supported using LINQ with Azure table storage)
    var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
    var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
    Console.WriteLine(resultString);
    }
    }
    
    private static string MetricsString(MetricsEntity entity, OperationContext opContext)
    {
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
    
    }




## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Was Gebühren Sie beim speichermetriken aktivieren?

Schreiben Sie Anfragen zu Tabellenentitäten für Metriken Standardsätze für alle Azure Storage Vorgänge berechnet werden.

Lesen und löschen fordert ein Client Metrikdaten sind auch berechenbare standard Preisen. Wenn Sie eine Datenaufbewahrungsrichtlinie konfiguriert haben, Sie Zahlen Azure Storage alten Metrikdaten löscht. Löschen der Analysedaten wird Ihr Konto für die Löschvorgänge belastet.

Kapazität von metriktabellen verwendeten ist auch berechenbare: können folgende Kapazität zum Speichern von Codemetrikdaten schätzen:

- Wenn jede Stunde jeder API Service ein Dienst verwendet, wird etwa 148 KB Daten gespeichert stündlich Buchungstabellen Metriken Service und API-Ebene Zusammenfassung aktiviert haben.

- Wenn jede Stunde jeder API Service ein Dienst verwendet, wird etwa 12 KB Daten gespeichert stündlich Buchungstabellen Metriken, wenn nur die Zusammenfassung Servicelevel aktiviert ist.

- Die Tabelle für Blobs hat zwei Zeilen pro Tag (sofern Benutzer Protokolle eingeschlossen ist) hinzugefügt: Dies bedeutet, dass jeden Tag diese Tabelle von bis zu ca. 300 Byte vergrößert.

## <a name="next-steps"></a>Nächste Schritte:
[Aktivieren der Speicheranalyse anmelden und Zugreifen auf Daten](https://msdn.microsoft.com/library/dn782840.aspx)
 