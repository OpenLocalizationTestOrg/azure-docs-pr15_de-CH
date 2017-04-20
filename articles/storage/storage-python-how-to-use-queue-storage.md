<properties
    pageTitle="Verwendung von Python Warteschlangenspeicher | Microsoft Azure"
    description="Informationen Sie zum Verwenden der Azure-Warteschlangendienst Python erstellen und Löschen von Warteschlangen fügen Sie ein, abrufen Sie und löschen."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Verwendung von Python Warteschlangenspeicher

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die allgemeine Szenarien mit der Speicherdienst Azure-Warteschlange. Die Proben sind in Python geschrieben und verwenden [Microsoft Azure Storage SDK für Python]. Die Szenarios umfassen **Einfügen**, **einsehen**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**. Weitere Informationen zu Warteschlangen finden Sie im Abschnitt [nächste Schritte].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen einer Warteschlange

Das **QueueService** -Objekt können Sie mit Warteschlangen arbeiten. Der folgende Code erstellt ein **QueueService** -Objekt. Fügen Sie Folgendes am oberen Rand eine Python-Datei in der Azure-Speicher programmgesteuert zugreifen möchten:

    from azure.storage.queue import QueueService

Der folgende Code erstellt ein **QueueService** Objekt Konto und Speicherschlüssel verwenden. Ersetzen Sie "Mein Konto" und "Mykey" mit Ihren Benutzernamen und Schlüssel.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange

Verwenden Sie zum Einfügen einer Nachricht in eine Warteschlange der **setzen\_Nachricht** Methode, um eine neue Nachricht erstellen und an die Warteschlange hinzufügen.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Die Nachricht in einer Warteschlange einsehen, ohne sie aus der Warteschlange durch Aufrufen der **Blick\_Nachrichten** Methode. Standardmäßig **Blick\_Nachrichten** sieht eine Nachricht.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Gewusst wie: Entfernen von Nachrichten

Der Code entfernt eine Meldung in zwei Schritten aus der Warteschlange. Beim Aufruf von **abrufen\_Nachrichten**, erhalten Sie die nächste Meldung in einer Warteschlange standardmäßig. Eine Nachricht von **abrufen\_Nachrichten** Lesen von Nachrichten aus dieser Warteschlange Code unsichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden ausgeblendet. Abschließend die Nachricht aus der Warteschlange entfernt, müssen Sie auch aufrufen **Löschen\_Nachricht**. Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der Code ruft **Löschen\_Nachricht** direkt nach dem Verarbeiten der Meldung.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Es gibt zwei Arten anpassen aus einer Warteschlange abrufen.
Zunächst erhalten Sie Nachrichten (bis zu 32). Zweitens können längere oder kürzere unsichtbarkeit Timeout festlegen Code mehr oder weniger jede Nachricht vollständig zu ermöglichen. Im folgenden Codebeispiel wird mithilfe der **abrufen\_Nachrichten** -Methode 16 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer for-Schleife. Außerdem wird das unsichtbarkeit Zeitlimit fünf Minuten für jede Nachricht festgelegt.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht direkt in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie diese Funktion, zum Aktualisieren des Status der Arbeitsaufgabe. Der Code unten verwendet das **Aktualisieren\_Nachricht** Methode, um eine Nachricht zu aktualisieren. Sichtbarkeitstimeout festgelegt ist 0, d. h. die Meldung sofort und der Inhalt wird aktualisiert.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen die Warteschlangenlänge

Sie erhalten eine Schätzung der Anzahl von Nachrichten in einer Warteschlange. Die **abrufen\_Warteschlange\_Metadaten** -Methode fragt der Warteschlangendienst Metadaten über die Warteschlange und **Approximate_message_count**zurückgegeben. Das Ergebnis ist nur ungefähre, da Nachrichten können hinzugefügt oder entfernt, nachdem der Warteschlangendienst auf die Anforderung reagiert.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen eine Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten der **Löschen\_Warteschlange** Methode.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

- [Python-Entwicklercenter](/develop/python/)
- [Azure-Speicherdienste REST-API](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-Speicher-Teamblog]
- [Microsoft Azure Storage SDK für Python]

[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK für Python]: https://github.com/Azure/azure-storage-python
