<properties
    pageTitle="Azure Funktionen Trigger und Bindungen für Azure Storage | Microsoft Azure"
    description="Verstehen Sie, wie Azure Storage Trigger und Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktionen, Funktionen, Verarbeitung von Ereignissen, dynamische Compute serverlose Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure Funktionen Trigger und Bindungen für Azure-Speicher

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel beschreibt konfigurieren und Code Azure Storage Trigger und Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure Storage Queue trigger

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON für Storage Queue trigger

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`Der Variablenname in den Code für die Warteschlange oder der Warteschlange verwendet. 
- `queueName`: Der Name der Warteschlange abgefragt. Warteschlange Benennungskonventionen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie lassen `connection` leere Trigger funktioniert mit Speicher-Verbindungszeichenfolge für Funktion app AzureWebJobsStorage app-Einstellung angegeben.
- `type`: Muss auf *QueueTrigger*festgelegt werden.
- `direction`: Muss *in*festgelegt werden. 

Beispiel *function.json* Storage Queue Trigger:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Warteschlange Trigger unterstützte Typen

Message Queue kann einer der folgenden Typen deserialisiert werden:

* Objekt (JSON)
* Zeichenfolge
* Byte-array 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Warteschlange Trigger Metadaten

Warteschlange Metadaten erhalten in Ihrer Funktion mit folgenden Namen:

* expirationTime
* insertionTime
* nextVisibleTime
* ID
* popReceipt
* dequeueCount
* QueueTrigger (anders Warteschlange Nachrichtentext als Zeichenfolge abzurufen)

In diesem C#-Codebeispiel abgerufen und Protokolle Warteschlange Metadaten:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Behandeln von Nachrichten unzustellbar Warteschlange

Nachrichten, deren Inhalt eine Funktion fehlschlagen verursacht, werden *unzustellbare Nachrichten*bezeichnet. Wenn die Funktion fehlschlägt, wird die warteschlangennachricht nicht gelöscht und schließlich wird wieder aufgenommen, verursacht des Zyklus wiederholt werden. Das SDK kann Zyklus automatisch nach einer begrenzten Anzahl von Iterationen unterbrechen oder manuell vornehmen.

Das SDK rufen eine Funktion bis zu 5 Mal eine Warteschlange Nachricht. Fünfte Versuch fehlschlägt, wird die Nachricht unzustellbar Warteschlange verschoben.

Gifte Warteschlange lautet *{Originalqueuename}*-Gift. Sie können eine Funktion zum Verarbeiten von Nachrichten aus der Warteschlange Gift zusa¨tzlich oder einer Benachrichtigung, dass manuelle Aufmerksamkeit benötigt schreiben. 

Wenn Sie unzustellbare Nachrichten manuell verarbeiten möchten, erhalten Sie oft eine Nachricht abgeholt zur Verarbeitung überprüfen `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure Storage Queue Ausgabe Bindung

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON für Speicherwarteschlange Ausgabe Bindung

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`Der Variablenname in den Code für die Warteschlange oder der Warteschlange verwendet. 
- `queueName`: Der Name der Warteschlange. Warteschlange Benennungskonventionen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie lassen `connection` leere Trigger funktioniert mit Speicher-Verbindungszeichenfolge für Funktion app AzureWebJobsStorage app-Einstellung angegeben.
- `type`: *Warteschlange*müssen festgelegt werden.
- `direction`: *Out*muss festgelegt werden. 

Beispiel *function.json* für eine Speicherwarteschlange Bindung Warteschlange Trigger verwendet und schreibt eine Warteschlange Meldung ausgeben:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Warteschlange Ausgabe unterstützt Bindungstypen

Die `queue` Bindung kann folgende Nachricht eine Warteschlange serialisieren:

* Objekt (`out T` C#, wird eine Meldung mit einem der Parameter null ist, wenn die Funktion beendet)
* Zeichenfolge (`out string` in C# erstellt Message Queue Parameterwert nicht Null ist, wenn die Funktion beendet)
* Bytearray (`out byte[]` funktioniert wie in C#, Zeichenfolge) 
* `out CloudQueueMessage`(C#, wie String) 

In C# kann die Binden an `ICollector<T>` oder `IAsyncCollector<T>` , `T` ist einer der unterstützten Typen.

#### <a name="queue-output-binding-code-examples"></a>Warteschlange Ausgabe Bindung Codebeispiele

In diesem C#-Codebeispiel schreibt eine einzelne Ausgabe Warteschlange Meldung für jede Nachricht Eingabewarteschlange.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

In diesem C#-Codebeispiel schreibt mehrere Nachrichten mit `ICollector<T>` (verwenden Sie `IAsyncCollector<T>` im Async-Funktion):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure Storage BLOB-trigger

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON für Speicher BLOB-trigger

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`Der Variablenname Blob zum Funktionscode. 
- `path`: Pfad, der den Container überwachen angibt, und optional eine BLOB-Muster.
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie lassen `connection` leere Trigger funktioniert mit Speicher-Verbindungszeichenfolge für Funktion app AzureWebJobsStorage app-Einstellung angegeben.
- `type`: Muss auf *BlobTrigger*festgelegt werden.
- `direction`: Muss *in*festgelegt werden.

Beispiel *function.json* -BLOB-Speicher-Trigger Blobs überwacht, die Beispiele Workitems Container hinzugefügt werden:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>BLOB-Trigger unterstützte Typen

Das Blob kann auf folgende Knoten oder C# Funktionen deserialisiert werden:

* Objekt (JSON)
* Zeichenfolge

C#-Funktionen können Sie auch auf die folgenden Typen binden:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere Typen von [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) deserialisiert 

#### <a name="blob-trigger-c-code-example"></a>BLOB-Trigger C# Codebeispiel

In diesem C#-Codebeispiel protokolliert den Inhalt jedes BLOB überwachten Container hinzugefügt wird.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>BLOB-Trigger Suchmustern

Geben Sie ein Namensmuster Blob in der `path` Eigenschaft. Zum Beispiel:

```json
"path": "input/original-{name}",
```

Dieser Pfad würde einen Blob mit dem Namen im *input* Container und der Wert der *Ursprünglichen Blob1.txt* finden die `name` Variable Funktionscode wäre `Blob1`.

Ein weiteres Beispiel:

```json
"path": "input/{blobname}.{blobextension}",
```

Dieser Pfad würde auch namens *Original-Blob1.txt*und der Wert des Blob finden die `blobname` und `blobextension` Variablen Funktionscode wäre *Original Blob1* und *Txt*.

Sie können die Typen der Blobs, die die Funktion Auslösen durch Angabe eines Musters mit einem festen Wert für die Erweiterung. Festlegen der `path` *Beispiele / {name} PNG*, löst nur *PNG* Blobs im Container *Beispiele* die Funktion.

Benötigen Sie ein Muster für BLOB-Namen anzugeben, die geschweifte Klammern in den Namen, verdoppeln Sie die geschweiften Klammern. Beispielsweise soll Blobs im Container *Bilder* , deren Namen wie folgt:

        {20140101}-soundfile.mp3

Verwenden Sie diese für die `path` Eigenschaft:

        images/{{20140101}}-{name}

Im Beispiel der `name` Wert wäre *soundfile.mp3*. 

#### <a name="blob-receipts"></a>BLOB-Zugänge

Die Laufzeit Azure-Funktionen wird sichergestellt, dass keine BLOB-Trigger-Funktion mehrmals für das gleiche neue oder aktualisierte Blob aufgerufen wird. Dies geschieht durch *Blob Zugänge* verwalten, um festzustellen, ob eine angegebene Blob verarbeitet wurde.

BLOB-Zugänge werden in einem Container mit dem Namen *Azure Webaufträge Hosts* angegebene Verbindungszeichenfolge AzureWebJobsStorage Azure Storage-Konto gespeichert. Eine Empfangsbestätigung Blob enthält die folgenden Informationen:

* Die Funktion hieß für Blob ("*{Funktion app Name}*. Funktionen. *{Funktionsname}*", zum Beispiel:"functionsf74b96f7. Functions.CopyBlob")
* Der Containername
* BLOB-Typ ("BlockBlob" oder "PageBlob")
* Der BLOB-name
* Das ETag (einen Versionsbezeichner Blob zum Beispiel: "0x8D1DC6E70A277EF")

Wenn Sie Blob Wiederaufarbeitung erzwingen möchten, können Sie manuell BLOB-Eingang für dieses Blob aus dem Container *Azure Webaufträge Hosts* löschen.

#### <a name="handling-poison-blobs"></a>Gift BLOB-Verarbeitung

Fällt eine BLOB-Trigger-Funktion wird das SDK wieder für den Fall, dass der Fehler durch einen vorübergehenden Fehler verursacht wurde. Wenn der Inhalt des BLOBs fehlschlägt, schlägt die Funktion jedes Mal versucht das Blob zu. Standardmäßig ruft SDK eine Funktion bis zu 5 Mal für einen angegebenen Blob. Fällt der fünfte Versuch fügt SDK eine Nachricht an eine Warteschlange mit dem Namen *Webjobs Blobtrigger Poison*.

Die Warteschlange für unzustellbare Blobs wird ein JSON-Objekt mit den folgenden Eigenschaften:

* FunctionId (im Format *{Funktion app Name}*. Funktionen. *{Funktionsname}*)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (einen Versionsbezeichner Blob zum Beispiel: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>BLOB Großbehältern abrufen

Enthält der blobcontainer, den Überwachung der Trigger 10.000 Blobs Protokolldateien Funktionen Runtime Scans auf neue oder geänderte Blobs. Dieser Prozess ist nicht in Echtzeit; eine Funktion kann nicht mehrere Minuten oder länger ausgelöst werden nach das Blob erstellt wird. Darüber hinaus [Speicher erstellten Protokolle auf einem"besten"](https://msdn.microsoft.com/library/azure/hh343262.aspx) Basis; Es gibt keine Garantie, dass alle Ereignisse erfasst werden. Unter gewissen Umständen können Protokolle fehlen. Wenn die Geschwindigkeit und Zuverlässigkeit Grenzen der BLOB-Trigger für Behälter nicht für Ihre Anwendung sind, ist die empfohlene Methode zum Erstellen einer warteschlangennachricht Blob erstellen und Trigger Blob Blob zu Warteschlange Trigger anstelle.
 
## <a id="storageblobbindings"></a>Azure-BLOB-Speicher und-Ausgabe Bindungen

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON für ein Speicher-Blob Eingabe- oder Bindung

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`Der Variablenname Blob zum Funktionscode. 
- `path`: Ein Pfad, der gibt den Container aus das Blob gelesen oder Blob, und optional eine BLOB-Muster.
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie lassen `connection` leere Bindung funktioniert mit Speicher-Verbindungszeichenfolge für Funktion app AzureWebJobsStorage app-Einstellung angegeben.
- `type`: Muß *BLOB*festgelegt werden.
- `direction`: *In* oder *out*festgelegt. 

Beispiel *function.json* für ein BLOB-Eingabe oder Ausgabe Bindung mithilfe eines Triggers Warteschlange Blob kopiert:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>BLOB-Eingabe- und unterstützte Typen

Die `blob` Bindung Serialisieren oder Deserialisieren folgende Node.js oder C#-Funktionen:

* Objekt (`out T` in C# für Ausgabe BLOBs: einen Blob als null-Objekt erstellt, wenn Parameterwert null nach Beendigung der Funktion)
* Zeichenfolge (`out string` in C# für Ausgabe BLOBs: einen Blob erstellt, nur, wenn der Parameter nicht Null ist, gibt die Funktion)

In C# Funktionen können Sie auch auf die folgenden Typen binden:

* `TextReader`(nur Eingabe)
* `TextWriter`(nur Ausgabe)
* `Stream`
* `CloudBlobStream`(nur Ausgabe)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>BLOB-Ausgabe C# Codebeispiel

In diesem C#-Codebeispiel kopiert einen Blob, deren Name in einer warteschlangennachricht empfangen wird.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure Tabellen und-Ausgabe Bindungen

#### <a name="functionjson-for-storage-tables"></a>Function.JSON für Tabellen

*Function.json* gibt die folgenden Eigenschaften.

- `name`Der Variablenname in den Code für die tabellenbindung verwendet. 
- `tableName`: Der Name der Tabelle.
- `partitionKey`und `rowKey` : gemeinsam verwendet werden, eine einzelne Entität in einer C#- oder Knoten oder eine einzelne Entität in einer Knoten-Funktion schreiben.
- `take`Die maximale Zeilenanzahl für Tabelle Eingabe in Knoten Funktion lesen.
- `filter`: OData-Filterausdruck für Tabelle Eingabe in einer Knoten-Funktion.
- `connection`: Der Name einer app-Einstellung, die eine Verbindungszeichenfolge Speicher enthält. Wenn Sie lassen `connection` leere Bindung funktioniert mit Speicher-Verbindungszeichenfolge für Funktion app AzureWebJobsStorage app-Einstellung angegeben.
- `type`: *Tabelle*müssen festgelegt werden.
- `direction`: *In* oder *out*festgelegt. 

Folgenden Beispiel *function.json* mithilfe einen Warteschlange Trigger eine Tabellenzeile gelesen. JSON stellt einen Partition hartcodierten Wert und gibt an, dass der Zeilenschlüssel warteschlangennachricht stammt.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Tabellen und-Ausgabe unterstützte Typen

Die `table` Bindung Serialisieren oder Deserialisieren von Objekten in Node.js oder C#-Funktionen. Die Objekte haben Eigenschaften RowKey und PartitionKey. 

In C# Funktionen können Sie auch auf die folgenden Typen binden:

* `T`wo `T` implementiert`ITableEntity`
* `IQueryable<T>`(nur Eingabe)
* `ICollector<T>`(nur Ausgabe)
* `IAsyncCollector<T>`(nur Ausgabe)

#### <a name="storage-tables-binding-scenarios"></a>Storage Tabellen Bindungsszenarios

Die tabellenbindung unterstützt die folgenden Szenarien:

* Lesen einer Zeile in einer C#- oder Knoten.

    Legen Sie `partitionKey` und `rowKey`. Die `filter` und `take` Eigenschaften werden in diesem Szenario nicht verwendet.

* Lesen Sie mehrere Zeilen in einer C#-Funktion.

    Die Runtime Funktionen bietet ein `IQueryable<T>` Objekt der Tabelle gebunden. Typ `T` ableiten müssen `TableEntity` oder `ITableEntity`. Die `partitionKey`, `rowKey`, `filter`, und `take` Eigenschaften wird nicht in diesem Szenario; Sie können die `IQueryable` Objekt gefiltert erforderlich. 

* Lesen Sie mehrere Zeilen in einer Knoten-Funktion.

    Legen Sie die `filter` und `take` Eigenschaften. Legen Sie nicht `partitionKey` oder `rowKey`.

* Schreiben Sie eine oder mehrere Zeilen in einer C#-Funktion.

    Die Runtime Funktionen bietet ein `ICollector<T>` oder `IAsyncCollector<T>` an die Tabelle gebunden, `T` gibt das Schema für die Entitäten, die Sie hinzufügen möchten. Normalerweise `T` abgeleitet `TableEntity` oder `ITableEntity`, muss aber nicht. Die `partitionKey`, `rowKey`, `filter`, und `take` Eigenschaften werden in diesem Szenario nicht verwendet.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Storage Tabellen Beispiel: Lesen eine einzelnen Tabellenentität in C# oder Knoten

Im folgende C#-Codebeispiel funktioniert mit der vorangehenden *function.json* Datei zuvor eine einzelne Tabellenentität gelesen. Warteschlangennachricht hat den Schlüsselwert der Zeile und Tabellenentität in der *run.csx* -Datei definierten eingelesen. Enthält die `PartitionKey` und `RowKey` Eigenschaften und nicht daraus `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Das folgende F# Codebeispiel funktioniert auch mit der vorangehenden *function.json* Datei eine einzelne Tabellenentität gelesen.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Codebeispiel von Knoten funktioniert auch mit der vorangehenden *function.json* Datei eine einzelne Tabellenentität gelesen.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Storage Tabellen Beispiel: mehrere Tabellenentitäten in C lesen# 

Die folgenden *function.json* und C# Codeentitäten Beispiel liest für Partitionsschlüssel in der warteschlangennachricht angegeben ist.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C#-Code wird ein Verweis auf Azure Storage SDK, damit der Entität vom Typ ableiten kann `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Speicher-Tabellen-Beispiel: c Tabellenentitäten erstellen# 

Im folgenden Beispiel wird *function.json* und *run.csx* veranschaulicht die Tabellenentitäten in C# schreiben.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Storage Tabellen Beispiel: Tabellenentitäten in F# erstellen#

Im folgenden Beispiel wird *function.json* und *run.fsx* die Tabellenentitäten in F# schreiben veranschaulicht.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Speicher-Tabellen-Beispiel: Erstellen einer Tabellenentität im Knoten

Im folgenden Beispiel wird *function.json* und *run.csx* zeigt eine Tabellenentität in Knoten schreiben.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
