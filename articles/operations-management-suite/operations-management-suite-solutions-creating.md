<properties
   pageTitle="Erstellen der Lösungen in Operations Management Suite (OMS) | Microsoft Azure"
   description="Lösungen erweitern die Funktionen von Operations Management Suite (OMS) mit verpackten Szenarien, die Kunden den OMS-Arbeitsbereich hinzufügen können.  Dieser Artikel enthält Informationen zum Erstellen der Lösungen in Ihrer Umgebung verwendet werden oder Ihre Kunden zur Verfügung gestellt."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Erstellen von Lösungen in Operations Management Suite (OMS) (Vorschau)

>[AZURE.NOTE]Dies ist eine vorläufige Dokumentation zum Erstellen der Lösungen in OMS die derzeit in der Vorschau. Alle unten beschriebenen Schema wird geändert.  

Lösungen erweitern die Funktionen von Operations Management Suite (OMS) mit verpackten Szenarien, die Kunden den OMS-Arbeitsbereich hinzufügen können.  Dieser Artikel enthält Details zum Erstellen Ihre eigene Lösung in Ihrer Umgebung verwenden oder Kunden durch die Gemeinschaft zur Verfügung.

## <a name="planning-your-management-solution"></a>Planen Ihre Management-Lösung
Management-Lösung OMS enthalten mehrere Ressourcen für ein bestimmtes Szenario.  Bei der Planung sollten Sie das Szenario, das Sie versuchen zu erreichen und alle erforderlichen Ressourcen für die Unterstützung konzentrieren.  Jede Lösung sollte eigenständig und definieren jede Ressource benötigt, auch wenn eine oder mehrere Ressourcen auch von anderen Projektmappen definiert sind.  Bei der Installation einer Lösung wird jede Ressource erstellt, sofern sie bereits vorhanden und Sie können definieren, was Ressourcen, wenn eine Projektmappe entfernt wird.  

Für beispielsweise eine Lösung ein [Azure Automation Runbook](../automation/automation-intro.md) , die sammelt Protokollanalyse Repository mit einem [Zeitplan](../automation/automation-schedules.md) und einer [Ansicht](../log-analytics/log-analytics-view-designer.md) , die verschiedenen Visualisierung von Daten bereitstellt.  Demselben Zeitplan kann von einer anderen Lösung verwendet werden.  Als Autor Lösung Management würde alle drei Ressourcen definieren jedoch festlegen, dass Runbook und Ansicht automatisch bei entfernt werden soll die Lösung entfernt.    Sie auch Zeitplan definieren, aber festlegen, dass an Stelle bleiben soll die Lösung von der Lösung entfernt wurden bei verwendet wurde.

## <a name="management-solution-files"></a>Projektmappendateien Management
Management-Lösung werden als [Ressourcenverwaltungs-Vorlagen](../resource-manager-template-walkthrough.md)implementiert.  Die Hauptaufgabe lernen Lösungen erstellen lernen wie [Autor eine Vorlage](../resource-group-authoring-templates.md).  Dieser Artikel enthält eindeutige Informationen Vorlagen für Projektmappen sowie gebräuchliche Lösung Ressourcen verwendet.

Die grundlegende Struktur einer Projektmappendatei Management entspricht einem [Ressourcenmanager Vorlage](resource-group-authoring-templates.md#template-format) wie folgt.  Die folgenden Abschnitte beschreiben die obersten Elemente und ihren Inhalt in einer Lösung.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameter

[Parameter](../resource-group-authoring-templates.md#parameters) sind Werte, die vom Benutzer erforderlich sein, wenn sie die Management-Lösung installieren.  Die Standardparameter, die alle Projektmappen sind und Sie können zusätzliche Parameter nach Bedarf für spezielle Lösung.  Wie Benutzer Parameterwerte können bei der Installation der Lösung hängt bestimmter Parameter und wie die Lösung installiert wird.


Bei Ihrem Verwaltungssystem [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) oder [Azure Schnellstart Vorlagen](operations-management-suite-solutions.md#finding-and-installing-solutions) werden sie aufgefordert eine [OMS-Arbeitsbereich und Automation-Konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account).  Diese werden verwendet, die Werte der Standardparameter aufgefüllt.  Der Benutzer wird nicht aufgefordert, Werte für die Standardparameter direkt bereitstellen, aber sie werden aufgefordert, Werte für zusätzliche Parameter.

Bei der [anderen Methode](operations-management-suite-solutions.md#finding-and-installing-solutions)installieren, müssen sie einen Wert für alle Standardparameter und alle weiteren Parameter angeben.

Nachfolgend finden Sie ein Beispiel-Parameter.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Die folgende Tabelle beschreibt die Attribute eines Parameters.

| Attribut | Beschreibung |
|:--|:--|
| Typ        | Der Datentyp für den Parameter. Für den Benutzer angezeigte Eingabesteuerelement hängt der Datentyp.<br><br>Bool - Dropdown-Feld<br>String - Textfeld<br>Int - Textfeld<br>SecureString - Kennwortfeld<br> |
| Kategorie    | Optionale Kategorie für den Parameter.  Parameter in der gleichen Kategorie werden gruppiert. |
| Steuerelement     | Zusätzliche Funktionen für Parameter.<br><br>DateTime - Datumssteuerelement angezeigt.<br>GUID - Guid-Wert wird automatisch generiert, und der Parameter wird nicht angezeigt. |
| Beschreibung | Optionale Beschreibung für den Parameter.  Ein Informationssymbol neben dem Parameter angezeigt. |


### <a name="standard-parameters"></a>Standardparameter
Die folgende Tabelle listet die Standardparameter für alle Informationsmanagement.  Diese Werte werden für den Benutzer statt nach ihnen bei der Installation der Lösung von Azure Marketplace oder Schnellstart Vorlagen gefüllt.  Benutzer muss Werte dafür angeben, wenn die Lösung mit einer anderen Methode installiert ist.

>[AZURE.NOTE]Die Benutzeroberfläche in Azure Marketplace und Schnellstart Vorlagen erwartet die Parameternamen in der Tabelle.  Verwenden Sie unterschiedliche Namen dann der Benutzer aufgefordert diese und sie nicht automatisch aufgefüllt.


| Parameter | Typ | Beschreibung |
|:--|:--|:--|
| Kontoname       | Zeichenfolge |  Azure Automation-Kontoname. |
| pricingTier       | Zeichenfolge | Tarif der Protokollanalyse Arbeitsbereich und Azure Automation-Konto. |
| regionId          | Zeichenfolge | Region des Kontos Azure Automation. |
| solutionName      | Zeichenfolge | Name der Lösung. |
| Ein     | Zeichenfolge | Analytics Arbeitsbereich Protokollname. |
| workspaceRegionId | Zeichenfolge | Bereich des Arbeitsbereichs Protokollanalyse. |





### <a name="sample"></a>Beispiel
Folgendes ist ein Beispiel-Parameter für eine Lösung.  Hierzu zählen alle Standardparameter und zwei zusätzliche Parameter in derselben Kategorie.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Verweisen auf Werte in anderen Elementen der Lösung mit Syntax **Parameter ("Parametername")**.  Z. B. Zugriff auf den Arbeitsbereichsnamen verwenden **parameters('workspaceName')** Sie 

## <a name="variables"></a>Variablen

**Variablen** -Element enthält Werte, mit denen Sie den Rest der Management-Lösung.  Diese Werte sind für den Benutzer die Projektmappe installieren nicht verfügbar.  Sie sollen den Autor zentrale sie Werte verwalten können, die mehrmals in der Lösung verwendet werden kann. Sie sollten alle Werte bestimmten Lösung Variablen statt Hartcodieren sie im **Ressourcenelement** gesetzt.  Dies ist der Code besser lesbar und können Sie einfach diese Werte in späteren Versionen ändern.

Es folgt ein Beispiel eines **Variablen** Elements mit normalen Parameter Solutions.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Verweisen auf Werte über die Projektmappe mit der Syntax **Variablen ("Variablenname")**.  Beispielsweise Zugriff auf die Variable SolutionName verwenden **variables('solutionName')** Sie 


## <a name="resources"></a>Ressourcen

**Ressourcen** -Element definiert die verschiedenen Ressourcen Management-Lösung.  Dadurch werden die größten und komplexesten Teil der Vorlage.  Ressourcen werden mit der folgenden Struktur definiert.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Abhängigkeit
**DependsOn** -Elemente gibt eine [Abhängigkeit](../resource-group-define-dependencies.md) von einer anderen Ressource.  Wenn die Projektmappe installiert ist, wird eine Ressource erst Abhängigkeit erstellt wurden erstellt.  Kann beispielsweise Ihre Lösung [ein Runbook starten](operations-management-suite-solutions-resources-automation.md#runbooks) beim [Auftrag Ressource](operations-management-suite-solutions-resources-automation.md#automation-jobs)installiert ist.  Job Ressource würden Runbook Ressource zu Runbook erstellt wird, bevor der Auftrag erstellt wurde abhängig sein.

### <a name="oms-workspace-and-automation-account"></a>OMS-Arbeitsbereich und Automation-Konto
Lösungen erfordern eine [OMS Arbeitsbereich](../log-analytics/log-analytics-manage-access.md) Ansichten enthalten und ein [Konto Automatisierung](../automation/automation-security-overview.md#automation-account-overview) Runbooks und zugehörige Ressourcen enthalten.  Diese müssen verfügbar sein, bevor die Ressourcen in der Projektmappe erstellt und sollten nicht in der Projektmappe selbst definiert.  Wird dem Benutzer [einen Arbeitsbereich und Konto angeben](operations-management-suite-solutions.md#oms-workspace-and-automation-account) als sie die Projektmappe bereitstellen, als Autor sollten Sie folgende Punkte berücksichtigen.


## <a name="solution-resource"></a>Lösung-Ressource
Jede Lösung erfordert einen Eintrag in der **Ressourcen** -Element, das die Projektmappe selbst definiert.  Dies muss ein **Microsoft.OperationsManagement/solutions**.  Es folgt ein Beispiel einer Lösung Ressource.  In den folgenden Abschnitten werden die verschiedenen Elemente beschrieben.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Projektmappenname
Der Projektmappenname muss in der Azure-Abonnement eindeutig sein. Der empfohlene Wert ist folgende.  Diese verwendet eine Variable namens **Projektmappenname** Basisnamen und **ein** Parameter um sicherzustellen, dass der Name eindeutig ist.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Dies würde einen Namen wie folgt aufgelöst.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Abhängigkeit
Lösung Ressource muss eine [Abhängigkeit](../resource-group-define-dependencies.md) für jede Ressource in der Projektmappe verfügen, da sie müssen vorhanden sein, bevor die Projektmappe erstellt werden kann.  Dazu müssen Sie einen Eintrag für jede Ressource im **DependsOn** -Element hinzufügen.


### <a name="properties"></a>Eigenschaften
Die Lösung Ressource besitzt die Eigenschaften in der folgenden Tabelle.  Dies umfasst Ressourcen verwiesen wird und die Lösung, die definiert, wie die Ressource verwaltet wird, ist die Installation die Projektmappe enthalten.  Jede Ressource in der Lösung sollten in die **ReferencedResources** oder die **ContainedResources** -Eigenschaft aufgelistet.

| Eigenschaft | Beschreibung |
|:--|:--|
| workspaceResourceId | ID des Arbeitsbereichs OMS in Form * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Arbeitsbereichsname\>*. |
| referencedResources   | Liste der Ressourcen in der Lösung nicht entfernt werden soll, wenn die Projektmappe entfernt wird. |
| containedResources   | Liste der Ressourcen in der Projektmappe entfernt werden soll, wenn die Projektmappe entfernt wird. |

Im obigen Beispiel wird mit ein Runbook planen und anzeigen.  Zeitplan und Runbook sind *referenzierten* **Eigenschaften** im Element, damit sie beim Entfernen der Lösung nicht entfernt werden.  Die Ansicht ist *enthaltenen* entfernt, wenn die Projektmappe entfernt wird.


### <a name="plan"></a>Planen
**Plan** Einheit der Ressource Lösung hat die Eigenschaften in der folgenden Tabelle. 

| Eigenschaft | Beschreibung |
|:--|:--|
| Name          | Name der Lösung. |
| Version       | Version der Lösung vom Autor bestimmt. |
| Produkt       | Eindeutige Zeichenfolge die Lösung. |
| Publisher     | Herausgeber. |


## <a name="other-resources"></a>Andere Ressourcen
Erhalten Sie Informationen und Ressourcen-Management-Lösungen in den folgenden Artikeln sind Beispiele.

- [Ansichten und dashboards](operations-management-suite-solutions-resources-views.md)
- [Automation-Ressourcen](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testen einer Lösung
Vor der Bereitstellung Ihrer Management-Lösung empfohlen mit [Test-AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell)testen.  Überprüft die Projektmappendatei und alle Probleme vor der Bereitstellung helfen.



## <a name="next-steps"></a>Nächste Schritte

- Enthält die Details der [Azure-Ressourcen-Manager erstellen Vorlagen](../resource-group-authoring-templates.md).
- Suche [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates) Beispiele für verschiedene Ressourcen-Manager-Vorlagen.
- Einzelheiten Sie zum [Hinzufügen von Ansichten einer Lösung](operations-management-suite-solutions-resources-views.md).
- Einzelheiten Sie zum [Hinzufügen von Ressourcen zu einer Lösung Automatisierung](operations-management-suite-solutions-resources-automation.md).
 