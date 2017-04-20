<properties
   pageTitle="Zuverlässige Dienste Benachrichtigungen | Microsoft Azure"
   description="Dokumentation für zuverlässige Fabric-Dienst-Benachrichtigung"
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
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Zuverlässige Dienste Benachrichtigungen

Anträge können Clients die Überarbeitungen an ein Objekt vorgenommen werden, denen sie interessieren. Unterstützt zwei Typen von Benachrichtigungen: *Zuverlässige Status-Manager* und *Zuverlässige Wörterbuch*.

Häufige Gründe für die Verwendung von Benachrichtigungen

- Materialisierte Ansichten wie sekundäre Indizes oder gefilterte Ansichten des Replikats Status zusammengefasst. Ein Beispiel ist ein sortierter Index aller Schlüssel im Wörterbuch zuverlässig.
- Sendende Überwachungsdaten, wie die Anzahl der Benutzer in der letzten Stunde.

Nachrichten werden als Teil der Anwendung Vorgänge ausgelöst. Deshalb sollten Anträge behandelt werden so schnell wie möglich und synchrone Ereignisse teure Vorgänge sollten nicht.

## <a name="reliable-state-manager-notifications"></a>Zuverlässige Status-Manager benachrichtigt

Zuverlässige Status-Manager bietet die folgenden Ereignisse:

- Buchung
    - Commit
- Status-manager
    - Neu erstellen
    - Hinzufügen von einem zuverlässigen Zustand
    - Entfernt einen zuverlässigen Zustand

Zuverlässige Status-Manager verfolgt die aktuelle Transaktionen an Bord. Die einzige Änderung im Zustand, bei dem eine Benachrichtigung ausgelöst werden, ist eine Transaktion festgeschrieben wird.

Zuverlässige Status-Manager verwaltet eine Auflistung von zuverlässigen Staaten zuverlässige Wörterbuch und zuverlässige Warteschlange. Zuverlässige Status-Manager wird Benachrichtigungen ausgelöst, wenn diese Auflistung ändert: ein zuverlässiger Zustand hinzugefügt oder entfernt oder die gesamte Auflistung neu erstellt.
Auflistung zuverlässigen Status-Manager ist in drei Fällen wiederhergestellt:

- Recovery: Beginnt ein Replikat wiederhergestellt Ursprungszustand vom Datenträger. Am Ende der Wiederherstellung wird **NotifyStateManagerChangedEventArgs** ein Ereignis auszulösen, das wiederhergestellte zuverlässigen Status enthält.
- Vollständige Kopie: bevor ein Replikat Konfigurationssatz beitreten kann, muss erstellt werden. Manchmal erfordert dies eine vollständige Kopie der zuverlässigen Zustand Manager Status primäres Replikat im Leerlauf sekundären Replikat angewendet werden. Zuverlässige Status-Manager auf dem sekundären Replikat verwendet **NotifyStateManagerChangedEventArgs** ein Ereignis ausgelöst, das zuverlässige Status enthält, das vom primären Replikat erworben.
- Wiederherstellen: In Disaster Recovery-Szenarien kann das Replikat Zustand aus einer Sicherung über **RestoreAsync**wiederhergestellt werden. In solchen Fällen verwendet zuverlässige auf dem primären Replikat **NotifyStateManagerChangedEventArgs** ein Ereignis ausgelöst, das zuverlässige Status enthält, das aus der Sicherung wiederhergestellt.

Registrieren für Transaktionen und/oder Status-Manager benachrichtigt, müssen Sie sich mit den Ereignissen **TransactionChanged** oder **StateManagerChanged** auf zuverlässige Status-Manager. Diese Ereignishandler registriert wird häufig der Konstruktor der statusbehaftete Dienst. Beim Registrieren der Konstruktor wird keiner Benachrichtigung verpassen, die sich während der Lebensdauer des **IReliableStateManager**verursacht wird.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Der **TransactionChanged** -Ereignishandler verwendet **NotifyTransactionChangedEventArgs** Informationen über das Ereignis. Es enthält die Action-Eigenschaft (z. B. **NotifyTransactionChangedAction.Commit**), die den Typ der Änderung angibt. Darüber hinaus enthält die Transaction-Eigenschaft, die einen Verweis auf die Transaktion enthält, die geändert.

>[AZURE.NOTE] Heute sind **TransactionChanged** Ereignisse nur ausgelöst, wenn die Transaktion festgeschrieben ist. Die Aktion entspricht **NotifyTransactionChangedAction.Commit**. Aber für andere Buchung ändert möglicherweise in Zukunft Ereignisse ausgelöst werden. Wir empfehlen Aktion überprüfen und das Ereignis verarbeitet, ist, die Sie erwarten.

Es folgt ein Beispiel **TransactionChanged** -Ereignishandler.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Der **StateManagerChanged** -Ereignishandler verwendet **NotifyStateManagerChangedEventArgs** Informationen über das Ereignis.
**NotifyStateManagerChangedEventArgs** hat zwei Unterklassen: **NotifyStateManagerRebuildEventArgs** und **NotifyStateManagerSingleEntityChangedEventArgs**.
Sie verwenden die Action-Eigenschaft in **NotifyStateManagerChangedEventArgs** **NotifyStateManagerChangedEventArgs** die richtige Unterklasse umgewandelt:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** und **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Es folgt ein Beispiel **StateManagerChanged** Benachrichtigungshandler.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Zuverlässige Wörterbuch Benachrichtigungen

Zuverlässiges Wörterbuch bietet für die folgenden Ereignisse:

- Rebuild: Aufgerufen, wenn **ReliableDictionary** den Zustand aus einem lokalen Zustand wiederhergestellt oder kopierte oder Sicherung wiederhergestellt ist.
- Löschen: Aufgerufen, wenn der Zustand der **ReliableDictionary** über die **ClearAsync** -Methode gelöscht wurde.
- Hinzufügen: Aufgerufen, wenn **ReliableDictionary**ein Element hinzugefügt wurde.
- Update: Aufgerufen, wenn ein Element im **IReliableDictionary** aktualisiert wurde.
- Entfernen: Aufgerufen, wenn ein Element im **IReliableDictionary** gelöscht wurde.

Um zuverlässige Wörterbuch Benachrichtigungen müssen Sie den **DictionaryChanged** -Ereignishandler auf **IReliableDictionary**registrieren. Diese Ereignishandler registriert wird häufig in **ReliableStateManager.StateManagerChanged** Benachrichtigung hinzufügen.
Registrierung beim **IReliableStateManager** **IReliableDictionary** hinzugefügt wird sichergestellt, dass Benachrichtigung verpassen.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** ist das Beispiel, das im vorhergehenden **OnStateManagerChangedHandler** wird.

Dieser Code legt die Schnittstelle **IReliableNotificationAsyncCallback** mit **DictionaryChanged**. Da **NotifyDictionaryRebuildEventArgs** eine **IAsyncEnumerable** -Schnittstelle enthält rebuild muss asynchron – aufgelistet werden Benachrichtigungen über **RebuildNotificationAsyncCallback** statt **OnDictionaryChangedHandler**ausgelöst werden.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Im vorangehenden Code als Teil der Verarbeitung Benachrichtigung Wiederherstellung wird zuerst der verwaltete zusammengesetzte Zustand gelöscht. Da zuverlässige Auflistung mit neuen neu erstellt wird, werden alle vorherigen Benachrichtigung irrelevant.

Der **DictionaryChanged** -Ereignishandler verwendet **NotifyDictionaryChangedEventArgs** Informationen über das Ereignis.
**NotifyDictionaryChangedEventArgs** hat fünf Unterklassen. Verwenden Sie die Action-Eigenschaft in **NotifyDictionaryChangedEventArgs** **NotifyDictionaryChangedEventArgs** die richtige Unterklasse umgewandelt:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** und **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Empfehlung

- *Führen Benachrichtigungsereignisse so schnell wie möglich.*
- *Nicht* teure Vorgänge (z. B. e/a-Vorgänge) als Teil der synchrone Ereignisse ausgeführt.
- *Überprüfen den Aktivitätstyp, bevor das Ereignis verarbeitet.* Neue Aktion möglicherweise in Zukunft hinzugefügt werden.

Hier sind einige Dinge zu beachten:

- Nachrichten werden im Rahmen der Ausführung eines Vorgangs ausgelöst. Beispielsweise wird eine Wiederherstellung Benachrichtigung als letzten Schritt einer Wiederherstellung ausgelöst. Wiederherstellung wird nicht abgeschlossen, bis das Benachrichtigungsereignis verarbeitet wird.
- Da Nachrichten als Teil der Anwendung ausgelöst werden, siehe Clients nur Benachrichtigungen für lokal festgeschriebenen Vorgänge. Und da Operationen garantiert nur lokal übernommen werden (also protokolliert) sie oder kann nicht rückgängig gemacht werden in der Zukunft.
- Auf dem Pfad wiederherstellen wird eine einzelne Benachrichtigung für jeden zugewiesenen Vorgang ausgelöst. Daher enthält Transaktion T1 Create(X), Delete(X) und Create(X), eine Benachrichtigung für die Erstellung von X für den Löschvorgang für die Erstellung wieder in dieser Reihenfolge abgerufen werden.
- Für Transaktionen, die mehrere Vorgänge enthalten, werden Vorgänge in der Reihenfolge angewendet in denen primären Replikat vom Benutzer empfangen wurden.
- Als Teil der falsche Status Verarbeitung können einige Vorgänge rückgängig gemacht werden. Benachrichtigung für diese Vorgänge rückgängig Rollback des Status des Replikats auf einen stabilen Punkt ausgelöst. Ein wichtiger Unterschied rückgängig Benachrichtigungen werden Ereignisse mit doppelten Schlüssel aggregiert werden. Wenn Transaktion T1 rückgängig gemacht wird wird, werden Sie eine einzelne Benachrichtigung Delete(X) angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Zuverlässige Sammlungen](service-fabric-work-with-reliable-collections.md)
- [Zuverlässige Dienste Schnellstart](service-fabric-reliable-services-quick-start.md)
- [Zuverlässige Sicherung und Wiederherstellung (Disaster Recovery)](service-fabric-reliable-services-backup-restore.md)
- [Entwicklerreferenz für zuverlässige Sammlungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
