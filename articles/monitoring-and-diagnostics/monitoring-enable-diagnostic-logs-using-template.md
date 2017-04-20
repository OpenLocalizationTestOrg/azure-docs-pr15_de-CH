<properties
    pageTitle="Diagnostische Einstellungen Ressourcenmanager Vorlage automatisch aktivieren | Microsoft Azure"
    description="Informationen Sie zum Ressourcen-Manager-Vorlage Diagnose Einstellungen erstellen, mit denen Sie die Diagnoseprotokolle an Ereignis Hubs stream oder in ein Speicherkonto speichern."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Erstellen von Ressourcen mit Ressourcen-Manager-Vorlage aktivieren Sie automatisch Diagnoseeinstellungen
In diesem Artikel zeigen wir die Verwendung einer [Vorlage Azure-Ressourcen-Manager](../resource-group-authoring-templates.md) zum Konfigurieren Diagnose für eine Ressource bei der Erstellung. Dadurch werden automatisch die Diagnoseprotokolle und Messverfahren Event-Hubs in ein Speicherkonto Archivierung oder Senden an Protokollanalyse beim Erstellen eine Ressource streaming.

Die Methode zum Aktivieren von Diagnoseprotokollen Ressourcenmanager Vorlage hängt der Ressource.

- **Nicht -** Serverressourcen (z. B. Netzwerk-Sicherheitsgruppen Logik Apps Automatisierung) verwenden [Diagnoseeinstellungen in diesem Artikel beschrieben](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Berechnen** Ressourcen (Bündel/JUNGE basierenden) verwenden [Bündel-JUNGE Konfigurationsdatei in diesem Artikel beschrieben](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

In diesem Artikel beschreiben wir mit einer Diagnose konfigurieren.

Die Schritte lauten:

1. Erstellen Sie eine Vorlage als JSON-Datei, die beschreibt, wie die Ressource und Diagnose aktivieren.
2. [Bereitstellen Bereitstellung mit Vorlage](../resource-group-template-deploy.md).

Im folgenden geben wir ein Beispiel der JSON-Vorlagendatei nicht Compute und Compute-Ressourcen generieren müssen.

## <a name="non-compute-resource-template"></a>Ressourcenvorlage nicht Compute
Für nicht-Serverressourcen müssen Sie zwei Dinge tun:

1. Fügen Sie Parameter Parameter Blob für die Storage-Kontonamen, Service Bus Regel-ID und OMS-Protokollanalyse Arbeitsplatz-ID (Archivierung Diagnoseprotokolle in ein Speicherkonto streaming-Protokolle Ereignis Hubs und Senden von Protokollen an Protokollanalyse aktivieren hinzu).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Fügen Sie im Array Ressourcen für die Diagnoseprotokolle soll Ressource eine Ressource der Art `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Eigenschaften-Blob für die Diagnose folgt [das Format in diesem Artikel beschrieben](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Hier ist ein vollständiges Beispiel, das eine Netzwerk-Sicherheitsgruppe erstellt und aktiviert auf Ereignis Hubs und Speicher in ein Speicherkonto.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Ressourcenvorlage berechnen
Zum Aktivieren der Diagnose für Compute-Ressourcen müssen beispielsweise ein Cluster virtuellen oder Fabric Service:

1. Die Definition der VM-Ressource die Azure-Diagnose-Erweiterung hinzufügen.
2. Storage-Konto und/oder Hub als Parameter angeben.
3. Fügen Sie der Inhalt der WADCfg XML-Datei in die XMLCfg-Eigenschaft alle XML-Zeichen richtig Escapezeichen hinzu

> [AZURE.WARNING] Der letzte Schritt kann wenig schwierig sein. [Finden Sie in diesem Artikel](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) ein Beispiel für die Variablen des Konfigurationsschemas Diagnose aufgeteilt, die mit Escapezeichen versehen und korrekt formatiert sind.

Der gesamte Prozess, einschließlich Beispielen wird beschrieben [in diesem Dokument](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen über Azure Diagnoseprotokolle](./monitoring-overview-of-diagnostic-logs.md)
- [Stream Azure Diagnoseprotokolle an Ereignis Hubs](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
