<properties
   pageTitle="Abhängigkeiten Ressourcenmanager Vorlagen | Microsoft Azure"
   description="Beschreibt, wie eine Ressource einer anderen Ressource abhängig während der Bereitstellung, um sicherzustellen, dass Ressourcen in der richtigen Reihenfolge bereitgestellt werden."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Dependencies in Azure Resource Manager Vorlagen definieren

Für eine bestimmte Ressource kann andere Ressourcen vorhanden sein müssen, bevor die Ressource bereitgestellt wird. SQL Server muss z. B. vor dem Bereitstellen einer SQL-Datenbank vorhanden. Diese Beziehung zu definieren, markieren eine Ressource der anderen Ressource abhängig. Definieren Sie eine Abhängigkeit mit der **DependsOn** normalerweise auch können sie über die Funktion **Verweis** . 

Ressourcenmanager wertet Abhängigkeiten Ressourcen und deren abhängige Reihenfolge bereitgestellt. Ressourcen nicht voneinander abhängig sind, lädt Ressourcen-Manager sie Parallel.

## <a name="dependson"></a>dependsOn

In der Vorlage kann DependsOn-Element als abhängig von einer oder mehreren Ressourcen eine Ressource definieren. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen. 

Das folgende Beispiel zeigt eine hängt ein Lastenausgleich, virtuelles Netzwerk und eine Schleife, die mehrere Speicherkonten erstellt VM-Skalierungsgruppe. Diese Ressourcen werden im folgenden Beispiel nicht angezeigt, müssen anderswo in der Vorlage.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Um eine Abhängigkeit zwischen einer Ressource und einer Schleife kopieren erstellten Ressourcen zu definieren, legen Sie DependsOn-Element Namen der Schleife. Ein Beispiel finden Sie unter [mehrere Instanzen von Ressourcen in Azure-Ressourcen-Manager erstellen](resource-group-create-multiple.md).

Während Sie DependsOn Beziehungen zwischen Ressourcen zuordnen können neigen, ist es wichtig zu verstehen, warum Sie es tun, da die Leistung Ihrer Bereitstellung auswirken können. Beispielsweise ist dokumentieren, wie Ressourcen miteinander, DependsOn nicht richtig. Sie können keine Abfragen Ressourcen nach der Bereitstellung im DependsOn-Element definiert wurden. Mithilfe von DependsOn auswirken, potenziell Bereitstellungszeit da Ressourcen-Manager nicht parallel zwei Ressourcen bereitstellt, die abhängig sind. Beziehungen zwischen Ressourcen zu dokumentieren, verwenden [Ressource verknüpfen](resource-group-link-resources.md).

## <a name="child-resources"></a>Untergeordnete Ressourcen

Resources-Eigenschaft können Sie untergeordnete Ressourcen angeben, die für die definierte Ressource verknüpft sind. Untergeordnete Ressourcen können nur definiert fünf Ebenen tief sein. Es ist wichtig zu beachten, dass eine implizite Abhängigkeit zwischen Ressource untergeordnete und übergeordnete Ressource nicht erstellt wird. Benötigen Sie die untergeordnete Ressource nach die übergeordnete Ressource bereitgestellt werden, müssen Sie explizit die Abhängigkeit mit der DependsOn-Eigenschaft angeben. 

Jede übergeordnete Ressource akzeptiert nur bestimmte Ressourcentypen als untergeordneten Ressourcen. Akzeptierte Ressourcentypen werden im [Schema der Vorlage](https://github.com/Azure/azure-resource-manager-schemas) der übergeordneten Ressource angegeben. Der Name des untergeordneten Ressourcentyp enthält den Namen des Ressourcentyps übergeordnete wie **Microsoft.Web/sites/config** und **Microsoft.Web/sites/extensions** sowohl untergeordneten Ressourcen **Microsoft.Web/sites**.

Das folgende Beispiel zeigt einen SQL Server und SQL-Datenbank. Beachten Sie, dass explizite Abhängigkeit zwischen der SQL-Datenbank und SQL Server definiert ist, auch wenn die Datenbank einen untergeordneten Server.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>Referenz

Die [Verweis-Funktion](resource-group-template-functions.md#reference) ermöglicht Ausdruck andere JSON-Wert-Paare oder ein Laufzeitressourcen seinen Wert abgeleitet. Referenzausdrücke deklarieren implizit, dass eine Ressource von einem anderen abhängt. 

    reference('resourceName').propertyPath

Sie können dieses Element oder DependsOn-Element abhängig, aber nicht für abhängige Ressource verwenden müssen. Verwenden Sie nach Möglichkeit einen impliziten Verweis um hinzufügen versehentlich eine unnötige Abhängigkeit zu vermeiden.

Weitere Informationen finden Sie unter [Referenz](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Nächste Schritte

- Zum Erstellen von Azure-Ressourcen-Manager-Vorlagen finden Sie unter [Erstellung Vorlagen](resource-group-authoring-templates.md). 
- Finden Sie eine Liste der verfügbaren Funktionen in einer Vorlage [Vorlagenfunktionen](resource-group-template-functions.md).

