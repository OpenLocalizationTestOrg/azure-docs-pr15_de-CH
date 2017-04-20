<properties
    pageTitle="Verwendung von Azure Diagnostics (.NET) mit Cloud-Services | Microsoft Azure"
    description="Mithilfe von Azure Diagnostics um Azure-Cloud-Dienste für Debuggen, Messen der Leistung, Überwachung, Analyse und weitere Daten zu sammeln."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Aktivieren von Azure Diagnostics in Azure Cloud Services

Hintergrund für Azure-Diagnose finden Sie unter [Azure Diagnostics Overview](../azure-diagnostics.md) .


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Aktivieren der Diagnose in eine Worker-Rolle

Dieser Spaziergang beschreibt Azure Worker-Rolle implementiert, die mit .NET EventSource-Klasse Telemetriedaten ausgibt. Azure-Diagnose zum Telemetriedaten sammeln und in Azure Storage-Konto speichern. Wenn eine Worker-Rolle erstellen aktiviert Visual Studio automatisch Diagnostics 1.0 als Teil der Lösung in Azure SDKs für .NET 2.4 und früher. Die folgende Anleitung beschreibt die Worker-Rolle deaktivieren Diagnostics 1.0 aus der Lösung und Bereitstellen von Diagnose 1.2 oder 1.3 der Worker-Rolle erstellen.

### <a name="pre-requisites"></a>Erforderliche Komponenten
Vorausgesetzt Sie Azure-Abonnement und Visual Studio 2013 Azure SDK. Wenn Sie keinen Azure-Abonnement können Sie sich für die [Kostenlose Testversion][]anmelden. Stellen Sie sicher, dass [Installieren und Konfigurieren von Azure PowerShell Version 0.8.7 oder höher][].

### <a name="step-1-create-a-worker-role"></a>Schritt 1: Erstellen Sie eine Worker-Rolle
1.  Starten Sie **Visual Studio 2013**.
2.  Erstellen Sie ein neues **Azure Cloud Service** -Projekt aus der **Cloud** -Vorlage, die auf.NET Framework 4.5.  Nennen Sie das Projekt "WadExample", und klicken Sie auf Ok.
3.  Wählen Sie **Worker-Rolle** , und klicken Sie auf Ok. Das Projekt wird erstellt.
4.  Doppelklicken Sie im **Projektmappen-Explorer**auf Eigenschaftsdatei **WorkerRole1** .
5.  Die **Konfiguration** der Registerkarte deaktivieren **Diagnose aktivieren** deaktivieren Diagnostics 1.0 (Azure SDK 2.4 und Eariler).
6.  Erstellen Sie die Projektmappe, um sicherzustellen, dass keine Fehler auftreten.

### <a name="step-2-instrument-your-code"></a>Schritt 2: Instrumentiert code
Ersetzen Sie den Inhalt des WorkerRole.cs durch den folgenden Code. Die Klasse SampleEventSourceWriter, von der [Ereignisquelle Klasse][]geerbt implementiert vier Protokollierungsmethoden: **SendEnums**, **MessageMethod**, **SetOther** und **HighFreq**. Der erste Parameter der Methode **WriteEvent** definiert die ID für das jeweilige Ereignis. Die Run-Methode implementiert eine Endlosschleife, die jeweils den Protokollierungsmethoden 10 Sekunden in der **SampleEventSourceWriter** -Klasse implementiert wird.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
    {
    sealed class SampleEventSourceWriter : EventSource
    {
        public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
        public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
        public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
        public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
        public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

    }

    enum MyColor
    {
        Red,
        Blue,
        Green
    }

    [Flags]
    enum MyFlags
    {
        Flag1 = 1,
        Flag2 = 2,
        Flag3 = 4
    }

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }
        }

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Schritt 3: Bereitstellen der Worker-Rolle
1.  Bereitstellen der Worker-Rolle in Azure in Visual Studio **WadExample** -Projekt im Projektmappen-Explorer und **Veröffentlichen** im Menü **Erstellen** ausgewählt.
2.  Wählen Sie Ihr Abonnement.
3.  Wählen Sie im Dialogfeld **Microsoft Azure Veröffentlichungseinstellungen** **Neu erstellen...**.
4.  Geben Sie im Dialogfeld **Cloud-Dienst erstellen und speichern** einen **Namen** (beispielsweise "WadExample") und wählen Sie eine Gruppe.
5.  **Staging**- **Umgebung** soll.
6.  Ändern Sie alle anderen **Einstellungen** nach Bedarf und auf **Veröffentlichen**.
7.  Nach Abschluss der Bereitstellung im klassischen Azure-Portal überprüfen Sie, ob der Cloud-Dienst **ausgeführt** wird.

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Schritt 4: Erstellen Sie Diagnose Konfigurationsdatei und installieren Sie die Erweiterung
1.  Herunterladen der Schemadefinition öffentliche Konfiguration Datei folgenden PowerShell-Befehl ausführen:
2.
        (Get-AzureServiceAvailableExtension-.-Kennung Erweiterungsname 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics'). PublicConfigurationSchema | Out-File-Codierung utf8 - Dateipfad 'WadConfig.xsd'

2.  **WorkerRole1** Projekt mit der rechten Maustaste auf das Projekt **WorkerRole1** eine XML-Datei hinzu, und wählen Sie **Hinzufügen** -> **Neues Element...**  ->  **Visual C# Elemente** -> **Daten** -> **XML-Datei**. Benennen Sie die Datei "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

3.  Ordnen Sie die WadConfig.xsd der Konfigurationsdatei. Stellen Sie sicher, dass das WadExample.xml-Editor-Fenster das aktive Fenster ist. Drücken Sie **F4** , um **das Eigenschaftenfenster** zu öffnen. Klicken Sie auf die **Schemas** -Eigenschaft im **Eigenschaftenfenster** . Klicken Sie auf **...** in der Eigenschaft des **Schemas** . Klicken Sie auf die **hinzufügen...** und navigieren Sie zum Speicherort der XSD-Datei und wählen Sie die Datei WadConfig.xsd. Klicken Sie auf **OK**.
4.  Ersetzen Sie den Inhalt der Konfigurationsdatei WadExample.xml mit dem folgenden XML-Code, und speichern Sie die Datei. Diese Datei definiert eine Reihe von Leistungsindikatoren sammeln: CPU-Auslastung und Speicherverwendung. Die Konfiguration definiert vier Ereignisse, die Methoden in der Klasse SampleEventSourceWriter entspricht.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Schritt 5: Installieren Sie Diagnostics auf Worker-Rolle
Die PowerShell-Cmdlets zur Verwaltung von Diagnose Web oder Worker-Rolle sind: Set AzureServiceDiagnosticsExtension Get-AzureServiceDiagnosticsExtension und AzureServiceDiagnosticsExtension entfernen.

1.  Azure PowerShell zu öffnen.
2.  Führen Sie das Skript um Diagnose auf die Worker-Rolle (ersetzen Sie *StorageAccountKey* durch den Speicher kontoschlüssel für das Speicherkonto Wadexample) installieren:

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Schritt 6: Betrachten Sie die Telemetriedaten
Navigieren Sie im Visual Studio- **Server-Explorer** Wadexample Speicherkonto. Nach der Cloud läuft Service ca. 5 Minuten Tabellen **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** und **WADSetOtherTable**angezeigt werden soll. Doppelklicken Sie auf eines der Telemetriedaten anzeigen, die gesammelt wurden.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Konfigurationsdateischema

Diagnose-Konfigurationsdatei definiert Werte, mit denen Diagnose Konfigurationen zu initialisieren, wenn Diagnose-Agent gestartet wird. Finden Sie [Referenzinformationen zum aktuellen](https://msdn.microsoft.com/library/azure/mt634524.aspx) gültigen Werte und Beispiele.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie Probleme haben, finden Sie unter [Problembehandlung bei Azure Diagnostics](../azure-diagnostics-troubleshooting.md) mit Probleme.

## <a name="next-steps"></a>Nächste Schritte
So ändern Sie die Daten, die Sie sammeln, [sehen Sie eine Liste der virtuellen Computer verwandte Azure Diagnostics Artikel](azure-diagnostics.md#cloud-services) Problembehandlung oder erfahren Sie mehr über Diagnose im Allgemeinen.


[Quelle-Klasse]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Kostenlose Testversion]: http://azure.microsoft.com/pricing/free-trial/
[Installieren und Konfigurieren von Azure PowerShell Version 0.8.7 oder höher]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
