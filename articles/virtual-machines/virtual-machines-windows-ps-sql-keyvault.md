<properties
    pageTitle="Konfigurieren der Azure-Tresor Integration für SQL Server Azure VMs (Ressourcen-Manager)"
    description="Informationen Sie zum Automatisieren der Konfiguration von SQL Server-Verschlüsselung für die Verwendung mit Azure Schlüssel. Dieses Thema erläutert die Azure Key Vault Integration mit SQL Server mit Ressourcen-Manager erstellte Maschinen verwenden."
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
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Konfigurieren der Azure-Tresor Integration für SQL Server Azure VMs (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-ps-sql-keyvault.md)
- [Classic](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Übersicht
Es gibt mehrere SQL Server-Funktionen, wie [transparente Verschlüsselung (DSA)](https://msdn.microsoft.com/library/bb934049.aspx), [Spalte Verschlüsselung (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)und [backup-Verschlüsselung](https://msdn.microsoft.com/library/dn449489.aspx). Diese Arten der Verschlüsselung erfordern verwalten und speichern die kryptografischen Schlüssel für die Verschlüsselung verwenden. Der Dienst Azure Key Vault (AKV) Dient zur Verbesserung der Sicherheit und Verwaltung dieser Schlüssel in einem sicheren und hochverfügbaren. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) ermöglicht SQL Server diese Schlüssel von Azure Key Vault verwenden.

Wenn Sie SQL Server mit lokalen es Maschinen [Sie](https://msdn.microsoft.com/library/dn198405.aspx)Schritte können zum Azure Schlüsseltresor vom lokalen SQL Server-Computer zugreifen. Für SQL Server in Azure VMs, Sie können aber Zeit sparen mit der *Integration von Azure Key Vault* .

Wenn dieses Feature aktiviert ist, automatisch der SQL Server-Connector installiert, konfiguriert den EKM Anbieter Azure Schlüsseltresor zugreifen und erstellt die Anmeldeinformationen Sie Ihrem Tresor zugreifen können. Sah die Schritte in der genannten vor-Ort-Dokumentation sehen Sie, dass dieses Feature automatisiert die Schritte 2 und 3. Das müssen Sie weiterhin manuell lediglich Schlüssel Depot und Schlüssel zu erstellen. Dort wird das gesamte Setup Ihrer SQL-VM automatisiert. Wenn dieses Feature Setup abgeschlossen ist, können Sie T-SQL-Anweisungen zunächst verschlüsselt Ihre Datenbanken oder Backups wie gewohnt ausführen.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Aktivieren und Konfigurieren von AKV-integration
Sie können ermöglichen AKV Integration während der Bereitstellung oder für vorhandene virtuelle Computer konfigurieren.

### <a name="new-vms"></a>Neuer VMs
Wenn Sie einen neuen SQL Server virtueller Computer mit Ressourcen-Manager bereitstellen, bietet Azure-Portal einen Schritt um Azure Key Vault-Integration aktivieren. Azure Key Vault-Funktion ist nur für Enterprise, Developer und Evaluation Editionen von SQL Server verfügbar.

![SQL Azure-Tresor Integration](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Eine detaillierte Anleitung für die Bereitstellung finden Sie unter [Bereitstellung eines virtuellen Computers SQL Server in der Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Vorhandene VMs
Vorhandenen virtuellen Computer SQL Server wählen Sie den virtuellen SQL Server-Computer. Dann wählen Sie **SQL Server-Konfiguration** des Blade- **Einstellungen** .

![SQL AKV Integration vorhandener VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Blatt **SQL Server Configuration** klicken Sie **Bearbeiten** im Abschnitt Schlüsseltresor automatisiert.

![SQL AKV Integration für vorhandene virtuelle Computer konfigurieren](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Klicken Sie abschließend auf die Schaltfläche **OK** auf der Unterseite des Blades **SQL Server-Konfiguration** zu ändern.

>[AZURE.NOTE] Sie können auch AKV Integration mithilfe einer Vorlage. Weitere Informationen finden Sie unter [Azure Schnellstart-Vorlage für die Integration von Azure Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
