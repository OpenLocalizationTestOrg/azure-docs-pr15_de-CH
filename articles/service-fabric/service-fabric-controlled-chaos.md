<properties
   pageTitle="Auslösen von Chaos in Service Fabric-Cluster | Microsoft Azure"
   description="Verwenden Cluster Analysis Service-APIs und Fault Injection Chaos im Cluster verwalten."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Kontrolliertes Chaos in Service Fabric-Cluster auslösen
Umfangreiche verteilte Systeme wie Cloud-Infrastrukturen generell unzuverlässig. Azure Service Fabric ermöglicht Entwicklern, zuverlässige Dienste auf eine unzuverlässige Infrastruktur schreiben. Robuste Services schreiben müssen Entwickler Fehler gegen solche unzuverlässige Infrastruktur die Stabilität ihrer Dienste testen auslösen können.

Fault Injection und Cluster Analysis Service (auch der Fehler Analysis) können Entwickler Fehler testen Aktionen auslösen. Aber man gezielte simulierter Fehler nur so weit. Nehmen Sie weiter testen, können Sie Chaos.

Chaos simuliert kontinuierlich, überlappende Fehler (sicheres und fehlerhaftes) im Cluster über längere Zeit. Nach dem Konfigurieren von Chaos mit dem und welche Fehler können starten oder über C# APIs oder PowerShell Fehler in dem Cluster und den Dienst zu beenden.

Laufender Chaos erzeugt verschiedene Ereignisse, die den Zustand zum Zeitpunkt der Ausführung aufzuzeichnen. Ein ExecutingFaultsEvent enthält z. B. alle Fehler, die in dieser Iteration ausgeführt werden. Ein ValidationFailedEvent enthält die Details eines Fehlers, das während der clustervalidierung gefunden wurde. Sie können die GetChaosReportAsync-API zu Bericht Chaos führt aufrufen.

## <a name="faults-induced-in-chaos"></a>Fehler im Chaos ausgelöst
Chaos generiert Fehler im gesamten Service Fabric-Cluster und komprimiert Fehler in Monaten oder Jahren in ein paar Stunden angezeigt werden. Aus überlappenden Fehler bei hoher Fehler findet Ausnahmefällen, die normalerweise übersehen werden. In dieser Übung Chaos führt eine deutliche Verbesserung der Codequalität des Dienstes.

Chaos verursacht Fehler in folgenden Kategorien:

 - Schalten
 - Starten einer bereitgestellten Paket
 - Entfernen eines Replikats
 - Starten eines Replikats
 - Verschieben Sie ein primäres Replikat (konfigurierbar)
 - Verschieben Sie eine sekundäre Kopie (konfigurierbar)

Chaos in mehreren Iterationen ausgeführt wird. Jede Iteration besteht und clustervalidierung für den angegebenen Zeitraum. Sie können den Zeitaufwand für den Cluster zu stabilisieren und Validierung erfolgreich. Wenn im clustervalidierung Fehler gefunden, Chaos generiert und ValidationFailedEvent mit der UTC-Zeitstempel und der Fehler weiterhin besteht.

Betrachten Sie beispielsweise eine Instanz des Chaos Stunde mit maximal drei gleichzeitige Fehler ausgeführt. Chaos führt drei Fehler und überprüft dann die Cluster Health. Durchläuft den vorherigen Schritt, bis sie explizit durch die StopChaosAsync-API beendet oder eine Stunde ist übergibt. Wird der Cluster Arbeitsprozesse in jeder Iteration (d. h. er ist nicht stabilisieren innerhalb der konfigurierten Zeit), Chaos wird eine ValidationFailedEvent generiert. Dieses Ereignis zeigt an, dass etwas schiefgelaufen Untersuchung.

In seiner aktuellen Form verursacht Chaos sicher Fehler. Dies bedeutet, dass in Ermangelung externer Fehler quorumverlust oder Datenverlust nicht auftritt.

## <a name="important-configuration-options"></a>Wichtige Optionen
 - **TimeToRun**: Summe Ausführung dieses Chaos bevor sie erfolgreich abgeschlossen wurde. Vor Ablauf der Periode TimeToRun über die StopChaos-API können Sie Chaos beenden.
 - **MaxClusterStabilizationTimeout**: maximal gesund vor dem erneuten Überprüfen Cluster warten. Dabei ist die Last auf dem Cluster während wiederhergestellt wird. Geprüft werden:
    - Wenn der Cluster Zustand OK ist
    - Wenn der Dienststatus OK ist
    - Wenn das Zielreplikat bemessen wird für die Servicepartition erreicht.
    - TS Replikate vorhanden sind
 - **MaxConcurrentFaults**: die maximale Anzahl von gleichzeitigen Fehler, die in jeder Iteration verursacht werden. Je höher die Zahl, aggressivere Chaos ist. Dadurch komplexer Failover und Übergang Kombinationen. Chaos garantiert, dass keine externen Fehler kein Quorum verloren gehen oder Datenverlust hat unabhängig davon, wie hohen Wert dieser Konfiguration.
 - **EnableMoveReplicaFaults**: aktiviert oder deaktiviert die Fehler, die die primäre oder sekundäre Replikate zu verursachen. Diese Fehler sind standardmäßig deaktiviert.
 - **WaitTimeBetweenIterations**: die Zeitspanne zwischen Iterationen, d. h. nach einer entsprechenden Prüfung und warten.
 - **WaitTimeBetweenFaults**: die Zeitspanne zwischen zwei aufeinander folgenden Fehler in einer Iteration.

## <a name="how-to-run-chaos"></a>Das Chaos ausführen
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
