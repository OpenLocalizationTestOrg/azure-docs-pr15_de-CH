<properties
    pageTitle="Ein IoT Hub mithilfe der REST-API erstellen | Microsoft Azure"
    description="Folgen Sie dieser Anleitung zum Einstieg in die REST-API IoT Hub zu erstellen."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Lernprogramm: Erstellen eines IoT Hubs mit einem C#-Programm und die REST-API

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

[IoT Hub Ressourcenanbieter REST API] verwenden[ lnk-rest-api] erstellen und Azure IoT Hubs programmgesteuert verwalten. In diesem Lernprogramm wird veranschaulicht, wie mit IoT Hub Ressourcenanbieter REST API IoT Hub aus einem C#-Programm.

> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Azure Resource Manager und Classic](../resource-manager-deployment-model.md).  Dieser Artikel behandelt das Bereitstellungsmodell Azure-Ressourcen-Manager verwenden.

Um dieses Lernprogramm benötigen Sie Folgendes:

- Microsoft Visual Studio 2015.
- Ein aktives Azure-Konto. <br/>Wenn Sie ein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in wenigen Minuten.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] oder höher.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Vorbereiten der Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein Visual C#-Windows-Projekt mit der Projektvorlage **Konsolenanwendungsprojekt** . Nennen Sie das Projekt **CreateIoTHubREST**.

2. Im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und klicken Sie die **NuGet-Pakete verwalten**.

3. In Nuget Paket-Manager **umfassen Vorabversion**überprüfen und **Microsoft.Azure.Management.ResourceManager**suchen. Klicken Sie auf **Installieren**, **Änderungen** klicken Sie auf **OK**und klicken Sie auf **annehmen** , akzeptieren die Lizenzen.

4. Suchen Sie in Nuget Paket-Manager nach **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Klicken Sie auf **Installieren**, **Änderungen** klicken Sie auf **OK**und klicken Sie auf **ich stimme** um die Lizenz zu akzeptieren.

6. Ersetzen Sie in Program.cs vorhandene **mithilfe von** Anweisungen durch Folgendes:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Fügen Sie in Program.cs die folgenden statischen Variablen Platzhalterwerte ersetzt. Sie notiert **ApplicationId**, **SubscriptionId** **TenantId**und **Kennwort** zuvor in diesem Lernprogramm. **Ressource Gruppenname** ist der Name der Ressourcengruppe, die Sie beim Erstellen des IoT Hubs verwenden, kann eine vorhandene Ressourcengruppe oder eine neue. **IoT Hub-Name** ist der Name des IoT Hubs, wie **MyIoTHub** erstellen (diese muss global eindeutig sein, Ihren Namen oder Initiale enthalten soll). **Deployment-Name** ist ein Name für die Bereitstellung wie **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Verwenden Sie die REST-API IoT Hub erstellen

[IoT Hub REST API] verwenden[ lnk-rest-api] IoT Hub in der Ressourcengruppe erstellen. Die REST-API können Sie einen vorhandenen IoT Hub ändern.

1. "Program.cs" die folgende Methode hinzugefügt:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Fügen Sie folgenden Code der **CreateIoTHub** -Methode zum Erstellen eines Objekts **HttpClient** mit Authentifizierungstoken in den Headern:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Fügen Sie den folgenden Code an die **CreateIoTHub** Methode beschreiben IoT Hub zu erstellen und eine JSON-Darstellung (die aktuelle Liste der Standorte, die IoT Hub unterstützen finden Sie unter [Azure Status][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Fügen Sie folgenden Code der Methode **CreateIoTHub** fordern Sie in Azure REST und das Überprüfen mit den Zustand des Ausbringungstasks überwacht URL abrufen:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Fügen Sie folgenden Code am Ende der Methode **CreateIoTHub** , die **AsyncStatusUri** Adresse im vorherigen Schritt abgerufen um zu warten, bis die Bereitstellung abgeschlossen:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Fügen Sie folgenden Code am Ende der **CreateIoTHub** -Methode zum Abrufen von Schlüsseln IoT Hub erstellt und auf der Konsole ausgegeben:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Führen Sie die Anwendung und führen

Sie können die Anwendung jetzt ausführen, durch Aufrufen der **CreateIoTHub** -Methode vor dem Erstellen und ausführen.

1. Fügen Sie folgenden Code am Ende der **Main** -Methode:

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Klicken Sie auf **Erstellen** und dann auf **Projektmappe erstellen**. Beheben Sie alle Fehler.

3. Klicken Sie auf **Debuggen** und dann **Debuggen starten** um die Anwendung auszuführen. Es kann mehrere Minuten für die Bereitstellung ausführen.

4. Sie können überprüfen, ob Ihre Anwendung neuen IoT Hub auf [Azure-Portal] hinzugefügt[ lnk-azure-portal] und Anzeigen der Liste von Ressourcen oder das PowerShell-Cmdlet **Get-AzureRmResource** .

> [AZURE.NOTE] Diese beispielanwendung Fügt einem S1 Standard IoT Hub für die Rechnung gestellt werden. Wenn Sie fertig sind, können Sie IoT Hub [Azure-Portal] [ lnk-azure-portal] oder **AzureRmResource entfernen** PowerShell-Cmdlet verwenden, wenn Sie fertig sind.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie einen IoT Hub mithilfe der REST-API bereitgestellt haben, sollten Sie weiter:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
