<properties 
    pageTitle="NoSQL Python-Beispiele DocumentDB | Microsoft Azure" 
    description="Verwendungsbeispielen Sie NoSQL Python auf Github für häufige Aufgaben in DocumentDB einschließlich CRUD-Operationen für JSON-Dokumente NoSQL-Datenbanken." 
    keywords="Python-Beispiele"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>DocumentDB Python-Beispiele

> [AZURE.SELECTOR]
- [Beispiele für .NET](documentdb-dotnet-samples.md)
- [Node.js-Beispiele](documentdb-nodejs-samples.md)
- [Python-Beispiele](documentdb-python-samples.md)
- [Azure Code Sample Gallery](https://azure.microsoft.com/documentation/samples/?service=documentdb)

[Azure Documentdb Python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub Repository gehören Beispielprojektmappen, die CRUD-Operationen und andere gemeinsame Vorgänge Azure DocumentDB Ressourcen ausführen. Dieser Artikel enthält:

- Links zu den Aufgaben in jedem Beispiel Python-Projektdateien. 
- Links zu der zugehörigen API auf Inhalt verweisen.

**Erforderliche Komponenten**

1. Sie benötigen ein Azure-Konto diese Python-Beispiele verwenden:
    - [Ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/)können: Sie erhalten ein Guthaben können Sie ausprobieren, bezahlte Azure Services und sogar nachdem sie verwendet bis hält das Konto verwenden kostenlosen Azure Services wie Websites. Kreditkarte wird nicht belastet, sofern explizit ändern und Fragen belastet.
   - [Visual Studio-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)können: der Visual Studio-Abonnement erhalten Sie Credits monatlich für bezahlte Azure Services verwendet werden können.
2. Sie benötigen außerdem die [Python-SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Jedes Beispiel ist unabhängig und es sich selbst bereinigt. Die Beispiele stellen so mehrere Aufrufe von [Document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). 1 Stunde pro Tier Leistung der Auflistung erstellt werden jedes Mal, wenn Ihr Abonnement dies abgerechnet. 

## <a name="database-examples"></a>Datenbankbeispiele

Die [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) -Datei des Projekts [Datenbank-Management](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) veranschaulicht die folgenden Aufgaben.

Aufgabe | API-Referenz
--- | ---
[Erstellen einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [Document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Ein Konto für eine Datenbank Abfragen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [Document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lesen einer Datenbank nach Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [Document_client. Schreibgeschützte](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Liste Datenbanken für ein Konto](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [Document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Löschen einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [Document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Beispiele für die Auflistung 

Die [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) -Datei des Projekts [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) veranschaulicht die folgenden Aufgaben ausführen.

Aufgabe | API-Referenz
--- | ---
[Erstellen einer Auflistung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [Document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Eine Liste aller Sammlungen in einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [Document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Abrufen einer Auflistung von Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [Document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Abrufen einer Auflistung Leistungsebene](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Ändern einer Auflistung Leistungsebene](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [Document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Löschen einer Sammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [Document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
