<properties
    pageTitle="Analytics Designer nebeneinander Ansichtsreferenz anmelden | Microsoft Azure"
    description="Ansicht-Designer im Protokollanalyse können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Konsole, die verschiedene Visualisierung von Daten in die OMS-Repository enthalten. Dieser Artikel enthält eine Einstellung aller Kacheln in benutzerdefinierten Ansichten verwendet."
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
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Log Analytics Ansichtsreferenz Designer nebeneinander
Ansicht-Designer im Protokollanalyse können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Konsole, die verschiedene Visualisierung von Daten in die OMS-Repository enthalten. Dieser Artikel enthält eine Einstellung aller Kacheln in benutzerdefinierten Ansichten verwendet.

Andere Artikel für Ansicht-Designer verfügbar sind:

- [Ansicht-Designer](log-analytics-view-designer.md) - Übersicht der Ansicht-Designer und Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.
- [Visualisierung Teilereferenz](log-analytics-view-designer-parts.md) - Verweis Einstellungen für alle Kacheln in benutzerdefinierten Ansichten verwendet. 


Die folgende Tabelle listet die verschiedenen Kacheln im Ansicht-Designer verfügbar.  Die folgenden Abschnitte beschreiben jede Kachel und deren Eigenschaften.

| Nebeneinander | Beschreibung |
|:--|:--|
| [Anzahl](#number-tile) | Einzelne Zahl zeigt die Anzahl der Datensätze aus einer Abfrage. |
| [Zwei Zahlen](#two-numbers-tile) | Zwei einfache Zahlen Anzahl Datensätze aus zwei unterschiedlichen Abfragen. |
| [Ring](#donut-tile) | Ringdiagramm basierend auf einer Abfrage mit einem zusammenfassenden Wert in der Mitte. |
| [Legende & Diagramm](#line-chart-amp-callout-tile) | Basierend auf einer Abfrage und eine Legende mit einem Summenwert Liniendiagramm. |
| [Liniendiagramm](#line-chart-tile) | Ein Liniendiagramm auf einer Abfrage basiert. |
| [Zwei Zeitpläne](#two-timelines-tile) | Säulendiagramm mit zwei jeweils eine separate Abfrage abhängig. |



## <a name="number-tile"></a>Anzahl nebeneinander

**Anzahl** nebeneinander zeigt eine einzelne Zahl zeigt die Anzahl der Datensätze aus einer Protokollabfrage und eine Bezeichnung.

![Anzahl nebeneinander](media/log-analytics-view-designer/tile-number.png)

| Einstellung | Beschreibung |
|:--|:--|
| Name        | Am oberen Rand der Kachel anzuzeigende Text. |
| Beschreibung | Unter dem Kachelnamen anzuzeigende Text.    |
| **Nebeneinander** |
| Legende | Unter dem Wert anzuzeigende Text. |
| Abfrage | Abfrage ausführen.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| **Erweiterte** |  **> Datenfluss Überprüfung** |
| Aktiviert | Wählen Sie Datenfluss Überprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung Wenn Daten nicht für die Kachel verfügbar ist.  Dies ist typischerweise eine Nachricht während des Zeitraums bereitstellen, wenn die Ansicht installiert und die Daten zur Verfügung. |
| Abfrage | Abfrage um zu überprüfen, ob für die Ansicht Daten ausgeführt.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle der Wert aus der Hauptabfrage angezeigt. |
| Nachricht | Anzuzeigende Meldung, wenn der Datenfluss Überprüfung Abfrage keine Daten zurückgegeben.  Wenn Sie keine Nachricht bereitstellen, wird *Bewertung durchführen* . |
| **Zeitintervall** |
| Dauer | Dauer ab dem aktuellen Datum für das Zeitintervall für die Abfrage verwendet.  Beispielsweise ist **7 Tage** angegeben ist, die Abfrage erstellt von 7 Tagen auf das aktuelle Datum auf. |
| Endoffset Daten | Optionaler Offset aus der aktuellen Daten für das Zeitintervall der Hauptabfrage.  Z. B. ist **-1 Tag** für **Endoffset Datum** und **7 Tagen** für die **Dauer**verwendet wird, die Abfrage auf 8 Tage vor gestern erstellt. |

## <a name="two-numbers-tile"></a>Zwei Zahlen nebeneinander

**Zwei** nebeneinander zeigt zwei Zahlen die Anzahl der Datensätze aus zwei verschiedenen Protokolldateien Abfragen und eine Bezeichnung für jede.

![Zwei Zahlen nebeneinander](media/log-analytics-view-designer/tile-two-numbers.png)

| Einstellung | Beschreibung |
|:--|:--|
| Name        | Am oberen Rand der Kachel anzuzeigende Text. |
| Beschreibung | Unter dem Kachelnamen anzuzeigende Text.    |
| **Erstes Musterelement** |
| Legende | Unter dem Wert anzuzeigende Text. |
| Abfrage | Abfrage ausführen.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| **Zweite Kachel** |
| Legende | Unter dem Wert anzuzeigende Text. |
| Abfrage | Abfrage ausführen.  Die Anzahl der von der Abfrage zurückgegebenen Datensätze werden angezeigt. |
| **Erweiterte** | **> Datenfluss Überprüfung** |
| Aktiviert | Wählen Sie Datenfluss Überprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung Wenn Daten nicht für die Kachel verfügbar ist.  Dies ist typischerweise eine Nachricht während des Zeitraums bereitstellen, wenn die Ansicht installiert und die Daten zur Verfügung. |
| Abfrage | Abfrage um zu überprüfen, ob für die Ansicht Daten ausgeführt.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle der Wert aus der Hauptabfrage angezeigt. |
| Nachricht | Anzuzeigende Meldung, wenn der Datenfluss Überprüfung Abfrage keine Daten zurückgegeben.  Wenn Sie keine Nachricht bereitstellen, wird *Bewertung durchführen* . |
| **Zeitintervall** |
| Dauer | Dauer ab dem aktuellen Datum für das Zeitintervall für die Abfrage verwendet.  Beispielsweise ist **7 Tage** angegeben ist, die Abfrage erstellt von 7 Tagen auf das aktuelle Datum auf. |
| Endoffset Daten | Optionaler Offset aus der aktuellen Daten für das Zeitintervall der Hauptabfrage.  Z. B. ist **-1 Tag** für **Endoffset Datum** und **7 Tagen** für die **Dauer**verwendet wird, die Abfrage auf 8 Tage vor gestern erstellt. |

## <a name="donut-tile"></a>Donut-Kachel

**Donut** Kachel zeigt eine einzelne Zahl aus einer Wertespalte eine Protokollabfrage zusammengefasst.  Der Ring zeigt grafisch Ergebnisse der oberen drei Datensätze.

![Donut-Kachel](media/log-analytics-view-designer/tile-donut.png)

| Einstellung | Beschreibung |
|:--|:--|
| Name        | Am oberen Rand der Kachel anzuzeigende Text. |
| Beschreibung | Unter dem Kachelnamen anzuzeigende Text.    |
| **Ring** |
| Abfrage | Abfrage für den Donut ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Dies ist normalerweise eine Abfrage, die **Maßnahme** -Schlüsselwort verwendet, Ergebnisse zusammengefasst. |
| **Ring** | **> Center** |
| Text | Unter den Wert in den Ring anzuzeigende Text. |
| Vorgang | Der Vorgang auf die Value-Eigenschaft in einen einzelnen Wert zusammengefasst.<br><br>-Summe: Die Werte aller Datensätze mit dem Eigenschaftswert hinzufügen.<br>-Prozentsatz: Prozentsatz der summierten Werte von Datensätzen mit dem Eigenschaftswert auf die summierten Werte aller Datensätze verglichen. |
| Ergebniswerte in Betrieb verwendet | Klicken Sie optional auf das Pluszeichen einen oder mehrere Werte hinzufügen.  Die Ergebnisse der Abfrage werden Datensätze mit den Eigenschaftswerten auf die Sie angeben.  Wenn keine Werte hinzugefügt werden, als alle Datensätze in der Abfrage enthalten sind. |
| **Ring** | **> Weitere Optionen** |
| Farben | Die Farbe für alle drei oberen Eigenschaften angezeigt.  Wenn Sie andere Farben für bestimmte Eigenschaftswerte angeben möchten, verwenden Sie erweiterte Farbe zuordnen. |
| Erweiterte Zuordnung | Zeigt eine bestimmte Eigenschaftswerte.  Ist der Wert in den oberen drei ist die Alternative Farbe statt der Standardfarbe angezeigt.  Wenn die Eigenschaft nicht in den oberen drei Farbe nicht angezeigt wird. |
| **Erweiterte** | **> Datenfluss Überprüfung** |
| Aktiviert | Wählen Sie Datenfluss Überprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung Wenn Daten nicht für die Kachel verfügbar ist.  Dies ist typischerweise eine Nachricht während des Zeitraums bereitstellen, wenn die Ansicht installiert und die Daten zur Verfügung. |
| Abfrage | Abfrage um zu überprüfen, ob für die Ansicht Daten ausgeführt.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle der Wert aus der Hauptabfrage angezeigt. |
| Nachricht | Anzuzeigende Meldung, wenn der Datenfluss Überprüfung Abfrage keine Daten zurückgegeben.  Wenn Sie keine Nachricht bereitstellen, wird *Bewertung durchführen* . |
| **Zeitintervall** |
| Dauer | Dauer ab dem aktuellen Datum für das Zeitintervall für die Abfrage verwendet.  Beispielsweise ist **7 Tage** angegeben ist, die Abfrage erstellt von 7 Tagen auf das aktuelle Datum auf. |
| Endoffset Daten | Optionaler Offset aus der aktuellen Daten für das Zeitintervall der Hauptabfrage.  Z. B. ist **-1 Tag** für **Endoffset Datum** und **7 Tagen** für die **Dauer**verwendet wird, die Abfrage auf 8 Tage vor gestern erstellt. |

## <a name="line-chart-tile"></a>Zeile Diagramm nebeneinander

Kachel **Liniendiagramm** zeigt ein Liniendiagramm mit mehreren Serien aus eine Protokollabfrage langfristig.  

![Linien- & Legende Kachel](media/log-analytics-view-designer/tile-line-chart.png)

| Einstellung | Beschreibung |
|:--|:--|
| Name        | Am oberen Rand der Kachel anzuzeigende Text. |
| Beschreibung | Unter dem Kachelnamen anzuzeigende Text.    |
| **Liniendiagramm** |  
| Abfrage | Abfrage für das Liniendiagramm ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Dies ist normalerweise eine Abfrage, die **Maßnahme** -Schlüsselwort verwendet, Ergebnisse zusammengefasst.  Wenn die Abfrage das Schlüsselwort **Intervall** verwendet verwendet die X-Achse des Diagramms dieses Zeitintervall.  Wenn die Abfrage nicht das Schlüsselwort **Intervall** werden stündlich für die X-Achse verwendet. |
| **Liniendiagramm** | **> Y-Achse** |
| Logarithmische Skalierung verwenden | Wählen Sie für die Y-Achse eine logarithmische Skalierung verwendet. |
| Einheiten | Geben Sie die Einheit für die von der Abfrage zurückgegebenen Werte.  Diese Informationen zum Etiketten auf dem Diagramm Werttypen und optional für das Konvertieren der Werte angezeigt.  **Typ** gibt die Kategorie der Einheit und definiert die **Aktuelle Einheitentyp** Werte verfügbar.  Wenn Sie einen Wert im **Konvertieren** auswählen werden die numerischen Werte aus der **Aktuellen Einheit** Typ **Konvertieren** konvertiert. |
| Benutzerdefiniertes Etikett | Für die Y-Achse neben der Bezeichnung für den Einheitentyp anzuzeigende Text.  Wenn keine Bezeichnung angegeben ist, wird nur der Einheitentyp angezeigt. |
| **Erweiterte** | **> Datenfluss Überprüfung** |
| Aktiviert | Wählen Sie Datenfluss Überprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung Wenn Daten nicht für die Kachel verfügbar ist.  Dies ist typischerweise eine Nachricht während des Zeitraums bereitstellen, wenn die Ansicht installiert und die Daten zur Verfügung. |
| Abfrage | Abfrage um zu überprüfen, ob für die Ansicht Daten ausgeführt.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle der Wert aus der Hauptabfrage angezeigt. |
| Nachricht | Anzuzeigende Meldung, wenn der Datenfluss Überprüfung Abfrage keine Daten zurückgegeben.  Wenn Sie keine Nachricht bereitstellen, wird *Bewertung durchführen* . |
| **Zeitintervall** |
| Dauer | Dauer ab dem aktuellen Datum für das Zeitintervall für die Abfrage verwendet.  Beispielsweise ist **7 Tage** angegeben ist, die Abfrage erstellt von 7 Tagen auf das aktuelle Datum auf. |
| Endoffset Daten | Optionaler Offset aus der aktuellen Daten für das Zeitintervall der Hauptabfrage.  Z. B. ist **-1 Tag** für **Endoffset Datum** und **7 Tagen** für die **Dauer**verwendet wird, die Abfrage auf 8 Tage vor gestern erstellt. |


## <a name="line-chart--callout-tile"></a>Diagramm & Legende Kachel Zeile

**Linien- & Legende** Kachel zeigt ein Liniendiagramm mit mehreren Serien aus eine Protokollabfrage und eine Legende mit einem zusammengefassten Wert.  

![Linien- & Legende Kachel](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Einstellung | Beschreibung |
|:--|:--|
| Name        | Am oberen Rand der Kachel anzuzeigende Text. |
| Beschreibung | Unter dem Kachelnamen anzuzeigende Text.    |
| **Liniendiagramm** |  
| Abfrage | Abfrage für das Liniendiagramm ausgeführt.  Die erste Eigenschaft sollte einen Textwert und die zweite Eigenschaft einen numerischen Wert.  Dies ist normalerweise eine Abfrage, die **Maßnahme** -Schlüsselwort verwendet, Ergebnisse zusammengefasst.  Wenn die Abfrage das Schlüsselwort **Intervall** verwendet verwendet die X-Achse des Diagramms dieses Zeitintervall.  Wenn die Abfrage nicht das Schlüsselwort **Intervall** werden stündlich für die X-Achse verwendet. |
| **Liniendiagramm** | **> Legende** |
| Legende | Titeltext Legende Wert angezeigt. |
| Name der Datenreihe | Der Eigenschaftswert für die Datenreihe für die Legende Wert.  Wenn keine Datenreihe angegeben wird, werden alle Datensätze von der Abfrage verwendet. |
| Vorgang | Der Vorgang auf die Value-Eigenschaft einen Wert für die Legende zusammenfassen.<br>-Durchschnitt: Durchschnittliche Wert aus allen Datensätzen.<br><br>-Anzahl: Alle Datensätze von der Abfrage zurückgegeben.<br>-Letzte Beispiel: Wert aus dem letzten Intervall im Diagramm enthalten.<br>-Max: Höchstwert Intervalle im Diagramm enthalten.<br>-Min: Minimalwert Intervalle im Diagramm enthalten.<br>-Summe: Summe aus allen Datensätzen. |
| **Liniendiagramm** | **> Y-Achse** |
| Logarithmische Skalierung verwenden | Wählen Sie für die Y-Achse eine logarithmische Skalierung verwendet. |
| Einheiten | Geben Sie die Einheit für die von der Abfrage zurückgegebenen Werte.  Diese Informationen zum Etiketten auf dem Diagramm Werttypen und optional für das Konvertieren der Werte angezeigt.  **Typ** gibt die Kategorie der Einheit und definiert die **Aktuelle Einheitentyp** Werte verfügbar.  Wenn Sie einen Wert im **Konvertieren** auswählen werden die numerischen Werte aus der **Aktuellen Einheit** Typ **Konvertieren** konvertiert. |
| Benutzerdefiniertes Etikett | Für die Y-Achse neben der Bezeichnung für den Einheitentyp anzuzeigende Text.  Wenn keine Bezeichnung angegeben ist, wird nur der Einheitentyp angezeigt. |
| **Erweiterte** | **> Datenfluss Überprüfung** |
| Aktiviert | Wählen Sie Datenfluss Überprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung Wenn Daten nicht für die Kachel verfügbar ist.  Dies ist typischerweise eine Nachricht während des Zeitraums bereitstellen, wenn die Ansicht installiert und die Daten zur Verfügung. |
| Abfrage | Abfrage um zu überprüfen, ob für die Ansicht Daten ausgeführt.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle der Wert aus der Hauptabfrage angezeigt. |
| Nachricht | Anzuzeigende Meldung, wenn der Datenfluss Überprüfung Abfrage keine Daten zurückgegeben.  Wenn Sie keine Nachricht bereitstellen, wird *Bewertung durchführen* . |
| **Zeitintervall** |
| Dauer | Dauer ab dem aktuellen Datum für das Zeitintervall für die Abfrage verwendet.  Beispielsweise ist **7 Tage** angegeben ist, die Abfrage erstellt von 7 Tagen auf das aktuelle Datum auf. |
| Endoffset Daten | Optionaler Offset aus der aktuellen Daten für das Zeitintervall der Hauptabfrage.  Z. B. ist **-1 Tag** für **Endoffset Datum** und **7 Tagen** für die **Dauer**verwendet wird, die Abfrage auf 8 Tage vor gestern erstellt. |

## <a name="two-timelines-tile"></a>Zwei Zeitpläne Kachel

Die **zwei Zeitpläne** Kachel zeigt die Ergebnisse Protokoll Abfragen wie Säulendiagramme.  Jede Reihe wird eine Legende angezeigt.  

![Zwei Zeitpläne Kachel](media/log-analytics-view-designer/tile-two-timelines.png)

| Einstellung | Beschreibung |
|:--|:--|
| Name        | Am oberen Rand der Kachel anzuzeigende Text. |
| Beschreibung | Unter dem Kachelnamen anzuzeigende Text.    |
| Erste Diagramm   
| Legende | Unter die Legende der ersten Datenreihe anzuzeigende Text.
| Farbe | Farbe für die Spalten in der ersten Reihe.
| Diagramm-Abfrage | Abfrage für die erste Datenreihe ausgeführt.  Die Anzahl der Datensätze über jedes Zeitintervall wird durch die Säulen im Diagramm dargestellt.
| Vorgang | Der Vorgang auf die Value-Eigenschaft einen Wert für die Legende zusammenfassen.<br><br>-Durchschnitt: Durchschnittliche Wert aus allen Datensätzen.<br>-Anzahl: Alle Datensätze von der Abfrage zurückgegeben.<br>-Letzte Beispiel: Wert aus dem letzten Intervall im Diagramm enthalten.<br>-Max: Höchstwert Intervalle im Diagramm enthalten.
| **Im zweiten Diagramm** |
| Legende | In der Legende für die zweite Reihe anzuzeigende Text.
| Farbe | Farbe für die Spalten in der zweiten Reihe.
| Diagramm-Abfrage | Abfrage für die zweite Reihe ausführen.  Die Anzahl der Datensätze über jedes Zeitintervall wird durch die Säulen im Diagramm dargestellt.
| Vorgang | Der Vorgang auf die Value-Eigenschaft einen Wert für die Legende zusammenfassen.<br><br>-Durchschnitt: Durchschnittliche Wert aus allen Datensätzen.<br>-Anzahl: Alle Datensätze von der Abfrage zurückgegeben.<br>-Letzte Beispiel: Wert aus dem letzten Intervall im Diagramm enthalten.<br>-Max: Höchstwert Intervalle im Diagramm enthalten. |
| **Erweiterte** | **> Datenfluss Überprüfung** |
| Aktiviert | Wählen Sie Datenfluss Überprüfung für die Kachel aktiviert werden soll.  Dadurch wird eine andere Meldung Wenn Daten nicht für die Kachel verfügbar ist.  Dies ist typischerweise eine Nachricht während des Zeitraums bereitstellen, wenn die Ansicht installiert und die Daten zur Verfügung. |
| Abfrage | Abfrage um zu überprüfen, ob für die Ansicht Daten ausgeführt.  Wenn die Abfrage keine Ergebnisse zurückgibt, wird eine Meldung anstelle der Wert aus der Hauptabfrage angezeigt. |
| Nachricht | Anzuzeigende Meldung, wenn der Datenfluss Überprüfung Abfrage keine Daten zurückgegeben.  Wenn Sie keine Nachricht bereitstellen, wird *Bewertung durchführen* . |
| **Zeitintervall** |
| Dauer | Dauer ab dem aktuellen Datum für das Zeitintervall für die Abfrage verwendet.  Beispielsweise ist **7 Tage** angegeben ist, die Abfrage erstellt von 7 Tagen auf das aktuelle Datum auf. |
| Endoffset Daten | Optionaler Offset aus der aktuellen Daten für das Zeitintervall der Hauptabfrage.  Z. B. ist **-1 Tag** für **Endoffset Datum** und **7 Tagen** für die **Dauer**verwendet wird, die Abfrage auf 8 Tage vor gestern erstellt. |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Abfragen in Kacheln unterstützen.
- Die benutzerdefinierte Ansicht [Visualisierung Teile](log-analytics-view-designer-parts.md) hinzufügen.