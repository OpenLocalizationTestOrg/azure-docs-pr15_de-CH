<properties
    pageTitle="Liste von Azure Speicherressourcen mit Microsoft Azure Storage-Clientbibliothek C++ | Microsoft Azure"
    description="Informationen Sie zum Angebot APIs in Microsoft Azure Storage-Clientbibliothek für C++ verwenden, um Container, Blobs, Queues, Tabellen und Entitäten aufzulisten."
    documentationCenter=".net"
    services="storage"
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

# <a name="list-azure-storage-resources-in-c"></a>Liste Azure Speicherressourcen in C++

Angebot Operationen sind für viele Entwicklungsszenarien mit Azure-Speicher. Dieser Artikel beschreibt, wie effizient Objekte in Azure Storage Angebot für C++ in Microsoft Azure Storage-Clientbibliothek bereitgestellten APIs verwenden.

>[AZURE.NOTE] Diese Anleitung richtet Azure Storage-Clientbibliothek für C++ 2.x über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](https://github.com/Azure/azure-storage-cpp).

Speicher-Clientbibliothek bietet eine Vielzahl von Methoden zur Liste oder Objekte in Azure-Speicher. Dieser Artikel behandelt die folgenden Szenarien:

-   Listencontainer in einem Konto
-   Liste Blobs in einem Container oder BLOB-virtuelle Verzeichnis
-   Liste von Warteschlangen in einem Konto
-   Tabellen in einem Konto
-   Abfrage von Entitäten in einer Tabelle

Jede dieser Methoden wird mit anderen Überladungen für verschiedene Szenarien angezeigt.

## <a name="asynchronous-versus-synchronous"></a>Asynchrone und synchrone

Da der Speicher-Clientbibliothek für C++ auf den [REST der C++-Bibliothek](https://github.com/Microsoft/cpprestsdk)basiert, wird grundsätzlich asynchrone Vorgänge mithilfe von [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html)unterstützt. Zum Beispiel:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Synchrone Vorgänge umschließen die entsprechenden asynchronen Vorgänge:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Arbeiten mit mehreren Threads Applikationen oder Services sollten die Verwendung asynchroner APIs direkt anstatt einen Thread Sync-APIs aufrufen, wodurch die Leistung erheblich beeinträchtigt.

## <a name="segmented-listing"></a>Segmentierte Angebot

Der Umfang der Cloud-Speicher erfordert segmentierten Angebot. Beispielsweise können Sie über eine Million Blobs in Azure Blob-Container oder eine Milliarde Entitäten in Azure-Tabelle. Diese sind nicht nur theoretische Angaben, aber Kunden Anwendungsfälle.

Daher ist es nicht empfehlenswert, alle Objekte in einer einzelnen Antwort. Stattdessen können Sie Objekte mit Paging auflisten. Angebot APIs jeweils eine *segmentierte* überladen.

Die Antwort für eine segmentierte Auflistungsvorgang enthält:

-   <i>_segment</i>, die für einen einzelnen Aufruf der Angebot-API zurückgegebenen Ergebnisse enthält.
-   *Continuation_token*, die an den nächsten Aufruf um die nächste Seite der Ergebnisse zu erhalten. Es sind keine weiteren Ergebnisse zurückgegeben, wird das Fortsetzungstoken null.

Beispielsweise kann der folgende Codeausschnitt ein normaler Aufruf alle Blobs in einem Container aufgelistet aussehen. Der Code ist in unseren [Beispielen](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp)verfügbar:

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Beachten Sie, dass die Anzahl der Ergebnisse auf einer Seite der Parameter *Max_results* in der Überladung jede API beispielsweise gesteuert werden kann:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Wenn Sie den *Max_results* -Parameter nicht angeben, wird der maximalen Standardwert 5.000 Ergebnisse auf einer einzelnen Seite zurückgegeben.

Beachten Sie, dass eine Abfrage Azure-Tabellenspeicher zurückgeben keine Datensätze oder weniger Datensätze als der Wert des Parameters *Max_results* angegeben, selbst wenn das Fortsetzungstoken nicht leer ist. Ein Grund könnte sein, dass die Abfrage nicht in 5 Sekunden abgeschlossen werden konnte. Als das Fortsetzungstoken nicht leer ist, sollten weiterhin und Code nicht die Größe des segmentergebnisse vorausgesetzt.

Empfohlene Codierungsmuster für die meisten Szenarios ist die explizite Fortschritt Angebot oder Abfragen und Reaktion des Dienstes auf jede Anforderung bietet segmentiert auflisten. Besonders für C++ Applikationen oder Services kann untergeordnete Liste Fortschritt Steuerelement Arbeitsspeicher und Leistung kontrollieren.

## <a name="greedy-listing"></a>Gierige Angebot

Frühere Versionen von Speicher-Clientbibliothek für C++ (Versionen 0.5.0 Vorschau und früheren Versionen) nicht segmentierten Angebot APIs für Tabellen und Warteschlangen, wie im folgenden Beispiel enthalten:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Diese Methoden wurden als Wrapper segmentierten APIs implementiert. Für jede Antwort segmentierten Angebot die Ergebnisse an einen Vektor angehängt und alle Ergebnisse zurückgegeben, nachdem die vollständige Container gescannt wurden.

Dieser Ansatz funktioniert das Speicherkonto oder Tabelle enthält eine kleine Anzahl von Objekten. Jedoch konnte mit einer Zunahme der Anzahl von Objekten erforderliche Speicher ohne Limit erhöhen, da alle Ergebnisse im Gedächtnis geblieben. Einem Auflistungsvorgang kann sehr lange dauern der Aufrufer keine Informationen über den Fortschritt hatte.

Diese gierigen Angebot APIs im SDK sind in C#, Java und JavaScript Node.js-Umgebung nicht vorhanden. Zur Vermeidung potenzieller Probleme dieser gierige APIs verwenden wir entfernt diese Version 0.6.0 Vorschau.

Dieser Code gierige APIs aufrufen:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Ändern Sie dann den Code, um die segmentierten Auflistung APIs verwenden:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Durch den *Max_results* -Parameter des Segments angeben, können Sie zwischen den Anfragen und Speicherverwendung Leistungsaspekte für Ihre Anwendung zu ausgleichen.

Außerdem, wenn Sie segmentierte Angebot APIs verwenden, aber die Daten in eine lokale Sammlung "gierige" Stil speichern, empfehlen wir auch gestalten Sie den Code zum Speichern von Daten in einer lokalen Auflistung Ebene sorgfältig behandeln.

## <a name="lazy-listing"></a>Verzögerte Angebot

Obwohl gierige Liste potenzieller Probleme ausgelöst, ist es praktisch, stehen nicht zu viele Objekte im Container.

Wenn Sie auch C# oder Oracle Java SDKs verwenden, sollten Sie vertraut mit Enumerable Programmiermodell sein bietet eine verzögerte Stil auflisten, die Daten an einem bestimmten Offset nur bei Bedarf abgerufen werden. In C++ bietet die Iterator Vorlage einen ähnlichen Ansatz.

Normale verzögerte Angebot API mit **List_blobs** so aussehen:

    list_blob_item_iterator list_blobs() const;

Normale Codeausschnitt, der das verzögerte Angebot Muster verwendet könnte wie folgt aussehen:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Beachten Sie, dass verzögerte Angebot nur im synchronen Modus.

Verglichen mit Gras, ruft verzögerte Angebot Daten nur bei Bedarf. Im Hintergrund Ruft Daten aus dem Azure-Speicher nur bei der nächste Iterator in nächsten verschiebt. Daher Speicher erfolgt mit einer begrenzten Größe und der Vorgang schneller ist.

Verzögerte Angebot APIs gehören in der Speicher-Clientbibliothek für C++ Version 2.2.0.

## <a name="conclusion"></a>Abschluss

In diesem Artikel erläutert unterschiedliche Konstruktorüberladungen aufgeführt, APIs für verschiedene Objekte in der Speicher-Clientbibliothek für C++. Zusammenfassung:

-   Asynchrone APIs werden dringend empfohlen, mehrere Threads Szenarien.
-   Segmentierte Angebot ist für die meisten Szenarien empfohlen.
-   Verzögerte Angebot wird in der Bibliothek als einfache Wrapper in synchronen Szenarien bereitgestellt.
-   Gierige Angebot nicht empfohlen und wurde aus der Bibliothek entfernt.

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Speicher und -Clientbibliothek für C++ finden Sie unter den folgenden Ressourcen.

-   [Verwendung von C++-BLOB-Speicher](storage-c-plus-plus-how-to-use-blobs.md)
-   [Verwendung von C++ Tabellenspeicher](storage-c-plus-plus-how-to-use-tables.md)
-   [Verwendung von C++ Warteschlangenspeicher](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure Storage-Clientbibliothek für C++ API-Dokumentation.](http://azure.github.io/azure-storage-cpp/)
-   [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
