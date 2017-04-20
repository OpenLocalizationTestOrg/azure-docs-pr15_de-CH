<properties
   pageTitle="Ein Dienstcontainer Cloud mit PowerShell erstellen | Microsoft Azure"
   description="Erläutert, wie ein Dienstcontainer Cloud mit PowerShell erstellt. Der Container enthält Web-und Workerrollen."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Ein Azure PowerShell-Befehl einen leere Cloud Service-Container erstellen
Erläutert, wie mithilfe von Azure PowerShell-Cmdlets Clouddienste Container erstellen. Führen Sie die folgenden Schritte aus:

1. Installieren Sie das Microsoft Azure PowerShell-Cmdlet von [Azure PowerShell](http://aka.ms/webpi-azps) -Downloadseite.
2. Öffnen Sie die PowerShell-Befehlszeile.
3. Verwenden Sie [Add AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) anmelden.

    > [AZURE.NOTE] Weitere Anweisungen auf Azure PowerShell-Cmdlets installieren und eine Verbindung zu Ihr Azure-Abonnement finden Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

4. Verwenden Sie das Cmdlet **Neu AzureService** einen leere Azure Cloud Service-Container erstellen.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Führen Sie dieses Beispiel mit dem Cmdlet aufrufen:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Weitere Informationen zum Erstellen von Azure Cloud-Dienst ausführen:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Nächste Schritte

 * Zum Verwalten der Cloudbereitstellung-Dienst finden Sie die Befehle [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx) [Entfernen AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)und [AzureService festlegen](https://msdn.microsoft.com/library/azure/dn495242.aspx) . Sie können auch [Cloud-Dienste konfigurieren](cloud-services-how-to-configure.md) Informationen verweisen.

 * Um Ihre Cloud-Dienstprojekt auf Azure veröffentlichen, finden Sie im Beispiel **PublishCloudService.ps1** [kontinuierlichen Bereitstellung für Cloud-Dienst in Azure](cloud-services-dotnet-continuous-delivery.md).
