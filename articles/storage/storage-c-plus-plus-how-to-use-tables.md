<properties
    pageTitle="Wie Sie Tabellenspeicher (C++) | Microsoft Azure"
    description="Strukturierte Daten in der Cloud Azure-Tabellenspeicher, einem NoSQL-Datenspeicher gespeichert."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Verwendung von C++ Tabellenspeicher

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht  
Dabei zeigen Ihnen Szenarien unter Verwendung von Azure Table Storage durchführen. Die Beispiele sind in C++ geschrieben und der [Azure-Speicher-Clientbibliothek für C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Die fallen Szenarien **Erstellen und Löschen einer Tabelle** und **mit Tabelle**.

>[AZURE.NOTE] Dabei zielt Azure Storage-Clientbibliothek für C++ Version 1.0.0 und höher. Die empfohlene Version ist Speicher-Clientbibliothek 2.2.0, über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendung  
Verwenden Sie in diesem Handbuch Speicherfunktionen, die in einer C++-Anwendung ausgeführt werden kann. Hierzu müssen Sie installieren Azure Storage-Clientbibliothek für C++ und Azure-Speicher in der Azure-Abonnement registrieren.  

Installation von Azure Storage-Clientbibliothek für C++ können Sie die folgenden Methoden:

-   **Linux:** Anweisungen Sie auf [Azure Storage-Clientbibliothek C++-Infodatei](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windows:** Klicken Sie in Visual Studio auf **Tools > NuGet Paket-Manager > Paket-Manager Konsole**. Geben Sie folgenden Befehl in die [Konsole NuGet Paket-Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) und drücken Sie die EINGABETASTE.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurieren Sie Ihre Anwendung auf Tabellenspeicher  
Fügen Sie folgende Anweisung am Anfang der Datei C++ enthalten den Azure-Speicher-APIs verwenden, um Tabellen zugegriffen werden soll:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Richten Sie eine Verbindungszeichenfolge Azure-Speicher  
Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und Anmeldeinformationen für den Zugriff auf Daten-Management-Services. Wenn eine Clientanwendung müssen Speicher Verbindungszeichenfolge im folgenden Format angeben. Verwenden Sie den Namen Ihres Speicherkontos und Zugriffstaste Speicher für das Speicherkonto in [Azure-Portal](https://portal.azure.com) für *Kontoname* und *AccountKey* aufgeführt. Informationen zu Speicherkonten und Zugriffstasten anzeigen Sie [zu Azure-Speicherkonten](storage-create-storage-account.md) Dieses Beispiel zeigt, wie Sie ein statisches Feld der Verbindungszeichenfolge zu deklarieren:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Zum Testen einer Anwendung in der lokalen Windows-basierten Computer können Sie Azure [Speicheremulator](storage-use-emulator.md) , die mit dem [Azure SDK](https://azure.microsoft.com/downloads/)installiert ist. Der Speicheremulator ist ein Dienstprogramm, das die Azure BLOB-Warteschlange und Tabelle Dienste auf dem lokalen Entwicklungscomputer simuliert. Das folgende Beispiel zeigt, wie Sie ein statisches Feld Verbindungszeichenfolge die lokalen Speicheremulator zu deklarieren:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Um den Azure-Speicher-Emulator zu starten, klicken Sie auf die Schaltfläche **Start** oder drücken Sie auf Windows. Geben Sie **Azure Speicheremulator**, und wählen Sie dann aus der Liste der Programme **Microsoft Azure Speicheremulator** .  

In den folgenden Beispielen davon ausgehen, dass Sie eine dieser beiden Methoden zu Speicher-Verbindungszeichenfolge verwendet haben.  

## <a name="retrieve-your-connection-string"></a>Abrufen der Verbindungszeichenfolge  
Die **Cloud_storage_account** -Klasse können Sie speicherkontoinformationen darstellen. Rufen Sie Ihre speicherkontoinformationen aus der Verbindungszeichenfolge Storage können Sie die Parse-Methode.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Als Nächstes rufen Sie einen Verweis auf eine **Cloud_table_client** Klasse als Referenzobjekte für Tabellen und Entitäten in der Speicherdienst Tabelle gespeicherten abrufen können. Der folgende Code erstellt ein **Cloud_table_client** -Objekt mithilfe des Konto-Speicherobjekts wir über abgerufen:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Erstellen einer Tabelle
Ein **Cloud_table_client** -Objekt können Sie Referenzobjekte für Tabellen und Entitäten. Der folgende Code erstellt ein **Cloud_table_client** -Objekt und zum Erstellen einer neuen Tabelle verwendet.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle
Hinzufügen einer Entität zu einer Tabelle, Erstellen eines neuen **Table_entity** -Objekts und an **table_operation::insert_entity**übergeben. Der folgende Code verwendet Vorname des Kunden als Zeilenschlüssel und Nachnamen als Partitionsschlüssel. Zusammen eindeutig eine Entität Partition und Zeilenschlüssel Entität in der Tabelle. Entitäten mit der gleichen Partitionsschlüssel schneller als mit anderen Partition abgefragt werden können, jedoch mit unterschiedlichen Partition größer Parallelbetrieb Skalierbarkeit ermöglicht. Weitere Informationen finden Sie im [Microsoft Azure-Speicher-Performance und Skalierung Prüfliste](storage-performance-checklist.md).

Der folgende Code erstellt eine neue Instanz der **Table_entity** mit einigen Kundendaten gespeichert werden. Der Code weiter ruft **table_operation::insert_entity** zum Erstellen eines **Table_operation** -Objekts, um eine Entität in eine Tabelle einfügen und die neue Tabellenentität zugeordnet. Schließlich ruft der Code die Execute-Methode für das **Cloud_table** -Objekt. Und neue **Table_operation** sendet eine Anforderung an die Tabelle neue Kundenentität in die Tabelle "Personen" einfügen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Fügen Sie mehrerer Elemente ein
Sie können einen Stapel von Entitäten, die der Dienst in einem Schreibvorgang einfügen. Der folgende Code erstellt ein **Table_batch_operation** -Objekt und fügt dann hinzu, drei insert-Operationen zu. Jedem Einfügevorgang wird durch eine neue Entität erstellen, seine festgelegt und Aufrufen der Insert-Methode für das **Table_batch_operation** -Objekt eine Insert-Operation die Entität zugeordnet hinzugefügt. Dann wird **cloud_table.execute** aufgerufen, um den Vorgang auszuführen.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Beachten Sie Batchoperationen Folgendes:  

-   Sie können bis zu 100 einfügen, löschen, zusammenführen, ersetzen, einfügen oder zusammenführen und einfügen oder Ersetzen-Vorgänge in beliebiger Kombination in einem einzigen Batch ausführen.  
-   Ein Batchvorgang können einen Vorgang abrufen, ist der einzige Vorgang im Batch.  
-   Alle Entitäten in einem einzigen Arbeitsgang muss denselben Partitionsschlüssel.  
-   Ein Batchvorgang ist auf 4 MB Datennutzlast.  

## <a name="retrieve-all-entities-in-a-partition"></a>Rufen Sie aller Entitäten in einer Partition ab
Verwenden Sie zum Abfragen einer Tabelle für alle Entitäten in einer Partition ein **Table_query** -Objekt. Das folgende Codebeispiel gibt einen Filter für die Entitäten "Smith" der Partitionsschlüssel ist. Dieses Beispiel druckt die Felder jeder Entität in den Abfrageergebnissen in der Konsole.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Die Abfrage in diesem Beispiel wird die Entitäten, die den Filterkriterien entsprechen. Wenn Sie große Tabellen müssen die Tabellenentitäten herunterladen oft empfohlen speichern Sie die Daten in Azure-Speicher Blobs stattdessen.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Einen Bereich von Elementen in einer partition
Möchten Sie alle Elemente in einer Partition Abfragen, können Sie einen Bereich angeben, durch die Kombination der Partition Schlüsselfilter mit einem Schlüssel Zeilenfilter. Im folgenden Codebeispiel verwendet zwei Filter zu allen Entitäten Partition "Smith", wo der Zeilenschlüssel (Vorname) beginnt mit einem Buchstaben "E" im Alphabet vor und gibt die Ergebnisse der Abfrage.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Abrufen einer Entität
Sie können eine Abfrage zum Abrufen einer bestimmten Entität schreiben. Der folgende Code verwendet die **table_operation::retrieve_entity** an den Kunden "Jeff Smith". Diese Methode gibt nur eine Entität statt einer Auflistung und der zurückgegebene Wert ist in **Table_result**. Tasten Partition und Zeilen in einer Abfrage angeben ist die schnellste Möglichkeit, eine einzelne Entität aus der Tabelle Service abrufen.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Ein Element ersetzen
Ersetzen einer Entität aus der Tabelle Service abrufen, ändern das Entitätsobjekt und dann die Änderungen an den Dienst. Der folgende Code ändert einen bestehenden Kunden Telefonnummer und e-Mail-Adresse. Dieser Code verwendet anstatt **table_operation::insert_entity** **table_operation::replace_entity**. Dadurch wird die Entität auf dem Server vollständig ersetzt werden, wenn die Entität auf dem Server geändert wurde, nachdem sie abgerufen wurden, in diesem Fall schlägt der Vorgang fehl. Dieser Fehler wird der Anwendung versehentlich überschrieben Änderung zwischen dem Abrufen und Aktualisieren von einer anderen Komponente der Anwendung verhindern. Handhabung dieses Fehlers ist Entität abrufen, ändern (sofern noch gültig) und führen Sie dann einen anderen **table_operation::replace_entity** Vorgang. Im nächste Abschnitt zeigen Ihnen, wie dieses Verhalten überschrieben.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Einfügen-oder-Ersetzen einer Entität
**table_operation::replace_entity** Vorgänge schlagen fehl, wenn die Entität geändert wurde, nachdem sie vom Server abgerufen wurden. Darüber hinaus müssen Sie die Entität aus dem Server zuerst für **table_operation::replace_entity** zu abrufen. Manchmal jedoch nicht sicher, ob die Entität auf dem Server vorhanden ist, deren aktuellen Werte gespeichert – die Aktualisierung sollten sie alle überschreiben. Dazu verwenden Sie einen **table_operation::insert_or_replace_entity** Vorgang. Dieser Vorgang fügt die Entität ist nicht vorhanden, oder andernfalls unabhängig davon, wann die letzte Aktualisierung wurde ersetzt. Im folgenden Codebeispiel wird die Kundenentität Jeff Smith noch abgerufen, jedoch wird dann an den Server über **table_operation::insert_or_replace_entity**. Zwischen dem Abrufen und Aktualisieren der Entität Aktualisierungen werden überschrieben.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Abfrage einer Teilmenge von Entitätseigenschaften  
Eine Abfrage in eine Tabelle kann nur wenige Eigenschaften einer Entität abrufen. Die Abfrage im folgenden Code verwendet die **table_query::set_select_columns** -Methode nur die e-Mail-Adressen von Entitäten in der Tabelle zurückgegeben.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Einige Eigenschaften einer Entität Abfragen ist effizienter als das Abrufen aller Eigenschaften.

## <a name="delete-an-entity"></a>Löschen einer Entität
Nachdem sie abgerufen haben, können Sie problemlos eine Entität löschen. Nachdem die Entität abgerufen, rufen Sie **table_operation::delete_entity** mit der Entität gelöscht. Rufen Sie die Methode **cloud_table.execute** . Der folgende Code Ruft ab und löscht eine Entität mit einer Partition von "Schmidt" und einem Zeilenschlüssel von "Jeff".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Löschen einer Tabelle
Schließlich löscht im folgenden Codebeispiel wird eine Tabelle in ein Speicherkonto. Eine gelöschte Tabelle verloren für einen Zeitraum nach dem Löschen neu erstellt werden.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen der Tabellenspeicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr über Azure-Speicher:  

-   [Verwendung von C++-BLOB-Speicher](storage-c-plus-plus-how-to-use-blobs.md)
-   [Verwendung von C++ Warteschlangenspeicher](storage-c-plus-plus-how-to-use-queues.md)
-   [Liste der Azure-Speicher-Ressourcen in C++](storage-c-plus-plus-enumeration.md)
-   [Speicher-Clientbibliothek für C++-Referenz](http://azure.github.io/azure-storage-cpp)
-   [Azure Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
