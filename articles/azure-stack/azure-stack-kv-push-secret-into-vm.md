<properties
    pageTitle="Einen virtueller Computer mit einem Zertifikat mit Azure Stack Schlüssel Depot bereitstellen | Microsoft Azure"
    description="Erfahren Sie, wie einen virtueller Computer bereitstellen und fügen Sie ein Zertifikat von Azure Stapel Key Vault"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Virtuelle Computer erstellen und Zertifikate von Key Vault abgerufen einbeziehen

Azure Stapel VMs durch Azure-Ressourcen-Manager bereitgestellt werden, und Sie können Zertifikate in Azure Stack Schlüsseltresor speichern. Dann legt Azure Stapel (Ressourcenanbieter zu bestimmten Microsoft.Compute) sie die VMs beim virtuellen Computer bereitgestellt werden. Zertifikate können in vielen Szenarios, einschließlich SSL, Verschlüsselung und Authentifizierung Zertifikat verwendet.

Mithilfe dieser Methode können Sie das Zertifikat schützen. Es ist jetzt nicht in der VM-Image und Konfigurationsdateien der Anwendung oder an einem unsicheren Speicherort. Entsprechende Richtlinien für die wichtigsten Tresor festlegen, können Sie auch steuern, wer Zugriff auf das Zertifikat. Verwalten alle Zertifikate in einer Azure Stapel Key Vault ist.

Hier wird ein Überblick über den Prozess:

-   Sie benötigen ein Zertifikat im PFX-Format.

-   Erstellen eines schlüsseltresors (mit einer Vorlage oder das folgende Beispielskript).

-   Sicherstellen Sie, dass die EnabledForDeployment-Option aktiviert ist.

-   Laden Sie das Zertifikat geheim.

## <a name="deploying-vms"></a>Bereitstellung von VMs

Dieses Beispielskript erstellt ein Schlüssel Depot und speichert ein Zertifikat in die PFX-Datei in einem lokalen Verzeichnis Schlüssel Depot geheim.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Der erste Teil des Skripts liest die PFX-Datei und speichert als eine JSON Objekt mit der Datei Content base64-codiert. Das JSON-Objekt auch base64-codiert ist.

Anschließend erstellt eine neue Ressourcengruppe und erstellt dann ein Schlüssel Depot. Beachten Sie die letzten Parameter für den Befehl neu AzureKeyVault '-EnabledForDeployment', die Zugriff gewährt in Azure (insbesondere Ressourcenanbieter Microsoft.Compute) lesen Geheimnisse Schlüssel Depot für die Bereitstellung.

Der letzte Befehl speichert lediglich base64-codierte JSON-Objekt im Schlüssel Tresor geheim.

Hier wird eine Beispielausgabe aus dem vorherigen Skript:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Nun können wir eine VM-Vorlage bereitstellen. Beachten Sie den URI der Schlüssel der Ausgabe (wie in der obigen Ausgabe Grün).

Sie benötigen eine Vorlage hier. Die Parameter von besonderem Interesse (außer den üblichen VM-Parametern) sind Depotnamen ein Depot Ressourcengruppe und geheimen URI. Sie können natürlich auch GitHub herunterladen und ändern Sie nach Bedarf.

Bei diesem virtuellen Computer bereitgestellt wird, fügt Azure das Zertifikat in der VM.
Unter Windows werden Zertifikate in die PFX-Datei mit dem privaten Schlüssel nicht exportierbar hinzugefügt. Das Zertifikat wird an Zertifikat LocalMachine mit dem Zertifikatspeicher hinzugefügt, die vom Benutzer bereitgestellten. Unter Linux befindet sich die Zertifikatsdatei im Verzeichnis /var/lib/waagent unter dem Namen &lt;UppercaseThumbprint&gt;für die X509 CRT Zertifikatsdatei, und &lt;UppercaseThumbprint&gt;.prv für den privaten Schlüssel.
Beide Dateien sind .pem formatiert.

Die Anwendung normalerweise mithilfe des Fingerabdrucks des Zertifikats findet und nicht ändern.

## <a name="retiring-certificates"></a>Außerbetriebnehmen der Zertifikate


Im vorherigen Abschnitt wurde die Vorgehensweise zu Ihrer vorhandenen virtuellen Computer müssen ein neues Zertifikat. Aber das alte Zertifikat noch die VM und kann nicht entfernt werden. Für zusätzliche Sicherheit können Sie das Attribut für den alten Schlüssel 'Deaktiviert' ändern, auch wenn eine alte Vorlage einen virtueller Computer mit dieser älteren Version des Zertifikats erstellen, wird. Hier ist festlegen eine bestimmte geheime Version deaktiviert werden:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Abschluss


Mit dieser neuen Methode kann das Zertifikat von VM-Image oder der Anwendungsnutzdaten getrennt werden. So haben wir einen Punkt der Exposition entfernt.

Das Zertifikat kann auch erneuert und ohne VM-Image oder das Bereitstellungspaket Anwendung neu erstellen Schlüssel Tresor hochgeladen. Die Anwendung muss jedoch mit dem neuen URI für diese neue Zertifikatsversion angegeben werden.

Das Zertifikat von der VM oder der Anwendungsnutzdaten trennen, haben wir jetzt die Anzahl der Mitarbeiter gesenkt, die direkten Zugriff auf das Zertifikat.

Als zusätzlichen Vorteil haben Sie einem Ort im Schlüsseltresor verwalten alle Zertifikate, einschließlich der alle Versionen, die mit der Zeit bereitgestellt wurden.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen einer VM mit einem Schlüssel Vault-Kennwort](azure-stack-kv-deploy-vm-with-secret.md)

[Eine Anwendung auf Schlüssel Depot](azure-stack-kv-sample-app.md)