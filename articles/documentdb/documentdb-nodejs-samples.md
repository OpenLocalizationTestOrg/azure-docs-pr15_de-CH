<properties
    pageTitle="NoSQL Node.js-Beispiele DocumentDB | Microsoft Azure"
    description="Verwendungsbeispielen Sie NoSQL Node.js auf Github für häufige Aufgaben in DocumentDB einschließlich CRUD-Operationen für JSON-Dokumente NoSQL-Datenbanken."
    keywords="Node.js-Beispiele"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>DocumentDB Node.js-Beispiele

> [AZURE.SELECTOR]
- [Beispiele für .NET](documentdb-dotnet-samples.md)
- [Node.js-Beispiele](documentdb-nodejs-samples.md)
- [Python-Beispiele](documentdb-python-samples.md)
- [Azure Code Sample Gallery](https://azure.microsoft.com/documentation/samples/?service=documentdb)

[Azure-Documentdb-Nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub Repository gehören Beispielprojektmappen, die CRUD-Operationen und andere gemeinsame Vorgänge Azure DocumentDB Ressourcen ausführen. Dieser Artikel enthält:

- Links zu den Aufgaben aller Node.js-Beispiel Projektdateien.
- Links zu der zugehörigen API auf Inhalt verweisen.

**Erforderliche Komponenten**

1. Sie benötigen ein Azure-Konto diese Node.js-Beispiele verwenden:
    - [Ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/)können: Sie erhalten ein Guthaben können Sie ausprobieren, bezahlte Azure Services und sogar nachdem sie verwendet bis hält das Konto verwenden kostenlosen Azure Services wie Websites. Kreditkarte wird nicht belastet, sofern explizit ändern und Fragen belastet.
   - [Visual Studio-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)können: der Visual Studio-Abonnement erhalten Sie Credits monatlich für bezahlte Azure Services verwendet werden können.
2. Sie benötigen außerdem [Node.js-SDK](documentdb-sdk-node.md).

    > [AZURE.NOTE] Jedes Beispiel ist unabhängig und es sich selbst bereinigt. Die Beispiele stellen so mehrere Aufrufe von [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). 1 Stunde pro Tier Leistung der Auflistung erstellt werden jedes Mal, wenn Ihr Abonnement dies abgerechnet.

## <a name="database-examples"></a>Datenbankbeispiele

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) [Datenbank-Management](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) -Projekt veranschaulicht die folgenden Aufgaben.

Aufgabe | API-Referenz
--- | ---
[Erstellen einer Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Ein Konto für eine Datenbank Abfragen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Lesen einer Datenbank nach Id](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Liste Datenbanken für ein Konto](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Löschen einer Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Beispiele für die Auflistung

[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) -Datei des Projekts [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) veranschaulicht die folgenden Aufgaben ausführen.

Aufgabe | API-Referenz
--- | ---
[Erstellen einer Auflistung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Eine Liste aller Sammlungen in einer Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Abrufen einer Auflistung von _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Abrufen einer Auflistung von Id](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Abrufen einer Auflistung Leistungsebene](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Ändern einer Auflistung Leistungsebene](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Löschen einer Sammlung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Dokument-Beispiele

[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) -Datei des Projekts [Dokumentenverwaltung](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) veranschaulicht die folgenden Aufgaben ausführen.

Aufgabe | API-Referenz
--- | ---
[Dokumente erstellen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Lesen Sie das Dokument für eine Auflistung feed](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Lesen eines Dokuments nach ID](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Dokument lesen, nur wenn Dokument geändert hat](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Abfrage für Dokumente](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Ersetzen eines Dokuments](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Dokument mit bedingten ETag ersetzen](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Löschen eines Dokuments](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Beispiele für Indizierung

[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) -Datei des Projekts [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) veranschaulicht die folgenden Aufgaben ausführen.

Aufgabe | API-Referenz
--- | ---
Erstellen Sie eine Auflistung mit Standard-Indizierung | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Ein bestimmtes Dokument manuell index](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [IndexingDirective: "include"](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Ein bestimmtes Dokument manuell aus dem Index ausgeschlossen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Verzögerte Indizierung für Massenimport verwenden oder schwere Sammlungen lesen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Bestimmte Pfade eines Dokuments in Index einschließen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Indizierung bestimmter Pfade ausschließen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Ermöglichen Sie einen Scan Zeichenfolge Pfad während eines Vorgangs Bereich](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Erstellen eines Indexes Bereich Pfad: Zeichenfolge](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Eine Auflistung mit Standard-IndexPolicy erstellen und dann diese online aktualisieren](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Weitere Informationen zur Indizierung finden Sie unter [DocumentDB Indexierungsrichtlinien](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Beispiele für serverseitige Programmierung

[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) -Datei des Projekts [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) veranschaulicht die folgenden Aufgaben ausführen.

Aufgabe | API-Referenz
--- | ---
[Erstellen einer gespeicherten Prozedur](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Ausführen einer gespeicherten Prozedur](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Weitere Informationen über serverseitige Programmierung finden Sie unter [DocumentDB serverseitige Programmierung: gespeicherte Prozeduren, Datenbanktrigger und UDF-Dateien](documentdb-programming.md).

## <a name="partitioning-examples"></a>Beispiele für die Partitionierung

[App.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) -Datei des Projekts [Partitioning](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) veranschaulicht die folgenden Aufgaben ausführen.

Aufgabe | API-Referenz
--- | ---
[Verwenden einer HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Weitere Informationen zum Partitionieren von Daten in DocumentDB finden Sie unter [Partition und Skalierung von Daten in DocumentDB](documentdb-partition-data.md).
