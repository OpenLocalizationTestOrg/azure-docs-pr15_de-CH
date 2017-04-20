<properties
   pageTitle="Ansichten in Management Solutions Operations Management Suite (OMS) | Microsoft Azure"
   description="Management Solutions in Operations Management Suite (OMS) beinhalten in der Regel eine oder mehrere Ansichten Daten.  Dieser Artikel beschreibt das Exportieren einer Ansicht von der Ansicht-Designer erstellt und in einer Lösung. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Ansichten in Operations Management Suite (OMS) Management Solutions (Vorschau)

>[AZURE.NOTE]Dies ist eine vorläufige Dokumentation zum Erstellen der Lösungen in OMS die derzeit in der Vorschau. Alle unten beschriebenen Schema wird geändert.    

[Management Solutions in Operations Management Suite (OMS)](operations-management-suite-solutions.md) beinhalten in der Regel eine oder mehrere Ansichten Daten.  Dieser Artikel beschreibt das Exportieren einer Ansicht von der [Ansicht-Designer](../log-analytics/log-analytics-view-designer.md) erstellt und in einer Lösung.  

>[AZURE.NOTE]Die Beispiele in diesem Artikel verwenden Sie Parameter und Variablen, die erforderlich oder common Management Solutions und [Erstellen von](operations-management-suite-solutions-creating.md) Lösungen in Operations Management Suite (OMS) beschrieben 


## <a name="prerequisites"></a>Erforderliche Komponenten
Es wird vorausgesetzt, dass Sie bereits mit [eine Lösung erstellen](operations-management-suite-solutions-creating.md) und die Struktur einer vertraut.


## <a name="overview"></a>Übersicht

Auf eine Ansicht in einer Lösung, erstellen Sie eine **Ressource** für ihn in der [Projektmappendatei](operations-management-suite-solutions-creating.md).  JSON, die detaillierte Konfiguration der Ansicht beschreibt ist in der Regel komplexer und kein gebräuchliche Lösung Autor manuell erstellen können.  Die gebräuchlichste Methode ist die Ansicht mit dem [Ansicht-Designer](../log-analytics/log-analytics-view-designer.md)erstellen, exportieren und dann die detaillierte Konfiguration der Projektmappe hinzufügen. 

Die grundlegenden Schritte zum Hinzufügen einer Ansicht zu einer Lösung sind.  Jeder Schritt wird ausführlicher in den folgenden Abschnitten beschrieben.

1. Exportieren Sie die Ansicht in eine Datei.
2. Erstellen Sie die Ansicht Ressource in der Projektmappe.
3. Fügen Sie Details anzeigen.

## <a name="export-the-view-to-a-file"></a>Die Ansicht in eine Datei exportieren
Führen Sie die Schritte am [Protokoll Analytics Ansicht-Designer](../log-analytics/log-analytics-view-designer.md) eine Ansicht in eine Datei exportieren.  Die exportierte Datei werden im JSON-Format die gleichen [Elemente wie die Projektmappendatei](operations-management-suite-solutions-creating.md#management-solution-files).  

**Ressourcenelement Datei anzeigen** haben eine Ressource mit einem **Microsoft.OperationalInsights/workspaces** , die OMS-Arbeitsbereich darstellt.  Dieses Element wird ein Unterelement mit **Ansichten** verfügen, stellt die Ansicht und enthält detaillierte Konfiguration.  Sie kopieren die Details dieses Elements und kopieren Sie sie in Ihrer Lösung.


## <a name="create-the-view-resource-in-the-solution"></a>Die Ansicht Ressource in der Projektmappe erstellen
Die folgende Ansicht Ressource **Ressourcen** Element die Projektmappendatei hinzufügen  Variablen, die unter beschrieben werden, müssen Sie auch hinzufügen verwendet.  Beachten Sie, dass die **Dashboards** und **OverviewTile** Eigenschaften Platzhalter, die Sie mit den entsprechenden Eigenschaften in die exportierte Ansichtsdatei überschreiben.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

[Variablen](operations-management-suite-solutions-creating.md#variables) Bestandteil der Projektmappendatei die folgenden Variablen hinzu, und Ersetzen Sie die Werte für die Projektmappe.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Beachten Sie, dass die gesamte Ressource konnte von der exportierte Ansichtsdatei kopieren Sie müssten die folgenden Änderungen in der Lösung funktionieren.  

- **Typ** für die Ressource anzeigen muss von **Ansichten** in **Microsoft.OperationalInsights/workspaces**geändert werden.
- Die **Name** -Eigenschaft für die Ressource anzeigen muss geändert werden, um den Arbeitsbereich einfügen.
- Die Abhängigkeit der Arbeitsbereich muss entfernt werden, da die Arbeitsbereich-Ressource in der Lösung definiert ist.
- **DisplayName** -Eigenschaft soll der Ansicht hinzugefügt werden.  **Id**, **Name**und **DisplayName** müssen alle übereinstimmen.
- Parameternamen müssen geändert werden, um die erforderlichen Parameter entsprechen.
- Variablen werden in der Projektmappe festgelegt und in den entsprechenden Eigenschaften verwendet.

## <a name="add-the-view-details"></a>Fügen Sie Details anzeigen
Zwei Elemente im **Eigenschaften** Element **Dashboard** und **OverviewTile** enthält die detaillierte Konfiguration der Ansicht enthalten Ansicht Ressource in die exportierte Ansichtsdatei.  Kopieren Sie diese beiden Elemente und deren Inhalte in **Eigenschaftselement Ressource anzeigen in der Projektmappendatei** . 

## <a name="example"></a>Beispiel
Im folgende Beispiel wird z. B. eine einfache Datei mit einer Ansicht.  Auslassungspunkte (...) für das **Dashboard** und **OverviewTile** Inhalt aus Platzgründen werden angezeigt.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie Einzelheiten [Management](operations-management-suite-solutions-creating.md)Lösungen.
- Enthalten Sie [Automatisierung Runbooks Management-Lösung](operations-management-suite-solutions-resources-automation.md).