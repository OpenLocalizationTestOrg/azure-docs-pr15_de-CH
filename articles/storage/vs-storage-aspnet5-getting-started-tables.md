<properties
    pageTitle="Erste Schritte mit Tabelle Visual Studio verbunden Services (ASP.NET 5) | Microsoft Azure"
    description="Einstieg mit Azure Table Storage in einem ASP.NET 5-Projekt in Visual Studio nach dem Anschließen an ein Speicherkonto mit Visual Studio verbunden services"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Einstieg in Azure Table Storage und Visual Studio verbunden services

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt, wie die ersten Schritte mit Azure-Tabellenspeicher in Visual Studio nach erstellt oder Visual Studio **Verbunden Dienste hinzufügen** Dialogfeld Azure Speicherkonto in einem ASP.NET 5-Projekt verwiesen.

Azure Table Storage Service können Sie große Mengen von strukturierten Daten. Der Dienst ist ein NoSQL-Datenspeicher, der authentifizierte Aufrufe von innerhalb und außerhalb der Azure-Cloud akzeptiert. Azure-Tabellen eignen sich zum Speichern von strukturierten, nicht-relationalen Daten.

Operation **Verbundenen Dienste hinzufügen** installiert die entsprechenden NuGet-Pakete zum Azure-Speicher in Ihrem Projekt zugreifen und die Verbindungszeichenfolge für das Speicherkonto Projektdateien Konfiguration hinzugefügt.

Weitere allgemeine Informationen zur Verwendung von Azure Table Storage finden Sie unter [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md).

Zunächst müssen Sie eine Tabelle in das Speicherkonto erstellen. Wir zeigen Ihnen eine Azure-Tabelle im Code erstellen. Wir werden auch wie einfache Tabelle und Entität Operationen wie hinzufügen, ändern, lesen und Lesen Table-Elemente angezeigt. Die Beispiele sind in C# geschrieben\# code und Azure Storage-Clientbibliothek für .NET.

**Hinweis** : einige der APIs, die Azure-Speicher in ASP.NET 5 sind asynchrone Aufrufe ausführen. Weitere Informationen finden Sie unter [Asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Im folgenden Code wird angenommen, dass asynchrone Programmierung Methoden verwendet werden.

## <a name="access-tables-in-code"></a>Access-Tabellen in code

Um Tabellen in ASP.NET 5-Projekten müssen Sie alle C#-Quelldateien Folgendes, die Azure-Tabellenspeicher zugreifen.

1. Sicherstellen Sie, dass die Namespacedeklarationen am oberen Rand der C#-Datei diese **Verwendung** enthalten.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Abzurufen Sie ein **CloudStorageAccount** -Objekt, Ihre speicherkontoinformationen darstellt. Verwenden Sie den folgenden Code zu dem Ihre Verbindungszeichenfolge Speicher und Speicher-Kontoinformationen von Azure-Dienstkonfiguration.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Hinweis** : alle der oben aufgeführten Code vor dem Code in den folgenden Beispielen verwenden.

3. Rufen Sie ein **CloudTableClient** -Objekt auf die Tabellenobjekte in das Speicherkonto.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Erhalten Sie **CloudTable** Referenzobjekt auf einer bestimmten Tabelle und Entitäten.

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Erstellen einer Tabelle in code

Zum Erstellen der Azure Tabelle nur fügen Sie einen Aufruf von **CreateIfNotExistsAsync() hinzu**.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um eine Entität zu einer Tabelle hinzufügen, erstellen Sie eine Klasse, die Eigenschaften der Entität definiert. Der folgende Code definiert eine Entitätsklasse bezeichnet **CustomerEntity** , der Vorname des Kunden als Zeilenschlüssel und Nachnamen als Partitionsschlüssel verwendet.

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

Tabellenoperationen Entitäten werden mit **CloudTable** -Objekt Sie in "Access-Tabellen in Code". Das **TableOperation** -Objekt stellt die Operation ausgeführt werden. Im folgenden Codebeispiel wird veranschaulicht, wie ein **CloudTable** -Objekt und ein **CustomerEntity** -Objekt erstellen. Vorbereitung auf die Operation wird eine **TableOperation** erstellt die Kundenentität in die Tabelle einfügen. Schließlich wird der Vorgang durch Aufrufen von CloudTable.ExecuteAsync ausgeführt.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Fügen Sie mehrerer Elemente ein

Sie können mehrere Entitäten in einer Tabelle in einem einzelnen Schreibvorgang einfügen. Im folgenden Codebeispiel erstellt zwei Entitätsobjekten ("Jeff Smith" und "Ben Smith"), ein **TableBatchOperation** -Objekt mit der **Insert** -Methode hinzugefügt und der Vorgang durch Aufrufen von CloudTable.ExecuteBatchAsync beginnt.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Abrufen Sie aller Entitäten in einer partition
Verwenden Sie zum Abfragen einer Tabelle für alle Entitäten in einer Partition ein **TableQuery** -Objekt. Das folgende Codebeispiel gibt einen Filter für die Entitäten "Smith" der Partitionsschlüssel ist. Dieses Beispiel druckt die Felder jeder Entität in den Abfrageergebnissen in der Konsole.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a>Abrufen einer Entität
Sie können eine Abfrage zu einer bestimmten Entität erstellen. Der folgende Code verwendet ein **TableOperation** -Objekt an einen Kunden namens "Ben Smith. Diese Methode gibt nur ein Element, sondern eine Auflistung und der zurückgegebene Wert in **TableResult.Result** ist ein **CustomerEntity** -Objekt. Tasten Partition und Zeilen in einer Abfrage angeben ist die schnellste Möglichkeit, eine einzelne Entität aus der **Tabelle** Service abrufen.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Löschen einer Entität
Sie können eine Entität löschen, nach dem Sie suchen. Der folgende Code sucht eine Customer-Entität mit dem Namen "Ben Smith" und wenn es sie findet, löscht es.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
