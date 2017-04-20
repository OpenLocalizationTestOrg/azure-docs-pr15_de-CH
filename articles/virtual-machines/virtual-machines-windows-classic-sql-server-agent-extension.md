<properties
    pageTitle="SQL Server-Agent-Erweiterung für SQL Server VMs (klassisch) | Microsoft Azure"
    description="Dieses Thema beschreibt die SQL Server-Agent-Erweiterung verwalten, die bestimmte SQL Server-Verwaltungsaufgaben automatisiert. Dazu gehören automatische Sicherung automatische Patches und Integration von Azure Key Vault. Dieses Thema verwendet das klassische Bereitstellungsmodus."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server-Agent-Erweiterung für SQL Server VMs (klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
- [Classic](virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS-Agent-Erweiterung (SQLIaaSAgent) führt auf Azure virtuelle Maschinen Verwaltungsaufgaben automatisieren. Dieses Thema enthält eine Übersicht über die Dienste unterstützt die Erweiterung sowie Installations-, Status und entfernen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Der Ressourcen-Manager-Version dieses Artikels finden Sie unter [SQL Server-Agent-Erweiterung für SQL Server VMs-Manager](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Unterstützte Dienste

SQL Server IaaS-Agent-Erweiterung unterstützt folgende Verwaltungsaufgaben:

| Administration-Funktion | Beschreibung |
|---------------------|-------------------------------|
| **SQL automatisches Backup** | Automatisiert Backup-Planung für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer. Weitere Informationen finden Sie unter [Automatisches Backup für SQL Server in Azure Virtual Machines (klassisch)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL automatische Patches** | Konfiguriert ein Wartungsfenster während der Updates für die VM stattfinden können Updates während der Spitzenzeiten für Ihre Arbeitslast zu vermeiden. Weitere Informationen finden Sie unter [Automatische Patches für SQL Server in Azure Virtual Machines (klassisch)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Integration von Azure Key Vault** | Ermöglicht das automatische Installieren und Konfigurieren von Azure Key Vault auf Ihrem SQL Server-VM. Weitere Informationen finden Sie unter [Konfigurieren Azure Key Vault Integration für SQL Server auf Azure VMs (klassisch)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Erforderliche Komponenten

Anforderungen SQL Server IaaS-Agent-Erweiterung auf Ihrem virtuellen Computer verwendet:

### <a name="operating-system"></a>Betriebssystem:

- Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>SQL Server-Versionen:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:

[Herunterladen und konfigurieren die neuesten Azure PowerShell-Befehle](../powershell-install-configure.md).

Starten Sie Windows PowerShell und Azure Abonnements mit dem Befehl **Add-AzureAccount** an.

    Add-AzureAccount

Sie mehrere Abonnements verwenden **Wählen Sie AzureSubscription** das Abonnement aus, das Ziel enthält, klassische VM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Zu diesem Zeitpunkt erhalten Sie eine Liste der klassischen VMs und zugeordneten Namen mit dem Befehl **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Installation

Für klassische VMs verwenden Sie PowerShell SQL Server IaaS-Agent-Erweiterung installieren und die damit verbundenen Dienste konfigurieren. Verwenden Sie **Set AzureVMSqlServerExtension** PowerShell-Cmdlets für die Erweiterung. Beispielsweise der folgende Befehl die Erweiterung auf einem Windows Server-VM (klassisch) und mit dem Namen "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Aktualisieren auf die neueste Version der Erweiterung SQL IaaS-Agent müssen Sie den virtuellen Computer neu starten nach der Aktualisierung der Erweiterung.

>[AZURE.NOTE] Klassische virtuelle Computer keine Option zum Installieren und Konfigurieren von SQL IaaS-Agent-Erweiterung über das Portal.

## <a name="status"></a>Status

Eine Möglichkeit um sicherzustellen, dass die Erweiterung installiert ist Agentstatus in Azure-Portal anzeigen. **Alle** Einstellungen in der virtuellen Blade- und klicken Sie dann auf **Erweiterung**. Die aufgeführten **SQLIaaSAgent** Erweiterung sollte angezeigt werden.

![SQL Server Agent IaaS Erweiterung in Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Sie können auch das Azure Powershell-Cmdlet **Get-AzureVMSqlServerExtension** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Entfernen   

Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie auf die Auslassungszeichen auf die **Erweiterung** der Eigenschaften. Klicken Sie auf **Löschen**.

![Deinstallieren Sie SQL Server Agent IaaS-Erweiterung in Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Sie können auch das Powershell-Cmdlet **AzureVMSqlServerExtension entfernen** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit einer von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie im Abschnitt [Dienste unterstützt](#supported-services) dieses Artikels verwiesen wird.

Weitere Informationen zum Ausführen von SQL Server auf Azure-Computer finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
