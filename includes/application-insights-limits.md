Grenzen für die Anzahl von Metriken und Ereignissen pro Anwendung sind (d. h. pro instrumentationsschlüssel). 

Grenzen hängen die [Preisstufe](https://azure.microsoft.com/pricing/details/application-insights/) , die Sie auswählen.

**Ressource** | **Standardlimit** | **Maximale Anzahl**
-------- | ------------- | -------------
Sitzungsdaten Punkte<sup>1, 2</sup> pro Monat | unbegrenzt | 
Total Datenpunkte pro Monat für Anforderung, Ereignis Abhängigkeit, Trace, Ausnahme und Seitenansicht | 5 Millionen | 50 Millionen<sup>3</sup>
Datenrate [verfolgen und protokollieren](../articles/application-insights/app-insights-search-diagnostic-logs.md) | 200 dp-s | 500 dp-s
[Ausnahme](../articles/application-insights/app-insights-asp-net-exceptions.md) -Datenrate | 50 dp-s | 50 dp-s
Gesamte Datenrate für Anforderung, Ereignis Abhängigkeit und Seite Ansicht Telemetrie | 200 dp-s | 500 dp-s
Rohdaten Aufbewahrung für [Suche](../articles/application-insights/app-insights-diagnostic-search.md) und [Analyse](../articles/application-insights/app-insights-analytics.md) | 7 Tage
Aggregierte datenaufbewahrung [Metrik-explorer](../articles/application-insights/app-insights-metrics-explorer.md) | 90 Tage
Anzahl der [Eigenschaft](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) name | 100 |
Eigenschaftslänge | 150 | 
Eigenschaft Wertlänge | 8192 | 
Trace und Ausnahme Länge | 10000 |
Anzahl [Metrik](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) | 100 |
Metrische Länge |  150 | 
[Verfügbarkeitstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> ein Datenpunkt ist eine einzelne Metrikwert oder Ereignis angefügten Eigenschaften mit Maßeinheiten.

<sup>2</sup> ein Datenpunkt meldet sich am Anfang oder Ende einer Sitzung und Identität protokolliert.

<sup>3</sup> zusätzlichen Kapazität 50 Millionen erhältlich.
 
[Zu Preisen und Quoten Anwendung Einblicke](../articles/application-insights/app-insights-pricing.md)
