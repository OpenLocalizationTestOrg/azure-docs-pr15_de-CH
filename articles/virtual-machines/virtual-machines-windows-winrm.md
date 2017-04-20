<properties
    pageTitle="Einrichten von WinRM Zugriff für virtuelle Computer in Azure-Ressourcen-Manager | Microsoft Azure"
    description="WinRM Zugriff mit einem Ressourcenmanager Azure virtuellen Computer einrichten"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Einrichten von WinRM Zugriff für virtuelle Computer in Azure-Ressourcen-Manager

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM in Azure Service Management Vs Azure-Ressourcen-Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell

* Eine Übersicht der Azure-Ressourcen-Manager finden Sie in diesem [Artikel](../azure-resource-manager/resource-group-overview.md)
* Unterschiede zwischen Azure Service Management und Azure Resource Manager finden Sie in diesem [Artikel](../resource-manager-deployment-model.md)

Der Hauptunterschied im WinRM Konfiguration zwischen zwei Stapel ist wie das Zertifikat auf dem virtuellen Computer installiert wird. Zertifikate werden im Stapel Azure-Ressourcen-Manager als Schlüssel Vault-Ressourcenproviders verwaltete Ressourcen erstellt. Daher muss der Benutzer ihr eigenes Zertifikat und Schlüssel Tresor vor der Verwendung in einer VM hochladen.

Hier werden die Schritte zu einer VM mit WinRM einrichten

1. Ein Schlüssel Depot erstellen
2. Ein selbstsigniertes Zertifikat erstellen
3. Das selbstsignierte Zertifikat auf Schlüssel Depot hochladen
4. Ruft die URL für das selbstsignierte Zertifikat in Tresor Schlüssel
5. Verweisen Sie Ihre URL selbstsignierte Zertifikate beim Erstellen einer VM

## <a name="step-1-create-a-key-vault"></a>Schritt 1: Erstellen Sie ein Schlüssel Depot

Sie können den folgenden Befehl Schlüssel Depot erstellen

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Schritt 2: Erstellen eines selbstsignierten Zertifikats
Sie können ein selbstsigniertes Zertifikat mithilfe von PowerShell-Skript erstellen.

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Schritt 3: Hochladen Sie das selbstsignierte Zertifikat auf Schlüssel Depot

Vor dem Hochladen des Zertifikats in Schritt 1 erstellten Schlüssel Depot muss in einem Format verstehen Ressourcenanbieter Microsoft.Compute konvertiert. Die folgenden PowerShell Skript können Sie dies

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Schritt 4: Rufen Sie die URL für das selbstsignierte Zertifikat in Tresor Schlüssel

Ressourcenanbieter Microsoft.Compute benötigt eine URL, um den Schlüssel im Schlüsseltresor während die VM-Bereitstellung. Dadurch können Microsoft.Compute Ressourcenanbieter zu den geheimen Schlüssel das entsprechende Zertifikat auf dem virtuellen Computer erstellen.

>[AZURE.NOTE]Die URL des geheimen Schlüssels muss die Version enthalten. Eine Beispiel-URL sieht unter Https://contosovault.vault.azure.net:443/Geheimnisse/Contososecret/01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Vorlagen

Erhalten Sie den Link auf die URL in der Vorlage mit dem Code unten

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell

Erhalten Sie diese URL mit dem folgenden PowerShell-Befehl

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Schritt 5: Verweisen Sie den URL selbst-signierte Zertifikate beim Erstellen einer VM

#### <a name="azure-resource-manager-templates"></a>Azure Ressourcenmanager Vorlagen

Beim Erstellen einer VM Vorlagen, wird das Zertifikat in die Geheimnisse und WinRM nachstehend verwiesen:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Eine Beispielvorlage der [201-Vm-Winrm-schlüsseltresor -](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows) Fenster finden Sie hier

Quellcode für diese Vorlage finden Sie auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Schritt 6: Verbindung zur VM
Bevor Sie die VM verbinden können, müssen Sie sicherstellen, ist der Computer für die Remoteverwaltung von WinRM konfiguriert. PowerShell als Administrator starten, und führen Sie den folgenden Befehl, um sicherzustellen, Sie sind eingerichtet.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Sie müssen sicherstellen, dass der WinRM-Dienst ausgeführt wird, wenn dieser nicht funktioniert. Verwenden kann`Get-Service WinRM`

Nach Abschluss die Installation können Sie die VM mit dem folgenden Befehl

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate