<properties
    pageTitle="Übersicht über Azure Diagnostics | Microsoft Azure"
    description="Verwenden Sie Azure Diagnostics für Debuggen, Leistung, Überwachung, Analyse Cloud-Diensten und virtuellen Maschinen Service fabric"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Was ist Microsoft Azure-Diagnose


Azure-Diagnose ist eine Funktion in Azure, die Sammlung der Diagnosedaten in einer bereitgestellten Anwendung ermöglicht. Die diagnoseerweiterung können Sie aus verschiedenen Quellen. Derzeit sind Azure Cloud Service Web- und Workerrollen Azure virtuelle Computer unter Microsoft Windows und Service Fabric. Andere Azure-Dienste haben ihre eigenen separaten Diagnose.

## <a name="data-you-can-collect"></a>Sie sammeln Daten

Azure-Diagnose können folgende Daten sammeln:

Datenquelle|Beschreibung
---|---
Leistungsindikatoren | Betriebssystem und benutzerdefinierte Leistungsindikatoren
Anwendungsprotokolle     | Verfolgen von Nachrichten von der Anwendung
Windows-Ereignisprotokolle   | Informationen zu Windows-Ereignis Protokollierungssystem
.NET Quelle    | Code mit .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) -Klasse schreiben
IIS-Protokolle             | Informationen zu IIS-Websites
Manifest-ETW-Basis   | Event Tracing for Windows Ereignissen von einem Prozess
Speicherabbilder          | Informationen über den Zustand des Prozesses bei einem Absturz der Anwendung
Benutzerdefinierte Protokolle    | Durch die Anwendung oder den Dienst erstellten Protokolle
Azure Diagnoseinfrastruktur Protokolle|Informationen zur Diagnose selbst

Azure Diagnostics-Erweiterung kann diese Daten auf ein Azure-Speicher übertragen oder Dienste wie [Application Insights](./application-insights/app-insights-cloudservices.md)senden. Sie können Daten für Debuggen und Problembehandlung Messen der Leistung, Überwachen der Ressourcenverwendung, Analyse und Planung und Überwachung.


## <a name="versioning"></a>Versionskontrolle
[Azure Diagnostics Versioning Verlauf](azure-diagnostics-versioning-history.md)anzeigen

## <a name="next-steps"></a>Nächste Schritte
Wählen Sie die Dienst auf Diagnoseinformationen sammeln und die folgenden Artikeln Einstieg verwenden möchten. Verwenden Sie die Links allgemeine Azure Diagnostics als Referenz für bestimmte Aufgaben.

## <a name="web-apps"></a>Webapps
Beachten Sie, dass Web Apps Azure-Diagnose nicht verwenden. Finden Sie entsprechende Informationen unter [Web Apps](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Cloud-Diensten mit Azure-Diagnose
- Wenn Visual Studio, [Mit Visual Studio eine Cloud Services-Anwendung verfolgen](./vs-azure-tools-debug-cloud-services-virtual-machines.md) Schritte anzeigen Andernfalls finden Sie unter
- [Überwachen von Cloud-Diensten mit Azure-Diagnose](./cloud-services/cloud-services-how-to-monitor.md)
- [Einrichten von Azure Diagnostics in einer Cloud Services-Anwendung](./cloud-services/cloud-services-dotnet-diagnostics.md)

Themen Sie erweiterte

- [Mithilfe von Azure Diagnostics Anwendung Einblicke für Cloud-Services](./application-insights/app-insights-cloudservices.md)
- [Verfolgen des Ablaufs einer Cloud-Services-Anwendung mit Azure-Diagnose](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Mithilfe von PowerShell Diagnose für Cloud-Dienste einrichten](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuelle Maschinen mit Azure-Diagnose
- Wenn Sie Visual Studio verwenden, [Verwendet Visual Studio verfolgen Azure Virtual Machines](./vs-azure-tools-debug-cloud-services-virtual-machines.md) Schritte anzeigen Andernfalls finden Sie unter
- [Einrichten von Azure Diagnostics ein Azure Virtual Machine](./virtual-machines-dotnet-diagnostics.md)

Themen Sie erweiterte

- [Mithilfe von PowerShell Diagnose auf Azure Virtual Machines einrichten](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Überwachung und Diagnose mit Azure Ressourcenmanager Vorlage erstellen Sie Windows Virtual machine](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Service Stoff Azure-Diagnose
Fangen Sie am [Monitor Service Fabric-Anwendung](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Viele andere Service Fabric Diagnose-Artikel sind in der Navigationsstruktur auf der linken Seite verfügbar für diesen Artikel erhalten.

## <a name="general-azure-diagnostics-articles"></a>Artikel Allgemein Azure-Diagnose
- [Konfiguration von Azure Diagnostics Schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) - Informationen zum Ändern der Schemadatei zum Sammeln und Diagnose Daten. Beachten Sie, dass Sie auch Visual Studio können die Schemadatei ändern.
- [Wie in Azure Storage Daten Azure Diagnostics](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - die Namen der Tabellen und der Blobs, die Diagnosedaten geschrieben.
- Informationen Sie zum [Verwenden von Leistungsindikatoren in Azure-Diagnose](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Informationen Sie zum [Arbeitsplan Azure Diagnoseinformationen Anwendung Einblicke](./azure-diagnostics-configure-applicationinsights.md)
- Haben Sie Probleme mit Diagnose starten oder Suchen von Daten in Azure Tabellen finden Sie unter [Problembehandlung bei Azure-Diagnose](./azure-diagnostics-troubleshooting.md)
