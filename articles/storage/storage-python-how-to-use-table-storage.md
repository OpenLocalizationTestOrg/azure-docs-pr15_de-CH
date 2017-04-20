<properties
    pageTitle="Verwendung von Python Tabellenspeicher | Microsoft Azure"
    description="Strukturierte Daten in der Cloud Azure-Tabellenspeicher, einem NoSQL-Datenspeicher gespeichert."
    services="storage"
    documentationCenter="python"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-python"></a>Verwendung von Python Tabellenspeicher

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die Szenarien mithilfe von Azure Table Storage Service ausführen. Die Proben sind in Python geschrieben und verwenden [Microsoft Azure Storage SDK für Python]. Abgedeckte Szenarien erstellen und Löschen einer Tabelle neben Einfügen und Abfragen von Entitäten in einer Tabelle.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-table"></a>Erstellen einer Tabelle

Das **TableService** -Objekt können Sie mit Tabelle arbeiten. Der folgende Code erstellt ein **TableService** -Objekt. Fügen Sie den Code im oberen Bereich einer Python-Datei, in der Sie programmgesteuert Azure-Speicher zugreifen möchten:

    from azure.storage.table import TableService, Entity

Der folgende Code erstellt ein **TableService** -Objekt mit dem Storage-Konto und Schlüssel.  Ersetzen Sie "Mein Konto" und "Mykey" mit Ihren Benutzernamen und Schlüssel.

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um ein Element hinzuzufügen, müssen Sie zuerst erstellen Sie ein Wörterbuch oder Entität, die die Entität Eigenschaftennamen und-Werte definiert. Beachten Sie, dass für jede Einheit **PartitionKey** und **RowKey**angeben müssen. Der eindeutige Bezeichner der Entitäten sind. Sie können Abfragen mit diesen Werten viel schneller als andere Eigenschaften Abfragen können. Das System verwendet **PartitionKey** Tabellenentitäten über viele Speicherknoten automatisch verteilen.
Entitäten, die den gleichen **PartitionKey** befinden sich auf demselben Knoten. **RowKey** ist die eindeutige ID der Entität innerhalb der Partition, der er angehört.

Um der Tabelle eine Entität hinzuzufügen, übergeben Sie ein Wörterbuchobjekt der **Einfügen\_Entität** Methode.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

Sie können auch eine Instanz **der Entitätsklasse** übergeben der **Einfügen\_Entität** Methode.

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Dieser Code zeigt, wie die alte Version einer vorhandenen Entität durch eine aktualisierte Version zu ersetzen.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

Wenn die Entität, die aktualisiert wird, nicht vorhanden ist, wird der Update-Vorgang fehl. Möchten Sie eine Entität unabhängig davon, ob es vorher zu speichern, verwenden Sie **Einfügen\_oder\_Replace_entity**.
Im folgenden Beispiel wird der erste Aufruf vorhandene Entität ersetzt. Der zweite Aufruf fügt eine neue Entität seit keine Entität mit dem angegebenen **PartitionKey** und **RowKey** in der Tabelle vorhanden.

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '3', 'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task, content_type='application/atom+xml')

## <a name="change-a-group-of-entities"></a>Eine Gruppe von Elementen ändern

Manchmal ist es sinnvoll, mehrere Operationen zusammen in einem Batch darauf atomaren Verarbeitung durch den Server zu senden. Dazu verwenden Sie die **TableBatch** -Klasse. Wenn Sie Stapel senden möchten, rufen Sie **Commit\_Stapel**. Beachten Sie, dass alle Entitäten in der gleichen Partition sein müssen, um als Batch geändert werden. Im folgenden Beispiel addiert zwei Entitäten in einem Batch.

    from azure.storage.table import TableBatch
    batch = TableBatch()
    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    batch.insert_entity(task10)
    batch.insert_entity(task11)
    table_service.commit_batch('tasktable', batch)

Batches können auch mit der Kontext-Manager-Syntax verwendet werden:

    task12 = {'PartitionKey': 'tasksSeattle', 'RowKey': '12', 'description' : 'Go grocery shopping', 'priority' : 400}
    task13 = {'PartitionKey': 'tasksSeattle', 'RowKey': '13', 'description' : 'Clean the bathroom', 'priority' : 100}

    with table_service.batch('tasktable') as batch:
        batch.insert_entity(task12)
        batch.insert_entity(task13)


## <a name="query-for-an-entity"></a>Abfrage für eine Entität

Um eine Entität in einer Tabelle abzufragen, verwenden die **abrufen\_Entität** Methode **PartitionKey** und **RowKey**.

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## <a name="query-a-set-of-entities"></a>Eine Reihe von Entitäten Abfragen

Dieses Beispiel sucht alle Aufgaben in Seattle basierend auf **PartitionKey**.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## <a name="query-a-subset-of-entity-properties"></a>Abfrage einer Teilmenge von Entitätseigenschaften

Eine Abfrage in eine Tabelle kann nur wenige Eigenschaften einer Entität abrufen.
Diese Technik *Projektion*genannt Bandbreite reduziert und abfrageleistung, insbesondere bei großen Unternehmen verbessern kann. **Wählen Sie** Parameter verwenden Sie, und übergeben Sie die Namen der Eigenschaften, die an den Client übermittelt werden soll.

Die Abfrage im folgenden Code gibt die Beschreibung der Entitäten in der Tabelle.

[AZURE.NOTE] Der folgende Codeausschnitt funktioniert nur in Verbindung mit dem Cloud-Speicherservice. Dies ist von der Speicheremulator nicht unterstützt.

    tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
    for task in tasks:
        print(task.description)

## <a name="delete-an-entity"></a>Löschen einer Entität

Sie können eine Entität mit der Partition und Zeile löschen.

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## <a name="delete-a-table"></a>Löschen einer Tabelle

Der folgende Code Löscht eine Tabelle aus ein Speicherkonto.

    table_service.delete_table('tasktable')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Tabellenspeicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherdienste REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure Storage SDK für Python]

[Azure Storage-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK für Python]: https://github.com/Azure/azure-storage-python
