<properties 
    pageTitle="Verwendung von Ruby Warteschlangenspeicher | Microsoft Azure" 
    description="Informationen Sie zum Verwenden des Azure-warteschlangendiensts erstellen und Löschen von Warteschlangen fügen Sie ein, abrufen Sie und löschen. Beispiele in Ruby geschrieben." 
    services="storage" 
    documentationCenter="ruby" 
    authors="robinsh" 
    manager="carmonm" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-ruby"></a>Verwendung von Ruby Warteschlangenspeicher

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die Szenarien mit Microsoft Azure Queue Storage-Dienst ausführen. Die Beispiele wurden mithilfe von Ruby Azure-API.
Die Szenarios umfassen **Einfügen**, **einsehen**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-Anwendung erstellen

Ruby-Anwendung zu erstellen. Eine Anleitung finden Sie in [Ruby on Rails-Anwendung auf einer Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Der Anwendung Zugriff

Um Azure-Speicher zu verwenden, müssen Sie downloaden und Ruby Azure Paket Komfort Bibliotheken enthält, die mit Storage REST-Dienste kommunizieren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems des Pakets zu verwenden

1. Verwenden Sie eine Befehlszeilenschnittstelle **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster Gem und Abhängigkeiten installieren "Gem installieren Azure".

### <a name="import-the-package"></a>Paket importieren

Verwenden Sie einen Texteditor, fügen Sie Folgendes zu den Dateianfang Ruby Speicher verwendet werden soll:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Ein Azure-Speicher Verbindung

Azure-Modul liest die Umgebungsvariablen **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS_KEY** für die Verbindung zu Ihrem Konto Azure-Speicher erforderlich. Wenn diese Umgebungsvariablen nicht festgelegt werden, geben Sie die Kontoinformationen vor **Azure::QueueService** mit dem folgenden Code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your Azure storage access key>"

 
Diese Werte aus einer klassischen oder Ressourcenmanager Speicherkonto in Azure-Portal zu erhalten:

1. Auf der [Azure-Portal](https://portal.azure.com)anmelden.
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

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen einer Warteschlange

Der folgende Code erstellt ein **Azure::QueueService** -Objekt, das mit Warteschlangen arbeiten ermöglicht.

    azure_queue_service = Azure::QueueService.new

Verwenden Sie die **create_queue()** -Methode zum Erstellen einer Warteschlange mit dem angegebenen Namen.

    begin
      azure_queue_service.create_queue("test-queue")
    rescue
      puts $!
    end

## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange

Zum Einfügen einer Nachricht in einer Warteschlange mithilfe der **create_message()** -Methode eine neue Nachricht erstellen und an die Warteschlange hinzufügen.

    azure_queue_service.create_message("test-queue", "test message")

## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Die Nachricht in einer Warteschlange einsehen, ohne sie aus der Warteschlange durch Aufrufen der **Blick\_messages()** Methode. Standardmäßig **Blick\_messages()** sieht eine Nachricht. Sie können auch angeben, wie viele Nachrichten einsehen möchten.

    result = azure_queue_service.peek_messages("test-queue",
      {:number_of_messages => 10})

## <a name="how-to-dequeue-the-next-message"></a>Gewusst wie: Abrufen der nächsten Nachricht

Sie können eine Nachricht aus einer Warteschlange in zwei Schritten entfernen.

1. Beim Aufruf von **Liste\_messages()**, erhalten Sie die nächste Meldung in einer Warteschlange standardmäßig. Sie können auch angeben, wie viele Nachrichten abrufen möchten. Fehlermeldungen aus **Liste\_messages()** Lesen von Nachrichten aus dieser Warteschlange Code unsichtbar. Sie übergeben die sichtbarkeitstimeout in Sekunden als Parameter.

2. Um die Meldung aus der Warteschlange entfernt haben, müssen Sie auch **delete_message()**aufrufen.

Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der Code ruft **Löschen\_message()** direkt nach dem Verarbeiten der Meldung.

    messages = azure_queue_service.list_messages("test-queue", 30)
    azure_queue_service.delete_message("test-queue", 
      messages[0].id, messages[0].pop_receipt)

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht direkt in der Warteschlange ändern. Der folgende Code verwendet die **update_message()** -Methode eine Nachricht zu aktualisieren. Die Methode liefert ein Tupel enthält den pop Zugang Message Queue und einen UTC-Zeitwert, bei der die Meldung in der Warteschlange angezeigt werden.

    message = azure_queue_service.list_messages("test-queue", 30)
    pop_receipt, time_next_visible = azure_queue_service.update_message(
      "test-queue", message.id, message.pop_receipt, "updated test message", 
      30)

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Gewusst wie: Zusätzliche Optionen für Warteschlangen Nachrichten

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen.

1. Sie erhalten ein Nachrichtenbatch.

2. Einen Timeout längere oder kürzere unsichtbarkeit lassen Code ermöglichen, mehr oder weniger jede Nachricht vollständig verarbeitet werden.

Im folgenden Codebeispiel wird die **Liste\_messages()** -Methode 15 Nachrichten aufrufen. Druckt und löscht jede Nachricht. Außerdem wird das unsichtbarkeit Zeitlimit fünf Minuten für jede Nachricht festgelegt.

    azure_queue_service.list_messages("test-queue", 300
      {:number_of_messages => 15}).each do |m|
      puts m.message_text
      azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
    end

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen die Warteschlangenlänge

Sie erhalten eine Schätzung der Anzahl der Nachrichten in der Warteschlange. Die **abrufen\_Warteschlange\_metadata()** -Methode fragt der Warteschlangendienst ungefähre Nachrichtenanzahl und Metadaten der Warteschlange zurückgegeben.

    message_count, metadata = azure_queue_service.get_queue_metadata(
      "test-queue")

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen eine Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten der **Löschen\_queue()** Methode für das Warteschlangenobjekt.

    azure_queue_service.delete_queue("test-queue")

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexer Speicheraufgaben erfahren.

- Besuchen Sie den [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- Besuchen Sie GitHub Repository [Azure SDK Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby)

Vergleich der Azure-Warteschlangendienst erläutert in diesem Artikel und in Artikel [wie Service Bus Warteschlangen](/develop/ruby/how-to-guides/service-bus-queues/) beschrieben Azure Service Bus Warteschlangen finden Sie unter [Azure-Warteschlangen und Service Bus Warteschlangen - verglichen und Contrasted](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
