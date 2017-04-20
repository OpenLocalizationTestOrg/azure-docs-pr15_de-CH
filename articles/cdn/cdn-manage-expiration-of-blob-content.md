<properties
 pageTitle="Ablauf des Azure BLOB-Inhalt in Azure CDN verwalten | Microsoft Azure"
 description="Erfahren Sie mehr über die Optionen zum Steuern von Time to live für Blobs in Azure CDN Zwischenspeichern."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Verwalten Sie Ablauf des Azure BLOB-Inhalt in Azure CDN

> [AZURE.SELECTOR]
- [Azure Web Apps-Cloud Services, ASP.NET und IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure Blob-Speicherdienst](cdn-manage-expiration-of-blob-content.md)

[BLOB-Dienst](../storage/storage-introduction.md#blob-storage) in [Azure-Speicher](../storage/storage-introduction.md) ist eine mehrere Azure-basierte Ursachen Azure CDN integriert.  Öffentlich zugängliche BLOB-Inhalte kann in Azure CDN zwischengespeichert werden, bis zum Ablauf der Time-to-live (TTL).  Die TTL bestimmt [ *Cache-Control* -Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort von Azure-Speicher.

>[AZURE.TIP] Sie können auf ein Blob keine Gültigkeitsdauer festlegen.  In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen.
>
>Weitere Informationen zur Funktionsweise von Azure CDN beschleunigen des Zugriffs Blobs und andere Dateien finden Sie unter [Übersicht über Azure CDN](./cdn-overview.md).
>
>Weitere Informationen zu den Azure-Speicher BLOB-Dienst finden Sie unter [BLOB-Konzepte](https://msdn.microsoft.com/library/dd179376.aspx). 

Dieses Lernprogramm demonstriert verschiedene Arten die Gültigkeitsdauer für ein Blob in Azure-Speicher festlegen können.  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md) ist der schnellsten, leistungsstärksten Verfahren zur Verwaltung von Azure Services.  Verwenden der `Get-AzureStorageBlob` Cmdlet ein Verweis auf das Blob legen die `.ICloudBlob.Properties.CacheControl` Eigenschaft. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] PowerShell verwenden Sie [die CDN-Profile](./cdn-manage-powershell.md)und Endpunkte.

## <a name="azure-storage-client-library-for-net"></a>Azure-Speicher-Clientbibliothek für .NET

Um ein Blob TTL mit .NET festzulegen, verwenden Sie [Azure Storage-Clientbibliothek für .NET](../storage/storage-dotnet-how-to-use-blobs.md) [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) -Eigenschaft festgelegt.

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] Stehen viele weitere .NET Codebeispiele in [Azure BLOB-Speicher Beispiele für .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).

## <a name="other-methods"></a>Andere Methoden

- [Azure Befehlszeilenschnittstelle](../xplat-cli-install.md)

    Beim Hochladen des BLOBs legen *CacheControl* -Eigenschaft mit dem `-p` wechseln.  In diesem Beispiel wird die Gültigkeitsdauer eine Stunde (3600 Sekunden).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure-Speicherdienste REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Festlegen Sie bei [Blob platzieren](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)oder [BLOB-Eigenschaften](https://msdn.microsoft.com/library/azure/ee691966.aspx) die *X-ms-Blob-Cache-Control* -Eigenschaft explizit.

- Management-Tools von Drittanbietern

    Einige Drittanbieter-Azure Storage Management-Tools können Sie die *CacheControl* -Eigenschaft Blobs festgelegt. 

## <a name="testing-the-cache-control-header"></a>Testen den *Cache-Control* -header

Sie können problemlos die Gültigkeitsdauer der Blobs überprüfen.  Mithilfe des Browsers [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)testen Sie, ob das Blob Antwortheader *Cache-Control* einschließlich.  Ein Tool wie **Wget**, [Postbote](https://www.getpostman.com/)oder [Fiddler](http://www.telerik.com/fiddler) können Antwortheader untersuchen.

## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich über den *Cache-Control* -header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Verwalten Sie Ablaufdatum des Inhalts in Azure CDN Cloud-Dienst](./cdn-manage-expiration-of-cloud-service-content.md)

