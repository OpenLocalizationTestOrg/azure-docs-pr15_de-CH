<properties
    pageTitle="Mithilfe von Tags Organisieren Ihrer Azure-Ressourcen | Microsoft Azure"
    description="Veranschaulicht das Anwenden von Tags zum Organisieren von Ressourcen für die Abrechnung und Verwaltung."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Mithilfe von Tags Organisieren Ihrer Azure-Ressourcen

Ressourcen-Manager können Sie logisch Organisieren von Ressourcen durch Anwenden von Tags. Die Tags bestehen aus Schlüssel-Wert-Paare, die Ressourcen mit Eigenschaften zu identifizieren, die Sie definieren. Um Ressourcen zu derselben Kategorie markieren, wird wenden Sie dasselbe Tag Ressourcen an.

Wenn Sie Ressourcen mit bestimmten Tags anzeigen, sehen Sie Ressourcen aus allen Ressourcengruppen. Sie sind nicht nur Ressourcen in derselben Ressourcengruppe, können Sie Ihre Ressourcen verwalten, die unabhängige Bereitstellung Beziehung auf. Tags können hilfreich sein, wenn Sie Ressourcen für Abrechnung oder Management organisieren müssen.

Jeden Tag, einer Ressource oder Ressourcengruppe hinzufügen, wird Abonnement Wide Taxonomie automatisch hinzugefügt. Sie können die Taxonomie auch vorab für Ihr Abonnement mit Namen und Werten, wie Ressourcen in Zukunft markiert werden soll.

Jede Ressource oder Ressourcengruppe kann maximal 15 Kategorien haben. Der Tagname ist auf 512 Zeichen beschränkt und den Wert der Marke ist auf 256 Zeichen beschränkt.

> [AZURE.NOTE] Tags können nur auf Ressourcen angewendet werden, die Ressourcenmanager Operationen unterstützen. Wenn Sie einen virtuellen Computer, virtuelle Netzwerk- oder durch das klassische Bereitstellungsmodell erstellt (wie durch das Verwaltungsportal), einen Tag kann nicht auf die Ressource angewendet. Zur Unterstützung der Kennzeichnung erneut diese Ressourcen über Ressourcen-Manager. Alle anderen Ressourcen unterstützen tagging.

## <a name="templates"></a>Vorlagen

Um eine Ressource während der Bereitstellung markieren, einfach die Ressource, die Sie bereitstellen **Tags** Element hinzu, und geben Sie den Tagnamen und Wert. Tag-Namen und Wert müssen nicht in Ihrem Abonnement bereits vorhanden. Bis zu 15 Kategorien können Sie für jede Ressource.

Das folgende Beispiel zeigt ein Speicherkonto mit einem Tag.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Derzeit unterstützt Ressourcen-Manager kein Objekt für die Namen und Werte verarbeiten. Übergeben Sie stattdessen ein Objekt für die Variablenwerte, aber dennoch Geben Sie Namen, wie im folgenden Beispiel gezeigt.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST-API

Das Portal und PowerShell verwenden beide die [Ressourcenmanager REST-API](https://msdn.microsoft.com/library/azure/dn848368.aspx) im Hintergrund. Ggf. integrieren tagging in einer anderen Umgebung können Sie Tags mit GET auf die Ressourcen-Id erhalten und die Tags mit einem PATCH aktualisieren.


## <a name="tags-and-billing"></a>Tags und Abrechnung

Tags können Sie für die unterstützten Dienste Ihre Rechnungsdaten gruppieren. Z. B. können [virtuelle Computer integriert mit Azure-Ressourcen-Manager](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) Sie definieren und Anwenden von Tags, die Abrechnung Verwendung für virtuelle Computer verwalten. Ausführen von mehreren VMs für verschiedene Organisationen können Sie die Verwendung von Tags nach Kostenstelle.  
Tags können auch Kosten von Runtime-Umgebung kategorisieren. wie die Abrechnung Verwendung für VMs in Produktion ausgeführt.

Sie können Informationen über Tags durch [Azure Ressourcenverwendung und RateCard APIs](billing-usage-rate-card-overview.md) oder Verbrauch durch Kommas getrennte Werte (CSV) Datei abrufen. Sie downloaden Sie die Datei Verwendung von [Azure Konten Portal](https://account.windowsazure.com/) oder [EA-Portal](https://ea.azure.com). Weitere Informationen über den programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Einblicke in Ihre Microsoft Azure Ressourcenverbrauch](billing-usage-rate-card-overview.md). REST-API Operationen finden Sie unter [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Wenn Sie für Services die Verwendung CSV herunterladen, die mit Abrechnung unterstützen die Tags in der **Tag** -Spalte angezeigt. Weitere Informationen finden Sie auf [Ihrer Rechnung für Microsoft Azure verstehen](billing/billing-understand-your-bill.md).

![Finden Sie unter Tags in Rechnung](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Nächste Schritte

- Sie können eingeschränkt und Konventionen für Ihr Abonnement mit angepassten Richtlinien anwenden. Die Richtlinie definieren kann, dass alle Ressourcen einen Wert für ein bestimmtes Tag erfordern. Weitere Informationen finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Zugriffskontrolle](resource-manager-policy.md).
- Eine Einführung in Azure PowerShell beim Bereitstellen von Ressourcen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](./powershell-azure-resource-manager.md).
- Einführung in Azure CLI verwenden, bei der Bereitstellung von Ressourcen finden Sie unter [Verwendung der Azure-CLI für Mac, Linux und Windows Azure Resource Management](./xplat-cli-azure-resource-manager.md).
- Einführung in das Portal finden Sie unter [Verwenden des Azure-Portals Azure Ressourcen verwalten](./azure-portal/resource-group-portal.md)  
