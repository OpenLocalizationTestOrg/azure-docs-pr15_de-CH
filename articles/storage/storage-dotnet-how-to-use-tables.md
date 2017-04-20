<properties
    pageTitle="Erste Schritte mit Azure Table Storage mit .NET | Microsoft Azure"
    description="Strukturierte Daten in der Cloud Azure-Tabellenspeicher, einem NoSQL-Datenspeicher gespeichert."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Erste Schritte mit Azure Table Storage mit .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Azure Table Storage ist ein Dienst, der strukturierte NoSQL-Daten in der Cloud gespeichert. Tabellenspeicher ist ein Key-Attribut mit einer schemalos. Da Tabellenspeicher schemalos ist, ist es einfach Ihre Daten müssen Ihre Anwendung entwickeln. Zugriff auf Daten ist schnell und kostengünstig für alle Anwendungstypen. Tabellenspeicher ist normalerweise deutlich Kosten als herkömmliche SQL für ähnliche Datenmengen.

Table Storage können Sie flexible Datasets wie Benutzerdaten für Web Applications, Adressbücher, Informationen und eine andere Art von Metadaten, die der Dienst benötigt speichern. Sie können eine beliebige Anzahl von Entitäten in einer Tabelle speichern und ein Speicherkonto kann eine beliebige Anzahl von Tabellen, bis die Kapazitätsgrenze des Speicherkontos enthalten.

### <a name="about-this-tutorial"></a>Zu diesem Lernprogramm

Dieses Lernprogramm zeigt .NET Programmieren für einige Szenarien Azure-Tabellenspeicher, einschließlich erstellen und Löschen einer Tabelle einfügen, aktualisieren, löschen und Abfragen von Daten verwenden.

**Geschätzte Dauer:** 45 Minuten

**Dabei:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure-Speicher-Clientbibliothek für .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Ein [Azure Storage-Konto](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Weitere Beispiele

Weitere Beispiele mit Tabellenspeicher finden Sie [Einstieg in Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Sie können die beispielanwendung herunterladen und ausführen oder Durchsuchen Sie den Code auf GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Namespacedeklarationen hinzuzufügen

Fügen Sie den folgenden `using` Aussagen am oberen Rand der `program.cs` Datei:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Erstellen des Tabelle Service-Clients

Die **CloudTableClient** -Klasse können Sie Tabellen und Tabellenspeicher gespeicherten Elemente abrufen. Hier ist eine Möglichkeit, den Webdienstclient erstellen:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Jetzt können Sie Code schreiben, die Daten liest und schreibt Daten in Tabellenspeicher.

## <a name="create-a-table"></a>Erstellen einer Tabelle

Dieses Beispiel zeigt, wie eine Tabelle erstellen, wenn es nicht bereits vorhanden ist:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Entitäten zuordnen C\# von **TableEntity**abgeleiteten Objekte mithilfe einer benutzerdefinierten Klasse. Um eine Entität zu einer Tabelle hinzuzufügen, erstellen Sie eine Klasse, die Eigenschaften der Entität definiert. Der folgende Code definiert eine Entitätsklasse, die Vorname des Kunden als Zeilenschlüssel und Nachnamen als Partitionsschlüssel verwendet. Zusammen eindeutig eine Entität Partition und Zeilenschlüssel Entität in der Tabelle. Entitäten mit der gleichen Partitionsschlüssel schneller als mit anderen Partition abgefragt werden können, aber unterschiedlichen Partitionsschlüssel ermöglicht größere Skalierbarkeit von parallelen Vorgängen.  Die Eigenschaft muss für jede Eigenschaft, die in dem Dienst gespeichert werden soll, eine öffentliche Eigenschaft eines unterstützten Typs, die beide verfügbar macht, `get` und `set`.
Auch die Entität Typ *muss* verfügbar machen einen parameterlosen Konstruktor.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Tabellenoperationen mit Entitäten erfolgt über das **CloudTable** -Objekt, die weiter oben im Abschnitt "Tabelle erstellen" erstellt. Die auszuführende Operation wird durch ein **TableOperation** -Objekt dargestellt.  Im folgenden Codebeispiel wird die Erstellung des **CloudTable** und dann ein **CustomerEntity** -Objekt.  Vorbereitung die Operation wird ein **TableOperation** -Objekt erstellt, um die Kundenentität in die Tabelle einfügen.  Schließlich wird der Vorgang durch Aufrufen von **CloudTable.Execute**ausgeführt.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Fügen Sie mehrerer Elemente ein

Sie können einen Stapel von Entitäten in einer Tabelle in einem Schreibvorgang einfügen. Einige andere Hinweise auf Stapel:

-  Updates können gelöscht und in der gleichen Partie Vorgang eingefügt.
-  Einem einzigen Arbeitsgang kann bis zu 100 Elemente enthalten.
-  Alle Entitäten in einem einzigen Arbeitsgang muss denselben Partitionsschlüssel.
-  Während eine Abfrage eines Batchvorgangs ausführen kann, muss der einzige Vorgang im Batch.

<!-- -->
Im folgenden Codebeispiel zwei Entitätsobjekten erstellt und jeweils der **TableBatchOperation** mithilfe der **Insert** -Methode hinzugefügt. Dann wird **CloudTable.Execute** aufgerufen, um den Vorgang auszuführen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Rufen Sie aller Entitäten in einer Partition ab

Verwenden Sie zum Abfragen einer Tabelle für alle Entitäten in einer Partition ein **TableQuery** -Objekt.
Das folgende Codebeispiel gibt einen Filter für die Entitäten "Smith" der Partitionsschlüssel ist. Dieses Beispiel druckt die Felder jeder Entität in den Abfrageergebnissen in der Konsole.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Einen Bereich von Elementen in einer partition

Möchten Sie alle Elemente in einer Partition Abfragen, können Sie einen Bereich angeben, durch die Kombination der Partition Schlüsselfilter mit einem Schlüssel Zeilenfilter. Im folgenden Codebeispiel verwendet zwei Filter zu allen Entitäten Partition "Smith", wo der Zeilenschlüssel (Vorname) beginnt mit einem Buchstaben "E" im Alphabet vor und gibt die Ergebnisse der Abfrage.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Abrufen einer Entität

Sie können eine Abfrage zum Abrufen einer bestimmten Entität schreiben. Der folgende Code verwendet die **TableOperation** an den Kunden "Ben Smith.
Diese Methode gibt nur eine Entität statt einer Auflistung und den zurückgegebenen Wert in **TableResult.Result** ist ein **CustomerEntity** -Objekt.
Tasten Partition und Zeilen in einer Abfrage angeben ist die schnellste Möglichkeit, eine einzelne Entität aus der Tabelle Service abrufen.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Ein Element ersetzen

Zum Aktualisieren einer Entität aus der Tabelle Service abrufen, das Entitätsobjekt ändern und dann die Änderungen an den Dienst. Der folgende Code ändert einen bestehenden Kunden Telefonnummer. Dieser Code verwendet anstatt **Einfügen** **Ersetzen**. Dadurch wird die Entität auf dem Server vollständig ersetzt werden, wenn die Entität auf dem Server geändert wurde, nachdem sie abgerufen wurden, in diesem Fall schlägt der Vorgang fehl.  Dieser Fehler wird der Anwendung versehentlich überschrieben Änderung zwischen dem Abrufen und Aktualisieren von einer anderen Komponente der Anwendung verhindern.  Handhabung dieses Fehlers ist die Entität abrufen, ändern (sofern noch gültig) und führen Sie dann einen anderen Vorgang **Ersetzen** .  Im nächste Abschnitt zeigen Ihnen, wie dieses Verhalten überschrieben.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Einfügen-oder-Ersetzen einer Entität

**Ersetzen** Vorgänge schlagen fehl, wenn die Entität geändert wurde, nachdem sie vom Server abgerufen wurden.  Darüber hinaus müssen Sie die Entität vom Server zuerst im Arbeitsgang **Ersetzen** erfolgreich abrufen.
Manchmal jedoch unklar Sie, ob die Entität auf dem Server vorhanden ist, deren aktuellen Werte gespeichert. Das Update sollte alle überschrieben.  Dazu verwenden Sie einen **InsertOrReplace** Vorgang.  Dieser Vorgang fügt die Entität ist nicht vorhanden, oder andernfalls unabhängig davon, wann die letzte Aktualisierung wurde ersetzt.  Im folgenden Codebeispiel wird die Kundenentität Ben Smith noch abgerufen, jedoch wird dann an den Server über **InsertOrReplace**.  Zwischen dem Abrufen und Aktualisieren der Entität Aktualisierungen werden überschrieben.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Abfrage einer Teilmenge von Entitätseigenschaften

Eine Tabellenabfrage kann nur einige Eigenschaften einer Entität statt der Entitätseigenschaften abrufen. Diese Technik namens Projektion Bandbreite reduziert und abfrageleistung, insbesondere bei großen Unternehmen verbessern kann. Die Abfrage im folgenden Code gibt nur die e-Mail-Adressen von Entitäten in der Tabelle. Dies erfolgt mithilfe einer Abfrage des **DynamicTableEntity** und **EntityResolver**. Sie erfahren mehr über Projektion auf [Einführung Upsert und Abfrageprojektion Blog veröffentlichen][]. Beachten Sie, dass die Projektion auf lokalen Speicheremulator nicht unterstützt wird, damit dieser Code ausgeführt wird, nur, wenn Sie ein Konto für den Dienst verwenden.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Löschen einer Entität

Nachdem Sie es mithilfe des gleichen Musters zum Aktualisieren einer Entität abgerufen haben, können Sie problemlos eine Entität löschen.  Der folgende Code ruft und löscht eine Customer-Entität.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Löschen einer Tabelle

Schließlich löscht im folgenden Codebeispiel wird eine Tabelle in ein Speicherkonto. Eine gelöschte Tabelle verloren für einen Zeitraum nach dem Löschen neu erstellt werden.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Elemente auf Seiten asynchron abrufen

Wenn Sie eine große Anzahl von Entitäten lesen und anzuzeigenden Prozess/Entitäten warten alle zurückzugeben, können Sie Elemente mit einer segmentierten Abfrage abrufen, anstatt sie abgerufen werden soll. Dieses Beispiel veranschaulicht die Ergebnisse in Seiten mit dem Muster Async erwarten, damit die Ausführung nicht blockiert, während eine große Menge von Ergebnissen warten zurück. Weitere Informationen zur Verwendung des asynchrone Await-Musters in .NET finden Sie unter [asynchrone Programmierung mit Async und Await (C# und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Tabellenspeicher bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren:

- Finden Sie weitere Tabelle Speicher in [Azure Table Storage in .NET starten](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Ansicht der Tabelle servicedokumentation Weitere Informationen zu verfügbaren APIs:
    - [Speicher-Clientbibliothek für .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)
- Erfahren Sie mehr zur Vereinfachung des Codes schreiben, mit Azure Storage mit [Azure Webaufträge SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Zeigen Sie weitere Features Leitfäden zusätzliche Optionen zum Speichern von Daten in Azure an
    - [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) unstrukturierte Daten speichern.
    - Mit relationalen Datenspeicher [mit SQL-Datenbank mit .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) .

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Einführung in Upsert und Abfrageprojektion Blogbeitrag]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
