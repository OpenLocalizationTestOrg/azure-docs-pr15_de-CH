
<properties
   pageTitle="Anwendung aktualisieren: Parameter aktualisieren | Microsoft Azure"
   description="Beschreibt Parameter Aktualisieren einer Anwendung Service Fabric, einschließlich Healthchecks ausführen und Richtlinien für die Aktualisierung automatisch rückgängig zu machen."
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



# <a name="application-upgrade-parameters"></a>Upgrade Anwendungsparameter

Dieser Artikel beschreibt die verschiedenen Parameter, die während der Aktualisierung einer Azure Service Fabric-Anwendung gelten. Die Parameter umfassen den Namen und die Version der Anwendung. Griffe, die steuern, Timeouts und Gesundheitskontrolle, die bei der Aktualisierung sind, und geben sie die Richtlinien, die angewendet werden müssen, wenn ein Upgrade fehlschlägt.


<br>

| Parameter | Beschreibung |
| --- | --- |
| Anwendungsname | Name der Anwendung, die aktualisiert wird. Beispiele: Fabric: / VisualObjects, Fabric: / ClusterMonitor  |
| TargetApplicationTypeVersion | Geben Sie die Version der Anwendung, die die Aktualisierung Ziele. |
| FailureAction | Die Aktion Service Fabric Wenn die Aktualisierung fehlschlägt. Anwendung kann ein Rollback auf die Version vor der Aktualisierung (Rollback) oder die Aktualisierung an der aktuellen Domäne aktualisieren beendet werden kann. In diesem Fall wird der Aktualisierungsmodus auch manuell geändert. Zulässige Werte sind Rollback und manuell. |
| HealthCheckWaitDurationSec | Die Wartezeit (in Sekunden) nach Abschluss des Upgrades auf die Upgrade-Domäne vor Service Fabric wertet den Zustand der Anwendung. Diese Dauer kann die Zeit berücksichtigt werden, die eine Anwendung ausgeführt werden soll, bevor es fehlerfrei angesehen werden kann. Wenn die Gesundheitskontrolle besteht fährt der Upgrade-Vorgang mit der nächsten Aktualisierungsdomäne.  Schlägt die Überprüfung auf Fehlerfreiheit wartet Service Fabric Intervall (UpgradeHealthCheckInterval) Health Check erneut gesendet, bis das HealthCheckRetryTimeout erreicht ist. Der Standardwert und der empfohlene Wert ist 0 Sekunden. |
| HealthCheckRetryTimeoutSec | Die Dauer (in Sekunden) weiterhin, Fabric Service Health Bewertung vor die Aktualisierung als fehlerhaft deklariert. Der Standardwert ist 600 Sekunden. Diese Dauer startet nach HealthCheckWaitDuration erreicht ist. Innerhalb dieses HealthCheckRetryTimeout können Service Fabric mehrere Gesundheitskontrolle der Gesundheit Anwendung ausführen. Der Standardwert beträgt 10 Minuten und für die Anwendung entsprechend angepasst werden sollte. |
| HealthCheckStableDurationSec | Die Dauer (in Sekunden) sicher, dass die Anwendung vor der nächsten Aktualisierungsdomäne verschieben oder die Aktualisierung stabil ist. Diese Wartedauer wird zum verhindern unentdeckt des richtigen Zustands nach Health Check ausgeführt wird. Der Standardwert beträgt 120 Sekunden und für die Anwendung entsprechend angepasst werden sollte. |
| UpgradeDomainTimeoutSec | Maximale Zeit (in Sekunden) für eine einzelne Domäne aktualisieren aktualisieren. Wenn dieses Zeitlimit erreicht ist, wird die Aktualisierung beendet und wird basierend auf der Einstellung für UpgradeFailureAction. Der Standardwert ist nie (unendlich) und für die Anwendung entsprechend angepasst werden sollte. |
| UpgradeTimeout | Ein Timeout (in Sekunden), die für die gesamte Aktualisierung gilt. Wenn dieses Zeitlimit erreicht ist, die Aktualisierung beendet und UpgradeFailureAction ausgelöst. Der Standardwert ist nie (unendlich) und für die Anwendung entsprechend angepasst werden sollte. |
| UpgradeHealthCheckInterval | Die Häufigkeit, der Status aktiviert ist. Dieser Parameter wird im Abschnitt Abrufen von _Cluster_ - _manifest_angegeben und nicht als Teil des Upgrade Cmdlets angegeben. Der Standardwert ist 60 Sekunden.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Service Health Bewertung Anwendung aktualisieren

<br>
Gesundheitskriterien sind optional. Bewertungskriterien Zustand beim Starten einer Aktualisierung nicht angegeben sind, verwendet Fabric Service die Anwendung Integritätsrichtlinien ApplicationManifest.xml der Anwendungsinstanz angegeben.


<br>

| Parameter | Beschreibung |
| --- | --- |
| ConsiderWarningAsError | Standardwert ist False. Behandeln Sie die Warnung Systemereignisse für die Anwendung bei den Zustand der Anwendung während der Aktualisierung als Fehler. In der Standardeinstellung wertet Service Fabric nicht Warnung Systemereignisse zu Fehler (Fehler), damit die Aktualisierung fortgesetzt werden kann, auch wenn Warnungsereignisse.   |
| MaxPercentUnhealthyDeployedApplications | Standardwert und der empfohlene Wert ist 0. Geben Sie die maximale Anzahl der bereitgestellten Anwendung (siehe [Gesundheit](service-fabric-health-introduction.md)), die fehlerhaft sein kann, bevor die Anwendung als fehlerhaft angesehen und schlägt die Aktualisierung fehl. Dieser Parameter definiert die Anwendungszustands auf dem Knoten, und hilft bei der Ermittlung Probleme während der Aktualisierung. Normalerweise erhalten die Replikate der Anwendung mit Lastenausgleich auf den anderen Knoten, der die Anwendung fehlerfrei, damit die Aktualisierung fortgesetzt werden kann. Durch Angabe einer strengen MaxPercentUnhealthyDeployedApplications Gesundheits, können Service Fabric ein Problem mit der Anwendung schnell erkennen und eine schnelle Aktualisierung nicht erzeugt. |
| MaxPercentUnhealthyServices | Standardwert und der empfohlene Wert ist 0. Geben Sie die maximale Anzahl der Dienste in der Anwendungsinstanz fehlerhaft sein kann, bevor die Anwendung als fehlerhaft angesehen und schlägt die Aktualisierung fehl. |
| MaxPercentUnhealthyPartitionsPerService | Standardwert und der empfohlene Wert ist 0. Geben Sie die maximale Anzahl der Partitionen in einem Dienst fehlerhaft sein kann, bevor der Dienst als fehlerhaft angesehen wird. |
| MaxPercentUnhealthyReplicasPerPartition | Standardwert und der empfohlene Wert ist 0. Geben Sie die maximale Anzahl von Replikaten Partition fehlerhaft sein kann, bevor die Partition als fehlerhaft angesehen wird. |
| UpgradeReplicaSetCheckTimeout | **Statusfreie Service**- versucht innerhalb einer Einzeldomäne Upgrade Service Fabric weitere Instanzen des Dienstes zur Verfügung stehen. Mehr als eine Instanz verfügbar, bis eine maximale Timeoutwert ist die Instanzenzahl Ziel mehrere wartet Fabric Service. Dieses Timeout wird mithilfe der UpgradeReplicaSetCheckTimeout-Eigenschaft angegeben. Wenn das Timeout abläuft, wird die Aktualisierung unabhängig von der Anzahl der Dienstinstanzen Service Fabric fortgesetzt. Wird die Anzahl der Instanzen Ziel, Fabric Service wird und sofort mit der Aktualisierung ausgeführt wird. **Statusbehaftete Dienst**- versucht innerhalb einer Einzeldomäne Upgrade Service Fabric hat die Replikatgruppe ein Quorum. Fabric Service wartet ein Quorum, bis eine maximale Timeoutwert (angegeben durch die UpgradeReplicaSetCheckTimeout-Eigenschaft) verfügbar. Wenn das Timeout abläuft, wird die Aktualisierung unabhängig von Quorum Service Fabric fortgesetzt. Diese Einstellung ist wie nie (unendlich) beim Weiterleiten und 900 Sekunden beim Zurücksetzen. |
| ForceRestart | Wenn Sie Konfigurations- oder Datenpaket aktualisieren, ohne den Code aktualisieren, der Dienst neu gestartet wird, wenn die ForceRestart-Eigenschaft festgelegt auf True. Wenn die Aktualisierung abgeschlossen ist, benachrichtigt Service Fabric dem Dienst, dass eine neue Konfiguration oder Datenpaket verfügbar ist. Der Dienst ist zuständig für die Übernahme der Änderung. Bei Bedarf kann der Dienst neu gestartet. |



<br>
<br>
MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService und MaxPercentUnhealthyReplicasPerPartition Kriterien können pro Diensttyp für eine Instanz der Anwendung angegeben werden. Eine Anwendung für verschiedene Diensttypen mit unterschiedlichen Bewertung ermöglicht dieser Parameter pro Dienst festlegen. Beispielsweise kann ein statusfreie Gateway Diensttyp ein MaxPercentUnhealthyPartitionsPerService haben eine statusbehaftete Engine Diensttyp für eine bestimmte Anwendungsinstanz unterscheidet.

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Sie die Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch ein Upgrade der Anwendung mit Visual Studio.

[Aktualisieren Sie die Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine anwendungsaktualisierung mit PowerShell.

Machen Sie die Upgrades der Anwendung zu [Daten](service-fabric-application-upgrade-data-serialization.md)Serialisierung kompatibel.

Informationen Sie zum erweiterten Funktionen beim Upgrade Ihrer Anwendung auf [Weiterführende Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von Problemen in Anwendungsupgrades auf die Schritte [Zur Problembehandlung Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
 
