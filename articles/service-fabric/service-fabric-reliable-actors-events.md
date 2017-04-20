<properties
   pageTitle="Zuverlässige Akteure Ereignisse | Microsoft Azure"
   description="Einführung in Ereignisse für Service Fabric zuverlässig."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Actor-Ereignisse
Schauspieler Ereignisse bieten eine Möglichkeit, bestmöglichen Schauspieler Clients Benachrichtigungen. Akteur Ereignisse für Schauspieler Clientkommunikation vorgesehen und sollte nicht für Schauspieler Actor-Kommunikation verwendet werden.

Die folgenden Codeausschnitte veranschaulichen die Actor-Ereignisse in Ihrer Anwendung verwenden.

Definieren Sie eine Schnittstelle, die vom Actor veröffentlichte Ereignisse beschreibt. Diese Schnittstelle muss abgeleitet werden die `IActorEvents` Schnittstelle. Der Methoden sein [Datenvertrag serialisierbar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Die Methoden müssen void zurückgeben, als Ereignis Benachrichtigung und bemühen.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Deklarieren Sie die Ereignisse des Akteurs im Actor-Schnittstelle veröffentlicht.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Implementieren Sie den Ereignishandler auf der Clientseite.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Auf dem Client erstellt einen Proxy für den Akteur, der das Ereignis veröffentlicht und abonnieren die zugehörigen Ereignisse.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Bei einem Failover möglicherweise der Akteur über einen anderen Prozess oder Knoten. Schauspieler-Proxy aktive Abonnements verwaltet und automatisch neu abonniert. Das erneuten Intervall durch Steuern der `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Um Ihr Abonnement zu verwenden, die `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Auf den Akteur einfach zu veröffentlichen die Ereignisse auftreten. Abonnenten das Ereignis gibt, wird Akteure Runtime sie die Benachrichtigung.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Nächste Schritte
 - [Schauspieler Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Schauspieler-Diagnose und Überwachung](service-fabric-reliable-actors-diagnostics.md)
 - [Schauspieler-API-Dokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispielcode](https://github.com/Azure/servicefabric-samples)
