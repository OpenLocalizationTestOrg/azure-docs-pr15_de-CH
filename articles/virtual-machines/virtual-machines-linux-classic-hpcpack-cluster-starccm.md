<properties
 pageTitle="Ausführen von STAR-HPC Pack unter Linux VMs CCM + | Microsoft Azure"
 description="Einen Cluster Microsoft HPC Pack Azure bereitstellen und Ausführen eine STAR-CCM + Auftrag auf mehrere Linux compute-Knoten in einem Netzwerk RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Ausführen von STAR-CCM + mit Microsoft HPC Pack auf Linux RDMA cluster in Azure
Dieser Artikel veranschaulicht das Bereitstellen eines Clusters Microsoft HPC Pack auf Azure und führen eine [CD Adapco STAR-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) Auftrag auf mehrere Linux-Datenverarbeitungsknoten, die mit InfiniBand verbunden sind.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack bietet eine Vielzahl von großen HPC und parallele Anwendungen MPI, in Clustern von Microsoft Azure virtuellen Maschinen ausführen. HPC Pack unterstützt auch Linux HPC Programme unter Linux Compute-Knoten VMs, die in einem Cluster HPC Pack bereitgestellt werden. Eine Einführung in Linux compute-Knoten mit HPC Pack, finden Sie unter [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Einrichten eines Clusters HPC Pack
HPC Pack IaaS Bereitstellungsskripts aus dem [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) herunter und extrahieren Sie sie lokal.

Azure PowerShell ist. Wenn PowerShell nicht auf dem lokalen Computer konfiguriert ist, lesen Sie den Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Zum Zeitpunkt der Erstellung dieses Dokuments sind Bilder Linux Azure-Markt (enthält die InfiniBand-Treiber für Azure) SLES 12 CentOS 6.5 und CentOS 7.1. Dieser Artikel basiert auf der Nutzung der SLES 12. Führen Sie zum Abrufen des Namens des Linux-Images, die HPC im Markt unterstützen den folgenden PowerShell-Befehl:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Die Ausgabe enthält den Speicherort in dem Bilder verfügbar sind und den Abbildnamen (**ImageName**) später in der Vorlage für die Bereitstellung verwendet werden.

Bevor Sie den Cluster bereitstellen, müssen Sie eine HPC Pack Deployment-Datei erstellen. Da wir eine kleine Gruppe abzielen, wird der Head-Knoten der Domänencontroller und Hosten lokale SQL-Datenbank.

Die folgende Vorlage solche Head-Knoten bereitstellen, erstellen Sie eine XML-Datei mit dem Namen **MyCluster.xml**und Ersetzen Sie die Werte von **SubscriptionId**, **StorageAccount**, **Ort**, **VMName**und **Dienstname** mit.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Starten der Head-Knoten-Erstellung PowerShell-Befehl in ein Eingabeaufforderungsfenster mit erhöhten Rechten ausführen:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Nach 20 bis 30 Minuten sollte der Head-Knoten bereit. Sie können aus dem Azure-Portal zu verbinden, **Verbinden** des virtuellen Computers klicken.

Schließlich möglicherweise DNS-Weiterleitung zu beheben. Dazu starten Sie DNS-Manager.

1.  Maustaste auf den Server im DNS-Manager und klicken Sie dann auf die Registerkarte **Weiterleitung** wählen Sie **Eigenschaften**.

2.  Klicken Sie auf die Schaltfläche **Bearbeiten** , um eine Weiterleitung zu entfernen, und klicken Sie dann auf **OK**.

3.  **Aktivieren Sie das Kontrollkästchen **Verwenden von Stammhinweisen liegen keine Weiterleitung** **

## <a name="set-up-linux-compute-nodes"></a>Einrichten von Linux Compute-Knoten
Linux Compute-Knoten bereitstellen mithilfe derselben Bereitstellungsvorlage, die zur Erstellung den Head-Knoten.

Kopieren Sie die Datei **MyCluster.xml** vom lokalen Computer in den Head-Knoten und aktualisieren **NodeCount** Tag mit der Anzahl Knoten, die Sie bereitstellen möchten (< = 20). Vorsicht genügend verfügbaren Kerne in Ihr Kontingent Azure haben jeweils A9 16 Kerne im Rahmen Ihres Abonnements nutzen. A8 Instanzen (8 Kerne) können statt A9 mehrere VMs in demselben Budget verwendet werden soll.

Kopieren Sie auf dem Head-Knoten Bereitstellungsskripts HPC Pack IaaS.

Führen Sie die folgenden Azure PowerShell Befehle in Belegen:

1.  **Add-AzureAccount** für die Verbindung zu Ihrem Azure-Abonnement ausführen.

2.  Haben Sie mehrere Abonnements führen Sie **Get-AzureSubscription** , um sie aufzulisten.

3.  Festlegen ein Standard-Abonnements der **Wählen Sie AzureSubscription - SubscriptionName-Xxxx-Standard** Befehl.

4.  Ausführen **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** Linux zu Bereitstellung von compute-Knoten.

    ![Head-Knoten-Bereitstellung in Aktion][hndeploy]

Öffnen Sie das HPC Pack Cluster Manager-Tool. Nach einigen Minuten erscheint Linux Compute-Knoten regelmäßig in Liste Cluster Compute-Knoten. Mit dem Bereitstellungsmodus classic werden fortlaufend IaaS VMs erstellt. So ist die Anzahl der Knoten wichtig, nehmen alle bereitgestellt bekommen viel Zeit.

![Linux-Knoten HPC Pack Cluster Manager][clustermanager]

Da alle Knoten im Cluster ausgeführt werden, sind zusätzliche Einstellungen.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Ein Azure-Dateifreigabe für Windows- und Linux-Knoten einrichten
Den Azure-Dateidienst können Sie Skripts, Pakete und Datendateien speichern. Azure-Datei bietet CIFS-Funktionen auf Azure BLOB-Speicher als einen permanenten Speicher. Beachten Sie, dass dies nicht die skalierbarste Lösung ist, aber es ist einfacher und erfordert dedizierten VMs.

Erstellen einer Azure-Dateifreigabe [Einstieg in Windows Azure Dateispeicher](..\storage\storage-dotnet-how-to-use-files.md)Artikel.

Behalten Sie den Namen Ihres Speicherkontos **Saname**und den Freigabenamen als **Freigabename**Speicherschlüssel Konto als **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Bereitstellen der Azure-Dateifreigabe auf dem Head-Knoten
Öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Rechten, und führen Sie den folgenden Befehl zum Speichern der Anmeldeinformationen im lokalen Depot:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Um die Dateifreigabe Azure bereitzustellen, führen Sie

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Bereitstellen der Azure-Dateifreigabe auf Linux Compute-Knoten
Ein wirklich hilfreiches Tool mit HPC Pack ist Clusrun-Tool. Dieses Befehlszeilentool können Sie den gleichen Befehl auf Compute-Knoten gleichzeitig. In diesem Fall dient es die Azure-Dateifreigabe und Neustarts überdauern beizubehalten.
Führen Sie die folgenden Befehle in ein erweitertes Eingabeaufforderungsfenster auf dem Head-Knoten.

Bereitstellungsverzeichnis erstellen:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Azure-Dateifreigabe bereitstellen:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Mount-Freigabe beibehalten werden:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installieren von STAR-CCM +
Azure VM-Instanzen A8 und A9 bieten InfiniBand RDMA-Funktionen. Kerneltreiber, mit denen diese Funktionen stehen für Windows Server 2012 R2, SUSE 12 CentOS 6.5 und CentOS 7.1 Bilder in Azure Marketplace. Microsoft MPI und Intel MPI (Version 5.x) sind zwei MPI-Bibliotheken, die diese Treiber in Azure unterstützen.

CD Adapco STAR-CCM + freigeben 11.x und später wird mit Intel MPI Version 5.x so InfiniBand Unterstützung für Azure enthalten ist.

Abrufen der Linux64 STAR-CCM-Paket von [CD Adapco Portal](https://steve.cd-adapco.com). In unserem Fall verwendet haben wir Version 11.02.010 gemischten Genauigkeit.

Erstellen Sie auf dem Head-Knoten in Azure Dateifreigabe **/hpcdata** ein Shell-Skript namens **setupstarccm.sh** mit dem folgenden Inhalt. Dieses Skript wird auf jeder Compute-Knoten einrichten STAR-CCM + lokal.

#### <a name="sample-setupstarcmsh-script"></a>Beispielskript für setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Jetzt einrichten STAR-CCM + auf Ihrem Linux compute-Knoten, ein erweitertes Eingabeaufforderungsfenster öffnen und führen Sie den folgenden Befehl:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Während der Befehl ausgeführt wird, können Sie die CPU-Auslastung mit Wärmekarte Cluster-Manager überwachen. Nach einigen Minuten sollte alle Knoten ordnungsgemäß eingerichtet werden.

## <a name="run-star-ccm-jobs"></a>Ausführen von STAR-CCM + Aufträge
HPC Pack dient für seine Job Scheduler ausführen STAR-CCM + Aufträge. Dazu brauchen wir die Unterstützung einige Skripts, die den Auftrag starten und Ausführen STAR-CCM +. Die eingegebenen Daten in der Azure-Dateifreigabe erste Einfachheit bleibt.

PowerShell-Skript verwendet, um einen Stern Warteschlange-CCM + Job. Es hat drei Argumente:

*  Der Name des Modells

*  Die Anzahl der Knoten verwendet werden

*  Die Anzahl der Kerne auf jedem Knoten verwendet werden

Da STAR-CCM + können die Speicherbandbreite Ausfüllen weniger Kerne pro Compute-Knoten und Hinzufügen neuer Knoten besser ist. Die genaue Anzahl der Kerne pro Knoten hängt die Prozessoren und die Geschwindigkeit von Interconnect.

Knoten werden ausschließlich für den Auftrag reserviert und nicht mit anderen Projekten gemeinsam genutzt werden. Der Auftrag wird nicht direkt als MPI Job gestartet. **Runstarccm.sh** -Shell-Skript startet das MPI-Startprogramm.

Das Eingabemodell und das **runstarccm.sh** -Skript werden in der Freigabe **/hpcdata** gespeichert, die zuvor bereitgestellt wurde.

Die Auftrags-ID benannt und befinden sich in der **/hpcdata gemeinsam**mit den Stern-CCM + Ausgabedateien.


#### <a name="sample-submitstarccmjobps1-script"></a>Beispielskript für SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Ersetzen Sie **runner.java** durch Ihre bevorzugte STAR-CCM + Java Modell Launcher und Protokollierungscode.

#### <a name="sample-runstarccmsh-script"></a>Beispielskript für runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

In unserem Test verwendet haben wir ein Power-On-Demand-Lizenztoken. Für das Token müssen Sie die Umgebungsvariable **$CDLMD_LICENSE_FILE** **1999@flex.cd-adapco.com** und die **Podkey -** Option von der Befehlszeile aus.

Nach einigen Initialisierung extrahiert das Skript – die **$CCP_NODES_CORES** Umgebungsvariablen, HPC Pack – die Liste der Knoten eine Host-Datei erstellen, die das MPI-Startprogramm verwendet festgelegt. Diese Host-Datei enthält die Liste der Namen der Compute-Knoten, die für den Auftrag einen Namen pro Zeile verwendet werden.

Das Format der **$CCP_NODES_CORES** folgt diesem Muster:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Wo:

* `<Number of nodes>`die Anzahl der Knoten, die diesem Auftrag zugeordnet ist.

* `<Name of node_n_...>`der Name der einzelnen Knoten, die diesem Auftrag zugeordnet ist.

* `<Cores of node_n_...>`die Anzahl der Kerne auf dem Knoten, die diesem Auftrag zugeordnet ist.

Die Anzahl der Kerne (**$NBCORES**) auch die Anzahl der Knoten (**$NBNODES**) und die Anzahl der Kerne pro Knoten (angegeben als Parameter **$NBCORESPERNODE**) errechnet.

MPI-Optionen sind die Intel MPI in Azure verwendet werden:

*   `-mpi intel`Intel MPI angeben.

*   `-fabric UDAPL`Azure InfiniBand Verben verwendet.

*   `-cpubind bandwidth,v`zur Optimierung der Bandbreite für MPI mit Stern-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`zu Intel MPI Azure InfiniBand arbeiten und die erforderliche Anzahl der Kerne pro Knoten.

*   `-batch`zu Stern-CCM + im Batch-Modus ohne Benutzeroberfläche.


Um einen Auftrag zu starten, stellen Sie sicher, die Knoten laufen in der Clusterverwaltung online sind. Dann von PowerShell-Befehlszeile ausführen:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Knoten anhalten
Später Sie anschließend mit der Tests sind, können Sie folgende HPC Pack PowerShell Befehle zum Beenden und Starten von Knoten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Nächste Schritte
Versuchen Sie, andere Linux-Arbeitslasten. Beispielsweise finden Sie unter:

* [NAMD Microsoft HPC Pack auf Linux Computeknoten in Azure ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [OpenFOAM mit Microsoft HPC Pack auf einem Cluster Linux RDMA in Azure ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
