<properties
   pageTitle="Azure Service Fabric Entitäten anzeigen aggregiert Gesundheit | Microsoft Azure"
   description="Beschreibt, wie Abfragen, anzeigen und Auswerten von Azure Service Fabric Entitäten aggregierten Health Status Abfragen und allgemeine Abfragen."
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

# <a name="view-service-fabric-health-reports"></a>Service Fabric Integritätsberichte anzeigen
Azure Service Fabric stellt ein [Modell](service-fabric-health-introduction.md) , die Gesundheit Entitäten besteht aus, die welche Komponenten und Überwachungen Umweltbedingungen melden können, die sie überwachen. [Zustand speichern](service-fabric-health-introduction.md#health-store) aggregiert alle Zustandsdaten, um zu bestimmen, ob Entitäten fehlerfrei sind.

Standardmäßig werden Cluster von Systemkomponenten gesendeten Berichte übernommen. Lesen Sie mehr zur [Verwendung Systemberichte zu beheben](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric bietet mehrere Methoden, um den zusammengesetzten Zustand Entitäten:

- [Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md) oder andere Visualisierung

- Status Abfragen (über PowerShell, API oder REST)

- Allgemeine Abfragen das Zurückgeben eine Liste von Entitäten, die Gesundheit als eine der Eigenschaften (über PowerShell, API oder REST)

Um diese Optionen zu demonstrieren, nehmen Sie einen lokalen Cluster mit fünf Knoten. Neben den **Fabric: / System** Anwendung (standardmäßig vorhanden), andere Programme bereitgestellt. Eines dieser Programme ist **Fabric: / WordCount**. Diese Anwendung enthält einen statusbehafteten Dienst mit sieben konfiguriert. Da nur fünf Knoten vorhanden sind, zeigen Systemkomponenten eine Warnung, dass die Partition unterhalb der Zielanzahl.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Gesundheit in Service Fabric-Explorer
Service Fabric-Explorer bietet eine visuelle Ansicht des Clusters. In der folgenden Abbildung sehen Sie, dass:

- Die Anwendung **Fabric: / WordCount** ist Rot (Fehler) ein Fehlerereignis für die Eigenschaft **Verfügbarkeit**von **MyWatchdog** gemeldet.

- Die Dienste eines **Stoff: WordCount/WordCountService** ist Gelb (Warnung). Da der Dienst mit sieben konfiguriert ist, darf sie alle befinden gibt es nur fünf Knoten. Obwohl es hier nicht angezeigt wird, ist die Service-Partition aufgrund der Systembericht gelb. Gelbe Partition wird gelben Service.

- Der Cluster ist aufgrund der roten Rot.

Die Bewertung wird Standardrichtlinien clustermanifest und Anwendungsmanifest verwendet. Strenge Richtlinien sind und keine Fehler toleriert.

Ansicht des Clusters mit Service Fabric-Explorer:

![Cluster mit Service Fabric-Explorer anzeigen][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Weitere Informationen über [Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Status Abfragen
Fabric Service macht Gesundheit Abfragen für jede der unterstützten [Objekttypen](service-fabric-health-introduction.md#health-entities-and-hierarchy). Sie erfolgt durch API (Methoden **FabricClient.HealthManager**entnehmen), PowerShell-Cmdlets und REST. Diese Abfragen zurückgeben von Informationen zu der Entität vollständige Health: die zusammengesetzten Zustand, Systemereignisse Entität untergeordnete Zustände (falls zutreffend) und fehlerauswertungen, wenn das Objekt nicht fehlerfrei ist.

> [AZURE.NOTE] Vollständig im Speicher Gesundheit gefüllt ist eine Entität Zustand zurückgegeben. Die Entität muss aktiv sein (nicht gelöscht) und einen Systembericht. Die übergeordneten Elemente der Hierarchiekette müssen Berichte. Wenn alle diese Bedingung nicht erfüllt ist, stimmen Status Abfragen, der zeigt, warum das Element nicht zurückgegeben wird.

Status Abfragen müssen Entitätsbezeichner übergeben den Entitätstyp abhängig. Die Abfragen akzeptieren Parameter optional Gesundheit. Wenn keine Integritätsrichtlinien angegeben werden, die [Integritätsrichtlinien](service-fabric-health-introduction.md#health-policies) des Manifests Cluster- oder für die Auswertung verwendet. Die Abfragen akzeptieren auch Filter für nur teilweise Kinder oder Ereignisse, die angegebenen Filter berücksichtigt.

> [AZURE.NOTE] Die Ausgabefilter werden auf den Server angewendet, sodass Nachricht Antworten verkleinert. Sie sollten Sie die Ausgabefilter verwenden, um die zurückgegebenen Daten einzuschränken, sondern als Filter auf der Clientseite.

Eine Entität Zustand enthält:

- Zusammengesetzten Zustand der Entität. Basierend auf Entität Berichte, untergeordnete Zustände (falls zutreffend) und Integritätsrichtlinien Health-Speicher berechnet. Weitere Informationen über [Entity Gesundheit Auswertung](service-fabric-health-introduction.md#entity-health-evaluation).  

- Die Systemereignisse in der Entität.

- Die Auflistung der Zustände aller untergeordneten Elemente für die Elemente, die untergeordnete Elemente aufweisen können. Entity-IDs und zusammengesetzten Zustand enthalten Zustände. Um vollständige Gesundheit für Kinder, Health Query für den Entitätstyp untergeordneten aufrufen und untergeordneten Bezeichner übergeben.

- Die fehlerauswertungen, die auf den Bericht, der den Zustand der Entität ausgelöst ist die Entität nicht fehlerfrei.

## <a name="get-cluster-health"></a>Cluster Gesundheit
Gibt den Zustand der Entität Cluster und Zustände von Programmen und Knoten (untergeordnete Cluster) enthält. Eingabe:

- [Optional] Die Cluster-Integritätsrichtlinie Knoten und Clusterereignisse ausgewertet.

- [Optional] Die Anwendung Health Policy Karte mit Integritätsrichtlinien manifest Anwendungsrichtlinien außer Kraft gesetzt.

- [Optional] Filter für Ereignisse, Knoten und Applikationen, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Gesundheit aggregiert Entität unabhängig von den Filter ausgewertet werden alle Ereignisse, Knoten und Applikationen verwendet.

### <a name="api"></a>API
Zu Cluster Health erstellen einen `FabricClient` und die **HealthManager** [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) -Methode aufrufen.

Der folgende Aufruf ruft die Cluster Health:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Im folgenden Code wird mithilfe einer benutzerdefinierten Cluster Gesundheits- und Filter für Knoten und Cluster Health. Erstellt [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), die die eingegebenen Informationen enthält.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Cluster Health ist [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .

Der Status des Clusters ist fünf Knoten, Systems und Fabric: / WordCount wie beschrieben konfiguriert.

Das folgende Cmdlet ruft Cluster Health mit Standard-Integritätsrichtlinien. Zusammengesetzten Zustand wird Warnung, da der Fabric: / WordCount-Anwendung Warnung. Beachten Sie, wie die fehlerauswertungen bezüglich der Angaben, die den zusammengesetzten Zustand ausgelöst.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Das folgende PowerShell-Cmdlet Ruft den Zustand des Clusters mithilfe einer benutzerdefinierten Anwendungsrichtlinie. Ergebnisse Fehler oder Warnung Applikationen und Knoten zu filtern. Daher werden keine Knoten zurückgegeben, alle fehlerfrei sind. Das Fabric: / WordCount-Anwendung Applikationen Filter berücksichtigt. Da die benutzerdefinierte Richtlinie gibt Warnungen als Fehler für den Stoff berücksichtigt: WordCount Anwendung, die als fehlerhaft ausgewertet, und der Cluster ist.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>REST
Sie können Cluster Health eine [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707669.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707696.aspx) mit Integritätsrichtlinien beschrieben im Text erhalten.

## <a name="get-node-health"></a>Knoten Gesundheit
Gibt den Zustand einer Entität Knoten und enthält die Systemereignisse auf dem Knoten gemeldet. Eingabe:

- [Required] Der Knotenname, das den Knoten identifiziert.

- [Optional] Die Cluster Health Benutzerrichtlinien Health ausgewertet.

- [Optional] Filter für Ereignisse, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse werden zum Health Entität aggregiert unabhängig von der Filter auswerten.

### <a name="api"></a>API
Zum Abrufen von Knoten Zustand über die API erstellen einen `FabricClient` und die HealthManager [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) -Methode aufrufen.

Im folgenden Code wird die Gesundheit Knoten für den angegebenen Namen:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Der folgende Code Ruft den Knoten Zustand für den angegebenen Namen und übergibt Ereignisse filtern und benutzerdefinierte Richtlinie über [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Knoten Health ist [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .
Das folgende Cmdlet Ruft den Knoten Zustand mit Standard-Integritätsrichtlinien:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Das folgende Cmdlet Ruft den Zustand aller Knoten im Cluster:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>REST
Sie können Knoten Gesundheit eine [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707650.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707665.aspx) mit Integritätsrichtlinien beschrieben im Text erhalten.

## <a name="get-application-health"></a>Anwendung Gesundheit
Gibt den Zustand einer Entität Anwendung. Es enthält die Zustände der bereitgestellten Anwendung und Service Kinder. Eingabe:

- [Required] Der Anwendungsname (URI), der die Anwendung identifiziert.

- [Optional] Die Anwendung Gesundheitspolitik manifest Anwendungsrichtlinien außer Kraft gesetzt.

- [Optional] Filter für Ereignisse, Services und bereitgestellte Anwendung, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse, Services und bereitgestellte Anwendung dienen Health Entität aggregiert unabhängig von der Filter auswerten.

### <a name="api"></a>API
Zum Abrufen des Anwendungszustands erstellen einen `FabricClient` und die HealthManager [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) -Methode aufrufen.

Der folgende Code Ruft die Anwendungsintegrität angegebene Anwendungsname (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Im folgenden Code wird die Anwendungsintegrität angegebene Anwendungsname (URI), Filter und benutzerdefinierte Richtlinien über [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx)angegeben.

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Anwendung Health ist [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet gibt den Zustand der **Fabric: / WordCount** Anwendung:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Das folgende PowerShell-Cmdlet übergibt benutzerdefinierte Richtlinien. Auch Filtern Kinder und Ereignisse.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Sie können Anwendungszustands eine [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707681.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707643.aspx) mit Integritätsrichtlinien beschrieben im Text erhalten.

## <a name="get-service-health"></a>Dienststatus abrufen
Gibt den Zustand einer Entität Service. Es enthält die Partition Zustände. Eingabe:

- [Required] Der Dienstname (URI), der den Dienst identifiziert.

- [Optional] Verwendet, um die Anwendungsrichtlinie manifest Anwendung Integritätsrichtlinie.

- [Optional] Filter für Ereignisse und Partitionen, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Gesundheit aggregiert Entität unabhängig von den Filter ausgewertet werden alle Ereignisse und Partitionen verwendet.

### <a name="api"></a>API
Zum Abrufen des Dienstzustands über die API erstellen einen `FabricClient` und die HealthManager [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) -Methode aufrufen.

Im folgenden Beispiel wird den Zustand des Dienstes mit angegebenen Dienstnamens (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Im folgenden Code wird der Dienststatus für den angegebenen Namen (URI), Filter und benutzerdefinierte Richtlinie über [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)angeben:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu den Dienststatus ist [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet ruft der Dienststatus mit Health Standardrichtlinien:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
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
```

### <a name="rest"></a>REST
Sie können Dienststatus mit [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707609.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707646.aspx) mit Integritätsrichtlinien beschrieben im Text erhalten.

## <a name="get-partition-health"></a>Partition Gesundheit
Gibt den Zustand einer Entität Partition. Es enthält die Replikat-Zustände. Eingabe:

- [Required] Die Partition ID (GUID), der die Partition identifiziert.

- [Optional] Verwendet, um die Anwendungsrichtlinie manifest Anwendung Integritätsrichtlinie.

- [Optional] Filter für Ereignisse und Replikate, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse und Replikate dienen Health Entität aggregiert unabhängig von der Filter auswerten.

### <a name="api"></a>API
Zu Partition Zustand über die API, Erstellen einer `FabricClient` und die HealthManager [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) -Methode aufrufen. Wenn Sie optionale Parameter angeben, erstellen Sie [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Partition Health ist [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet Ruft die Gesundheit für alle Partitionen der **Stoff: WordCount/WordCountService** Service:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können Health Partition mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707683.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707680.aspx) mit Integritätsrichtlinien beschrieben im Textkörper erhalten.

## <a name="get-replica-health"></a>Replikat Gesundheit
Gibt den Zustand der statusbehaftete dienstreplikat oder eine statusfreie Dienstinstanz. Eingabe:

- [Required] Die Partition ID (GUID) und Replikat ID, des Replikats angibt.

- [Optional] Die Anwendung Health Richtlinienparameter manifest Anwendungsrichtlinien außer Kraft gesetzt.

- [Optional] Filter für Ereignisse, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse werden zum Health Entität aggregiert unabhängig von der Filter auswerten.

### <a name="api"></a>API
Zum Abrufen des Replikat-Zustands über die API erstellen einen `FabricClient` und die HealthManager [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) -Methode aufrufen. Verwenden Sie [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx), um erweiterte Parameter.

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Gesundheit Replikat ist [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .

Das folgende Cmdlet Ruft den Zustand des primären Replikats für alle Partitionen des Dienstes:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie erhalten Replikat Zustand mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707673.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707641.aspx) mit Integritätsrichtlinien beschrieben im Textkörper.

## <a name="get-deployed-application-health"></a>Bereitgestellte Anwendung Gesundheit
Gibt den Zustand einer Anwendung auf eine knotenentität bereitgestellt. Es enthält bereitgestellte Dienst Paketzustände. Eingabe:

- [Required] Die Anwendung (URI) und Knotennamen (String), die bereitgestellte Anwendung zu identifizieren.

- [Optional] Die Anwendung Gesundheitspolitik manifest Anwendungsrichtlinien außer Kraft gesetzt.

- [Optional] Filter für Ereignisse und bereitgestellter Service-Pakete, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse und bereitgestellten Service-Pakete, zum Health Entität aggregiert unabhängig von der Filter auswerten.

### <a name="api"></a>API
Um den Zustand einer Anwendung auf einem Knoten über die API bereitgestellt bekommen, erstellen eine `FabricClient` und die HealthManager [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) -Methode aufrufen. Wenn Sie optionale Parameter angeben, verwenden Sie [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Gesundheit bereitgestellte Anwendung ist [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) . Um herauszufinden, wo eine Anwendung bereitgestellt wird, führen Sie [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) und Kinder bereitgestellte Anwendung betrachten.

Das folgende Cmdlet Ruft den Zustand der **Fabric: / WordCount** Anwendung auf **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können bereitgestellte Anwendung Health eine [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707644.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707688.aspx) mit Integritätsrichtlinien beschrieben im Text erhalten.

## <a name="get-deployed-service-package-health"></a>Zustand des bereitgestellten Pakets abrufen
Gibt den Zustand einer Entität bereitgestellten Service-Paket. Eingabe:

- [Required] Der Anwendungsname (URI) Knotenname (Zeichenfolge) und manifest Dienstname (String), die das Paket bereitgestellte Dienst identifizieren.

- [Optional] Verwendet, um die Anwendungsrichtlinie manifest Anwendung Integritätsrichtlinie.

- [Optional] Filter für Ereignisse, die angeben, welche Einträge von Interesse sind und im Ergebnis (z. B. nur Fehler oder Warnungen und Fehler) zurückgegeben werden soll. Alle Ereignisse werden zum Health Entität aggregiert unabhängig von der Filter auswerten.

### <a name="api"></a>API
Um den Zustand eines Pakets bereitgestellte Dienst über die API zu erhalten, Erstellen einer `FabricClient` und die HealthManager [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) -Methode aufrufen. Wenn Sie optionale Parameter angeben, verwenden Sie [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu den bereitgestellten Paket Dienststatus ist [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) . Um anzuzeigen, wo eine Anwendung bereitgestellt wird, führen Sie [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) und die bereitgestellte Anwendung betrachten. Die Service sehen Sie-Pakete in einer Anwendung sind bereitgestellte Dienst Paket Kinder in der Ausgabe von [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) .

Das folgende Cmdlet Ruft den Zustand des **WordCountServicePkg** Service-Pakets von der **Fabric: / WordCount** Anwendung auf **_Node_2**. Die Entität weist **System.Hosting** Berichte für erfolgreiche Service Einstiegspunkt Aktivierung und Registrierung Diensttyp.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Sie können bereitgestellte Paket Dienststatus mit [GET-Anforderung](https://msdn.microsoft.com/library/azure/dn707677.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/dn707689.aspx) mit Integritätsrichtlinien beschrieben im Text erhalten.

## <a name="health-chunk-queries"></a>Gesundheit Chunk Abfragen
Gesundheit Chunk Abfragen können mehrstufige Cluster Kinder (rekursiv) pro Eingabefilter zurückgeben. Erweiterte Filter, die Flexibilität, die bestimmte Kinder zurückgegeben werden, ermöglichen identifizierten eindeutigen Bezeichner, andere Gruppen-ID oder Zustand unterstützt. Standardmäßig sind keine untergeordneten Elemente enthalten Health-Befehle, die erste untergeordnete immer enthalten.

[Status Abfragen](service-fabric-view-entities-aggregated-health.md#health-queries) zurückgeben nur erste untergeordnete der angegebenen Entität pro erforderlichen Filter. Zu Kinder Kinder müssen Sie für jede Entität an gesundheitlichen APIs aufrufen. Zu den Zustand von bestimmten Entitäten müssen Sie auch eine Gesundheit API für jede gewünschte Entität aufrufen. Erweiterte Filterung Chunk-Abfrage können Sie mehrere Objekte in einer Abfrage minimiert die Größe und die Anzahl der Nachrichten anfordern.

Wert der Chunk-Abfrage ist, dass Sie Zustand für mehrere clusterentitäten (möglicherweise alle clusterentitäten erforderlich Stamm) aufrufen können. Sie können komplexe Gesundheit Abfrage wie Ausdrücken:

- Nur Fehler und Anwendungsbereiche Anwendungsbereiche alle Dienste Warnung zurück | Fehler. Zurückgegebene Dienste enthalten Sie alle Partitionen.

- Den Zustand des mit Namen angegebenen 4 Anwendung zurück.

- Nur den Zustand der Anwendung des gewünschten Anwendungstyp zurück.

- Alle bereitgestellte Entitäten auf einem Knoten zurück. Gibt alle Anträge auf den angegebenen Knoten und die bereitgestellten Servicepakete auf diesem Knoten bereitgestellt.

- Alle Replikate mit Fehler zurückgeben. Applikationen, Services, Partitionen und nur Replikate mit Fehler zurückgegeben.

- Alle zurück. Enthalten Sie für einen angegebenen Dienst alle Partitionen.

Abfrage Ausschnitt Status wird derzeit nur für die Cluster-Entität verfügbar gemacht. Es gibt einen Cluster Health Ausschnitt, enthält:

- Der Integritätsstatus Cluster zusammengefasst.

- Die Health Status Chunk Liste der Knoten, die Eingabefilter berücksichtigen.

- Der Health Status Chunk Liste der Programme, die Eingabefilter berücksichtigen. Jeder Block Anwendung Health Status enthält eine Liste Stück mit allen Diensten, die respektieren Eingabefilter und eine Liste Stück mit allen bereitgestellt, die Filter zu berücksichtigen. Für Kinder von Diensten und bereitgestellte Anwendung identisch. Auf diese Weise können alle Elemente im Cluster möglicherweise zurückgegeben werden hierarchisch angefordert.

### <a name="cluster-health-chunk-query"></a>Cluster Health Chunk Abfrage
Gibt den Zustand der Entität Cluster und hierarchische Health Status Abschnitte erforderlichen untergeordneten Elemente enthält. Eingabe:

- [Optional] Die Cluster-Integritätsrichtlinie Knoten und Clusterereignisse ausgewertet.

- [Optional] Die Anwendung Health Policy Karte mit Integritätsrichtlinien manifest Anwendungsrichtlinien außer Kraft gesetzt.

- [Optional] Filter für Knoten und Applikationen, die angeben, welche Einträge von Interesse sind und im Ergebnis zurückgegeben werden soll. Die Filter sind für eine Entität/Gruppe von Entitäten oder für alle Elemente auf dieser Ebene. Die Liste der Filter kann einen allgemeinen Filter oder Filter für bestimmte Bezeichner genaue von der Abfrage zurückgegebenen Entitäten enthalten. Wenn leer, werden die Kinder nicht standardmäßig zurückgegeben.
Weitere Informationen über Filter auf [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) und [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Die Anwendung Filter kann rekursiv Festlegen erweiterter Filter für Kinder

Chunk Ergebnis enthält untergeordnete Elemente, die Filter zu berücksichtigen.

Chunk-Abfrage gibt derzeit keine fehlerhaften Test- oder Entität Ereignisse zurück. Diese zusätzlichen Informationen kann mit bestehenden Cluster Health Abfrage abgerufen werden.

### <a name="api"></a>API
Zu Cluster Health Abschnitt Erstellen einer `FabricClient` und die **HealthManager** [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) -Methode aufrufen. Sie übergeben [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) Integritätsrichtlinien beschreiben und Filter.

Im folgenden Code wird die Cluster Health Chunk mit erweiterter Filter.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Das Cmdlet zu Cluster Health ist [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Schließen Sie zuerst den Cluster mithilfe des Cmdlets [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) .

Der folgende Code ruft Knoten nur dann Fehler außer einem bestimmten Knoten immer zurückgegeben werden soll.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Das folgende Cmdlet ruft Cluster Chunk mit Anwendungsfilter.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Das folgende Cmdlet gibt alle bereitgestellte Entitäten auf einem Knoten.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>REST
Sie können Cluster Health Chunk mit einer [GET-Anforderung](https://msdn.microsoft.com/library/azure/mt656722.aspx) oder einer [POST-Anforderung](https://msdn.microsoft.com/library/azure/mt656721.aspx) mit Integritätsrichtlinien und erweiterte Filter beschrieben im Text erhalten.

## <a name="general-queries"></a>Allgemeine Abfragen
Allgemeine Abfragen zurückgeben eine Liste von Service Fabric Entitäten eines bestimmten Typs. Sie sind über die API (über Methoden für **FabricClient.QueryManager**), PowerShell-Cmdlets und REST verfügbar. Diese Abfragen aggregieren Unterabfragen aus mehreren Komponenten. Davon ist der [Zustand gespeichert](service-fabric-health-introduction.md#health-store), das den zusammengesetzten Zustand für jedes Abfrageergebnis ausfüllt.  

> [AZURE.NOTE] Allgemeine Abfragen zusammengesetzten Zustand der Entität zurückgeben und keine umfangreiche Daten enthalten. Wenn eine Entität nicht fehlerfrei ist, können Sie sich mit Status Abfragen alle ihre Gesundheitsinformationen, einschließlich Ereignissen, untergeordnete Zustände und fehlerauswertungen zu folgen.

Allgemeine Abfragen einen unbekannten Zustand für eine Entität zurück, kann Speicher Gesundheit vollständige Daten über die Entität besitzt. Es ist auch möglich, eine Unterabfrage Health Speicher nicht erfolgreich war (z. B. ein Kommunikationsfehler aufgetreten oder Gesundheit Shop gedrosselt wurde). Verfolgen Sie mit einer Status-Abfrage für die Entität. Wenn die Unterabfrage vorübergehender Fehler wie Netzwerkprobleme auftreten kann Weiterverfolgung Abfrage erfolgreich. Sie können Ihnen auch Details aus dem Speicher Gesundheit zu Warum Entität nicht verfügbar gemacht wird.

Die Abfragen, die **HealthState** für Entitäten enthalten sind:

- Knotenliste: Gibt die Listenknoten im Cluster (ausgelagert).
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Liste: Gibt die Liste der Programme im Cluster (ausgelagert).
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Liste der Dienste: Gibt die Liste der Dienste in einer Anwendung (ausgelagert).
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Partitionsliste: Gibt die Liste der Partitionen in einem Dienst (ausgelagert).
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Replikatliste: Gibt die Liste der Replikate in einer Partition (ausgelagert).
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- Liste bereitgestellt: die Liste der bereitgestellten Programme auf einem Knoten zurückgegeben.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- Liste der Service-Paket bereitgestellt: Gibt die Liste der Service-Pakete in einer bereitgestellten Anwendung.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Einige Abfragen zurückgegeben ausgelagerter Ergebnisse. Diese Abfragen ist eine Liste von abgeleiteten [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Die Ergebnisse eine Nachricht passen nicht nur eine Seite zurückgegeben und ein ContinuationToken verfolgt, Enumeration wurde beendet. Sollte weiterhin die gleiche Abfrage und das Fortsetzungstoken aus der vorherigen Abfrage weiter Ergebnisse zu übergeben.

### <a name="examples"></a>Beispiele

Im folgenden Code wird die fehlerhafte Anwendung im Cluster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Das folgende Cmdlet Ruft die Anwendungsdetails für Fabric: / WordCount-Anwendung. Beachten Sie, dass Status Warnung.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Das folgende Cmdlet Ruft die Dienste mit einem Zustand der Warnung:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Cluster und Anwendung-upgrades
Während eines überwachten Upgrades des Clusters und die Anwendung überprüft Fabric Service Health alle fehlerfrei bleibt. Eine Entität gilt fehlerhaft konfigurierten Integritätsrichtlinien mit ausgewertet, das Upgrade Upgrade-spezifische Richtlinien, um die nächste Aktion festzulegen. Die Aktualisierung kann um Benutzer zulassen (wie Fehler beheben oder Ändern von Richtlinien) angehalten, oder es möglicherweise automatisch Rollback auf die vorherige, einwandfreie Version.

Während einer Aktualisierung *Cluster* erhalten Sie den Cluster-Aktualisierungsstatus. Der Aktualisierungsstatus enthält fehlerauswertungen zeigen, was im Cluster fehlerhaft ist. Wenn die Aktualisierung aus gesundheitlichen Gründen zurückgesetzt wird, speichert der Aktualisierungsstatus letzten fehlerhaften Gründe. Diese Informationen können Administratoren untersuchen, welche Fehler aufgetreten sind, nachdem die Aktualisierung zurückgesetzt oder beendet.

Auch während einer Aktualisierung *Anwendung* fehlerhaften Bewertungen den Aktualisierungsstatus Anwendung enthalten.

Im folgenden finden Sie den Aktualisierungsstatus Anwendung für geänderte Fabric: / WordCount-Anwendung. Watchdog meldete einen Fehler bei einer seiner Replikate. Die Aktualisierung rollt, da die Healthchecks nicht eingehalten werden.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Weitere Informationen über den [Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Verwenden Sie zur Problembehandlung Health Produkttests
Wenn ein Problem mit dem Cluster oder eine Anwendung ist betrachten Sie Cluster- oder Gesundheit zu ermitteln, was falsch ist. Fehlerhaften Bewertungen Einzelheiten über den aktuellen fehlerhaften Zustand Auslöser. Sie benötigen können Sie fehlerhafte untergeordnete Entitäten die Ursache ermitteln aufgliedern.

> [AZURE.NOTE] Fehlerhaften Bewertungen anzeigen, dass Erstens die Entität zum aktuellen Zustand ausgewertet wird. Möglicherweise mehrere Ereignisse, die diese auslösen, aber werden nicht in die Bewertung wiedergegeben werden. Weitere Informationen zu erhalten, Drilldown Health Entitäten, die fehlerhafte Berichte im Cluster herauszufinden.

## <a name="next-steps"></a>Nächste Schritte
[Mit Systemberichte zu Problembehandlung](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Fügen Sie benutzerdefinierter Berichte Service Fabric hinzu](service-fabric-report-health.md)

[Melden und Überprüfen des Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Überwachung und diagnose Dienste lokal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md)
