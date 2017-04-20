<properties
   pageTitle="Polymorphismus in Framework zuverlässige Akteure | Microsoft Azure"
   description="Erstellen von Hierarchien .NET Schnittstellen und zuverlässige Akteure Framework Wiederverwendung von Funktionen und API-Definitionen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorphismus in Framework zuverlässige Akteure

Zuverlässige Akteure Rahmen können Sie unter Verwendung zahlreicher Techniken, die Verwendung objektorientierter Entwurf Akteure erstellen. Diese Techniken gehört Polymorphie, die Typen und Schnittstellen mehr erben Eltern verallgemeinert. Vererbung im Rahmen zuverlässige Akteure folgt im Allgemeinen .NET Modell mit ein paar zusätzliche Einschränkungen.

## <a name="interfaces"></a>Schnittstellen

Zuverlässige Akteure Framework müssen Sie definieren mindestens eine Schnittstelle von der Akteurtyp implementiert werden. Diese Schnittstelle wird verwendet, um eine Proxyklasse generieren, die von Clients verwendet werden kann, mit der Kommunikation. Schnittstellen können von anderen Schnittstellen erben, wie jede Schnittstelle, die von einem Akteurtyp implementiert und alle übergeordneten letztendlich von IActor abgeleitet. IActor ist die Plattform Basisschnittstelle für Akteure. So kann klassische Polymorphie Beispiel Formen wie folgt aussehen:

![Schnittstellenhierarchie Form Akteure][shapes-interface-hierarchy]


## <a name="types"></a>Typen

Sie können auch eine Typhierarchie Akteur, erstellen die von Actor-Basisklasse abgeleitet werden, die von der Plattform bereitgestellt. Bei Shapes möglicherweise Basis `Shape` Typ:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypen von `Shape` Methoden von der Basisklasse überschreiben.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Hinweis Die `ActorService` Attribut auf den Akteur. Dieses Attribut mitgeteilt, zuverlässige Akteur, dass einen Dienst zum Hosten von Akteuren dieses Typs automatisch erstellt werden soll. Möglicherweise möchten einen Basistyp erstellen, der ausschließlich für Funktionen mit Untertypen und nie instanziiert konkrete Akteure verwendet wird. In diesen Fällen verwenden Sie die `abstract` Schlüsselwort, Sie nie Schauspieler je nach Typ erstellt werden.


## <a name="next-steps"></a>Nächste Schritte

- [Wie zuverlässig Akteure Framework Service Fabric-Plattform nutzt](service-fabric-reliable-actors-platform.md) Zuverlässigkeit, skalierbar und konsistenten Zustand anzeigen
- Erfahren Sie mehr über [Schauspieler Lebenszyklus](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
