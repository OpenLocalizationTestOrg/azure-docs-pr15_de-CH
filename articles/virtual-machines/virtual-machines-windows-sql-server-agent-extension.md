<properties
    pageTitle="SQL Server-Agent-Erweiterung für SQL Server VMs (Resource Manager) | Microsoft Azure"
    description="Dieses Thema beschreibt die SQL Server-Agent-Erweiterung verwalten, die bestimmte SQL Server-Verwaltungsaufgaben automatisiert. Dazu gehören automatische Sicherung automatische Patches und Integration von Azure Key Vault. Dieses Thema verwendet Bereitstellungsmodus Ressourcenmanager."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server-Agent-Erweiterung für SQL Server VMs (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
- [Classic](virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS-Agent-Erweiterung (SQLIaaSExtension) führt auf Azure virtuelle Maschinen Verwaltungsaufgaben automatisieren. Dieses Thema enthält eine Übersicht über die Dienste unterstützt die Erweiterung sowie Installations-, Status und entfernen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell. Die klassische Version dieses Artikels finden Sie unter [SQL Server-Agent-Erweiterung für SQL Server VMs Classic](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Unterstützte Dienste

SQL Server IaaS-Agent-Erweiterung unterstützt folgende Verwaltungsaufgaben:

| Administration-Funktion | Beschreibung |
|---------------------|-------------------------------|
| **SQL automatisches Backup** | Automatisiert Backup-Planung für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer. Weitere Informationen finden Sie unter [Sicherungsdateien für SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL automatische Patches** | Konfiguriert ein Wartungsfenster während der Updates für die VM stattfinden können Updates während der Spitzenzeiten für Ihre Arbeitslast zu vermeiden. Weitere Informationen finden Sie unter [Automatische Patches für SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).|
| **Integration von Azure Key Vault** | Ermöglicht das automatische Installieren und Konfigurieren von Azure Key Vault auf Ihrem SQL Server-VM. Weitere Informationen finden Sie unter [Konfigurieren Azure Key Vault Integration für SQL Server auf Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Erforderliche Komponenten

Anforderungen SQL Server IaaS-Agent-Erweiterung auf Ihrem virtuellen Computer verwendet:

**Betriebssystem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Versionen**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Herunterladen Sie und konfigurieren Sie die neuesten Azure PowerShell-Befehle](../powershell-install-configure.md)

## <a name="installation"></a>Installation

SQL Server IaaS-Agent-Erweiterung wird automatisch installiert, wenn Sie SQL Server virtuellen Katalog Bilder bereitstellen.

Bei der Erstellung einer virtuellen Maschine nur Betriebssystem Windows Server können Sie die Erweiterung mithilfe des **Set-AzureVMSqlServerExtension** PowerShell-Cmdlets manuell installieren. Beispielsweise der folgende Befehl die Erweiterung für eine VM nur Betriebssystem Windows Server und mit dem Namen "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Aktualisieren auf die neueste Version der Erweiterung SQL IaaS-Agent müssen Sie den virtuellen Computer neu starten nach der Aktualisierung der Erweiterung.

>[AZURE.NOTE] Wenn Sie SQL Server IaaS-Agent-Erweiterung manuell auf einem Windows Server-VM installieren, müssen Sie verwenden und Verwalten von PowerShell Befehle Funktionen. Die Portalschnittstelle steht nur für SQL Server Galeriebilder.

## <a name="status"></a>Status

Eine Möglichkeit um sicherzustellen, dass die Erweiterung installiert ist Agentstatus in Azure-Portal anzeigen. **Alle** Einstellungen in der virtuellen Blade- und klicken Sie dann auf **Erweiterung**. Die aufgeführten **SQLIaaSExtension** Erweiterung sollte angezeigt werden.

![SQL Server Agent IaaS Erweiterung in Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Sie können auch das Azure Powershell-Cmdlet **Get-AzureVMSqlServerExtension** .

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Der vorherige Befehl bestätigt der Agent installiert ist und bietet allgemeine Statusinformationen. Sie erhalten auch Statusinformationen über automatisierte Backups und Patchen mit den folgenden Befehlen.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Entfernen   

Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie auf die Auslassungszeichen auf die **Erweiterung** der Eigenschaften. Klicken Sie auf **Löschen**.

![Deinstallieren Sie SQL Server Agent IaaS-Erweiterung in Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Sie können auch das Powershell-Cmdlet **AzureRmVMSqlServerExtension entfernen** .

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit einer von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie im Abschnitt [Dienste unterstützt](#supported-services) dieses Artikels verwiesen wird.

Weitere Informationen zum Ausführen von SQL Server auf Azure-Computer finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
