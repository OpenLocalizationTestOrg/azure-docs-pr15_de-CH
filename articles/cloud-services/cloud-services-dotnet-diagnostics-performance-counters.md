<properties
   pageTitle="Verwenden Sie Leistungsindikatoren in Azure Diagnostics | Microsoft Azure"
   description="Verwenden Sie Leistungsindikatoren in Azure Cloud Services oder virtuellen Computer suchen Engpässe und Optimieren der Leistung."
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/29/2016"
   ms.author="robb" />

# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Erstellen und Verwenden von Leistungsindikatoren in Azure-Anwendung

Dieser Artikel beschreibt die Vorteile und wie Sie Leistungsindikatoren in der Azure-Anwendung. Können sie Daten sammeln, Engpässe finden und Optimieren der Leistung von System- und.

Leistungsindikatoren für Windows Server, IIS und ASP.NET können auch gesammelt und Gesundheit Azure Webrollen, Worker-Rollen und virtuelle Computer bestimmt. Sie können auch benutzerdefinierte Leistungsindikatoren verwenden.  

Sie können Daten von Leistungsindikatoren untersuchen.
1. Direkt auf dem Application Server mit dem Systemmonitor-Tool mit Remotedesktop zugegriffen
2. Mit System Center Operations Manager mit Azure Management Pack
3. Mit anderen Überwachungstools, die Zugriff auf Diagnosedaten in Azure-Speicher übertragen. Weitere Informationen finden Sie unter [Speichern und Anzeigen von Diagnosedaten in Azure-Speicher](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Weitere Informationen zum Überwachen der Leistung der Anwendung in [Azure-Verwaltungsportal](http://manage.azure.com/)finden Sie unter [How to Monitor Clouddienste](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Zusätzliche ausführliche Anleitung zum Erstellen einer Protokollierung und Strategie verfolgen und Diagnose und andere Techniken Problembehandlung und Optimierung von Azure Applications finden Sie unter [Problembehandlung bei Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx).


## <a name="enable-performance-counter-monitoring"></a>Systemmonitor-Leistungsindikator aktivieren

Leistungsindikatoren sind standardmäßig nicht aktiviert. Die Anwendung oder einen starttask muss die Diagnose Agent Standardkonfiguration spezifische Leistungsindikatoren enthalten, die Sie für jede Rolleninstanz überwachen möchten ändern.

### <a name="performance-counters-available-for-microsoft-azure"></a>Leistungsindikatoren für Microsoft Azure

Azure bietet eine Teilmenge der verfügbaren Leistungsindikatoren für Windows Server, IIS und ASP.NET Stapel. Die folgende Tabelle enthält einige Leistungsindikatoren von besonderem Interesse für Azure Applications.

|: Indikator Kategorieobjekt (Instanz)|Name des Leistungsindikators      |Referenz|
|---|---|---|
|.NET CLR-Ausnahmen (_Global_)|# Ausgelösten Ausnahmen / Sek.   |Ausnahme-Leistungsindikatoren|
|.NET CLR-Speicher (_Global_)    |GC-Zeitdauer in            |Speicherleistungsindikatoren|
|ASP.NET                      |Anwendung wird neu gestartet    |Leistungsindikatoren für ASP.NET|
|ASP.NET                      |Ausführungszeit der Anforderung  |Leistungsindikatoren für ASP.NET|
|ASP.NET                      |Unterbrochene Anfragen   |Leistungsindikatoren für ASP.NET|
|ASP.NET                      |Arbeitsprozess-Neustarts |Leistungsindikatoren für ASP.NET|
|ASP.NET Applications (__Gesamt__)|Anfragen insgesamt        |Leistungsindikatoren für ASP.NET|
|ASP.NET Applications (__Gesamt__)|Anfragen/Sekunde          |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Ausführungszeit der Anforderung  |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Wartezeit der Anforderung       |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Aktuelle Anfragen        |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Aktuelle Anfragen         |Leistungsindikatoren für ASP.NET|
|ASP.NET v4.0.30319           |Abgelehnte Anfragen       |Leistungsindikatoren für ASP.NET|
|Speicher                       |Verfügbare MB        |Speicherleistungsindikatoren|
|Speicher                       |Zugesicherte Bytes         |Speicherleistungsindikatoren|
|Prozessor(_Total)            |Prozessorzeit (%)        |Leistungsindikatoren für ASP.NET|
|TCPv4                        |Verbindungsfehler     |TCP-Objekt|
|TCPv4                        |Verbindung hergestellt |TCP-Objekt|
|TCPv4                        |Verbindung zurücksetzen       |TCP-Objekt|
|TCPv4                        |Segmente gesendet/s       |TCP-Objekt|
|Netzwerk Interface(*)         |Empfangene Bytes/Sek.      |Netzwerkschnittstellen-Objekt|
|Netzwerk Interface(*)         |Bytes gesendet/s          |Netzwerkschnittstellen-Objekt|
|Netzwerk-Schnittstelle (Microsoft Virtual Machine-Bus Netzwerk Adapter _2)|Empfangene Bytes/Sek.|Netzwerkschnittstellen-Objekt|
|Netzwerk-Schnittstelle (Microsoft Virtual Machine-Bus Netzwerk Adapter _2)|Bytes gesendet/s|Netzwerkschnittstellen-Objekt|
|Netzwerk-Schnittstelle (Microsoft Virtual Machine-Bus Netzwerk Adapter _2)|Gesamtanzahl Bytes/s|Netzwerkschnittstellen-Objekt|

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Erstellen und benutzerdefinierte Leistungsindikatoren zur Anwendung hinzufügen

Azure unterstützt benutzerdefinierte Leistungsindikatoren für Webrollen und Workerrollen zu erstellen. Die Leistungsindikatoren verwendet verfolgen und überwachen anwendungsspezifisches Verhalten. Sie können erstellen und Löschen von Bezeichner und benutzerdefinierte Leistungsindikatorkategorien Startaufgabe, Webrolle oder Worker-Rolle mit erweiterten Berechtigungen.

>[AZURE.NOTE] Code, der benutzerdefinierten Leistungsindikatoren ändert muss erweiterte Berechtigungen ausgeführt. Wenn Code in einer Webrolle oder Worker-Rolle ist, sind die Rolle das Tag <Runtime executionContext="elevated" /> in der Datei ServiceDefinition.csdef für die Rolle richtig initialisiert werden.

Sie können benutzerdefinierte Leistungsindikatordaten Azure-Speicher mit dem Diagnose-Agent senden.

Standard-Leistungsindikatordaten erzeugt Azure Prozesse. Benutzerdefinierte Leistungsindikatordaten müssen von der Webanwendung Rolle oder Worker-Rolle erstellt werden. Informationen über die Arten von Daten in benutzerdefinierte Leistungsindikatoren gespeichert werden können finden Sie unter [Leistungsindikatortypen](https://msdn.microsoft.com/library/z573042h.aspx) . Finden Sie ein Beispiel, das erstellt und benutzerdefinierte Leistungsindikatordaten in einer Webrolle [PerformanceCounters Beispiel](http://code.msdn.microsoft.com/azure/) .

## <a name="store-and-view-performance-counter-data"></a>Leistungsindikatordaten speichern und anzeigen

Azure speichert Leistungsindikatordaten mit anderen Diagnoseinformationen. Diese Daten werden für remote-Überwachung während die Rolleninstanz remote desktop Zugriff ausgeführt wird auf Tools wie Systemmonitor anzeigen. Zum Beibehalten der Daten außerhalb der Rolleninstanz muss der Diagnose-Agent Daten Azure-Speicher übertragen. Das Limit von zwischengespeicherten Leistungsindikatordaten im Diagnose-Agent konfiguriert werden und ist Teil einer freigegebenen Beschränkung für diagnostischen Daten konfiguriert. Weitere Informationen zum Festlegen der Puffergröße finden Sie unter [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) und [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Überblick über Diagnose-Agent ein Speicherkonto Datenübertragung einrichten finden Sie unter [Speichern und Anzeigen von Diagnosedaten in Azure-Speicher](https://msdn.microsoft.com/library/azure/hh411534.aspx) .

Jeder konfigurierte Leistungsindikatorinstanz an eine angegebene Samplingrate erfasst und die gesammelten Daten an das Speicherkonto einen geplanten Antrag oder ein Antrag auf Anforderung übertragen. Automatische Übertragung können als einmal pro Minute geplant werden. Leistungsindikatordaten vom Agent Diagnose übertragen werden in einer Tabelle, WADPerformanceCountersTable, das Speicherkonto gespeichert. Diese Tabelle Zugriff und mit standardmäßigen Azure Storage API abgefragt. Finden Sie [Microsoft Azure PerformanceCounters Beispiel](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) beispielsweise Abfragen und Anzeigen von Leistungsindikatordaten aus der WADPerformanceCountersTable-Tabelle.

>[AZURE.NOTE] Diagnose-Agent übertragen Häufigkeit und Wartezeit in Warteschlangen möglicherweise die neuesten Leistungsindikatordaten im Speicherkonto einige Minuten veraltet.

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Aktivieren Sie Leistungsindikatoren mit Diagnose-Konfigurationsdatei

Gehen Sie die Leistungsindikatoren in der Azure-Anwendung aktivieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Abschnitt wird vorausgesetzt, dass Sie Monitor für die Diagnose in die Anwendung importiert und Diagnose-Konfigurationsdatei (SDK 2.4 und unter diagnostics.wadcfg oder diagnostics.wadcfgx im SDK 2.5 oder höher) der Visual Studio-Projektmappe hinzugefügt haben. Siehe Schritt 1 und 2 [Aktivieren](./cloud-services-dotnet-diagnostics.md)Diagnose in Azure Cloud Services und virtuelle Computer) Weitere Informationen.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Schritt 1: Sammeln und Speichern von Leistungsindikatoren

Nachdem Sie die Diagnose der Visual Studio-Projektmappe hinzugefügt haben, können Sie Sammlung und Lagerung von Leistungsindikatordaten in Azure-Anwendung konfigurieren. Dies ist die Diagnosedatei Leistungsindikatoren hinzufügen. Diagnosedaten, einschließlich Leistungsindikatoren werden zuerst auf der Instanz. Daten werden dann der WADPerformanceCountersTable-Tabelle im Dienst Azure Tabelle beibehalten, müssen Sie auch das Speicherkonto in der Anwendung angeben. Wenn Sie Ihre Anwendung lokal in der Serveremulator testen, können Sie Diagnosedaten auch lokal in der Speicheremulator speichern. Bevor Sie Diagnosedaten speichern müssen Sie zum [Azure-Verwaltungsportal](http://manage.windowsazure.com/) und erstellen ein Speicherkonto. Eine bewährte Methode ist das Speicherkonto in demselben Standort wie der Azure-Anwendung um vermeiden externe Bandbreite und Latenz.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Hinzufügen von Leistungsindikatoren zur Diagnosedatei

Es gibt viele Leistungsindikatoren verwenden können. Das folgende Beispiel zeigt mehrere Leistungsindikatoren, die für Web und Arbeitskraft Rolle Überwachung empfohlen.

Die Datei Diagnose (SDK 2.4 und unter diagnostics.wadcfg oder diagnostics.wadcfgx im SDK 2.5 und höher) und das DiagnosticMonitorConfiguration-Element Folgendes hinzufügen:

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

Das BufferQuotaInMB-Attribut, das maximal Dateisystemspeicher angibt, für die Auflistung Datentyp (Azure-Protokolle, IIS-Protokolle usw.). Der Standardwert ist 0. Wenn das Kontingent erreicht ist, werden die ältesten Daten gelöscht, neue Daten hinzugefügt werden. Die Summe aller BufferQuotaInMB Eigenschaften muss größer als der Wert des Attributs OverallQuotaInMB. Für Weitere Informationen zu ermitteln, wie viel Speicher für die Sammlung der Diagnosedaten werden Siehe Abschnitt Setup Bündel [Problembehandlung Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Gibt das Intervall zwischen geplante Datenübermittlungen Attribut ScheduledTransferPeriod auf die nächste Minute aufgerundet. In den folgenden Beispielen wird es auf PT30M (30 Minuten) festgelegt. Übertragung Periode auf einen niedrigen Wert wie 1 Minute beeinträchtigt die Leistung der Anwendung in der Produktion sondern sehen Diagnose schnell arbeiten, wenn Sie Tests hilfreich. Die geplante Übertragung sein klein genug, um sicherzustellen, dass Daten nicht Instanz jedoch groß genug überschrieben werden, dass dadurch nicht die Leistung der Anwendung beeinträchtigt wird.

Das CounterSpecifier-Attribut gibt den Leistungsindikator gesammelt. SampleRate-Attribut gibt die Rate mit der Leistungsindikator, in diesem Fall 30 Sekunden zu beproben sind.

Nachdem Sie Leistungsindikatoren, die Sie sammeln möchten hinzugefügt haben, speichern Sie die Diagnosedatei. Als Nächstes müssen Sie das Speicherkonto angeben, in der die Diagnosedaten gespeichert werden.

### <a name="specify-the-storage-account"></a>Geben Sie das Speicherkonto

Die Diagnose-Informationen zu Ihrem Azure Storage-Konto müssen Sie eine Verbindungszeichenfolge in der Service-Konfigurationsdatei (ServiceConfiguration.cscfg) angeben.

Für Azure SDK 2.5 kann das Speicherkonto in der Datei diagnostics.wadcfgx angegeben werden.

>[AZURE.NOTE] Diese Anleitung gilt nur Azure SDK 2.4 und kleiner. Für Azure SDK 2.5 kann das Speicherkonto in der Datei diagnostics.wadcfgx angegeben werden.

So richten Sie die Verbindungszeichenfolgen

1. Öffnen Sie die ServiceConfiguration.Cloud.cscfg-Datei mit Ihrem bevorzugten Editor und legen Sie die Verbindungszeichenfolge für den Speicher. *Kontoname* und *AccountKey* Werte befinden sich im klassischen Azure-Portal im Dashboard Konto Speicher unter Schlüssel verwalten.

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Speichern Sie die Datei ServiceConfiguration.Cloud.cscfg.

3. Die ServiceConfiguration.Local.cscfg-Datei öffnen und überprüfen, ob UseDevelopmentStorage auf True.

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
Da die Verbindungszeichenfolgen festgelegt sind, bleiben die Anwendung beim Bereitstellen der Anwendung Diagnosedaten an das Speicherkonto.
4. Speichern Sie und erstellen Sie das Projekt und bereitzustellen Sie die Anwendung.

## <a name="step-2-optional-create-custom-performance-counters"></a>Schritt 2: (Optional) Erstellen benutzerdefinierter Leistungsindikatoren

Neben der vordefinierten Leistungsindikatoren können Sie eigene benutzerdefinierte Leistungsindikatoren zum Überwachen von Web-oder Arbeitskraft hinzufügen. Benutzerdefinierte Leistungsindikatoren können verwendet werden, zu verfolgen und überwachen anwendungsspezifisches Verhalten und erstellt oder gelöscht werden können Startaufgabe, Webrolle oder Worker-Rolle mit erweiterten Berechtigungen.

Azure Diagnostics-Agent aktualisiert die Performance Counter Konfiguration aus der Datei wadcfg eine Minute nach dem starten.  Erstellen benutzerdefinierter Leistungsindikatoren in der OnStart-Methode und Ihre Aufgaben länger als eine Minute ausgeführt, werden die benutzerdefinierten Leistungsindikatoren nicht erstellt wurden bei der Azure-Diagnose-Agent versucht, sie laden.  In diesem Szenario sehen Azure-Diagnose korrekt alle Diagnosedaten außer benutzerdefinierten Leistungsindikatoren erfasst.  Zum Beheben dieses Problems erstellen Sie die Leistungsindikatoren in einer Startaufgabe oder verschieben Sie die Start-Aufgabe der OnStart-Methode einiges nach dem Erstellen der Leistungsindikatoren.

Führen Sie die folgenden Schritte aus, um einen einfachen benutzerdefinierten Leistungsindikator namens "\MyCustomCounterCategory\MyButton1Counter" erstellen:

1. Öffnen Sie die Service-Definitionsdatei (CSDEF) für die Anwendung.
2. Fügen Sie die Runtime Element der WebRole oder WorkerRole-Element, um die Ausführung mit erhöhten hinzu:

    ```
    <runtime executioncontext="elevated"/>
    ```
3. Speichern Sie die Datei.
4. Öffnen Sie die Diagnosedatei (SDK 2.4 und unter diagnostics.wadcfg oder diagnostics.wadcfgx im SDK 2.5 und höher) und fügen Sie Folgendes zu der DiagnosticMonitorConfiguration 

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Speichern Sie die Datei.
6. Erstellen Sie benutzerdefinierte Leistungsindikatorkategorie in der OnStart-Methode Ihrer Rolle vor dem Aufruf von Base. OnStart. Im folgende C#-Beispiel erstellt eine benutzerdefinierte Kategorie, wenn es nicht bereits vorhanden ist:

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. Aktualisieren Sie die Leistungsindikatoren in der Anwendung. Das folgende Beispiel aktualisiert die benutzerdefinierten Leistungsindikators Button1_Click Ereignisse:

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. Speichern Sie die Datei.  

Benutzerdefinierte Leistungsindikatordaten werden jetzt von Azure Diagnostics Monitor gesammelt.

## <a name="step-3-query-performance-counter-data"></a>Schritt 3: Abfrage-Leistungsindikatordaten

Nachdem die Anwendung bereitgestellt und mit Monitor für die Diagnose beginnt Leistungsindikatoren sammeln und diese Daten Azure-Speicher. Mit Tools wie Server-Explorer in Visual Studio, [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/)oder [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) Cerebrata die Leistungsindikatordaten in WADPerformanceCountersTable-Tabelle anzeigen. Tabelle Service mit [C#](../storage/storage-dotnet-how-to-use-tables.d), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md)oder [PHP](../storage/storage-php-how-to-use-table-storage.md)können auch programmgesteuert abgefragt werden.

Im folgende C#-Beispiel zeigt eine einfache Abfrage für die WADPerformanceCountersTable-Tabelle und speichert die Diagnosedaten in eine CSV-Datei. Wenn die Leistungsindikatoren in einer CSV-Datei gespeichert werden können Grafik Funktionen in Microsoft Excel oder einem anderen Tool Sie die Daten. Unbedingt eine Microsoft.WindowsAzure.Storage.dll, die ist im Azure SDK für .NET Oktober 2012. Die Assembly wird in das Programm Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-Num\ref\ Verzeichnis installiert.

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

Entitäten zuordnen C# Objekte mit einer benutzerdefinierten Klasse, die von **TableEntity**abgeleitet. Der folgende Code definiert eine Entitätsklasse, die einen Leistungsindikator in der **WADPerformanceCountersTable** -Tabelle darstellt.


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }


## <a name="next-steps"></a>Nächste Schritte
[Zusätzliche Sichtartikel in Azure-Diagnose] (.. / Azure-diagnostics.md)
