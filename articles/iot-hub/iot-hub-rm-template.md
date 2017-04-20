<properties
    pageTitle="Erstellen einer IoT Hub ARM-Vorlage als C# mit | Microsoft Azure"
    description="Dieses Lernprogramm Einstieg in Azure Resource Manager Vorlagen IoT Hub mit einem C#-Programm erstellen."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Erstellen Sie einen IoT Hub mit einem C#-Programm mit einer Azure-Ressourcen-Manager-Vorlage

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können Azure-Ressourcen-Manager erstellen und Verwalten von Azure IoT Hubs programmgesteuert. Dieses Lernprogramm veranschaulicht die Ressourcenmanager Azure Vorlage IoT Hub aus einem C#-Programm erstellen.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Azure Resource Manager und Classic](../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Bereitstellungsmodell Azure-Ressourcen-Manager verwenden.

Um dieses Lernprogramm benötigen Sie Folgendes:

- Microsoft Visual Studio 2015.
- Ein aktives Azure-Konto. <br/>Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.
- Ein [Azure Storage-Konto] [ lnk-storage-account] können Sie die Vorlagendateien Azure Ressourcenmanager speichern.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] oder höher.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Vorbereiten der Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein Visual C#-Windows-Projekt mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **CreateIoTHub**.

2. Im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie die **NuGet-Pakete verwalten**.

3. In Nuget Paket-Manager **umfassen Vorabversion**überprüfen und **Microsoft.Azure.Management.ResourceManager**suchen. Klicken Sie auf **Installieren**, **Änderungen** klicken Sie auf **OK**und klicken Sie auf **annehmen** , akzeptieren die Lizenzen.

4. Suchen Sie in Nuget Paket-Manager nach **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicken Sie auf **Installieren**, **Änderungen** klicken Sie auf **OK**und klicken Sie auf **ich stimme** um die Lizenz zu akzeptieren.

5. Ersetzen Sie in Program.cs vorhandene **mithilfe von** Anweisungen durch Folgendes:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Fügen Sie in Program.cs die folgenden statischen Variablen Platzhalterwerte ersetzt. Sie notiert **ApplicationId**, **SubscriptionId** **TenantId**und **Kennwort** zuvor in diesem Lernprogramm. **Kontoname der Azure-Speicher** ist der Name von Azure Storage-Konto die Vorlagendateien Azure Ressourcenmanager speichern. **Ressource Gruppenname** ist der Name der Ressourcengruppe, die Sie beim Erstellen der IoT Hub verwenden, kann eine vorhandene Ressourcengruppe oder eine neue. **Deployment-Name** ist ein Name für die Bereitstellung wie **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Senden einer Vorlage Ressourcenmanager Azure IoT Hub erstellen

Verwenden Sie eine JSON-Vorlage und Parameter IoT Hub in der Ressourcengruppe erstellen. Eine Azure-Ressourcen-Manager-Vorlage können Sie eine vorhandene IoT Hub ändern.

1. **Klicken**Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt und klicken Sie auf **Hinzufügen**. Fügen Sie eine JSON-Datei namens **template.json** zu Ihrem Projekt hinzu.

2. Ersetzen Sie **template.json** durch die folgenden Ressourcendefinition Region **USA OST** standard IoT Hub hinzufügen (die aktuelle Liste der Regionen, die IoT Hub unterstützen finden Sie unter [Azure Status][lnk-status]):

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

3. **Klicken**Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt und klicken Sie auf **Hinzufügen**. Fügen Sie eine JSON-Datei namens **parameters.json** zu Ihrem Projekt hinzu.

4. Ersetzen Sie **parameters.json** durch die folgenden Parameterinformationen, die einen Namen für die neue IoT Hub wie legt **{Ihre Initialen} Mynewiothub**. Beachten Sie, dass IoT Hub Name global eindeutig sein muss, damit Ihren Namen oder Initiale enthalten soll):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. Im **Server-Explorer**mit der Azure-Abonnement und in Azure Storage Konto Behälter **Vorlagen**erstellen. Legen Sie im Bedienfeld " **Eigenschaften** " der **öffentlichen** Lesezugriff für den Container **Vorlagen** auf **Blob**.

6. Im **Server-Explorer**mit der rechten Maustaste auf den Container **Vorlagen** und klicken Sie auf **Ansicht BLOB-Container**. Klicken Sie **Blob hochladen** , wählen Sie die beiden Dateien **parameters.json** und **templates.json**und klicken Sie auf **Öffnen** in den **Vorlagen** Container JSON-Dateien hochladen. Die URLs von Blobs mit JSON-Daten sind:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. "Program.cs" die folgende Methode hinzugefügt:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. **CreateIoTHub** -Methode, um die Vorlage und Parameter Dateien an Azure-Ressourcen-Manager fügen Sie den folgenden Code hinzu:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Die **CreateIoTHub** -Methode, die den Status und die Schlüssel für die neue IoT Hub zeigt fügen Sie den folgenden Code hinzu:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Führen Sie die Anwendung und führen

Sie können die Anwendung jetzt ausführen, durch Aufrufen der **CreateIoTHub** -Methode vor dem Erstellen und ausführen.

1. Fügen Sie folgenden Code am Ende der **Main** -Methode:

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Klicken Sie auf **Erstellen** und dann auf **Projektmappe erstellen**. Beheben Sie alle Fehler.

3. Klicken Sie auf **Debuggen** und dann **Debuggen starten** um die Anwendung auszuführen. Es kann mehrere Minuten für die Bereitstellung ausführen.

4. Sie können überprüfen, ob Ihre Anwendung neuen IoT Hub auf [Azure-Portal] hinzugefügt[ lnk-azure-portal] und Anzeigen der Liste von Ressourcen oder das PowerShell-Cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] Diese beispielanwendung Fügt einem S1 Standard IoT Hub für die Rechnung gestellt werden. IoT Hub [Azure-Portal] löschen[ lnk-azure-portal] oder **AzureRmResource entfernen** PowerShell-Cmdlet verwenden, wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen IoT Hub ein C#-Programm mit einer Azure-Ressourcen-Manager-Vorlage bereitgestellt haben, sollten Sie weiter:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
