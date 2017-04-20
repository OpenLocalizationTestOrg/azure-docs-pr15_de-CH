<properties
   pageTitle="Statusbehaftete zuverlässige Dienste Diagnose | Microsoft Azure"
   description="Diagnose-Funktionen für statusbehaftete zuverlässige Dienste"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnose-Funktionen für statusbehaftete zuverlässige Dienste
Die statusbehaftete zuverlässige Dienste StatefulServiceBase Klasse gibt [Ereignisquelle](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) Ereignisse, die mit den Dienst debuggen, Einblicke in wie die Common Language Runtime wird und Unterstützung bei der Problembehandlung.

## <a name="eventsource-events"></a>Ereignisquelle Ereignisse
Die Ereignisquelle für statusbehaftete zuverlässige Dienste StatefulServiceBase Klasse heißt "Microsoft ServiceFabric Services". Ereignisse von dieser Ereignisquelle werden im Fenster [Diagnose Ereignisse](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) angezeigt, wenn der Dienst [in Visual Studio gedebuggt](service-fabric-debugging-your-application.md)wird.

Beispiele für Tools und Technologie bei der Erfassung bzw. Ereignisquelle Ereignisse sind [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md)und [Microsoft TraceEvent Bibliothek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Ereignisse

|Ereignisname|Ereignis-ID|Ebene|Beschreibung der Veranstaltung|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Information|Wenn RunAsync Aufgabe gestartet wird|
|StatefulRunAsyncCancellation|2|Information|Wenn RunAsync Aufgabe abgebrochen wird|
|StatefulRunAsyncCompletion|3|Information|Wenn Service RunAsync durchgeführt|
|StatefulRunAsyncSlowCancellation|4|Warnung|Wenn RunAsync Aufgabe Abbruch zu lange dauert|
|StatefulRunAsyncFailure|5|Fehler|Wenn RunAsync Aufgabe eine Ausnahme auslöst.|

## <a name="interpret-events"></a>Interpretieren von Ereignissen

Ereignisse StatefulRunAsyncInvocation, StatefulRunAsyncCompletion und StatefulRunAsyncCancellation sind die Verfasser verständlich Lifecycle Service – sowie die Anzeigedauer als Dienst gestartet, abgebrochen oder abgeschlossen ist. Dies kann hilfreich sein beim Debuggen Probleme oder Service-Lebenszyklus verstehen.

-Dienstverfasser sollten StatefulRunAsyncSlowCancellation und StatefulRunAsyncFailure achten, da sie Probleme mit dem Dienst hinweisen.

StatefulRunAsyncFailure wird ausgegeben, wenn die Dienst RunAsync() Aufgabe eine Ausnahme auslöst. Normalerweise gibt eine ausgelöste Ausnahme einen Fehler oder Fehler im Dienst. Außerdem wird die Ausnahme den Dienst nicht ausgeführt wird, auf einen anderen Knoten verschoben wird. Dies kann ein kostenträchtiger Vorgang und kann Anfragen eine Verzögerung, während der Dienst verschoben wird. -Dienstverfasser sollte die Ursache der Ausnahme und ggf. verringern.

StatefulRunAsyncSlowCancellation wird ausgegeben, wenn eine Absage für RunAsync-Vorgang länger als vier Sekunden dauert. Wenn ein Dienst Abbruch zu lange dauert, beeinträchtigt die Möglichkeit für den Dienst auf einem anderen Knoten schnell neu gestartet werden. Dies kann die allgemeine Verfügbarkeit des Dienstes auswirken.
