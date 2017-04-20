<properties 
    pageTitle="Mit Diagnose suchen | Microsoft Azure" 
    description="Suchen Sie und Filtern Sie einzelner Ereignisse, Anfragen und protokollieren Sie Spuren." 
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
    ms.date="06/09/2016" 
    ms.author="awills"/>
 
# <a name="using-diagnostic-search-in-application-insights"></a>Diagnostische Suchfunktion Anwendung Erkenntnisse

Diagnostische Suche ist ein Feature von [Application Insights] [ start] , suchen und entdecken einzelne Telemetrie, wie Seitenaufrufe, Ausnahmen oder web-Anfragen zu verwenden. Und Sie können Spuren Protokoll und Ereignisse, die Sie programmiert haben.

## <a name="where-do-you-see-diagnostic-search"></a>Wo sehen Sie diagnostische suchen?


### <a name="in-the-azure-portal"></a>Im Azure-portal

Sie können explizit Diagnose Suche öffnen:

![Diagnostische Suche öffnen](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)


Es wird geöffnet, wenn Sie durch einige Diagramme und Raster klicken. In diesem Fall werden Filter vor sich auf den Typ des Elements ausgewählten festgelegt. 

Ein Webdienst zeigt die Anwendung Übersicht Blade beispielsweise ein Diagramm der Abfragen. Klicken sie, und man eine detailliertere Tabelle eine Liste anzeigen, wie viele Anfragen für jede URL hat. Klicken Sie auf eine beliebige Zeile, und Sie erhalten eine Liste der einzelnen Anfragen für die URL:

![Diagnostische Suche öffnen](./media/app-insights-diagnostic-search/07-open-from-filters.png)


Hauptteil Diagnosesuche wird eine Liste der telemetrieelemente - Clientanforderungen Seite Ansichten benutzerdefinierte Ereignisse programmiert haben, und. Am Anfang der Liste ist eine Übersicht mit der Anzahl der Ereignisse mit der Zeit.

Ereignisse werden normalerweise in Diagnose Suchen in metrischen Explorer angezeigt. Obwohl das Blade selbst in Intervallen aktualisiert, können Sie aktualisieren klicken, wenn Sie für ein bestimmtes Ereignis warten.


### <a name="in-visual-studio"></a>In Visual Studio

Öffnen Sie das Fenster suchen, in Visual Studio:

![](./media/app-insights-diagnostic-search/32.png)

Das Fenster Suchen hat die gleichen Funktionen wie das Web-Portal:

![](./media/app-insights-diagnostic-search/34.png)


## <a name="sampling"></a>Probenahme

Wenn Ihre Anwendung viele Telemetrie generiert (und ASP.NET SDK Version 2.0.0-beta3 oder höher), adaptive Sampling-Modul reduziert das Volume, das das Portal an einen repräsentativen Teil Ereignisse senden. Jedoch wird Ereignisse bezüglich der gleichen Anforderung ausgewählt oder als Gruppe deaktiviert, damit Sie zwischen Ereignissen navigieren können. 

[Erfahren Sie mehr über Sampling](app-insights-sampling.md).


## <a name="inspect-individual-items"></a>Einzelne Elemente überprüfen

Wählen Sie jedes telemetrieelement an Schlüsselfelder und verknüpfte Elemente aus Klicken Sie auf "..." ", um den vollständigen Satz der Felder anzuzeigen. 


![Klicken Sie auf neue Arbeitsaufgabe und bearbeiten Sie Felder.](./media/app-insights-diagnostic-search/10-detail.png)

Verwenden Sie den vollständigen Satz der Felder, um einfache Zeichenfolgen (ohne Platzhalterzeichen). Die verfügbaren Felder hängen von Telemetrie.

## <a name="create-work-item"></a>Arbeitsaufgabe erstellen

Sie können einen Fehler in Visual Studio Team Services mit Detaildaten aus Telemetriedaten Elemente erstellen. 

![Klicken Sie auf neue Arbeitsaufgabe und bearbeiten Sie Felder.](./media/app-insights-diagnostic-search/42.png)

Diesen Vorgang zum ersten Mal werden Sie aufgefordert einen Link auf Ihre Project Team Services konfigurieren.

![Geben Sie die URL des Team Services-Server und den Namen des Projekts und auf Autorisieren](./media/app-insights-diagnostic-search/41.png)

(Sie erhalten auch an die Konfiguration Blade aus > Arbeitsaufgaben.)

## <a name="filter-event-types"></a>Filter-Ereignistypen

Öffnen Sie Blade Filter, und wählen Sie die Ereignistypen, die Sie anzeigen möchten. (Wenn Sie später die Filter mit dem Öffnen das Blade wiederherstellen möchten, klicken Sie auf Zurücksetzen.)


![Wählen Sie Filter und telemetrietypen](./media/app-insights-diagnostic-search/02-filter-req.png)


Die Ereignistypen sind:

* **Trace** - Diagnoseprotokolle einschließlich TrackTrace, log4Net, NLog und System.Diagnostic.Trace.
* **Request** - HTTP-Anfragen von der Serveranwendung, einschließlich Seiten, Skripts, Bilder, Styles und Daten. Diese Ereignisse wird die Anforderung und Antwort Übersicht Diagramme erstellen.
* **Seitenansicht** - Telemetrie per WebClient verwendet Seite anzeigen Berichte erstellen. 
* **Custom Event** - Aufrufe Trackevent()"zu [Überwachung der Nutzung]eingefügt[track], finden sie hier.
* **Ausnahme** - nicht abgefangene Ausnahmen in die Server und die Anmeldung mit Trackexception()".

## <a name="filter-on-property-values"></a>Filtern nach Eigenschaftswerten

Sie können Ereignisse auf die Werte ihrer Eigenschaften filtern. Die verfügbaren Eigenschaften hängen von ausgewählten Ereignistypen. 

Wählen Sie beispielsweise Anfragen mit einer bestimmten Antwort.

![Erweitern Sie eine Eigenschaft und wählen einen Wert aus](./media/app-insights-diagnostic-search/03-response500.png)

Wählen keine Werte einer bestimmten Eigenschaft hat die gleiche Auswirkung wie alle Werte; Diese deaktiviert Filter auf diese Eigenschaft.


### <a name="narrow-your-search"></a>Grenzen Sie Ihre Suche

Beachten Sie, dass Zahlen rechts Filterwerte wie häufig es im aktuellen gefilterten Satz zeigen. 

In diesem Beispiel wurde deutlich, dass die `Reports/Employees` führt die meisten 500 Fehler anfordern:

![Erweitern Sie eine Eigenschaft und wählen einen Wert aus](./media/app-insights-diagnostic-search/04-failingReq.png)

Außerdem möchten Sie Siehe auch andere Ereignisse während dieser Zeit passiert, können Sie **Ereignisse einschließen nicht definierte Eigenschaften**überprüfen.

## <a name="remove-bot-and-web-test-traffic"></a>Bot und Test Datenverkehr entfernen

Verwenden Sie den Filter **Real oder synthetische Datenverkehr** und überprüfen Sie **Real**.

Sie können auch durch **synthetische Datenverkehr**filtern.

## <a name="inspect-individual-occurrences"></a>Untersuchen der einzelne Vorkommen

Filtersatzes Anforderung namens hinzu, und Sie können dann einzelne Vorkommen dieses Ereignisses prüfen.

![Wählen Sie einen Wert](./media/app-insights-diagnostic-search/05-reqDetails.png)

Details anzeigen für Ereignisse Ausnahmen, die während der Verarbeitung der Anforderung ist.

Klicken Sie auf eine Ausnahme der Details einschließlich Stapelrahmen.

![Klicken Sie auf Ausnahme](./media/app-insights-diagnostic-search/06-callStack.png)

## <a name="find-events-with-the-same-property"></a>Ereignisse mit derselben

Suchen Sie alle Elemente mit dem gleichen Eigenschaftswert:

![Mit der rechten Maustaste einer Eigenschaft](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-by-metric-value"></a>Metrische Wert suchen

Alle die Anfragen Antwortzeit > 5 s zu erhalten.  Uhrzeiten werden in Ticks dargestellt: 10 000 Ticks = 1 ms.

!["Antwortzeit":(threshold TO *)](./media/app-insights-diagnostic-search/11-responsetime.png)



## <a name="search-the-data"></a>Durchsuchen der Daten

Sie können Ausdrücke in einem der Eigenschaftswerte suchen. Dies ist besonders nützlich, wenn Sie [benutzerdefinierte Ereignisse] geschrieben haben[ track] mit Eigenschaftswerten. 

Sie möchten eine Zeit als Suchläufe über eine kürzere Reichweite sind schneller. 

![Diagnostische Suche öffnen](./media/app-insights-diagnostic-search/appinsights-311search.png)

Suche nach Begriffen nicht Teilzeichenfolgen. Sind alphanumerische Zeichenfolgen wie einige Satzzeichen '.' und '_'. Zum Beispiel:

Begriff|*nicht* wird zugeordnet|aber diese entsprechen
---|---|---
HomeController.About|über<br/>Startseite|h\*über<br/>Startseite\*
IsLocal|lokale<br/>ist<br/>\*lokale|ISL\*<br/>IsLocal<br/>i\*l\*
Neue Verzögerung|w d|Neu<br/>Verzögerung<br/>n\* und d\*


Hier sind die Suchbegriffe, die Sie verwenden können:

Beispielabfrage | Effekt 
---|---
verlangsamen|Suchen Sie alle Ereignisse im Datumsbereich, deren Felder den Begriff "langsam"
Datenbank?|Entspricht database01, DatabaseAB...<br/>? darf nicht am Anfang eines Ausdrucks.
Datenbank * |Datenbank, database01, DatabaseNNNN entspricht<br/> * Anfang einen Suchbegriff darf nicht
Apple und Bananen|Suchereignisse,, die beide Begriffe enthalten. Kapital "und" nicht verwendet "und".
Apple oder Banane<br/>Apple banana|Finden Sie Ereignisse, die entweder Begriff. Verwenden Sie "Oder" keine "oder". < /br/ > kurze Formular.
Apple nicht banana<br/>Apple-Banana|Finden Sie Ereignisse, die enthalten eine jedoch nicht.<br/>Kurzform.
App * und Bananen-(grape pear)|Logische Operatoren und Belichtungsreihen.
"Metrik": 0 bis 500<br/>"Metrik": 500 an * | Finden Sie Ereignisse, die benannte Messung im Wertebereich enthalten.


## <a name="save-your-search"></a>Suche speichern

Wenn Sie alle Filter festgelegt haben, soll, können Sie die Suche als Favoriten speichern. Wenn Sie ein Konto Organisation arbeiten, können Sie auswählen, ob für andere Teammitglieder freigeben.

![Klicken Sie auf Favoriten, Namen Sie den und klicken Sie auf Speichern](./media/app-insights-diagnostic-search/08-favorite-save.png)


Zu den erneut suchen, **Gehen Sie zu Übersicht Blade** Favoriten öffnen:

![Favoriten anordnen](./media/app-insights-diagnostic-search/09-favorite-get.png)

Wenn Sie mit relativen Zeitraum gespeichert, hat wieder Blade die neuesten Daten Gespeichert mit absoluten Zeit sehen Sie jedes Mal dieselben Daten.


## <a name="send-more-telemetry-to-application-insights"></a>Weitere Telemetrie an Application Insights senden

Neben der Out-of-the-Box Telemetrie per Application Insights-SDK können Sie:

* Erfassen Protokoll Spuren aus Ihrem bevorzugten protokollierungsframework in [.NET] [ netlogs] oder [Java][javalogs]. Dies bedeutet die Spuren Protokoll durchsuchen und Seitenansichten, Ausnahmen und anderen Ereignissen zu korrelieren. 
* [Schreiben von Code] [ track] benutzerdefinierte Ereignisse Seitenansichten und Ausnahmen zu senden. 

[Erfahren Sie, wie Protokolle und benutzerdefinierte Telemetrie Anwendung Erkenntnisse][trace].


## <a name="questions"></a>Fragen & Antworten

### <a name="limits"></a>Wie viele Daten gespeichert?

Bis zu 500 Ereignisse pro Sekunde bei jeder Anwendung. Ereignisse werden sieben Tage lang aufbewahrt.

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Wie kann ich meine Server Anfragen POST-Daten anzeigen?

Wir nicht die POST-Daten automatisch anmelden, aber können [TrackTrace oder Protokoll Aufrufe][trace]. Legen Sie die POST-Daten der Message-Parameter Kann nicht gefiltert werden der Nachricht Sie Eigenschaften können wie die maximale Größe ist jedoch länger.

## <a name="add"></a>Nächste Schritte

* [Protokolle und benutzerdefinierte Telemetrie an Application Insights senden][trace]
* [Verfügbarkeit und Reaktionsfähigkeit Tests einrichten][availability]
* [Problembehandlung][qna]



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[javalogs]: app-insights-java-trace-logs.md
[netlogs]: app-insights-asp-net-trace-logs.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
[trace]: app-insights-search-diagnostic-logs.md
[track]: app-insights-api-custom-events-metrics.md

 