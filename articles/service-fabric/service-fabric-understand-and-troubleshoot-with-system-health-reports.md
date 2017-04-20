<properties
   pageTitle="Beheben Sie mit Systemberichte | Microsoft Azure"
   description="Beschreibt die Berichte von Azure Service Fabric-Komponenten und ihre Verwendung zur Problembehandlung Cluster und Anwendung gesendet."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="use-system-health-reports-to-troubleshoot"></a>Mit Systemberichte zu Problembehandlung

Azure Service Fabric Bericht Komponenten standardmäßig auf alle Elemente im Cluster. [Zustand speichern](service-fabric-health-introduction.md#health-store) erstellt und löscht Elemente, basierend auf der Systemberichte. Es organisiert auch in einer Hierarchie, die Element Interaktionen erfasst.

> [AZURE.NOTE] Zum Verständnis der Konzepte Gesundheit lesen [Fabric Service Health](service-fabric-health-introduction.md)Modell.

Systemberichte bieten Einblick in Cluster und Anwendungsfunktionen und Flag über Gesundheit. Programme und Dienste überprüfen Systemberichte, Entitäten werden ordnungsgemäß hinsichtlich Service Fabric verhält. Berichte bieten keine Gesundheitsberichterstattung Geschäftslogik des Dienstes oder Erkennung von hängenden Prozessen. Dienste können Gesundheitsdaten mit speziellen Informationen zur Logik bereichern.

> [AZURE.NOTE] Watchdogs Berichte sind nur *nach* die Komponenten eine Einheit sichtbar. Wenn ein Element gelöscht wird, löscht den Speicher Zustand automatisch alle Berichte zugeordnet. Gleiches gilt beim Erstellen eine neue Instanz der Entität (z. B. eine neue Dienstinstanz Replikat erstellt wird). Alle Berichte, die mit der alten Instanz gelöscht und aus dem Speicher entfernt.

Die Systemkomponente Berichte sind anhand der Quelle mit der "**System.**" Präfix. Überwachungen können wie Berichte mit ungültigen Parametern abgelehnt werden dasselbe Präfix für ihre Quellen verwenden.
Sehen wir uns einige Systemberichte sie auslöst und mögliche Probleme zu beheben, stellen sie verstehen.

> [AZURE.NOTE] Fabric Service weiterhin Berichte auf Zinsen hinzufügen, die Transparenz der Vorgänge im Cluster und Anwendung verbessern.

## <a name="cluster-system-health-reports"></a>Cluster Systemberichte
Die Cluster Health Entität wird im Speicher Zustand automatisch erstellt. Wenn alles ordnungsgemäß funktioniert, muss es einen Systembericht nicht.

### <a name="neighborhood-loss"></a>Themenwelt Verlust
**System.Federation** meldet einen Fehler erkennt Nachbarschaft Verlust. Der Bericht ist einzelne Knoten und Knoten-ID im Eigenschaftennamen enthalten. Wenn eine Umgebung in den gesamten Service Fabric verloren, erwarten Sie normalerweise zwei Ereignisse (beide Seiten des Berichts Lücke). Weitere Themenwelten verloren, treten weitere Ereignisse.

Der Bericht gibt das Timeout global Leasing als die Gültigkeitsdauer. Der Bericht wird jede Hälfte TTL für umgeleitet, als der Zustand aktiv. Das Ereignis wird automatisch entfernt, wenn es abläuft. Entfernen Sie abgelaufene sichergestellt, dass der Bericht aus dem Health Speicher ordnungsgemäß bereinigt wird auch bei der Berichterstattung Knoten.

- **SourceId**: System.Federation
- **Eigenschaft**: beginnt mit **Nachbarschaft** und Knoten enthält
- **Nächste Schritte**: untersuchen, warum der Umgebung verloren geht (z. B. die Kommunikation zwischen Clusterknoten überprüfen).

## <a name="node-system-health-reports"></a>Knoten Systemberichte
**System.FM**, entspricht den Failover-Manager-Dienst ist, der Informationen zu Clusterknoten verwaltet. Jeder Knoten muss ein Bericht System.FM zeigt den Zustand. Knoten Elemente werden entfernt, wenn der Knotenstatus entfernt wird (siehe [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Knoten nach oben/unten
System.FM meldet OK, wenn der Knoten des Rings Beitritt (wird ausgeführt). Meldet einen Fehler, wenn der Knoten den Ring verlässt (ist Sie entweder aktualisieren oder einfach da ausgefallen ist). Gesundheit Hierarchie erstellt Health Store Handlung bereitgestellten Entitäten mit System.FM Knoten Berichte. Den Knoten betrachtet virtuelle übergeordnete aller bereitgestellten Elemente. Die bereitgestellten Elemente auf diesem Knoten werden durch Abfragen verfügbar, wenn der Knoten als verfügbar gemeldet wird durch System.FM der gleichen Instanz wie die Instanz der Entitäten zugeordnet. Wenn System.FM meldet, dass der Knoten heruntergefahren ist oder neu gestartet (neue Instanz), bereinigt Health Shop automatisch bereitgestellten Entitäten, die nur auf Knoten oder die vorherige Instanz des Knotens vorhanden sein können.

- **SourceId**: System.FM
- **Eigenschaft**: Status
- **Nächste Schritte**: Wenn der Knoten ausfällt Aktualisierung, es sollte kommen wieder nachdem sie aktualisiert wurde. In diesem Fall sollte der Zustand auf OK gewechselt werden. Wenn der Knoten nicht abgeben oder diese nicht benötigt das Problem genauer untersucht.

Im folgende Beispiel wird das Ereignis System.FM mit Zustand OK für Knoten:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Gültigkeitsdauer
**System.FabricNode** gibt eine Warnung aus, wenn bei Ablauf des Knotens verwendeten Zertifikate sind. Es gibt drei Zertifikate pro Knoten: **Certificate_cluster**, **Certificate_server**und **Certificate_default_client**. Bei Ablauf mindestens zwei Wochen ist der Bericht Zustand OK. Bei Ablauf innerhalb von zwei Wochen, ist der Bericht eine Warnung. Gültigkeitsdauer dieser Ereignisse ist, und sie werden entfernt, wenn ein Knoten den Cluster verlässt.

- **SourceId**: System.FabricNode
- **Eigenschaft**: beginnt **mit** und enthält Informationen zu den Zertifikatstyp
- **Nächste Schritte**: Zertifikate sind in der Nähe Ablaufdatum aktualisieren.

### <a name="load-capacity-violation"></a>Kapazität Verstoß laden
Service Fabric-Lastenausgleich meldet eine Warnung erkennt einen Knoten Kapazität Verstoß.

 - **SourceId**: System.PLB
 - **Eigenschaft**: beginnt **mit**
 - **Nächste Schritte**: Überprüfen Metriken und die aktuelle Kapazität auf den Knoten.

## <a name="application-system-health-reports"></a>Anwendung Systemberichte
**System.CM**, entspricht den Cluster Manager-Dienst ist, der Informationen über eine Anwendung verwaltet.

### <a name="state"></a>Zustand
System.CM meldet als OK Anwendung erstellt oder aktualisiert wurde. Darüber informiert Health Shop bei die Anwendung gelöscht wurde, damit es aus dem Speicher entfernt werden kann.

- **SourceId**: System.CM
- **Eigenschaft**: Status
- **Nächste Schritte**: Wenn die Anwendung erstellt wurde, sollte es Integritätsbericht Clusterverwaltung. Überprüfen Sie den Status der Anwendung, durch die Ausgabe einer Abfrage (z. B. das PowerShell-Cmdlet * *Get-ServiceFabricApplication ApplicationName *ApplicationName***).

Das folgende Beispiel zeigt das Status-Ereignis auf die **Fabric: / WordCount** Anwendung:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>System Health Berichte
**System.FM**, entspricht den Failover-Manager-Dienst ist, der Informationen verwaltet.

### <a name="state"></a>Zustand
System.FM meldet als OK Wenn der Dienst erstellt wurde. Wenn der Dienst gelöscht wurde, werden die Entität aus dem Speicher Gesundheit gelöscht.

- **SourceId**: System.FM
- **Eigenschaft**: Status

Das folgende Beispiel zeigt die Status für den Dienst **Stoff: WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Nicht positionierte Replikate Verstoß
**System.PLB** gibt eine Warnung aus, wenn eine Position für eine oder mehrere Replikaten gefunden werden kann. Der Bericht wird entfernt, wenn es abläuft.

- **SourceId**: System.FM
- **Eigenschaft**: Status
- **Nächste Schritte**: Service Integritätsregeln und den aktuellen Status des Praktikums überprüfen.

Das folgende Beispiel zeigt einen Verstoß für einen Dienst mit 7 Ziel in einem Cluster mit 5 Knoten konfiguriert:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Partition Systemberichte
**System.FM**, entspricht den Failover-Manager-Dienst ist, der Informationen über Servicepartitionen verwaltet.

### <a name="state"></a>Zustand
System.FM meldet als OK die Partition erstellt wurde und fehlerfrei ist. Wenn die Partition gelöscht wird, werden die Entität Health Store gelöscht.

Wenn die Partition unter Anzahl der minimalen Replikat ist, meldet einen Fehler. Wenn die Partition nicht unter die minimale Replikat Anzahl ist unterhalb der Zielanzahl Replikat ist jedoch eine Warnung gemeldet. Wenn die Partition Quorum Verlust ist, meldet System.FM Fehler.

Andere wichtige Ereignisse enthalten eine Warnung bei die Neukonfiguration länger dauert als erwartet und der Build länger dauert als erwartet. Die erwarteten Zeiten für Build- und Neukonfiguration konfiguriert werden basierend auf Szenarien. Hat ein Dienst Terabyte Zustand wie SQL-Datenbank hat Build für einen Dienst mit wenig Zustand länger.

- **SourceId**: System.FM
- **Eigenschaft**: Status
- **Nächste Schritte**: Wenn der Integritätsstatus nicht OK ist, ist möglich, dass einige Replikate nicht erstellt, geöffnet, oder primären oder sekundären ordnungsgemäß zum. In vielen Fällen ist die Ursache ein Service-Fehler in der Implementierung öffnen oder Rolle ändern.

Das folgende Beispiel zeigt eine gesunde Partition:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Das folgende Beispiel zeigt den Zustand einer Partition unter Zielanzahl Replikat. Der nächste Schritt ist zu Konfiguration zeigt Beschreibung Partition: **MinReplicaSetSize** 2 und **TargetReplicaSetSize** 7. Erhalten Sie die Anzahl der Knoten im Cluster: fünf. So darf in diesem Fall zwei Replikate befinden.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Verletzung der Replikat-Einschränkung
**System.PLB** gibt eine Warnung aus, erkennt die Verletzung einer Replikat-Einschränkung und Replikate der Partition kann nicht eingefügt werden.

- **SourceId**: System.PLB
- **Eigenschaft**: beginnt mit **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Replikat Systemberichte
**System.RA**, entspricht die Neukonfiguration Agent-Komponente ist der Behörde für das Replikat Zustand.

### <a name="state"></a>Zustand
**System.RA** meldet als OK Wenn das Replikat erstellt wurde.

- **SourceId**: System.RA
- **Eigenschaft**: Status

Das folgende Beispiel zeigt eine fehlerfreie Kopie:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Replikatstatus öffnen
Die Beschreibung dieser Integritätsbericht enthält die Startzeit (Coordinated Universal Time) beim Aufruf des API-Aufrufs.

**System.RA** gibt eine Warnung aus, nimmt das Replikat öffnen länger als die konfigurierte (Standard: 30 Minuten). Wenn die API Verfügbarkeit auswirkt, wird der Bericht schneller (einem konfigurierbaren Intervall mit einem Standardwert von 30 Sekunden) ausgegeben. Gemessene Zeit umfasst die Zeit für Open Replicator und die Service. Die Eigenschaft wird zu OK Wenn öffnen abgeschlossen ist.

- **SourceId**: System.RA
- **Eigenschaft**: **ReplicaOpenStatus**
- **Nächste Schritte**: Wenn der Integritätsstatus nicht OK ist, untersuchen Sie Warum geöffneten Replikat länger dauert als erwartet.

### <a name="slow-service-api-call"></a>Langsame Service API-Aufruf
Bericht für **System.RAP** und **System.Replicator** eine Warnung, wenn ein Aufruf an den Dienst Benutzercode dauert länger als die konfigurierte Zeit. Die Warnung wird gelöscht, wenn der Aufruf abgeschlossen ist.

- **SourceId**: System.RAP oder System.Replicator
- **Eigenschaft**: der Name der langsamen API. Die Beschreibung enthält weitere Informationen über die Zeit die API ausstehend.
- **Nächste Schritte**: untersuchen, warum der Aufruf länger dauert als erwartet.

Das folgende Beispiel zeigt eine Partition Quorum verloren gehen und die Untersuchung Schritte durchgeführt, um herauszufinden, warum. Eines der Replikate haben einen Warnung Zustand deren Status abrufen. Es zeigt, dass der Vorgang länger dauert als erwartet, ein Ereignis System.RAP gemeldet. Nach dieser Informationen Erhalt werden im nächste Schritt sehen Sie sich den Code, und untersuchen Sie es. Für diesen Fall auslöst **RunAsync** Implementierung der statusbehaftete Dienst eine nicht behandelte Ausnahme. Replikate werden wiederverwendet, so keine Replikate in den Warnzustand angezeigt. Wiederholen Sie immer den Zustand und Unterschiede in der Replikat-ID suchen In bestimmten Fällen können Sie Wiederholungsversuche Hinweise geben.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Beim Starten der fehlerhaften Anwendung im Debugger anzeigen Diagnoseereignissen Windows Ausnahme von RunAsync:

![Visual Studio 2015 Ereignisse: RunAsync Fehler im Fabric: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 Ereignisse: RunAsync-Fehler in **Stoff: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Vollständige Replikationswarteschlange
**System.Replicator** gibt eine Warnung aus, wenn die Replikationswarteschlange voll ist. Auf dem primären liegt dies normalerweise eine oder mehrere sekundäre Replikate langsame Vorgänge bestätigt werden. Auf dem sekundären Server wahrscheinlich dadurch der Dienst langsam Operationen angewendet wird. Die Warnung wird gelöscht, wenn die Warteschlange nicht voll ist.

- **SourceId**: System.Replicator
- **Eigenschaft**: **PrimaryReplicationQueueStatus** oder **SecondaryReplicationQueueStatus**je nach replikatrolle

### <a name="slow-naming-operations"></a>Langsame Vorgänge Benennung

**System.NamingService** meldet Status auf dessen primäres Replikat, wenn Naming-Vorgang länger als zulässig dauert. Naming Vorgänge sind [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) oder [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Weitere Methoden FabricClient, z. B. unter [Management](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx) [Service managementmethoden](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) oder Eigenschaft finden Sie unter.

> [AZURE.NOTE] Naming Service löst Namen im Cluster an und ermöglicht die Verwaltung von Namen und Eigenschaften. Es ist eine partitionierte Service Fabric Service beibehalten. Eine Partition stellt Behörde Besitzer enthält Metadaten für alle Service Fabric und Services. Service Fabric-Namen sind unterschiedliche Partitionen genannt Inhabers Partitionen so der Dienst extensible zugeordnet. Weitere Informationen über [Naming Service](service-fabric-architecture.md).

Wenn Naming Vorgang länger dauert als erwartet, wird der Vorgang mit einer Warnung *primäres Replikat der Naming Service-Partition, die den Vorgang fungiert,*gekennzeichnet. Wenn der Vorgang erfolgreich abgeschlossen wird, ist die Warnung deaktiviert. Wenn der Abschluss des Vorgangs ein Fehler enthält der Bericht Health Details über den Fehler.

- **SourceId**: System.NamingService
- **Eigenschaft**: beginnt mit Präfix **Duration_** und langsamer Vorgang und auf die die Operation angewendet Service Fabric-Namen identifiziert. Beispielsweise wenn Dienst an Namen erstellt: MyApp/MyService dauert zu lange, kann die Eigenschaft Duration_AOCreateService.fabric:/MyApp/MyService. AO verweist auf die Rolle der Benennung Partition für diesen Namen und dieses Vorgangs.
- **Nächste Schritte**: Kontrollkästchen warum die Benennung durchgeführt. Jeder Vorgang kann verschiedene Ursachen haben. Löschen Sie beispielsweise Service kann auf einem Knoten fest, da Anwendungshost auf einem Knoten durch einen Benutzer Fehler in den Code abstürzt.

Das folgende Beispiel zeigt einen Dienstvorgang erstellen. Der Vorgang dauerte länger als die konfigurierte Dauer. AO wiederholt und Nr. Arbeit an NICHT den letzten Vorgang mit Timeout abgeschlossen. In diesem Fall ist das gleiche Replikat primäre AO und keine Rollen.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication Systemberichte
**System.Hosting** ist der bereitgestellten Entitäten.

### <a name="activation"></a>Aktivierung
System.Hosting meldet, als OK Wenn eine Anwendung auf dem Knoten erfolgreich aktiviert wurde. Andernfalls wird ein Fehler erzeugt.

- **SourceId**: System.Hosting
- **Eigenschaft**: Aktivierung, einschließlich der Einführung Version
- **Nächste Schritte**: Wenn die Anwendung fehlerhaft ist, überprüfen Sie fehlschlagen die Aktivierung.

Das folgende Beispiel zeigt die erfolgreichen Aktivierung:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Herunterladen
**System.Hosting** meldet einen Fehler, wenn Paketdownload Anwendung fehlschlägt.

- **SourceId**: System.Hosting
- **Eigenschaft**: * *herunterladen:*RolloutVersion***
- **Nächste Schritte**: untersuchen, warum der Download auf dem Knoten ist fehlgeschlagen.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage Systemberichte
**System.Hosting** ist der bereitgestellten Entitäten.

### <a name="service-package-activation"></a>Paket Aktivierung
System.Hosting meldet als OK, wenn die serviceaktivierung-Paket auf dem Knoten erfolgreich ist. Andernfalls wird ein Fehler erzeugt.

- **SourceId**: System.Hosting
- **Eigenschaft**: Aktivierung
- **Nächste Schritte**: untersuchen, warum die Aktivierung fehlgeschlagen.

### <a name="code-package-activation"></a>Aktivieren des Pakets Code
**System.Hosting** meldet OK für jedes Paket, wenn die Aktivierung erfolgreich ist. Wenn die Aktivierung fehlschlägt, wird eine Warnung konfiguriert. Wenn **CodePackage** nicht aktivieren, mit Fehler größer als der konfigurierte **CodePackageHealthErrorThreshold endet**meldet hostet einen Fehler. Enthält ein Service-Paket mehrere Pakete, wird jeweils ein Aktivierung Bericht generiert.

- **SourceId**: System.Hosting
- **Eigenschaft**: verwendet das Präfix **CodePackageActivation** und enthält den Namen der das Paket und der Einstiegspunkt als * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint-EntryPoint* ** (z. B. **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Service-Registrierung
**System.Hosting** Berichte als OK Diensttyp erfolgreich registriert wurde. Es meldet einen Fehler, erfolgt die Registrierung Zeit war nicht (wie mit **ServiceTypeRegistrationTimeout**konfiguriert). Typ des Knotens aufgehoben wird, ist die Laufzeit geschlossen wurde. Hosting, meldet eine Warnung.

- **SourceId**: System.Hosting
- **Eigenschaft**: verwendet das Präfix **ServiceTypeRegistration** und enthält den Typnamen Service (z. B. **ServiceTypeRegistration:FileStoreServiceType**)

Das folgende Beispiel zeigt ein fehlerfrei bereitgestellten Service-Paket:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Herunterladen
**System.Hosting** meldet einen Fehler, schlägt der Download des Service-Paket.

- **SourceId**: System.Hosting
- **Eigenschaft**: * *herunterladen:*RolloutVersion***
- **Nächste Schritte**: untersuchen, warum der Download auf dem Knoten ist fehlgeschlagen.

### <a name="upgrade-validation"></a>Prüfung der Aktualisierung
**System.Hosting** meldet einen Fehler, wenn Validierungsfehler während der Aktualisierung oder das Upgrade auf dem Knoten fehlschlägt.

- **SourceId**: System.Hosting
- **Eigenschaft**: verwendet das Präfix **FabricUpgradeValidation** und enthält die Updateversion
- **Beschreibung**: Zeigt die Fehler

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric Integritätsberichte anzeigen](service-fabric-view-entities-aggregated-health.md)

[Melden und Überprüfen des Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Überwachung und diagnose Dienste lokal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md)
