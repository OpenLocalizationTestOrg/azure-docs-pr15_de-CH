<properties
    pageTitle="Versionsverlauf Azure Diagnostics"
    description="Erläuterung der ändert sich in den verschiedenen Versionen von Azure Diagnostics als geliefert mit verschiedenen Versionen von Microsoft Azure SDKs."
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
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Microsoft Azure Diagnostics Versionsverlauf

Neu bei Azure Diagnostics? Übersicht der [Azure-Diagnose](azure-diagnostics.md).

Jede Version von Azure SDK ist in der Regel mit einer neuen Version von Azure Diagnostics. Die Tabelle beschreibt die Versionen Azure SDK und Azure-Diagnose die SDK-Version zugeordnet.



Azure SDK-version | Azure Diagnostics-version | Modell
--- | --- | ---
1.x      | 1.0 | Plug-in
2.0 2.4| 1.0 | "
2.5      | 1.2 | Erweiterung
2.6      | 1.3 | "
2.7      | 1.4 | "
2.8      | 1.5 | "


Die neueste Version ist 1.5 Azure SDK 2.8 geliefert. Die Version von Azure Diagnostics-Erweiterung, die mit dem SDK wird nur für lokale Emulator Szenarios verwendet. Bereitgestellte Anwendung verwendet automatisch die neueste Ausführung in Azure, unabhängig von der Version der SDK-Anwendung mit erstellt wurde. Jedoch, wenn Sie das aktuelle Azure SDK installieren, Sie möglicherweise wurden nicht alle Tools, mit denen die neuen Features nutzen.

Verschiedene Features und folgenden.

## <a name="azure-sdk-28"></a>Azure SDK 2.8
Azure SDK 2.8 hinzugefügt Diagnosedaten [Anwendung Einblicke](./application-insights/app-insights-cloudservices.md) in der Anwendung sowie die System- und Infrastruktur Problemdiagnose erleichtern senden.

## <a name="azure-26-diagnostics-changes"></a>2.6 Azure Diagnostics ändern

Azure SDK 2.6 Cloud Service-Projekte in Visual Studio wurden die folgenden Änderungen. (Diese Änderungen gelten auch für spätere Versionen von Azure SDK.)

- Lokale Emulator unterstützt jetzt Diagnose. Dies bedeutet Diagnosedaten sammeln und sicherstellen, dass Ihre Anwendung rechts Spuren während Sie entwickeln und Testen in Visual Studio erstellt. Die Verbindungszeichenfolge `UseDevelopmentStorage=true` Diagnose Datensammlung Cloud-Dienstprojekt in Visual Studio ausführen mit dem Emulator Azure-Speicher bietet. Alle Diagnosedaten werden im Speicherkonto (Development Storage).

- Verbindungszeichenfolge für Diagnose Storage-Konto (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) ist wieder in der Service-Konfigurationsdatei (.cscfg) gespeichert. In Azure SDK 2.5 wurde das Speicherkonto Diagnose in der Datei diagnostics.wadcfgx angegeben.

Es gibt einige wichtige Unterschiede zwischen wie die Verbindungszeichenfolge in Azure SDK 2.4 und früher gearbeitet und Funktionsweise in Azure SDK 2.6 oder höher.

- In Azure SDK 2.4 und früher die Verbindungszeichenfolge eine Laufzeit Diagnose Plug-in diente als Storage-Kontoinformationen für die Übertragung von Diagnoseprotokolle zu.

- In Azure SDK 2.6 und höher ist die Diagnose von Visual Studio verwendet diagnoseerweiterung mit den entsprechenden Informationen während der Veröffentlichung konfigurieren. Die Verbindungszeichenfolge können Sie verschiedene Konten für verschiedene Dienstkonfigurationen definieren, die beim Veröffentlichen von Visual Studio verwenden. Da das Diagnose-Plugin (nach Azure SDK 2.5) nicht mehr verfügbar ist, kann nicht .cscfg Datei allein Diagnose-Erweiterung aktivieren. Sie müssen die Erweiterung separat über PowerShell oder Visual Studio-Tools.

- Vereinfacht die diagnoseerweiterung mit PowerShell konfigurieren enthält die Paket-Ausgabe von Visual Studio öffentliche Konfigurations-XML für die diagnoseerweiterung für jede Rolle. Visual Studio verwendet die Verbindungszeichenfolge Diagnose die speicherkontoinformationen im öffentlichen Konfiguration auffüllen. Die Öffentliche Dateien werden im Ordner "Extensions" erstellt und folgen dem Muster PaaSDiagnostics. <RoleName>. PubConfig.xml. PowerShell basiert Einsatz können dieses Muster jede Konfiguration einer Rolle zuordnen.

- Die Verbindungszeichenfolge in der Datei .cscfg wird auch von Azure-Portal verwendet, die Diagnosedaten auf, damit die Registerkarte **Überwachung** angezeigt werden können. Die Verbindungszeichenfolge muss den Dienst zeigen ausführliche Daten im Portal konfigurieren.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrieren von Projekten auf Azure SDK 2.6 und höher

Migrieren Sie von Azure SDK 2.5 Azure SDK 2.6 oder höher, wäre eine Diagnose Speicherkonto in der Datei .wadcfgx angegeben, dann bleibt gibt es. Um die Flexibilität, verschiedene Konten für verschiedene Konfigurationen nutzen zu können, müssen Sie die Verbindungszeichenfolge manuell zum Projekt hinzufügen. Wenn Sie ein Projekt von Azure SDK 2.4 oder früher in Azure SDK 2.6 migrieren, werden die Verbindungszeichenfolgen Diagnose beibehalten. Beachten Sie jedoch die Änderung wie Verbindungszeichenfolgen in Azure SDK 2.6 wie im vorherigen Abschnitt behandelt werden.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Wie Visual Studio bestimmt das Speicherkonto für die Diagnose

- Wenn eine Diagnose-Verbindungszeichenfolge in der Datei .cscfg angegeben ist, verwendet Visual Studio diagnoseerweiterung beim Veröffentlichen und generieren die öffentlichen XML-Konfigurationsdateien beim Packen konfigurieren.

- Wenn keine Diagnose-Verbindungszeichenfolge in der Datei .cscfg angegeben wird, wird anschließend Visual Studio mit in der Datei .wadcfgx angegebenen Speicherkonto diagnoseerweiterung veröffentlichen und generieren die öffentlichen XML-Konfigurationsdateien beim Konfigurieren.

- Die Diagnose-Verbindungszeichenfolge in der Datei .cscfg Vorrang vor das Speicherkonto in der Datei .wadcfgx. Eine Diagnose-Verbindungszeichenfolge in der Datei .cscfg angegeben ist, dann Visual Studio verwendet wird, und ignoriert das Speicherkonto in .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Was ist die "Update Entwicklung Speicher Verbindungszeichenfolgen..." Kontrollkästchen tun?

Das Kontrollkästchen **Update Entwicklung Speicher Verbindungszeichenfolgen für Diagnose und Zwischenspeichern mit Microsoft Azure Storage Anmeldeinformationen beim Veröffentlichen in Microsoft Azure** bietet eine bequeme Möglichkeit, alle Verbindungszeichenfolgen Entwicklung Storage Konto während der Veröffentlichung angegebenen Azure Storage-Konto aktualisieren.

Angenommen, dieses Kontrollkästchen aktivieren und die Verbindungszeichenfolge Diagnose gibt `UseDevelopmentStorage=true`. Beim Veröffentlichen des Projekts in Azure aktualisiert Visual Studio automatisch Diagnose-Verbindungszeichenfolge das Speicherkonto im Webpublishing-Assistenten angegeben. Jedoch Wenn echte Speicherkonto Diagnose Verbindungszeichenfolge angegeben wurde, wird dann das Konto stattdessen verwendet.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnose Funktionsunterschiede zwischen Azure SDK 2.4 und früher und Azure SDK 2.5 und höher

Wenn Sie Ihr Projekt von Azure SDK 2.4 Azure SDK 2.5 oder höher aktualisieren, sollten Sie Funktionsunterschiede von Diagnose berücksichtigen.

- **Konfigurations-APIs sind veraltet** – programmgesteuerte Konfiguration von Diagnose in Azure SDK 2.4 oder früheren Versionen steht jedoch in Azure SDK 2.5 und höher veraltet ist. Wenn die Diagnosekonfiguration im Code definiert ist, müssen Sie diese Einstellung aus der migrierten Projekt für Diagnose Arbeiten neu konfigurieren. Diagnose-Konfigurationsdatei für Azure SDK 2.4 ist diagnostics.wadcfg und diagnostics.wadcfgx für Azure SDK 2.5 oder höher.

- **Diagnose für Cloudanwendungen Dienst kann nur auf Rollenebene, nicht auf Instanzebene konfiguriert werden.**

- **Die Diagnosekonfiguration wird aktualisiert, jedes Mal, wenn Sie Ihre Anwendung bereitstellen** , können dadurch Parität Fragen Ihre Diagnosekonfiguration im Server-Explorer ändern und dann erneut die app.

- **In der Konfigurationsdatei Diagnose nicht im Code in Azure SDK 2.5 und höher, Crash Dumps konfiguriert sind** – Speicherabbilder im Code konfiguriert haben, müssen Sie die Konfiguration manuell aus dem Code in die Konfigurationsdatei übertragen da die Speicherabbilder während der Migration in Azure SDK 2.6 übertragen werden nicht.
