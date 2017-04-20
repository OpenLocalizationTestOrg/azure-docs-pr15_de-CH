<properties
   pageTitle="Akteure Diagnose und Überwachung | Microsoft Azure"
   description="Dieser Artikel beschreibt die Diagnose und Features in der Runtime Service Fabric zuverlässige Akteure, z. B. Ereignisse und Leistungsindikatoren, die von ihr ausgegebenen zum Überwachen der Leistung."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnose und für zuverlässige Performance-Überwachung
Zuverlässige Akteure Runtime gibt [Ereignisquelle](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) Ereignisse und [Leistungsindikatoren](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Diese Einblicke in die Runtime wie arbeitet und Hilfe bei der Problembehebung und Überwachung.

## <a name="eventsource-events"></a>Ereignisquelle Ereignisse
Name der Ereignisquelle Runtime zuverlässig Akteure ist "Microsoft ServiceFabric Darsteller". Ereignisse von dieser Quelle werden im Fenster [Diagnose Ereignisse](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) , wenn Akteur-Anwendung [in Visual Studio gedebuggt](service-fabric-debugging-your-application.md)wird.

Beispiele für Tools und Technologie bei der Erfassung bzw. Ereignisquelle Ereignisse sind [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) [Semantische Protokollierung](https://msdn.microsoft.com/library/dn774980.aspx)und [Microsoft TraceEvent Bibliothek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Schlüsselwörter
Alle Ereignisse, die zuverlässige Quelle Akteure gehören, sind ein oder mehrere Schlüsselwörter zugeordnet. Dadurch können die Filterung von Ereignissen, die gesammelt werden. Die folgenden Schlüsselwort Bits sind definiert.

|Bit|Beschreibung|
|---|---|
|0 x 1|Festlegen von wichtigen Ereignissen, die die Operation der Laufzeit Fabric Akteure zusammenfassen.|
|0 x 2|Gruppe von Ereignissen, die Methodenaufrufe Akteur beschreiben. Weitere Informationen finden Sie im [einführenden Thema Akteure](service-fabric-reliable-actors-introduction.md#actors).|
|0 x 4|Mehrere Ereignisse Akteur Zustand. Weitere Informationen finden Sie im Thema [Akteur State](service-fabric-reliable-actors-state-management.md)Management.|
|0 x 8|Satz von Ereignissen aktivieren Parallelität in den Akteur. Weitere Informationen finden Sie im Thema auf [Parallelität](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Leistungsindikatoren
Zuverlässige Akteure Runtime definiert die folgenden Leistungsindikatorkategorien.

|Kategorie|Beschreibung|
|---|---|
|Service Fabric Akteur|Zähler für Azure Service Fabric Akteure, z.B. Akteur Zustand speichern|
|Service Fabric Actor-Methode|Leistungsindikatoren bestimmte Methoden implementiert Service Fabric Akteure, z. B. wie oft eine Akteur-Methode wird aufgerufen|

Kategorien jeweils einen oder mehrere Leistungsindikatoren.

[Windows-Systemmonitor](https://technet.microsoft.com/library/cc749249.aspx) -Anwendung, die standardmäßig im Windows-Betriebssystem kann verwendet werden, sammeln und Anzeigen von Leistungsindikatordaten. [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) ist eine weitere Option zum Sammeln von Leistungsindikatordaten und Azure Tabellen hochladen.

### <a name="performance-counter-instance-names"></a>Namen von Leistungsindikatorinstanzen
Ein Cluster, der eine große Anzahl von Actor-Dienste oder Akteur Servicepartitionen haben eine große Anzahl von Leistungsindikatorinstanzen Akteur. Die Namen von Leistungsindikatoreninstanzen hilft dabei bestimmten [Partition](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) und Akteur-Methode (falls zutreffend), dass die Leistungsindikatorinstanz zugeordnet ist.

#### <a name="service-fabric-actor-category"></a>Service Fabric Actor-Kategorie
Für die Kategorie `Service Fabric Actor`, die Namen der Leistungsindikatoren sind im folgenden Format:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* ist die String-Darstellung der Service Fabric Partitions-ID, die die Instanz zugeordnet ist. Die Partitions-ID ist eine GUID und seine Darstellung wird durch die [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) -Methode mit dem Formatbezeichner "D".

*ActorRuntimeInternalID* ist die String-Darstellung einer 64-Bit-Ganzzahl, die von der Fabric Akteure Runtime zur internen Nutzung generiert wird. Gehört der Instanzname der Eindeutigkeit und vermeiden Konflikte mit anderen Namen von Leistungsindikatorinstanzen. Benutzer sollten nicht interpretieren diesen Teil der Instanzname.

Im folgenden ist ein Beispiel für einen Leistungsindikator Instanznamen für einen Leistungsindikator gehört das `Service Fabric Actor` Kategorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Im obigen Beispiel `2740af29-78aa-44bc-a20b-7e60fb783264` ist String-Darstellung Service Fabric-Partitions-ID und `635650083799324046` ist die 64-Bit-ID, für die Laufzeit interne ist generiert wird.

#### <a name="service-fabric-actor-method-category"></a>Fabric Akteur Webdienstmethode Kategorie
Für die Kategorie `Service Fabric Actor Method`, die Namen der Leistungsindikatoren sind im folgenden Format:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*Methodenname* ist der Name der Akteur-Methode, der die Instanz zugeordnet ist. Das Format der Methodenname ist anhand Logik in der Runtime Fabric Akteure, die die Lesbarkeit des Namens mit Einschränkungen auf die maximale Länge der Namen von Leistungsindikatorinstanzen Windows gleicht.

*ActorsRuntimeMethodId* ist die Darstellung der Zeichenfolge eine 32-Bit-Ganzzahl, die von der Fabric Akteure Runtime zur internen Nutzung generiert. Gehört der Instanzname der Eindeutigkeit und vermeiden Konflikte mit anderen Namen von Leistungsindikatorinstanzen. Benutzer sollten nicht interpretieren diesen Teil der Instanzname.

*ServiceFabricPartitionID* ist die String-Darstellung der Service Fabric Partitions-ID, die die Instanz zugeordnet ist. Die Partitions-ID ist eine GUID und seine Darstellung wird durch die [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) -Methode mit dem Formatbezeichner "D".

*ActorRuntimeInternalID* ist die String-Darstellung einer 64-Bit-Ganzzahl, die von der Fabric Akteure Runtime zur internen Nutzung generiert wird. Gehört der Instanzname der Eindeutigkeit und vermeiden Konflikte mit anderen Namen von Leistungsindikatorinstanzen. Benutzer sollten nicht interpretieren diesen Teil der Instanzname.

Im folgenden ist ein Beispiel für einen Leistungsindikator Instanznamen für einen Leistungsindikator gehört das `Service Fabric Actor Method` Kategorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Im obigen Beispiel `ivoicemailboxactor.leavemessageasync` ist der Methodenname `2` die 32-Bit-ID für den Eigenbedarf der Runtime generierten `89383d32-e57e-4a9b-a6ad-57c6792aa521` ist String-Darstellung Service Fabric-Partitions-ID und `635650083804480486` ist die 64-Bit-ID für die Laufzeit interne ist generiert.

## <a name="list-of-events-and-performance-counters"></a>Liste der Ereignisse und Leistungsindikatoren

### <a name="actor-method-events-and-performance-counters"></a>Schauspieler-Methodenereignisse und Leistungsindikatoren
Zuverlässige Akteure Runtime gibt die folgenden Ereignisse für den [Akteur Methoden](service-fabric-reliable-actors-introduction.md#actors).

|Ereignisname|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorMethodStart|7|Ausführliche|0 x 2|Akteure Runtime wird eine Akteur-Methode aufrufen.|
|ActorMethodStop|8|Ausführliche|0 x 2|Eine Akteur-Methode wurde ausgeführt. Also lieferte die Laufzeit asynchronen Aufruf der Methode Akteur und Akteur-Methode zurückgegebene Task beendet.|
|ActorMethodThrewException|9|Warnung|0 x 3|Während der Ausführung einer Methode Akteur Ausnahme während der Laufzeit asynchronen Aufruf der Methode Akteur oder während der Ausführung der Aufgabe von Actor-Methode zurückgegeben. Dieses Ereignis zeigt eine Fehler im Code Akteur, die Untersuchung.|

Die Runtime zuverlässig Akteure veröffentlicht die folgenden Leistungsindikatoren für die Ausführung der Akteur.

|Kategorie|Name des Leistungsindikators|Beschreibung|
|---|---|---|
|Service Fabric Actor-Methode|Aufrufe/Sek.|Anzahl der pro Sekunde Akteur Webdienstmethode aufgerufen wird|
|Service Fabric Actor-Methode|Durchschnittliche Millisekunden pro Aufruf|Ausführungszeit für die Webdienstmethode Schauspieler in Millisekunden|
|Service Fabric Actor-Methode|Ausgelösten Ausnahmen/Sek|Wie oft der Akteur Methode service Ausnahmefehler pro Sekunde|

### <a name="concurrency-events-and-performance-counters"></a>Parallelität Ereignisse und Leistungsindikatoren
Zuverlässige Akteure Runtime gibt die folgenden Ereignisse für [Parallelität](service-fabric-reliable-actors-introduction.md#concurrency).

|Ereignisname|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Ausführliche|0 x 8|Dieses Ereignis wird zu Beginn jeder neuen runde Schauspieler geschrieben. Es enthält die Anzahl der ausstehenden Akteur-Aufrufe, die auf die Schauspieler-Sperre, die Turn-Parallelität erzwingt.|

Die Runtime zuverlässig Akteure veröffentlicht die folgenden Leistungsindikatoren für Parallelität.

|Kategorie|Name des Leistungsindikators|Beschreibung|
|---|---|---|
|Service Fabric Akteur|# der Akteur Aufrufe Akteur Sperre warten|Anzahl der ausstehenden Akteur Aufrufe der Schauspieler-Sperre, die Turn-Parallelität erzwingt auf|
|Service Fabric Akteur|Durchschnittliche Millisekunden pro Sperre warten|Zeit (in Millisekunden), die pro Actor-Sperre, die Turn-Parallelität erzwingt|
|Service Fabric Akteur|Durchschnittliche Millisekunden Akteur Sperre|Zeit (in Millisekunden) für die die pro Akteur gesperrt ist|

### <a name="actor-state-management-events-and-performance-counters"></a>Schauspieler Zustand Ereignisse und Leistungsindikatoren
Zuverlässige Akteure Runtime gibt die folgenden Ereignisse für den [Akteur Verwaltung](service-fabric-reliable-actors-state-management.md).

|Ereignisname|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorSaveStateStart|10|Ausführliche|0 x 4|Akteure Runtime ist den Akteur Zustand speichern.|
|ActorSaveStateStop|11|Ausführliche|0 x 4|Akteure Runtime wurde Akteur Status gespeichert.|

Die Runtime zuverlässig Akteure veröffentlicht die folgenden Leistungsindikatoren für Schauspieler Verwaltung.

|Kategorie|Name des Leistungsindikators|Beschreibung|
|---|---|---|
|Service Fabric Akteur|Durchschnittliche Millisekunden pro speichern Zustand|Zeit in Millisekunden Akteur Zustand speichern|
|Service Fabric Akteur|Durchschnittliche Millisekunden pro Ladevorgang Zustand|Zeit zum Laden Akteur Zustand in Millisekunden|

### <a name="events-related-to-actor-replicas"></a>Ereignisse im Zusammenhang mit Akteur Replikate
Zuverlässige Akteure Runtime gibt die folgenden Ereignisse für den [Akteur Replikate](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|Ereignisname|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Information|0 x 1|Schauspieler Replikat auf primäre Rolle geändert. Dies bedeutet, dass die Akteure für diese Partition in diesem Replikat erstellt werden.|
|ReplicaChangeRoleFromPrimary|2|Information|0 x 1|Schauspieler Replikat nicht primäre Rolle geändert. Dies bedeutet, dass die Akteure für diese Partition nicht innerhalb dieses Replikat erstellt werden. Akteure, die bereits in diesem Replikat werden keine neuen Anfragen übermittelt. Nach Abschluss jeder Statusabfragen Akteure zerstört.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktivierung und Deaktivierung Actor-Ereignisse und Leistungsindikatoren
Zuverlässige Akteure Runtime gibt die folgenden Ereignisse für den [Akteur Aktivierung und Deaktivierung](service-fabric-reliable-actors-lifecycle.md).

|Ereignisname|Ereignis-ID|Ebene|Schlüsselwort|Beschreibung|
|---|---|---|---|---|
|ActorActivated|5|Information|0 x 1|Ein Akteur ist aktiviert.|
|ActorDeactivated|6|Information|0 x 1|Ein Akteur ist deaktiviert.|

Die Runtime zuverlässig Akteure veröffentlicht die folgenden Leistungsindikatoren für Schauspieler Aktivierung und Deaktivierung.

|Kategorie|Name des Leistungsindikators|Beschreibung|
|---|---|---|
|Service Fabric Akteur|Durchschnittliche OnActivateAsync Millisekunden|OnActivateAsync-Methode in Millisekunden Ausführung|

### <a name="actor-request-processing-performance-counters"></a>Schauspieler Verarbeitung von Leistungsindikatoren
Wenn ein Client eine Methode über ein Proxyobjekt Akteur aufruft, wird eine Anforderungsnachricht Akteur-Service über das Netzwerk gesendet werden. Der Dienst verarbeitet die Request-Nachricht und sendet eine Antwort an den Client zurück. Die Runtime zuverlässig Akteure veröffentlicht die folgenden Leistungsindikatoren für Schauspieler Verarbeitung.

|Kategorie|Name des Leistungsindikators|Beschreibung|
|---|---|---|
|Service Fabric Akteur|# Ausstehende Anfragen|Anzahl der Anfragen in der Dienst verarbeitet|
|Service Fabric Akteur|Durchschnittliche Millisekunden pro Anforderung|Zeit (in Millisekunden), die der Dienst zum Verarbeiten einer Anforderung|
|Service Fabric Akteur|Durchschnittliche Millisekunden für die Anforderung Deserialisierung|Zeit (in Millisekunden) Akteur Anforderungsnachricht deserialisiert beim Empfang an den Dienst|
|Service Fabric Akteur|Durchschnittliche Millisekunden Antwort Serialisierung|Zeit (in Millisekunden) zum Serialisieren der Akteur-Response-Nachricht an den Dienst vor dem Senden der Antwort an den client|

## <a name="next-steps"></a>Nächste Schritte
 - [Verwendung von zuverlässigen Akteure Service Fabric-Plattform](service-fabric-reliable-actors-platform.md)
 - [Schauspieler-API-Dokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispielcode](https://github.com/Azure/servicefabric-samples)
