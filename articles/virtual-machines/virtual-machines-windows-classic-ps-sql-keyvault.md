<properties
    pageTitle="Konfigurieren von Azure-Tresor Integration für SQL Server auf Azure VMs (klassisch)"
    description="Informationen Sie zum Automatisieren der Konfiguration von SQL Server-Verschlüsselung für die Verwendung mit Azure Schlüssel. Dieses Thema erläutert Azure Key Vault Integration mit SQL Server erstellte virtuelle Maschinen im klassischen Bereitstellungsmodell."
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
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>Konfigurieren von Azure-Tresor Integration für SQL Server auf Azure VMs (klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-ps-sql-keyvault.md)
- [Classic](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Übersicht
Es gibt mehrere SQL Server-Funktionen, wie [transparente Verschlüsselung (DSA)](https://msdn.microsoft.com/library/bb934049.aspx), [Spalte Verschlüsselung (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)und [backup-Verschlüsselung](https://msdn.microsoft.com/library/dn449489.aspx). Diese Arten der Verschlüsselung erfordern verwalten und speichern die kryptografischen Schlüssel für die Verschlüsselung verwenden. Der Dienst Azure Key Vault (AKV) Dient zur Verbesserung der Sicherheit und Verwaltung dieser Schlüssel in einem sicheren und hochverfügbaren. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) ermöglicht SQL Server diese Schlüssel von Azure Key Vault verwenden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Wenn Sie SQL Server auf dem lokalen Computer ausführen, sind [Schritte können zum Azure Schlüsseltresor vom lokalen SQL Server-Computer zugreifen](https://msdn.microsoft.com/library/dn198405.aspx). Für SQL Server in Azure VMs, Sie können aber Zeit sparen mit der *Integration von Azure Key Vault* . Mit wenigen Azure PowerShell-Cmdlets zum Aktivieren dieses Features können Sie die Konfiguration für einen SQL-VM auf Ihrem Schlüssel Tresor automatisieren.

Wenn dieses Feature aktiviert ist, automatisch der SQL Server-Connector installiert, konfiguriert den EKM Anbieter Azure Schlüsseltresor zugreifen und erstellt die Anmeldeinformationen Sie Ihrem Tresor zugreifen können. Sah die Schritte in der genannten vor-Ort-Dokumentation sehen Sie, dass dieses Feature automatisiert die Schritte 2 und 3. Das müssen Sie weiterhin manuell lediglich Schlüssel Depot und Schlüssel zu erstellen. Dort wird das gesamte Setup Ihrer SQL-VM automatisiert. Wenn dieses Feature Setup abgeschlossen ist, können Sie T-SQL-Anweisungen zunächst verschlüsselt Ihre Datenbanken oder Backups wie gewohnt ausführen.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV Integration konfigurieren
Mithilfe von PowerShell Azure Key Vault-Integration konfigurieren. Folgende Abschnitte enthalten eine Übersicht über die erforderlichen Parameter und PowerShell-Beispielskript.

### <a name="install-the-sql-server-iaas-extension"></a>Installieren Sie SQL Server IaaS-Erweiterung

Zuerst [Installieren Sie SQL Server IaaS-Erweiterung](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Verstehen der Eingabeparameter
Die folgende Tabelle listet die Parameter auf die PowerShell-Skript im nächsten Abschnitt.

|Parameter|Beschreibung|Beispiel|
|---|---|---|
|**$akvURL**|**Key Vault-URL**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Dienstprinzipalnamen**|"fde2b411 - 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Dienstprinzipalnamen Geheimnis**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm-azF1XDKM ="|
|**$credName**|**Anmeldenamen**: AKV Integration Anmeldeinformationen in SQL Server ermöglicht die VM auf die wichtigsten Tresor erstellt. Wählen Sie einen Namen für diese Anmeldeinformationen.|"mycred1"|
|**$vmName**|**Name des virtuellen Computers**: den Namen der zuvor erstellten SQL-VM.|"Myvmname"|
|**$serviceName**|**Dienstname**: SQL VM zugeordnet ist die Cloud-Dienst-Name.|"Mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Integration mit PowerShell AKV aktivieren
**Neu-AzureVMSqlServerKeyVaultCredentialConfig** -Cmdlet erstellt ein Konfigurationsobjekt für die Integration von Azure Key Vault. **Set AzureVMSqlServerExtension** konfiguriert diese Integration mit dem **KeyVaultCredentialSettings** -Parameter. Die folgenden Schritte zeigen, wie diese Befehle.

1. In Azure PowerShell zuerst konfigurieren Sie die Eingabeparameter mit bestimmten Werten wie in den vorherigen Abschnitten dieses Themas beschrieben. Das folgende Skript ist ein Beispiel.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Können Sie das folgende Skript konfigurieren und aktivieren AKV Integration.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

SQL IaaS-Agent-Erweiterung aktualisiert SQL VM mit dieser neuen Konfiguration.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
