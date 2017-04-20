<properties
    pageTitle="Erstellen Sie einen schreibgeschützten Snapshot eines Blob | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Momentaufnahme eines Blob BLOB-Daten zu einem angegebenen Zeitpunkt sichern. Verstehen Sie, wie Snapshots berechnet werden und wie sie Kapazitätskosten minimieren."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Einen BLOB-Snapshot erstellen

## <a name="overview"></a>Übersicht

Ein Snapshot ist eine schreibgeschützte Version eines Blob zu einem Zeitpunkt ausgeführt wird. Snapshots eignen sich zum Sichern von Blobs. Nachdem Sie einen Snapshot erstellen, lesen, kopieren oder löschen, aber nicht ändern.

Eine Momentaufnahme eines Blob entspricht der Basis-Blob, Blob URI hat einen **DateTime** -Wert angehängt Blob-URI, der den Zeitpunkt anzugeben, an dem die Momentaufnahme erstellt wurde. Angenommen, eine Seite BLOB-URI ist `http://storagesample.core.blob.windows.net/mydrives/myvhd`, URI dem ähnelt, Snapshot `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Alle Snapshots freigeben Basis-Blob-URI. Der einzige Unterschied zwischen dem Basis-Blob und die ist der angefügten **DateTime** -Wert.

Ein Blob kann eine beliebige Anzahl von Snapshots haben. Snapshots bleiben erhalten, bis sie explizit gelöscht werden. Ein Snapshot kann nicht die Basis-Blob Überleben. Sie können die Basis-Blob zum Nachverfolgen Ihrer aktuellen Snapshots zugeordnete Snapshots auflisten.

Beim Erstellen einer Momentaufnahme eines Blob werden das Blob Systemeigenschaften auf den Snapshot mit den gleichen Werten kopiert. Wenn Sie separate Metadaten für den Snapshot angeben, bei der Erstellung auch Basis-Blob Metadaten auf den Snapshot kopiert.

Basis-Blob zugeordnete Leases wirken sich nicht auf den Snapshot aus. Eine Lease für einen Snapshot kann nicht abgerufen werden.

## <a name="create-a-snapshot"></a>Erstellen eines Snapshots

Im folgenden Codebeispiel wird veranschaulicht, wie einen Snapshot in .NET erstellt. Dieses Beispiel gibt separate Metadaten für den Snapshot beim Erstellen.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Kopieren von snapshots

Mit Blobs und Snapshots Kopiervorgänge Regeln diese:

- Sie können einen Snapshot über die Basis-Blob. Durch einen Snapshot der Position des Basis-Blob heraufstufen, können Sie eine frühere Version eines Blob wiederherstellen. Snapshot bleibt aber der Basis Blob mit eine überschreibbare Kopie des Snapshots überschrieben.

- Sie können einen Snapshot auf ein Ziel-Blob mit einem anderen Namen. Das resultierende Ziel-Blob ist schreibbar Blob und keine Momentaufnahme.

- Wenn Quelle Blob kopiert wird, werden keine Snapshots des Quell-Blob nicht zum Ziel kopiert. Wenn ein Ziel-Blob mit einer Kopie überschrieben wird, bleiben alle Snapshots das ursprüngliche Ziel-Blob zugeordnet.

- Beim Erstellen eines Snapshot ein wird das Blob festgeschrieben Liste auch auf den Snapshot kopiert. Alle nicht festgeschriebenen Blöcke werden nicht kopiert.

## <a name="specify-an-access-condition"></a>Geben Sie eine zugriffsbedingung

Sie können eine Access-Bedingung, dass der Snapshot erstellt wird, nur, wenn eine Bedingung erfüllt ist. Um eine zugriffsbedingung anzugeben, verwenden Sie die **AccessCondition** -Eigenschaft. Wenn die angegebene Bedingung nicht erfüllt, ist keine Momentaufnahme und BLOB-Dienst gibt Statuscode HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Löschen von snapshots

Sie können ein Blob mit Snapshots löschen, wenn die Snapshots gelöscht werden. Sie können einzeln löschen oder angeben, dass alle Snapshots gelöscht werden, wenn das Quell-Blob gelöscht wird. Wenn Sie versuchen, einen Blob löschen, die noch Snapshots, tritt ein Fehler auf.

Im folgenden Codebeispiel wird veranschaulicht, wie ein Blob und der Momentaufnahmen in .NET löschen, in dem `blockBlob` wird eine Variable vom Typ **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Snapshots mit Azure Premium

Verwenden Snapshots mit Premium-Speicher diese Regeln:

- Die maximale Anzahl Snapshots pro Seitenblob in ein Speicherkonto Premium ist 100. Wenn diese Grenze überschritten wird, gibt Snapshot BLOB-Vorgang Fehlercode 409 (**SnapshotCountExceeded**).

- Sie können einen Snapshot eines Seitenblob ein Speicherkonto Premium 10 Minuten teilnehmen. Wenn diese Rate überschritten wird, gibt Snapshot BLOB-Vorgang Fehlercode 409 (**SnaphotOperationRateExceeded**).

- Get Blob einen Snapshot von einem Seitenblob in ein Speicherkonto Premium lesen kann nicht aufgerufen werden. Snapshot in ein Speicherkonto Premium Get Blob fordert zurückgegeben Fehlercode 400 (**InvalidOperation**). Jedoch können Sie Get BLOB-Eigenschaften und Get BLOB-Metadaten für eine Momentaufnahme ein Speicherkonto Premium aufrufen.

- Copy Blob-Vorgang können Sie folgendermaßen einen Snapshot einen Snapshot auf einer anderen Seitenblob im Konto kopieren. Ziel-Blob für den Kopiervorgang müssen keine vorhandenen Snapshots. Ziel-Blob Snapshots vorhanden, gibt Copy Blob-Vorgang Fehlercode 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Den absoluten URI auf einen Snapshot zurück

In diesem C#-Codebeispiel erstellt einen Snapshot und schreibt den absoluten URI für den primären Standort.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Verstehen Sie, wie Snapshots Gebühren anfallen

Erstellen einen Snapshot eine schreibgeschützte Kopie eines Blob ist führen Storage zusätzliche Kosten zu Ihrem Konto. Beim Entwurf einer Anwendung ist es wichtig zu beachten wie diese Gebühren anfallen können, dass unnötige Kosten minimieren können.

### <a name="important-billing-considerations"></a>Wichtig für Rechnungsadresse

Die folgende Liste enthält wichtige Punkte sollten beim Erstellen eines Schnappschusses.

- Sie Speicherkonto fallen Gebühren für eindeutige Blöcke oder Seiten, ob im Blob oder in der Momentaufnahme. Ihr Konto wird nicht fallen zusätzliche Gebühren für Snapshots Blob zugeordnet, bis Sie das Blob aktualisieren sie basieren. Nach der Aktualisierung von Basis-BLOBs weicht er aus seiner Snapshots. In diesem Fall werden Sie eindeutige Blöcke oder Seiten in jedem Blob oder Snapshot berechnet.

- Beim Ersetzen eines Blocks in ein erhoben Blocks anschließend als eindeutiger Block. Dies gilt selbst wenn der Block denselben Block-ID und dieselben Daten wie in der Momentaufnahme. Nach der Block übergeben wiederum weicht von der entsprechenden alle Snapshots und wird für die Daten berechnet. Gleiches gilt für eine Seite in einem Seiten-Blob mit identischen Daten aktualisiert wird.

- Ein durch Aufrufen der Methode **UploadFile**, **UploadText**, **UploadStream**oder **UploadByteArray** ersetzen ersetzt alle Blöcke im Blob. Haben Sie eine Momentaufnahme dieses Blob zugeordnet, alle Blöcke im Basis-Blob und Snapshot jetzt abweichen und wird für alle die beiden Blobs berechnet. Dies gilt auch, wenn die Daten im Basis-Blob und die Momentaufnahme identisch sind.

- Azure BLOB-Dienst muss nicht dazu bestimmt, ob zwei identische Daten enthalten. Jeder Block, die hochgeladen und Commit als eindeutig werden auch wenn dieselben Daten und dieselbe Block-ID Da eindeutige Blöcke Gebühren anfallen, ist wichtig, ein Blob mit einem Snapshot führt zusätzliche eindeutige Blöcke und zusätzliche Gebühren aktualisiert.

> [AZURE.NOTE] Bewährte diktieren, Verwalten von Snapshots sorgfältig, um zusätzliche Gebühren zu vermeiden. Wir empfehlen, Verwalten von Snapshots auf folgende Weise:

> - Löschen Sie und erneut erstellen Sie zugeordneten Snapshots Blob aktualisieren BLOBs, selbst wenn mit identischen Daten aktualisiert werden, wenn Anwendungsentwurf Snapshots verwalten muss. Löschen und Neuerstellen der Blob-Snapshots können Sie sicherstellen, dass BLOBs und Snapshots nicht abweichen.

> - Wenn Sie Snapshots für ein Blob verwalten, vermeiden Sie **UploadFile**, **UploadText**, **UploadStream**oder **UploadByteArray** um den Blob zu aktualisieren. Diese Methoden ersetzen alle die dem Blob, damit Ihre Basis-Blob und Snapshots deutlich voneinander abweichen. Aktualisieren Sie stattdessen die geringste Anzahl Blöcke mit den Methoden **PutBlock** und **PutBlockList** .


### <a name="snapshot-billing-scenarios"></a>Snapshot Abrechnung Szenarien


Die folgenden Szenarien zeigen, wie Gebühren für ein und die Snapshots anfallen.

In Szenario 1 hat base-Blob nicht aktualisiert wurde, nachdem die Momentaufnahme erstellt wurde, Gebühren für eindeutige Blöcke 1, 2 und 3.

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

In Szenario 2 base-Blob aktualisiert wurde, jedoch des Snapshots nicht. Block 3 wurde aktualisiert und, obwohl sie die gleichen Daten und dieselbe ID enthält, ist nicht gleich 3 im Snapshot blockieren. Dadurch wird das Konto vier belastet.

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

In Szenario 3 base-Blob aktualisiert wurde, jedoch des Snapshots nicht. Block 3 mit 4 im Basis-Blob ersetzt wurde, aber der Snapshot spiegelt noch Block 3. Dadurch wird das Konto vier belastet.

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

Base-Blob in Szenario 4 wurde vollständig überarbeitet und enthält keine der ursprünglichen Blöcke. Daher ist die Rechnung für alle acht eindeutige Blöcke. Dieses Szenario kann eintreten, verwenden eine Aktualisierungsmethode **UploadFile**, **UploadText**, **UploadFromStream**oder **UploadByteArray**, da diese Methoden den Inhalt eines Blob ersetzen.

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele für die Verwendung von BLOB-Speicher finden Sie unter [Azure Codebeispiele](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Sie können eine beispielanwendung herunterladen und ausführen oder Durchsuchen Sie den Code auf GitHub. 
