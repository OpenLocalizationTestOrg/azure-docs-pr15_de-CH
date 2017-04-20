<properties
    pageTitle="Ein Azure-CLI mit IoT Hub erstellen | Microsoft Azure"
    description="Folgendermaßen Sie erstellen einen IoT Hub Azure Befehlszeilenschnittstelle."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Erstellen Sie ein Azure-CLI mit IoT Hub

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Azure-Befehlszeilenschnittstelle können Sie erstellen und Verwalten von Azure IoT Hubs programmgesteuert. Dieser Artikel beschreibt, wie mit der Azure-CLI IoT Hub.

Um dieses Lernprogramm benötigen Sie Folgendes:

- Ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.
- [Azure CLI 0.10.4] [ lnk-CLI-install] oder höher. Wenn noch Azure-Befehlszeilenschnittstelle können Sie die aktuelle Version in der Befehlszeile mit dem folgenden Befehl überprüfen:
```
    azure --version
```

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Azure Resource Manager und Classic](../resource-manager-deployment-model.md). Azure-Ressourcen-Manager-Modus muss der Azure-CLI:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Legen Sie Ihre Azure-Konto und Ihr Abonnement 

1. Bei der Befehlszeile Anmeldung durch Eingabe des folgenden Befehls
```
    azure login
```
Verwenden Sie vorgeschlagene Web-Browser und Code zur Authentifizierung.

2. Haben mehrere Azure-Abonnements wird Azure mit Zugriff auf alle Ihre Anmeldeinformationen zugeordneten Azure Abonnements gewähren. Können der Azure-Abonnements sowie, der Standardwert ist mit dem Befehl
```
    azure account list 
```

Abonnement-Kontext festlegen unter dem auszuführenden die restlichen Befehle verwenden

```
    azure account set <subscription name>
```

3. Wenn Sie eine Ressourcengruppe keinen erstellen Sie eine benannte **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] Die Artikel [Azure-CLI zur Verwaltung Azure Ressourcen und Ressourcengruppen] [ lnk-CLI-arm] finden Sie weitere Informationen zur Verwendung von Azure CLI Azure Ressourcen verwalten. 


## <a name="create-an-iot-hub"></a>IoT Hub erstellen

Erforderliche Parameter:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Um alle Parameter anzuzeigen erstellt den Befehl Help in der Befehlszeile können Sie
```
    azure iothub create -h 
```
Beispiel:

 Erstellen einer IoT Hub in Ressource Gruppe **ExampleResourceGroup** führen Sie einfach den folgenden Befehl aufgerufen **exampleIoTHubName**
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Dieser Befehl Azure CLI erstellt einen S1 Standard IoT Hub für die Rechnung gestellt werden. Sie können IoT Hub **ExampleIoTHubName** mit folgendem Befehl löschen. 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Entwicklung für IoT Hub finden Sie hier:
- [IoT Hub SDKs][lnk-sdks]

Weitere Funktionen des IoT Hub kann, finden Sie unter:

- [Mithilfe von Azure-Portal IoT Hub verwalten][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
