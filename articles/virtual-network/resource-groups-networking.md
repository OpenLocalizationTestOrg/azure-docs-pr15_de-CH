<properties
   pageTitle="Ressource-Anbieter (Übersicht) | Microsoft Azure"
   description="Erfahren Sie mehr über die neue Ressource Netzwerkanbieter in Azure-Ressourcen-Manager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Anbieter von Netzwerk-Ressourcen
Eine Grundlage im heutigen geschäftlichen Erfolg muss die Fähigkeit zum Erstellen und Verwalten von umfangreichen Netzwerk bewusst Programme agile, flexible, sichere und wiederholbare Weise. Azure Resource Manager (ARM) können Sie diese Anwendung als eine einzelne Sammlung von Ressourcen in Ressourcengruppen erstellen. Diese Ressourcen werden durch verschiedene Ressourcenanbieter unter ARM verwaltet.

Azure Ressourcen-Manager basiert auf verschiedenen Ressourcenprovider für den Zugriff auf Ihre Ressourcen. Gibt es drei wichtigste Ressourcenanbieter: Netzwerk, Speicher und Compute. Dieses Dokument beschreibt die Merkmale und Vorteile des Ressourcenanbieters Netzwerk einschließlich:

- **Metadaten** -Tags mit Informationen hinzufügen. Diese Tags können Ressourcen in Ressourcengruppen und Abonnements Nachverfolgen verwendet werden.
- **Mehr Kontrolle über Ihr Netzwerk** - Netzwerk, Ressourcen lose, können sie genauere Weise steuern. Dies bedeutet, dass Sie mehr Flexibilität bei der Verwaltung der Netzwerkressourcen.
- **Schnellere Konfiguration** – da lose Netzwerkressourcen erstellen und Netzwerkressourcen parallel koordinieren. Konfiguration hat sich drastisch verringert.
- **Rolle Based Access Control** - RBAC bietet Standardrollen mit bestimmten Sicherheitsbereich neben die Erstellung von benutzerdefinierten Rollen für eine sichere Verwaltung.
- **Vereinfachung des Managements und Bereitstellung** – es ist einfacher, bereitstellen und Verwalten von Applikationen seit einen gesamten Anwendungsstapel als eine einzelne Sammlung von Ressourcen in einer Ressourcengruppe erstellen können. Und da Sie bereitstellen können, indem Sie einfach eine Vorlage JSON-Nutzlast bereitstellen.
- **Schnelle Anpassung** – können Sie deklarative Stilvorlagen um wiederholbare und schnelle Anpassung der Bereitstellung zu aktivieren.
- **Wiederholbare Anpassung** – können Sie deklarative Stilvorlagen um wiederholbare und schnelle Anpassung der Bereitstellung zu aktivieren.
- **Verwaltungsschnittstellen** - können Sie eine der folgenden Schnittstellen der Ressourcen:
    - REST-basierten API
    - PowerShell
    - .NET SDK
    - Node.JS-SDK
    - Java SDK
    - Azure CLI
    - Vorschauportal
    - ARM-Vorlagensprache

## <a name="network-resources"></a>Netzwerkressourcen
Sie können Netzwerkressourcen jetzt unabhängig verwalten, anstatt sie mithilfe einer einzelnen Compute-Ressource (VM) verwaltet. Dies gewährleistet ein höheres Maß an Flexibilität und Agilität in einer komplexen und Infrastruktur in einer Ressourcengruppe erstellen.

Nachfolgend finden Sie eine konzeptuelle Ansicht einer Beispiel-Bereitstellung eine mehrschichtige Anwendung. Jede Ressource sehen Sie, wie Netzwerkkarten, öffentliche IP-Adressen und VMs kann unabhängig voneinander verwaltet werden.

![Netzwerk-Ressourcenmodell](./media/resource-groups-networking/Figure2.png)

Jede Ressource enthält einen gemeinsamen Satz von Eigenschaften und ihre einzelnen Eigenschaften. Allgemeinen Eigenschaften sind:

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**Name**|Eindeutige Ressourcenname. Jeder Ressourcentyp verfügt über einen eigenen naming Restrictions.|PIP01, VM01, NIC01|
|**Speicherort**|Azure-Region, in dem sich die Ressource befindet.|Westus eastus|
|**ID**|Eindeutige URI basiert|/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Überprüfen Sie die einzelnen Eigenschaften von Ressourcen in den folgenden Abschnitten.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Verwaltungsschnittstellen
Sie können Ihre Azure Netzwerkressourcen über verschiedene Schnittstellen verwalten. In diesem Dokument wir konzentrieren uns auf zwei Schnittstellen: REST-API und Vorlagen.

### <a name="rest-api"></a>REST-API
Wie bereits erwähnt, können über verschiedene Schnittstellen, darunter REST API .NET SDK Node.JS SDK Java SDK PowerShell CLI Azure-Portal und Vorlagen Netzwerkressourcen verwaltet werden.

Die Rest-API entsprechen der Spezifikation HTTP 1.1-Protokoll. Die allgemeine Struktur der URI der API ist unten dargestellt:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

Und die Parameter in Klammern die folgenden Elemente:

- **Abonnement-Id** - Mitgliedsnamen Azure-Abonnement.
- **Ressource Anbieternamespace** - Namespace für den Datenanbieter verwendet wird. Der Wert für die Ressource Netzwerkanbieter ist *Microsoft.Network*.
- **Name der Region** - Azure Bereichsname

Die folgenden HTTP-Methoden werden unterstützt, wenn die REST-API aufrufen:

- **Einfügen** - Ressourcen eines bestimmten Typs erstellt eine Ressourceneigenschaft zu ändern oder eine Zuordnung zwischen Ressourcen ändern.
- **GET** - Informationen für eine bereitgestellte Ressource abgerufen.
- **Löschen** : Löschen Sie eine vorhandene Ressource verwendet.

Die Anforderung und Antwort entsprechen ein JSON-Nutzlast Format. Weitere Informationen finden Sie unter [Azure Resource Management-APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>ARM-Vorlagensprache
Neben Ressourcen imperativ (über APIs oder SDK), können Sie auch einen deklarativen Programmierstil erstellen und Verwalten von Netzwerkressourcen mithilfe von ARM-Vorlagensprache.

Nachfolgend finden Sie eine Beispiel-Darstellung einer Vorlage –

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Die Vorlage ist in erster Linie eine JSON Beschreibung der Ressourcen und die Instanzwerte über Parameter eingefügt. Im folgenden Beispiel lässt sich ein virtuelles Netzwerk mit 2 Subnetze erstellen.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Sie können Parameterwerte manuell bereitstellen, wenn Sie eine Vorlage verwenden, oder eine Datei verwenden. Das folgende Beispiel zeigt mögliche Parameterwerte mit obengenannter Vorlage:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Die wichtigsten Vorteile der Verwendung von Vorlagen sind:

- Sie können eine komplexe Infrastruktur in einer Ressourcengruppe in einem deklarativen Stil erstellen. Die Orchestrierung erstellen Ressourcen einschließlich Abhängigkeit Management erfolgt über ARM.
- Die Infrastruktur kann wiederholbare Weise in verschiedenen Regionen und innerhalb eines Bereichs erstellt werden durch Parameter geändert.
- Die deklarative Methode führt zu kürzen Lieferzeit Vorlagen und Bereitstellung der Infrastruktur.

Beispielvorlagen finden Sie unter [Azure Schnellstart Vorlagen](https://github.com/Azure/azure-quickstart-templates).

Finden Sie weitere Informationen auf die ARM-Vorlagensprache [Azure Ressourcenmanager Vorlagensprache](../resource-group-authoring-templates.md).

Virtuelles Netzwerk und Subnetzressourcen verwendet oben Vorlage. Es gibt andere Netzwerkressourcen, wie folgt verwenden können:

### <a name="using-a-template"></a>Mithilfe einer Vorlage

Sie können Dienste in Azure aus einer Vorlage bereitstellen, PowerShell, AzureCLI, oder klicken Sie zur Bereitstellung von GitHub durchführen. Zum Bereitstellen von Diensten aus einer Vorlage in GitHub führen Sie die folgenden Schritte aus:

1. Öffnen Sie die Datei template3 von GitHub. So öffnen Sie [virtuelle Netzwerk mit zwei Subnetzen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Klicken Sie auf **in Azure bereitstellen**und dann anmelden Azure-Portal mit Ihren Anmeldeinformationen.
3. Überprüfen Sie die Vorlage und klicken Sie dann auf **Speichern**.
4. Klicken Sie auf **Parameter bearbeiten** , und wählen Sie einen Speicherort für die Vnet und Subnetze wie *Westen der USA*.
5. Gegebenenfalls ändern Sie die Parameter **ADDRESSPREFIX** und **SUBNETPREFIX** , und klicken Sie auf **OK**.
6. **Wählen Sie** auf, und klicken Sie auf die Ressourcengruppe Vnet und Subnetze hinzufügen möchten. Alternativ können Sie eine neue Ressourcengruppe erstellen oder **neu erstellen**.
3. Klicken Sie auf **Erstellen**. Beachten Sie die Kachel **Bereitstellen Bereitstellung**anzeigen. Nach Abschluss die Bereitstellung wird einen Bildschirm ähnlich unten angezeigt werden.

![Bereitstellung von Beispiel](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Nächste Schritte

[Azure Ressourcenmanager Vorlagensprache](../resource-group-authoring-templates.md)

[Azure-Netzwerken – häufig verwendete Vorlagen](https://github.com/Azure/azure-quickstart-templates)

[Ressourcenanbieter berechnen](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md)
