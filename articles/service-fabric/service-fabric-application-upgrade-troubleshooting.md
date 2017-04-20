<properties
   pageTitle="Problembehandlung bei Anwendungsupgrades | Microsoft Azure"
   description="Dieser Artikel behandelt einige häufig auftretende Probleme, um die Aktualisierung einer Anwendung Service Fabric und deren Behebung."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Problembehandlung bei Upgrades der Anwendung

Dieser Artikel behandelt einige der Probleme, um die Aktualisierung einer Anwendung Azure Service Fabric und deren Behebung.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Problembehandlung bei einer Anwendung aktualisieren

Wenn ein Upgrade fehlschlägt, enthält die Ausgabe des Befehls **Get-ServiceFabricApplicationUpgrade** zusätzliche Informationen zum Debuggen des Fehlers.  Die folgende Liste gibt Verwendung Weitere Informationen:

1. Bestimmen Sie den Fehlertyp.
2. Ermitteln Sie die Fehlerursache.
3. Isolieren Sie ein oder mehrere fehlerhafte Komponenten zur weiteren Untersuchung.

Diese Information steht Service Fabric erkennt Fehler unabhängig davon, ob die **FailureAction** zurücksetzen oder die Aktualisierung unterbrechen.

### <a name="identify-the-failure-type"></a>Bestimmen Sie den Fehler

In der Ausgabe von **Get-ServiceFabricApplicationUpgrade**gibt **FailureTimestampUtc** den Zeitstempel (UTC) mit einem Upgrade Fehler durch Service und **FailureAction** ausgelöst wurde. **FailureReason** bezeichnet drei allgemeine Ursachen des Fehlers:

1. UpgradeDomainTimeout - gibt an, dass bestimmte Aktualisierungsdomäne dauerte zu lange und **UpgradeDomainTimeout** abgelaufen.
2. OverallUpgradeTimeout - gibt an, dass die gesamte Aktualisierung dauerte zu lange und **UpgradeTimeout** abgelaufen.
3. HealthCheck - gibt an, nach der Aktualisierung einer Domäne aktualisieren, die Anwendung nach der angegebenen Integritätsrichtlinien fehlerhafte blieb **HealthCheckRetryTimeout** abgelaufen.

Diese Einträge werden nur in die Ausgabe Wenn die Aktualisierung fehlschlägt und Rollback. Weitere Informationen werden je nach Art des Fehlers angezeigt.

### <a name="investigate-upgrade-timeouts"></a>Upgrade-Timeouts überprüfen

Aktualisieren Sie Timeout, durch Verfügbarkeitsprobleme am häufigsten Fehler verursacht werden. Die Ausgabe nach diesem Absatz ist typisch für Upgrades, nicht mit Replikaten oder Instanzen in der neuen Codeversion gestartet werden. Das **UpgradeDomainProgressAtFailure** -Feld erfasst eine Momentaufnahme der ausstehenden Beginn der Arbeiten zum Zeitpunkt des Fehlers.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

In diesem Beispiel die Aktualisierung fehlgeschlagen bei Aktualisierungsdomäne *MYUD1* und stecken zwei Partitionen (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* und *4b43f4d8-b26b-424e-9307-7a7a62e79750*). Partitionen wurden fest, da die Laufzeit nicht primäre Replikate (*WaitForPrimaryPlacement*) auf *Knoten 1* und *Knoten 4*Zielknoten platzieren.

Der Befehl **Get-ServiceFabricNode** kann verwendet werden, um sicherzustellen, dass diese beiden Knoten in Domäne aktualisieren *MYUD1*. *Preupgradesafetycheck* sagt *PostUpgradeSafetyCheck*, was bedeutet, dass diese Sicherheitskontrollen nach allen Knoten in der Domäne aktualisieren aktualisieren auftreten. Diese Informationen verweist auf ein mögliches Problem mit der neuen Version des Anwendungscodes. Die häufigsten Probleme sind Service Fehler öffnen oder Förderung primäre Pfade.

Ein *Preupgradesafetycheck* des *Upgradephase* bedeutet gab die Upgrade-Domäne vorbereiten, bevor es ausgeführt wurde. Die häufigsten Probleme sind in diesem Fall Service Fehler schließen oder Herabstufung von primären Codepfaden.

Der aktuelle **UpgradeState** ist *RollingBackCompleted*ursprüngliche Aktualisierung mit einer **FailureAction**durchgeführt muss die automatisch Aktualisierung bei einem Fehler ein Rollback. Bei die ursprüngliche Aktualisierung mit manuellen **FailureAction**würden dann die Aktualisierung stattdessen angehalten zu live der Anwendung Debuggen werden.

### <a name="investigate-health-check-failures"></a>Health Check-Fehler überprüfen

Health Check-Fehler können durch verschiedene Probleme ausgelöst werden, die auftreten können, nachdem alle Knoten in einer Domäne aktualisieren aktualisieren und alle Sicherheitskontrollen übergeben. Die Ausgabe nach diesem Absatz ist typisch für ein Upgrade Fehler durch fehlerhafte Gesundheitskontrolle. Das **UnhealthyEvaluations** -Feld erfasst eine Momentaufnahme der Fehler bei der Aktualisierung der angegebenen [Integritätsrichtlinien](service-fabric-health-introduction.md)Gesundheitskontrolle.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Health Check-Fehler untersuchen muss zunächst verstehen des Zustandsmodells Service Fabric. Aber auch ohne diese Kenntnisse, wir sehen, dass zwei Dienste fehlerhaft sind: *Stoff: DemoApp/Svc3* und *Stoff: DemoApp/Svc2*, zusammen mit den Fehlerberichte (in diesem Fall "InjectedFault"). In diesem Beispiel sind zwei der vier Dienste fehlerhaft, unter Standardziel 0 % fehlerhaften (*MaxPercentUnhealthyServices*).

Die Aktualisierung wurde auf fehlerhafte durch Angabe einer **FailureAction** Handbuch Beginn der Aktualisierung angehalten. Dieser Modus ermöglicht zu live-System ist fehlgeschlagen, bevor weitere Maßnahmen.

### <a name="recover-from-a-suspended-upgrade"></a>Unterbrochene Aktualisierung wiederherstellen

Mit einem **FailureAction**ist nicht erforderlich, da die Aktualisierung automatisch auf fehlerhafte Rollback wiederherstellen. Mit einem manuellen **FailureAction**sind verschiedene Wiederherstellungsoptionen:

1. Manuell Auslösen eines Rollbacks
2. Der Rest des Upgrades manuell durchlaufen
3. Die überwachte Aktualisierung fortsetzen

Befehl **Start ServiceFabricApplicationRollback** kann jederzeit verwendet werden, zum Starten die Anwendung wiederherstellen. Nachdem der Befehl erfolgreich zurückgegeben wird, die Rollback-Anforderung im System registriert wurde und danach startet.

Die **Resume-ServiceFabricApplicationUpgrade** kann verwendet werden, der Rest des Upgrades manuell durchlaufen einer Domäne gleichzeitig aktualisieren. In diesem Modus werden nur Sicherheit vom System ausgeführt. Mehr Zustand geprüft. Dieser Befehl kann nur verwendet werden, wenn *UpgradeState* zeigt *aufweist*, was bedeutet, dass die aktuelle Domäne aktualisieren aktualisieren (Ausstehend) nächsten wurde nicht gestartet.

**Update-ServiceFabricApplicationUpgrade** Befehl überwachten Upgrade mit Sicherheit wieder verwendet werden und Gesundheitskontrollen durchgeführt.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Die Aktualisierung weiterhin Aktualisierungsdomäne, wo der letzte angehalten, und dasselbe Parameter und Integritätsrichtlinien als vor dem upgrade verwenden. Bei Bedarf können Upgrade-Parameter und Richtlinien in der obigen Ausgabe dargestellt in einem Befehl geändert werden bei die Aktualisierung. In diesem Beispiel wurde die Aktualisierung im Modus überwacht die Parameter mit den Integritätsrichtlinien unverändert fortgesetzt.

## <a name="further-troubleshooting"></a>Weitere Fehlerbehebung

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric folgt nicht die angegebenen Integritätsrichtlinien

1. mögliche Ursache:

Service Fabric Istzahlen Entitäten (Replikate, Partitionen und Services) für Gesundheitszustands aller Prozentsätze übersetzt und ganze Entitäten immer aufrundet. Z. B. wenn der maximale _MaxPercentUnhealthyReplicasPerPartition_ 21 % und fünf Replikate, dann Service Fabric können bis zu zwei fehlerhafte Replikate (, is,'Math.Ceiling (5\*0,21)). Daher sollten Richtlinien entsprechend festgelegt.

2. mögliche Ursache:

Integritätsrichtlinien werden prozentual insgesamt Dienste und nicht Instanzen angegeben. Beispielsweise vor der Aktualisierung, wenn eine Anwendung vier Dienstinstanzen A, B, C und D, D-Dienst fehlerhaft ist jedoch mit wenig Einfluss auf die Anwendung. Den fehlerhaften Dienst D aktualisieren und Festlegen der Parameter *MaxPercentUnhealthyServices* 25 %, wenn nur A, B und C müssen fehlerfrei ignorieren soll.

Allerdings kann während der Aktualisierung D gesund C fehlerhaft ist. Die Aktualisierung erfolgreich sein, da nur 25 % der fehlerhaft sind. Jedoch möglicherweise unerwartete Fehler durch C unerwartet fehlerhaft anstatt D. dadurch In diesem Fall D sollte als einen anderen Diensttyp aus A modelliert B und C. Da Integritätsrichtlinien pro Diensttyp angegeben sind, können andere fehlerhafte Prozentsatz Schwellenwerte auf verschiedene Dienste angewendet werden. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Ich haben eine Integritätsrichtlinie für die anwendungsaktualisierung nicht angegeben, aber die Aktualisierung dennoch fehlschlägt, für einige nie angegebenen Timeouts

Wenn Integritätsrichtlinien Upgrade Anforderung bereitgestellt werden, werden sie aus *ApplicationManifest.xml* aktuelle Version der Anwendung übernommen. Z. B. Wenn Sie von Version 1.0 auf Version 2.0 Anwendung Integritätsrichtlinien gemäß Version 1.0 Anwendung X aktualisieren werden verwendet. Wenn eine andere Richtlinie für die Aktualisierung verwendet werden soll, muss die Richtlinie als Teil der Anwendung aktualisieren API-Aufruf angegeben werden. Als Teil der API-Aufruf angegebenen Richtlinien gelten nur während der Aktualisierung. Sobald die Aktualisierung abgeschlossen ist, werden in der *ApplicationManifest.xml* angegebenen Richtlinien verwendet.

### <a name="incorrect-time-outs-are-specified"></a>Falsche Timeouts angegeben

Sie können Vorgänge Timeouts uneinheitlich festgelegt sind gefragt haben. Beispielsweise müssen Sie eine *UpgradeTimeout* , die kleiner als *UpgradeDomainTimeout*ist. Die Antwort ist, dass ein Fehler zurückgegeben wird. Fehler werden zurückgegeben, wenn *UpgradeDomainTimeout* kleiner ist als die Summe der *HealthCheckWaitDuration* und *HealthCheckRetryTimeout*oder *UpgradeDomainTimeout* kleiner als die Summe der *HealthCheckWaitDuration* und *HealthCheckStableDuration*ist.

### <a name="my-upgrades-are-taking-too-long"></a>Meine Upgrades sind zu lange dauert.

Die Zeit für ein Upgrade abgeschlossen hängt Healthchecks und Timeouts angegeben. Healthchecks und Timeouts, hängt davon ab, wie lange es dauert, zu kopieren, bereitstellen und Stabilisieren der Anwendung. Zu aggressiv mit Timeouts bedeuten mehr fehlgeschlagenen Upgrades, so empfehlen wir konservativ mit längeren Timeouts.

Hier ist eine kurze Gedächtnisstütze die Timeouts mit der Aktualisierung Interaktion:

Upgrades für eine Aktualisierungsdomäne *HealthCheckWaitDuration*schneller abschließen kann + *HealthCheckStableDuration*.

Aktualisierung kann nicht auftreten schneller *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Die Zeit für eine Aktualisierungsdomäne ist durch *UpgradeDomainTimeout*begrenzt.  Wenn *HealthCheckRetryTimeout* und *HealthCheckStableDuration* beide NULL sind und der Zustand der Anwendung wechseln hält, dann das Upgrade auf *UpgradeDomainTimeout*schließlich Timeout. *UpgradeDomainTimeout* beginnt nach dem Beginn der Aktualisierung für die aktuelle Domäne aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Sie die Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch ein Upgrade der Anwendung mit Visual Studio.

[Aktualisieren Sie die Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine anwendungsaktualisierung mit PowerShell.

Steuern Sie, wie Ihre Anwendung aktualisiert [Aktualisieren Parameter](service-fabric-application-upgrade-parameters.md).

Machen Sie die Upgrades der Anwendung zu [Daten](service-fabric-application-upgrade-data-serialization.md)Serialisierung kompatibel.

Informationen Sie zum erweiterten Funktionen beim Upgrade Ihrer Anwendung auf [Weiterführende Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von Problemen in Anwendungsupgrades auf die Schritte [Zur Problembehandlung Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
 
