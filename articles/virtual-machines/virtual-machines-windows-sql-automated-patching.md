<properties
    pageTitle="Automatische Patches für SQL Server VMs (Resource Manager) | Microsoft Azure"
    description="Erläutert die Funktion Automatische Patches für SQL Server virtueller Maschinen in Azure mit Ressourcen-Manager."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatische Patches für SQL Server in Azure virtuellen Maschinen (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-patching.md)
- [Classic](virtual-machines-windows-classic-sql-automated-patching.md)

Automatische Patches richtet ein Wartungsfenster für ein Azure virtuelle Computer mit SQL Server. Automatische Updates können nur in diesem Zeitraum installiert werden. Für SQL Server sorgt für diese Rescriction System-Updates und alle zugehörigen Neustarts Zeitpunkt am besten für die Datenbank. Automatische Patches hängt von der [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell. Die klassische Version dieses Artikels finden Sie unter [Automatische Patches für SQL Server in Azure VMs Classic](virtual-machines-windows-classic-sql-automated-patching.md).

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

- [Installieren Sie die neuesten Azure PowerShell Befehle](../powershell-install-configure.md) automatisiert Patches mit PowerShell konfiguriert werden soll.

>[AZURE.NOTE] Automatische Patches basiert auf SQL Server IaaS-Agent-Erweiterung. Aktuelle SQL virtuellen Galeriebilder Hinzufügen dieser Erweiterung standardmäßig. Weitere Informationen finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

Die folgende Tabelle beschreibt die Optionen für automatische Patches konfiguriert werden können. Die Konfiguration abhängen, ob Azure-Portal oder Azure Windows PowerShell-Befehle verwenden.

|Einstellung|Mögliche Werte|Beschreibung|
|---|---|---|
|**Automatische Patches**|Aktivieren Sie (deaktiviert)|Aktiviert bzw. deaktiviert automatische Patches für Azure Virtual Machine.|
|**Wartungsplan**|Täglich, Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag|Der Zeitplan zum Downloaden und Installieren von Windows, SQL Server und Microsoft Updates für den virtuellen Computer.|
|**Wartung Startstunde**|0-24|Die lokale Starten der virtuellen Computer aktualisiert.|
|**Wartung Fensterdauer**|30-180|Die Anzahl der Minuten erlaubt, Download und Installation der Updates abzuschließen.|
|**Patchkategorie**|Wichtig|Die Kategorie des Updates herunterladen und installieren.|

## <a name="configuration-in-the-portal"></a>Konfiguration des Portals
Azure-Portal können Sie automatische Patches während der Bereitstellung oder für vorhandene virtuelle Computer konfigurieren.

### <a name="new-vms"></a>Neuer VMs
Verwenden des Azure-Portals automatische Patches konfigurieren, wenn ein neuer virtueller Computer für SQL Server in der Ressourcen-Manager-Bereitstellungsmodell erstellen.

**SQL Server Settings** Blatt wählen Sie **Automatische Patches aus** Der folgende Azure Portal Screenshot zeigt **SQL automatische Patch** -Blade.

![SQL Azure Portal Patches automatisiert](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Kontext finden Sie vollständig auf die [Bereitstellung eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Vorhandene VMs
Vorhandenen virtuellen Computer SQL Server wählen Sie den virtuellen SQL Server-Computer. Dann wählen Sie **SQL Server-Konfiguration** des Blade- **Einstellungen** .

![SQL automatisch gepatcht vorhandener VMs](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Blatt **SQL Server-Konfiguration** klicken Sie auf die Schaltfläche **Bearbeiten** im Abschnitt Patches automatisiert.

![SQL automatische Patches für vorhandene virtuelle Computer konfigurieren](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Klicken Sie abschließend auf die Schaltfläche **OK** auf der Unterseite des Blades **SQL Server-Konfiguration** zu ändern.

Wenn Sie automatische Patches zum ersten Mal aktivieren, konfiguriert Azure SQL Server IaaS-Agent im Hintergrund. Während dieser Zeit kann das Azure-Portal nicht anzeigen automatische Patches konfiguriert ist Warten Sie einige Minuten für den Agent installiert, konfiguriert. Danach gibt das Azure-Portal die neue Einstellung.

>[AZURE.NOTE] Sie können auch automatische Patches mithilfe einer Vorlage konfigurieren. Weitere Informationen finden Sie unter [Azure Schnellstart-Vorlage für automatische Patches](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Nach der Bereitstellung der VM SQL mithilfe von PowerShell automatisiert Patches konfigurieren.

Im folgenden Beispiel dient PowerShell automatisiert Patches auf eine vorhandene SQL Server-VM konfigurieren. **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** -Befehl wird eine neue Wartungsfenster für automatische Updates konfiguriert.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Anhand dieses Beispiels werden in der folgenden Tabelle praktische Auswirkung auf das Ziel Azure VM beschrieben:

|Parameter|Effekt|
|---|---|
|**DayOfWeek**|Patches installiert jeden Donnerstag.|
|**MaintenanceWindowStartingHour**|Beginn um 11 Uhr aktualisiert.|
|**MaintenanceWindowsDuration**|Patches installiert innerhalb von 120 Minuten. Basierend auf den Zeitpunkt, müssen sie nach 1:00 Uhr.|
|**PatchCategory**|Die einzige Einstellung für diesen Parameter ist **Wichtig**.|

Installieren und Konfigurieren von SQL Server IaaS-Agent mehrere Minuten kann dauern.

Deaktivieren Sie automatische patchen, ausgeführt werden ohne die **-aktiviert** Parameter **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Das Fehlen der **-aktiviert** -Parameter signalisiert den Befehl zum Deaktivieren der Funktion.

## <a name="next-steps"></a>Nächste Schritte

Informationen über andere verfügbare Automatisierungsaufgaben finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md).

Weitere Informationen zum Ausführen von SQL Server auf Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
