<properties
   pageTitle="Problembehandlung bei der lokalen Service Fabric-Cluster-Setup | Microsoft Azure"
   description="Dieser Artikel behandelt eine Reihe von Vorschlägen zur Problembehandlung Cluster lokale Entwicklung"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Problembehandlung bei der Clusterinstallation Entwicklung

Wenn Sie ein Problem bei der Interaktion mit lokalen Azure Service Fabric Entwicklung Cluster ausführen, überprüfen Sie die folgenden Lösungen.

## <a name="cluster-setup-failures"></a>Setupfehler Cluster

### <a name="cannot-clean-up-service-fabric-logs"></a>Service Fabric-Protokolle können nicht bereinigt werden

#### <a name="problem"></a>Problem

Beim Ausführen des DevClusterSetup-Skripts finden Sie unter Fehler wie folgt:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Lösung

Schließen Sie das aktuelle PowerShell-Fenster und öffnen Sie ein neues PowerShell als Administrator. Nun werden kann das Skript erfolgreich ausgeführt.

## <a name="cluster-connection-failures"></a>Cluster-Verbindungsfehler

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service Fabric-PowerShell-Cmdlets werden in Azure PowerShell nicht erkannt.

#### <a name="problem"></a>Problem

Wenn Sie versuchen, Service Fabric-PowerShell-Cmdlets wie `Connect-ServiceFabricCluster` in einem Fenster Azure PowerShell fehlschlagen, dass das Cmdlet nicht erkannt wird. Dafür werden Azure PowerShell 32-Bit-Version von Windows PowerShell (auch auf 64-Bit-Betriebssystemversionen) verwendet der Service Fabric-Cmdlets nur 64-Bit-Umgebung arbeiten.

#### <a name="solution"></a>Lösung

Service Fabric Cmdlets direkt in Windows PowerShell immer ausgeführt.

>[AZURE.NOTE] Die neueste Version von Azure PowerShell erstellt keine spezielle Verknüpfung, damit dies nicht der Fall.

### <a name="type-initialization-exception"></a>Geben Sie die Initialisierung Ausnahme

#### <a name="problem"></a>Problem

Beim Verbinden mit dem Cluster in PowerShell Fehler TypeInitializationException für System.Fabric.Common.AppTrace angezeigt.

#### <a name="solution"></a>Lösung

Die Pfadvariable wurde während der Installation nicht ordnungsgemäß festgelegt. Windows abmelden und wieder anmelden. Dadurch werden den Pfad vollständig aktualisiert.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Cluster-Verbindung schlägt mit "Objekt wird geschlossen"

#### <a name="problem"></a>Problem

Ein Aufruf verbinden ServiceFabricCluster tritt ein Fehler wie folgt:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Lösung

Schließen Sie das aktuelle PowerShell-Fenster und öffnen Sie ein neues PowerShell als Administrator. Sie sollten jetzt herstellen können.

### <a name="fabric-connection-denied-exception"></a>Verbindung verweigert Fabric-Ausnahme

#### <a name="problem"></a>Problem

Beim Debuggen von Visual Studio erhalten Sie einen FabricConnectionDeniedException-Fehler.

#### <a name="solution"></a>Lösung

Dieser Fehler tritt gewöhnlich auf, wenn Sie versuchen zu versuchen zu einem Diensthost manuell anstatt Service Fabric Runtime starten Sie verarbeiten.

Stellen Sie sicher, dass Sie nicht als Startprojekte in der Projektmappe serviceprojekte. Nur Service Fabric-Anwendungsprojekte sollte als Startprojekte festgelegt werden.

>[AZURE.TIP] Wenn Setup lokale Cluster ungewöhnlich Verhalten beginnt, können Sie mit lokalen Cluster Manager-Taskleistenanwendung zurücksetzen. Den vorhandenen Cluster entfernt und eine neue einrichten. Beachten Sie, dass alle Applikationen bereitgestellt und zugeordnete Daten entfernt.

## <a name="next-steps"></a>Nächste Schritte

- [Verstehen Sie und behandeln Sie die Cluster Health Systemberichte](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Visualisieren der Cluster Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md)
