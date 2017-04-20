<properties
    pageTitle="Verwendung von PHP Tabellenspeicher | Microsoft Azure"
    description="Informationen zum Verwenden des Tabelle Service von PHP erstellen und Löschen einer Tabelle einfügen, löschen und die Tabelle."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Verwendung von PHP Tabellenspeicher

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die allgemeine Szenarien mit der Azure-Diensts. Die Beispiele in PHP geschrieben und [Azure SDK für PHP]verwenden[download]. Die fallen Szenarien **Erstellen und Löschen einer Tabelle einfügen, löschen und Abfragen von Entitäten in einer Tabelle**. Weitere Informationen zu den Azure-Diensts finden Sie im Abschnitt [Weiter](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine PHP-Anwendung

Voraussetzung für das Erstellen einer PHP-Anwendung, die der Azure-Diensts zugreift, ist das Verweisen auf Klassen in Azure SDK für PHP aus im Code. Alle Entwicklungstools können zum Erstellen einer Anwendung, wie z. B. Editor.

In diesem Handbuch Verwenden Sie Tabelle Service-Features, die von innerhalb einer PHP-Anwendung lokal oder in einer Webrolle Azure Worker-Rolle oder Website Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen der Azure-Clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfigurieren Sie Ihre Anwendung auf den Dienst zugreifen

Um die Tabelle Azure Service-APIs verwenden, müssen Sie:

1. Die Autoloader Datei [per] Verweis[ require_once] -Anweisung, und
2. Verweisen auf alle Klassen, die Sie verwenden können.

Im folgenden Beispiel wird veranschaulicht, wie Includedatei Autoloader und die **ServicesBuilder** -Klasse.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) angenommen, PHP-Clientbibliotheken für Azure über Composer installiert haben. Wenn die Bibliotheken manuell installiert wird, müssen Sie auf die <code>WindowsAzure.php</code> Autoloader-Datei.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In den folgenden Beispielen der `require_once` -Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel ausgeführt wird.

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindungs Azure-Speicher

Um ein Webdienstclient Azure Tabelle zu instanziieren, müssen Sie zunächst eine gültige Verbindungszeichenfolge. Das Format für die Verbindungszeichenfolge der Tabelle Service ist:

Für den Zugriff auf live-Dienstes:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Für den Zugriff auf den Emulator-Speicher:

    UseDevelopmentStorage=true


Um Azure Webdienstclient erstellen, müssen Sie die **ServicesBuilder** -Klasse verwenden. Sie können:

* die Verbindungszeichenfolge direkt übergeben oder
* Verwenden Sie **CloudConfigurationManager (CCM)** externe Quellen für die Verbindungszeichenfolge zu überprüfen:
    * Standardmäßig bietet es Unterstützung für eine externe Quelle - Umgebungsvariablen
    * neue Datenquellen können durch Erweitern der **ConnectionStringSource** -Klasse

Die hier aufgeführten Beispiele wird die Verbindungszeichenfolge direkt übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Erstellen einer Tabelle

Ein **TableRestProxy** -Objekt können Sie eine Tabelle mit der **CreateTable** -Methode erstellen. Beim Erstellen einer Tabelle können Sie das Tabelle Service Timeout festlegen. (Weitere Informationen über die Tabelle Service Timeout finden Sie [Einstellung Timeouts für Dienstvorgänge Tabelle][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Informationen zu Namen finden Sie unter [Understanding Tabelle Service-Datenmodell][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um eine Entität zu einer Tabelle hinzuzufügen, erstellen Sie **ein neues Entitätsobjekt** und **TableRestProxy**-> InsertEntity übergeben. Beachten Sie, dass Sie beim Erstellen einer Entität angeben müssen einen `PartitionKey` und `RowKey`. Diese sind eindeutige Bezeichner für eine Entität und Werte, die schneller als andere Entitätseigenschaften abgefragt werden können. Das System verwendet `PartitionKey` die Tabelle Entitäten über viele Speicherknoten automatisch verteilen. Entitäten mit dem gleichen `PartitionKey` auf demselben Knoten gespeichert sind. (Operationen auf mehreren Personen auf demselben Knoten gespeicherte besser als Entitäten auf anderen Knoten gespeichert.) Die `RowKey` ist die eindeutige ID eines Elements innerhalb einer Partition.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Weitere Informationen über Eigenschaften und Typen [Verstehen des Datenmodells Tabelle Service][table-data-model].

Die **TableRestProxy** -Klasse bietet zwei alternative Methoden zum Einfügen von Entitäten: **InsertOrMergeEntity** und **InsertOrReplaceEntity**. Diese Methoden verwenden, eine neue **Entität** erstellen und als Parameter an eine Methode übergeben. Jede Methode wird Entität eingefügt, wenn er nicht vorhanden ist. Wenn die Entität bereits vorhanden ist, **InsertOrMergeEntity** Eigenschaftswerte aktualisiert, wenn die Eigenschaften vorhanden und neue Eigenschaften hinzugefügt, wenn diese nicht vorhanden sind, während **InsertOrReplaceEntity** eine vorhandene Entität vollständig ersetzt. Im folgenden Beispiel wird veranschaulicht, wie mit **InsertOrMergeEntity**. Wenn die Entität mit `PartitionKey` "TasksSeattle" und `RowKey` "1" bereits vorhanden ist, wird eingefügt. Wenn es zuvor eingefügt wurde (wie im obigen Beispiel gezeigt), die `DueDate` -Eigenschaft aktualisiert werden, und `Status` Eigenschaft hinzugefügt. Die `Description` und `Location` Eigenschaften werden auch aktualisiert, aber mit Werten, die effektiv unverändert lassen. Diesen beiden Eigenschaften wurden nicht hinzugefügt, wie im Beispiel gezeigt, aber in der Zielentität vorhanden, würde ihre vorhandenen Werte unverändert.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Abrufen einer Entität

**TableRestProxy -> GetEntity** -Methode können Sie eine einzelne Entität abrufen für die `PartitionKey` und `RowKey`. Im Beispiel unten die Partitionsschlüssel `tasksSeattle` und Zeile `1` **GetEntity** -Methode übergeben werden.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Rufen Sie aller Entitäten in einer Partition ab

Entity-Abfragen mit Filter erstellt werden (Weitere Informationen finden Sie unter [Abfragen von Tabellen und Entitäten][filters]). Um alle Entitäten in Partition abzurufen, verwenden Sie den Filter "PartitionKey Eq *Partitionsname*". Das folgende Beispiel zeigt das Abrufen aller Entitäten in der `tasksSeattle` Partition einen Filter auf die **QueryEntities** -Methode übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Abrufen einer Teilmenge der Entitäten in einer partition

Im vorherigen Beispiel verwendete dasselbe Muster kann eine beliebige Teilmenge der Entitäten in einer Partition abrufen verwendet werden. Die Teilmenge der Elemente, die Sie abrufen anhand der Filter Sie verwenden (Weitere Informationen finden Sie unter [Abfragen von Tabellen und Entitäten][filters]). Das folgende Beispiel zeigt, wie Sie Filter zum Abrufen aller Entitäten mit einer bestimmten `Location` und `DueDate` unter einem angegebenen Datum.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Abrufen einer Teilmenge von Entitätseigenschaften

Eine Abfrage kann eine Teilmenge von Entitätseigenschaften abrufen. Diese Technik *Projektion*genannt Bandbreite reduziert und abfrageleistung, insbesondere bei großen Unternehmen verbessern kann. **Abfrage -> AddSelectField** -Methode zum Festlegen einer Eigenschaft abgerufen werden soll übergeben Sie den Namen der Eigenschaft. Sie können diese Methode mehrfach aufrufen, weitere Eigenschaften hinzufügen. Nach dem **TableRestProxy -> QueryEntities**ausführen, müssen die zurückgegebenen Entitäten nur ausgewählten Eigenschaften. (Um eine Teilmenge der Tabellenentitäten zurückzugeben, verwenden Sie Filter siehe oben stehende Abfragen.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Eine vorhandene Entität kann Methoden **Entity -> SetProperty** und **Entity -> AddProperty** für die Entität und das anschließende **TableRestProxy -> UpdateEntity**aktualisiert. Im folgende Beispiel ruft ein Element ab, ändert eine Eigenschaft einer anderen Eigenschaft entfernt und eine neue Eigenschaft hinzugefügt. Beachten Sie, dass Sie eine Eigenschaft entfernen können, indem Sie den Wert auf **null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Löschen einer Entität

Um ein Element zu löschen, übergeben Sie den Tabellennamen und der Entität `PartitionKey` und `RowKey` **TableRestProxy -> DeleteEntity** -Methode.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Beachten Sie, dass für Parallelität, das Etag für eine Entität gelöscht werden soll, mithilfe der Methode **DeleteEntityOptions -> SetEtag** und das **DeleteEntityOptions** -Objekt zu **DeleteEntity** als vierter festlegen können Parameter.

## <a name="batch-table-operations"></a>Tabelle Batchoperationen

**TableRestProxy -> Batch** -Methode können Sie mehrere Operationen in einer Anforderung ausgeführt. Das Muster beinhaltet **BatchRequest** Objekt Vorgänge hinzufügen und dann das **BatchRequest** -Objekt an die Methode **TableRestProxy -> Batch** . Um einen Vorgang für ein **BatchRequest** -Objekt hinzuzufügen, können Sie eine der folgenden Methoden mehrmals aufrufen:

* **addInsertEntity** (fügt eine InsertEntity Operation)
* **addUpdateEntity** (fügt eine UpdateEntity Operation)
* **addMergeEntity** (fügt eine MergeEntity Operation)
* **addInsertOrReplaceEntity** (fügt eine InsertOrReplaceEntity Operation)
* **addInsertOrMergeEntity** (fügt eine InsertOrMergeEntity Operation)
* **addDeleteEntity** (fügt eine DeleteEntity Operation)

Das folgende Beispiel zeigt, wie **InsertEntity** und **DeleteEntity** -Operationen in einer Anforderung ausgeführt:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Finden Sie weitere Informationen zur Batchverarbeitung Tabellenvorgänge [Entität Gruppe Transaktionen][entity-group-transactions].

## <a name="delete-a-table"></a>Löschen einer Tabelle

Schließlich um eine Tabelle zu löschen, übergeben Sie den Tabellennamen an die **TableRestProxy -> DeleteTable** -Methode.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Nächste Schritte

Grundlagen der Azure-Diensts bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren.

- Besuchen Sie den [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Weitere Informationen finden Sie auch die [PHP-Entwicklercenter](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
