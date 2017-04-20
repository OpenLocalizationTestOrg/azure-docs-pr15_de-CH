<properties
   pageTitle="Erste Schritte mit Service Fabric zuverlässige | Microsoft Azure"
   description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen, Debuggen und Bereitstellen eine einfache Akteur-Service mit Service Fabric zuverlässige Akteure."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit zuverlässigen

> [AZURE.SELECTOR]
- [C# unter Windows](service-fabric-reliable-actors-get-started.md)
- [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)

Dieser Artikel erläutert die Grundlagen der Azure Service Fabric zuverlässige Akteure und führt Sie durch die Erstellung, Debuggen und Bereitstellen einer einfachen zuverlässige Akteur-Anwendung in Visual Studio.

## <a name="installation-and-setup"></a>Installation und setup
Bevor Sie beginnen, daß die Fabric Service Development Environment auf Ihrem Computer eingerichtet haben.
Einrichten, finden Sie unter Informationen zum [Einrichten der Development Environment](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Zunächst mit zuverlässigen müssen Sie nur einige grundlegende Konzepte verstehen:

 * **Schauspieler-Service**. Zuverlässige Akteure werden im zuverlässige Dienste zusammengefasst, die in der Service-Fabric-Infrastruktur bereitgestellt werden. Schauspieler-Instanzen werden in eine benannte Instanz aktiviert.
 
 * **Schauspieler-Registrierung**. Wie muss zuverlässige Dienste zuverlässige Akteur Service Service Fabric-Runtime registriert werden. Darüber hinaus muss der Akteurtyp Akteur Runtime registriert werden.
 
 * **Actor-Schnittstelle**. Akteur-Schnittstelle wird verwendet, um eine stark typisierte, öffentliche Schnittstelle eines Akteurs definieren. Zuverlässige Akteur Modell Terminologie definiert die Schnittstelle Akteur Arten von Nachrichten, die der Akteur verstehen und verarbeiten. Actor-Schnittstelle wird von anderen Akteuren und Clientanwendungen senden "(asynchron) Nachrichten an den Akteur" verwendet. Zuverlässige Akteure können mehrere Schnittstellen implementieren.
 
 * **ActorProxy-Klasse**. ActorProxy-Klasse wird von Clientanwendungen zum Actor-Schnittstelle offen gelegten Methoden aufrufen. ActorProxy-Klasse bietet zwei wichtige Funktionen:
    * Auflösen des Namens: den Akteur im Cluster (Suchen den Knoten im Cluster gehostet wird) zu finden ist.
    * Fehlerbehandlung: kann Methodenaufrufe wiederholen und neu auflösen Akteur Speicherort, z. B. ein Fehler, der Akteur auf einen anderen Knoten im Cluster verschoben werden muss.

Erwähnenswert sind Akteur Schnittstellen betreffen Folgendes:

- Schauspieler Methoden können überladen werden.
- Schauspieler-Schnittstelle müssen Methoden nicht, Ref oder optionale Parameter.
- Generische Schnittstellen werden nicht unterstützt.

## <a name="create-a-new-project-in-visual-studio"></a>Erstellen eines neuen Projekts in Visual Studio
Nach der Installation der Service Fabric-Tools für Visual Studio können Sie neue Projekttypen erstellen. Neue Projekttypen sind unter der Kategorie **Cloud** im Dialogfeld **Neues Projekt** .


![Service Fabric-Tools für Visual Studio - neues Projekt][1]

Im nächsten Dialogfeld können Sie den Typ des Projekts, die Sie erstellen möchten.

![Service Fabric-Projektvorlagen][5]

Für das Projekt "HelloWorld" Wir nutzen Service Fabric zuverlässige Akteure.

Nachdem die Projektmappe erstellt haben, erhalten Sie die folgende Struktur:

![Service Fabric Projektstruktur][2]

## <a name="reliable-actors-basic-building-blocks"></a>Zuverlässige Akteure Grundbausteine

Eine normale zuverlässige Akteure drei besteht aus:

* **Das Anwendungsprojekt (MyActorApplication)**. Dies ist das Projekt, das alle Dienste für die Bereitstellung zusammen verpackt. Es enthält die *ApplicationManifest.xml* und PowerShell Skripts zum Verwalten der Anwendung.

* **Das Projekt (MyActor.Interfaces)**. Dies ist das Projekt, das die Schnittstellendefinition für den Akteur enthält. Im Projekt MyActor.Interfaces definieren Sie die Schnittstellen, die von Akteuren in der Projektmappe verwendet wird. Schauspieler Schnittstellen können in jedem Projekt mit beliebigem Namen definiert werden, jedoch die Schnittstelle der Akteur Vertrag definiert, der der Akteur-Implementierung gemeinsam und Aufrufen des Akteurs Clients, in der Regel ist es sinnvoll in einer Assembly definiert, die unabhängig von der Akteur-Implementierung und können von mehreren Projekten gemeinsam genutzt werden.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Das Dienstprojekt Akteur (MyActor)**. Dies ist das Projekt Service Fabric Service definiert, der den Akteur gehostet wird. Es enthält die Implementierung des Akteurs. Eine Akteur-Implementierung ist eine Basisklasse abgeleitete Klasse `Actor` und implementiert die Schnittstellen, die im MyActor.Interfaces-Projekt definiert sind. Eine Akteursklasse implementieren muss auch einen Konstruktor akzeptiert ein `ActorService` Instanz und `ActorId` und übergibt sie an die Basisklasse `Actor` Klasse. Dadurch Abhängigkeitsinjektion Konstruktor der Plattform abhängig.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Actor-Dienst muss in der Runtime Service Fabric mit Service registriert. Damit der Akteur Dienst Akteur Instanzen ausführen muss Actor-Typ auch mit dem Actor-Dienst registriert werden. Die `ActorRuntime` Registrierung führt diese Akteure.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Wenn Sie über ein neues Projekt in Visual Studio starten und nur ein Akteur Definition, ist die Registrierung standardmäßig im Code enthalten, der Visual Studio generiert. Wenn Sie andere Akteure im Dienst definieren, müssen Sie Akteur-Registrierung hinzufügen:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Die Runtime Service Fabric Akteure gibt einige [Ereignisse und Leistungsindikatoren im Zusammenhang mit Akteur-Methoden](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Sie eignen sich Diagnose und Überwachung.


## <a name="debugging"></a>Debuggen

Service Fabric-Tools für Visual Studio auf dem lokalen Computer Debuggen unterstützt. Die Taste F5, um eine Debugsitzung zu starten. Visual Studio erstellt (falls notwendig) Pakete. Außerdem stellt die Anwendung auf dem lokalen Cluster Service Fabric bereit und fügt den Debugger.

Während der Bereitstellung den Fortschritt im Fenster **Ausgabe** angezeigt.

![Service Fabric Debug-Ausgabefenster][3]


## <a name="next-steps"></a>Nächste Schritte
 - [Verwendung von zuverlässigen Akteure Service Fabric-Plattform](service-fabric-reliable-actors-platform.md)
 - [Schauspieler Verwaltung](service-fabric-reliable-actors-state-management.md)
 - [Schauspieler-Lebenszyklus und Garbage collection](service-fabric-reliable-actors-lifecycle.md)
 - [Schauspieler-API-Dokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Beispielcode](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
