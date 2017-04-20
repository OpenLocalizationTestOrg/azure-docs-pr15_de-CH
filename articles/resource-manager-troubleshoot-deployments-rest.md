<properties
   pageTitle="Bereitstellungsvorgänge REST API anzeigen | Microsoft Azure"
   description="Beschreibt das Azure Ressourcenmanager REST API Ressourcenmanager Bereitstellung Probleme erkennen."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Bereitstellungsvorgänge Azure Ressourcenmanager REST API anzeigen

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Wenn Sie einen Fehler Ressourcen in Azure bereitstellen erhalten, möchten Sie mehr über die Bereitstellungsvorgänge angezeigt, die ausgeführt wurden. Die REST-API enthält Vorgänge, mit denen Sie den Fehler und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sie können Fehler vermeiden, indem Ihre Vorlage und die Infrastruktur vor der Bereitstellung überprüfen. Sie können auch zusätzliche Anforderung und Antwortinformationen während der Bereitstellung, die später zur Fehlerbehebung protokollieren. Überprüfung und Protokollierung Anforderung und Antwort Informationen finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Problembehandlung bei REST API

1. Bereitstellen von Ressourcen mit der [vorlagenbereitstellung erstellen](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Um Informationen beizubehalten, die für das debugging hilfreich sein können, legen Sie die **DebugSetting** -Eigenschaft in JSON-Anforderung an **RequestContent** oder **ResponseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",      
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Standardmäßig ist der Wert **DebugSetting** auf **none**festgelegt. Bei der Angabe des **DebugSetting** Werts sorgfältig die Art der Informationen, die Sie während der Bereitstellung übergeben werden. Protokollieren von Informationen über die Anforderung oder Antwort konnte Sie potenziell vertrauliche Daten verfügbar machen, die durch die Bereitstellungsvorgänge abgerufen werden. 

2. Erhalten Sie Informationen über eine Bereitstellung mit [Informationen über die Bereitstellung einer Vorlage](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Beachten Sie in der Antwort insbesondere die ** **ProvisioningState** , **CorrelationId** und** Elemente. **CorrelationId** wird verwendet, um Ereignisse zu verfolgen und kann hilfreich sein, beim Arbeiten mit technischen Support, um ein Problem zu beheben.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Erhalten Sie Informationen über Bereitstellungsvorgänge mit der [Liste aller Vorlage Bereitstellungsvorgänge](https://msdn.microsoft.com/library/azure/dn790518.aspx) . 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Die Antwort enthält Angaben bzw. Antwort abhängig, was Sie während der Bereitstellung in der **DebugSetting** -Eigenschaft angegeben.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Ereignisse von Audit-Protokolle für die Bereitstellung mit der [Liste der Ereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) abrufen

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Nächste Schritte

- Hilfe zu bestimmten Bereitstellungsfehler beheben finden Sie unter [beheben Sie häufige Fehler bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zur Verwendung der Überwachungsprotokolle andere Aktionen überwachen, finden Sie unter [Vorgänge mit Ressourcen-Manager überwachen](resource-group-audit.md).
- Überprüfen Sie die Bereitstellung vor Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
