<properties 
    pageTitle="DocumentDB Skalierung und Performance-Tests | Microsoft Azure" 
    description="Erfahren Sie, wie Skalierung und Performance-Tests mit Azure DocumentDB"
    keywords="Leistungstests"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Performance und Skalierung mit Azure DocumentDB
Testen der Leistung und Skalierung ist ein wichtiger Schritt bei der Anwendungsentwicklung. Viele Anwendungsmöglichkeiten die Datenbankebene wirkt sich deutlich auf die Leistung und Skalierung und ist daher eine wichtige Komponente von Leistungstest. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) ist für flexible Skalierung und vorhersagbare Performance und somit passt hervorragend für Applikationen, die leistungsstarke Datenbankebene. 

Dieser Artikel ist eine Referenz für Entwickler implementieren Leistung Testsammlungen für Arbeitslasten DocumentDB oder DocumentDB für hochleistungsfähige Anwendungsszenarien auswerten. Es konzentriert sich hauptsächlich auf isolierte Leistungstests der Datenbank aber auch best Practices für die Produktion.

Nach dem Lesen dieses Artikels werden Sie folgenden Fragen beantworten:   

- Wo finde ich eine Beispiel .NET-Clientanwendung für Leistungstests von Azure DocumentDB? 
- Wie erreiche ich hohen Durchsatz Ebenen mit Azure DocumentDB aus meiner Clientanwendung?

Zunächst mit Code Laden Sie das Projekt aus [DocumentDB Leistung testen](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [AZURE.NOTE] Diese Anwendung soll veranschaulichen best Practices für eine bessere Leistung aus DocumentDB mit einer geringen Anzahl von Clientcomputern extrahieren. Dies wurde nicht unbegrenzt skalieren maximaler Kapazität des Diensts zeigen.

Wenn Sie clientseitige Konfigurationsoptionen DocumentDB Leistung suchen, finden Sie unter [Tipps zum DocumentDB](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Führen Sie Leistungstests Anwendung
Die schnellste Möglichkeit zum Einstieg ist zum Kompilieren und Ausführen von .NET Beispiel, wie nachstehend beschrieben. Sie können auch den Quellcode und ähnliche Konfigurationen zum Client-Anwendung implementieren.

**Schritt 1:** Projekt aus [DocumentDB Leistung testen Beispiel](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)oder Verzweigung Github Repository.

**Schritt 2:** Ändern der Einstellungen für EndpointUrl, AuthorizationKey, CollectionThroughput und DocumentTemplate in App.config (optional).

> [AZURE.NOTE] Vor der Bereitstellung mit hohem Durchsatz, siehe [Seite Preise](https://azure.microsoft.com/pricing/details/documentdb/) , die Kosten pro Auflistung. DocumentDB Wechsel Speicher- und Durchsatz separat auf Stundenbasis, damit sparen Sie Kosten können durch Löschen oder verringern den Durchsatz des DocumentDB Sammlungen nach dem Testen.

**Schritt 3:** Kompilieren und Ausführen der Konsolenanwendung von der Befehlszeile aus. Folgende Ausgabe sollte angezeigt werden:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Schritt 4 (falls erforderlich):** Der Durchsatz gemeldet (RU/s) aus dem Tool sollte gleich oder höher als der bereitgestellte Durchsatz der Auflistung. Wenn nicht, DegreeOfParallelism in kleinen Schritten erhöhen, können Sie den Höchstwert erreichen. Durchsatz von der Clientanwendung hochebenen, hilft starten mehrere Instanzen der Anwendung auf demselben oder auf verschiedenen Computern in verschiedenen Instanzen bereitgestellten Grenzwert erreichen. Benötigen Sie Hilfe mit diesem Schritt, Schreiben Sie eine e-Mail an askdocdb@microsoft.com oder Support-Ticket.

Haben Sie die Anwendung ausführen, können Sie verschiedene [Richtlinien für Indizierung](documentdb-indexing-policies.md) und [Konsistenz](documentdb-consistency-levels.md) ihrer Auswirkung auf Durchsatz und Wartezeit verstehen versuchen. Sie können auch den Quellcode und ähnliche Konfigurationen eigene Testsammlungen oder Produktion zu implementieren.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben wir wie Performance und Skalierung mit DocumentDB eine .NET Konsole Anwendung ausführen können. Finden Sie die Links unten für Weitere Informationen zum Arbeiten mit DocumentDB.

* [DocumentDB Leistungsbeispiel](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Konfigurationsoptionen beim Client DocumentDB Leistung](documentdb-performance-tips.md)
* [Serverseitige Partitionierung in DocumentDB](documentdb-partition-data.md)
* [DocumentDB Sammlungen und Leistung](documentdb-performance-levels.md)
* [DocumentDB .NET SDK-Dokumentation auf MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET-Beispiele](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB Blog Tipps](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
