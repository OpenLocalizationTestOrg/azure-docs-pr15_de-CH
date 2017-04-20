<properties
    pageTitle="Abfragen von Azure Suchindex | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen einer Suchabfrage in Azure Suche und Suchparameter Suchergebnisse filtern und Sortieren verwenden."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>Abfragen von Azure Suchindex
> [AZURE.SELECTOR]
- [Übersicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Bei Suchabfragen nach Azure sind eine Reihe von Parametern, die zusammen mit der tatsächlichen Wortzahl angegeben werden können, die in das Suchfeld der Anwendung eingegeben werden. Diese Abfrageparameter können Sie zu einigen tiefer Kontrolle der Volltextsuche.

Im folgenden ist eine Liste, die häufige Verwendungen von Abfrageparametern in Azure Suche kurz erläutert. Vollständige Abdeckung der Abfrageparameter und ihr Verhalten finden Sie in den Detailseiten [REST-API](https://msdn.microsoft.com/library/azure/dn798927.aspx) und [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Arten von Abfragen

Azure Suche bietet leistungsfähige Abfragen zu erstellen. Sind die beiden wichtigsten Arten der Abfrage wird `search` und `filter`. Ein `search` Abfrage sucht einen oder mehrere Begriffe in alle _durchsuchbaren_ Felder im Index und erwarten eine Suchmaschine wie Google oder Bing funktioniert. Ein `filter` Abfrage ergibt einen booleschen Ausdruck über alle _filterbar_ Felder in einem Index. Im Gegensatz zu `search` Abfragen `filter` Abfragen entsprechen den genauen Inhalt ein Feld für Zeichenfolgenfelder beachtet werden.

Suchen und Filtern können einzeln oder zusammen verwendet werden. Verwenden Sie sie zusammen, der Filter zunächst für den gesamten Index und die Suche wird auf die Ergebnisse des Filters ausgeführt. Filter können daher eine nützliche Technik, da sie Dokumente die Suchabfrage reduzieren zu steigern.

Die Syntax für Filterausdrücke ist eine Teilmenge der [OData Filtersprache](https://msdn.microsoft.com/library/azure/dn798921.aspx). Suchabfragen können Sie für die [vereinfachte Syntax](https://msdn.microsoft.com/library/azure/dn798920.aspx) oder die [Abfragesyntax Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) die weiter unten behandelt.

### <a name="simple-query-syntax"></a>Einfache Abfragesyntax
[Einfache Abfragesyntax](https://msdn.microsoft.com/library/azure/dn798920.aspx) ist die Standard-Abfragesprache in Azure Suche verwendet. Einfache Abfragesyntax unterstützt eine Reihe von gemeinsamen Operatoren wie und, oder nicht, Ausdruck, Suffix und Rangfolge Operatoren.

### <a name="lucene-query-syntax"></a>Lucene Abfragesyntax
[Lucene Abfragesyntax](https://msdn.microsoft.com/library/azure/mt589323.aspx) ermöglicht die große Verbreitung und beeindruckende Abfragesprache [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)entwickelt.

Diese Abfragesyntax ermöglicht einfache Funktionen: [Feld Bereich Abfragen](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields) [fuzzy-Suche](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [Suche](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [Begriff angehoben](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [Suche mit regulären Ausdrücken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [Platzhaltersuche](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [Syntax Grundlagen](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)und [Abfragen mithilfe von booleschen Operatoren](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Sortierung der Ergebnisse
Beim Empfangen von Ergebnissen für eine Suchabfrage können Sie anfordern, Azure Suche die Ergebnisse sortiert nach Werten in einem bestimmten Feld dient. Standardmäßig ordnet Azure Suche Suchergebnisse basierend auf den Rang des Dokuments Suche Ergebnis [TF IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)abgeleitet wird.

Wenn Azure Suche zurückzugebenden Suchergebnisse sortiert einen anderen Wert als Ergebnis suchen können Sie die `orderby` Parameter suchen. Geben Sie den Wert von der `orderby` Parameter Feldnamen und Aufrufe der [ `geo.distance()` Funktion](https://msdn.microsoft.com/library/azure/dn798921.aspx) für Geodaten Werte. Jeder Ausdruck folgt `asc` an, dass die Ergebnisse in aufsteigender Reihenfolge angefordert werden und `desc` an, dass die Ergebnisse in absteigender Reihenfolge angefordert werden. Die standardmäßige Rangfolge Aufsteigend.

## <a name="paging"></a>Paging
Azure Suche vereinfacht die Auslagerung der Suchergebnisse zu implementieren. Mit dem `top` und `skip` Parameter reibungslos ausstellen Suchanfragen, die die Gesamtmenge der Suchergebnisse in verwaltbare, geordnete Teilmengen empfangen, die problemlos suchen UI ermöglicht. Wenn diese kleineren Teilmengen der Ergebnisse erhalten, erhalten Sie auch die Anzahl der Dokumente in der gesamten Suchergebnisse.

Sie erfahren mehr Suchergebnisse im Artikel [wie Seite Suchergebnisse in Azure Search](search-pagination-page-layout.md)paging.


## <a name="hit-highlighting"></a>Treffern
In Azure Search Hervorhebung Bereich genaue Suchergebnisse, die der Suchabfrage entsprechen erfolgt einfach mithilfe der `highlight`, `highlightPreTag`, und `highlightPostTag` Parameter. Sie können die _durchsuchbaren_ Felder sowie angeben, gibt die exakte Zeichenfolge Tags am Anfang und Ende der übereinstimmende Text anfügen, Azure-Suche übereinstimmende Text betont haben sollte.
