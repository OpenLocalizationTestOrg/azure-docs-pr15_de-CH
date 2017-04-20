<properties
   pageTitle="Zuverlässige Sammlungen | Microsoft Azure"
   description="Statusbehaftete Dienstfabric-Services bieten zuverlässige Sammlungen, die Sie hoch verfügbar, skalierbar und niedriger Latenz Cloudanwendungen schreiben können."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Einführung in die zuverlässige Sammlungen statusbehaftete Azure Service Fabric-services

Zuverlässige Sammlungen können hoch verfügbar, skalierbar und niedriger Latenz Cloudanwendungen schreiben, als ob Einzelcomputer Applikationen geschrieben wurden. Klassen im **Microsoft.ServiceFabric.Data.Collections** -Namespace bieten Out-of-the-Box-Auflistungen, die automatisch Ihren hoch verfügbar zu machen. Entwickler müssen Programm nur zuverlässige Auflistung APIs und zuverlässige Sammlungen replizierten und lokalen Zustand verwalten.

Der Hauptunterschied zwischen zuverlässige Sammlungen und andere hohe Verfügbarkeit (wie Redis, Azure-Diensts und Azure Warteschlangendienst) werden der Zustand in die Instanz lokal aufbewahrt werden, während auch hochgradig verfügbar gemacht wird. Das bedeutet:

- Alle Lesevorgänge sind lokale, niedriger Latenz und hohem Durchsatz liest.
- Alle Schreibvorgänge anfallen Mindestanzahl Netzwerk IOs, wodurch niedrige Latenz und hohem Durchsatz schreibt.

![Bild der Entwicklung von Sammlungen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Zuverlässige Sammlungen als Weiterentwicklung der Klassen **System.Collections** vorstellen: einen neuen Satz von Sammlungen, die für die Cloud und mehrere Computer ohne Komplexität für Entwickler entwickelt wurden. Daher sind zuverlässige Sammlungen:

- Replikation: Zustandsänderungen werden für hohe Verfügbarkeit repliziert.
- Beibehalten: Daten werden gespeichert auf Festplatte Beständigkeit gegen große Ausfällen (z. B. Datacenter Stromausfall).
- Asynchron: APIs sind asynchron, daß Threads nicht blockiert werden, wenn zusätzliche e/a.
- Transaktionale: APIs nutzen die Abstraktion der Transaktionen mehrere zuverlässige Sammlungen in einem Dienst problemlos verwalten.

Zuverlässige Sammlungen bieten garantiert starker Konsistenz aus Feld Begründung zum Anwendungsstatus zu erleichtern.
Starker Konsistenz wird dadurch erreicht Transaktion Commits beenden erst die gesamte Transaktion eine Mehrheit Quorum von Replikaten, einschließlich der primären protokolliert wurde.
Um schwächere Konsistenz zu erreichen, können Applikationen zurück an den Client/anfordernden bestätigen, bevor das asynchrone Commit gibt.

Die zuverlässige Sammlungen APIs sind eine Weiterentwicklung von gleichzeitigen Garbage Collections APIs (im **System.Collections.Concurrent** -Namespace gefunden):

- Asynchron: Gibt eine Aufgabe gleichzeitig Auflistungen Vorgänge repliziert und beibehalten werden.
- Keine out-Parameter: verwendet `ConditionalValue<T>` Bool und einem Wert out-Parameter zurückgegeben. `ConditionalValue<T>`wie `Nullable<T>` , aber nicht zu einer Struktur erforderlich ist.
- Transaktionen: Ein Transaktionsobjekt ermöglichen den Benutzer Aktionen auf mehrere zuverlässige Sammlungen in einer Transaktion verwendet.

Heute enthält **Microsoft.ServiceFabric.Data.Collections** zwei Sammlungen:

- [Zuverlässige Wörterbuch](https://msdn.microsoft.com/library/azure/dn971511.aspx): eine replizierte Transaktions- und asynchrone Auflistung von Schlüssel-Wert-Paare. Ähnlich **ConcurrentDictionary**können den Schlüssel und den Wert eines beliebigen Typs sein.
- [Zuverlässige Warteschlange](https://msdn.microsoft.com/library/azure/dn971527.aspx): eine replizierte Transaktions- und asynchrone strenge First in, First Out (FIFO) Warteschlange darstellt. Wie **ConcurrentQueue**kann der Wert eines beliebigen Typs sein.

## <a name="isolation-levels"></a>Isolationsstufen
Isolationsstufe definiert den Grad, die Buchung von Änderungen von anderen Transaktionen isoliert sein muss.
Es gibt zwei Isolationsstufen, die zuverlässige Auflistungen unterstützt werden:

- **Wiederholbare Lesevorgänge**: Gibt Anweisungen Daten gelesen werden können, die geändert, aber noch nicht von anderen Transaktionen ein Commit ausgeführt wurde und keine anderen Transaktionen Daten ändern können, die von der aktuellen Transaktion gelesen wurden, bis die aktuelle Transaktion abgeschlossen ist. Weitere Informationen finden Sie unter [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Snapshot**: Gibt an, dass jede Anweisung in einer Transaktion gelesenen Daten konsistent Version der Daten, die zu Beginn der Transaktion vorhanden waren.
Die Transaktion kann nur geänderte Daten erkennen, die vor Beginn der Transaktion festgeschrieben wurden.
Datenänderungen von anderen Transaktionen nach dem Beginn der aktuellen Transaktion sind nicht sichtbar für Anweisungen in der aktuellen Transaktion ausgeführt.
Der Effekt ist als Anweisungen in einer Transaktion an den Anfang der Transaktion einen Snapshot der festgeschriebenen Daten erhalten.
Snapshots sind konsistent über zuverlässige Sammlungen.
Weitere Informationen finden Sie unter [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Zuverlässige Sammlungen wählen automatisch die Isolationsstufe für einen bestimmten Lesevorgang des Vorgangs und die Rolle des Replikats zum Zeitpunkt der Erstellung der Transaktion verwendet.
Folgendes ist die Tabelle, die Standardeinstellungen Isolationsmodus auf zuverlässige Wörterbuch und Warteschlange zeigt.

| Operation \ Rolle      | Primäre          | Sekundäre        |
| --------------------- | :--------------- | :--------------- |
| Einheit lesen    | Wiederholbare Lesevorgänge  | Snapshot         |
| Enumeration \ Count   | Snapshot         | Snapshot         |

>[AZURE.NOTE] Beispiele für einzelne Entität sind `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Zuverlässige Wörterbuch und die zuverlässige Warteschlange unterstützen Lesen der schreibt.
In anderen Worten, werden alle schreiben innerhalb einer Transaktion auf folgenden lesen angezeigt, die zu derselben Transaktion gehört.

## <a name="locking"></a>Sperren
Zuverlässige Sammlungen alle Transaktionen sind zwei Phasen: eine Transaktion erworben hat, bis zum Ende die Transaktion einen Abbruch oder Commit Sperren nicht freigegeben.

Zuverlässiges Wörterbuch verwendet Zeile Sperren für alle Einheit.
Zuverlässige Warteschlange Kompromiss zwischen Parallelität für strenge FIFO Transaktionseigenschaft.
Zuverlässige Warteschlange verwendet Vorgang Sperren einer Transaktion mit `TryPeekAsync` oder `TryDequeueAsync` und eine Buchung mit `EnqueueAsync` gleichzeitig.
Beachten Sie, dass um FIFO, erhalten einen `TryPeekAsync` oder `TryDequeueAsync` je sei die zuverlässige Warteschlange leer ist, werden sie auch Sperren `EnqueueAsync`.

Schreiben Sie, dass Vorgänge immer exklusive Sperren.
Einige Faktoren hängt das Sperren von für Lesevorgänge.
Jeder Lesevorgang durchgeführt, die mit Snapshot-Isolation ist Sperre frei.
Alle Repeatable Read-Operation standardmäßig verwendet gemeinsame Sperren.
Jedoch kann alle Lesevorgang, der wiederholbare Lesevorgänge unterstützt, der Benutzer für eine Aktualisierungssperre anstatt die gemeinsame Sperre anfordern.
Eine Aktualisierungssperre wird eine asymmetrische Sperre eine häufige Form von Deadlocks verhindert, das auftritt, wenn mehrere Transaktionen Ressourcen für mögliche Updates zu einem späteren Zeitpunkt sperren.

Kompatibilitätsmatrix für Sperren finden unter:

| Anforderung \ gewährt | Keine         | Freigegeben       | Update      | Exklusive    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Freigegeben            | Keine Konflikte  | Keine Konflikte  | Konflikt    | Konflikt     |
| Update            | Keine Konflikte  | Keine Konflikte  | Konflikt    | Konflikt     |
| Exklusive         | Keine Konflikte  | Konflikt     | Konflikt    | Konflikt     |

Beachten Sie, dass ein Argument Timeout zuverlässige Sammlungen APIs für Deadlock-Erkennung verwendet wird.
Z. B. versuchen zwei Transaktionen (T1 und T2) zu lesen K1.
Kann für sie deadlock, da sie am Ende die Sperre freigegeben.
In diesem Fall werden eine oder beide Vorgänge.

Beachten Sie, dass das obige Deadlock-Szenario ist ein gutes Beispiel wie eine Aktualisierungssperre Deadlocks verhindern kann.

## <a name="persistence-model"></a>Persistenzmodell
Zuverlässigen Status-Manager und zuverlässige Sammlungen folgen Vorbild Persistenz und Prüfpunktdateien aufgerufen wird.
Dies ist ein Modell, in dem jede Änderung Datenträger angemeldet und im Arbeitsspeicher angewendet.
Der vollständige Zustand selbst nur gelegentlich (so genanntes beibehalten Checkpoint).
Der Vorteil ist, dass sequenzielle nur Anhängen Schreibvorgänge auf der Festplatte für verbesserte Leistung Deltas werden.

Um besser zu verstehen und Prüfpunktdateien Modell zunächst betrachten infinite Disk-Szenario.
Zuverlässige Status-Manager protokolliert jeden Vorgang, bevor repliziert wird.
Dadurch wird die zuverlässige Auflistung den Vorgang im Arbeitsspeicher angewendet.
Protokolle beibehalten werden, auch wenn das Replikat schlägt fehl und muss neu gestartet werden, seit der zuverlässigen Zustand genügend Informationen in seine Protokolle Alle wiedergeben Replikat verloren.
Der Datenträger unendlich ist, Protokolleinträge nie entfernt werden müssen und die zuverlässige Auflistung muss nur im Arbeitsspeicher Verwalten des.

Jetzt sehen wir uns die begrenzte Disk-Szenario.
Wie Datensätze sammeln wird Speicherplatz zuverlässigen Status-Manager ausgeführt.
Zuverlässige Status-Manager muss davor Protokoll um Platz für neueren Datensätzen abgeschnitten.
Es fordert die zuverlässige Sammlungen Checkpoint Zustand im Arbeitsspeicher auf den Datenträger.
Es obliegt die zuverlässige Sammlungen bis dahin seinen Zustand beibehalten.
Abschluss zuverlässige Sammlungen ihre Prüfpunkte können zuverlässige Status-Manager anmelden, um Speicherplatz freizugeben kürzen.
So, wenn das Replikat muss neu gestartet werden, erholen zuverlässige Sammlungen Zustand ändern und zuverlässigen Status-Manager wiederherstellen und alle Zustandsänderungen, die seit dem Prüfpunkt wiedergeben.

>[AZURE.NOTE] Einen weiteren Wert hinzufügen von Prüfpunkten in Fällen Recovery-Performance verbessert wird.
Deswegen Prüfpunkte nur die neuesten Versionen enthalten.

## <a name="recommendations"></a>Empfehlung

- Ein Objekt des benutzerdefinierten Typs zurückgegebene Lesevorgänge nicht ändern (z. B. `TryPeekAsync` oder `TryGetValueAsync`). Zuverlässige Sammlungen wie gleichzeitigen Garbage Collections, zurück auf die Objekte und keine Kopie.
- Führen Sie tiefe Kopie zurückgegebene Objekt eines benutzerdefinierten Typs vor dem ändern. Da Strukturen und integrierte Typen übergeben Wert sind, brauchen Sie eine tiefe Kopie auf.
- Verwenden Sie nicht `TimeSpan.MaxValue` für Timeouts. Timeouts sollte verwendet werden, um Deadlocks zu erkennen.
- Verwenden Sie eine Transaktion nicht, nachdem sie festgeschrieben, abgebrochen, oder freigegeben wurde.
- Verwenden Sie eine Enumeration nicht außerhalb der Transaktion, in der Sie erstellt wurde.
- Erstellen Sie eine Transaktion in einer anderen Transaktion nicht `using` Anweisung da Deadlocks auftreten können.
- Stellen Sie sicher, dass Ihre `IComparable<TKey>` Implementierung ist einwandfrei. Gelangen Abhängigkeit auf diese Prüfpunkte zusammengeführt.
- Verwenden Sie Aktualisierungssperre beim Lesen eines Elements mit der Absicht um zu aktualisieren, um eine bestimmte Klasse von Deadlocks zu verhindern.
- Verwenden Sie Backup und Wiederherstellen Sie Funktionen zur Wiederherstellung.
- Mischen von Einheit und Multi-Entität zu vermeiden (z.B. `GetCountAsync`, `CreateEnumerableAsync`) in derselben Transaktion durch die verschiedenen Isolationsstufen.

Hier sind einige Dinge zu beachten:

- Das Standardzeitlimit beträgt 4 Sekunden für die zuverlässige Auflistung-APIs. Die meisten Benutzer sollten diese nicht überschreiben.
- Das Standardabbruchtoken ist `CancellationToken.None` in allen zuverlässige Sammlungen APIs.
- Schlüsseltyp Parameters (*TKey*) für eine zuverlässige Wörterbuch müssen ordnungsgemäß `GetHashCode()` und `Equals()`. Schlüssel müssen unveränderlich sein.
- Um hohe Verfügbarkeit für die zuverlässige Sammlungen zu erreichen, müssen jeden Dienst mindestens ein Ziel und einen minimalen Replikat von 3.
- Lesevorgänge auf dem sekundären Server möglicherweise Versionen lesen, die nicht zugesichert Quorum.
Dies bedeutet, dass eine Version der Daten aus einer sekundären gelesen werden false vorangetrieben werden kann.
Liest aus sind immer stabile: kann nie werden falsch ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

- [Zuverlässige Dienste Schnellstart](service-fabric-reliable-services-quick-start.md)
- [Arbeiten mit zuverlässigen Sammlungen](service-fabric-work-with-reliable-collections.md)
- [Zuverlässige Dienste Benachrichtigungen](service-fabric-reliable-services-notifications.md)
- [Zuverlässige Sicherung und Wiederherstellung (Disaster Recovery)](service-fabric-reliable-services-backup-restore.md)
- [Zuverlässige Manager-Konfiguration](service-fabric-reliable-services-configuration.md)
- [Erste Schritte mit Service Fabric Web API-Dienste](service-fabric-reliable-services-communication-webapi.md)
- [Erweiterte Nutzung der zuverlässigen Programmiermodell](service-fabric-reliable-services-advanced-usage.md)
- [Entwicklerreferenz für zuverlässige Sammlungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
