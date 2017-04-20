<properties
   pageTitle="Service Fabric-App-Upgrade mit PowerShell | Microsoft Azure"
   description="Dieser Artikel führt die Erfahrung der Service Fabric-Anwendung bereitstellen, den Code ändern und eine Aktualisierung mit PowerShell Rollout."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Fabric Service Application Upgrade mit PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Die am häufigsten verwendeten und wird der überwachten Aktualisierung Upgrade empfohlen.  Azure Service Fabric überwacht die Integrität der Anwendung aktualisiert basierend auf Richtlinien. Nach der Aktualisierung einer Domäne aktualisieren (UD) Service Fabric wertet die Anwendungsintegrität und fährt mit der nächsten Domäne aktualisieren oder je nach den Richtlinien die Aktualisierung fehlschlägt.

Überwachte anwendungsaktualisierung erfolgt mithilfe der verwalteten oder systemeigenen APIs, PowerShell und REST. Informationen eine Aktualisierung mit Visual Studio finden Sie unter [Aktualisieren der Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md).

Mit Service Fabric überwacht parallele Updates kann Anwendungsadministrator Bewertung Integritätsrichtlinie konfigurieren, Service Fabric zu bestimmen, ob die Anwendung fehlerfrei ist. Darüber hinaus kann der Administrator konfigurieren die auszuführende Aktion fällt die Gesundheit Auswertung (beispielsweise einen automatischen Rollback ausführen.) Dieser Abschnitt erläutert schrittweise überwachten Aktualisierung eines SDK-Beispiele, die PowerShell verwendet.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Schritt 1: Erstellen und Bereitstellen von visuellen Objekten-Beispiel


Erstellen Sie und veröffentlichen Sie die Anwendung mit der rechten Maustaste auf das Anwendungsprojekt, **VisualObjectsApplication,** Befehl **Veröffentlichen** .  Weitere Informationen finden Sie im [Lernprogramm upgrade Service Fabric-Anwendung](service-fabric-application-upgrade-tutorial.md).  Alternativ können Sie PowerShell zum Bereitstellen der Anwendung.

> [AZURE.NOTE] Bevor Service Fabric-Befehle in PowerShell verwendet werden kann, müssen Sie zunächst mit dem Cluster Herstellen der `Connect-ServiceFabricCluster` Cmdlet. Ebenso wird davon ausgegangen, dass der Cluster bereits auf dem lokalen Computer eingerichtet wurde. Finden Sie im Artikel zum [Einrichten der Umgebung Service Fabric](service-fabric-get-started.md).

Nach dem Erstellen des Projekts in Visual Studio können Sie PowerShell-Befehl **Copy ServiceFabricApplicationPackage** Anwendungspaket in die ImageStore kopiert. Im nächste Schritt ist die Anwendung mit dem **Journal-ServiceFabricApplicationPackage** -Cmdlet Service Fabric-Runtime registrieren. Der letzte Schritt ist eine Instanz der Anwendung mithilfe des Cmdlets **ServiceFabricApplication neu** starten.  Diese drei Schritte entsprechen in Visual Studio mit Menüelement **Bereitstellen** .

Jetzt können Sie [Service Fabric-Explorer zum Anzeigen des Clusters und der Anwendung](service-fabric-visualizing-your-cluster.md). Die Anwendung hat einen Webdienst in Internet Explorer navigiert werden kann [http://localhost: 8081/Visualobjects](http://localhost:8081/visualobjects) in die Adressleiste eingeben.  Einige schwebende visuelle Objekte im Fenster sollte angezeigt werden.  **Get-ServiceFabricApplication** können Sie außerdem um den Anwendungsstatus zu überprüfen.

## <a name="step-2-update-the-visual-objects-sample"></a>Schritt 2: Beispiel visuelle Objekte aktualisieren

Sie können feststellen, dass mit der Version, die in Schritt 1 bereitgestellt wurde, die visuellen Objekte nicht drehen. Wir aktualisieren diese Anwendung zu, in dem die visuellen Objekte auch drehen.

Wählen Sie das VisualObjects.ActorService Projekt innerhalb der Projektmappe VisualObjects, und öffnen Sie die Datei StatefulVisualObjectActor.cs. Navigieren Sie in der Datei an die Methode `MoveObject`, kommentieren Sie `this.State.Move()`, und `this.State.Move(true)`. Diese Änderung dreht die Objekte nach der Aktualisierung des Dienstes.

Wir müssen die Datei *ServiceManifest.xml* (unter "PackageRoot") des Projekts **VisualObjects.ActorService**aktualisieren. Aktualisieren Sie *CodePackage* und Service-Version 2.0 und die entsprechenden Zeilen in der Datei *ServiceManifest.xml* .
Nachdem Sie die Lösung für die Manifestdatei ändern klicken, können Sie die Option Visual Studio *Manifest-Dateien bearbeiten* .


Nachdem die Änderungen sollte das Manifest wie folgt aussehen (markierten Bereiche zeigen die Änderung):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Jetzt wird die *ApplicationManifest.xml* -Datei ( **VisualObjects** -Projekt unter **VisualObjects** Lösung) auf Version 2.0 des Projekts **VisualObjects.ActorService** aktualisiert. Darüber hinaus wird die Version der Anwendung von 1.0.0.0 auf 2.0.0.0 aktualisiert. *ApplicationManifest.xml* sieht wie im folgenden Codeausschnitt:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Erstellen Sie das Projekt jetzt markieren nur das **ActorService** -Projekt mit der rechten Maustaste und dann die Option **Erstellen** in Visual Studio. Wenn Sie **Alles neu erstellen**auswählen, sollten Sie Versionen für alle Projekte aktualisieren, da Code geändert hätte. Weiter wir ***VisualObjectsApplication***Rechtsklick und Auswahl im Fabric Service **Paket**Packen die aktualisierte Anwendung. Diese Aktion erstellt ein Anwendungspaket bereitgestellt werden kann.  Die aktualisierte Anwendung kann bereitgestellt werden.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Schritt 3: Legen Sie Richtlinien und aktualisieren Parameter

Machen Sie sich mit der [Anwendung Parameter aktualisieren](service-fabric-application-upgrade-parameters.md) und die [Aktualisierung](service-fabric-application-upgrade.md) auf ein gutes Verständnis der verschiedenen Upgrade-Parameter, Timeouts und Gesundheit Kriterium angewendet. Für diese exemplarische Vorgehensweise Service Health Bewertungskriterium auf den Standardwert festgelegt (und empfohlene) Werte bedeutet, dass alle Dienste und Instanzen _fehlerfrei_ nach der Aktualisierung.  

Jedoch erhöhen wir die *HealthCheckStableDuration* auf 60 Sekunden (so dass Dienste fehlerfrei für mindestens 20 Sekunden vor die Aktualisierung der nächsten Domäne aktualisieren).  Außerdem definieren *UpgradeDomainTimeout* 1200 Sekunden und *UpgradeTimeout* 3000 Sekunden.

Abschließend sehen wir auch *UpgradeFailureAction* auf Rollback festgelegt. Diese Option erfordert Service Fabric Rollback auf die vorherige Version die Anwendung, falls während der Aktualisierung Probleme auftreten. Daher werden beim Starten der Aktualisierung (in Schritt 4) die folgenden Parameter angegeben:

FailureAction = Rollback

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Schritt 4: Anwendung für die Aktualisierung vorbereiten

Die Anwendung ist nun erstellt und aktualisiert werden. Wenn Sie als Administrator an, und geben **Sie ServiceFabricApplication**ein PowerShell-Fenster öffnen, sollten sie Sie wissen, dass diese Anwendung 1.0.0.0 **VisualObjects** ist, das bereitgestellt wurde.  

Das Anwendungspaket ist unter den folgenden relativen Pfad gespeichert, in dem Sie den Service Fabric-SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*nicht komprimiert. Einen "Paket" Ordner finden in diesem Verzeichnis Sie Anwendungspaket Speicherort. Überprüfen Sie die Timestamps um sicherzustellen, dass sie die neueste (möglicherweise müssen die Pfade entsprechend auch ändern).

Jetzt kopieren wir aktualisierte Anwendungspaket in den Service Fabric-ImageStore (die Anwendungspakete Speicherorte von Service Fabric). Der Parameter *ApplicationPackagePathInImageStore* informiert Service Fabric es Anwendungspaket finden. Wir haben die aktualisierte Anwendung "VisualObjects\_V2" mit dem folgenden Befehl (Sie müssen Pfade erneut ändern).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Im nächste Schritt wird dieser Anwendung erfassen mit Service mit dem folgenden Befehl ausgeführt werden kann:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Befehl nicht erfolgreich, ist es wahrscheinlich, dass Sie alle Dienste neu zu erstellen. Wie in Schritt2, müssen Sie Ihre WebService-Version aktualisieren.

## <a name="step-5-start-the-application-upgrade"></a>Schritt 5: Starten Sie die Aktualisierung der Anwendung

Jetzt sind wir bereit, die Aktualisierung der Anwendung mit dem folgenden Befehl starten:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Der Anwendungsname ist dasselbe wie in der Datei *ApplicationManifest.xml* beschrieben wurde. Service Fabric verwendet diesen Namen zum Identifizieren die Anwendung aktualisiert abrufen. Festlegen von Timeouts zu kurz können eine Fehlermeldung auftreten, die das Problem angibt. Siehe Abschnitt zur Problembehandlung oder erhöhen Sie die Timeouts.

Als Erlöse Upgrade Anwendung können Service Fabric-Explorer überwachen oder mit dem folgenden PowerShell Befehl: **Get-ServiceFabricApplicationUpgrade Fabric: / VisualObjects**.

In wenigen Minuten der Status mit dem vorherigen Befehl PowerShell hat anzugeben, dass alle aktualisierungsdomänen aktualisiert (abgeschlossen). Und finden Sie die visuellen Objekte in Ihrem Browserfenster gestartet haben drehen.

Aktualisierung von Version 2 3 oder von Version 2 1 Übung probieren. Verschieben von Version 2 zu Version 1 Aktualisierung gilt. Experimentieren Sie mit Timeouts und Integritätsrichtlinien sich vertraut machen. Beim Bereitstellen von Azure Cluster müssen die Parameter entsprechend eingestellt werden. Es empfiehlt sich, die Timeouts konservativ festlegen.


## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Sie die Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch ein Upgrade der Anwendung mit Visual Studio.

Steuern Sie, wie Ihre Anwendung aktualisiert [Aktualisieren Parameter](service-fabric-application-upgrade-parameters.md).

Machen Sie die Upgrades der Anwendung zu [Daten](service-fabric-application-upgrade-data-serialization.md)Serialisierung kompatibel.

Informationen Sie zum erweiterten Funktionen beim Upgrade Ihrer Anwendung auf [Weiterführende Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von Problemen in Anwendungsupgrades auf [Anwendungsupgrades Problembehandlung](service-fabric-application-upgrade-troubleshooting.md)die Schritte.
