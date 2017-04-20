<properties 
    pageTitle="Anwendung Karte Anwendung Erkenntnisse | Microsoft Azure" 
    description="Eine visuelle Darstellung der Abhängigkeiten Anwendung Komponenten mit KPIs und Warnungen gekennzeichnet." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Anwendung Karte Anwendung Einblicke

In [Visual Studio Application Insights](app-insights-overview.md)ist Anwendung Karte visuelle Layout Abhängigkeit Beziehungen die Anwendungskomponenten. Jede Komponente werden KPIs laden, Leistung, Fehler und Warnungen, jede Komponente verursacht ein Performance-Problem oder Fehler aufgedeckt werden. Sie können durch Klicken von jeder Komponente weitere Diagnose von Application Insights-nutzt Ihre app Azure Services - Azure Diagnostics wie SQL Datenbank Advisor Recommendations.

Wie andere Diagramme können Sie eine Anwendung Zuordnung Azure Dashboard anheften, voll funktionsfähig ist. 

## <a name="open-the-application-map"></a>Öffnen Sie die Anwendung Zuordnung

Öffnen Sie die Karte aus dem Blade Übersicht für Ihre Anwendung:

![app-Zuordnung öffnen](./media/app-insights-app-map/01.png)

![App-Karte](./media/app-insights-app-map/02.png)

Die Karte zeigt:

* Verfügbarkeitstests
* Client-seitige Komponenten (überwacht JavaScript-SDK)
* Serverbasierten Komponente
* Abhängig von der Client- und Serverkomponenten

Erweitern und reduzieren Linkgruppen Abhängigkeit:

![Reduzieren](./media/app-insights-app-map/03.png)
 
Haben Sie eine große Anzahl von Abhängigkeiten eines Typs (SQL, HTTP usw.), können sie gruppierte erscheinen. 


![gruppierte Abhängigkeiten](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Probleme

Jeder Knoten hat relevanten Leistungsindikatoren wie die Auslastung und Leistung Fehler für diese Komponente. 

Warnsymbole markieren Sie mögliche Probleme. Orange Warnung bedeutet, dass Fehler Anfragen Seitenansichten oder Abhängigkeitseigenschaft aufrufen. Rot bedeutet eine Fehlerquote über 5 %.


![Fehler-Symbole](./media/app-insights-app-map/04.png)

 
Aktive warnt auch anzeigen von: 


![aktive Alarme](./media/app-insights-app-map/05.png)
 
Verwenden Sie SQL Azure, wird ein Symbol für die beim werden zur Verbesserung der Leistung. 


![Azure-Empfehlung](./media/app-insights-app-map/06.png)

Klicken Sie auf ein Symbol, um weitere Informationen zu erhalten:


![Azure-Empfehlung](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnose durch Klicken

Alle Knoten auf der Karte bietet gezielte Klicken durch auf Diagnose. Die Optionen variieren je nach Art des Knotens.

![Serveroptionen](./media/app-insights-app-map/09.png)

 
Für Komponenten, die in Azure gehostet werden, enthalten die Optionen Links direkt zu ihnen.


## <a name="filters-and-time-range"></a>Filter und Zeitraum

Standardmäßig werden die Zuordnung aller Daten für den ausgewählten Zeitraum zusammengefasst. Aber Sie können filtern, um nur bestimmte Operationsnamen oder Abhängigkeiten enthalten.

* Vorgangsname: Hierbei Seitenaufrufe und Anforderungstypen für Server-Seite. Diese Option zeigt die Karte KPI auf Server-Client Side-Knoten für die ausgewählten Vorgänge. Die Abhängigkeit im Kontext dieser bestimmten Operationen aufgerufen wird.
* Basisname Abhängigkeit: Hierbei AJAX Browser Seite Abhängigkeiten und Server Seite abhängig. Wenn Sie benutzerdefinierte Abhängigkeit Telemetrie API TrackDependency melden, werden sie auch hier zeigen. Sie können die Abhängigkeit auf der Karte auswählen. Beachten Sie, dass zu diesem Zeitpunkt dies keine Anfragen Seite Server oder Client Side Seitenansichten herausfiltert.


![Filter einrichten](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Filter speichern

Speichern der Filter Sie angewendet haben, fixieren die gefilterte Ansicht auf einem [Dashboard](app-insights-dashboards.md).


![Dashboard anheften](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Feedback

Bitte [Geben Sie Feedback über das Portal Bewertungsoption](app-insights-get-dev-support.md).


![1 Link Bild](./media/app-insights-app-map/13.png)


