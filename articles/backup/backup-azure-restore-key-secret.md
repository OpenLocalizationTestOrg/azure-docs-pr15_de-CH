<properties
    pageTitle="Key Vault-Schlüssel und geheime für verschlüsselte VMs mit Azure Backup wiederherstellen | Microsoft Azure"
    description="Informationen Sie zum Schlüssel Depot Schlüssel und geheime in Azure Backup mit PowerShell wiederherstellen"
    services="backup"
    documentationCenter=""
    authors="JPallavi"
    manager="vijayts"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="JPallavi" />

# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Wiederherstellen Sie Key Vault-Schlüssel und geheime für verschlüsselte VMs mit Azure-Sicherung
Dieser Artikel spricht zu Azure VM Backup für die Wiederherstellung verschlüsselter Azure VMs, wenn Ihre Schlüssel und Schlüssel im schlüsseltresor nicht vorhanden sind. Diese Schritte können auch verwendet werden, möchten Sie eine Kopie der Schlüssel (Key Encryption) und Schlüssel (BitLocker-Verschlüsselungsschlüssel) für den wiederhergestellten virtuellen Computer verwalten.

## <a name="pre-requisites"></a>Erforderliche Komponenten

1. **Sicherung VMs** - Azure VMs verschlüsselt gesichert wurden mit Azure Backup. Ausführliche Informationen zum Sichern verschlüsselter Azure VMs [Management von Backup und Wiederherstellung von Azure VMs mit PowerShell](backup-azure-vms-automation.md) Artikel finden.

2. **Konfigurieren von Azure Key Vault** sicher, die Vault Schlüssel, Schlüssel und geheime Schlüssel wiederhergestellt werden müssen, ist bereits vorhanden. Siehe Artikel [Erste Schritte mit Azure Schlüssel](../key-vault/key-vault-get-started.md) Key Vault-Management.

## <a name="setup-recovery-services-vault"></a>Recovery Services Depot einrichten 
Gehen Sie PowerShell und Recovery Services Depot Kontext festlegen

### <a name="log-in-to-azure-powershell"></a>Melden Sie sich bei Azure PowerShell 

Mit dem folgenden Cmdlet Azure-Konto anmelden

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Recovery Services Depot Kontext festlegen

Nach der Anmeldung verwenden Sie das folgende Cmdlet, um die Liste der Abonnements verfügbar

```
PS C:\> Get-AzureRmSubscription
```

Wählen Sie den Dauerauftrag aus, in dem Ressourcen verfügbar sind

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Mit Recovery Services Vault, Sicherung wurde für verschlüsselte VMs aktiviert Vault-Kontext festlegen

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Recovery-Punkt zu erhalten 

Wählen Sie Container im Tresor, der verschlüsselte Azure Virtual Machine darstellt

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Mithilfe dieses Containers Artikel für den entsprechenden virtuellen Computer abrufen

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Ein Array von Wiederherstellungspunkten für die Sicherung ausgewählte Element in Variable Rp abrufen

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Verschlüsselte virtuelle Computer wiederherstellen
Gehen Sie verschlüsselte VM der Schlüssel und Schlüssel wiederherstellen.

### <a name="restore-key"></a>Schlüssel wiederherstellen

Das Array $rp oben ist mit der letzte Wiederherstellungspunkt am Index 0 in umgekehrter Reihenfolge sortiert. Beispiel: $rp [0] den letzten Wiederherstellungspunkt ausgewählt.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [AZURE.NOTE]
Nachdem dieses Cmdlet erfolgreich ausgeführt wird, ruft BLOB-Datei im angegebenen Ordner auf dem Computer generiert, wo es ausgeführt wird. BLOB-Datei stellt verschlüsselten Schlüssel verschlüsselt.

Wiederherstellen Sie zurück zum Schlüssel Tresor mit dem folgenden Cmdlet. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Schlüssel wiederherstellen

Wiederherstellen Sie geheime Daten von erhaltenen Wiederherstellungspunkt

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [AZURE.NOTE]
Der Text vor dem vault.azure.net Originalnamen Schlüssel Depot darstellt. Der Text nach Geheimnisse / geheimen Namen darstellt. 

Ermitteln Sie geheime Name und Wert aus der Ausgabe des Cmdlets, führen Sie denselben geheimen Namen verwenden möchten. In anderen Fällen sollte $secretname unter den neuen geheimen Namen aktualisiert werden. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Festlegen von Tags für den Schlüssel bei VM sowie wiederhergestellt werden muss. Für das DiskEncryptionKeyFileName-Tag sollte Wert Name des geheimen Schlüssels, den Sie verwenden möchten. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [AZURE.NOTE]
Wert für DiskEncryptionKeyFileName ist geheimen abgerufene Name identisch. Wert für DiskEncryptionKeyEncryptionKeyURL Schlüssel Depot erteilt nach Wiederherstellen der Schlüssel zurück und Cmdlet " [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) "   

Set geheimen Back Key Depot

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Virtuelle Computer wiederherstellen
Die obigen PowerShell-Cmdlets können Sie Schlüssel und geheime zurück zum Schlüssel Tresor wiederherstellen, wenn verschlüsselte VM mit Azure VM Backup gesichert wurden. Finden Sie nach der Wiederherstellung Artikel zum Wiederherstellen verschlüsselter VMs [Management von Backup und Wiederherstellung von Azure VMs mit PowerShell](backup-azure-vms-automation.md) .
