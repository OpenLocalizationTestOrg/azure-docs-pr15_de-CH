<properties
   pageTitle="Erstellen von Azure Ressourcenmanager Vorlagen | Microsoft Azure"
   description="Mit JSON-Deklarationssyntax bereitzustellende Anwendung in Azure Azure-Ressourcen-Manager Vorlagen erstellen."
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
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Azure-Ressourcen-Manager Vorlagen erstellen

Dieses Thema beschreibt die Struktur einer Vorlage Azure-Ressourcen-Manager. Es stellt die verschiedenen Abschnitte der Vorlage und die Eigenschaften, die in den Abschnitten zur Verfügung stehen. Die Vorlage umfasst JSON und Ausdrücke mit Werten für die Bereitstellung erstellen. 

Die Vorlage für Ressourcen, die Sie bereits bereitgestellt haben finden Sie unter [Exportieren eine Vorlage Azure Resource Manager von vorhandenen Ressourcen](resource-manager-export-template.md). Anleitung zum Erstellen einer Vorlage finden Sie unter [Exemplarische Vorgehensweise Ressourcenmanager Vorlage](resource-manager-template-walkthrough.md). Vorschläge zum Erstellen von Vorlagen finden Sie unter [Best Practices für Azure-Ressourcen-Manager Vorlagen erstellen](resource-manager-template-best-practices.md).

Ein guter JSON-Editor kann die Vorlagen vereinfachen. Informationen über Visual Studio mit Vorlagen finden Sie unter [Erstellen und Bereitstellen von Azure Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informationen über VS-Code finden Sie unter [Arbeiten mit Azure Ressourcenmanager Vorlagen in Visual Studio Code](resource-manager-vs-code.md).

Die Größe der Vorlage 1 MB und jede Parameterdatei auf 64 KB begrenzt. 1 MB-Begrenzung gilt für den endgültigen Status der Vorlage nach iterative Ressourcendefinitionen und Werte von Variablen und Parametern erweitert wurde. 

## <a name="template-format"></a>Vorlage

In ihrer einfachsten Struktur enthält eine Vorlage für die folgenden Elemente:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Elementnamen   | Erforderlich | Beschreibung
| :------------: | :------: | :----------
| $schema        |   Ja    | Speicherort der JSON-Schemadatei, die die Version der Vorlagensprache beschreibt. Verwenden Sie den URL im vorherigen Beispiel gezeigt.
| contentVersion |   Ja    | Version der Vorlage (z. B. 1.0.0.0). Sie können einen Wert für dieses Element bereitstellen. Bei der Bereitstellung von Ressourcen mithilfe der Vorlage kann dieser Wert verwendet werden, um sicherzustellen, dass die richtige Vorlage verwendet wird.
| Parameter     |   Nein     | Werte, die Ausführung-Bereitstellung anzupassende Ressourceneinsatz bereitgestellt werden.
| Variablen      |   Nein     | Werte, die als JSON-Fragmente in der Vorlage verwendeten Vorlagenausdrücke vereinfachen.
| Ressourcen      |   Ja    | Ressourcentypen, die bereitgestellt oder in einer Ressourcengruppe aktualisiert.
| Ausgaben        |   Nein     | Werte, die nach der Bereitstellung zurückgegeben werden.

Die Abschnitte der Vorlage weiter unten in diesem Thema ausführlich untersucht. Jetzt werden wir einige der Syntax überprüfen, die aus der Vorlage.

## <a name="expressions-and-functions"></a>Ausdrücke und Funktionen

Die grundlegende Syntax der Vorlage ist JSON. Ausdrücke und Funktionen erweitern, JSON, die in die Vorlage. Mit Ausdrücken können Werte strikten Literalwerte. Ausdrücke in Klammern eingeschlossen [und] und ausgewertet, wenn die Vorlage bereitgestellt wird. Ausdrücke können überall in einen Zeichenfolgenwert JSON und anderen JSON Wert immer zurück. Möchten Sie ein Zeichenfolgenliteral verwenden, bei denen eine Klammer [, verwenden Sie zwei Klammern [[.

Normalerweise verwenden Sie Ausdrücke mit Operationen für die Bereitstellung konfigurieren. Wie werden in JavaScript-Funktionsaufrufe als **functionName(arg1,arg2,arg3)**formatiert. Eigenschaften verweisen mithilfe der Operatoren Punkte und [Index].

Das folgende Beispiel zeigt, wie verschiedene Funktionen beim Erstellen Werte:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Die vollständige Liste der Vorlagenfunktionen finden Sie in [Azure Ressourcenmanager Vorlagenfunktionen](resource-group-template-functions.md). 


## <a name="parameters"></a>Parameter

Im Parameterabschnitt der Vorlage Geben Sie die Werte eingeben können, wenn die Ressourcen bereitstellen. Diese Werte können Sie die Bereitstellung mit zugeschnitten sind Werte für eine bestimmte Umgebung (z. B. Entwicklung, Test und Produktion) anpassen. Sie keinen Parameter in der Vorlage aber ohne Parameter wird die Vorlage immer dieselben Ressourcen mit demselben Namen, Speicherorte und Eigenschaften bereitstellen.

Diese Parameterwerte in der Vorlage können Sie Werte für die bereitgestellten Ressourcen festlegen. In anderen Abschnitten der Vorlage können nur im Parameterabschnitt deklarierten Parameter verwendet werden.

Definieren Sie die Parameter mit der folgenden Struktur:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Elementnamen   | Erforderlich | Beschreibung
| :------------: | :------: | :----------
| parameterName  |   Ja    | Name des Parameters. Muss ein gültiger Bezeichner JavaScript.
| Typ           |   Ja    | Typ des Parameterwerts. Finden Sie in der Liste der zulässigen Typen.
| Standardwert   |   Nein     | Der Standardwert für den Parameter, wenn kein Wert für den Parameter bereitgestellt wird.
| allowedValues  |   Nein     | Array der zulässigen Werte für den Parameter zu der richtige Wert bereitgestellt wird.
| minValue       |   Nein     | Der minimale Wert für Parameter vom Typ Int ist dieser Wert liegen.
| maxValue       |   Nein     | Der maximale Wert für Parameter vom Typ Int ist dieser Wert liegen.
| minLength      |   Nein     | Die minimale Länge für Zeichenfolge SecureString und Array-Typparameter ist dieser Wert liegen.
| maxLength      |   Nein     | Die maximale Länge für Zeichenfolge SecureString und Array-Typparameter ist dieser Wert liegen.
| Beschreibung    |   Nein     | Beschreibung der Parameter, der der Vorlage über die Schnittstelle Portal benutzerdefinierte Vorlage angezeigt wird.

Die zulässigen Typen und Werte sind:

- **Zeichenfolge**
- **secureString**
- **int**
- **bool**
- **Objekt** 
- **secureObject**
- **Array**

Geben Sie als optionalen Parameter angeben, DefaultValue (kann leer sein). 

Wenn Sie einen Parameternamen angeben, der eines der im Befehl zum Bereitstellen der Vorlage entspricht, werden Sie aufgefordert, einen Wert für einen Parameter mit dem Suffix **FromTemplate**bereitzustellen. Z. B. Wenn Sie einen Parameter mit dem Namen **ResourceGroupName** in der Vorlage, die entspricht dem Parameters **ResourceGroupName** [Neu AzureRmResourceGroupDeployment] [ deployment2cmdlet] -Cmdlets werden aufgefordert, einen Wert für **ResourceGroupNameFromTemplate**angeben. Im Allgemeinen sollten Sie diese Verwirrung durch Parameter nicht mit demselben Namen als Parameter für Bereitstellungsvorgänge benennen.

>[AZURE.NOTE] Alle Kennwörter, Schlüssel und andere Kennwörter sollten **SecureString** -Typ verwenden. Vorlagenparameter mit dem SecureString können nach der Bereitstellung der Ressource gelesen werden. 

Das folgende Beispiel zeigt, wie Parameter definieren:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Für das Eingabeparameter während der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Variablen

Im Variablenabschnitt erstellen Sie in der Vorlage Werte verwendet werden können. In der Regel Variablen Werte aus den Parametern basieren. Keine Variablen definieren müssen, aber sie oft die Vorlage vereinfachen, indem Sie komplexe Ausdrücke.

Definieren Sie Variablen mit der folgenden Struktur:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Im folgenden Beispiel wird veranschaulicht, wie eine Variable definieren, die aus zwei Parameterwerte:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Das nächste Beispiel zeigt eine Variable, die einen komplexen Typ JSON und Variablen, die von anderen Variablen erstellt werden:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Ressourcen

Im Ressourcenabschnitt legen Sie Ressourcen, die bereitgestellt oder aktualisiert werden. Dieser Abschnitt kann kompliziert werden, da die Typen verstehen müssen, um die richtigen Werte bereitstellen. 

Definieren Sie mit der folgenden Struktur:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Elementnamen             | Erforderlich | Beschreibung
| :----------------------: | :------: | :----------
| Anforderungsparameter               |   Ja    | Version der REST-API zum Erstellen der Ressource.
| Typ                     |   Ja    | Typ der Ressource. Dieser Wert ist eine Kombination aus den Namespace des Ressourcenanbieters und Ressourcen (z. B. **Microsoft.Storage/storageAccounts**).
| Name                     |   Ja    | Name der Ressource. Der Name muss ein URI-Komponente Beschränkungen in RFC3986. Darüber hinaus Azure Services, die den Ressourcennamen externen Parteien auf Namen überprüfen verfügbar machen ist nicht versucht eine Identität vortäuschen. Sehen Sie [Ressourcenname](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| Speicherort                 |   Variiert  | Geo-Standorte der angegebenen Ressource unterstützt. Wählen Sie eines der verfügbaren Standorte, aber in der Regel sinnvoll, die in der Nähe der Benutzer auswählen. In der Regel ist es sinnvoll Ressourcen an, die im selben Bereich interagieren. Die meisten Ressourcentypen erfordern einen Speicherort, aber einige Typen (z. B. die rollenzuweisung einer) erfordern einen Speicherort.
| Tags                     |   Nein     | Tags, die der Ressource zugeordnet sind.
| Kommentare                 |   Nein     | Notizen zum Dokumentieren der Ressourcen in der Vorlage
| dependsOn                |   Nein     | Ressourcen, denen die definierten Ressource abhängig. Die Abhängigkeit zwischen Ressourcen ausgewertet und Ressourcen in der abhängige Reihenfolge bereitgestellt werden. Wenn Ressourcen nicht voneinander abhängig sind, werden sie gleichzeitig bereitgestellt. Der Wert kann eine durch Trennzeichen getrennte Liste einer Ressource Namen oder eindeutiger Ressourcenbezeichner.
| Eigenschaften               |   Nein     | Spezifische Konfigurationen. Werte für die Eigenschaften entsprechen den Werten im Hauptteil Anforderung für die REST API-Operation (PUT-Methode) zum Erstellen der Ressource verwendet. Links zu Ressourcen Schemadokumentation oder REST-API finden Sie unter [Ressourcenmanager Anbieter, Regionen, API-Versionen und Schemas](resource-manager-supported-services.md).
| Kopieren                     |   Nein     | Ggf. mehrere Instanzen der Anzahl der Ressourcen zu erstellen. Weitere Informationen finden Sie unter [mehrere Instanzen von Ressourcen in Azure-Ressourcen-Manager erstellen](resource-group-create-multiple.md). |
| Ressourcen                |   Nein     | Untergeordnete Ressourcen, die definierte Ressource abhängig. Sie können nur Typen, die durch das Schema die übergeordnete Ressource zulässig sind. Der vollqualifizierte Name des Ressourcentyps untergeordneten enthält den übergeordneten Ressourcentyp wie **Microsoft.Web/sites/extensions**. Abhängigkeit auf die übergeordnete Ressource gilt nicht; Sie müssen diese Abhängigkeit explizit definieren. 

Wissen, welche Werte für **Anforderungsparameter**angeben, ist **Typ**und **Speicherort** nicht offensichtlich. Glücklicherweise können Sie diese Werte mithilfe von Azure PowerShell oder Azure CLI feststellen.

Um alle verwenden Ressourcenprovider **PowerShell**:

    Get-AzureRmResourceProvider -ListAvailable

Die zurückgegebene Liste Suchen Sie Ressourcenprovider interessiert sind. Verwenden Sie zum Abrufen der Ressourcentypen für ein Ressourcenanbieter (z.B. Speicher)

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Um API-Versionen für einen Ressourcentyp (solche Speicherkonten) abzurufen, verwenden Sie:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Zu unterstützten Speicherorte für einen Ressourcentyp zu verwenden:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Um alle verwenden Ressourcenprovider mit **Azure-Befehlszeilenschnittstelle**:

    azure provider list

Die zurückgegebene Liste Suchen Sie Ressourcenprovider interessiert sind. Verwenden Sie zum Abrufen der Ressourcentypen für ein Ressourcenanbieter (z.B. Speicher)

    azure provider show Microsoft.Storage

Unterstützte Speicherorte und API-Versionen verwenden:

    azure provider show Microsoft.Storage --details --json

Weitere Informationen zu Ressourcen finden Sie unter [Ressourcenmanager Anbieter, Regionen, API-Versionen und Schemas](resource-manager-supported-services.md).

Ressourcenabschnitt enthält eine Reihe von Ressourcen bereitstellen. Einer einzelnen Ressource können Sie auch ein Array von untergeordneten Ressourcen definieren. Daher könnte die Ressourcenabschnitt eine Struktur haben:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Das folgende Beispiel zeigt eine **Microsoft.Web/serverfarms** und eine **Microsoft.Web/sites** -Ressource mit einem untergeordneten **Extensions** Ressource. Beachten Sie, dass die Website als abhängig von der Serverfarm gekennzeichnet ist, da die Serverfarm vorhanden sein muss, bevor die Website bereitgestellt werden kann. Beachten Sie auch **Extensions** -Ressource ist ein untergeordnetes Element des Standorts.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Ausgaben

Im Abschnitt Ergebnisse geben Sie die Bereitstellung zurückgegebenen Werte. Beispielsweise konnte den URI um eine bereitgestellte Ressource zurück.

Das folgende Beispiel zeigt die Struktur einer Ausgabe-Definition:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Elementnamen   | Erforderlich | Beschreibung
| :------------: | :------: | :----------
| outputName     |   Ja    | Name der Ausgabewert. Muss ein gültiger Bezeichner JavaScript.
| Typ           |   Ja    | Typ der Ausgabewert. Ausgabewerte unterstützt folgende Vorlage von Eingabeparametern.
| Wert          |   Ja    | Vorlage Language-Ausdruck, der ausgewertet und als Wert zurückgegeben.


Das folgende Beispiel zeigt einen Wert im Bereich Ausgaben zurückgegeben wird.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Weitere Informationen zum Arbeiten mit anzeigen Sie [Freigabe Status im Ressourcenmanager Azure Vorlagen](best-practices-resource-manager-state.md)

## <a name="next-steps"></a>Nächste Schritte
- Vollständige Vorlagen für viele verschiedene Typen von Projektmappen finden Sie unter [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/).
- Informationen über die Funktionen, die innerhalb einer Vorlage verwendet werden können, finden Sie in [Azure Ressourcenmanager Vorlage Funktionen](resource-group-template-functions.md).
- Kombinieren mehrerer Vorlagen während der Bereitstellung finden Sie unter [verknüpfte Vorlagen mit Azure-Ressourcen-Manager](resource-group-linked-templates.md).
- Sie möchten Ressourcen verwenden, die in einer anderen Ressourcengruppe vorhanden sind. Dieses Szenario wird häufig bei der Arbeit mit Speicherkonten oder virtuelle Netzwerke, die über mehrere Ressourcengruppen verwendet werden. Weitere Informationen finden Sie unter [ResourceId-Funktion](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
