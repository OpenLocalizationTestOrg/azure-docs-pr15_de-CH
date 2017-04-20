<properties
   pageTitle="Prüfbarkeit: Service-Kommunikation | Microsoft Azure"
   description="Dienst-Kommunikation ist eine wichtige Integration Service Fabric-Anwendung. Dieser Artikel beschreibt die Vorüberlegungen und Testverfahren."
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
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Service Fabric Prüfbarkeit Szenarien: Service-Kommunikation

Microservices und Service-orientierte Architektur Stile Fläche natürlich in Azure Service Fabric. In diesen verteilten Architekturen gegliederte Microservice Anwendung normalerweise mehrere Dienste bestehen, die miteinander kommunizieren. Selbst die einfachsten Fällen müssen Sie im Allgemeinen eine statusfreie Webdienst und eine statusbehaftete Data Storage Service, der kommunizieren müssen.

Dienst-Kommunikation ist eine wichtige Integrationspunkt einer Anwendung jeden Dienst remote API für andere Dienste verfügbar macht. Arbeiten mit API-Grenzen, in der Regel bei der e/a, erfordert etwas Sorgfalt mit viel Tests und Validierung.

Es gibt zahlreiche Aspekte bei dieser Dienstgrenzen in einem verteilten System miteinander verbunden sind:

 - *Transportprotokoll*. Verwenden Sie HTTP für Interoperabilität oder benutzerdefinierten binären Protokolls für maximalen Durchsatz?
 - *Fehlerbehandlung*. Wie werden permanente oder temporäre Fehler behandelt? Was geschieht, wenn ein Dienst auf einen anderen Knoten verschoben wird?
 - *Timeouts und Wartezeit*. In mehrstufigen Anwendung wird wie jeder Dienstebene Wartezeit durch den Stapel und die Benutzer?

Verwenden Sie eine integrierte Kommunikationskomponenten des Service Fabric bereitgestellt oder Erstellen eigener, Testen der Interaktionen zwischen Ihrem ist für eine Flexibilität in der Anwendung.

## <a name="prepare-for-services-to-move"></a>Vorbereitung der Dienste verschieben

Instanzen können mit der Zeit verschieben. Dies gilt insbesondere, wenn sie mit Load Metriken für maßgeschneiderte optimale Ressourcenausgleich konfiguriert sind. Service Fabric verschiebt die Dienstinstanzen maximieren die Verfügbarkeit auch bei Upgrades, Failover, Skalierung und andere Situationen, die im Laufe eines verteilten Systems auftreten.

Wenn Dienste im Cluster verschieben, sollte Ihre Clients und anderen Diensten bereit zwei Szenarios spricht für einen Dienst:

- Das Service-Instanz oder Partition Replikat wurde seit dem letzten, Sprach. Dies ist ein normaler Bestandteil des servicelebenszyklus und sollte während der Lebensdauer der Anwendung geschehen erwartet werden.
- Das Service-Instanz oder Partition Replikat ist verschieben. Tritt, zwar ein Failover von einem Knoten zu einem anderen Dienst sehr schnell in Service Fabric möglicherweise eine Verzögerung in der Verfügbarkeit ist die Kommunikationskomponente Ihres Dienstes nur langsam gestartet.

Diese Szenarien ordnungsgemäß behandeln ist wichtig für einen reibungslosen System. Hierzu Folgendes bedenken:

- Jeder Dienst, die mit, hat eine *Adresse* , die sie (z. B. HTTP oder WebSockets) überwacht. Wenn eine Dienstinstanz oder Partition, wechselt seine Adresse Endpunkt. (Verschoben auf einen anderen Knoten mit einer anderen IP-Adresse.) Wenn Sie die integrierte Kommunikationskomponenten verwenden, werden sie neu auflösen serviceadressen behandelt.
- Möglicherweise eine vorübergehende Erhöhung Service Wartezeit als die Instanz gestartet der Listener erneut. Dies hängt davon ab, wie schnell der Dienst den Listener öffnet nach dem Verschieben der Dienstinstanz.
- Alle vorhandenen Partnerschaften müssen geschlossen und wieder geöffnet, nachdem der Dienst auf einem anderen Knoten geöffnet. Knoten ordnungsgemäßes Herunterfahren oder Neustart kann bei vorhandenen Verbindungen ordnungsgemäß heruntergefahren werden.

### <a name="test-it-move-service-instances"></a>Testen: Dienstinstanzen verschieben

Mithilfe von Service Fabric Prüfbarkeit Tools können Sie ein Testszenario testen Situationen unterschiedlich erstellen:

1. Verschieben Sie eine statusbehaftete Dienst primäres Replikat.

    Primäre Replikat der statusbehafteten Servicepartition kann verschiedene Gründe verschoben werden. Verwenden Sie diese auf das primäre Replikat einer bestimmten Partition, wie Ihre Dienste zum Umzug auf kontrollierte Weise reagieren.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Beenden eines Knotens.

    Beendet ein Knoten verschoben Fabric Service alle Instanzen oder Partitionen, die auf diesem Knoten auf einen anderen verfügbaren Knoten im Cluster. Können Sie eine Situation testen, wo ein Knoten aus Cluster und alle Dienstinstanzen und Replikate auf diesem Knoten verschieben.

    Sie können einen Knoten mit PowerShell **Stop-ServiceFabricNode** -Cmdlet beenden:

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Verwalten der Verfügbarkeit

Als Plattform Fabric Service bietet hohe Verfügbarkeit der Dienste. Aber in extremen Fällen zugrunde liegende Infrastruktur weiterhin Probleme nicht verfügbar. Es ist wichtig, diese Szenarien zu testen.

Statusbehaftete Dienste verwenden ein Quorum-System replizieren Zustand für hohe Verfügbarkeit. Dies bedeutet, dass ein Quorum von Replikaten für Schreibvorgänge durchführen. In seltenen Fällen wie weit Hardwarefehler Quorum Replikate möglicherweise nicht verfügbar. In diesen Fällen keine Vorgänge ausgeführt werden, aber dennoch Lesevorgänge ausgeführt werden.

### <a name="test-it-write-operation-unavailability"></a>Testen: Schreiben Operation nicht verfügbar

Mithilfe der Tools Prüfbarkeit Fabric Service können Sie einen Fehler einfügen, der Quorum verloren gehen als Test verursacht. Obwohl dieses Szenario selten ist, ist es wichtig, dass Kunden und statusbehaftete Dienst abhängigen Dienste zur Behandlung von Situationen, in denen sie herstellen können Anfragen zu schreiben. Außerdem ist es wichtig, dass der statusbehaftete Dienst selbst diese Möglichkeit ist und kann ordnungsgemäß Aufrufern kommunizieren.

Sie können quorumverlust mit PowerShell **Invoke-ServiceFabricPartitionQuorumLoss** -Cmdlet auslösen:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

In diesem Beispiel legen wir `QuorumLossMode` , `QuorumReplicas` an dem quorumverlust ohne Sie alle Replikate auslösen soll. Auf diese Weise sind Lesevorgänge möglich. Testen ein Szenarios eine ganze Partition nicht verfügbar ist, können Sie diese Option auf festlegen `AllReplicas`.

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen über Prüfbarkeit Aktionen](service-fabric-testability-actions.md)

[Erfahren Sie mehr über Prüfbarkeit Szenarien](service-fabric-testability-scenarios.md)
