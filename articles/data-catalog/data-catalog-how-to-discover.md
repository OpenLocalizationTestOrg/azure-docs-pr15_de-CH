<properties
   pageTitle="Datenquellen entdecken | Microsoft Azure"
   description="Gewusst wie-Artikel wie registrierte Datenbestände mit Azure Data Catalog, einschließlich durchsuchen und Filtern sowie mithilfe den Treffer hervorheben Funktionen von Azure Data Catalog-Portal zu markieren."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Ermitteln von Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Data Catalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. Also darum **Azure Data Catalog** Menschen entdecken, verstehen und Verwenden von Datenquellen und helfen Unternehmen mehr Nutzen aus vorhandenen Daten. Sobald eine Datenquelle mit **Azure Data Catalog**registriert wurde, wird seine Metadaten vom Dienst indiziert, so dass Benutzer problemlos suchen können, die Daten entdecken, die sie benötigen.

## <a name="searching-and-filtering"></a>Suchen und Filtern

Erkennung in **Azure Data Catalog** verwendet zwei primäre: Suchen und filtern.

Suchen soll intuitiv und leistungsstark – standardmäßig, Suchbegriffe mit einer Eigenschaft im Katalog vom Benutzer bereitgestellte Strukturänderungen einschließlich abgeglichen.

Filtern als Ergänzung suchen. Benutzer können bestimmte Eigenschaften wie Experten Datenquellentyp, Objekttyp und Tags anzeigen nur übereinstimmende Datenbestände und kongruente Vermögenswerte sowie Suchergebnisse einschränken aktivieren.

Mithilfe einer Kombination aus suchen und Filtern können Benutzer schnell die Datenquellen, die mit **Azure Data Catalog** zu Datenquellen, die Sie registriert wurden.

## <a name="search-syntax"></a>Suchsyntax

Zwar standardmäßig Freitextsuche einfach und intuitiv, können Benutzer Suchsyntax **Azure Data Catalog**auch mehr Kontrolle über die Suchergebnisse. **Azure Data Catalog** unterstützt die folgenden Techniken:

| Verfahren                 | Verwendung                                                                                                                                     | Beispiel                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Einfache Suche              | Einfache Suche einen oder mehrere Suchbegriffe verwenden. Die Ergebnisse sind Anlagen, die eine Eigenschaft mit einem oder mehreren angegebenen Begriffe entsprechen. | Daten                                                |
| Scoping-Eigenschaft          | Datenquellen, in denen der Suchbegriff mit der angegebenen Eigenschaft entspricht, nur zurück                                                   | Name: Finanzen                                              |
| Boolesche Operatoren         | Erweitern oder eingrenzen einer Suche boolesche Operationen mit                                                                                     | Finanzen nicht Unternehmen                                     |
| Mit Klammern | Mithilfe von Klammern Gruppe Teile der Abfrage zu logischen Isolierung, insbesondere im Zusammenhang mit booleschen Operatoren              | Name: Finanzen und (Q1: Tags oder Tags: Q2) |
| Vergleichsoperatoren      | Verwenden Sie Vergleiche als Gleichheit für Eigenschaften mit numerischen und Datumsdatentypen                                                | ModifiedTime > "11/05/2014"                                 |

Weitere Informationen zu **Azure Data Catalog** Search finden Sie unter [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Treffern
Beim Anzeigen von Suchergebnissen werden alle angezeigten Eigenschaften angegebenen Suchbegriffe – wie Daten Ressourcenname, Beschreibung und Tags – markiert damit leichter zu erkennen, warum Daten Anlage von einer Suche zurückgegeben wurde.

> [AZURE.NOTE] Benutzer können Treffertests ggf. mit der Option "Hervorheben" in **Azure Data Catalog** -Portal aus Hervorhebung aktivieren.

Beim Anzeigen von Suchergebnissen kann es nicht immer offensichtlich, warum eine Anlage Daten enthalten, ist es selbst bei Suchtreffer markieren aktiviert. Da alle Eigenschaften standardmäßig durchsucht werden, kann durch eine Übereinstimmung einer Eigenschaft auf eine Datenressource zurückgegeben werden. Und da mehrere Benutzer registrierte Datenbestände mit Tags und Beschreibung versehen können, möglicherweise nicht alle Metadaten in der Liste der Suchergebnisse angezeigt.

In der standardmäßigen Ansicht nebeneinander, Ziegel in den Suchergebnissen angezeigt wird ein "View Suchbegriff entspricht" Symbol Benutzer schnell entspricht und deren Position anzeigen, und wechseln Ihnen ggf. kann enthalten.

 ![Treffern und Suchergebnisse im Azure Data Catalog-portal](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Zusammenfassung
Registrieren einer Datenquelle **Azure Data Catalog** erleichtert die Datenquelle zu entdecken, strukturelle und beschreibenden Metadaten aus der Datenquelle in der Katalog-Dienst kopiert. Sobald eine Datenquelle registriert wurde, können Benutzer mit Filtern und Suchen von **Azure Data Catalog** -Portal ermitteln.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md) -Lernprogramm schrittweise Informationen zu Datenquellen.
