<properties
   pageTitle="Health monitoring Service Fabric | Microsoft Azure"
   description="Eine Einführung in Azure Service Fabric Health monitoring Modell ermöglicht die Überwachung des Clusters Programme und Dienste."
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

# <a name="introduction-to-service-fabric-health-monitoring"></a>Einführung in Service Fabric Überwachung
Azure Service Fabric stellt ein Zustandsmodell, das umfassende, flexible und erweiterbare Gesundheitszustands reporting und. Das ermöglicht nahe-Echtzeit-Überwachung des Status des Clusters und die Dienste ausgeführt. Einfach Gesundheitsinformationen abrufen und korrigieren Sie potenzielle Probleme, bevor sie überlappen und große Ausfällen. Im normalen Modell Cluster-Ebene Services Berichte basierend auf lokalen Ansichten senden und Informationen zum Bereitstellen einer aggregiert werden anzeigen.

Service Fabric-Komponenten verwenden diese umfangreiche Zustandsmodell ihren aktuellen Status gemeldet. Derselben Mechanismus können Bericht Gesundheit aus einer Anwendung heraus. Sie investieren Sie in qualitativ hochwertige Gesundheitsberichterstattung, die benutzerdefinierte Bedingung erfasst, erkennt und leichter beheben für Ihre Anwendung.

> [AZURE.NOTE] Wir beginnen das Gesundheit Subsystem überwachten Upgrades Notwendigkeit. Service Fabric bietet überwachten Anwendung und Cluster-Upgrades, die vollständige, ohne Ausfallzeiten Verfügbarkeit und minimale ohne Benutzereingriff. Um diese Ziele zu erreichen, überprüft die Aktualisierung basierend auf Gesundheit Upgraderichtlinien konfiguriert und eine Aktualisierung fortfahren nur bei Health gewünschten Grenzwerte berücksichtigt. Andernfalls die Aktualisierung automatisch ein Rollback oder angehalten, um Administratoren das Problem beheben kann. Weitere Informationen zu Upgrades der Anwendung finden Sie [in diesem Artikel](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Health-Speicher
Gesundheit Informationsspeicher enthält Informationen über Entitäten im Cluster für einfachen Abruf und Bewertung. Es implementiert wie Service Fabric statusbehaftete Dienst, um hohe Verfügbarkeit und Skalierung beibehalten. Health-Speicher ist Teil der **Fabric: / System** Anwendung und ist verfügbar, wenn der Cluster ist und läuft.

## <a name="health-entities-and-hierarchy"></a>Gesundheit Entitäten und Hierarchie
Gesundheit Entitäten sind in einer logischen Hierarchie organisiert, die Interaktionen und Abhängigkeiten zwischen verschiedenen Entitäten erfasst. Entitäten und Hierarchie werden automatisch vom Health Speicher basierend auf Berichte Service Fabric-Komponenten erstellt.

Gesundheit Elemente spiegeln Service Fabric-Entitäten. ( **Health Anwendung Entität** entspricht beispielsweise eine Instanz der Anwendung im Cluster bereitgestellt, während **Health knotenentität** einen Service Fabric-Clusterknoten entspricht.) Hierarchie Health erfasst Aktivitäten Systementitäten und bildet die Grundlage für erweiterte Gesundheitszustands. Erfahren Sie Service Fabric Grundbegriffe [Service Fabric – technische Übersicht](service-fabric-technical-overview.md). Weitere Anwendung finden Sie unter [Service Fabric-Anwendungsmodell](service-fabric-application-model.md).

Gesundheit Entitäten und Hierarchie können Cluster und Programme effektiv gemeldet, gedebuggt und überwacht werden. Das Zustandsmodell stellt, *präzise* Darstellung der Gesundheit viele bewegliche Teile im Cluster.

![Health-Entitäten.][1]
Gesundheit Entitäten organisiert zu über-und untergeordneten Elementen.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Die Gesundheit sind:

- **Cluster**. Stellt den Zustand eines Service Fabric-Clusters. Cluster Berichte beschreiben Vorschriften, die den gesamten Cluster und können nicht auf ein oder mehrere fehlerhafte Kinder eingegrenzt werden. Beispiele sind das Gehirn des Clusters aufgrund von Netzwerkproblemen partitionieren oder Kommunikation aufteilen.

- **Knoten**. Stellt den Zustand eines Knotens Service Fabric. Knoten Berichte beschreiben die Knoten Funktionen auswirken. Sie betreffen normalerweise alle bereitgestellten Elemente ausgeführt. Beispiele: Wenn ein Knoten aus Festplatte Speicherplatz (oder einem anderen computerweiten-Eigenschaft Arbeitsspeicher Verbindungen) und ein Knoten ausfällt. Knotenentität wird durch den Knotennamen (Zeichenfolge) identifiziert.

- **Anwendung**. Stellt den Zustand einer Instanz einer Anwendung im Cluster. Anwendungsberichte beschreiben den Gesamtzustand der Anwendung auswirken. Sie können nicht auf einzelne Kinder (Services bereitgestellte Anwendung) eingegrenzt werden. Die End-to-End-Interaktion zwischen verschiedenen Diensten gehören in der Anwendung. Die Entität Anwendung wird durch den Anwendungsnamen (URI) identifiziert.

- **Service**. Stellt den Zustand eines Dienstes im Cluster. Dienst Berichte beschreiben den Gesamtzustand des Diensts auswirken, und sie können nicht auf eine Partition oder ein Replikat eingegrenzt werden. Beispiele hierfür sind eine Dienstkonfiguration (z. B. Port oder externe Dateifreigabe), die Probleme für alle Partitionen ist. Dienst-Einheit wird durch den Namen (URI) identifiziert.

- **Partition**. Stellt den Zustand der Service-Partition. Partition Berichte beschreiben die gesamten Replikatgruppe auswirken. Beispiele sind die Anzahl der Replikate unter Zielanzahl und Quorum Verlust eine Partition ist. Der Partitions-ID (GUID) die Partition Entität identifizierte.

- **Replikat**. Stellt den Zustand der statusbehaftete dienstreplikat oder eine statusfreie Dienstinstanz. Die kleinste Wachhunde und Komponenten können für eine Anwendung melden. Statusbehaftete Dienste gehören ein primäres Replikat Wenn replizieren können keine Vorgänge sekundäre und nicht auf den erwarteten ist Tempo. Außerdem kann eine statusfreie Instanz melden, wenn es Ressourcen genügend oder Verbindungsprobleme. Die Replikat-Entität identifizierte Partitions-ID (GUID) und das Replikat oder Instanz-ID (lang).

- **DeployedApplication**. Stellt den Zustand einer *Anwendung auf einem Knoten ausgeführt*. Berichte bereitgestellte Anwendung beschreiben Vorschriften für die Anwendung auf dem Knoten, Service-Pakete auf dem gleichen Knoten bereitgestellt eingegrenzt werden kann nicht. Beispiele, wenn das Anwendungspaket auf diesem Knoten und ein Problem einrichten Sicherheitsprinzipale auf dem Knoten Anwendung heruntergeladen werden kann. Die bereitgestellte Anwendung identifizierte Anwendung (URI) und Knotennamen (Zeichenfolge).

- **DeployedServicePackage**. Stellt den Zustand eines Service-Pakets auf einem Knoten im Cluster ausgeführt. Er beschreibt Zustände für ein Service-Paket, die andere Servicepakete auf dem gleichen Knoten für dieselbe Anwendung nicht beeinträchtigen. Beispiele sind ein Paket in das Servicepaket, das nicht gestartet werden kann und ein Konfigurationspaket gelesen werden kann. Bereitgestellte Servicepaket identifizierte Anwendungsname (URI) Knotenname (Zeichenfolge) und manifest Dienstname (Zeichenfolge).

Die Granularität des Zustandsmodells erleichtert das Erkennen und Beheben von Problemen. Z. B. wenn ein Dienst nicht reagiert, ist es möglich, meldet, dass die Anwendungsinstanz fehlerhaft ist. Reporting auf ist Ebene nicht ideal, aber da das Problem nicht alle Dienste innerhalb der Anwendung auswirken kann. Der Bericht für den fehlerhaften Dienst oder eine bestimmte untergeordnete Partition anzuwenden weist Informationen für diese Partition. Die Daten automatisch Flächen durch die Hierarchie und einer fehlerhaften Partition sichtbar gemacht Dienst- und Ebenen. Diese Aggregation kann und die Ursache des Problems schnell beheben.

Die Health-Hierarchie besteht aus über-und untergeordneten Elementen. Ein Cluster besteht aus Knoten und Applikationen. Applications Services und Applikationen bereitgestellt. Bereitgestellte Anwendung haben Service-Pakete bereitgestellt. Dienste verfügen über Partitionen und einem oder mehreren Replikaten besitzt. Gibt es eine besondere Beziehung zwischen Knoten und bereitgestellt. Ist ein Knoten fehlerhaft gemeldet von der Behörde Systemkomponente (Failover-Manager-Dienst), betrifft das bereitgestellte Anwendung, Service-Pakete und Replikate auf bereitgestellt.

Health-Hierarchie stellt Stand des Systems basiert auf der neuesten Berichte, die Informationen nahezu in Echtzeit.
Interne und externe Überwachungen können auf die gleiche Basis anwendungsspezifische Logik oder benutzerdefinierten überwachten melden. Berichte mit dem System verwendet werden.

Planen Sie zu melden und Gesundheit beim Entwurf der großen Cloud-Dienst den Dienst einfacher debuggen, Überwachung und Betrieb reagieren.

## <a name="health-states"></a>Zustände
Service Fabric verwendet drei Zustände beschreiben, ob eine Entität fehlerfrei ist: OK, Warnung und Fehler. Jeder Bericht dem Zustand speichern muss diese Zustände angeben. Health-Bewertung ist eines dieser Staaten.

Die möglichen [Zustände](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) sind:

- **OK**. Die Entität ist fehlerfrei. Es gibt keine bekannten Probleme oder untergeordneten (falls zutreffend).

- **Warnung**. Die Entität Probleme auftritt, jedoch noch nicht fehlerhaft (z. B. verzögert, aber sie verursachen Funktionsprobleme noch). In einigen Fällen die Warnung kann ohne besondere Eingriff selbst beheben und nach Möglichkeit bieten Einblick in Was wird. In anderen Fällen kann die Warnung in ein schwerwiegendes Problem ohne Eingreifen des Benutzers beeinträchtigen.

- **Fehler**. Das Element ist fehlerhaft. Maßnahmen ergriffen werden, den Zustand der Entität zu beheben da funktionieren kann.

- **Unbekannt**. Die Entität ist nicht im Speicher Gesundheit vorhanden. Dadurch kann verteilten Abfragen gewonnen werden, die Ergebnisse aus mehreren Komponenten. Beispielsweise erhalten Sie Knoten Abfrage geht **FailoverManager** und **HealthManager**, Get Anwendung Abfrage **Abrufen** und **HealthManager**geht. Diese Abfragen zusammenführen Ergebnisse aus mehreren Komponenten. Eine andere Systemkomponente eine Entität zurückgibt, die nicht erreicht oder wurde aus dem Speicher Gesundheit bereinigt wurden, hat der zusammengefügten unbekannt Zustand.

## <a name="health-policies"></a>Integritätsrichtlinien
Gesundheit Speicher gilt Integritätsrichtlinien um festzustellen, ob eine Entität auf der Grundlage der Berichte und untergeordneten fehlerfrei ist.

> [AZURE.NOTE] Integritätsrichtlinien können im clustermanifest (für Cluster und Knoten Gesundheit) oder im Anwendungsmanifest (für Anwendung Bewertung und seine untergeordneten Elemente) angegeben werden. Gesundheit Auswertung Anfragen können auch benutzerdefinierte Auswertung Integritätsrichtlinien für diese Bewertung dient übergeben.

Standardmäßig wendet Service Fabric Regeln (alle fehlerfrei erforderlich) für die übergeordneten und untergeordneten hierarchischen Beziehung. Auch die Kinder eine fehlerhafte Ereignis ist, das übergeordnete Element fehlerhaft gilt.

### <a name="cluster-health-policy"></a>Cluster-Integritätsrichtlinie
Die [Cluster-Integritätsrichtlinie](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) dient Zustand Cluster und Knoten-Zustände. Die Richtlinie kann im clustermanifest definiert werden. Wenn es nicht vorhanden ist, wird die Standardrichtlinie (null zulässigen Fehler) verwendet.
Cluster-Integritätsrichtlinie enthält:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Gibt an, ob die Warnung Health behandelt Fehler während Gesundheitszustands meldet. Standardwert: false.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Gibt die zulässigen Höchstsatz Applikationen fehlerhaft sein kann, bevor der Cluster Fehler gilt.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Gibt den maximalen zulässigen Prozentsatz Knoten fehlerhaft sein kann, bevor der Cluster Fehler gilt. In großen Clustern stehen einige Knoten heruntergefahren oder, für Reparaturen, damit dieser Prozentsatz dulden konfiguriert werden soll.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Die Anwendung Typ Gesundheit Weltkarte Cluster Health Auswertung lässt sich spezielle Anwendung beschreiben. Standardmäßig werden alle in einem Pool und mit MaxPercentUnhealthyApplications ausgewertet. Einige Anwendungstypen behandelt werden soll, können sie aus der globalen Pool entnehmen. Stattdessen werden sie ihre Anwendung Typnamen in der Karte zugeordnete Prozentsätze ausgewertet. Z. B. in einem Cluster gibt es Tausende verschiedener Anwendungstypen und einige Steuerelement Anwendungsinstanzen eines speziellen Anwendung. Steuerelement-Applikationen sollte niemals Fehler. Sie können globale MaxPercentUnhealthyApplications von 20 % auf einige Fehler verkraftet jedoch den Anwendungstyp "ControlApplicationType" der MaxPercentUnhealthyApplications auf 0 festgelegt. Auf diese Weise würde sind einige der vielen fehlerhaften jedoch dem Anteil der globalen fehlerhafte Cluster Warnung ausgewertet werden. Ein Warnung Zustand wirkt sich nicht auf Cluster aktualisieren oder anderen Überwachung von Zustand Fehler ausgelöst. Aber auch eine Anwendung Fehler Cluster fehlerhaft, Rollback Trigger oder hält Aktualisierung Cluster aktualisieren abhängig.
Für die Anwendungstypen zuordnen werden alle Instanzen aus der globalen Applications. Sie werden anhand des insgesamt Applikationen Anwendungstyp mit bestimmten MaxPercentUnhealthyApplications aus der Zuordnung. Der Rest der Anwendung im globalen Pool bleiben und mit MaxPercentUnhealthyApplications ausgewertet.

Im folgende Beispiel ist ein Auszug aus einem clustermanifest. Einträge in der Anwendung geben Sie Map definieren, Präfix der Parametername mit"ApplicationTypeMaxPercentUnhealthyApplications", gefolgt von den Namen der Anwendung.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Anwendung Integritätsrichtlinie
Die [Anwendung Integritätsrichtlinie](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) beschrieben Vorgehensweise Bewertung und untergeordneten Status Aggregation für Applikationen und ihre Kinder. Sie können im Anwendungsmanifest **ApplicationManifest.xml**im Anwendungspaket definiert werden. Wenn keine Richtlinien angegeben sind, vorausgesetzt Service Fabric die Entität ist fehlerhaft, wenn es ein oder ein untergeordnetes Element am Zustand Warn- oder Fehlermeldung.
Die konfigurierbaren Richtlinien sind:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Gibt an, ob die Warnung Health behandelt Fehler während Gesundheitszustands meldet. Standardwert: false.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Gibt maximalen zulässigen Prozentsatz bereitgestellte Anwendung fehlerhaft sein kann, bevor die Anwendung Fehler gilt. Dieser Prozentsatz wird berechnet, indem die Anzahl der fehlerhaften bereitgestellte Anwendung über die Anzahl der Knoten, die die Anwendung derzeit auf im Cluster bereitgestellt werden. Die Berechnung Rundet einen Fehler auf wenigen Knoten tolerieren. Prozentsatz Standard: 0 (null).

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Gibt die Dienst Typ Health Standardrichtlinie, die Gesundheit Standardrichtlinie für alle Diensttypen in der Anwendung ersetzt.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Enthält eine Übersicht der Gesundheitspolitik pro Service Service. Diese Richtlinien ersetzen Standard Service Typ Integritätsrichtlinien für jeden angegebenen Dienst. Wenn eine Anwendung eine statusfreie Gateway Service und statusbehaftete Engine Service verfügt, können Sie Integritätsrichtlinien bewertet z. B. anders konfigurieren. Wenn Sie pro Dienst Richtlinie angeben, erhalten Sie eine genauere Kontrolle des Zustands des Dienstes.

### <a name="service-type-health-policy"></a>Service-Typ Integritätsrichtlinie
[Service-Typ Integritätsrichtlinie](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) gibt an, wie und Dienste sowie die untergeordneten Elemente des Services. Die Richtlinie enthält:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Gibt den maximalen zulässigen Prozentsatz fehlerhafte Partitionen, bevor Dienst als fehlerhaft angesehen wird. Prozentsatz Standard: 0 (null).

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Gibt zulässigen Höchstsatz fehlerhafte Replikate an, bevor eine Partition als fehlerhaft angesehen wird. Prozentsatz Standard: 0 (null).

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Gibt zulässigen Höchstsatz fehlerhafte Dienste vor als fehlerhaft betrachtet wird. Prozentsatz Standard: 0 (null).

Im folgende Beispiel ist ein Auszug aus einem Anwendungsmanifest:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Gesundheitszustands
Benutzer und automatisierte Dienste können jederzeit Zustand für eine Entität. Auswertung einer Entität Integrität Health Store Aggregate alle Berichte für die Entität und wertet alle untergeordneten (falls zutreffend). Die Gesundheit Aggregation Algorithmus Integritätsrichtlinien, die angeben, wie Berichte und wie untergeordnete Zustände (falls zutreffend).

### <a name="health-report-aggregation"></a>Gesundheit Berichtaggregation
Eine Entität kann mehrere Berichte per andere Reporter (Systemkomponenten oder Watchdogs) unterschiedliche Eigenschaften verfügen. Die Aggregation verwendet zugeordnete Integritätsrichtlinien insbesondere ConsiderWarningAsError Mitglied Anwendung oder Cluster Integritätsrichtlinie. ConsiderWarningAsError gibt an, wie Warnungen.

Die *schlimmsten* Berichte für die Entität der zusammengesetzten Zustand ausgelöst. Mindestens ein Fehler Integritätsbericht ist, der zusammengesetzten Zustand Fehler.

![Gesundheit Berichtaggregation mit Fehlermeldung.][2]

Eine Health Entität, ein oder mehrere Fehlerberichte wird als Fehler interpretiert. Dies gilt auch für einen Bericht Abgelaufene Zustand unabhängig von dessen Zustand.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Gibt keine Fehlerberichte und eine oder mehrere Warnungen, ist der zusammengesetzten Zustand Warnung oder Fehler je nach ConsiderWarningAsError Richtlinie Flag.

![Gesundheit Berichtaggregation Warnung und ConsiderWarningAsError False.][3]

Gesundheit Berichtaggregation Warnung und ConsiderWarningAsError auf False (Standard) festgelegt.

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Aggregation von untergeordneten Status
Zusammengesetzten Zustand einer Entität gibt untergeordnete Zustände (falls zutreffend). Der Algorithmus zum Aggregieren von untergeordneten Zustände verwendet Health anwendbaren Richtlinien basierend auf der Entität.

![Untergeordnete Entitäten Health Aggregation.][4]

Untergeordnete Aggregation basierend auf Richtlinien.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Nach Shop Zustand aller untergeordneten Elemente ausgewertet, aggregiert die Zustände basierend auf den konfigurierten maximalen Prozentsatz fehlerhafte Kinder. Dieser Prozentsatz wird basierend auf der Entität und untergeordneten Richtlinie entnommen.

- Haben alle untergeordneten Status OK ist untergeordneten aggregiert Zustand OK.

- Wenn Kinder OK und Status Warnung Warnung Zustand untergeordneten aggregiert.

- Kinder mit Fehler, die nicht die maximal zulässige Prozentsatz der fehlerhaften Kinder sind, ist der zusammengesetzten Zustand Fehler.

- Mit Fehlerstatus zulässigen Höchstprozentsatz fehlerhafte Kinder berücksichtigen, ist der aggregierten Integritätsstatus Warnung.

## <a name="health-reporting"></a>Gesundheitsberichterstattung
Komponenten, System Fabric Applikationen und interne und externe Überwachungen können gegen Service Fabric Unternehmen melden. Der Reporter Entscheidungen *lokalen* Zustand der überwachten Entitäten anhand der überwachten. Sie müssen die globalen Status oder aggregierte Daten betrachten. Das gewünschte Verhalten ist einfach Reporter und keine komplexe Organismen, die viele Dinge zu senden Informationen ableiten.

Zum Senden von Daten im Speicher Gesundheit muss Reporter identifizieren die betroffene Entität und ein. Der Bericht kann dann über die API mit [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx)über PowerShell oder Rest gesendet.

### <a name="health-reports"></a>Berichte
[Berichte](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) für die einzelnen Entitäten im Cluster enthalten die folgende Informationen:

- **SourceId**. Eine Zeichenfolge, die eindeutig das Systemereignis Reporter.

- **Instanzkennzeichner**. Identifiziert die Entität, auf der Bericht angewendet wird. Es unterscheidet sich abhängig von der [Entität geben](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Cluster. Keine.

  - Knoten. Knotenname (Zeichenfolge).

  - Anwendung. Anwendungsname (URI). Gibt den Namen der Instanz der Anwendung im Cluster bereitgestellt.

  - Dienst. Service Name (URI). Gibt den Namen der Dienstinstanz im Cluster bereitgestellt.

  - Partition. Partitions-ID (GUID). Partition Eindeutiger Bezeichner darstellt.

  - Replikat. Die statusbehaftete Replikat-ID oder die statusfreie Dienstinstanz-ID (INT64).

  - DeployedApplication. Anwendung (URI) und Knotennamen (Zeichenfolge).

  - DeployedServicePackage. Anwendungsname (URI) Knotenname (Zeichenfolge) und Service manifest Name (Zeichenfolge).

- **Eigenschaft**. Eine *Zeichenfolge* (keine festen Aufzählung), die das Ereignis für eine bestimmte Eigenschaft der Entität kategorisieren Reporter ermöglicht. Reporter eine meldet z. B. kann der Zustand der Eigenschaft Node01 "Speicher" und Reporter B den Zustand der Eigenschaft Node01 "Verbindung" melden kann. Diese Berichte werden im Speicher Gesundheit als separate Systemereignisse Node01 Entität.

- **Beschreibung**. Eine Zeichenfolge, die einen Reporter mit detaillierten Informationen über das Ereignis ermöglicht. **SourceId** **-Eigenschaft**und **HealthState** sollte den Bericht vollständig beschreiben. Die Beschreibung fügt lesbare Informationen zum Bericht hinzu. Text erleichtert den Administratoren und Benutzern, die Gesundheit Bericht.

- **HealthState**. Eine [Enumeration](service-fabric-health-introduction.md#health-states) , die den Zustand des Berichts beschreibt. Zulässige Werte sind OK, Warnung und Fehler.

- **TimeToLive**. Eine Zeitspanne, die angibt, wie lange der Integritätsbericht gültig ist. Verbindung mit **RemoveWhenExpired**können Health Speicher wie abgelaufene Ereignisse auswerten. Standardmäßig ist des Werts unendlich und Bericht ist immer gültig.

- **RemoveWhenExpired**. Ein boolescher Wert. Wenn auf True festgelegt, abgelaufene Integritätsbericht Health Store und der Bericht automatisch entfernt wird auswirken nicht Entität Gesundheit Auswertung. Wenn der Bericht für einen angegebenen Zeitraum nur gilt und Reporter nicht explizit löschen muss. Dient auch zum Löschen von Berichten aus dem Speicher Gesundheit (z. B. Watchdog geändert und Berichte mit vorherigen Datenquelle und beendet). Sie können einen Bericht mit kurzen TimeToLive mit RemoveWhenExpired deaktivieren Sie alle vorherigen Zustand aus dem Speicher Gesundheit senden. Wenn der Wert auf False festgelegt ist, werden abgelaufene Bericht als Fehler Health Bewertung. Der Wert false Health Store signalisiert, dass die Quelle für diese Eigenschaft regelmäßig Bericht erstatten. Wenn nicht, muss vorhanden sein etwas mit der Watchdog. Der Watchdog-Zustand wird unter Berücksichtigung des Ereignis als Fehler erfasst.

- **SequenceNumber**. Eine positive ganze Zahl, die immer, wird die Reihenfolge der Berichte. Es wird von Health Speicher zum veraltete Berichte erkennen, die später durch Netzwerkprobleme oder andere Probleme vorliegen. Ein Bericht wird abgelehnt, wenn die Sequenznummer ist kleiner oder gleich die optimale Anzahl für dieselbe Entität, Quelle und Eigenschaft zuletzt angewendet. Wenn nicht angegeben ist, wird die laufende Nummer automatisch generiert. Es ist notwendig, die Sequenznummer nur wenn Zustand melden. In diesem Fall muss die Quelle zu beachten die Berichte senden die Informationen für die Wiederherstellung bei einem Failover.

Diese vier Teile Informationen SourceId, Entity ID-Eigenschaft und HealthState – sind für jeden Integritätsbericht erforderlich. Zeichenfolge darf nicht mit SourceId meldet das Präfix "**System**" das System reserviert ist. Für dieselbe Entität ist nur ein Bericht für dieselbe Quelle und Eigenschaft. Mehrere Berichte für dieselbe Quelle und Eigenschaft gegenseitig überschreiben, Gesundheitsschutz Clientseite (Wenn sie zusammengefasst werden) oder auf die Gesundheit Seite speichern. Die Ersetzung basiert auf Sequenznummern. neuere Berichte (mit höheren Folgenummern) ersetzen ältere Berichte.

### <a name="health-events"></a>Systemereignisse
Intern hält Health Store [Systemereignisse](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), alle Informationen von Berichten und zusätzliche Metadaten enthalten. Die Metadaten enthalten dem Bericht Health Client angegeben wurde und die Zeit auf dem Server geändert wurde. Die Systemereignisse werden von [Status Abfragen](service-fabric-view-entities-aggregated-health.md#health-queries)zurückgegeben.

Die hinzugefügte Metadaten enthält:

- **SourceUtcTimestamp**. Die Zeit wurde Bericht Health Client (Coordinated Universal Time) angegeben.

- **LastModifiedUtcTimestamp**. Die Zeit der letzten Änderung des Berichts auf der Serverseite (Coordinated Universal Time).

- **IsExpired**. Ein Flag, das angibt, ob der Bericht beim Ausführen der Abfrage im Informationsspeicher Gesundheit abgelaufen war. Ein Ereignis kann nur, wenn RemoveWhenExpired false ist abgelaufen. Andernfalls wird das Ereignis wird von der Abfrage zurückgegeben und aus dem Speicher entfernt.

- **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Zeitpunkt des letzten Übergänge OK, Warnung, Fehler. Diese Felder geben den Verlauf der Gesundheit Zustandsübergänge für das Ereignis.

Übergang Statusfelder intelligenter Alarme und "Historisch" Systemereignisinformationen einsetzbar. Sie ermöglichen Szenarien wie:

- Warnung, wenn eine Eigenschaft zur Warnung/Fehler für mehr als X Minuten wurde. Überprüfen der Bedingung für einen Zeitraum vermeidet Alerts temporäre abhängig. Beispielsweise kann eine Warnung, wenn der Zustand länger als fünf Minuten Warnung wurde in übersetzt (HealthState == Warnung und -LastWarningTransitionTime > 5 Minuten).

- Warnung nur abhängig, die in den letzten geändert haben X Minuten. Ein Bericht auf Fehler vor dem angegebenen Zeitpunkt schon, kann ignoriert werden, da es zuvor bereits signalisiert wurde.

- Umschalten eine Eigenschaft zwischen Warn- und zu bestimmen, wie lange sie fehlerhafte wurde (d. h. nicht OK). Beispielsweise kann eine Warnung, wenn die Eigenschaft nicht mehr als fünf Minuten fehlerfrei wurde in übersetzt (HealthState! = Ok und -LastOkTransitionTime > 5 Minuten).

## <a name="example-report-and-evaluate-application-health"></a>Beispiel: Bericht und Auswerten des Anwendungszustands
Das folgende Beispiel sendet ein Integritätsbericht über PowerShell zur Anwendung **Fabric: / WordCount** von der Quelle **MyWatchdog**. Health-Bericht enthält Informationen über die Health-Eigenschaft einen Fehlerzustand mit unendlicher TimeToLive "Verfügbarkeit". Anschließend fragt Anwendungszustands gibt die Gesundheit gestellt und die gemeldeten Systemereignisse in der Liste der Systemereignisse aggregiert.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

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
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Gesundheit Modellverwendung
Das Zustandsmodell ermöglicht Cloud-Dienste und der zugrunde liegenden Plattform Service Fabric skalieren, da Überwachung und Health Bestimmung der verschiedenen Bildschirmen im Cluster verteilt.
Andere Systeme haben einen zentralen Dienst auf, die *potenziell* nützliche Informationen von Services ausgegeben analysiert. Dieser Ansatz verhindert die Skalierung. Keine können Informationen zur Identifizierung sowie potenzielle Probleme als Ursache möglichst nahe sammeln.

Das Zustandsmodell wird häufig für Diagnose und Überwachung, Auswertung Cluster und Anwendungszustand und überwachten Upgrades verwendet. Andere Dienste können automatische Reparaturen, Gesundheitsdaten Cluster erstellen und Ausgabe Alarmen unter bestimmten Umständen Daten zu.

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric Integritätsberichte anzeigen](service-fabric-view-entities-aggregated-health.md)

[Verwenden Sie Systemberichte für die Problembehandlung](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Melden und Überprüfen des Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Fügen Sie benutzerdefinierter Berichte Service Fabric hinzu](service-fabric-report-health.md)

[Überwachung und diagnose Dienste lokal](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md)
