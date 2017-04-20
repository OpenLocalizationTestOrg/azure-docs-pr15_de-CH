<properties
   pageTitle="Bereitstellen von Ressourcen mit REST-API und Vorlage | Microsoft Azure"
   description="Verwenden Sie Azure Resource Manager und Ressourcenmanager REST-API eine Ressourcen in Azure bereitstellen. Die Ressourcen werden in einer Ressourcen-Manager-Vorlage definiert."
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
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Bereitstellen von Ressourcen mit Ressourcen-Manager und Ressourcenmanager REST-API

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

Dieser Artikel erläutert die Ressourcenmanager REST API Ressourcenmanager Vorlagen verwenden, um Ressourcen in Azure bereitstellen.  

> [AZURE.TIP] Hilfe zum Debuggen eines Fehlers während der Bereitstellung finden Sie unter:
>
> - [Bereitstellung Ansichtsoperationen REST API](resource-manager-troubleshoot-deployments-rest.md) zum Abrufen von Informationen zu beheben den Fehler
> - [Problembehandlung bei Fehlermeldungen bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md) Informationen zu allgemeinen Bereitstellungsfehler

Die Vorlage kann eine lokale Datei oder eine externe Datei, die durch einen URI. Wenn die Vorlage in ein Speicherkonto befindet, können Sie Zugriff auf die Vorlage und Bereitstellung einer gemeinsamen Zugriff Signaturtoken (SAS) bieten.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Die REST-API bereitstellen
1. Legen Sie [Allgemeine Parameter und Header](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common)Authentifizierungstokens.
2. Wenn Sie eine vorhandene Ressourcengruppe nicht haben, erstellen Sie eine Ressourcengruppe. Geben Sie Ihre Abonnenten-Id, den Namen der neuen Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen. Weitere Informationen finden Sie unter [Erstellen einer Ressourcengruppe](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Überprüfen Sie die Bereitstellung vor der Ausführung von [Validate vorlagenbereitstellung](https://msdn.microsoft.com/library/azure/dn790547.aspx) Betrieb. Beim Testen der Bereitstellung Geben Sie Parameter, genau wie bei die Bereitstellung (siehe nächstes) ausführen.

3. Erstellen Sie eine Bereitstellung. Geben Sie Ihre Abonnenten-Id, den Namen der Ressourcengruppe bereitzustellen, den Namen der Bereitstellung sowie einen Link zu Ihrer Vorlage. Informationen über die Vorlagendatei finden Sie [Parameterdatei](#parameter-file). Weitere Informationen über die REST-API können Sie eine Ressourcengruppe erstellen finden Sie unter [vorlagenbereitstellung erstellen](https://msdn.microsoft.com/library/azure/dn790564.aspx). Beachten Sie, dass **inkrementelle** **Modus** festgelegt ist. Führen Sie eine vollständige Bereitstellung **Modus** auf **abgeschlossen**festgelegt. Vorsicht beim vollständigen Modus als versehentlich Ressourcen löschen, die nicht in der Vorlage sind.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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
              }
            }
          }
   
      Wenn Sie Antwortinhalt protokollieren möchten, enthalten Inhalte angefordert oder beides **DebugSetting** in der Anforderung.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Sie können das Speicherkonto einrichten, mit einem gemeinsamen Zugriff Signaturtoken (SAS). Weitere Informationen finden Sie unter [Zugriff mit einem SAS delegieren](https://msdn.microsoft.com/library/ee395415.aspx).

4. Abrufen des Status der vorlagenbereitstellung. Weitere Informationen finden Sie unter [Informationen zur vorlagenbereitstellung einer](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Nächste Schritte
- Ein Beispiel zum Bereitstellen von Ressourcen durch die Clientbibliothek .NET finden Sie unter [Ressourcen bereitstellen Proxybibliothek und eine Vorlage verwenden](virtual-machines/virtual-machines-windows-csharp-template.md).
- Zum Definieren von Parametern in Vorlage anzuzeigen Sie [Erstellung Vorlagen](resource-group-authoring-templates.md#parameters).
- Bereitstellen der Projektmappe in unterschiedlichen Umgebungen Siehe [Entwicklung und testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
- Informationen zur Referenz Schlüsseltresor sichere Werte übergeben finden Sie unter [sichere Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).
