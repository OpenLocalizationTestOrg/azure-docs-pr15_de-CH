<properties
    pageTitle="Wie Warteschlangenspeicher (C++) | Microsoft Azure"
    description="Erfahren Sie, wie mit der Warteschlange Speicherdienst in Azure. Beispiele sind in C++ geschrieben."
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

# <a name="how-to-use-queue-storage-from-c"></a>Verwendung von C++ Warteschlangenspeicher  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht
Dabei wird mithilfe den Azure-Warteschlange Speicherdienst Szenarien durchführen angezeigt. Die Beispiele sind in C++ geschrieben und der [Azure-Speicher-Clientbibliothek für C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Die Szenarios umfassen **Einfügen**, **einsehen**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**.

>[AZURE.NOTE] Dabei zielt Azure Storage-Clientbibliothek für C++ Version 1.0.0 und höher. Die empfohlene Version ist Speicher-Clientbibliothek 2.2.0, über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendung  
In diesem Handbuch Verwenden Sie Storage-Funktionen in eine C++-Anwendung ausgeführt werden kann.

Hierzu müssen Sie installieren Azure Storage-Clientbibliothek für C++ und Azure-Speicher in der Azure-Abonnement registrieren.

Installation von Azure Storage-Clientbibliothek für C++ können Sie die folgenden Methoden:

-   **Linux:** Anweisungen Sie auf [Azure Storage-Clientbibliothek C++-Infodatei](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
-   **Windows:** Klicken Sie in Visual Studio auf **Tools > NuGet Paket-Manager > Paket-Manager Konsole**. Geben Sie folgenden Befehl in die [Konsole NuGet Paket-Manager](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) und drücken Sie die **EINGABETASTE**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurieren Sie Ihre Anwendung auf Warteschlangenspeicher
Fügen Sie folgende Anweisung am Anfang der Datei C++ enthalten soll mit den Azure-Speicher-APIs auf Warteschlangen zugreifen:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Richten Sie eine Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und Anmeldeinformationen für den Zugriff auf Daten-Management-Services. Beim Ausführen in einer Clientanwendung müssen Sie Speicher Verbindungszeichenfolge im folgenden Format angeben, mit dem Namen Ihres Speicherkontos und Zugriffstaste Speicher für das Speicherkonto in [Azure-Portal](https://portal.azure.com) für *Kontoname* und *AccountKey* aufgeführt. Informationen zu Speicherkonten und Zugriffstasten anzeigen Sie [Zu Azure-Speicherkonten](storage-create-storage-account.md) Dieses Beispiel zeigt, wie Sie ein statisches Feld der Verbindungszeichenfolge zu deklarieren:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Zum Testen einer Anwendung in Ihren lokalen Computer können Sie Microsoft Azure [Speicheremulator](storage-use-emulator.md) verwenden, die mit dem [Azure SDK](https://azure.microsoft.com/downloads/)installiert ist. Der Speicheremulator ist ein Dienstprogramm, das die BLOB-Warteschlange und Tabelle Angebote in Azure auf Ihrem lokalen Entwicklungscomputer simuliert. Das folgende Beispiel zeigt, wie Sie ein statisches Feld Verbindungszeichenfolge die lokalen Speicheremulator zu deklarieren:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Um den Azure-Speicher-Emulator zu starten, wählen Sie die Schaltfläche **Start** oder drücken Sie **Windows** . Geben Sie **Azure Speicheremulator**, und wählen Sie aus der Liste der Programme **Microsoft Azure Speicheremulator** .

In den folgenden Beispielen davon ausgehen, dass Sie eine dieser beiden Methoden zu Speicher-Verbindungszeichenfolge verwendet haben.

## <a name="retrieve-your-connection-string"></a>Abrufen der Verbindungszeichenfolge
Die **Cloud_storage_account** -Klasse können Sie Kontoinformationen Speicher darstellen. Rufen Sie Ihre speicherkontoinformationen aus der Verbindungszeichenfolge Storage können Sie die **parse** -Methode.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen einer Warteschlange
Ein **Cloud_queue_client** -Objekt können Sie Referenzobjekte für Warteschlangen. Der folgende Code erstellt ein **Cloud_queue_client** -Objekt.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Verwenden Sie das **Cloud_queue_client** -Objekt zu einem Verweis auf die Warteschlange, die Sie verwenden möchten. Wenn vorhanden ist, können Sie die Warteschlange erstellen.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange
Zum Einfügen einer Nachricht in eine vorhandene Warteschlange zuerst erstellen Sie einen neuen **Cloud_queue_message**. Anschließend wird die **Add_message** -Methode aufrufen. Ein **Cloud_queue_message** kann eine Zeichenfolge oder **ein Bytearray** erstellt werden. Hier steht die Warteschlange erstellt (falls nicht vorhanden) und die Meldung "Hello, World":

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht
Sie können die Nachricht in einer Warteschlange einsehen, ohne durch Aufruf der **Peek_message** -Methode aus der Warteschlange entfernt.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange
Sie können den Inhalt einer Nachricht direkt in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie diese Funktion, zum Aktualisieren des Status der Arbeitsaufgabe. Der folgende Code Message Queue mit neuen Inhalten aktualisiert und sichtbarkeitstimeout erweitern Sie weitere 60 Sekunden festgelegt. Dies speichert den Zustand der Arbeit, die der Meldung zugeordnet und erhält der Client eine Minute weiterhin auf die Nachricht. Dieses Verfahren können Sie Warteschlangennachrichten, ohne am Anfang beginnen durch Hardware-oder Softwarefehler schlägt einen mehrstufigen Workflows verfolgen. Normalerweise würde eine Wiederholungsanzahl sowie halten, und wenn die Nachricht mehr als n Mal wiederholt wird, möchten Sie es löschen. Dies schützt vor einer Nachricht, die einen Anwendungsfehler jedem Trigger verarbeitet wird.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Gewusst wie: Warteschlange die nächste Nachricht
Code Warteschlange eine Nachricht aus einer Warteschlange in zwei Schritten. Wenn Sie **Get_message**aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Nachricht von **Get_message** wird zum Lesen von Nachrichten aus dieser Warteschlange Code unsichtbar. Um die Meldung aus der Warteschlange entfernt haben, müssen Sie auch **Delete_message**aufrufen. Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der Code ruft **Delete_message** rechts nach dem Verarbeiten der Meldung.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Gewusst wie: Nutzen Sie zusätzliche Optionen für die Warteschlange Nachrichten
Es gibt zwei Arten anpassen aus einer Warteschlange abrufen. Zunächst erhalten Sie Nachrichten (bis zu 32). Zweitens können längere oder kürzere unsichtbarkeit Timeout festlegen Code mehr oder weniger jede Nachricht vollständig zu ermöglichen. Das folgende Codebeispiel verwendet die **Get_messages** -Methode zu 20 Nachrichten aufrufen. Anschließend verarbeitet jede Nachricht mit einer **for** -Schleife. Außerdem wird das unsichtbarkeit Zeitlimit fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass 5 Minuten für alle Nachrichten zum gleichen Zeitpunkt nach 5 Minuten übergeben, da der Aufruf von **Get_messages**, nicht gelöschte Nachrichten wieder sichtbar.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen die Warteschlangenlänge
Sie erhalten eine Schätzung der Anzahl von Nachrichten in einer Warteschlange. **Download_attributes** -Methode fordert der Warteschlangendienst Warteschlange Attribute, einschließlich der Anzahl der Nachrichten abzurufen. **Approximate_message_count** -Methode ruft die ungefähre Anzahl der Nachrichten in der Warteschlange.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen einer Warteschlange
Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten **Delete_queue_if_exists** Methode für das Warteschlangenobjekt.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr über Azure-Speicher.

-   [Verwendung von C++-BLOB-Speicher](storage-c-plus-plus-how-to-use-blobs.md)
-   [Verwendung von C++ Tabellenspeicher](storage-c-plus-plus-how-to-use-tables.md)
-   [Liste Azure Speicherressourcen in C++](storage-c-plus-plus-enumeration.md)
-   [Speicher-Clientbibliothek für C++-Referenz](http://azure.github.io/azure-storage-cpp)
-   [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)

