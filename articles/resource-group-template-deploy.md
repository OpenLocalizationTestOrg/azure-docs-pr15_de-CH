<properties
   pageTitle="Bereitstellen von Ressourcen mit PowerShell und Vorlage | Microsoft Azure"
   description="Verwenden Sie Azure Resource Manager und Azure PowerShell Azure eine Ressourcen bereitstellen. Die Ressourcen werden in einer Ressourcen-Manager-Vorlage definiert."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Bereitstellen von Ressourcen mit Ressourcen-Manager und Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

Dieses Thema erläutert die Azure PowerShell Ressourcenmanager Vorlagen verwenden, um Ressourcen in Azure bereitstellen.  

> [AZURE.TIP] Hilfe zum Debuggen eines Fehlers während der Bereitstellung finden Sie unter:
>
> - [Bereitstellung Ansichtsoperationen Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md) Informationen zum Abrufen von Informationen beheben den Fehler
> - [Problembehandlung bei Fehlermeldungen bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md) Informationen zu allgemeinen Bereitstellungsfehler

Die Vorlage kann eine lokale Datei oder eine externe Datei, die durch einen URI. Wenn die Vorlage in ein Speicherkonto befindet, können Sie Zugriff auf die Vorlage und Bereitstellung einer gemeinsamen Zugriff Signaturtoken (SAS) bieten.

## <a name="quick-steps-to-deployment"></a>Schritte zur Bereitstellung

Dieser Artikel beschreibt alle anderen Optionen, die während der Bereitstellung zur Verfügung. Jedoch beliebig oft zwei einfache Befehlen. Um schnell mit der Bereitstellung beginnen, verwenden Sie die folgenden Befehle:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Weiterlesen Sie weitere Informationen zu Optionen für die Bereitstellung, die besser für Ihr Szenario könnte diesen Artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>PowerShell bereitstellen

1. Auf der Azure-Konto anmelden.

        Add-AzureRmAccount

     Eine Zusammenfassung Ihres Kontos wird zurückgegeben.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Haben Sie mehrere Abonnements bieten Sie die Abonnement-ID für die Bereitstellung mit dem Befehl **Set AzureRmContext** verwenden möchten. 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. In der Regel soll bei der Bereitstellung einer neuen Vorlage erstellen eine Ressourcengruppe Ressourcen enthalten. Haben Sie eine Ressourcengruppe, der Sie zum bereitstellen möchten, können Sie diesen Schritt überspringen und Gruppe verwenden. 

     Um eine Ressourcengruppe zu erstellen, geben Sie einen Namen und einen Speicherort für die Ressourcengruppe. Sie müssen einen Speicherort für die Ressourcengruppe bereitstellen, da die Ressourcengruppe Metadaten Ressourcen gespeichert. Aus Gründen der Kompatibilität sollten Sie angeben, wo die Metadaten gespeichert ist. Es wird empfohlen, dass Sie einen Speicherort angeben, in dem die Ressourcen befinden. Mit demselben Speicherort kann Ihre Vorlage vereinfachen.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Eine neue Ressourcengruppe wird zurückgegeben.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Bevor Sie die Bereitstellung ausführen, können Sie die Bereitstellungseigenschaften überprüfen. **Test-AzureRmResourceGroupDeployment** -Cmdlet können Sie Probleme ermitteln, bevor Sie tatsächliche Ressourcen erstellen. Im folgenden Beispiel wird veranschaulicht, wie eine Bereitstellung überprüft.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Um Ressourcen in der Ressourcengruppe bereitstellen, führen Sie den Befehl **Neu AzureRmResourceGroupDeployment** und geben Sie die erforderlichen Parameter. Die Parameter umfassen einen Namen für die Bereitstellung der Name der Ressourcengruppe, der Pfad oder URL der Vorlage erstellten und andere Parameter für Ihr Szenario erforderlich. Wenn der **Mode** -Parameter nicht angegeben ist, wird der Standardwert **inkrementell** verwendet. Führen Sie eine vollständige Bereitstellung **Modus** auf **abgeschlossen**festgelegt. Vorsicht beim vollständigen Modus als versehentlich Ressourcen löschen, die nicht in der Vorlage sind.

     Verwenden Sie zum Bereitstellen einer lokalen Vorlage **TemplateFile** -Parameter:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Verwenden Sie zum Bereitstellen einer externen Vorlage **TemplateUri** Parameter:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Sie haben die folgenden Optionen zum Bereitstellen der Parameterwerte: 
   
     1. Verwenden Sie Inline-Parameter.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Verwenden Sie ein Parameterobjekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Verwenden einer lokalen Datei. Informationen über die Vorlagendatei finden Sie [Parameterdatei](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Verwenden einer externen Parameterdatei. Informationen über die Vorlagendatei finden Sie [Parameterdatei](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Bei Verwendung eine externen Parameterdatei kann nicht übergeben werden andere Werte entweder Inline oder aus einer lokalen Datei. Weitere Informationen finden Sie unter [Parameter Vorrang](#parameter-precendence).

     Nach der Bereitstellung der Ressourcen sehen Sie eine Zusammenfassung der Bereitstellung.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Wenn die Vorlage einen Parameter mit demselben Namen wie einer der Parameter in der PowerShell-Befehl enthält, werden Sie aufgefordert, einen Wert für diesen Parameter angeben. Der Parameter in der Vorlage enthalten Postfix **FromTemplate**. Z. B. einen Parameter mit dem Namen **ResourceGroupName** in Ihre Vorlage Konflikte mit dem Parameter **ResourceGroupName** in [Neu-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) -Cmdlet. Sie werden aufgefordert, einen Wert für **ResourceGroupNameFromTemplate**angeben. Im Allgemeinen sollten Sie diese Verwirrung durch Parameter nicht mit demselben Namen als Parameter für Bereitstellungsvorgänge benennen.

6. Möchten Sie weitere Informationen zur Bereitstellung zu protokollieren, die Ihnen helfen können, Bereitstellungsfehler zu beheben, verwenden Sie den Parameter **DeploymentDebugLogLevel** . Sie können angeben, dass Inhalte angefordert oder Antwortinhalt mit den Bereitstellungsvorgang protokolliert werden.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Weitere Informationen zur Verwendung dieser debugging Inhalt Bereitstellung finden Sie unter [Troubleshooting Resource Gruppe Deployments Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Vorlage mit SAS-Token bereitstellen

Sie können ein Speicherkonto, und verknüpfen Sie sie während der Bereitstellung mit einem SAS-Token Vorlagen hinzufügen.

> [AZURE.IMPORTANT] Folgt, wird das Blob mit der Vorlage nur der Kontobesitzer für. Beim Erstellen von SAS-Token für das Blob ist Blob für alle Benutzer mit diesen URI. Wenn ein anderer Benutzer den URI abfängt, ist dieser Benutzer auf die Vorlage zugreifen. Mit einem SAS-Token ist eine gute Möglichkeit zum Beschränken des Zugriffs auf Ihre Vorlagen sollten Sie vertrauliche Daten wie Kennwörter nicht direkt in die Vorlage aufnehmen.

### <a name="add-private-template-to-storage-account"></a>Speicherkonto private Vorlage hinzufügen

Richten Sie ein Speicherkonto für Vorlagen folgendermaßen:

1. Erstellen Sie eine Ressourcengruppe.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Erstellen Sie ein Speicherkonto. Der speicherkontoname für Azure eindeutig sein, so bieten eigene Namen für das Konto.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Einrichten des Speicherkontos des aktuellen.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Erstellen Sie einen Container. Die Berechtigung wird auf **Off** festgelegt, was bedeutet, dass der Container nur für den Besitzer.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Container die Vorlage hinzufügen.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>SAS-Token während der Bereitstellung bereitstellen

Bereitstellen eine private Vorlage in ein Speicherkonto rufen Sie SAS-Token ab und in den URI für die Vorlage.

1. Ändert das aktuelle Speicherkonto soll aktuelle Speicherkonto mit Vorlagen.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Erstellen Sie ein SAS-Token mit Leseberechtigungen und eine Ablaufzeit zu beschränken. Rufen Sie die vollständige URI der Vorlage, einschließlich SAS-Token ab

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Bereitstellen Sie die Vorlage, indem den URI, der das SAS-Token enthält.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Beispielsweise ein SAS-Token mit verknüpften Vorlagen finden Sie unter [verknüpfte Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Parameter-Vorrang

Sie können Inline Parameter und lokalen Parameterdatei im gleichen Bereitstellungsvorgang. Beispielsweise können Sie einige Werte in der lokalen Datei angeben und andere Werte während der Bereitstellung hinzufügen. Wenn Sie Werte für Parameter in der lokalen Datei und Inline angeben, wird die Inline-Wert Vorrang.

Jedoch kann nicht Inline Parameter mit einer externen Parameterdatei verwendet werden. Wenn Sie eine Datei im **TemplateParameterUri** -Parameter angeben, werden alle Inline ignoriert. Sie müssen alle Parameterwerte in der externen Datei angeben. Wenn Ihre Vorlage einen vertraulichen Wert, den in der Parameterdatei enthalten darf enthält, Schlüssel Tresor Wert hinzufügen und Schlüssel Depot in der externen Datei verweisen oder dynamisch alle Parameter Werte Inline bereitstellen.

Informationen zur Referenz Schlüsseltresor sichere Werte übergeben finden Sie unter [sichere Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Nächste Schritte
- Ein Beispiel zum Bereitstellen von Ressourcen durch die Clientbibliothek .NET finden Sie unter [Ressourcen bereitstellen Proxybibliothek und eine Vorlage verwenden](virtual-machines/virtual-machines-windows-csharp-template.md).
- Zum Definieren von Parametern in Vorlage anzuzeigen Sie [Erstellung Vorlagen](resource-group-authoring-templates.md#parameters).
- Bereitstellen der Projektmappe in unterschiedlichen Umgebungen Siehe [Entwicklung und testumgebungen in Microsoft Azure](solution-dev-test-environments.md).

