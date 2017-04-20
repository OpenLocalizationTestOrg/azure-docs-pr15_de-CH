<properties
    pageTitle="Erstellen Sie eine mit einer Vorlage Ressourcenmanager Azure PowerShell IoT Hub | Microsoft Azure"
    description="Dieses Lernprogramm Einstieg in Azure Resource Manager Vorlagen IoT Hub mit PowerShell erstellt."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Erstellen Sie einen IoT Hub mit PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können Azure-Ressourcen-Manager erstellen und Verwalten von Azure IoT Hubs programmgesteuert. Dieses Lernprogramm veranschaulicht die Ressourcenmanager Azure Vorlage IoT Hub mit PowerShell erstellt.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Azure Resource Manager und Classic](../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Bereitstellungsmodell Azure-Ressourcen-Manager verwenden.

Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein aktives Azure-Konto. <br/>Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] oder höher.

> [AZURE.TIP] Artikel [Mithilfe von Azure PowerShell Azure Ressourcenmanager] [ lnk-powershell-arm] finden Sie weitere Informationen zur Verwendung von PowerShell-Skripts und Azure Ressourcenmanager Vorlagen Azure Ressourcen erstellen. 

## <a name="connect-to-your-azure-subscription"></a>Verbinden Sie mit der Azure-Abonnement

Geben Sie den folgenden Befehl der Azure-Abonnement anmelden in PowerShell-Befehlszeile:

```
Login-AzureRmAccount
```

Die folgenden Befehle können Sie ermitteln, wo Sie IoT Hub und die derzeit unterstützten API-Versionen bereitstellen können:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Erstellen einer Ressourcengruppe Ihr IoT Hub mit dem folgenden Befehl in einer der unterstützten Speicherorte für IoT Hub enthalten. Dieses Beispiel erstellt eine Ressourcengruppe namens **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Senden einer Vorlage Ressourcenmanager Azure IoT Hub erstellen

Verwenden einer Vorlage JSON IoT Hub in der Ressourcengruppe erstellen. Eine Azure-Ressourcen-Manager-Vorlage können Sie eine vorhandene IoT Hub ändern.

1. Verwenden Sie einen Text-Editor eine Azure-Ressourcen-Manager-Vorlage namens **template.json** mit folgender Ressourcendefinition einen neuen standard IoT Hub zu erstellen. Dieses Beispiel fügt IoT Hub in der Region **Ost USA** , zwei Verbrauchergruppen (**cg1** und **cg2**) Ereignis Hub-kompatiblen Endpunkt erstellt und verwendet **2016-02-03** -API-Version. Diese Vorlage erwartet IoT Hub-Namen als Parameter aufgerufen **HubName**übergeben. Die aktuelle Liste der Standorte, die IoT Hub unterstützen finden Sie unter [Azure Status][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Speichern Sie die Vorlagendatei Azure-Ressourcen-Manager auf dem lokalen Computer. Es wird angenommen, dass im Ordner **c:\templates**speichern.

3. Führen Sie den folgenden Befehl zum Bereitstellen des neuen IoT Hubs IoT Hub als Parameter übergeben. In diesem Beispiel ist der Name des IoT Hubs **Abcmyiothub** (Beachten Sie, dass dieser Name eindeutig sein muss, Ihren Namen oder Initiale enthalten soll):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Die Ausgabe zeigt die Schlüssel an IoT Hub erstellte.

5. Sie können überprüfen, ob Ihre Anwendung neuen IoT Hub auf [Azure-Portal] hinzugefügt[ lnk-azure-portal] und Anzeigen der Liste von Ressourcen oder das PowerShell-Cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] Diese beispielanwendung Fügt einem S1 Standard IoT Hub für die Rechnung gestellt werden. IoT Hub [Azure-Portal] löschen[ lnk-azure-portal] oder **AzureRmResource entfernen** PowerShell-Cmdlet verwenden, wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen IoT Hub PowerShell mit einer Azure-Ressourcen-Manager-Vorlage bereitgestellt haben, sollten Sie weiter:

- Informationen zu Funktionen des [IoT Hub Ressourcenanbieter REST API][lnk-rest-api].
- [Azure-Ressourcen-Manager] -Übersicht[ lnk-azure-rm-overview] Weitere Informationen zu den Funktionen von Azure-Ressourcen-Manager.

Erfahren Sie mehr über die Entwicklung für IoT Hub finden Sie hier:

- [Einführung in C# SDK][lnk-c-sdk]
- [IoT Hub SDKs][lnk-sdks]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Ein SDK Gateway IoT simulieren][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
