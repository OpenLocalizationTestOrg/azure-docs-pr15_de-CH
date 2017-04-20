<properties
   pageTitle="Eingebettete Microsoft Power BI - Herstellen einer Verbindung mit einer Datenquelle"
   description="Schalten Sie eingebettete BI, Verbinden mit Datenquellen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Verbinden Sie mit einer Datenquelle

Mit **Power BI Embedded**können Sie Berichte in Ihre Anwendung einbetten. Wenn Sie Power BI-Bericht in Ihre Anwendung einbetten, verbindet Bericht zum zugrunde liegenden **Importieren** eine Kopie der Daten oder durch **eine direkte Verbindung** mit der Datenquelle **DirectQuery**.

Hier sind die Unterschiede zwischen **Import** und **DirectQuery**.

|Importieren | DirectQuery
|---|---
|Tabellen, Spalten *und Daten* werden importiert oder in den Bericht Dataset kopiert. Dazu ändert, die an die zugrunde liegenden Daten müssen aktualisieren oder eine vollständige aktuelle Dataset erneut zu importieren.|Nur *Tabellen und Spalten* werden importiert oder in den Bericht Dataset kopiert. Immer werden die aktuellen Daten anzeigen.
Mit Power BI eingebettet sind können Sie DirectQuery mit Cloud-Datenquellen aber keine lokalen Daten zu diesem Zeitpunkt.

## <a name="benefits-of-using-directquery"></a>Vorteile von DirectQuery

Es gibt zwei Hauptvorteile **DirectQuery**verwenden:

   -    **DirectQuery** können Sie Visualisierung aller Daten über große Datasets, andernfalls erste nicht realisierbar wäre erstellen.
   -    Zugrunde liegende Daten geändert benötigen eine Aktualisierung der Daten und einige Berichte kann müssen die aktuellen Daten anzeigen umfangreicher Datenübertragungen tätigen erneut importieren verlangt. **DirectQuery** Berichte dagegen immer aktuelle Daten.

## <a name="limitations-of-directquery"></a>Grenzen des DirectQuery

   Es gibt einige Nachteile mit **DirectQuery**:

   -    Alle Tabellen müssen aus einer Datenbank stammen.
   -    Wenn die Abfrage zu komplex ist, tritt ein Fehler auf. Um den Fehler zu beheben müssen Sie die Abfrage ist weniger komplex gestalten. Wenn die Quuery komplexer sein muss, müssen Sie den Datenimport anstelle von **DirectQuery**.
   -    Beziehung Filtern ist auf eine Richtung, anstatt beide Richtungen.
   -    Sie können den Datentyp einer Spalte nicht ändern.
   -    Standardmäßig werden Beschränkungen DAX Ausdrücke Maßnahmen zulässig. Siehe [DirectQuery und Maßnahmen](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery und Maßnahmen

Um sicherzustellen, dass Anfragen an die zugrunde liegende Datenquelle eine akzeptablen Leistung, Grenzen Maßnahmen auferlegt. Wenn mit **Power BI Desktop**erweiterte Benutzer können wählen diese Beschränkung umgehen **Datei > Einstellungen > Optionen**. Wählen Sie im Dialogfeld **Optionen** **DirectQuery**und wählen Sie **uneingeschränkte Maßnahmen im DirectQuery-Modus**. Wenn diese Option aktiviert ist, kann ein DAX-Ausdruck, der für ein Measure verwendet werden. Benutzer müssen sich bewusst sein; jedoch führen, dass einige Ausdrücke, die sehr beim Importieren der Daten gut sehr langsamen Abfragen an die Back-End-Quelle im **DirectQuery** -Modus. 

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
