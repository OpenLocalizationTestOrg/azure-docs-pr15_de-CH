<properties
    pageTitle="Einführung in Stream Analytics-Funktionen | Microsoft Azure"
    description="Lernen Sie drei Funktionen im Stream Analytics (tumbling, hüpfen, verschieben)."
    keywords="Tumbling Fenster Schiebefenster hopping Fenster"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Einführung in Stream Analytics-Funktionen

In vielen Echtzeit streamenden Szenarien muss Operationen auf Daten in temporären Windows. Systemeigene Unterstützung für Windowing-Funktionen ist ein wichtiges Feature von Azure Stream Analytics, die Entwicklerproduktivität komplexe Verarbeitungsaufträge erstellen die Nadel bewegt. Stream Analytics kann Entwickler [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) und [**Sliding**](https://msdn.microsoft.com/library/dn835051.aspx) Windows zeitliche Operationen auf Daten. Es ist erwähnenswert, dass alle [Fenster](https://msdn.microsoft.com/library/dn835019.aspx) Vorgänge Ergebnisse am **Ende** des Fensters auszugeben. Die Ausgabe im Fenster werden einzelne Ereignis basierend auf der Aggregatfunktion verwendet. Das Ereignis muss den Zeitstempel am Ende des Fensters und alle Funktionen werden mit einer festen Länge definiert. Schließlich ist zu beachten, dass alle Funktionen in einer [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) -Klausel verwendet werden soll.

![Stream Analytics Fenster Funktionen Konzepte](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Tumbling Fenster

Tumbling Fenster Funktionen verwendet, um einen Datenstrom in verschiedene Zeitabschnitte segment und eine Funktion, wie im folgenden Beispiel. Die Hauptmerkmale des Fensters Tumbling sind, dass sie wiederholt nicht überschneiden und ein Ereignis kann nicht mehrere Tumbling Fenster gehören.

![Stream Analytics Fensterfunktionen tumbling Einführung](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hopping Fenster

Hopping Fenster Funktionen Hop weiterleiten zeitlich befristet. Möglicherweise betrachten sie als Tumbling Windows, die überlappen können, damit Ereignisse auf mehrere Hopping Fenster Resultset gehören können leicht. Ein Hopping wäre ein Tumbling identisch Fenster einfach Hop Größe der Fenstergröße übereinstimmen angeben. 

![Stream Analytics Fensterfunktionen hopping Einführung](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Schiebefenster

Gleitende Fensterfunktionen, im Gegensatz zu taumeln oder Hopping Windows erzeugt eine Ausgabe **nur** beim Eintreten eines Ereignisses. Jedes Fenster muss mindestens ein Ereignis und Fenster kontinuierlich weiterentwickelt um eine (Epsilon). Ereignisse können wie Windows Hopping mehrere Schiebefenster angehören.

![Stream Analytics Fensterfunktionen gleitende Einführung](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Hilfe zu Funktionen

Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
