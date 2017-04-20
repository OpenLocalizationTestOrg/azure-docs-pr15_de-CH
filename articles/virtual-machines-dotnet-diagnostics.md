<properties
    pageTitle="Verwendung von Azure Diagnostics in virtuellen Maschinen | Microsoft Azure"
    description="Mithilfe von Azure Diagnostics Datensammlung von Azure-Computer debuggen, Messen der Leistung, Überwachung, Analyse und mehr."
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Aktivieren der Diagnose in Azure Virtual Machines

Hintergrund für Azure-Diagnose finden Sie unter [Azure Diagnostics Overview](azure-diagnostics.md) .

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Aktivieren der Diagnose auf einem virtuellen Computer

Dieser Spaziergang beschreibt Diagnose Azure virtuellen Maschine von einem Entwicklungscomputer Remoteinstallation. Sie erfahren außerdem, wie eine Anwendung implementiert, die diese Azure virtuelle Maschine und Telemetriedaten mit .NET [EventSource Klasse][]. Azure Diagnostics wird Telemetriedaten sammeln und in einem Azure Storage-Konto verwendet.

### <a name="pre-requisites"></a>Erforderliche Komponenten
Dieser Spaziergang nimmt ein Azure-Abonnement haben und Visual Studio 2013 Azure SDK verwenden. Wenn Sie keinen Azure-Abonnement können Sie sich für die [Kostenlose Testversion][]anmelden. Stellen Sie sicher, dass [Installieren und Konfigurieren von Azure PowerShell Version 0.8.7 oder höher][].

### <a name="step-1-create-a-virtual-machine"></a>Schritt 1: Erstellen einer virtuellen Maschine
1.  Starten Sie auf dem Entwicklungscomputer Visual Studio 2013.
2.  Im Visual Studio- **Server-Explorer** erweitern Sie **Azure**, **virtuelle Computer** Maustaste, und wählen **virtuellen Computer erstellen**.
3.  Wählen Sie Azure-Abonnement im Dialogfeld **Wählen Sie ein Abonnement aus** und auf **Weiter**.
4.  **Windows Server 2012 R2 Datacenter November 2014** im Dialogfeld **Abbild eines virtuellen Computers auswählen** und auf **Weiter.**
5.  In den **Standardeinstellungen Virtual Machine**Name des virtuellen Computers zu "Wadexample" fest Legen Sie Ihren Administratorbenutzernamen und Ihr Kennwort, und klicken Sie auf **Weiter**.
6.  Erstellen Sie einen neuen Clouddienst mit dem Namen "WadexampleVM" **Cloud Service** Dateispeicherort. Erstellen Sie ein neues Speicherkonto namens "Wadexample", und klicken Sie auf **Weiter**.
7.  Klicken Sie auf **Erstellen**.

### <a name="step-2-create-your-application"></a>Schritt 2: Erstellen der Anwendung
1.  Starten Sie auf dem Entwicklungscomputer Visual Studio 2013.
2.  Erstellen Sie eine neue Visual C#, das.NET Framework 4.5 ausgerichtet. Nennen Sie das Projekt "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Ersetzen Sie den Inhalt von "Program.cs" durch den folgenden Code. Die **SampleEventSourceWriter** -Klasse implementiert vier Protokollierungsmethoden: **SendEnums**, **MessageMethod**, **SetOther** und **HighFreq**. Der erste Parameter der Methode WriteEvent definiert die ID für das jeweilige Ereignis. Die Run-Methode implementiert eine Endlosschleife, die jeweils den Protokollierungsmethoden 10 Sekunden in der **SampleEventSourceWriter** -Klasse implementiert wird.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
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

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

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
        }
        }


4.  Speichern Sie die Datei, und wählen Sie **Projektmappe** Code erstellen im Menü **Erstellen** .


### <a name="step-3-deploy-your-application"></a>Schritt 3: Bereitstellen der Anwendung
1.  Mit der rechten Maustaste auf das Projekt **WadExampleVM** im **Projektmappen-Explorer** , und wählen Sie **Ordner im Explorer öffnen**.
2.  Navigieren Sie zum *Bin\Debug* -Ordner und kopieren Sie alle Dateien (WadExampleVM.*)
3.  Im **Server-Explorer** mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Remotedesktop verwenden**.
4.  Wenn die VM mit erstellen Sie einen Ordner namens WadExampleVM und in den Ordner Anwendungsdateien einfügen.
5.  Starten Sie die Anwendung WadExampleVM.exe. Ein leeres Konsolenfenster sollte angezeigt werden.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Schritt 4: Erstellen Sie Diagnose-Konfiguration und installieren Sie die Erweiterung
1.  Herunterladen der Schemadefinition öffentliche Konfiguration Datei auf dem Entwicklungscomputer folgenden PowerShell-Befehl ausführen:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Öffnen Sie eine neue XML-Datei in Visual Studio entweder in ein Projekt bereits geöffnet oder in einer Visual Studio-Instanz keine Projekte geöffnet. Wählen Sie in Visual Studio **Hinzufügen** -> **Neues Element...**  ->  **Visual C# Elemente** -> **Daten** -> **XML-Datei**. Benennen Sie die Datei "WadExample.xml"
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

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Schritt 5: Remoteinstallation Diagnose Ihre Azure Virtual Machine
Die PowerShell-Cmdlets zur Verwaltung von Diagnose für eine VM sind: Set AzureVMDiagnosticsExtension Get-AzureVMDiagnosticsExtension und AzureVMDiagnosticsExtension entfernen.

1.  Öffnen Sie auf Ihrem Entwicklercomputer Azure PowerShell.
2.  Führen Sie das Skript Remoteinstallation Diagnose auf die VM (ersetzen Sie *StorageAccountKey* durch den Speicher kontoschlüssel für das Speicherkonto Wadexamplevm):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Schritt 6: Betrachten Sie die Telemetriedaten
Navigieren Sie im Visual Studio- **Server-Explorer** Wadexample Speicherkonto. Die VM ca. 5 Minuten aktiv sollte Tabellen, **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** und **WADSetOtherTable**angezeigt werden. Doppelklicken Sie auf eines der Telemetriedaten anzeigen, die gesammelt wurden.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Konfigurationsdateischema

Diagnose-Konfigurationsdatei definiert Werte, mit denen Diagnose Konfigurationen zu initialisieren, wenn Diagnose-Agent gestartet wird. Finden Sie [Referenzinformationen zum aktuellen](https://msdn.microsoft.com/library/azure/mt634524.aspx) gültigen Werte und Beispiele.

## <a name="troubleshooting"></a>Problembehandlung

Weitere Informationen finden Sie unter [Problembehandlung bei Azure-Diagnose](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Nächste Schritte
So ändern Sie die Daten, die Sie sammeln, [sehen Sie eine Liste der virtuellen Computer verwandte Azure Diagnostics Artikel](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) Problembehandlung oder erfahren Sie mehr über Diagnose im Allgemeinen.


[Quelle-Klasse]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Kostenlose Testversion]: http://azure.microsoft.com/pricing/free-trial/
[Installieren und Konfigurieren von Azure PowerShell Version 0.8.7 oder höher]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
