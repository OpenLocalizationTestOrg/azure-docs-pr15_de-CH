<properties
    pageTitle="Verwendung von Java Tabellenspeicher | Microsoft Azure"
    description="Strukturierte Daten in der Cloud Azure-Tabellenspeicher, einem NoSQL-Datenspeicher gespeichert."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Wie Tabellenspeicher aus Java

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch wird wie allgemeine Szenarien mit Azure Table Storage Service angezeigt. Die Proben sind in Java geschrieben und [Azure Storage SDK für Java][]verwenden. Die fallen Szenarien **Erstellen**, **Auflisten**und **Löschen von** Tabellen sowie **Einfügen**, **Abfragen**, **Ändern**und **Löschen von** Entitäten in einer Tabelle. Weitere Informationen zu Tabellen finden Sie im Abschnitt [Weiter](#Next-Steps) .

Hinweis: Eine SDK ist für Entwickler, Azure-Speicher auf Android-Geräte verfügbar. Weitere Informationen finden Sie in [Azure Storage SDK für Android][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen Sie eine Java-Anwendung

Verwenden Sie in diesem Handbuch Speicherfunktionen in einer Java-Anwendung lokal oder Code in einer Webrolle oder Worker-Rolle in Azure ausgeführt werden können.

Hierzu müssen Sie Java Development Kit (JDK) installiert und Azure-Speicher in der Azure-Abonnement registrieren. Nachdem Sie dies getan haben, müssen Sie überprüfen, ob Entwicklungssystem erfüllt die Mindestanforderungen und Abhängigkeiten in [Azure Storage SDK für Java][] Repository auf GitHub aufgeführt sind. Wenn Ihr System die Anforderungen können Sie herunterladen und Installieren von Azure Storage Bibliotheken für Java auf Ihrem System aus Repository befolgen. Nach Abschluss dieser Aufgaben werden Sie in den Beispielen in diesem Artikel verwendet eine Java-Anwendung erstellen.

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurieren Sie Ihre Anwendung auf Tabellenspeicher

Fügen Sie folgenden Import-Anweisung am Anfang der Java-Datei mit Microsoft Azure-Speicher-APIs auf Tabellen zugegriffen werden soll:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und Anmeldeinformationen für den Zugriff auf Daten-Management-Services. Beim Ausführen in einer Clientanwendung müssen Sie Speicher Verbindungszeichenfolge im folgenden Format angeben, mit dem Namen Ihres Speicherkontos und der primäre Schlüssel für das Speicherkonto in [Azure-Portal](https://portal.azure.com) für *Kontoname* und *AccountKey* aufgeführt. Dieses Beispiel zeigt, wie Sie ein statisches Feld der Verbindungszeichenfolge zu deklarieren:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In einer Anwendung eine Rolle in Microsoft Azure diese Zeichenfolge in die Dienstkonfigurationsdatei *ServiceConfiguration.cscfg*gespeichert werden und mit einem Aufruf der Methode **RoleEnvironment.getConfigurationSettings** zugegriffen werden. Hier ist ein Beispiel für das Abrufen der Verbindungszeichenfolge aus einer **Einstellung** Element mit dem Namen *StorageConnectionString* in der Service-Konfigurationsdatei:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

In den folgenden Beispielen davon ausgehen, dass Sie eine dieser beiden Methoden zu Speicher-Verbindungszeichenfolge verwendet haben.

## <a name="how-to-create-a-table"></a>Gewusst wie: Erstellen einer Tabelle

Ein **CloudTableClient** -Objekt können Sie Referenzobjekte für Tabellen und Entitäten. Der folgende Code erstellt ein **CloudTableClient** -Objekt und erstellen ein neues **CloudTable** -Objekt repräsentiert eine Tabelle namens "Personen" verwendet. (Hinweis: Gibt es weitere Methoden zum Erstellen von **CloudStorageAccount** -Objekten; Weitere Informationen finden Sie unter **CloudStorageAccount** in [Azure Storage Client SDK-Referenz].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Gewusst wie: Auflisten der Tabellen

Um eine Liste der Tabellen abzurufen, rufen Sie die **CloudTableClient.listTables()** Methode eine iterativen Liste von Tabellennamen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Gewusst wie: hinzufügen eine Entität zu einer Tabelle

Java-Objekte mit einer benutzerdefinierten Klasse implementieren **TableEntity**Entitäten zuordnen. Zur Vereinfachung die **TableServiceEntity** Klasse implementiert **TableEntity** und Reflektion Getter und Setter-Methoden der Eigenschaften namens Eigenschaften zuordnen. Um eine Entität zu einer Tabelle hinzuzufügen, erstellen Sie zuerst eine Klasse, die Eigenschaften der Entität definiert. Der folgende Code definiert eine Entitätsklasse, die Vorname des Kunden als Zeilenschlüssel und Nachnamen als Partitionsschlüssel verwendet. Zusammen eindeutig eine Entität Partition und Zeilenschlüssel Entität in der Tabelle. Entitäten mit der gleichen Partitionsschlüssel können schneller als mit anderen Partition abgefragt werden.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Tabellenoperationen Entitäten müssen ein **TableOperation** -Objekt. Dieses Objekt definiert die auszuführende Operation für eine Entität mit einem **CloudTable** -Objekt ausgeführt werden können. Der folgende Code erstellt eine neue Instanz der **CustomerEntity** -Klasse mit einigen Kundendaten gespeichert werden. Der Code ruft **TableOperation.insertOrReplace** zum Erstellen eines **TableOperation** -Objekts, um eine Entität in eine Tabelle einfügen weiter und neue **CustomerEntity** zugeordnet. Schließlich ruft der Code die Methode **execute** für das **CloudTable** -Objekt gibt die Tabelle "Mitarbeiter" und die neue **TableOperation**, sendet dann eine Anforderung an den Speicher neue Kundenentität in die Tabelle "Personen" Einfügen oder die Entität ersetzen, falls bereits vorhanden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Gewusst wie: Einfügen ein Batches von Entitäten

Sie können einen Stapel von Entitäten, die der Dienst in einem Schreibvorgang einfügen. Der folgende Code erstellt ein **TableBatchOperation** -Objekt und fügt drei insert-Operationen zu. Jedem Einfügevorgang wird hinzugefügt, seine festgelegt, eine neue Entität erstellen und Aufrufen von **insert** -Methode für das **TableBatchOperation** -Objekt eine Insert-Operation die Entität zugeordnet. Anschließend ruft der Code **Ausführen** bei **CloudTable** -Objekt, wobei die Tabelle "Personen" und das **TableBatchOperation** -Objekt, das Stapelverarbeitung Tabellenvorgänge Speicherdienst in einer Anforderung gesendet.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Beachten Sie Batchoperationen Folgendes:

- Sie können bis zu 100 Einfügen ausführen, löschen, zusammenführen, ersetzen, Einfügen zusammenführen, und einfügen oder ersetzen alle zusammen in einem einzigen Batch.
- Ein Batchvorgang können einen Vorgang abrufen, ist der einzige Vorgang im Batch.
- Alle Entitäten in einem einzigen Arbeitsgang muss denselben Partitionsschlüssel.
- Ein Batchvorgang ist auf 4MB Datennutzlast.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Gewusst wie: Abrufen aller Entitäten in einer Partition

Verwenden Sie zum Abfragen einer Tabelle für Entitäten in einer Partition **TableQuery**. Rufen Sie **TableQuery.from** zum Erstellen einer Abfrage auf eine bestimmte Tabelle, die einen Ergebnistyp angegebene zurückgibt. Der folgende Code gibt einen Filter für die Entitäten "Smith" der Partitionsschlüssel ist. **TableQuery.generateFilterCondition** ist eine Hilfsmethode zum Erstellen von Filtern für Abfragen. Auf die **TableQuery.from** -Methode zum Filtern der Abfrage zurückgegebene **,** aufrufen. Beim Ausführen der Abfrage mit einem Aufruf von **CloudTable** Objekt **Ausführen** gibt einen **Iterator** mit dem angegebenen **CustomerEntity** Ergebnis zurück. Können Sie den **Iterator** zurückgegeben eine for each-Schleife, die Ergebnisse verarbeiten. Dieser Code gibt die Felder jeder Entität in den Abfrageergebnissen in der Konsole.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Gewusst wie: einen Bereich von Elementen in einer Partition

Möchten Sie alle Elemente in einer Partition Abfragen, können Sie einen Bereich mithilfe von Vergleichsoperatoren in einem Filter angeben. Der folgende Code kombiniert zwei Filter zu allen Personen in Partition der Zeilenschlüssel (Vorname) mit einem Buchstaben "E" bis beginnt "Smith" im Alphabet. Dann werden die Ergebnisse der Abfrage. Verwenden Sie die Elemente der Tabelle im Stapel hinzugefügt Abschnitt einfügen, werden nur zwei Entitäten diesmal (Ben und Denise Smith) zurückgegeben Jeff Smith ist nicht enthalten.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Gewusst wie: Abrufen eine Entität

Sie können eine Abfrage zum Abrufen einer bestimmten Entität schreiben. Der folgende Code ruft **TableOperation.retrieve** mit Partitionsschlüssel und Schlüsselparameter an Kunden "Jeff Smith", anstatt einen **TableQuery** erstellen und Verwenden von Filtern auf dasselbe. Bei der Ausführung gibt die Operation Abrufen nur eine Entität statt einer Auflistung zurück. Die **GetResultAsType** -Methode wandelt das Ergebnis in den Typ der Zuordnung Ziel ein **CustomerEntity** -Objekt. Wenn dieser Typ nicht kompatibel mit dem für die Abfrage angegeben ist, wird eine Ausnahme ausgelöst. Ein null-Wert wird zurückgegeben, wenn keine Entität eine genaue Partition und Zeilenschlüssel übereinstimmen. Tasten Partition und Zeilen in einer Abfrage angeben ist die schnellste Möglichkeit, eine einzelne Entität aus der Tabelle Service abrufen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Gewusst wie: ändern eine Entität

Ändern eine Entität aus der Tabelle Service abrufen, ändern Sie das Entitätsobjekt und Änderungen an der Tabelle Service mit einem Vorgang ersetzen oder zusammenführen. Der folgende Code ändert einen bestehenden Kunden Telefonnummer. Anstatt **TableOperation.insert** wie einfügen, ruft dieser Code **TableOperation.replace**. **CloudTable.execute** -Methode ruft den Dienst und die Entität ersetzt, es sei denn, eine andere Anwendung verändert er in der Anwendung abgerufen. In diesem Fall wird eine Ausnahme ausgelöst und die Entität abgerufen, geändert und erneut gespeichert werden muss. Dieses vollständige Parallelität wiederholen Muster ist in einem dezentralen System häufig.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Gewusst wie: Abfragen eine Teilmenge von Entitätseigenschaften

Eine Abfrage in eine Tabelle kann nur wenige Eigenschaften einer Entität abrufen. Diese Technik namens Projektion Bandbreite reduziert und abfrageleistung, insbesondere bei großen Unternehmen verbessern kann. Die Abfrage im folgenden Code verwendet die **select** -Methode nur die e-Mail-Adressen von Entitäten in der Tabelle zurückgegeben. Die Ergebnisse werden in eine **Zeichenfolge** mithilfe einer **EntityResolver**projiziert die Typumwandlung auf den vom Server zurückgegebenen Entitäten. Erfahren Sie mehr über Projektion [Azure Tabellen: Introducing Upsert und Abfrageprojektion][]. Beachten Sie, dass die Projektion auf lokalen Speicheremulator nicht unterstützt wird, damit dieser Code ausgeführt wird, sobald ein Konto für den Dienst verwenden.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Gewusst wie: Einfügen oder Ersetzen eine Entität

Häufig möchten Sie eine Entität zu einer Tabelle hinzufügen, ohne bereits in der Tabelle vorhanden ist. Einfügen oder Ersetzen-Vorgang können Sie eine einzelne Anforderung die Entität eingefügt werden, wenn es nicht vorhanden oder vorhandenen ersetzen, wenn dies der Fall ist. Aufbauend auf früheren Beispielen der folgende Code fügt oder Entität "Walter Harfe" ersetzt. Nach dem Erstellen einer neuen Entität, ruft dieser Code die **TableOperation.insertOrReplace** -Methode. Dieser Code dann auf das Objekt **CloudTable** der Tabelle und Einfügen ruft **Ausführen** oder Ersetzungsvorgang Tabelle als Parameter. Aktualisieren Sie nur einen Teil einer Entität kann stattdessen die **TableOperation.insertOrMerge** -Methode verwendet werden. Hinweis: Diese einfügen oder ersetzen auf lokalen Speicheremulator nicht unterstützt wird, damit dieser Code ausgeführt wird, sobald ein Konto für den Dienst verwenden. Erfahren Sie mehr über einfügen oder ersetzen und einfügen oder Zusammenführen in diesem [Azure Tabellen: Introducing Upsert und Abfrageprojektion][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Gewusst wie: löschen eine Entität

Nachdem sie abgerufen haben, können Sie problemlos eine Entität löschen. Nachdem die Entität abgerufen, rufen Sie **TableOperation.delete** mit der Entität löschen. Rufen Sie dann für das **CloudTable** -Objekt **Ausführen** . Der folgende Code ruft und löscht eine Customer-Entität.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Gewusst wie: Löschen einer Tabelle

Der folgende Code löscht schließlich eine Tabelle aus ein Speicherkonto. Eine Tabelle gelöscht wurde werden nicht für einen Zeitraum nach dem Löschen in der Regel weniger als 40 Sekunden neu erstellt.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Tabellenspeicher bearbeitet haben, folgen Sie diesen Links komplexer Storage Aufgaben erläutert.

- [Azure-Speicher SDK für Java][]
- [Azure-Speicher Client SDK-Referenz][]
- [Azure Storage REST-API][]
- [Azure-Speicher-Teamblog][]

Weitere Informationen finden Sie auch [Java Developer Center](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure-Speicher SDK für Java]: https://github.com/azure/azure-storage-java
[Azure-Speicher SDK für Android]: https://github.com/azure/azure-storage-android
[Azure-Speicher Client SDK-Referenz]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST-API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tabellen: Einführung in Upsert und Abfrageprojektion]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
