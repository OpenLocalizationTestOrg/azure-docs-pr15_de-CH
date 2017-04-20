<properties
 pageTitle="Skalierungsgröße HPC Pack Clusterknoten | Microsoft Azure"
 description="Automatisch vergrößern Sie und verkleinern Sie die Anzahl der HPC Pack Compute Clusterknoten in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatisch vergrößern und Verkleinern der HPC Pack Clusterressourcen in Azure nach Arbeitslast cluster




Wenn Azure "Ausbruch" Knoten im Cluster HPC Pack bereitstellen ein Clusters HPC Pack in Azure VMs erstellen, sollten Sie können automatisch vergrößert oder verkleinert die Anzahl der Azure computeressourcen Kerne nach der aktuellen Arbeitslast im Cluster oder Knoten. Dadurch können Sie Ihre Azure-Ressourcen effizienter und ihre Kosten kontrollieren.
Hierzu richten Sie HPC Pack Cluster-Eigenschaft **AutoGrowShrink**. Auch das Skript **AzureAutoGrowShrink.ps1** HPC PowerShell, die mit HPC Pack installiert ist.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Auch können aktuell automatisch vergrößert und verkleinert HPC Pack Compute-Knoten, die ein Windows Server-Betriebssystem ausgeführt werden.

## <a name="set-the-autogrowshrink-cluster-property"></a>Legen Sie die Eigenschaft AutoGrowShrink cluster

### <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack 2012 R2 Update 2 oder höher Cluster** - Head Clusterknoten kann entweder lokal bereitgestellt oder in Azure-VM. Finden Sie Einstieg in einen lokalen Kopf und Azure "Ausbruch" Knoten [ein Hybrid-Cluster mit HPC Pack eingerichtet](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Lesen Sie [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) HPC Pack-Cluster in Azure VMs schnell bereitstellen.


* **Für einen Cluster mit Head-Knoten in Azure** - verwenden Sie das Bereitstellungsskript HPC Pack IaaS Cluster erstellen, **AutoGrowShrink** Cluster-Eigenschaft durch Festlegen der Option AutoGrowShrink in der Clusterkonfigurationsdatei aktivieren. Informationen finden Sie in der Dokumentation zum [Skript herunterladen](https://www.microsoft.com/download/details.aspx?id=44949). 

    Alternativ aktivieren Sie **AutoGrowShrink** Cluster-Eigenschaft nach der Bereitstellung des Clusters mit HPC-PowerShell-Befehlen im folgenden Abschnitt beschrieben. Zur Vorbereitung dieser zuerst folgende Schritte:
    1. Konfigurieren eines Zertifikats Azure Management auf dem Head-Knoten und in der Azure-Abonnement. Für eine Testkonfiguration können das Standard Microsoft HPC Azure selbstsignierte Zertifikat, das HPC Pack auf dem Head-Knoten installiert und das Zertifikat zu Ihrem Azure-Abonnement geladen. Optionen und Schritten finden Sie in der [TechNet-Bibliothek Anleitung](https://technet.microsoft.com/library/gg481759.aspx).
    2. Führen Sie **Regedit** auf dem Head-Knoten, gehen Sie zu HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo und fügen Sie einen neuen Wert hinzu. Wertname auf "Fingerabdruck", und Wert auf den Fingerabdruck des Zertifikats in Schritt 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC-PowerShell-Befehle zum Festlegen der AutoGrowShrink-Eigenschaft

Folgende Beispiele sind HPC-PowerShell-Befehlen, **AutoGrowShrink** und dessen Verhalten mit zusätzlichen Parametern abstimmen. [AutoGrowShrink-Parameter](#AutoGrowShrink-parameters) in diesem Artikel eine vollständige Liste der Einstellungen anzeigen 

Um diese Befehle auszuführen, starten Sie HPC PowerShell auf Cluster-Head-Knoten als Administrator.

**So aktivieren Sie die AutoGrowShrink-Eigenschaft**

    Set-HpcClusterProperty –EnableGrowShrink 1

**So deaktivieren Sie die AutoGrowShrink-Eigenschaft**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Ändern des Intervalls wachsen in Minuten**

    Set-HpcClusterProperty –GrowInterval <interval>

**Ändern des Intervalls verkleinern in Minuten**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Zum Anzeigen der aktuellen Konfigurations des AutoGrowShrink**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink Parameter

Im folgenden werden AutoGrowShrink-Parameter, die Sie mit dem Befehl **Set HpcClusterProperty** ändern können.

* **EnableGrowShrink** - Option aktiviert oder deaktiviert die **AutoGrowShrink** -Eigenschaft.
* **ParamSweepTasksPerCore** - Anzahl der parametrischen Sweep Aufgaben zu einem Kern. Der Standardwert ist ein Kern pro Vorgang zu. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 wird **ParamSweepTasksPerCore** in **TasksPerResourceUnit**geändert. Es kann basiert auf den Ressourcentyp Auftrag und Knoten, Socket oder Core.
    
* **GrowThreshold** - Schwellenwert der Warteschlange stehender Aufgaben Automatische Wachstum. Der Standardwert ist 1, d. h., der 1 oder mehrere Aufgaben in der Warteschlange gibt Knoten wachsen.
* **GrowInterval** - Intervall in Minuten automatische Wachstum. Das Standardintervall ist 5 Minuten.
* **ShrinkInterval** - Intervall in Minuten automatische Verkleinerung auslösen. Der Standardwert beträgt 5 Minuten. |
* **ShrinkIdleTimes** - kontinuierlichen Kontrollen zu verkleinern, um anzugeben, dass die Knoten im Leerlauf befinden. Der Standardwert ist 3. Beispielsweise ist **ShrinkInterval** 5 Minuten, prüft HPC Pack der Knoten alle 5 Minuten im Leerlauf befindet. Wenn die Knoten im Leerlauf sind nach Prüfung des kontinuierlichen 3 (15 Minuten) verkleinert HPC Pack Knotens.
* **ExtraNodesGrowRatio** - Weitere Prozentsatz der Knoten Message Passing Interface (MPI) Aufträge zu. Der Standardwert ist 1, d.h. HPC Pack Knoten 1 % für MPI Aufträge zunimmt. 
* **GrowByMin** - Switch an, ob die automatische Richtlinie für das Projekt erforderlichen Mindestressourcen basiert. Der Standardwert ist false, d.h. HPC Pack Knoten für Aufträge basierend auf der maximalen Ressourcen für die Projekte erforderlich wird.
* **SoaJobGrowThreshold** - Schwellenwert von SOA-Anfragen an das automatische Auslösen wachsen Prozess. Der Standardwert ist 50000.  
    
    >[AZURE.NOTE] Dieser Parameter wird im HPC Pack 2012 R2 Update 3 unterstützt.
    
* **SoaRequestsPerCore** -Anzahl der eingehenden SOA-Anfragen zu einem Kern. Der Standardwert ist 20000.  

    >[AZURE.NOTE] Dieser Parameter wird im HPC Pack 2012 R2 Update 3 unterstützt.

### <a name="mpi-example"></a>MPI-Beispiel

Standardmäßig nimmt HPC Pack 1 % zusätzlichen Knoten für MPI-Projekte (**ExtraNodesGrowRatio** ist auf 1 festgelegt). Deshalb MPI möglicherweise mehrere Knoten, und der Auftrag kann nur ausgeführt werden, wenn alle Knoten bereit. Wenn Azure Knoten beginnt, gelegentlich einen Knoten mehr Zeit als andere, wodurch andere Knoten im Leerlauf, beim Warten auf diesem Knoten bereit zu starten. Wachsende zusätzliche Knoten, HPC Pack verringert diese Ressource warten und potenziell spart. Erhöhen Sie den Prozentsatz der zusätzlichen Knoten MPI Aufträge (z. B. 10 %) führen Sie einen Befehl aus

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-Beispiel

Standardmäßig ist **SoaJobGrowThreshold** auf 50000 und **SoaRequestsPerCore** bis 200000 festgelegt. Vor einer SOA-Projekt mit 70000 gibt es eine Aufgabe in der Warteschlange und Anfragen 70000. In diesem Fall nimmt HPC Pack wächst 1 Core für die Aufgabe in der Warteschlange und eingehende Anfragen (70000 50000) / 20000 = 1 core, also insgesamt 2 Kernen SOA dafür wachsen.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Führen Sie das Skript AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack 2012 R2 Update 1 oder höher Cluster** - **AzureAutoGrowShrink.ps1** -Skript befindet sich im Ordner CCP_HOME Bin. Cluster-Head-Knoten kann entweder lokal bereitgestellt oder in Azure-VM. Finden Sie Einstieg in einen lokalen Kopf und Azure "Ausbruch" Knoten [ein Hybrid-Cluster mit HPC Pack eingerichtet](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Finden Sie das [Bereitstellungsskript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) schnell bereitstellen ein Clusters HPC Pack in Azure VMs oder verwenden Sie ein [Azure Schnellstart-Vorlage](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** - Skript derzeit hängt dieser spezifischen Version von Azure PowerShell. Wenn Sie eine neuere Version auf dem Head-Knoten ausgeführt, möglicherweise, Azure PowerShell auf [Version 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) ausgeführt. 

* **Für einen Cluster mit Knoten Azure Burst** - Skript auf einem Clientcomputer dem HPC Pack installiert ist oder auf dem Head-Knoten. Auf einem Clientcomputer ausgeführt wird, stellen Sie sicher, dass Sie die Variable $env: CCP_SCHEDULER ordnungsgemäß auf dem Head-Knoten. Azure "Ausbruch" Knoten müssen bereits zum Cluster hinzugefügt werden können, sie jedoch im Zustand nicht bereitgestellt.


* **Für einen Cluster bereitgestellt in Azure VMs** - führen Sie das Skript auf dem Headknoten VM hängt **HpcIaaSNode.ps1 Start** und **Stop HpcIaaSNode.ps1** Skripts, sind installiert ist. Diese Skripts Außerdem benötigen Azure Zeugnisse oder veröffentlichen Einstellungsdatei (siehe [Manage compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Stellen Sie sicher alle Computeknoten VMs, die Sie bereits zum Cluster hinzugefügt. Sie können den Zustand beendet sein.

### <a name="syntax"></a>Syntax

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Parameter

 * **NodeTemplates** - Namen der Knoten Vorlagen zum Definieren des Bereichs für die Knoten vergrößern und verkleinern. Wenn nicht angegeben (der Standardwert ist @()), alle Knoten in der Knotengruppe **AzureNodes** sind im Bereich **NodeType** hat einen Wert von AzureNodes und alle Knoten in der Knotengruppe **ComputeNodes** sind im Bereich Wenn **NodeType** den Wert ComputeNodes hat.

 * **JobTemplates** - Namen der Auftragsvorlagen zum Knoten zu definieren.

 * **NodeType** - Knoten vergrößern und verkleinern. Werte werden unterstützt:

     * **AzureNodes** – Azure PaaS (Burst) Knoten in einer lokalen oder Azure IaaS.

     * **ComputeNodes** - für Compute-Knoten VMs in einem Cluster Azure IaaS.

* **NumOfQueuedJobsPerNodeToGrow** - Anzahl der Aufträge zu einem Knoten erforderlich.

* **NumOfQueuedJobsToGrowThreshold** - die Anzahl der Aufträge zum Starten des Prozesses vergrößern.

* **NumOfActiveQueuedTasksPerNodeToGrow** - die Anzahl der aktiven Warteschlange Aufgaben zu einem Knoten. **NumOfQueuedJobsPerNodeToGrow** mit einem Wert größer als 0 angegeben ist, wird dieser Parameter ignoriert.

* **NumOfActiveQueuedTasksToGrowThreshold** - die Zahl der aktiven Warteschlange Aufgaben zum Starten des Prozesses vergrößern.

* **NumOfInitialNodesToGrow** - erste Mindestanzahl Knoten wachsen, wenn alle Knoten im Bereich **Nicht bereitgestellt** oder **beendet (Deallocated)**.

* **GrowCheckIntervalMins** - Intervall in Minuten zwischen weiter.

* **ShrinkCheckIntervalMins** - Intervall in Minuten zwischen verkleinern.

* **ShrinkCheckIdleTimes** - Anzahl der verkleinern überprüft (getrennt durch **ShrinkCheckIntervalMins**) um anzugeben, dass die Knoten im Leerlauf befinden.

* **UseLastConfigurations** - der vorherigen Konfigurationen in der Argumentdatei gespeichert.

* **ArgFile**- der Name der Argumentdatei verwendet, um speichern und Konfigurationen, um das Skript auszuführen.

* **LogFilePrefix** - Präfixname der Protokolldatei. Sie können einen Pfad angeben. Standardmäßig wird das Protokoll in das aktuelle Arbeitsverzeichnis geschrieben.

### <a name="example-1"></a>Beispiel 1

Das folgende Beispiel konfiguriert die Azure Burst-Knoten mit der Standardvorlage AzureNode vergrößern und verkleinern automatisch bereitgestellt. Wenn alle Knoten zunächst im Zustand **Nicht bereitgestellt** werden, werden mindestens 3 Knoten gestartet. Übersteigt die Anzahl der Aufträge in 8, startet das Skript Knoten, bis deren Anzahl Anteil Warteschlangenaufträge **NumOfQueuedJobsPerNodeToGrow**überschreitet. Wenn ein Knoten in Leerlauf 3 Mal im Leerlauf befindet, wird er beendet.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Beispiel 2

Das folgende Beispiel konfiguriert den Azure Compute-Knoten VMs mit der Standardvorlage ComputeNode vergrößern und verkleinern automatisch bereitgestellt.
Konfiguriert die Standardvorlage Auftrag Aufträge definieren des Umfangs der Arbeitslast im Cluster. Wenn alle Knoten zunächst angehalten werden, werden mindestens 5 Knoten gestartet. Übersteigt die Anzahl der aktiven Warteschlange Aufgaben 15, startet das Skript Knoten, bis deren Anzahl das Verhältnis von aktiven Aufgaben in der Warteschlange **NumOfActiveQueuedTasksPerNodeToGrow überschreitet**. Wenn ein Knoten im Leerlauf in 10 aufeinander folgenden Leerlaufzeiten gefunden wird, wird er beendet.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
