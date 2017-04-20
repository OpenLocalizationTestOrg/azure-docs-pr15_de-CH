<properties
    pageTitle="Hochladen von Daten in Azure suchen | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Informationen Sie zum Hochladen von Daten auf einen Index in Azure suchen."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Daten nach Azure hochladen
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Daten-upload Modelle in Azure suchen
Es gibt zwei Möglichkeiten Azure Suchindex mit Daten aufgefüllt. Die erste Option ist Ihre Daten manuell in Azure Search [REST-API](search-import-data-rest-api.md) oder [.NET SDK](search-import-data-dotnet.md)mit drücken. Die zweite Option [unterstützte Datenquelle zeigen](search-indexer-overview.md) Ihre Azure-Suchindex und Azure Suche automatisch die Daten in den Suchdienst ziehen kann.

Dieses Handbuch deckt nur zur Verwendung Pushmodell Datenupload (unterstützt nur im [REST-API](search-import-data-rest-api.md) und [.NET SDK](search-import-data-dotnet.md)), aber Sie noch erfahren Modells ziehen.

### <a name="push-data-to-an-index"></a>Verschieben von Daten auf einen index

Dies bezieht sich auf Senden Daten programmgesteuert Azure Suche zu suchen. Für sehr niedrige Latenz Anforderungen Applikationen (z.B. Wenn Sie Vorgänge mit dynamischen Lager Datenbanken zu suchen), Push-Modell ist nur die Option.

Die [REST-API](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [.NET SDK](search-import-data-dotnet.md) können Push Daten eines Indexes. Es ist zurzeit kein Tool wie Daten über das Portal.

Dieser Ansatz ist flexibler als das Pullmodell Dokumente einzeln oder in Batches hochladen können (bis zu 1000 pro Batch oder 16 MB, abhängig von dem Limit kommt erste). Push-Modell können Sie Dokumente in Azure Search unabhängig davon, wo die Daten hochladen.

### <a name="pull-data-into-an-index"></a>Daten in einem index

Pull-Modell eine unterstützte Datenquelle durchsucht und lädt automatisch die Daten in Azure Search Index für Sie. Verfolgung ändert und löscht vorhandene Dokumente neben neue Dokumente entfernen Indexer müssen die Daten im Index aktiv.

Diese Funktion wird in Azure Suche über *Indexer*derzeit für [BLOB-Speicher (Vorschau)](search-howto-indexing-azure-blob-storage.md) [DocumentDB](http://aka.ms/documentdb-search-indexer) [Azure SQL-Datenbank und SQL Server auf Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)implementiert.

Indexer-Funktionalität wird in der [Azure-Portal](search-import-data-portal.md) wie die [REST-API](https://msdn.microsoft.com/library/azure/dn946891.aspx)verfügbar gemacht.
