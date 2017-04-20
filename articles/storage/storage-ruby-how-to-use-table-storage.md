<properties
    pageTitle="Verwendung von Ruby Azure-Tabellenspeicher | Microsoft Azure"
    description="Strukturierte Daten in der Cloud Azure-Tabellenspeicher, einem NoSQL-Datenspeicher gespeichert."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor=""/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-ruby"></a>Wie Sie Azure-Tabellenspeicher Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die allgemeine Szenarien mit der Azure-Diensts. Die Beispiele wurden mithilfe der Ruby-API. Die fallen Szenarien **Erstellen und Löschen einer Tabelle einfügen und Abfragen von Entitäten in einer Tabelle**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-Anwendung erstellen

Weitere Informationen zum Erstellen einer Ruby-Anwendung finden Sie unter [Ruby on Rails-Anwendung auf einer Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).


## <a name="configure-your-application-to-access-storage"></a>Konfigurieren Sie Ihre Anwendung auf Speicher zugreifen

Um Azure-Speicher zu verwenden, müssen Sie downloaden und verwenden das Ruby Azure Paket Komfort Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems des Pakets zu verwenden

1. Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster Gem und Abhängigkeiten installieren **Gem Azure installieren** .

### <a name="import-the-package"></a>Paket importieren

Verwenden Sie einen Texteditor, fügen Sie Folgendes zu den Dateianfang Ruby Speicher verwendet werden soll:

    require "azure"

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindungs Azure-Speicher

Azure-Modul liest die Umgebungsvariablen **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_Zugang\_Schlüssel** für die Verbindung zu Ihrem Azure Storage-Konto erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen vor **Azure::TableService** mit dem folgenden Code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

Diese Werte aus einer klassischen oder Ressourcenmanager Speicherkonto in Azure-Portal zu erhalten:

1. [Azure-Portal](https://portal.azure.com)anmelden.
2. Navigieren Sie zu Speicher-Konto zu verwenden.
3. Klicken Sie im Blatt Einstellungen auf der rechten Seite auf **Zugriffstasten**.
4. In Access Keys Blade, das angezeigt wird, sehen Sie die Taste 1 und Taste 2. Sie können diese verwenden. 
5. Klicken Sie auf Kopieren, um den Schlüssel in die Zwischenablage kopieren. 

Diese Werte aus einem klassischen Speicherkonto im klassischen Azure-Portal zu erhalten:

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Navigieren Sie zu Speicher-Konto zu verwenden.
3. Klicken Sie am unteren Rand des Navigationsbereichs **ZUGRIFFSTASTEN verwalten** .
4. Im Popup-Dialogfeld sehen Sie Storage-Kontoname, primäre Schlüssel und sekundäre Taste. Zugriffstaste können Sie entweder der primären oder der sekundären. 
5. Klicken Sie auf Kopieren, um den Schlüssel in die Zwischenablage kopieren.

## <a name="create-a-table"></a>Erstellen einer Tabelle

Das **Azure::TableService** -Objekt können Sie Elemente mit Tabellen arbeiten. Verwenden Sie zum Erstellen einer Tabelle die **Erstellen\_table()** Methode. Im folgende Beispiel erstellt eine Tabelle oder drucken Sie den Fehler, wenn vorhanden.

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Um ein Element hinzuzufügen, zunächst ein Hashobjekt, das die Eigenschaften der Entität definiert. Beachten Sie, dass für jede Entität ein **PartitionKey** und **RowKey**angeben müssen. Diese sind eindeutige Bezeichner von Entitäten und Werte, die schneller als andere Eigenschaften abgefragt werden können. Azure-Speicher verwendet **PartitionKey** automatisch der Tabelle Entitäten über viele Speicherknoten verteilen. Entitäten mit der gleichen **PartitionKey** befinden sich auf demselben Knoten. **RowKey** ist die eindeutige ID der Entität innerhalb der Partition, der er angehört.

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Es gibt mehrere Methoden zum Aktualisieren einer vorhandenen Entität:

* **Aktualisieren\_entity():** Aktualisieren einer vorhandenen Entität ersetzt.
* **Merge\_entity():** Aktualisiert eine vorhandene Entität neue Eigenschaftswerte in die vorhandene Entität zusammengeführt.
* **Einfügen\_oder\_Merge\_entity():** Aktualisiert eine vorhandene Entität ersetzt. Wenn keine Entität vorhanden ist, wird eine neue eingefügt:
* **Einfügen\_oder\_ersetzen\_entity():** Aktualisiert eine vorhandene Entität neue Eigenschaftswerte in die vorhandene Entität zusammengeführt. Wenn keine Entität vorhanden ist, wird eine neue eingefügt.

Das folgende Beispiel veranschaulicht eine Entität mit aktualisieren **update\_entity()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

Mit **update\_entity()** und **Merge\_entity()**, wenn die Entität, die Sie aktualisieren dann der Aktualisierungsvorgang nicht existiert. Daher möchten Sie speichern eine Entität unabhängig davon, ob er bereits vorhanden ist, verwenden Sie stattdessen **Einfügen\_oder\_ersetzen\_entity()** oder **Einfügen\_oder\_Merge\_entity()**.

## <a name="work-with-groups-of-entities"></a>Arbeiten Sie mit Entitäten

Manchmal ist es sinnvoll, mehrere Operationen zusammen in einem Batch darauf atomaren Verarbeitung durch den Server zu senden. Dazu zunächst ein **Batch** -Objekt erstellen und dann die **Ausführen\_batch()** **TableService**Methode. Das folgende Beispiel veranschaulicht zwei Entitäten mit RowKey 2 und 3 in einem Batch übermitteln. Beachten Sie, dass es nur für Entitäten mit dem gleichen PartitionKey funktioniert.

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## <a name="query-for-an-entity"></a>Abfrage für eine Entität

Um eine Entität in einer Tabelle abzufragen, verwenden die **abrufen\_entity()** -Methode übergeben den Namen der Tabelle **PartitionKey** und **RowKey**.

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## <a name="query-a-set-of-entities"></a>Eine Reihe von Entitäten Abfragen

Abfrage mehrere Entitäten in einer Tabelle, einer Abfrage Hash-Objekt erstellt und die **Abfrage\_entities()** Methode. Das folgende Beispiel veranschaulicht das Abrufen aller Entitäten mit demselben **PartitionKey**:

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Wenn die Ergebnismenge ist zu groß für eine einzelne Abfrage zurückzugebenden ein Fortsetzungstoken zurückgegeben, mit nachfolgende Seiten abzurufen.

## <a name="query-a-subset-of-entity-properties"></a>Abfrage einer Teilmenge von Entitätseigenschaften

Eine Abfrage in eine Tabelle kann nur wenige Eigenschaften einer Entität abrufen. Diese Technik namens "Projektion" Bandbreite reduziert und abfrageleistung, insbesondere bei großen Unternehmen verbessern kann. Verwenden der select-Klausel, und übergeben Sie die Namen der Eigenschaften, die an den Client übermittelt werden sollen.

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## <a name="delete-an-entity"></a>Löschen einer Entität

Um ein Element zu löschen, verwenden Sie die **Löschen\_entity()** Methode. Sie müssen den Namen der Tabelle übergeben die Entität, PartitionKey und RowKey der Entität enthält.

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## <a name="delete-a-table"></a>Löschen einer Tabelle

Um eine Tabelle zu löschen, verwenden Sie die **Löschen\_table()** -Methode und übergeben den Namen der Tabelle, die Sie löschen möchten.

        azure_table_service.delete_table("testtable")

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu komplexeren Speicheraufgaben folgen Sie diesen Links:

- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) Repository auf GitHub
