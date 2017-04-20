<properties
    pageTitle="Automatisches Backup für SQL Server virtuelle Maschinen (klassisch) | Microsoft Azure"
    description="Erläutert das automatische Sicherungsfeature für SQL Server in Azure virtuelle Maschinen mit Ressourcen-Manager ausgeführt. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatisierte Sicherung für SQL Server in Azure virtuellen Maschinen (klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-backup.md)
- [Classic](virtual-machines-windows-classic-sql-automated-backup.md)

Automatische Sicherung konfiguriert [Backup auf Microsoft Azure verwaltet](https://msdn.microsoft.com/library/dn449496.aspx) automatisch für alle vorhandenen und neuen Datenbanken eine Azure VM SQL Server 2014 Standard oder Enterprise ausgeführt. Dadurch können Sie reguläre Datenbank Backups konfigurieren, die robuste Azure BLOB-Speicher nutzen. Automatische Sicherung hängt von der [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Der Ressourcen-Manager-Version dieses Artikels finden Sie unter [Automatische Sicherung für SQL Server in Azure virtuelle Maschinen Ressourcen-Manager](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Um automatische Sicherung verwenden, sollten Sie die folgenden Komponenten:

**Betriebssystem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version/Edition**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

>[AZURE.NOTE] SQL Server 2016 wird für die automatische Sicherung noch nicht unterstützt.

**Konfiguration**:

- Zieldatenbanken müssen das vollständige Wiederherstellungsmodell verwenden.

**Azure PowerShell**:

- [Installieren Sie die neuesten Azure PowerShell-Befehle](../powershell-install-configure.md).

**SQL Server IaaS-Erweiterung**:

- [Installieren Sie SQL Server IaaS-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

Die folgende Tabelle beschreibt die Optionen für automatische Sicherung konfiguriert werden können. Für klassische VMs verwenden Sie PowerShell diese Einstellungen.

|Einstellung|Bereich (Standard)|Beschreibung|
|---|---|---|
|**Automatische Sicherung**|Aktivieren Sie (deaktiviert)|Aktiviert bzw. deaktiviert automatische Sicherung für eine Azure-VM SQL Server 2014 Standard oder Enterprise ausgeführt.|
|**Aufbewahrungsdauer**|1-30 (30 Tage)|Die Anzahl der Tage zum Aufbewahren einer Sicherung.|
|**Konto**|Azure Storage-Konto (das Speicherkonto für die angegebene VM erstellt)|Ein Azure Storage-Konto zum Speichern von Sicherungskopien automatisiert im BLOB-Speicher. Ein Container ist an diesem Speicherort speichern alle Sicherungsdateien erstellt. Die Namenskonvention Sicherungsdatei enthält Datum, Zeit und Name des Computers.|
|**Verschlüsselung**|Aktivieren Sie (deaktiviert)|Aktiviert oder deaktiviert die Verschlüsselung. Bei aktivierter Verschlüsselung befinden sich Zertifikate zum Wiederherstellen der Sicherung im angegebenen Speicherkonto in einem Automaticbackup-Container verwenden dieselbe Namenskonvention. Ändert das Kennwort ein neues Zertifikats mit diesem Kennwort generiert, das alte Zertifikat bleibt jedoch ältere Sicherungskopien wiederherstellen.|
|**Kennwort**|Kennworttext (keine)|Ein Kennwort für den Verschlüsselungsschlüssel. Dies ist nur erforderlich, wenn die Verschlüsselung aktiviert ist. Um eine verschlüsselte Sicherung wiederherstellen, müssen Sie das Passwort und zugehörige Zertifikat, die zum Zeitpunkt der Sicherung verwendet wurde.|

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Im folgenden Beispiel PowerShell ist automatische Sicherung für eine vorhandene SQL Server 2014 VM konfiguriert. Der Befehl **Neu AzureVMSqlServerAutoBackupConfig** konfiguriert die Einstellungen automatische Sicherung Sicherungen in Azure Storage Konto durch die $storageaccount-Variable angegeben. Diese Sicherung werden 10 Tage aufbewahrt werden. Der Befehl **Set AzureVMSqlServerExtension** aktualisiert angegebenen Azure VM zu.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Installieren und Konfigurieren von SQL Server IaaS-Agent mehrere Minuten kann dauern.

Ändern Sie zum Aktivieren der Verschlüsselung das vorherige Skript Parameter EnableEncryption mit Kennwort (sichere Zeichenfolge) für den CertificatePassword-Parameter übergeben. Das folgende Skript ermöglicht die automatische Sicherung Einstellungen im vorherigen Beispiel und verschlüsselt.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Deaktivieren Sie die automatische Sicherung ausgeführt werden ohne die **-aktiviert** Parameter **Neu AzureVMSqlServerAutoBackupConfig**. Wie bei der Installation können sie automatische Sicherung deaktivieren dauern.

>[AZURE.NOTE] Deaktivieren und Deinstallieren von SQL Server IaaS-Agent entfernt zuvor konfigurierten Managed Backup-Einstellungen nicht. Deaktivieren Sie automatische Sicherung deaktivieren oder Deinstallieren der SQL Server-IaaS-Agenten.

## <a name="next-steps"></a>Nächste Schritte

Automatische Sicherung konfiguriert Managed Backup auf Azure VMs. So ist es wichtig, [Lesen Sie die Dokumentation für verwaltete Sicherung](https://msdn.microsoft.com/library/dn449496.aspx) Verhalten und folgen.

Zusätzliche sichern und Wiederherstellen Anleitung für SQL Server auf Azure VMs im folgenden Thema: [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

Informationen über andere verfügbare Automatisierungsaufgaben finden Sie unter [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

Weitere Informationen zum Ausführen von SQL Server auf Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
