<properties
    pageTitle="Analytics Ansicht-Designer anmelden | Microsoft Azure"
    description="Ansicht-Designer im Protokollanalyse können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Konsole, die verschiedene Visualisierung von Daten in die OMS-Repository enthalten. Dieser Artikel enthält eine Einstellung für die Visualisierung Teile zur benutzerdefinierten Ansichten verwenden."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Ansicht-Designer Analytics Visualisierung Teilereferenz protokollieren
Ansicht-Designer im Protokollanalyse können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Konsole, die verschiedene Visualisierung von Daten aus dem OMS-Repository enthalten. Dieser Artikel enthält eine Einstellung für die Visualisierung Teile zur benutzerdefinierten Ansichten verwenden.

Andere Artikel für Ansicht-Designer verfügbar sind:

- [Ansicht-Designer](log-analytics-view-designer.md) - Übersicht der Ansicht-Designer und Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.
- [Kachel Referenz](log-analytics-view-designer-tiles.md) - Verweis Einstellungen für alle Kacheln in benutzerdefinierten Ansichten verwendet. 

Die folgende Tabelle beschreibt die verschiedenen Kacheln im Ansicht-Designer verfügbar.  Die folgenden Abschnitte beschreiben jede Kachel und deren Eigenschaften.

| Typ anzeigen | Beschreibung |
|:--|:--|
| [Liste der Abfragen](#list-of-queries-part) | Zeigt eine Liste von Suchabfragen Protokoll.  Der Benutzer kann für jede Abfrage auf Ergebnisse anzeigen klicken.  |
| [Anzahl & Liste](#number-amp-list-part) | Header hat einzelne Zahl zeigt Datensätze aus einer Suchabfrage Protokoll.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf. |
| [Zwei Zahlen & Liste](#two-numbers-amp-list-part) | Header weist zwei Zahlen Datensätze aus getrennten Protokollen Suchabfragen.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf. |
| [Rad & Liste](#donut-amp-list-part) | Kopfzeile zeigt eine einzelne Zahl aus einer Wertespalte eine Protokollabfrage zusammengefasst.  Der Ring zeigt grafisch Ergebnisse der oberen drei Datensätze. |
| [Zwei Zeitachsen & Liste](#two-timelines-amp-list-part) | Kopfzeile zeigt die Ergebnisse Protokoll Abfragen wie Säulendiagramme mit einer Legende anzeigen eine einzelne Zahl aus einer Wertespalte eine Protokollabfrage zusammengefasst.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf. |   
| [Informationen](#information-part) | Kopfzeile Zeigt statischen Text und einen optionalen Link.  Liste zeigt ein oder mehrere Elemente mit statischem Text und Titel. |
| [Liniendiagramm, Legende & Liste](#line-chart-callout-amp-list-part) | Kopfzeile zeigt ein Liniendiagramm mit mehreren Serien aus eine Protokollabfrage und eine Legende mit einem zusammengefassten Wert.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf. |
| [Linien- & Liste](#line-chart-amp-list-part) | Kopfzeile zeigt ein Liniendiagramm mit mehreren Serien aus eine Protokollabfrage langfristig.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf. |
| [Stapel Teil Diagramme Zeile](#stack-of-line-charts-part) | Zeigt drei separate Liniendiagrammen mit mehreren Serien aus eine Protokollabfrage langfristig. |


## <a name="list-of-queries-part"></a>Liste der Abfragen Teil

Zeigt eine Liste von Suchabfragen Protokoll.  Der Benutzer kann für jede Abfrage auf Ergebnisse anzeigen klicken.  Die Ansicht enthält eine einzelne Abfrage standardmäßig, und Sie können auf **+ Abfrage** zusätzliche Abfragen hinzufügen.

![Liste von Abfragen](media/log-analytics-view-designer/view-list-queries.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Titel | Am oberen Rand der Ansicht anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Vorauswahl Filter | Durch Kommas getrennte Liste mit Eigenschaften im linken Filterbereich einbezogen, wenn der Benutzer eine Abfrage auswählt. |
| Modus | Erste Ansicht angezeigt, wenn die Abfrage ausgewählt wird.  Der Benutzer kann alle verfügbaren Ansichten auswählen, nach dem Öffnen der Abfrage. |
| **Abfragen** |
| Suchabfrage | Abfrage ausführen. |
| Angezeigter name | Beschreibender Name der Abfrage, die dem Benutzer angezeigt. |


## <a name="number--list-part"></a>Anzahl & Liste Teil

Header hat einzelne Zahl zeigt Datensätze aus einer Suchabfrage Protokoll.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf.


![Liste von Abfragen](media/log-analytics-view-designer/view-number-list.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Ansicht anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt.
| Symbol | Wählen Sie die Anzeige des Symbols. |
| **Titel** |
| Legende | Am oberen Rand der Kopfzeile anzuzeigende Text. |
| Abfrage | Abfrage für den Header ausgeführt.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| **Liste** |
| Abfrage | Abfrage der Liste ausgeführt.  Die ersten beiden Eigenschaften für die ersten 10 Datensätze in den Ergebnissen angezeigt werden.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Balken werden automatisch basierend auf dem relativen Wert der numerischen Spalte erstellt.<br><br>Befehl Sortieren in der Abfrage die Datensätze in der Liste sortiert.  Der Benutzer kann dann alle zu der Abfrage alle Datensätze anzeigen. |
| Diagramm ausblenden | Wählen Sie das Diagramm rechts numerische Spalte deaktivieren. |
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Farbe | Farbe der Balken oder Sparklines. |
| Name / Wert-Trennzeichen | Einzelnen Trennzeichen die Texteigenschaft in mehrere Werte analysieren möchten.  Einzelheiten finden Sie unter [Allgemeine Optionen](#name-value-separator) . |
| Navigationsbereich-Abfrage | Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#navigation-query) . |
| **Liste** | **> Spaltentitel** |
| Name | Text am oberen Rand der ersten Spalte der Liste angezeigt. |
| Wert | Text oben in der zweiten Spalte der Liste angezeigt. |
| **Liste** | **> Schwellenwerte** |
| Schwellenwerte aktivieren | Wählen Sie Schwellenwerte aktivieren.  Einzelheiten finden Sie unter [Allgemeine Optionen](#thresholds) . |


## <a name="two-numbers--list-part"></a>Zwei Zahlen & Listenteil

Header weist zwei Zahlen Datensätze aus getrennten Protokollen Suchabfragen.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf.

![Zwei Zahlen & Liste](media/log-analytics-view-designer/view-two-numbers-list.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Ansicht anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt.
| Symbol | Wählen Sie die Anzeige des Symbols. |
| **Titel** |
| Legende | Am oberen Rand der Kopfzeile anzuzeigende Text. |
| Abfrage | Abfrage für den Header ausgeführt.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| **Liste** |
| Abfrage | Abfrage der Liste ausgeführt.  Die ersten beiden Eigenschaften für die ersten 10 Datensätze in den Ergebnissen angezeigt werden.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Balken werden automatisch basierend auf dem relativen Wert der numerischen Spalte erstellt.<br><br>Befehl Sortieren in der Abfrage die Datensätze in der Liste sortiert.  Der Benutzer kann dann alle zu der Abfrage alle Datensätze anzeigen. |
| Diagramm ausblenden | Wählen Sie das Diagramm rechts numerische Spalte deaktivieren. |
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Farbe | Farbe der Balken oder Sparklines. |
| Vorgang | Vorgang für die Sparkline ausgeführt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Name / Wert-Trennzeichen | Einzelnen Trennzeichen die Texteigenschaft in mehrere Werte analysieren möchten.  Einzelheiten finden Sie unter [Allgemeine Optionen](#name-value-separator) . |
| Navigationsbereich-Abfrage | Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#navigation-query) . |
| **Liste** | **> Spaltentitel** |
| Name | Text am oberen Rand der ersten Spalte der Liste angezeigt. |
| Wert | Text oben in der zweiten Spalte der Liste angezeigt. |
| **Liste** | **> Schwellenwerte** |
| Schwellenwerte aktivieren | Wählen Sie Schwellenwerte aktivieren.  Einzelheiten finden Sie unter [Allgemeine Optionen](#thresholds) . |

## <a name="donut--list-part"></a>Rad & Liste Teil

Kopfzeile zeigt eine einzelne Zahl aus einer Wertespalte eine Protokollabfrage zusammengefasst.  Der Ring zeigt grafisch Ergebnisse der oberen drei Datensätze.

![Rad & Liste anzeigen](media/log-analytics-view-designer/view-donut-list.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Kachel anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt. |
| Symbol | Wählen Sie die Anzeige des Symbols. |
| **Kopfzeile** |
| Titel | Am oberen Rand der Kopfzeile anzuzeigende Text.
| Untertitel | Unter dem Titel am oberen Rand der Kopfzeile anzuzeigende Text.
| **Ring** |
| Abfrage | Abfrage für den Donut ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert. |
| **Ring** |  **> Center** |
| Text | Unter den Wert in den Ring anzuzeigende Text. |
| Vorgang | Der Vorgang auf die Value-Eigenschaft in einen einzelnen Wert zusammengefasst.<br><br>-Summe: Die Werte aller Datensätze hinzufügen.<br>-Prozentsatz: Prozentsatz der Werte im **Ergebniswerte im Betrieb** in die Gesamtanzahl der Datensätze in der Abfrage zurückgegebenen Datensätze. |
| Ergebniswerte in Betrieb verwendet | Klicken Sie optional auf das Pluszeichen einen oder mehrere Werte hinzufügen.  Die Ergebnisse der Abfrage werden Datensätze mit den Eigenschaftswerten auf die Sie angeben.  Wenn keine Werte hinzugefügt werden, werden alle Datensätze in der Abfrage enthalten. |
| **Zusätzliche Optionen** | **> Farben** |
| Farbe 1<br>Farbe 2<br>Farbe 3 | Wählen Sie für jeden der Werte in den Ring angezeigt. |
| **Zusätzliche Optionen** | **> Erweiterte Zuordnung** |
| Feldwert | Geben Sie den Namen eines Feldes in den Ring enthalten eine andere Farbe anzuzeigen. |
| Farbe | Wählen Sie für das Feld. |
| **Liste** |
| Abfrage | Abfrage der Liste ausgeführt.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| Diagramm ausblenden | Wählen Sie das Diagramm rechts numerische Spalte deaktivieren. |
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Farbe | Farbe der Balken oder Sparklines. |
| Vorgang | Vorgang für die Sparkline ausgeführt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Name / Wert-Trennzeichen | Einzelnen Trennzeichen die Texteigenschaft in mehrere Werte analysieren möchten.  Einzelheiten finden Sie unter [Allgemeine Optionen](#name-value-separator) . |
| Navigationsbereich-Abfrage | Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#navigation-query) . |
| **Liste** | **> Spaltentitel** |
| Name | Text am oberen Rand der ersten Spalte der Liste angezeigt. |
| Wert | Text oben in der zweiten Spalte der Liste angezeigt. |
| **Liste** | **> Schwellenwerte** |
| Schwellenwerte aktivieren | Wählen Sie Schwellenwerte aktivieren.  Einzelheiten finden Sie unter [Allgemeine Optionen](#thresholds) . |

## <a name="two-timelines--list-part"></a>Zwei Zeitachsen und Teilen

Kopfzeile zeigt die Ergebnisse Protokoll Abfragen wie Säulendiagramme mit einer Legende anzeigen eine einzelne Zahl aus einer Wertespalte eine Protokollabfrage zusammengefasst.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf.

![Zwei Zeitachsen & Liste anzeigen](media/log-analytics-view-designer/view-two-timelines-list.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Kachel anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt. |
| Symbol | Wählen Sie die Anzeige des Symbols. |
| **Zuerst Diagramm<br>zweites Diagramm** |
| Legende | Unter die Legende der ersten Datenreihe anzuzeigende Text. |
| Farbe | Farbe für die Spalten in der Reihe. |
| Abfrage | Abfrage für die erste Datenreihe ausgeführt.  Die Anzahl der Datensätze über jedes Zeitintervall wird durch die Säulen im Diagramm dargestellt. |
| Vorgang | Der Vorgang auf die Value-Eigenschaft einen Wert für die Legende zusammenfassen.<br><br>-Summe: Summe aus allen Datensätzen.<br>-Durchschnitt: Durchschnittliche Wert aus allen Datensätzen.<br>-Letzte Beispiel: Wert aus dem letzten Intervall im Diagramm enthalten.<br>-Erste Beispiel: Wert das erste Intervall im Diagramm enthalten.<br>-Anzahl: Alle Datensätze von der Abfrage zurückgegeben.|
| **Liste** |
| Abfrage | Abfrage der Liste ausgeführt.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| Diagramm ausblenden | Wählen Sie das Diagramm rechts numerische Spalte deaktivieren. |
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Farbe | Farbe der Balken oder Sparklines. |
| Vorgang | Vorgang für die Sparkline ausgeführt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Navigationsbereich-Abfrage | Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#navigation-query) . |
| **Liste** | **> Spaltentitel** |
| Name | Text am oberen Rand der ersten Spalte der Liste angezeigt. |
| Wert | Text oben in der zweiten Spalte der Liste angezeigt. |
| **Liste** | **> Schwellenwerte** |
| Schwellenwerte aktivieren | Wählen Sie Schwellenwerte aktivieren.  Einzelheiten finden Sie unter [Allgemeine Optionen](#thresholds) . |

## <a name="information-part"></a>Daten in

Kopfzeile Zeigt statischen Text und einen optionalen Link.  Liste zeigt ein oder mehrere Elemente mit statischem Text und Titel.

![Informationen anzeigen](media/log-analytics-view-designer/view-information.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Kachel anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Farbe | Hintergrundfarbe für den Header. |
| **Kopfzeile** |
| Bild | Image-Datei in der Kopfzeile angezeigt. |
| Bezeichnung | In der Kopfzeile anzuzeigende Text. |
| **Kopfzeile** | **> Link** |
| Bezeichnung | Linktext. |
| URL | URL für den Link. |
| **Informationen** |
| Titel | Text für Titel jedes Elements angezeigt. |
| Inhalt | Für jedes Element anzuzeigende Text. |


## <a name="line-chart-callout--list-part"></a>Liniendiagramm, Legende & Listenteil

Kopfzeile zeigt ein Liniendiagramm mit mehreren Serien aus eine Protokollabfrage und eine Legende mit einem zusammengefassten Wert.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf.

![Liniendiagramm, Legende und Listenansicht](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Kachel anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt. |
| Symbol | Wählen Sie die Anzeige des Symbols. |
| **Kopfzeile** |
| Titel | Am oberen Rand der Kopfzeile anzuzeigende Text. |
| Untertitel | Unter dem Titel am oberen Rand der Kopfzeile anzuzeigende Text. |
| **Liniendiagramm** |
| Abfrage | Abfrage für das Liniendiagramm ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Dies ist normalerweise eine Abfrage, die **Maßnahme** -Schlüsselwort verwendet, Ergebnisse zusammengefasst.  Wenn die Abfrage das Schlüsselwort **Intervall** verwendet verwendet die X-Achse des Diagramms dieses Zeitintervall.  Wenn die Abfrage nicht das Schlüsselwort **Intervall** werden stündlich für die X-Achse verwendet. |
| **Liniendiagramm** | **> Legende** |
| Legende mit Titel | Über den Wert der Legende anzuzeigende Text. |
| Name der Datenreihe | Der Eigenschaftswert für die Datenreihe für die Legende Wert.  Wenn keine Datenreihe angegeben wird, werden alle Datensätze von der Abfrage verwendet. |
| Vorgang | Der Vorgang auf die Value-Eigenschaft einen Wert für die Legende zusammenfassen.<br><br>-Durchschnitt: Durchschnittliche Wert aus allen Datensätzen.<br>-Count Anzahl aller Datensätze zurückgegeben von der Abfrage.<br>-Letzte Beispiel: Wert aus dem letzten Intervall im Diagramm enthalten.<br>-Max: Höchstwert Intervalle im Diagramm enthalten.<br>-Min: Minimalwert Intervalle im Diagramm enthalten.<br>-Summe: Summe aus allen Datensätzen. |
| **Liniendiagramm** | **> Y-Achse** |
| Logarithmische Skalierung verwenden | Wählen Sie für die Y-Achse eine logarithmische Skalierung verwendet. |
| Einheiten | Geben Sie die Einheit für die von der Abfrage zurückgegebenen Werte.  Diese Informationen zum Etiketten auf dem Diagramm Werttypen und optional für das Konvertieren der Werte angezeigt.  Der Einheitentyp gibt die Kategorie der Einheit und definiert die aktuelle Einheitentyp Werte verfügbar.  Wählt einen Wert in konvertieren Sie, um numerische Werte zu konvertieren, geben Sie aus der aktuellen Einheit konvertiert werden. |
| Benutzerdefiniertes Etikett | Für die Y-Achse neben der Bezeichnung für den Einheitentyp anzuzeigende Text.  Wenn keine Bezeichnung angegeben ist, wird nur der Einheitentyp angezeigt. |
| **Liste** |
| Abfrage | Abfrage der Liste ausgeführt.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| Diagramm ausblenden | Wählen Sie das Diagramm rechts numerische Spalte deaktivieren. |
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Farbe | Farbe der Balken oder Sparklines. |
| Vorgang | Vorgang für die Sparkline ausgeführt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Name / Wert-Trennzeichen | Einzelnen Trennzeichen die Texteigenschaft in mehrere Werte analysieren möchten.  Einzelheiten finden Sie unter [Allgemeine Optionen](#name-value-separator) . |
| Navigationsbereich-Abfrage | Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#navigation-query) . |
| **Liste** | **> Spaltentitel** |
| Name | Text am oberen Rand der ersten Spalte der Liste angezeigt. |
| Wert | Text oben in der zweiten Spalte der Liste angezeigt. |
| **Liste** | **> Schwellenwerte** |
| Schwellenwerte aktivieren | Wählen Sie Schwellenwerte aktivieren.  Einzelheiten finden Sie unter [Allgemeine Optionen](#thresholds) . |

## <a name="line-chart--list-part"></a>Diagramm & Liste Teil Zeile

Kopfzeile zeigt ein Liniendiagramm mit mehreren Serien aus eine Protokollabfrage langfristig.  Liste zeigt die ersten zehn Ergebnisse aus einer Abfrage Diagramm zeigt den relativen Wert einer nummerischen Spalte oder die Änderung im Zeitverlauf.

![Positionsansicht Liste und Diagramm](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Kachel anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt. |
| Symbol | Wählen Sie die Anzeige des Symbols. |
| **Kopfzeile** |
| Titel | Am oberen Rand der Kopfzeile anzuzeigende Text. |
| Untertitel | Unter dem Titel am oberen Rand der Kopfzeile anzuzeigende Text. |
| **Liniendiagramm** |
| Abfrage | Abfrage für das Liniendiagramm ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Dies ist normalerweise eine Abfrage, die **Maßnahme** -Schlüsselwort verwendet, Ergebnisse zusammengefasst.  Wenn die Abfrage das Schlüsselwort **Intervall** verwendet verwendet die X-Achse des Diagramms dieses Zeitintervall.  Wenn die Abfrage nicht das Schlüsselwort **Intervall** werden stündlich für die X-Achse verwendet. |
| **Liniendiagramm** | **> Y-Achse** |
| Logarithmische Skalierung verwenden | Wählen Sie für die Y-Achse eine logarithmische Skalierung verwendet. |
| Einheiten | Geben Sie die Einheit für die von der Abfrage zurückgegebenen Werte.  Diese Informationen zum Etiketten auf dem Diagramm Werttypen und optional für das Konvertieren der Werte angezeigt.  Der Einheitentyp gibt die Kategorie der Einheit und definiert die aktuelle Einheitentyp Werte verfügbar.  Wählt einen Wert in konvertieren Sie, um numerische Werte zu konvertieren, geben Sie aus der aktuellen Einheit konvertiert werden. |
| Benutzerdefiniertes Etikett | Für die Y-Achse neben der Bezeichnung für den Einheitentyp anzuzeigende Text.  Wenn keine Bezeichnung angegeben ist, wird nur der Einheitentyp angezeigt. |
| **Liste** |
| Abfrage | Abfrage der Liste ausgeführt.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| Diagramm ausblenden | Wählen Sie das Diagramm rechts numerische Spalte deaktivieren. |
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Farbe | Farbe der Balken oder Sparklines. |
| Vorgang | Vorgang für die Sparkline ausgeführt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#sparklines) . |
| Name / Wert-Trennzeichen | Einzelnen Trennzeichen die Texteigenschaft in mehrere Werte analysieren möchten.  Einzelheiten finden Sie unter [Allgemeine Optionen](#name-value-separator) . |
| Navigationsbereich-Abfrage | Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Einzelheiten finden Sie unter [Allgemeine Optionen](#navigation-query) . |
| **Liste** | **> Spaltentitel** |
| Name | Text am oberen Rand der ersten Spalte der Liste angezeigt. |
| Wert | Text oben in der zweiten Spalte der Liste angezeigt. |
| **Liste** | **> Schwellenwerte** |
| Schwellenwerte aktivieren | Wählen Sie Schwellenwerte aktivieren.  Einzelheiten finden Sie unter [Allgemeine Optionen](#thresholds) . |

## <a name="stack-of-line-charts-part"></a>Stapel Teil Diagramme Zeile

Zeigt drei separate Liniendiagrammen mit mehreren Serien aus eine Protokollabfrage langfristig.

![Stapel von Liniendiagrammen](media/log-analytics-view-designer/view-stack-line-charts.png)

| Einstellung | Beschreibung |
|:--|:--|
| **Allgemein** |
| Gruppe | Am oberen Rand der Kachel anzuzeigende Text. |
| Neue Gruppe | Wählen Sie zum Erstellen einer neuen Gruppe in der aktuellen Ansicht ab. |
| Symbol | Bilddatei neben dem Ergebnis in der Kopfzeile angezeigt. |
| **Diagramm 1<br>Diagramm 2<br>Diagramm 3** | **> Header** |
| Titel | Text am oberen Rand des Diagramms angezeigt. |
| Untertitel | Unter dem Titel oben im Diagramm anzuzeigende Text. |
| **Diagramm 1<br>Diagramm 2<br>Diagramm 3** | **Liniendiagramm** |
| Abfrage | Abfrage für das Liniendiagramm ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Dies ist normalerweise eine Abfrage, die **Maßnahme** -Schlüsselwort verwendet, Ergebnisse zusammengefasst.  Wenn die Abfrage das Schlüsselwort **Intervall** verwendet verwendet die X-Achse des Diagramms dieses Zeitintervall.  Wenn die Abfrage nicht das Schlüsselwort **Intervall** werden stündlich für die X-Achse verwendet. |
| **Diagramm** | **> Y-Achse** |
| Logarithmische Skalierung verwenden | Wählen Sie für die Y-Achse eine logarithmische Skalierung verwendet. |
| Einheiten | Geben Sie die Einheit für die von der Abfrage zurückgegebenen Werte.  Diese Informationen zum Etiketten auf dem Diagramm Werttypen und optional für das Konvertieren der Werte angezeigt.  Der Einheitentyp gibt die Kategorie der Einheit und definiert die aktuelle Einheitentyp Werte verfügbar.  Wählt einen Wert in konvertieren Sie, um numerische Werte zu konvertieren, geben Sie aus der aktuellen Einheit konvertiert werden. |
| Benutzerdefiniertes Etikett | Für die Y-Achse neben der Bezeichnung für den Einheitentyp anzuzeigende Text.  Wenn keine Bezeichnung angegeben ist, wird nur der Einheitentyp angezeigt. |

## <a name="common-settings"></a>Allgemeine Einstellung
Die folgenden Abschnitte beschreiben Einstellungen für verschiedene Teile der Visualisierung.

### <a name="name-value-separator">Name / Wert-Trennzeichen</a>
Einzelnen Trennzeichen die Text-Eigenschaft aus einer Abfrage in mehrere Werte analysieren möchten.  Wenn Sie ein Trennzeichen angeben, erhalten Sie Namen für jedes Feld mit der gleichen im Namen voneinander getrennt.

Beispielsweise eine Eigenschaft namens *Speicherort* , die Werte wie *Redmond Gebäude 41* und *Bellevue Building12*enthalten.  Sie können – für Name und Wert Trennzeichen und *City Building* Namen angeben.  Dies würde jeder Wert in zwei Eigenschaften *Stadt* und *Gebäude*analysiert werden. 

### <a name="navigation-query">Navigationsbereich-Abfrage</a>
Abfrage ausgeführt, wenn der Benutzer ein Element in der Liste auswählt.  Verwenden Sie *{ausgewählten}* Syntax für Artikel enthalten, die der Benutzer ausgewählt.

Beispielsweise verfügt die Abfrage eine Spalte *Computer* und die Navigation Abfrage ist *{ausgewählte Element}*eine Abfrage wie *Computer = "Arbeitsplatz"* führen einen Computer ausgewählt.  Ist die Navigation Abfrage *Typ Ereignis {ausgewählte Element} =* die Abfrage *Typ = Ereignis Computer = "Arbeitsplatz"* würde ausgeführt.

### <a name="sparklines">Sparklines</a>
Eine Sparkline ist eine kleine Liniendiagramm, das den Wert einen Eintrag mit der Zeit zeigt.  Visualisierung Teile einer Liste können Sie wählen, ob einen horizontalen Balken zeigt den relativen Wert einer numerischen Spalte oder eine Sparkline, dessen Wert mit der Zeit angezeigt.

Die folgende Tabelle beschreibt die Einstellungen für Sparklines.

| Einstellung | Beschreibung |
|:--|:--|
| Sparklines aktivieren | Wählen Sie Sparkline statt horizontale Leiste angezeigt. |
| Vorgang | Sparklines aktiviert sind, wird die Operation für jede Eigenschaft in der Liste der Werte für die Sparkline berechnen ausführen.<br><br>-Letzte Beispiel: Letzte Wert für die Serie über das Zeitintervall.<br>-Max: Höchstwert für die Serie über das Zeitintervall.<br>-Min: Minimalwert für die Serie über das Zeitintervall.<br>-Summe: Die Summe der Werte für die Serie über das Zeitintervall.<br>-Zusammenfassung: Verwendet denselben messbefehl als die Abfrage in der Kopfzeile. |

### <a name="thresholds">Schwellenwerte</a>
Schwellenwerte können Sie ein farbiges Symbol neben jedem Element in einer Liste mit schnellen optisch von Elementen, die einen bestimmten Wert oder innerhalb eines bestimmten Bereichs anzeigen.  Beispielsweise können Sie ein grünes Symbol für Elemente mit einer akzeptablen Wert Gelb, wenn der Wert in einem Bereich, der eine Warnung angibt und Rot überschreitet einen Fehlerwert darstellen.

Wenn Sie Schwellenwerte für ein Teil aktivieren, müssen Sie eine oder mehrere Schwellenwerte angeben.  Ist der Wert eines Elements größer als ein Schwellenwert und niedriger als der nächsten Schwellenwert, wird diese Farbe verwendet.  Wenn das Element dann höchsten Schwellenwert überschreitet, wird diese Farbe festgelegt.   

Jeder Schwellenwert hat einen Schwellenwert mit **Standard**.  Dies ist die Farbe festgelegt, wenn keine anderen Werte überschritten werden.  Hinzufügen oder entfernen Schwellenwerte für die **+** oder **X** .

Die folgende Tabelle beschreibt die Einstellungen für Kapitalbeteiligungen.

| Einstellung | Beschreibung |
|:--|:--|
| Schwellenwerte aktivieren | Wählen Sie ein farbiges Symbol neben jeder Wert, der die Integrität relativ festgelegten Schwellenwerte angezeigt. |
| Name | Name der Schwellenwert zu. |
| Schwellenwert | Wert für den Schwellenwert.  Die Farbe der höchsten Schwellenwert überschritten durch den Wert des Elements Health Farbe für jedes Listenelement fest.  Es gibt einen Standardschwellenwert mit der Farbe keine Schwellenwerte überschritten. |
| Farbe | Farbe für den Schwellenwert. |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Abfragen teilweise Visualisierung unterstützen.