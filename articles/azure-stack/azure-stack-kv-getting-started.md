<properties
    pageTitle="Erste Schritte mit Azure Stack Schlüssel | Microsoft Azure"
    description="Erste Schritte mit Azure Stapel Key Vault"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Erste Schritte mit Schlüssel

Dieser Abschnitt beschreibt die Schritte zum Erstellen eines Depots Schlüssel und Kennwörter verwalten sowie autorisieren Benutzer oder Vorgänge im Tresor Azure Stack aufrufen. Die folgenden Schritte gehen Mieter Abonnement vorhanden und Schlüsseltresor Service innerhalb dieses Abonnement registriert ist. Bei allen Beispielbefehlen basieren auf den Schlüsseltresor-Cmdlets verfügbar als Teil des Azure PowerShell SDK.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Aktivieren das Abonnement Mieter für Vault-Operationen 

Bevor Sie gegen alle Vault ausgeben können, müssen Sie sicherstellen, dass Ihr Abonnement für Vault-Operationen aktiviert ist. Sie können bestätigen, die dem folgenden PowerShell-Befehl:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Die Ausgabe des obigen Befehls melden "Registriert" für den Status "Registrierung" jeder Zeile.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Wenn dies nicht der Fall ist, sollten Sie den folgenden Befehl zum Schlüsseltresor-Dienst in Ihrem Abonnement registrieren aufrufen:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

Und die folgende Ausgabe des Befehls:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Wenn Sie die Fehlermeldung: "*das Abonnement nicht mit Azure Schlüssel registriert*" beim Schlüsseltresor Cmdlets bestätigen Ressourcenanbieter Schlüsseltresor pro obigen Instruktionen aktiviert haben.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Erstellen eines verstärkten Containers (Vault) in Azure Stapel zum Speichern und verwalten kryptografische Schlüssel und Schlüssel

Um das Erstellen eines Depots sollten Mieter zuerst eine Ressourcengruppe erstellen. Die folgenden PowerShell Befehle Erstellen einer Ressourcengruppe und ein Depot in Gruppe. Das Beispiel enthält außerdem die normale Ausgabe dieses Cmdlet.

### <a name="creating-a-resource-group"></a>Erstellen einer Ressourcengruppe:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Ausgabe:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Erstellen ein Depot:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Ausgabe:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Die Ausgabe dieses Cmdlet zeigt Eigenschaften der soeben erstellten schlüsseltresors. Die zwei wichtigsten Eigenschaften sind:

-   **Depotnamen**: In dem Beispiel ist **vault010**. Sie werden diesen Namen für andere Schlüssel Vault-Cmdlets verwenden.

-   **Vault-URI**: In dem Beispiel ist https://vault010.vault.azurestack.local. Ihrem Tresor über die REST-API verwenden, müssen dieser URI verwenden.

Ein Azure-Konto darf jetzt alle Operationen auf diesen Schlüssel. Ist niemand.


## <a name="operating-on-keys-and-secrets"></a>Auf den Schlüssel und geheime Schlüssel

Nach Abschluss der Erstellung eines Depots, die Schlüssel und geheime Informationen so zu verwalten:

### <a name="creating-a-key"></a>Erstellen eines Schlüssels

Um einen Schlüssel zu erstellen, verwenden Sie **Hinzufügen AzureKeyVaultKey** pro Beispiel unten. Nach dem erfolgreichen Erstellen gibt das Cmdlet die neu erstellten Schlüssel Details.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Die Ausgabe des Cmdlets *Hinzufügen AzureKeyVaultKey* lautet:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Sie können nun diesen Schlüssel verweisen, die erstellt oder mithilfe des URI Azure Schlüssel Tresor hochgeladen. **Schlüssel/Https://vault010.vault.azurestack.local:443/keyVaultKeyName001** der aktuellen Version zu verwenden. und **Https://vault010.vault.azurestack.local:443/Schlüssel/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** zu dieser speziellen Version.

### <a name="retrieving-a-key"></a>Einen Schlüssel abrufen

Verwenden Sie **Get-AzureKeyVaultKey** , um einen Schlüssel und seine Details pro im folgenden Beispiel abzurufen:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

So sieht die Ausgabe von Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Festlegen eines Kennwortes

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Ausgabe

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Abrufen eines geheimen Schlüssels

    Get-AzureKeyVaultSecret -VaultName vault010

Ausgabe

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Jetzt kann der Schlüssel Depot und Schlüssel oder geheimen Applikationen verwendet.
Autorisieren Sie Applikationen verwenden.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorisieren Sie die Anwendung des Schlüssels oder geheimen 

Verwenden Sie zur Autorisierung der Anwendung Zugriff auf die Schlüssel oder Schlüssel im Tresor das Cmdlet "Set -**AzureRmKeyVaultAccessPolicy** ".

Wenn Vault *ContosoKeyVault heißt* Anwendung zu autorisieren eine *Client-ID* des *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed hat*, und Sie die Anwendung zum Entschlüsseln und Signieren mit Schlüsseln in Ihrem Tresor autorisieren möchten führen Sie z. B. die folgenden:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Wenn Sie dieselbe Anwendung vertrauliche Informationen in Ihrem Tresor lesen autorisieren möchten, führen Sie folgenden Befehl:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen einer VM mit einem Schlüssel Vault-Kennwort](azure-stack-kv-deploy-vm-with-secret.md)

[Bereitstellen einer VM mit einem Schlüssel Depot](azure-stack-kv-push-secret-into-vm.md)