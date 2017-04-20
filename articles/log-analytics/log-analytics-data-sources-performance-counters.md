<properties 
   pageTitle="Windows und Linux Leistungsindikatoren in Protokollanalyse | Microsoft Azure"
   description="Leistungsindikatoren werden durch Protokollanalyse zur Leistungsanalyse auf Windows- und Linux-Agents gesammelt.  Dieser Artikel beschreibt die Sammlung der Leistungsindikatoren für Windows konfigurieren und Linux-Agenten, Details der OMS-Repository und im Portal OMS Analyse gespeichert."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows und Linux Leistung Datenquellen Protokollanalyse 

Leistungsindikatoren in Windows und Linux Einblicke in die Leistung von Hardwarekomponenten, Betriebssystem und Applikationen.  Protokollanalyse können in regelmäßigen Abständen nahe Echtzeit (NRT) Analyse neben aggregieren Daten länger Begriff Analyse und Berichterstattung Leistungsindikatoren erfassen.

![Leistungsindikatoren](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurieren von Leistungsindikatoren

Konfigurieren der Leistungsindikatoren aus dem [Menü Daten in Analytics Einstellungen](log-analytics-data-sources.md#configuring-data-sources).

Beim Konfigurieren von Windows oder Linux Leistung Leistungsindikatoren für einen neuen OMS Arbeitsbereich erhalten Sie schnell mehrere allgemeine Leistungsindikatoren erstellen.  Sie werden mit einem Kontrollkästchen neben jedem aufgeführt.  Sicherstellen Sie, dass alle gewünschten zunächst Leistungsindikatoren überprüft werden und klicken Sie auf **die ausgewählten Leistungsindikatoren hinzufügen**.

![Konfigurieren von Windows-Leistungsindikatoren](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Gehen Sie zum Hinzufügen eines neuen Windows-Leistungsindikators sammeln.

1. Geben Sie den Namen des Leistungsindikators im Format- *Objekt (Instanz) \counter*im Textfeld.  Wenn Sie eingeben, wird eine entsprechende Liste Allgemeine Indikatoren angezeigt.  Sie können entweder einen Leistungsindikator aus der Liste oder geben Sie eine eigene auswählen.  Sie können auch alle Instanzen eines bestimmten Leistungsindikators zurückgeben, indem *das Objekt/Datenquelle*angeben. 
2. Klicken Sie auf **+** oder Drücken der **EINGABETASTE** den Leistungsindikator zur Liste hinzuzufügen.
3. Wenn Sie einen Leistungsindikator hinzufügen, verwendet den Standardwert von 10 Sekunden für das **Abtastintervall**.  Sie können dies höher 1800 Sekunden (30 Minuten) ändern Sie Speicherbedarf gesammelten Leistungsdaten zu.
4. Beim Hinzufügen von Leistungsindikatoren fertig sind, klicken Sie oben auf dem Bildschirm, um die Konfiguration zu speichern auf **Speichern** .

![Konfigurieren von Linux-Leistungsindikatoren](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Gehen Sie zum Hinzufügen eines neuen Linux-Leistungsindikators sammeln.

1. Standardmäßig werden alle Neukonfiguration automatisch an alle Agenten abgelegt.  Für Linux-Agenten wird eine Konfigurationsdatei Datensammlungsprogramm Fluentd gesendet.  Möchten Sie diese Datei auf jedem Linux-Agent manuell ändern, dann deaktivieren Sie das Kontrollkästchen *unter Konfiguration meiner Linux-Computer anwenden*.
2. Geben Sie den Namen des Leistungsindikators im Format- *Objekt (Instanz) \counter*im Textfeld.  Wenn Sie eingeben, wird eine entsprechende Liste Allgemeine Indikatoren angezeigt.  Sie können entweder einen Leistungsindikator aus der Liste oder geben Sie eine eigene auswählen.  
2. Klicken Sie auf **+** oder **Geben Sie** den Zähler der Liste weitere Leistungsindikatoren für das Objekt hinzufügen.
3. Alle Leistungsindikatoren für ein Objekt verwenden das gleiche **Abtastintervall**.  Der Standardwert ist 10 Sekunden.  Sie ändern einen höheren Wert 1800 Sekunden (30 Minuten) möchten Sie verringern den Speicherbedarf Gesammelte Leistungsdaten.
4. Beim Hinzufügen von Leistungsindikatoren fertig sind, klicken Sie oben auf dem Bildschirm, um die Konfiguration zu speichern auf **Speichern** .

## <a name="data-collection"></a>Datensammlung

Protokollanalyse erfasst alle angegebenen Leistungsindikatoren die angegebenen Abtastintervall auf alle Agents mit installiertem Zähler.  Die Daten nicht aggregiert und Rohdaten steht in allen Ansichten der Protokoll-Suche für die Dauer Ihres Abonnements OMS angegeben.


## <a name="performance-record-properties"></a>Eigenschaften von Leistung

Performance-Rekorde haben ein **Perf** und Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer         | Computer, auf dem das Ereignis entnommen wurde. |
| CounterName      | Name des Leistungsindikators |
| CounterPath      | Vollständigen Pfad des Leistungsindikators in der Form \\ \\ \<Computer >\\Objekt(Instanz)\\Zähler. |
| Gegenwert     | Numerische Wert des Zählers.  |
| Instanzname     | Name der Ereignisinstanz.  Leer, wenn keine Instanz. |
| Objektname       | Name des Leistungsobjekts |
| SourceSystem  | Typ des Agents gesammelten Daten aus. <br> OpsManager-Windows Agent, direkt verbinden oder SCOM <br> Linux-Linux-Agenten  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated       | Datum und Uhrzeit Daten aufgenommen wurde. |


## <a name="sizing-estimates"></a>Schätzt die Größe

 Eine grobe Schätzung für die Auflistung eines bestimmten Leistungsindikators in Intervallen von 10 Sekunden ist ca. 1 MB pro Instanz.  Schätzen Sie den Speicherbedarf eines bestimmten Indikators mit der folgenden Formel.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Protokolldatei suchen mit Leistungsnachweis

Die folgende Tabelle bietet unterschiedliche Beispiele für Protokoll-Suchvorgänge, die Leistungsdaten abzurufen.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = Perf | Alle Leistungsdaten |
| Typ = Perf Computer = "Arbeitsplatz" | Alle Leistungsdaten von einem bestimmten computer |
| Typ = Perf CounterName = "Aktuelle Warteschlangenlänge" | Alle Leistungsdaten für einen bestimmten Indikator |
| Typ = Perf (Objektname = Prozessor) CounterName = "% Prozessorzeit" InstanceName = _Total & #124; Avg(Average) AVGCPU Computer messen | Durchschnittliche CPU-Auslastung auf allen Computern |
| Typ = Perf (CounterName = "Prozessorzeit (%)") & #124;  max(Max) Computer messen | Maximale CPU-Auslastung auf allen Computern |
| Typ = Perf Objektname = logischer CounterName = Computer "Aktueller Datenträger Warteschlangenlänge" = "MyComputerName" & #124; Avg(Average) von InstanceName messen | Durchschnittliche Aktuelle Warteschlangenlänge für alle Instanzen eines bestimmten Computers |
| Typ = Perf CounterName = "DiskTransfers/s" & #124; percentile95(Average) Computer messen | 95. Quantil der Festplatte/s auf allen Computern |
| Typ = Perf CounterName = "% Prozessorzeit" InstanceName = "_Total" & #124; avg(CounterValue) Computer Intervall 1 Stunde messen | Stündliche durchschnittliche CPU-Nutzung auf allen Computern |
| Typ = Perf Computer "MyComputer" CounterName = = % * InstanceName = _Total & #124; percentile70(CounterValue) CounterName Intervall 1 Stunde messen | Stündliche 70 %-Quantil der jeder Leistungsindikator % Prozent für einen bestimmten computer |
| Typ = Perf CounterName = "% Prozessorzeit" InstanceName = "_Total" (Computer = "Arbeitsplatz") & #124; Messen Sie min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) Computer Intervall 1 Stunde | Stündliche Mittelwert, minimum, maximum und 75 Quantil CPU-Auslastung für einen bestimmten computer |

## <a name="viewing-performance-data"></a>Anzeigen von Leistungsdaten

Beim Ausführen von Protokoll suchen Leistungsdaten ist die **Protokoll** -Ansicht standardmäßig angezeigt.  Um die Daten grafisch anzuzeigen, klicken Sie auf **Kriterien**.  Detaillierte grafisch, klicken Sie auf die **+** neben einem Zähler.  

![Metriken Ansicht reduziert](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Wenn der ausgewählten Zeitraum 6 Stunden oder weniger wird im Diagramm alle paar Sekunden aktualisiert.  Live-Daten werden auf der rechten Seite des Diagramms in blau angezeigt.

![Metriken Ansicht Erweitert mit live-Daten](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Aggregieren von Daten in einer Protokolldatei suchen, finden [bei Bedarf metrische Aggregation und Visualisierung in OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren.  
- Exportieren Sie Daten in [Power BI](log-analytics-powerbi.md) zusätzliche Visualisierung und Analyse.