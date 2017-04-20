<properties
    pageTitle="Richten Sie Schlüsseltresor für virtuelle Computer in Azure-Ressourcen-Manager | Microsoft Azure"
    description="Wie Schlüsseltresor mit einem Ressourcenmanager Azure virtuellen Computer eingerichtet."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Richten Sie Schlüsseltresor für virtuelle Computer in Azure-Ressourcen-Manager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell

In Azure Ressourcenmanager Stapel werden Geheimnisse-Zertifikate als Ressourcen erstellt, die von dem Ressourcenanbieter Schlüssel Depot bereitgestellt werden. Erfahren Sie mehr über Azure Key Vault finden Sie unter [Neuigkeiten Azure Key Vault?](../key-vault/key-vault-whatis.md)

Key Vault Azure Resource Manager virtuelle Computer verwendet werden, die *EnabledForDeployment* Eigenschaft auf Schlüssel muss werden festgelegt auf True. Sie können verschiedene Clients dazu."

## <a name="use-cli-to-set-up-key-vault"></a>Verwenden von CLI Schlüssel Depot einrichten
Zum Erstellen eines Schlüssels Depots Befehlszeilenschnittstelle (CLI) Siehe [Key Vault verwalten CLI verwenden](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

CLI müssen Sie wichtige Depot erstellen, bevor Sie die Bereitstellungsrichtlinie zuweisen. Dazu können Sie den folgenden Befehl verwenden:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Schlüssel Depot einrichten
Wenn eine Vorlage verwenden, müssen Sie festlegen der `enabledForDeployment` -Eigenschaft auf `true` Key Vault-Ressource.

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
