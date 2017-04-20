<properties
   pageTitle="Bereitstellen von Ressourcen mit Azure CLI und Vorlage | Microsoft Azure"
   description="Verwenden Sie Azure Resource Manager und Azure CLI eine Ressourcen in Azure bereitstellen. Die Ressourcen werden in einer Ressourcen-Manager-Vorlage definiert."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Ressourcen-Manager und Azure-CLI

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

Dieses Thema erläutert die Azure-CLI mit Ressourcen-Manager-Vorlagen verwenden, um Ressourcen in Azure bereitstellen.  

> [AZURE.TIP] Hilfe zum Debuggen eines Fehlers während der Bereitstellung finden Sie unter:
>
> - [Bereitstellung Ansichtsoperationen mit Azure CLI](resource-manager-troubleshoot-deployments-cli.md) Informationen zum Abrufen von Informationen beheben den Fehler
> - [Problembehandlung bei Fehlermeldungen bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md) Informationen zu allgemeinen Bereitstellungsfehler

Die Vorlage kann eine lokale Datei oder eine externe Datei, die durch einen URI. Wenn die Vorlage in ein Speicherkonto befindet, können Sie Zugriff auf die Vorlage und Bereitstellung einer gemeinsamen Zugriff Signaturtoken (SAS) bieten.

## <a name="quick-steps-to-deployment"></a>Schritte zur Bereitstellung

Dieser Artikel beschreibt alle anderen Optionen, die während der Bereitstellung zur Verfügung. Jedoch beliebig oft zwei einfache Befehlen. Um schnell mit der Bereitstellung beginnen, verwenden Sie die folgenden Befehle:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Weiterlesen Sie weitere Informationen zu Optionen für die Bereitstellung, die besser für Ihr Szenario könnte diesen Artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Bereitstellen von Azure CLI

Wenn Sie nicht zuvor Azure CLI mit Ressourcen-Manager verwendet haben, finden Sie unter [Verwendung der Azure-CLI für Mac, Linux und Windows Azure Resource Management](xplat-cli-azure-resource-manager.md).

1. Auf der Azure-Konto anmelden. Nach Anmeldeinformationen, gibt der Befehl aus Ihren Benutzernamen.

        azure login
  
        ...
        info:    login command OK

2. Wenn Sie mehrere Abonnements, bieten Sie die Abonnement-Id für die Bereitstellung verwenden möchten.

        azure account set <YourSubscriptionNameOrId>

3. Wechseln Sie zur Azure Resource Manager-Modul. Bestätigung des neuen Modus.

        azure config mode arm
   
        info:     New mode is arm

4. Wenn Sie eine vorhandene Ressourcengruppe nicht haben, erstellen Sie eine Ressourcengruppe. Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen. Sie müssen einen Speicherort für die Ressourcengruppe bereitstellen, da die Ressourcengruppe Metadaten Ressourcen gespeichert. Aus Gründen der Kompatibilität sollten Sie angeben, wo die Metadaten gespeichert ist. Es wird empfohlen, dass Sie einen Speicherort angeben, in dem die Ressourcen befinden. Mit demselben Speicherort kann Ihre Vorlage vereinfachen.

        azure group create -n ExampleResourceGroup -l "West US"

     Eine neue Ressourcengruppe wird zurückgegeben.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Überprüfen Sie die Bereitstellung vor der Ausführung mit dem Befehl **Azure Gruppenvorlage überprüfen** . Beim Testen der Bereitstellung Geben Sie Parameter, genau wie bei die Bereitstellung (siehe nächstes) ausführen.

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Um Ressourcen in der Ressourcengruppe bereitstellen, führen Sie folgenden Befehl, und geben Sie die erforderlichen Parameter. Die Parameter umfassen einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, der Pfad oder URL der Vorlage und andere Parameter für Ihr Szenario erforderlich. 
   
     Sie haben die folgenden drei Optionen für die Bereitstellung von Parameterwerte: 

     1. Verwenden Sie Inline-Parameter und lokale Vorlage. Jeder Parameter wird im Format: `"ParameterName": { "value": "ParameterValue" }`. Das folgende Beispiel zeigt die Parameter mit Escapezeichen.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Verwenden Sie Inline-Parameter und einen Link zu einer Vorlage.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Verwenden einer Parameterdatei. Informationen über die Vorlagendatei finden Sie [Parameterdatei](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Nachdem die Ressourcen über einen der drei oben genannten Methoden bereitgestellt haben, sehen Sie eine Zusammenfassung der Bereitstellung.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Führen Sie eine vollständige Bereitstellung **Modus** auf **abgeschlossen**festgelegt. Vorsicht bei diesem versehentlich Ressourcen löschen können, die nicht in der Vorlage sind.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Möchten Sie weitere Informationen zur Bereitstellung zu protokollieren, die Ihnen helfen können, Bereitstellungsfehler zu beheben, verwenden Sie den Parameter **Debug-Einstellung** . Sie können angeben, dass Inhalte angefordert oder Antwortinhalt mit den Bereitstellungsvorgang protokolliert werden.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Vorlage mit SAS-Token bereitstellen

Sie können ein Speicherkonto, und verknüpfen Sie sie während der Bereitstellung mit einem SAS-Token Vorlagen hinzufügen.

> [AZURE.IMPORTANT] Folgt, wird das Blob mit der Vorlage nur der Kontobesitzer für. Beim Erstellen von SAS-Token für das Blob ist Blob für alle Benutzer mit diesen URI. Wenn ein anderer Benutzer den URI abfängt, ist dieser Benutzer auf die Vorlage zugreifen. Mit einem SAS-Token ist eine gute Möglichkeit zum Beschränken des Zugriffs auf Ihre Vorlagen sollten Sie vertrauliche Daten wie Kennwörter nicht direkt in die Vorlage aufnehmen.

### <a name="add-private-template-to-storage-account"></a>Speicherkonto private Vorlage hinzufügen

Richten Sie ein Speicherkonto für Vorlagen folgendermaßen:

1. Erstellen Sie eine Ressourcengruppe.

        azure group create -n "ManageGroup" -l "westus"

2. Erstellen Sie ein Speicherkonto. Der speicherkontoname für Azure eindeutig sein, so bieten eigene Namen für das Konto.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Legen Sie Variablen für das Speicherkonto und Schlüssel.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Erstellen Sie einen Container. Die Berechtigung wird auf **Off** festgelegt, was bedeutet, dass der Container nur für den Besitzer.

        azure storage container create --container templates -p Off 
        
4. Container die Vorlage hinzufügen.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>SAS-Token während der Bereitstellung bereitstellen

Bereitstellen eine private Vorlage in ein Speicherkonto rufen Sie SAS-Token ab und in den URI für die Vorlage.

1. Erstellen Sie ein SAS-Token mit Leseberechtigungen und eine Ablaufzeit zu beschränken. Legen Sie die Ablaufzeit genügend Zeit zum Abschließen der Bereitstellung. Rufen Sie die vollständige URI der Vorlage, einschließlich SAS-Token ab

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Bereitstellen Sie die Vorlage, indem den URI, der das SAS-Token enthält.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Beispielsweise ein SAS-Token mit verknüpften Vorlagen finden Sie unter [verknüpfte Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Nächste Schritte
- Ein Beispiel zum Bereitstellen von Ressourcen durch die Clientbibliothek .NET finden Sie unter [Ressourcen bereitstellen Proxybibliothek und eine Vorlage verwenden](virtual-machines/virtual-machines-windows-csharp-template.md).
- Zum Definieren von Parametern in Vorlage anzuzeigen Sie [Erstellung Vorlagen](resource-group-authoring-templates.md#parameters).
- Bereitstellen der Projektmappe in unterschiedlichen Umgebungen Siehe [Entwicklung und testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
- Informationen zur Referenz Schlüsseltresor sichere Werte übergeben finden Sie unter [sichere Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

