<properties
    pageTitle="Automatisches Backup für SQL Server virtuelle Maschinen (Resource Manager) | Microsoft Azure"
    description="Erläutert das automatische Sicherungsfeature für SQL Server in Azure virtuelle Maschinen mit Ressourcen-Manager ausgeführt. "
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
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatisches Backup für SQL Server in Azure virtuellen Maschinen (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-backup.md)
- [Classic](virtual-machines-windows-classic-sql-automated-backup.md)

Automatische Sicherung konfiguriert [Backup auf Microsoft Azure verwaltet](https://msdn.microsoft.com/library/dn449496.aspx) automatisch für alle vorhandenen und neuen Datenbanken eine Azure VM SQL Server 2014 Standard oder Enterprise ausgeführt. Dadurch können Sie reguläre Datenbank Backups konfigurieren, die robuste Azure BLOB-Speicher nutzen. Automatische Sicherung hängt von der [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell. Die klassische Version dieses Artikels finden Sie unter [Automatische Sicherung für SQL Server in Azure VMs Classic](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um automatische Sicherung verwenden, sollten Sie die folgenden Komponenten:

**Betriebssystem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version/Edition**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Konfiguration**:

- Zieldatenbanken müssen das vollständige Wiederherstellungsmodell verwenden.

**Azure PowerShell**:

- [Installieren Sie die neuesten Azure PowerShell-Befehlen](../powershell-install-configure.md) , automatisierte Sicherung mit PowerShell konfiguriert werden soll.

>[AZURE.NOTE] Automatische Sicherung basiert auf SQL Server IaaS-Agent-Erweiterung. Aktuelle SQL virtuellen Galeriebilder Hinzufügen dieser Erweiterung standardmäßig. Weitere Informationen finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

Die folgende Tabelle beschreibt die Optionen für automatische Sicherung konfiguriert werden können. Die Konfiguration abhängen, ob Azure-Portal oder Azure Windows PowerShell-Befehle verwenden.

|Einstellung|Bereich (Standard)|Beschreibung|
|---|---|---|
|**Automatische Sicherung**|Aktivieren Sie (deaktiviert)|Aktiviert bzw. deaktiviert automatische Sicherung für eine Azure-VM SQL Server 2014 Standard oder Enterprise ausgeführt.|
|**Aufbewahrungsdauer**|1-30 (30 Tage)|Die Anzahl der Tage zum Aufbewahren einer Sicherung.|
|**Konto**|Azure Storage-Konto (das Speicherkonto für die angegebene VM erstellt)|Ein Azure Storage-Konto zum Speichern von Sicherungskopien automatisiert im BLOB-Speicher. Ein Container ist an diesem Speicherort speichern alle Sicherungsdateien erstellt. Die Namenskonvention Sicherungsdatei enthält Datum, Zeit und Name des Computers.|
|**Verschlüsselung**|Aktivieren Sie (deaktiviert)|Aktiviert oder deaktiviert die Verschlüsselung. Bei aktivierter Verschlüsselung befinden sich Zertifikate zum Wiederherstellen der Sicherung im angegebenen Speicherkonto in einem Automaticbackup-Container verwenden dieselbe Namenskonvention. Ändert das Kennwort ein neues Zertifikats mit diesem Kennwort generiert, das alte Zertifikat bleibt jedoch ältere Sicherungskopien wiederherstellen.|
|**Kennwort**|Kennworttext (keine)|Ein Kennwort für den Verschlüsselungsschlüssel. Dies ist nur erforderlich, wenn die Verschlüsselung aktiviert ist. Um eine verschlüsselte Sicherung wiederherstellen, müssen Sie das Passwort und zugehörige Zertifikat, die zum Zeitpunkt der Sicherung verwendet wurde.|

## <a name="configuration-in-the-portal"></a>Konfiguration des Portals
Azure-Portal können Sie automatisierte Sicherung während der Bereitstellung oder vorhandene virtuelle Computer konfigurieren.

### <a name="new-vms"></a>Neuer VMs
Mit der automatisierten Sicherung konfigurieren Sie beim Erstellen einer neuen SQL Server 2014 virtuellen Computer in der Ressourcen-Manager-Bereitstellungsmodell Azure-Portal.

Wählen Sie **SQL Server Settings** Blade **Automatisches Backup**. Der folgende Azure Portal Screenshot zeigt **SQL automatisierte Backup** -Blade.

![SQL automatisierte Backup-Konfiguration in Azure-portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontext finden Sie vollständig auf die [Bereitstellung eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Vorhandene VMs
Vorhandenen virtuellen Computer SQL Server wählen Sie den virtuellen SQL Server-Computer. Dann wählen Sie **SQL Server-Konfiguration** des Blade- **Einstellungen** .

![SQL automatisierte Backups für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Blatt **SQL Server-Konfiguration** klicken Sie auf die Schaltfläche **Bearbeiten** im Abschnitt für die automatische Sicherung.

![Automatisierte Sicherung SQL für vorhandene virtuelle Computer konfigurieren](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Klicken Sie abschließend auf die Schaltfläche **OK** auf der Unterseite des Blades **SQL Server-Konfiguration** zu ändern.

Wenn Sie zum ersten Mal automatische Sicherung aktivieren, konfiguriert Azure SQL Server IaaS-Agent im Hintergrund. Während dieser Zeit möglicherweise Azure-Portal konfiguriert automatische Sicherung nicht angezeigt. Warten Sie einige Minuten für den Agent installiert, konfiguriert. Danach wird das Azure-Portal neuen Einstellungen widerspiegeln.

>[AZURE.NOTE] Sie können auch automatische Sicherung mit einer Vorlage. Weitere Informationen finden Sie unter [Azure Schnellstart-Vorlage für automatische Sicherung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Nach der Bereitstellung der VM SQL mithilfe von PowerShell Automatisierte Sicherung konfigurieren.

Im folgenden Beispiel PowerShell ist automatische Sicherung für eine vorhandene SQL Server 2014 VM konfiguriert. Der Befehl **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** konfiguriert die Einstellungen automatische Sicherung Sicherungen in Azure Storage-Konto mit dem virtuellen Computer. Diese Sicherung werden 10 Tage aufbewahrt werden. Der Befehl **Set AzureRmVMSqlServerExtension** aktualisiert angegebenen Azure VM zu.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Installieren und Konfigurieren von SQL Server IaaS-Agent mehrere Minuten kann dauern.

Ändern Sie zum Aktivieren der Verschlüsselung das vorherige Skript Parameter **EnableEncryption** mit Kennwort (sichere Zeichenfolge) für den **CertificatePassword** -Parameter übergeben. Das folgende Skript ermöglicht die automatische Sicherung Einstellungen im vorherigen Beispiel und verschlüsselt.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Deaktivieren Sie die automatische Sicherung ausgeführt werden ohne die **-aktiviert** Parameter für den Befehl **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** . Das Fehlen der **-aktiviert** -Parameter signalisiert den Befehl zum Deaktivieren der Funktion. Wie bei der Installation können sie automatische Sicherung deaktivieren dauern.

>[AZURE.NOTE] Entfernen von SQL Server IaaS-Agent entfernt nicht konfigurierten automatisierte Backup-Einstellungen. Deaktivieren Sie automatische Sicherung deaktivieren oder Deinstallieren der SQL Server-IaaS-Agenten.

## <a name="next-steps"></a>Nächste Schritte

Automatische Sicherung konfiguriert Managed Backup auf Azure VMs. So ist es wichtig, [Lesen Sie die Dokumentation für verwaltete Sicherung](https://msdn.microsoft.com/library/dn449496.aspx) Verhalten und folgen.

Zusätzliche sichern und Wiederherstellen Anleitung für SQL Server auf Azure VMs im folgenden Thema: [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informationen über andere verfügbare Automatisierungsaufgaben finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md).

Weitere Informationen zum Ausführen von SQL Server auf Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
