<properties
    pageTitle="Automatische Patches für SQL Server VMs (klassisch) | Microsoft Azure"
    description="Das Feature automatische Patches erläutert für SQL Server virtuellen Computern in Azure Modus classic Bereitstellung."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatische Patches für SQL Server in Azure virtuellen Maschinen (klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-patching.md)
- [Classic](virtual-machines-windows-classic-sql-automated-patching.md)

Automatische Patches richtet ein Wartungsfenster für ein Azure virtuelle Computer mit SQL Server. Automatische Updates können nur in diesem Zeitraum installiert werden. Für SQL Server sorgt für das System-Updates und alle zugehörigen Neustarts Zeitpunkt am besten für die Datenbank. Automatische Patches hängt von der [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Der Ressourcen-Manager-Version dieses Artikels finden Sie unter [Automatische Patches für SQL Server in Azure virtuelle Maschinen Ressourcen-Manager](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um automatische Patches verwenden, beachten Sie die folgenden Komponenten:

**Betriebssystem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Installieren Sie die neuesten Azure PowerShell-Befehle](../powershell-install-configure.md).

**SQL Server IaaS-Erweiterung**:

- [Installieren Sie SQL Server IaaS-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

Die folgende Tabelle beschreibt die Optionen für automatische Patches konfiguriert werden können. Für klassische VMs verwenden Sie PowerShell diese Einstellungen.

|Einstellung|Mögliche Werte|Beschreibung|
|---|---|---|
|**Automatische Patches**|Aktivieren Sie (deaktiviert)|Aktiviert bzw. deaktiviert automatische Patches für Azure Virtual Machine.|
|**Wartungsplan**|Täglich, Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag|Der Zeitplan zum Downloaden und Installieren von Windows, SQL Server und Microsoft Updates für den virtuellen Computer.|
|**Wartung Startstunde**|0-24|Die lokale Starten der virtuellen Computer aktualisiert.|
|**Wartung Fensterdauer**|30-180|Die Anzahl der Minuten erlaubt, Download und Installation der Updates abzuschließen.|
|**Patchkategorie**|Wichtig|Die Kategorie des Updates herunterladen und installieren.|

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Im folgenden Beispiel dient PowerShell automatisiert Patches auf eine vorhandene SQL Server-VM konfigurieren. Der Befehl **AzureVMSqlServerAutoPatchingConfig neu** konfiguriert eine neue Wartungsfenster für automatische Updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Anhand dieses Beispiels werden in der folgenden Tabelle praktische Auswirkung auf das Ziel Azure VM beschrieben:

|Parameter|Effekt|
|---|---|
|**DayOfWeek**|Patches installiert jeden Donnerstag.|
|**MaintenanceWindowStartingHour**|Beginn um 11 Uhr aktualisiert.|
|**MaintenanceWindowsDuration**|Patches installiert innerhalb von 120 Minuten. Basierend auf den Zeitpunkt, müssen sie nach 1:00 Uhr.|
|**PatchCategory**|Die einzige Einstellung für diesen Parameter ist "Wichtig".|

Installieren und Konfigurieren von SQL Server IaaS-Agent mehrere Minuten kann dauern.

Führen, um automatische Patches deaktivieren dasselbe Skript ohne den Parameter aktivieren, neu-AzureVMSqlServerAutoPatchingConfig. Wie bei der Installation kann es deaktiviert automatische Patches dauern.

## <a name="next-steps"></a>Nächste Schritte

Informationen über andere verfügbare Automatisierungsaufgaben finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

Weitere Informationen zum Ausführen von SQL Server auf Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
