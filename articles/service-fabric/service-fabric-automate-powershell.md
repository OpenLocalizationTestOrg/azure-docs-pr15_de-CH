<properties
    pageTitle="Fabric Service Application Management mithilfe von PowerShell Automatisierung | Microsoft Azure"
    description="Bereitstellen, aktualisieren, testen und Service Fabric-Anwendung mithilfe von PowerShell entfernen."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatisieren Sie mit PowerShell Anwendungslebenszyklus

Viele Aspekte des [Anwendungslebenszyklus Service Fabric](service-fabric-application-lifecycle.md) können automatisiert werden.  Dieser Artikel beschreibt wie Sie PowerShell Automatisierung häufiger Aufgaben zum Bereitstellen, aktualisieren, entfernen und Anwendungstests Azure Service Fabric.  Verwaltet und HTTP-APIs für die Verwaltung der Anwendung zur Verfügung stehen. [Lebenszyklus der Anwendung](service-fabric-application-lifecycle.md) Weitere Informationen anzeigen  

## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie die Aufgaben im Artikel verschieben, müssen Sie:

+ Beschriebenen [technischen](service-fabric-technical-overview.md)Überblick Service Fabric Service Fabric-Konzepten vertraut zu machen.
+ [Laufzeit, SDK und Tools installieren](service-fabric-get-started.md), das PowerShell-Modul **ServiceFabric installiert** .
+ [Aktivieren Sie PowerShell Skript ausführen](service-fabric-get-started.md#enable-powershell-script-execution).
+ Starten Sie einen lokalen Cluster.  Starten Sie neues PowerShell-Fenster als Administrator zu und führen Sie das Setupskript Cluster im SDK-Ordner:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Vor dem Ausführen von PowerShell-Befehlen in diesem Artikel zunächst eine Verbindung mit dem lokalen Cluster Service Fabric mit [**ServiceFabricCluster verbinden**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ Die folgenden Vorgänge erfordern eine v1-Anwendungspaket bereitstellen und ein Anwendungspaket v2 für die Aktualisierung. Downloaden Sie die [ **WordCount** -Beispiel-Anwendung](http://aka.ms/servicefabricsamples) (in den Beispielen Einstieg). Erstellen Sie und Verpacken Sie die Anwendung in Visual Studio (Rechtsklick auf **WordCount** im Projektmappen-Explorer und wählen Sie **Paket**). Kopieren Sie das Paket v1 in `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` , `C:\Temp\WordCount`. Copy `C:\Temp\WordCount` , `C:\Temp\WordCountV2`, v2-Anwendungspaket für Aktualisierung erstellen. Open `C:\Temp\WordCountV2\ApplicationManifest.xml` in einem Texteditor. Ändern Sie im **ApplicationManifest** Element das **ApplicationTypeVersion** -Attribut "1.0.0" "2.0.0" app Version aktualisieren. Speichern Sie die geänderte Datei ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Aufgabe: Eine Service Fabric-Anwendung bereitstellen

Nachdem Sie erstellt und verpackt die Anwendung (oder das Anwendungspaket heruntergeladen), können Sie die Anwendung in einem lokalen Service Fabric-Cluster bereitstellen. Die Bereitstellung umfasst Anwendungspaket hochladen, registrieren den Anwendungstyp und Instanz der Anwendung erstellen. Verwenden Sie die Anleitung in diesem Abschnitt eine neue Anwendung zu einem Cluster bereitgestellt.

### <a name="step-1-upload-the-application-package"></a>Schritt 1: Anwendungspaket hochladen
Der Abbildspeicher Anwendungspaket Hochladen wird an einem Ort für Service Fabric-Komponenten.  Anwendungspaket enthält die erforderlichen Konfigurationsschritte Anwendungsmanifest Service Manifeste und Code und Daten Pakete zum Erstellen der Anwendung und Instanzen.  Befehl [**Kopieren ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) lädt das Paket. Zum Beispiel:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Schritt 2: Registrieren Sie den Anwendungstyp
Registriert das Anwendungspaket stellt die Anwendung und Version zur Verwendung im Anwendungsmanifest verfügbar deklariert. Das System liest das Paket hochgeladen in Schritt 1, Überprüfen des Pakets (lokal ausgeführte [**Test-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) entspricht) verarbeitet den Paketinhalt und verarbeitete Paket an einen internen Speicherort kopieren.  [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) -Cmdlet ausführen:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Um alle registrierten im Cluster Anwendungstypen anzuzeigen, führen Sie das Cmdlet " [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) ":

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Schritt 3: Erstellen der Anwendungsinstanz
Eine Anwendung kann mit einer beliebigen Version der Anwendung-Typ, die erfolgreich registriert wurde, mit dem Befehl [**Neu ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) instanziiert werden. Der Name jeder Anwendung auf deklariert, Bereitstellung und mit beginnen die **Stoff:** System und für jede Anwendungsinstanz eindeutig sein. Anwendungsname Typ und Version der Anwendung Typ werden in der Datei **ApplicationManifest.xml** des Anwendungspakets deklariert. Wenn irgendwelche Standarddienste im Anwendungsmanifest des Zieltyps Anwendung definiert wurden, werden die zu diesem Zeitpunkt erstellt.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Der Befehl [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) Listet alle Instanzen, die erfolgreich, sowie deren Gesamtstatus erstellt wurden. Der Befehl [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) Listet alle Dienstinstanzen, die innerhalb einer Anwendung erstellt wurden. Standarddienste (falls vorhanden) aufgeführt.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Aufgabe: Aktualisieren einer Anwendung Fabric Service
Sie können eine zuvor bereitgestellte Service Fabric-Anwendung mit einer aktualisierten Anwendung aktualisieren. Diese Aufgabe aktualisiert WordCount-Anwendung, die in bereitgestellt "Aufgabe: Bereitstellen einer Anwendung Fabric Service." Weitere Informationen finden Sie über [Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md) .

Um Punkte für dieses Beispiel einfach zu halten, wurde die Versionsnummer im Anwendungspaket WordCountV2 erstellt die erforderlichen Komponenten aktualisiert. Ein realistischeres Szenario wäre die Service Code, Konfiguration oder die Datendateien aktualisieren neu und Packen der Anwendung mit einer aktualisierten Versionsnummer.  

### <a name="step-1-upload-the-updated-application-package"></a>Schritt 1: Laden Sie das Paket aktualisierten Anwendung
WordCount v1 Application kann aktualisiert werden. Öffnen Sie ein PowerShell-Fenster Administrator und [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)angezeigt, dass diese Version 1.0.0 Anwendungstyp WordCount bereitgestellt wird.  

Kopieren Sie jetzt aktualisierte Anwendungspaket in Service Fabric-Abbildspeicher (wo der Anwendungspakete von Service Fabric gespeichert sind). Der Parameter **ApplicationPackagePathInImageStore** informiert Service Fabric es Anwendungspaket finden. Der folgende Befehl kopiert das Anwendungspaket auf **WordCountV2** im Speicher Bild:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Schritt 2: Registrieren Sie den aktualisierten Anwendungstyp
Der nächste Schritt ist die neue Version der Anwendung mit Service erfassen mit [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) -Cmdlet ausgeführt werden können:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Schritt 3: Starten der Aktualisierung
Um Anwendungsupgrades können verschiedene Upgrade-Parameter Timeouts und Kriterien angewendet werden. Lesen Sie weitere Dokumente [Anwendung Parameter](service-fabric-application-upgrade-parameters.md) und [Aktualisieren Process](service-fabric-application-upgrade.md) . Alle Dienste und Instanzen sollten nach der Aktualisierung _gesund_ sein.  (So dass Dienste fehlerfrei für mindestens 20 Sekunden vor die Aktualisierung der nächsten Aktualisierungsdomäne) **HealthCheckStableDuration** auf 60 Sekunden gesetzt.  **UpgradeDomainTimeout** auch auf 1.200 Sekunden und **UpgradeTimeout** 3000 Sekunden festlegen. Legen Sie abschließend die **UpgradeFailureAction** **Rollback**, die verlangt, dass Fabric Service die Anwendung auf die vorherige Version Rollback Fehler während der Aktualisierung aufgetreten sind.

Nun können Sie die Aktualisierung der Anwendung mit dem [**Start-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) -Cmdlet:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Beachten Sie, dass der Anwendungsname der zuvor bereitgestellte v1.0.0 Anwendung identisch (Fabric: / WordCount). Service Fabric verwendet diesen Namen zum Identifizieren die Anwendung aktualisiert abrufen. Festlegen von Timeouts zu kurz, treffen Sie eine Timeout-Fehlermeldung, die das Problem angibt. Finden Sie unter [Problembehandlung bei Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md)oder erhöhen Sie die Timeouts.

### <a name="step-4-check-upgrade-progress"></a>Schritt 4: Überprüfen des Aktualisierungsvorgangs
Anwendung des Aktualisierungsvorgangs können Sie mithilfe des [Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)oder mit dem Cmdlet [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) überwachen:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

In wenigen Minuten das Cmdlet " [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) " zeigt an, dass alle Domänen Upgrade aktualisiert wurden (abgeschlossen).

## <a name="task-test-a-service-fabric-application"></a>Aufgabe: Eine Service Fabric-Anwendung testen

Entwickler müssen zum Schreiben von qualitativ hochwertigen Dienstleistungen unzuverlässige Infrastruktur Fehler die Stabilität ihrer Dienste testen auslösen können. Service Fabric können Entwickler Fehler Aktionen und Services von Fehlern mithilfe von Testszenarien Chaos und Failover testen.  Lesen Sie [Übersicht über die Prüfbarkeit](service-fabric-testability-overview.md) Informationen.

### <a name="step-1-run-the-chaos-test-scenario"></a>Schritt 1: Ausführen des Testszenarios chaos
Testszenario Chaos generiert Fehler im gesamten Service Fabric-Cluster. Das Szenario komprimiert Fehler im Allgemeinen über Monate oder Jahre auf wenige Stunden. Aus überlappenden Fehler mit hoher Fehler findet Ausnahmefällen andernfalls übersehen würden. Im folgenden Beispiel wird das Testszenario Chaos 60 Minuten.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Schritt 2: Ausführen des Testszenarios failover
Testen Sie das Failover Szenario Ziele einer bestimmten Servicepartition statt des gesamten Clusters, und er bewirkt, dass andere Dienste nicht betroffen. Das Szenario durchläuft eine Sequenz von simulierten Fehler bei der Überprüfung der Service während Ihrer Geschäftslogik ausgeführt wird. Fehler bei der Überprüfung der Service gibt eine Frage, die weitere Untersuchung. Failover-Test führt nur einen Fehler zu einem Zeitpunkt das Testszenario Chaos, das mehrere Fehler auslösen kann. Im folgenden Beispiel wird die Failovertestes 60 Minuten gegen den Stoff: WordCount/WordCountService-Dienst.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Aufgabe: Entfernen einer Anwendung Fabric Service
Sie können eine Instanz einer bereitgestellten Anwendung löschen bereitgestellte Anwendungstyp aus dem Cluster entfernen und das Anwendungspaket aus der ImageStore entfernen.

### <a name="step-1-remove-an-application-instance"></a>Schritt 1: Entfernen einer Anwendungsinstanz
Wenn eine Instanz der Anwendung nicht mehr benötigt wird, können Sie es löschen mithilfe des Cmdlets [**Entfernen ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) . Dies entfernt automatisch alle Dienste zur Anwendung dauerhaft entfernt alle Dienststatus. Dieser Vorgang kann nicht rückgängig gemacht und Anwendungsstatus nicht wiederhergestellt werden.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Schritt 2: Aufheben der Registrierung des Anwendungstyps
Wenn Sie eine bestimmte Version eines Anwendungstyps nicht mehr benötigen, Ihre Registrierung aufheben Sie mit [**Registrierung-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) -Cmdlet. Aufheben der Registrierung von nicht verwendeten Typen frei Speicherplatz in der Abbildspeicher Anwendungspaket. Ein Anwendungstyp kann aufgehoben werden, solange keine Programme gegen oder Anwendung Upgrades verweisen instanziiert.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Schritt 3: Entfernen des Anwendungspakets
Nach Anwendungstyp aufgehoben wird, kann das Anwendungspaket mithilfe des Cmdlets [**Entfernen ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) vom Abbildspeicher entfernt.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric-Anwendungslebenszyklus](service-fabric-application-lifecycle.md)

[Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md)

[Anwendung aktualisieren](service-fabric-application-upgrade.md)

[Azure Service Fabric-cmdlets](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure Service Fabric Prüfbarkeit cmdlets](https://msdn.microsoft.com/library/azure/mt125844.aspx)
