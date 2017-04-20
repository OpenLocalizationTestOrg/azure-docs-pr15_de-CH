<properties
    pageTitle="Verwalten von anonymen Lesezugriff auf Container und Blobs | Microsoft Azure"
    description="Enthält Informationen zu Containern und Blobs für den anonymen Zugriff und wie Sie programmgesteuert zugreifen."
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

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Verwalten von anonymen Lesezugriff auf Container und blobs

## <a name="overview"></a>Übersicht

Standardmäßig kann nur der Besitzer des Speicherkontos Speicherressourcen in das Konto zugreifen. BLOB-Speicher können Sie einen Container Berechtigungen ermöglicht anonymen Lesezugriff auf den Container und die Blobs, damit Sie Zugriff auf Ressourcen gewähren können, ohne Ihr kontoschlüssel festlegen.

Anonymer Zugriff ist für Szenarien, in denen bestimmte Blobs anonymen Lesezugriff immer verfügbar. Feinere Kontrolle können Sie eine SAS erstellen eingeschränkt Stellvertretungszugriff mit verschiedenen Berechtigungen und ein bestimmtes Zeitintervall. Weitere Informationen zum Erstellen von SAS finden Sie unter [Verwendung von gemeinsamen Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Berechtigungen Sie anonyme Benutzer für Container und blobs

Standardmäßig können ein Container und alle Blobs darin nur vom Besitzer des Speicherkontos zugegriffen werden. Geben Sie anonyme Benutzern Leseberechtigungen für einen Container und dessen Blobs können Sie Berechtigungen für Container zum Zulassen öffentlichen Zugriffs festlegen. Anonyme Benutzer können ohne Authentifizierung der Anforderung Blobs in einem öffentlich zugänglichen Container lesen.

Container bieten die folgenden Optionen zur Verwaltung von Container:

- **Vollständige öffentliche Lesezugriff:** Container und BLOB-Daten können über anonyme Anforderung gelesen werden. Clients können Blobs im Container über anonyme Anforderung aufzählen, aber das Speicherkonto Container können nicht aufgelistet werden.

- **Public für Blobs nur Lesezugriff:** BLOB-Daten in diesem Container können über anonyme Anforderung gelesen werden, aber Containerdaten sind nicht verfügbar. Clients können nicht Blobs im Container über anonyme Anforderung aufgelistet werden.

- **Keine öffentliche Lesezugriff:** Container und BLOB-Daten können vom Kontoinhaber nur gelesen werden.

Sie können Berechtigungen für Container folgendermaßen festlegen:

- Aus dem [Azure-Portal](https://portal.azure.com).
- Programmgesteuert mithilfe der Speicher-Clientbibliothek oder die REST-API.
- Mithilfe von PowerShell. Zum Festlegen von Berechtigungen für Container Azure PowerShell finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](storage-powershell-guide-full.md#how-to-manage-azure-blobs).

### <a name="setting-container-permissions-from-the-azure-portal"></a>Azure-Portal festlegen Container Berechtigungen

Schritte zum Festlegen von Berechtigungen für Container aus dem [Azure-Portal](https://portal.azure.com):

1. Navigieren Sie zu dem Dashboard für das Speicherkonto.
2. Auswählen der Containername. Durch Klicken auf macht die Blobs im ausgewählten container
3. Wählen Sie **Richtlinien** auf der Symbolleiste.
4. Wählen Sie im Feld **Zugriffstyp** die gewünschte Berechtigungsstufe wie im folgenden Screenshot gezeigt aus.

    ![Dialogfeld Container Metadaten bearbeiten](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>Festlegen von Berechtigungen für Container programmgesteuert mit .NET

Zum Festlegen von Berechtigungen für einen Container mit .NET-Clientbibliothek zunächst rufen Sie vorhandene Berechtigungen für den Container durch Aufrufen der **GetPermissions** -Methode ab. Legen Sie die **PublicAccess** -Eigenschaft für das **BlobContainerPermissions** -Objekt, das von der **GetPermissions** -Methode zurückgegeben wird. Schließlich rufen Sie **SetPermissions** -Methode mit den aktualisierten Berechtigungen.

Im folgenden Beispiel wird der Container Berechtigungen zum vollständigen öffentlichen Zugriff. Um Berechtigungen für festgelegt öffentliche Lesezugriff für Blobs nur, **PublicAccess** -Eigenschaft auf **BlobContainerPublicAccessType.Blob**. Entfernen Sie alle Berechtigungen für anonyme Benutzer wird die Eigenschaft auf **BlobContainerPublicAccessType.Off**fest.

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>Container und Blobs anonym zugreifen

Ein Client, der Container und Blobs anonym zugreift, kann Konstruktoren verwenden, die keine Anmeldeinformationen erforderlich sind. Den folgenden Beispielen werden einiger Arten auf BLOB-Ressourcen anonym.

### <a name="create-an-anonymous-client-object"></a>Erstellen eines anonymen Client-Objekts

Ein neues Client-Dienstobjekt für den anonymen Zugriff können durch Bereitstellen des BLOB-Endpunkts für das Konto. Allerdings müssen Sie den Namen eines Containers in diesem Konto kennen, die für den anonymen Zugriff verfügbar ist.

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>Einen Container anonym verweisen

Haben Sie die URL in einen Container, der anonym verfügbar ist, können sie Sie direkt auf den Container verweisen.

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>Einen Blob anonym verweisen

Haben Sie die URL in ein Blob, das für den anonymen Zugriff verfügbar ist, können Sie direkt mit dem URL Blob verweisen:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>Funktionen für anonyme Benutzer

Die folgende Tabelle zeigt, welche Vorgänge bei der ACL des Containers Öffentliche Zugriff durch anonyme Benutzer aufgerufen werden können.

| REST-Vorgang                                         | Mit vollständiger öffentlicher Zugriff | Mit öffentlichen Lesezugriff für Blobs nur |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| Listencontainer                                        | Nur                              | Nur                                        |
| Container erstellen                                       | Nur                              | Nur                                        |
| Abrufen von Eigenschaften                               | Alle                                     | Nur                                        |
| Container-Metadaten erhalten                                 | Alle                                     | Nur                                        |
| Container-Metadaten                                 | Nur                              | Nur                                        |
| Container-ACL abrufen                                      | Nur                              | Nur                                        |
| Container-ACL festlegen                                      | Nur                              | Nur                                        |
| Container löschen                                       | Nur                              | Nur                                        |
| Liste Blobs                                             | Alle                                     | Nur                                        |
| BLOBs speichern                                               | Nur                              | Nur                                        |
| Abrufen von BLOBs                                               | Alle                                     | Alle                                               |
| BLOB-Eigenschaften                                    | Alle                                     | Alle                                               |
| BLOB-Eigenschaften festlegen                                    | Nur                              | Nur                                        |
| Abrufen von BLOB-Metadaten                                      | Alle                                     | Alle                                               |
| Blob-Metadaten                                      | Nur                              | Nur                                        |
| Block einfügen                                              | Nur                              | Nur                                        |
| Block Liste (Commit Blöcke)                 | Alle                                     | Alle                                               |
| Block Liste (nicht festgeschriebene Blöcke oder alle Blöcke) | Nur                              | Nur                                        |
| Blockieren Liste                                         | Nur                              | Nur                                        |
| Blob löschen                                            | Nur                              | Nur                                        |
| Kopieren des BLOBs                                              | Nur                              | Nur                                        |
| Snapshot-Blob                                          | Nur                              | Nur                                        |
| Leasing-Blob                                             | Nur                              | Nur                                        |
| Seite                                               | Nur                              | Nur                                        |
| Get Seitenbereiche                                        | Alle                                     | Alle                                                  |
| Blob Anfügen                                            | Nur                              | Nur                                                  |


## <a name="see-also"></a>Siehe auch

- [Authentifizierung für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [Mithilfe SAS (SAS)](storage-dotnet-shared-access-signature-part-1.md)
- [Delegieren des Zugriffs mit einem SAS](https://msdn.microsoft.com/library/azure/ee395415.aspx)
