<properties 
    pageTitle="Verknüpfen von Ressourcen in Azure-Ressourcen-Manager | Microsoft Azure" 
    description="Erstellen einer Verknüpfung zwischen verwandten Ressourcen in verschiedenen Ressourcengruppen in Azure-Ressourcen-Manager." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Verknüpfen von Ressourcen in Azure-Ressourcen-Manager

Bereitstellung eine Ressource eine andere Ressource abhängig markieren während dieses Lebenszyklus endet bei der Bereitstellung. Nach der Bereitstellung keine identifizierte Beziehung besteht zwischen abhängigen Ressourcen. Ressourcen-Manager bietet ein Feature namens Ressource um dauerhafte Beziehungen zwischen Ressourcen verknüpfen.

Mit der Ressource verknüpfen, können Sie Beziehungen dokumentieren, die Ressourcengruppen umfassen. Beispielsweise ist es üblich, um eine Datenbank mit seinen eigenen Lebenszyklus in einer Ressourcengruppe befinden und eine Anwendung mit einem anderen Lebenszyklus befinden sich in einer anderen Ressourcengruppe. Die Anwendung verbindet sich mit der Datenbank sollten Sie eine Verknüpfung zwischen der Anwendung und der Datenbank zu markieren. 

Alle verknüpfte Ressourcen müssen dieselbe Abonnement gehören. Jede Ressource kann mit 50 andere Ressourcen verknüpft. Durch die REST-API ist die einzige Möglichkeit, verwandte Ressourcen abzufragen. Verknüpfte Ressourcen gelöscht oder verschoben werden, muss bestehende Verknüpfung Link Besitzer bereinigen. Sie werden **nicht** gewarnt beim Löschen von Ressourcen mit anderen Ressourcen verknüpft ist.

## <a name="linking-in-templates"></a>Verknüpfen mit Vorlagen

Eine Verknüpfung in einer Vorlage zu definieren, schließen Sie einen Ressourcentyp, der Namespace Anbieter Ressource kombiniert Ressourcentyp Quelle mit **/providers/links**. Der Name muss der Name der Quelle Ressource enthalten. Sie bieten die Ressourcen-Id der Ressource. Im folgenden Beispiel wird eine Verknüpfung zwischen einer Website und ein Speicherkonto.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Eine vollständige Beschreibung der Vorlage finden Sie [Ressourcenlinks - Vorlage Schema](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Verknüpfung mit REST-API

Um eine Verknüpfung zwischen bereitgestellten Ressourcen zu definieren, führen Sie Folgendes aus:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Die Abonnement-Id {Abonnement-Id} ersetzen. Ersetzen Sie {Ressourcengruppe}, {Namespace Anbieter, {Resource Type} und {Ressourcenname} mit den Werten, die die erste Ressource in der Verknüpfung identifizieren. Der Name des zu erstellenden links ersetzen Sie {Link-Name}. Mit 2015-01-01 für die api-Version.

Enthalten Sie in der Anforderung ein Objekt, das die zweite Ressource in den Link definiert:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Properties-Element enthält den Bezeichner für die zweite Ressource.

Sie können Links in Ihrem Abonnement mit Abfragen:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Weitere Beispiele zum Abrufen von Informationen über Links finden Sie unter [Verknüpfte Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Nächste Schritte

- Sie können auch Ihre Ressourcen mit Tags organisieren. Tagging Ressourcen finden Sie unter [Tags zu Ressourcen verwenden](resource-group-using-tags.md).
- Eine Beschreibung der Vorlagen erstellen und definieren die Ressourcen bereitgestellt werden finden Sie unter [Erstellung Vorlagen](resource-group-authoring-templates.md).
