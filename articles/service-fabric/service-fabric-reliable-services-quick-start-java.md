<properties
   pageTitle="Erste Schritte mit zuverlässige Dienste | Microsoft Azure"
   description="Einführung in die statusfreie und statusbehaftete Microsoft Azure Service Fabric-Anwendung erstellen."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Erste Schritte mit zuverlässige Dienste

> [AZURE.SELECTOR]
- [C# unter Windows](service-fabric-reliable-services-quick-start.md)
- [Java unter Linux](service-fabric-reliable-services-quick-start-java.md)

Dieser Artikel erläutert die Grundlagen der Azure Fabric zuverlässigen Dienst und führt Sie durch das Erstellen und Bereitstellen einer einfachen zuverlässigen Service-Anwendung in Java geschrieben.

## <a name="installation-and-setup"></a>Installation und setup
Bevor Sie beginnen, stellen Sie sicher, die Fabric Service Development Environment auf Ihrem Computer eingerichtet haben.
Benötigen Sie es einrichten, gehen Sie [Erste Schritte mit Mac](service-fabric-get-started-mac.md) oder [Erste Schritte unter Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundlegende Konzepte
Um zuverlässige Dienste Einstieg müssen Sie nur einige grundlegende Konzepte verstehen:

 - **Diensttyp**: Dies ist Ihre Service-Implementierung. Wird durch die Klasse schreiben, erweitert definiert `StatelessService` und anderen Code oder Abhängigkeiten, einen Namen und eine Versionsnummer verwendet.

 - **Benannte Dienstinstanz**: zum Ausführen des Dienstes erstellen benannte Instanzen Ihres Typs Service viel wie Instanzen eines Klassentyps zu erstellen. Instanzen sind Instanziierungen eines Objekts von der Dienstklasse, die Sie schreiben. 

 - **Diensthost**: benannte Dienstinstanzen erstellen müssen, der in einem Host ausgeführt. Der Diensthost ist nur ein Prozess, wo Instanzen des Dienstes ausgeführt werden.

 - **Anmeldung**: Anmeldung vereint alles. Der Diensttyp muss mit Service Fabric Laufzeit Diensthost Service Fabric erstellen Instanzen zu ausführen registriert werden.  

## <a name="create-a-stateless-service"></a>Statusfreien Dienst erstellen

Erstellen einer neuen Service Fabric-Anwendung starten. Service Fabric-SDK für Linux enthält ein Yeoman Generator Gerüstbau für Service Fabric-Anwendung mit einer statusfreien bereitstellen. Starten Sie mit den folgenden Yeoman Befehl:

```bash
$ yo azuresfjava
```

Führen Sie die Schritte zum Erstellen eines **Statusfreien zuverlässig**. Für dieses Lernprogramm benennen die Anwendung "HelloWorldApplication" und die "Hello World". Das Ergebnis sind Verzeichnisse für die `HelloWorldApplication` und `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Implementieren des Dienstes

Öffnen Sie **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Diese Klasse definiert den Diensttyp und kein Code ausgeführt. Die Dienst-API bietet zwei Einstiegspunkte für den Code:

 - Eine offene Einstiegspunktmethode aufgerufen `runAsync()`, beginnen Sie können alle Arbeitslasten einschließlich langer Compute-Workloads ausgeführt.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Einen Einstiegspunkt für die Kommunikation, der Kommunikationsstapel Wahl einstecken. Deshalb beginnen Sie Anfragen von Benutzern und anderen Diensten.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

In diesem Lernprogramm wir konzentrieren uns auf die `runAsync()` Einstiegspunktmethode. Dies ist unmittelbar beginnen Sie Ihren Code ausführt.

### <a name="runasync"></a>RunAsync

Die Plattform ruft diese Methode auf, wenn eine Instanz eines Dienstes platzierten und ausgeführt. Der Zyklus öffnen und Schließen einer Dienstinstanz kann mehrmals im Laufe des Dienstes insgesamt auftreten. Dies kann aus verschiedenen Gründen geschehen:

- Das System verschiebt die Dienstinstanzen Ressourcenausgleich.
- Fehler auftreten in Ihrem Code.
- Die Anwendung oder das System wird aktualisiert.
- Die zugrunde liegende Hardware auftreten ein Ausfalls.

Diese Orchestrierung wird durch Service Ihren Dienst hochgradig verfügbaren und ausgewogene beibehalten.

#### <a name="cancellation"></a>Abbruch

Wichtig ist, Code in `runAsync()` können beendet bei Benachrichtigung durch Service Fabric. Die `CompletableFuture` von zurückgegebenen `runAsync()` wird abgebrochen, wenn Service Fabric erfordert den Dienst stoppen. Im folgenden Beispiel wird veranschaulicht, wie ein Abbruch Ereignis behandeln: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Registrierung

Typen müssen mit Service Fabric-Runtime registriert werden. Der Diensttyp gemäß dem `ServiceManifest.xml` und die Dienstklasse implementiert `StatelessService`. Registrierung erfolgt in der Haupteinstiegspunkt Prozess. In diesem Beispiel ist der Haupteinstiegspunkt Prozess `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Führen Sie die Anwendung

Die Yeoman Gerüstbau enthält ein Gradle Skript die Anwendung und bash Skripten bereitzustellen und un-bereitzustellen. Führen Sie die Anwendung zunächst erstellen Sie mit Gradle:

```bash
$ gradle
```

Dies erzeugt ein Service Fabric-Anwendungspaket, das mit Service Fabric Azure CLI bereitgestellt werden können. Das Skript install.sh enthält erforderlichen Azure CLI-Befehle an das Anwendungspaket bereitstellen. Führen Sie einfach das Skript install.sh bereitstellen:

```bask
$ ./install.sh
```
