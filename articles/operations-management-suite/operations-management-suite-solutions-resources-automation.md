<properties
   pageTitle="Ressourcen in OMS Solutions Automatisierung | Microsoft Azure"
   description="Projektmappen in OMS gehören normalerweise Runbooks in Azure Automation wie sammeln und Verarbeiten von Daten automatisieren.  Dieser Artikel beschreibt die Runbooks und zugehörigen Ressourcen in eine Lösung integrieren."
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
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatisierung von Ressourcen in OMS Solutions (Vorschau)

>[AZURE.NOTE]Dies ist eine vorläufige Dokumentation zum Erstellen der Lösungen in OMS die derzeit in der Vorschau. Alle unten beschriebenen Schema wird geändert.   

[Management-Lösung OMS](operations-management-suite-solutions.md) gehören normalerweise Runbooks in Azure Automation wie sammeln und Verarbeiten von Daten automatisieren.  Automatisierungskonten umfasst neben Runbooks wie Variablen und Zeitpläne, die in der Lösung verwendet Runbooks unterstützen.  Dieser Artikel beschreibt die Runbooks und zugehörigen Ressourcen in eine Lösung integrieren.

>[AZURE.NOTE]Die Beispiele in diesem Artikel verwenden Sie Parameter und Variablen, die erforderlich oder common Management Solutions und [Erstellen von](operations-management-suite-solutions-creating.md) Lösungen in Operations Management Suite (OMS) beschrieben 


## <a name="prerequisites"></a>Erforderliche Komponenten
Es wird vorausgesetzt, dass Sie bereits mit [eine Lösung erstellen](operations-management-suite-solutions-creating.md) und die Struktur einer vertraut.

## <a name="samples"></a>Beispiele
Sie können Ressourcenmanager Beispielvorlagen Automatisierung Ressourcen [Schnellstart Vorlagen in GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates)abrufen.

## <a name="automation-account"></a>Automation-Konto
Alle Ressourcen in Azure Automation [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview)enthält.  Wie in [OMS-Arbeitsbereich und Automatisierung](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) ist nicht Automation-Konto in der Management-Lösung enthalten muss vorhanden sein, bevor die Projektmappe installiert wird.  Wenn es nicht verfügbar ist, wird die Projektmappe installieren fehl.

Der Name des Kontos Automatisierung ist im Namen jeder Ressource Automatisierung.  Dies geschieht in der Projektmappe mit der **Kontoname** Parameter wie im folgenden Beispiel für ein Runbook Ressource.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure Automation Runbook](../automation/automation-runbook-types.md) Ressourcen haben ein **Microsoft.Automation/automationAccounts/runbooks** und Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| runbookType | Gibt die Typen des Runbooks. <br><br> Skript - PowerShell <br>PowerShell - PowerShell-workflow <br> GraphPowerShell - Grafik PowerShell Skript runbook <br> GraphPowerShellWorkflow - Grafik PowerShell Workflow runbook   |
| logProgress | Gibt an, ob [Datensätze Fortschritt](../automation/automation-runbook-output-and-messages.md) für Runbooks generiert werden soll. |
| logVerbose  | Gibt an, ob [ausführliche Datensätze](../automation/automation-runbook-output-and-messages.md) für Runbooks generiert werden soll. |
| Beschreibung | Optionale Beschreibung für die Runbook. |
| publishContentLink | Gibt den Inhalt des Runbooks. <br><br>URI - Uri, der den Inhalt des Runbooks.  Dadurch werden eine. ps1-Datei für PowerShell und Skript Runbooks und eine Datei exportierten Grafiken Runbook für ein Runbook Diagramm.  <br> Version – Version Runbooks zur eigenen Überwachung. |

Unten ist ein Beispiel für ein Runbook Ressource.  In diesem Fall ruft Runbooks [PowerShell Katalog](https://www.powershellgallery.com)ab.

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Ein Runbook starten
Es gibt zwei Methoden, um ein Runbook in einer Lösung starten.

- Um Runbooks starten, wenn die Projektmappe installiert wird, erstellt eine [Projekt Ressourcen](#automation-jobs) wie unten beschrieben.
- Um Runbooks nach einem Zeitplan zu [Zeitplan Ressource](#schedules) wie unten beschrieben erstellen. 


## <a name="automation-jobs"></a>Automatisierung Aufträge
Um ein Runbook starten, wenn die Management-Lösung installiert ist, erstellen Sie eine **Auftrag** Ressource.  Projekt Ressourcen haben ein **Microsoft.Automation/automationAccounts/jobs** und Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Runbook    | **Name** Entität mit dem Namen des Runbooks.  |
| Parameter | Entität für jeden Parameter Runbooks erforderlich. |


Der Auftrag enthält Runbook Namen und alle Parameterwerte Runbooks an.  Der Auftrag muss [hängt](operations-management-suite-solutions-creating.md#resources) vor dem Starten seit dem Runbook Runbook erstellt werden muss.  Außerdem erstellen Sie Abhängigkeiten auf andere Runbooks vor dem aktuellen Knoten ausgeführt werden soll.

Der Name einer Ressource Auftrag muss eine GUID normalerweise Parameter zugewiesen.  Erfahren Sie mehr über GUID-Parameter in [Creating Solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Es folgt ein Beispiel einer Job-Ressource, die ein Runbook beginnt, wenn die Management-Lösung installiert ist.  Ein anderer Runbooks, auszufüllen startet eine, diese Aufträge für diese Runbook abhängig ist.  Die Details für den Auftrag erhalten über Parameter und Variablen statt direkt angegeben.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Zertifikate
[Azure Automation Zertifikate](../automation/automation-certificates.md) haben ein **Microsoft.Automation/automationAccounts/certificates** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| base64Value   | Base-64-Wert für das Zertifikat. |
| Fingerabdruck    | Fingerabdruck des Zertifikats. |

Nachfolgend ein Beispiel einer Ressource Zertifikat.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Anmeldeinformationen
[Azure-Automatisierung](../automation/automation-credentials.md) Typ **Microsoft.Automation/automationAccounts/credentials** und haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Benutzername   | Benutzername für die Anmeldeinformationen. |
| Kennwort   | Kennwort für die Anmeldeinformationen. |

Unten ist ein Beispiel für eine Ressource Anmeldeinformationen.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Zeitpläne
[Azure Automation Zeitpläne](../automation/automation-schedules.md) haben ein **Microsoft.Automation/automationAccounts/schedules** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Beschreibung | Optionale Beschreibung für den Zeitplan. |
| Startzeit   | Gibt die Startzeit der Zeitplan als DateTime-Objekt. Eine Zeichenfolge kann angegeben werden, wenn ein gültiges DateTime konvertiert werden kann. |
| isEnabled   | Gibt an, ob der Zeitplan aktiviert ist. |
| Intervall    | Der Typ des Intervalls für den Zeitplan.<br><br>Tag<br>Stunde |
| Häufigkeit   | Häufigkeit, mit der Anzahl von Tagen oder Stunden der Zeitplan ausgelöst werden soll. |

Unten ist ein Beispiel für eine Ressource planen.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Variablen
[Azure Automation Variablen](../automation/automation-variables.md) haben ein **Microsoft.Automation/automationAccounts/variables** und Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Beschreibung | Optionale Beschreibung für die Variable. |
| isEncrypted | Gibt an, ob die Variable verschlüsselt werden soll. |
| Typ        | Der Datentyp der Variablen. |
| Wert       | Wert für die Variable. |

Nachfolgend ein Beispiel einer Variable Ressource.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Module
Management-Lösung muss nicht zum Definieren von [globalen Module](../automation/automation-integration-modules.md) von der Runbooks verwendet, da sie immer verfügbar sind.  Sie müssen eine Ressource für andere Modul die Runbooks verwendet und Runbooks sollten Modul Ressource sicherstellen, dass vor dem Runbook erstellt wird abhängig.

[Integrationsmodule](../automation/automation-integration-modules.md) haben ein **Microsoft.Automation/automationAccounts/modules** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| contentLink | Gibt den Inhalt des Moduls. <br><br>URI - Uri, der den Inhalt des Runbooks.  Dadurch werden eine. ps1-Datei für PowerShell und Skript Runbooks und eine Datei exportierten Grafiken Runbook für ein Runbook Diagramm.  <br> Version – Version Runbooks zur eigenen Überwachung. |

Nachfolgend ein Beispiel einer Ressource Modul.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Aktualisieren von Modulen
Wenn Sie eine managementlösung, die ein Runbook enthält, einen Zeitplan verwendet, aktualisieren und die neue Version der Projektmappe ein neues Modul, Runbook verwendet wurde, können Runbooks die alte Version des Moduls.  Sie enthalten die folgenden Runbooks in der Projektmappe und einen Auftrag ausführen vor anderen Runbooks erstellen.  Dadurch wird sichergestellt, dass alle Module aktualisiert werden müssen, bevor die Runbooks geladen werden.

- [Update ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) gewährleistet, dass alle Module von Runbooks in der Lösung verwendet die neueste Version.  
- [ReRegisterAutomationSchedule MS Management](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) registrieren alle die Ressourcen planen, um sicherzustellen, dass die Runbooks werden mit die neuesten Modulen verknüpft.


Folgendes ist ein Beispiel für die erforderlichen Elemente einer Lösung Modul-Update unterstützt.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen einer Ansicht der Projektmappe](operations-management-suite-solutions-resources-views.md) gesammelten Daten.