<properties
    pageTitle="Diagnosedaten in Azure-Speicher speichern und anzeigen | Microsoft Azure"
    description="Azure-Diagnosedaten in Azure Storage und anzeigen"
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
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Speichern und anzeigen Diagnosedaten in Azure-Speicher

Diagnosedaten werden nicht dauerhaft gespeichert, sofern Sie Microsoft Azure Speicheremulator oder Azure-Speicher übertragen. Einmal im Speicher, es mit mehreren Werkzeugen ersichtlich.

## <a name="specify-a-storage-account"></a>Geben Sie ein Speicherkonto

Sie geben das Speicherkonto in die Datei ServiceConfiguration.cscfg verwenden möchten. Die Kontoinformationen bezeichnet eine Verbindungszeichenfolge konfiguriert. Das folgende Beispiel zeigt die Standardverbindungszeichenfolge für eine neue Cloud-Dienst-Projekt in Visual Studio erstellt:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Sie können diese Verbindungszeichenfolge Kontoinformationen für ein Azure-Speicher-Konto zu ändern.

Je nach Typ der Daten, die gesammelt wird, verwendet Azure-Diagnose der BLOB-Dienst oder den Dienst. Die folgende Tabelle zeigt die Datenquellen, die beibehalten werden und deren Format.

|Datenquelle|Speicherformat|
|---|---|
|Azure-Protokolle|Tabelle|
|IIS 7.0-Protokolle|BLOB|
|Azure Diagnoseprotokolle Infrastruktur|Tabelle|
|Fehler bei Anforderung Ablaufverfolgungsprotokolle|BLOB|
|Windows-Ereignisprotokolle|Tabelle|
|Leistungsindikatoren|Tabelle|
|Speicherabbilder|BLOB|
|Benutzerdefinierte Protokolle|BLOB|

## <a name="transfer-diagnostic-data"></a>Daten übertragen

SDK 2.5 und höher kann die Anforderung zum Übertragen von Daten über die Konfigurationsdatei auftreten. Sie können Daten in regelmäßigen Abständen gemäß der Konfiguration übertragen.

SDK 2.4 und vorherigen Wunsch Diagnosedaten programmgesteuert über die Konfigurationsdatei sowie übertragen. Der programmgesteuerte Ansatz ermöglicht Ihnen auf Anforderung übertragen.


>[AZURE.IMPORTANT] Bei der Übertragung von Daten in Azure Speicherkonto entstehen Kosten für Speicherressourcen, die Diagnosedaten verwendet.

## <a name="store-diagnostic-data"></a>Daten speichern

Daten werden im Blob oder Tabelle Speicher mit den folgenden Namen gespeichert:

**Tabellen**

- **WadLogsTable** - Protokolle mithilfe des Ablaufverfolgungslisteners im Code geschrieben.

- **WADDiagnosticInfrastructureLogsTable** - Diagnose Monitor und Konfiguration ändert.

- **WADDirectoriesTable** -Verzeichnisse, die Diagnose Monitor überwacht werden.  Dies schließt IIS-Protokolle, IIS Anforderungsprotokolle und benutzerdefinierte Verzeichnisse ist fehlgeschlagen.  Der Speicherort der Protokolldatei Blob im Feld Container angegeben und der Name des Blob im Feld RelativePath.  Das Feld AbsolutePath gibt den Speicherort und Namen der Datei auf dem Azure virtuellen Computer vorhanden war.

- **WADPerformanceCountersTable** -Leistungsindikatoren.

- **WADWindowsEventLogsTable** – Windows-Ereignisprotokollen.

**BLOBs**

- **Bündel Steuerelementcontainer** enthält (nur für SDK 2.4 und vorherige) XML-Konfigurationsdateien, die Azure Diagnostics steuert.

- **Bündel-Iis-Failedreqlogfiles** – meldet enthält Informationen aus IIS-Anforderung ist fehlgeschlagen.

- **Bündel-Iis-Protokolldateien** – protokolliert Informationen über IIS.

- **"custom"** – benutzerdefinierte Container basierend auf Konfigurieren Verzeichnisse von Diagnose Monitor überwacht werden.  Der Name des diesem BLOB-Container wird in WADDirectoriesTable angegeben werden.

## <a name="tools-to-view-diagnostic-data"></a>Tools zum Anzeigen von Diagnosedaten
Verschiedene Tools stehen zum Anzeigen der Daten nach der Übertragung zu speichern. Zum Beispiel:

- Server-Explorer in Visual Studio - Wenn der Azure-Tools für Microsoft Visual Studio installiert haben Azure Storage Node im Server-Explorer können Sie von Azure-Speicherkonten nur-Lese-Blob und Tabellendaten anzeigen. Sie können Daten aus dem lokalen Speicher Emulator Konto anzeigen und auch von Speicherkonten erstellt haben für Azure. Weitere Informationen finden Sie unter [Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist eine eigenständige Anwendung, die Ihnen ermöglicht, problemlos mit Azure-Speicher unter Windows, OSX und Linux.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) enthält Azure Diagnostics Manager können Sie anzeigen, herunterladen und verwalten Diagnosedaten auf Windows Azure ausgeführte Anwendung.


## <a name="next-steps"></a>Nächste Schritte

[Verfolgen Sie den Datenfluss in einer Cloud-Services-Anwendung mit Azure-Diagnose](cloud-services-dotnet-diagnostics-trace-flow.md)
