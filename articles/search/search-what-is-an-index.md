<properties
    pageTitle="Erstellen eines Indexes Azure Search | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Was ist ein Index in Azure suchen und wie wird sie eingesetzt?"
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

# <a name="create-an-azure-search-index"></a>Erstellen eines Indexes Azure suchen
> [AZURE.SELECTOR]
- [Übersicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Was ist ein Index?

Ein *Index* ist einem permanenten Speicher *und andere Konstrukte der Azure-Suchdienst* . Ein Dokument ist eine Einheit von durchsuchbaren Daten im Index. Beispielsweise eine e-Commerce-Einzelhändler möglicherweise ein Dokument für jeden Artikel, den sie verkaufen, eine News-Organisation möglicherweise ein Dokument für jeden Artikel usw.. Diese Konzepte in vertrauter Datenbank entsprechenden Zuordnung: *Index* ähnelt einer *Tabelle*und *Dokumente* entsprechen ungefähr den *Zeilen* in einer Tabelle.

Wenn Sie Dokumente hinzufügen/Upload und Suchabfragen nach Azure senden, senden Sie Ihre Anfragen an einem bestimmten Index Suchdienst.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Feldtypen und Attribute in einer Azure-Suchindex

Definieren des Schemas müssen Sie Name, Typ und Eigenschaften der einzelnen Felder im Index angeben. Der Feldtyp wird in diesem Feld gespeicherten Daten klassifiziert. Attribute werden auf einzelne Felder festgelegt, an wie das Feld verwendet wird. Die folgenden Tabellen Auflisten von Typen und Attribute, die Sie angeben können.


### <a name="field-types"></a>Feldtypen
|Typ|Beschreibung|
|------------|-----------|
|*Edm.String*|Text, der optional für Volltextsuche (Trennfugen Wortstamm usw.) Token.|
|*Collection(EDM.String)*|Eine Liste von Zeichenfolgen, die optional für Volltextsuche Token. Keine theoretische Obergrenze für die Anzahl der Elemente in der Auflistung vorhanden ist, aber die Obergrenze von 16 MB für Nutzlastgröße für Sammlungen.|
|*Edm.Boolean*|Werte wahr/falsch.|
|*Edm.Int32*|32-Bit-Ganzzahl-Werte.|
|*Int64*|64-Bit-Ganzzahl-Werte.|
|*Edm.Double*|Numerische Daten mit doppelter Genauigkeit.|
|*Edm.DateTimeOffset*|Datum Zeitwerte OData V4-Format dargestellt (z. B. `yyyy-MM-ddTHH:mm:ss.fffZ` oder `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Ein Punkt, einen Standort auf der Welt darstellt.|

Ausführlichere Informationen zu Azure Search [unterstützten Datentypen auf MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx)finden.



### <a name="field-attributes"></a>Attribute
|Attribut|Beschreibung|
|------------|-----------|
|*Schlüssel*|Eine Zeichenfolge, die eindeutige ID der einzelnen Dokumente zum Dokument zu suchen. Jeder Index muss einen Schlüssel. Nur ein Feld kann den Schlüssel und seinen Typ auf Edm.String eingestellt werden.|
|*Abrufbar*|Gibt an, ob ein Feld in einem Suchergebnis zurückgegeben werden kann.|
|*Filtern*|Feld Filterabfragen verwendet werden können.|
|*Sortierbar*|Eine Abfrage, um dieses Feld Suchergebnisse sortieren können.|
|*Facetable*|Ein Feld in einer [facettierten](search-faceted-navigation.md) Navigationsstruktur für Benutzer selbst Filtern verwendet werden können. In der Regel am besten Felder, wiederkehrende Werte zum Gruppieren mehrerer Dokumente (z. B. mehrere Dokumente, die in einer einzelnen Marke oder Leistungsart fallen) als Facets.|
|*Durchsuchbare*|Markiert das Feld als Volltext durchsuchbaren.|

Weitere Informationen zu Azure Search [Indexattribute auf MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx)finden.



## <a name="guidance-for-defining-an-index-schema"></a>Anleitung zum Definieren eines Schemas index

Entwerfen den Index nehmen Sie sich Zeit in der Planung jeder Entscheidung denken. Es ist wichtig, dass Sie dabei Ihre Erfahrung und Bedürfnisse suchen Benutzer bei Index als jedes Feld die [richtigen Attribute](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden muss. Index ändern, nach der Bereitstellung umfasst Neuaufbau und Neuladen der Daten.


Speicherplatz für Daten mit der Zeit ändern, erhöhen oder Verringern der Kapazität durch Hinzufügen oder Entfernen von Partitionen. Einzelheiten finden Sie [in Azure Suchdienst verwalten](search-manage.md) oder für den [Dienst](search-limits-quotas-capacity.md).
