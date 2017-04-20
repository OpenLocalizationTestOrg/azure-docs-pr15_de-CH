<properties 
    pageTitle="Sortieren mit Order By DocumentDB Daten | Microsoft Azure" 
    description="Lernen Sie mit ORDER BY in DocumentDB Abfragen in LINQ und SQL und Indexierungsrichtlinie ORDER BY-Abfragen an." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Sortieren von DocumentDB Daten mit Order By
Microsoft Azure DocumentDB unterstützt Abfragen mit SQL über JSON-Dokumente Dokumente. Abfrageergebnisse können mithilfe der ORDER BY-Klausel in SQL abfrageanweisungen bestellt werden.

Nach dem Lesen dieses Artikels werden Sie folgenden Fragen beantworten: 

- Wie wird mit Order By Abfragen?
- Wie wird konfigurieren eine Volltextindizierung Richtlinie für Order By?
- Was kommt als Nächstes?

[Beispiele](#samples) und [häufig gestellte Fragen](#faq) werden ebenfalls bereitgestellt.

Eine vollständige Referenz zu SQL-Abfragen finden Sie im [Lernprogramm DocumentDB Abfrage](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Abfragen mit Order By
Wie in ANSI-SQL zählen Sie nun optionale Order By-Klausel in SQL-Anweisungen bei DocumentDB Abfragen. Die Klausel kann eines optionalen Arguments der ASC/DESC um die Reihenfolge festzulegen, in der Ergebnisse abgerufen werden müssen. 

### <a name="ordering-using-sql"></a>Bestellung mit SQL
Dies ist z. B. eine Abfrage Top 10 Bücher in absteigender Reihenfolge der Titel abrufen. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Filtern mit SQL Sortierung
Bestellen Sie geschachtelte Eigenschaft in Dokumenten wie Books.ShippingDetails.Weight, und Sie können zusätzliche Filter in die WHERE-Klausel in Kombination mit Order By wie in diesem Beispiel:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Mit Hilfe der LINQ-Anbieter für .NET
Mit .NET SDK, Version 1.2.0 und höher können Sie auch die OrderBy() oder OrderByDescending()-Klausel in LINQ-Abfragen wie in diesem Beispiel:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB unterstützt mit einzelnen numerischen, Zeichenfolge oder boolesche Eigenschaft pro Abfrage mit zusätzlichen Abfrage bald bestellen. Siehe [Was kommt als Nächstes](#Whats_coming_next) für weitere Details.

## <a name="configure-an-indexing-policy-for-order-by"></a>Konfigurieren einer Volltextindizierung Richtlinie für Order By

Beachten Sie, dass DocumentDB unterstützt zwei Arten von Indizes Hash und Bereich für bestimmte Pfade von Eigenschaften, Datentypen (Zeichenfolgen/Zahlen) und unterschiedlich präzise Werte (maximale Genauigkeit oder einen Wert mit fester Genauigkeit) festgelegt werden können. DocumentDB Hash Indizierung standardmäßig verwendet, müssen Sie eine neue Auflistung mit einem benutzerdefinierten Indizierung mit Zahlen, Zeichenfolgen oder beides erstellen, um Order By verwenden. 

>[AZURE.NOTE] Zeichenfolge Bereich Indizes wurden am 7. Juli 2015 REST API Version 2015-06-03. Um Richtlinien für Order By mit Zeichenfolgen zu erstellen, müssen Sie SDK, Version 1.2.0 .NET SDK oder Version 1.1.0 Python, Node.js oder Java SDK verwenden.
>
>Vor REST-API-Version 2015-06-03 war die Standardrichtlinie Auflistung indizieren Hash für Zeichenfolgen und Zahlen. Dieser wurde Hash für Zeichenfolgen und Zahlen geändert. 

Weitere Informationen finden Sie unter [DocumentDB Indexierungsrichtlinien](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indizierung für Order By gegen alle Eigenschaften
Hier wie Sie erstellen eine Auflistung mit "Alle Range" Indizierung für Order By gegen alle numerischen oder Eigenschaften, die in JSON-Dokumente darin erscheinen. Hier überschreiben Sie Index Standardtyp für Zeichenfolgenwerte Bereich und der maximalen Genauigkeit (1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Beachten Sie, dass Order By nur Ergebnisse der Datentypen (Zeichenfolge und Zahl) zurückgibt, die mit einem RangeIndex indiziert werden. Beispielsweise haben Sie Indizierung Richtlinie hat nur RangeIndex auf Standard wird Order By gegen einen Pfad mit Zeichenfolgenwerten keine Dokumente zurück.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indizierung für eine einzelne Eigenschaft für Order By
Erstellen eine Auflistung mit Indizierung für Order By gegen nur die Title-Eigenschaft, die eine Zeichenfolge ist hier. Es gibt zwei Wege, eine für die Title-Eigenschaft ("Titel /?") und Bereich indizieren für jede Eigenschaft mit der Indizierung Standardschema, die Hash für Zeichenfolgen und Zahlen.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Beispiele
Betrachten Sie diese [Github Beispielprojekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) , das veranschaulicht, wie mit Order By, einschließlich Erstellen von Richtlinien Indizierung und paging mit Order By. Beispiele sind open Source und wir empfehlen Ihnen, ziehen mit einzureichen, die andere Entwickler DocumentDB profitieren. Finden Sie den [Beitrag Richtlinien](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) wie beitragen.  

## <a name="faq"></a>Häufig gestellte Fragen

**Was ist die erwartete anfordern Einheit (RU) Order By-Abfragen?**

Da Order By DocumentDB Index für Suchvorgänge verwendet werden die Anzahl der Order By-Abfragen belegten anforderungseinheiten entsprechenden Abfragen ohne Order By ähnelt. Wie alle anderen Vorgänge DocumentDB hängt die Anzahl der anforderungseinheiten Größen Formen Dokumente sowie die Komplexität der Abfrage. 


**Was ist die erwartete Indizierung Aufwand für Order By**

Der Indexdienst Speicheraufwand werden proportional zur Anzahl von Eigenschaften. Im schlimmsten Fall werden der Indizierungsaufwand 100 % der Daten. Besteht kein Unterschied Durchsatz (Anforderungseinheiten) Belastung zwischen Range-Order By indizieren und Standard-Hash-Indizierung.

**Wie wird Abfragen meine vorhandenen Daten in DocumentDB mit Order By?**

Um Abfrageergebnisse durch Reihenfolge sortieren, müssen Sie die Volltextindizierung Richtlinie der Auflistung einen Bereich Index Typ für die Eigenschaft zum Sortieren ändern. Siehe [indizieren Richtlinie ändern](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Was sind die aktuellen Grenzen Order By?**

Order By kann nur für eine Eigenschaft entweder numerisch angegeben werden oder Zeichenfolge Bereich wird mit der maximalen Genauigkeit (1) indiziert.

Sie können nicht die folgenden Schritte:
 
- Order By mit internen Eigenschaften wie Id, _rid, _self (demnächst verfügbar).
- Order By mit Eigenschaften aus einer dokumentinternen Verknüpfung (demnächst verfügbar).
- Mehrere Eigenschaften (demnächst verfügbar) bestellen.
- Order By mit Abfragen auf Datenbanken, Sammlungen, Benutzer, Berechtigungen oder Anlagen (demnächst verfügbar).
- Sortieren Sie mit berechneten Eigenschaften z. B. das Ergebnis eines Ausdrucks oder einer UDF/built-in Funktion.

Order By wird derzeit nicht für Cross-Partition Abfragen unterstützt bei Abfrage Explorer im Azure-Portal verwenden.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie eine Fehlermeldung, dass Order By nicht unterstützt wird, überprüfen Sie eine [SDK](documentdb-sdk-dotnet.md) -Version verwenden, die Order By unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

Gabel [Github Beispielprojekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) und Sortierung Ihrer Daten beginnen! 

## <a name="references"></a>Referenzen
* [DocumentDB Abfrage-Referenz](documentdb-sql-query.md)
* [DocumentDB Indizierung Policy Reference](documentdb-indexing-policies.md)
* [DocumentDB-SQL-Referenz](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order By-Beispiele](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

