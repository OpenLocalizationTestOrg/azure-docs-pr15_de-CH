<properties
   pageTitle="Zuverlässige Akteure auf Akteur Notizen Serialisierung | Microsoft Azure"
   description="Beschreibt Mindestanforderungen für serialisierbare Klassen Service Fabric zuverlässige Akteure Zustände und Schnittstellen definieren"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Notizen auf Service Fabric zuverlässige Akteure Serialisierung


Methoden, Typen jede Methode in einer Schnittstelle Akteur zurückgegebene Aufgaben und Objekte in eines Akteurs Status-Manager sein [Datenvertrag serialisierbar](https://msdn.microsoft.com/library/ms731923.aspx). Dies gilt auch für die Argumente der [Akteur Ereignisschnittstellen](service-fabric-reliable-actors-events.md#actor-events)definierten Methoden. (Akteur Ereignismethoden immer void zurückgeben.)

## <a name="custom-data-types"></a>Benutzerdefinierte Datentypen

In diesem Beispiel definiert die folgenden Actor-Schnittstelle eine Methode, die einen benutzerdefinierten Datentyp namens `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Die Schnittstelle ist Impelemented Schauspieler, die den Status-Manager verwendet, um Speichern einer `VoicemailBox` Objekt:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

In diesem Beispiel die `VoicemailBox` Objekt serialisiert bei:
 - Das Objekt wird zwischen einer Instanz Akteur und einem Aufrufer übertragen.
 - Das Objekt wird in der Status-Manager gespeichert, auf einem Datenträger gespeichert und an andere Knoten repliziert.
 
Zuverlässige Akteur verwendet Datenvertragsserialisierung. Daher müssen benutzerdefinierte Objekte und ihre Member mit **DataContract-** und **DataMember** -Attributen bzw. gekennzeichnet werden

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Nächste Schritte
 - [Schauspieler-Lebenszyklus und Garbage collection](service-fabric-reliable-actors-lifecycle.md)
 - [Schauspieler-Zeitgeber und Erinnerung](service-fabric-reliable-actors-timers-reminders.md)
 - [Actor-Ereignisse](service-fabric-reliable-actors-events.md)
 - [Schauspieler Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Schauspieler Polymorphismus und objektorientierte Entwurfsmuster](service-fabric-reliable-actors-polymorphism.md)
 - [Schauspieler-Diagnose und Überwachung](service-fabric-reliable-actors-diagnostics.md)
