<properties
    pageTitle="Wie komplexe Datentypen in Azure Suche Modell | Microsoft Azure-Suche"
    description="Geschachtelte oder hierarchische Datenstrukturen in einem vereinfachten Rowset mit Sammlungen Datentyp Azure Suchindex modelliert werden können."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Wie komplexe Datentypen in Azure Suche Modell

Externe Datasets eine Azure-Suchindex manchmal aufgefüllt enthalten hierarchische oder geschachtelte Teilstrukturen, die in einem tabellarischen Rowset nicht ordentlich unterteilen. Beispiele für solche Strukturen können mehrere Standorte und Rufnummern für einen einzelnen Debitor, mehrere Farben und Größen für eine einzelne SKU mehrere Autoren ein Heft usw.. Modellieren Sie Begriffe, sehen Sie diese Strukturen genannt *komplexe Datentypen*, *zusammengesetzte Datentypen*, *zusammengesetzte Datentypen*oder *Daten aggregieren*, um nur einige zu nennen.

Komplexe Datentypen werden in Azure Search nicht nativ unterstützt, aber eine bewährte Lösung Schritten die Struktur reduzieren und dann **Auflistungstyp Daten** wiederherstellen die Struktur enthält. In diesem Artikel beschriebenen Verfahren ermöglicht es den Inhalt durchsucht werden, facettierten, gefiltert und sortiert.

## <a name="example-of-a-complex-data-structure"></a>Beispiel für eine komplexe Datenstruktur

Befindet sich in der Regel Daten, wie JSON oder XML-Dokumenten und Elementen in einem NoSQL-Speichers wie DocumentDB. Die Herausforderung stammt strukturell müssen mehrere untergeordnete Elemente, die durchsucht und gefiltert werden.  Nehmen Sie als Ausgangspunkt zur Veranschaulichung umgehen folgende JSON-Dokument, das eine Gruppe von Kontakten als Beispiel auflistet:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Die Felder "Id" genannt, 'Name' und 'Unternehmen' können problemlos zugeordnet werden als Felder in einem Index Azure Suche 1, Feld 'Lagerplätze' enthält ein Array von Positionen mit einem Satz von Standort-IDs sowie Speicherort. Da Azure Suche keinen Datentyp haben, der dies unterstützt, benötigen wir dieses Modell in Azure Suche anders. 

> [AZURE.NOTE] Dieses Verfahren wird auch von Kirk Evans in einem Blogbeitrag [Indizierung DocumentDB Azure Suche](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/)zeigt ein Verfahren namens "Zusammenfassen der Daten" beschrieben, ein Feld namens hätte `locationsID` und `locationsDescription` sind beide [Sammlungen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (oder ein Array von Zeichenfolgen).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Teil 1: Arrays in einzelne Felder reduzieren

Erstellen Sie zum Erstellen eines Indexes Azure suchen, das Dataset aufnimmt, einzelne Felder für geschachtelte Unterstruktur: `locationsID` und `locationsDescription` mit einem Datentyp [Sammlungen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (oder ein Array von Zeichenfolgen). Diese Felder würden index die Werte "1" und "2" in der `locationsID` Feld für John Smith und die Werte "3" und "4" in der `locationsID` für Jen Campbell.  

Die Daten in Azure Suche würde wie folgt aussehen: 

![Beispieldaten 2 Zeilen](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Teil 2: Hinzufügen eines Felds Auflistung in der Indexdefinition

Im IndexSchema können die Felddefinitionen in diesem Beispiel ähneln.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Überprüfen Sie Suchverhalten und optional erweitern Sie den index

Wenn den Index erstellt und die Daten geladen, können Sie jetzt die Lösung überprüfen Ausführung der Suche Abfragen für das Dataset testen. Sollte jedes Feld **Auflistung** **durchsucht**, **gefiltert** und **Facetable**. Sie sollten wie Abfragen können:

* Suchen Sie alle Mitarbeiter der zentrale"Adventureworks".
* Abrufen der Anzahl von Personen in einem Heimbüro.  
* Der Mitarbeiter an einem Heimbüro, welche anderen Niederlassungen sowie die Personen an jedem Standort Funktionsweise anzeigen  

Fällt diese Technik auseinander ist eine Suche durchführen, die sowohl die Id als auch die Beschreibung kombiniert werden sollen. Zum Beispiel:

* Nach allen Personen suchen Home Office haben und hat eine Lagerort ID 4.  

Falls Sie noch der ursprüngliche Inhalt sah folgendermaßen aus:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Da wir die Daten in separate Felder getrennt haben, wir haben jedoch keine Möglichkeit zu wissen, wenn Innenministerium Jen Campbell, bezieht sich `locationsID 3` oder `locationsID 4`.  

Um diesen Fall zu behandeln, definieren Sie ein anderes Feld im Index, die alle Daten in einer Auflistung.  In unserem Beispiel rufen wir dieses Feld `locationsCombined` und wir werden die Inhalte einer `||` zwar können Trennzeichen, die man wäre eine eindeutige Gruppe von Zeichen für den Inhalt. Zum Beispiel: 

![2 Zeilen mit Beispieldaten](./media/search-howto-complex-data-types/sample-data-2.png)

Mit diesem `locationsCombined` Feld wir jetzt bieten noch mehr Fragen wie:

* Zeigen Sie Anzahl Personen bei einem' Home' mit Id '4 an'  
* Mitarbeiter ein Heimbüro mit Id "4" suchen. 

## <a name="limitations"></a>Grenzen

Diese Methode eignet sich für Szenarien, aber ist nicht in jedem Fall anwendbar.  Zum Beispiel:

1. Wenn Sie keinen statischen Satz von Feldern in der komplexen Datentyp und gab es keine Möglichkeit, alle möglichen ein einzelnes Feld zuzuordnen. 
2. Geschachtelte Objekte aktualisieren erfordert einigen zusätzlichen Aufwand bestimmen, genau wie in Azure Suchindex aktualisiert werden muss

## <a name="sample-code"></a>Beispielcode

Ein Beispiel sehen wie komplexe JSON-Dataset in Azure Suche und eine Reihe von Abfragen in Dataset an diesem [GitHub Repo](https://github.com/liamca/AzureSearchComplexTypes)ausführen.

## <a name="next-step"></a>Nächstes

[Stimme für systemeigene Unterstützung für komplexe Datentypen](https://feedback.azure.com/forums/263029-azure-search) Azure Suche UserVoice Seite und bieten zusätzliche Eingaben, die Sie zur Implementierung von Features berücksichtigen möchten. Erreichen Sie mir auf Twitter unter @liamca.


 