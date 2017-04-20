<properties
    pageTitle="Azure-Diagnosedaten in Langsamster Pfad Ereignis Hubs mit Streaming | Microsoft Azure"
    description="Veranschaulicht Azure Diagnostics mit Ereignis Konfigurieren von Anfang bis Ende einschließlich Leitfäden für Szenarien."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Streaming von Azure-Diagnosedaten in Langsamster Pfad mit Event Hubs

Azure-Diagnose ermöglicht flexible Metriken und Protokolle von Cloud-Diensten für virtuelle Maschinen (VMs) und Ergebnisse in Azure-Speicher übertragen. Ab März 2016 (SDK 2.9) Zeitrahmen Sie Diagnostics vollständig benutzerdefinierten Datenquellen sinken und Datenübertragung Langsamster Pfad in Sekunden mit [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/).

Unterstützte Datentypen sind:

- Event Tracing for Windows (ETW) Ereignisse
- Leistungsindikatoren
- Windows-Ereignisprotokolle
- Anwendungsprotokolle
- Azure Diagnoseprotokolle Infrastruktur

Dieser Artikel veranschaulicht die Azure-Diagnose mit Ereignis zu konfigurieren. Anleitung dient auch für die folgenden Szenarien:

- Anpassen der Protokolle und Metriken versenkt Ereignis Hubs erhalten
- Zum Ändern der Konfigurationen in jeder Umgebung
- Ereignis-Hubs Daten anzeigen
- Die Verbindung zu Problembehandlung  

## <a name="prerequisites"></a>Erforderliche Komponenten

Ereignis-Hubs Auffangen von Azure-Diagnose wird unterstützt Clouddienste, VMs, Virtual Machine Maßstab legt und Service Fabric, Azure SDK 2.9 und die entsprechenden Azure Tools für Visual Studio ab.

- Azure Diagnostics Erweiterung 1.6 ([Azure SDK für .NET 2.9 oder höher](https://azure.microsoft.com/downloads/) abzielt dies standardmäßig)
- [Visual Studio 2013 oder höher](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Vorhandene Konfigurationen von Azure Diagnostics in einer Anwendung mithilfe einer *.wadcfgx* eine der folgenden Methoden:
    - Visual Studio: [Diagnose für Azure Cloud Services und virtuelle Computer konfigurieren](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
    - Windows PowerShell: [Diagnose in Azure-Cloud-Diensten mit PowerShell aktivieren](../cloud-services/cloud-services-diagnostics-powershell.md)
- Ereignis-Hubs Namespace pro Artikel [Erste Schritte mit Event Hubs](./event-hubs-csharp-ephcs-getstarted.md) bereitgestellt

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Verbinden von Azure Diagnostics Ereignis Hubs Senke

Azure Diagnostics sinkt Protokolle und Metriken immer standardmäßig ein Azure Storage-Konto. Eine Anwendung kann außerdem einen neuen Abschnitt **senken** **WadCfg** Element im Abschnitt **PublicConfig** der Datei *.wadcfgx* hinzufügen zu Ereignis Hubs sinken. In Visual Studio die Datei *.wadcfgx* im folgenden Pfad gespeichert: **Cloud Service Project** > **Rollen** >  **(RoleName)** > **diagnostics.wadcfgx** -Datei.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

In diesem Beispiel Ereignis Hub-URL auf den vollqualifizierten Namespace des Ereignisses festgelegt: Ereignis Hubs Namespace + "/" + Name Event Hub.  

Ereignis Hub-URL wird im Schaltpult Ereignis Hubs [Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) angezeigt.  

Name **Auffangen** kann eine beliebige gültige Zeichenfolge festgelegt werden, solange der gleiche Wert konsistent in der Config-Datei verwendet wird.

> [AZURE.NOTE]  Möglicherweise zusätzliche senken, wie *ApplicationInsights* in diesem Abschnitt konfiguriert. Azure-Diagnose kann mindestens Ereignissenken definiert werden, wenn jede Senke auch im **PrivateConfig** -Abschnitt deklariert.  

Die Senke Ereignis Hubs muss auch deklariert und definiert im **PrivateConfig** -Abschnitt der Konfigurationsdatei *.wadcfgx* .

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

Die `SharedAccessKeyName` Wert muss eine gemeinsame Zugriff Signatur (SAS) und Policy im **Ereignis Hubs** Namespace definiert wurde. Ereignis-Hubs Dashboard in [Azure-Portal](https://manage.windowsazure.com)durchsuchen, klicken Sie auf die Registerkarte **Konfigurieren** , und richten Sie eine benannte Richtlinie (z. B. "SendRule") die Berechtigung *Senden* . **StorageAccount** ist auch in **PrivateConfig**deklariert. Es muss keine Werte ändern, wenn sie arbeiten. In diesem Beispiel lassen wir die Werte leer ist, ein Zeichen eine nachgelagerte Anlage Werte festgelegt. *ServiceConfiguration.Cloud.cscfg* Umgebung-Konfigurationsdatei setzt die Umgebung geeigneten Namen und Schlüssel.  

> [AZURE.WARNING] Ereignis-Hubs SAS-Schlüssel wird im Klartext in der Datei *.wadcfgx* gespeichert. Dieser Schlüssel häufig Quellcodeverwaltungssystem eingecheckt werden oder ist als Anlage in der Buildserver entsprechend geschützt werden sollte. Es wird empfohlen, eine SAS-Taste *nur* Berechtigungen zu verwenden, damit ein böswilliger Benutzer an den Hub Ereignis schreiben jedoch nicht hören oder verwalten kann.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Konfigurieren Sie Diagnoseprotokolle Azure und Metriken mit Event sink

Wie zuvor erläutert alle standardmäßige und benutzerdefinierte Diagnosedaten also Metriken und Protokolle, Azure-Speicher automatisch versenkt in festgelegten Zeitabständen. Ereignis-Hubs und zusätzliche Empfänger soll alle Stamm- oder untergeordneten Knoten in der Hierarchie mit dem Ereignis gewidmet sein. Dazu gehören ETW-Ereignisse, Leistungsindikatoren, Windows-Ereignisprotokollen und Anwendungsprotokollen.   

Es ist wichtig zu berücksichtigen, wie viele Datenpunkte an Event Hubs tatsächlich übertragen werden soll. In der Regel Datenübertragung Entwickler niedriger Latenz hot Path, die verbraucht und schnell interpretiert werden müssen. Systeme, die Alarme und skalieren Regeln überwachen sind Beispiele. Entwickler kann auch einen Alternative Analyse Speicher konfigurieren oder Speicher – z. B. Azure Stream Analytics, Elasticsearch, benutzerdefinierte Überwachungssystem oder bevorzugten Überwachungssystem von anderen suchen.

Im folgenden sind einige Beispielkonfigurationen.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

Im folgenden Beispiel die Senke **PerformanceCounters** übergeordneten Knoten in der Hierarchie, d. h. gilt für alle untergeordneten **PerformanceCounters** Ereignis Hubs an.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

Im vorherigen Beispiel wird die Senke in nur drei Indikatoren: **Anfragen** **Abgelehnt**und **Prozessorzeit**.  

Das folgende Beispiel zeigt, wie ein Entwickler beschränken kann gesendete Daten wichtigen Kennzahlen, die für die Gesundheit von diesem Dienst verwendet werden.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

In diesem Beispiel die Senke auf Protokolle angewendet und ist nur für Ebene Fehlertrace gefiltert.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Bereitstellen und Aktualisieren einer Cloud-Dienste Anwendung und Diagnose-Konfiguration

Visual Studio bietet den einfachsten Weg zum Bereitstellen der Anwendung und Ereignis Hubs Senke Konfiguration. Zum Anzeigen und bearbeiten die Datei, öffnen Sie die Datei *.wadcfgx* in Visual Studio bearbeiten Sie und speichern. Der Pfad ist **Cloud-Dienstprojekt** > **Rollen** > **(RoleName)** > **diagnostics.wadcfgx**.  

Zu diesem Zeitpunkt alle und Bereitstellung Aktualisierungen in Visual Studio, Visual Studio Team System und alle Befehle oder Skripts, die auf MSBuild und Verwendung der **t: Veröffentlichen** *.wadcfgx* im Paketprozess hinzugefügt. Darüber hinaus bereitstellen Installationen und Updates der Datei in Azure anhand der entsprechenden Azure Diagnostics Agent Erweiterung der VMs.

Nach der Bereitstellung der Anwendung und Konfiguration von Azure Diagnostics sehen Sie sofort Aktivität im Dashboard des Ereignisses. Dies bedeutet, dass Sie bereit sind, gehen Sie zum Anzeigen der hot-Daten im Listener Client oder Analyse-Tool Ihrer Wahl.  

In der folgenden Abbildung das Ereignis Hubs Dashboard zeigt fehlerfrei Senden von Diagnosedaten an den Hub Ereignis nach 23 Uhr ab. Deshalb wurde die Anwendung mit einer aktualisierten *.wadcfgx* -Datei bereitgestellt und konfiguriert die Senke.

![][0]  

> [AZURE.NOTE] Updates der Azure-Diagnose-Konfigurationsdatei (.wadcfgx) vornehmen, wird empfohlen, die Updates auf die gesamte Anwendung sowie die Konfiguration mithilfe von Visual Studio veröffentlichen oder ein Windows PowerShell-Skript pushen.  

## <a name="view-hot-path-data"></a>Ansicht hot-Daten

Wie bereits erwähnt, sind viele Anwendungsbeispiele für Überwachung und Ereignis Hubs Daten.

Ein einfacher Ansatz ist, erstellen Sie eine kleine Event Hub und drucken den Ausgabestream. Sie können setzen Sie folgenden Code im [Ereignis Hubs Einstieg](./event-hubs-csharp-ephcs-getstarted.md)ausführlicher erläutert), in eine.  

Beachten Sie, dass das Konsolenanwendungsprojekt [Ereignis Prozessor Host Nuget-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/)enthalten muss.  

Beachten Sie die Werte in Klammern in der **Main** -Funktion für Ihre Ressourcen ersetzen.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Problembehandlung bei Ereignis-Hubs Senke

- Ereignis-Hub zeigt keine eingehenden oder ausgehenden Ereignisaktivität wie erwartet.

    Überprüfen Sie Ihr Ereignis Hub erfolgreich bereitgestellt. Alle Verbindungsinformationen in Abschnitt **PrivateConfig** *.wadcfgx* muss die Werte der Ressource wie im Portal. Stellen Sie sicher, dass Sie eine SAS-Richtlinie definiert (im Beispiel "SendRule") im Portal und *Berechtigung* gewährt wird.  

- Nach einer Aktualisierung werden Ereignis-Hub nicht mehr eingehenden oder ausgehenden Ereignisaktivität

    Stellen Sie zunächst sicher, Richtigkeit Info Event Hub und Konfiguration wie zuvor beschrieben. Manchmal wird die **PrivateConfig** Bereitstellung Update zurückgesetzt. Die empfohlene Fehlerbehebung ist alle Änderungen an *.wadcfgx* im Projekt und anschließend eine vollständige Anwendung aktualisieren. Wenn dies nicht möglich ist, unbedingt Diagnose Update legt eine vollständige **PrivateConfig** , die die SAS-Taste.  

- Ich versuchte Vorschläge und Event-Hub funktioniert immer noch nicht.

    Suchen Sie in Azure Storage-Tabelle, Protokolle und Fehler für Azure Diagnostics selbst enthält: **WADDiagnosticInfrastructureLogsTable**. Eine Option ist, verwenden Sie ein Tool wie [Azure Storage Explorer](http://www.storageexplorer.com) diesem Storage-Konto verbinden, diese Tabelle anzeigen und Hinzufügen einer Abfrage für Zeitstempel in den letzten 24 Stunden. Das Tool können Sie eine CSV-Datei exportieren und in einer Anwendung wie Microsoft Excel öffnen. Excel erleichtert die Callingcard Zeichenfolgen wie **EventHubs**zu sehen, welche Fehler suchen.  

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Ereignis-Hubs](https://azure.microsoft.com/services/event-hubs/) •

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Anhang: Azure-Diagnose (.wadcfgx) Konfigurationsdateibeispiel abgeschlossen

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Ergänzende *ServiceConfiguration.Cloud.cscfg* für dieses Beispiel sieht folgendermaßen aus.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png
