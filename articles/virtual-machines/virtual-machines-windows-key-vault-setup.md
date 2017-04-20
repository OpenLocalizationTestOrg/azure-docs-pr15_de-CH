<properties
    pageTitle="Richten Sie Schlüsseltresor für virtuelle Computer in Azure-Ressourcen-Manager | Microsoft Azure"
    description="Wie Schlüsseltresor mit einem Ressourcenmanager Azure virtuellen Computer eingerichtet."
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
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Richten Sie Schlüsseltresor für virtuelle Computer in Azure-Ressourcen-Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell

In Azure Ressourcenmanager Stapel werden Geheimnisse-Zertifikate als Ressourcen erstellt, die von dem Ressourcenanbieter Schlüssel Depot bereitgestellt werden. Über Schlüsseltresor finden Sie unter [Neuigkeiten Azure Key Vault?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Key Vault Azure Resource Manager virtuelle Computer verwendet werden, die **EnabledForDeployment** Eigenschaft auf Schlüssel muss werden festgelegt auf True. Dies ist bei verschiedenen Clients möglich.
>
>2. Schlüsseltresor soll in dem Abonnement und Speicherort wie die virtuelle Maschine erstellt werden.

## <a name="use-powershell-to-set-up-key-vault"></a>Mithilfe von PowerShell Schlüssel Depot einrichten
Ein Schlüssel Depot mit PowerShell finden Sie [Erste Schritte mit Azure Schlüssel](../key-vault/key-vault-get-started.md#vault).

Für neue Schlüssel Depots können Sie PowerShell-Cmdlet:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Für vorhandene Schlüssel Depots können Sie PowerShell-Cmdlet:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Uns CLI Schlüssel Depot einrichten
Zum Erstellen eines Schlüssels Depots Befehlszeilenschnittstelle (CLI) Siehe [Key Vault verwalten CLI verwenden](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

CLI müssen Sie wichtige Depot erstellen, bevor Sie die Bereitstellungsrichtlinie zuweisen. Dazu können Sie den folgenden Befehl verwenden:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Schlüssel Depot einrichten
Während Sie eine Vorlage verwenden, müssen Sie die `enabledForDeployment` -Eigenschaft `true` Key Vault-Ressource.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Andere Optionen, die beim Erstellen eines Schlüssels Depots Vorlagen konfigurieren können, finden Sie unter [erstellen Schlüssel Depot](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
