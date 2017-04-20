<properties 
    pageTitle="Analytics - mächtiges Werkzeug Anwendung Erkenntnisse | Microsoft Azure" 
    description="Übersicht über Analytics Diagnose mächtiges Werkzeug Anwendung Erkenntnisse. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>Analytics Anwendung Erkenntnisse


[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion [Anwendung](app-insights-overview.md)Erkenntnisse. Diese Seiten beschreiben Analytics Abfrage Lanquage. 

* **[Das Einführungsvideo anzeigen](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testen Analytics auf die simulierten Daten](https://analytics.applicationinsights.io/demo)** Wenn Ihre Anwendung Daten noch Anwendung Erkenntnisse mitsendet.

## <a name="queries-in-analytics"></a>Abfragen in Analytics
 
Eine normale Abfrage ist *eine Quelltabelle gefolgt von einer Reihe von *Operatoren* getrennt durch* `|`. 

Entdecken Sie beispielsweise, welcher Tageszeit Bürger Hyderabad unserer WebApp versuchen. Und während es Aufenthalts sehen ihre HTTP-Anfragen welche Ergebniscodes zurückgegeben werden. 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Wir haben unterschiedliche IP-Adressen, nach der Tageszeit gruppieren, der letzten 7 Tage. 

Wir zeigen Sie die Ergebnisse mit der Präsentation Balkendiagramm, die Ergebnisse von verschiedenen Antwortcodes Stapeln auswählen:

![Wählen Sie Balkendiagramm, X und y-Achsen und Segmentierung](./media/app-insights-analytics/020.png)

Sieht aus wie die app beliebtesten mittags und Bett Zeit in Hyderabad. (Und untersuchen wir die 500 Codes)


Gibt statistische Aufgaben:

![](./media/app-insights-analytics/025.png)


Die Sprache hat viele attraktive Funktionen:

* [Filter](app-insights-analytics-reference.md#where-operator) der Telemetrie raw app durch alle Felder, einschließlich benutzerdefinierter Eigenschaften und Metriken.
* [Verknüpfen](app-insights-analytics-reference.md#join-operator) mehrerer Tabellen – korrelieren Anfragen Seitenansichten und Abhängigkeit Aufrufe, Ausnahmen Protokoll verfolgt.
* Leistungsstarke statistische [Aggregationen](app-insights-analytics-reference.md#aggregations).
* So leistungsstark wie SQL, jedoch für komplexe Abfragen einfacher: anstelle von verschachtelten Anweisungen Daten aus einem elementaren Vorgang zu leiten.
* Sofortige und leistungsfähige Visualisierung.







## <a name="connect-to-your-application-insights-data"></a>Ihre Anwendung Einblicke Daten


Öffnen Sie Ihre app [Übersicht Blade](app-insights-dashboards.md) Anwendung Erkenntnisse Analytics: 

![Open portal.azure.com öffnen Application Insights-Ressource und Analysen auf.](./media/app-insights-analytics/001.png)


## <a name="limits"></a>Grenzen

Derzeit sind Abfrageergebnisse auf mehr als einer Woche Daten.



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>Nächste Schritte


* Wir empfehlen die [Sprache Tour](app-insights-analytics-tour.md)starten.