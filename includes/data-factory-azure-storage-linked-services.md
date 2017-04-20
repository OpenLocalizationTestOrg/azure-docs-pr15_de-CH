## <a name="azure-storage-linked-service"></a>Azure verknüpfter Dienst

**Verknüpfter Azure Service** ermöglicht Azure Speicherkonto ein Azure Data Factory mit **kontoschlüssel**verknüpfen. Dadurch Daten Factory globalen Zugriff auf den Azure-Speicher. Die folgende Tabelle beschreibt für JSON-Elemente für verknüpfte Azure Storage Service.

| Eigenschaft | Beschreibung | Erforderlich |
| :-------- | :----------- | :-------- |
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **AzureStorage** | Ja |
| connectionString | Geben Sie Informationen zum Azure-Speicher für die ConnectionString-Eigenschaft herstellen. | Ja |

Finden folgende Schritte zur Ansicht/kopieren kontoschlüssel für Azure Storage: [anzeigen, kopieren und Regenerieren Speicher Zugriffstasten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Beispiel:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure-Speicher Sas verknüpft Service  
SAS (SAS) bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Dies bedeutet, dass ein Client für einen bestimmten Zeitraum mit einer bestimmten Gruppe von Berechtigungen, Berechtigungen für Objekte in Ihrem Speicherkonto begrenzt, ohne Ihr Konto Zugriffstasten gemeinsam gewähren können. SAS ist ein URI, der die Abfrageparameter umfasst alle notwendigen Informationen für authentifizierten Zugriff auf eine Speicherressource. Zugriff auf Speicherressourcen mit SAS muss der Client nur SAS an den entsprechenden Konstruktor oder die Methode übergeben. Weitere Informationen über SAS [Shared Access Signatures: verstehen SAS-Modell](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Azure Storage SAS verknüpft-Dienst können Sie ein Speicherkonto Azure mithilfe einer freigegebenen Access Signatur (SAS) mit einer Azure Data Factory verknüpfen. Dadurch Daten Factory eingeschränkt/zeitgebundene Zugriff auf alle-spezifische Ressourcen (blobcontainer) im Speicher. Die folgende Tabelle beschreibt für JSON-Elemente für Azure Storage SAS verknüpft Service. 

| Eigenschaft | Beschreibung | Erforderlich |
| :-------- | :----------- | :-------- |
| Typ | Die Type-Eigenschaft muss auf festgelegt sein: **AzureStorageSas**  | Ja |
| sasUri | Shared Access Signatur URI Azure Speicherressourcen wie BLOB-Container oder Tabelle angeben. Siehe Hinweise unten für Details. | Ja | 


**Beispiel:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Wenn ein **SAS-URI**, unter Berücksichtigung der Folgendes erstellen:  

- Azure Data Factory unterstützt nur **SAS Dienst**kein Konto SAS. Details zu diesen beiden Typen finden Sie unter [Typen der gemeinsame Zugriff auf Signaturen](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) .
- Entsprechende Lese-Schreib- **Berechtigungen** für Objekte basierend auf der Verwendung des verknüpften Diensts (Lesen, schreiben, Lesen/Schreiben) in Ihrem Unternehmen Daten festgelegt werden müssen.
- **Ablaufzeit** muss entsprechend eingestellt werden. Stellen Sie sicher, dass auf Azure Speicherobjekte nicht aktiven innerhalb der Pipeline abläuft.
- URI sollte am richtigen Container-Blob oder Tabellenebene muss erstellt werden. Ein SAS-Uri in Azure Blob kann der Dienst Data Factory auf diesem bestimmten Blob. Ein SAS-Uri Azure Blob-Container können Daten Factorydienst Blobs im Container durchlaufen. Benötigen Sie Zugriff auf mehr/weniger Objekte später oder SAS-URI aktualisieren, müssen Sie die verknüpften Serviceartikel mit neuen URI aktualisieren.   
