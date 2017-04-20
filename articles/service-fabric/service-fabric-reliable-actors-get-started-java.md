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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Erste Schritte mit zuverlässigen

> [AZURE.SELECTOR]
- [C# unter Windows](service-fabric-reliable-actors-get-started.md)
- [Java unter Linux](service-fabric-reliable-actors-get-started-java.md)

Dieser Artikel erläutert die Grundlagen der Azure Service Fabric zuverlässige Akteure und führt Sie durch das Erstellen und Bereitstellen einer einfachen zuverlässige Akteur-Anwendung in Java.

## <a name="installation-and-setup"></a>Installation und setup
Bevor Sie beginnen, stellen Sie sicher, die Fabric Service Development Environment auf Ihrem Computer eingerichtet haben.
Benötigen Sie es einrichten, gehen Sie [Erste Schritte mit Mac](service-fabric-get-started-mac.md) oder [Erste Schritte unter Linux](service-fabric-get-started-linux.md).

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

## <a name="create-an-actor-service"></a>Einen Akteur-Dienst erstellen
Erstellen einer neuen Service Fabric-Anwendung starten. Service Fabric-SDK für Linux enthält ein Yeoman Generator Gerüstbau für Service Fabric-Anwendung mit einer statusfreien bereitstellen. Starten Sie mit den folgenden Yeoman Befehl:

```bash
$ yo azuresfjava
```

Führen Sie die Schritte zum Erstellen eines **Akteurs zuverlässig**. Für dieses Lernprogramm benennen die Anwendung "HelloWorldActorApplication" und die "HelloWorldActor". Das folgende Gerüst erstellt:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Zuverlässige Akteure Grundbausteine

Die zuvor beschriebenen grundlegenden Konzepte übersetzen in die grundlegenden Bausteine eines zuverlässigen Akteur-Dienstes.

### <a name="actor-interface"></a>Actor-Schnittstelle

Die Schnittstellendefinition für den Akteur enthält. Diese Schnittstelle definiert den Akteur Vertrag, der Implementierung Akteur und Aufrufen des Schauspielers in der Regel sinnvoll, es an einem Ort definieren, unabhängig von der Akteur-Implementierung und gemeinsam für mehrere Dienste oder Clientanwendungen Clients freigegeben.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Schauspieler-service 
Enthält Ihre Implementierung Akteur und Registrierungscode Akteur. Schauspieler-Klasse implementiert die Schnittstelle Akteur. Dies ist der Akteur seine Arbeit tut.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Schauspieler-Registrierung

Actor-Dienst muss in der Runtime Service Fabric mit Service registriert. Damit der Akteur Dienst Akteur Instanzen ausführen muss Actor-Typ auch mit dem Actor-Dienst registriert werden. Die `ActorRuntime` Registrierung führt diese Akteure.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testclient

Dies ist eine einfache Testclientanwendung Sie können separat vom Service Fabric-Anwendung zum Testen der Akteur-Dienstes ausführen. Dies ist ein Beispiel, in dem die ActorProxy aktivieren und kommunizieren mit Akteur verwendet werden kann. Es ist kein Dienst bereitgestellt.

### <a name="the-application"></a>Die Anwendung 

Die Anwendung verpackt schließlich Actor-Dienst und andere Dienste hinzugefügten zukünftig zusammen für die Bereitstellung. Ort und *ApplicationManifest.xml* Inhaber Akteur Service-Paket enthält.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Die Yeoman Gerüstbau enthält ein Gradle Skript die Anwendung und bash Skripten bereitzustellen und un-bereitzustellen. Führen Sie die Anwendung zunächst erstellen Sie mit Gradle:

```bash
$ gradle
```

Dies erzeugt ein Service Fabric-Anwendungspaket, das mit Service Fabric Azure CLI bereitgestellt werden können. Das Skript install.sh enthält erforderlichen Azure CLI-Befehle an das Anwendungspaket bereitstellen. Führen Sie einfach das Skript install.sh bereitstellen:

```bask
$ ./install.sh
```
