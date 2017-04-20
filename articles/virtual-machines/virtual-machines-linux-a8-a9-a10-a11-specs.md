<properties
 pageTitle="Um rechenintensive VMs mit Linux | Microsoft Azure"
 description="Erhalten Sie Hintergrundinformationen und Hinweise für die Verwendung der H-Serie und A8, A9 A10 und A11 rechenintensive für Linux VMs"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Über H-Serie und rechenintensiver eine Reihe VMs 

Hier ist Hintergrundinformationen und einige Aspekte der Verwendung neuer Azure H-Serie sowie die früheren A8, A9 und A10, A11 Größe auch *rechenintensive* Instanzen. Dieser Artikel befasst sich mit diesen Größen für Linux VMs. Dieser Artikel steht auch für [Windows-VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugriff auf das Netzwerk RDMA

Ausführen eines der folgenden HPC Linux-Distributionen und unterstützten MPI-Implementierung zu Azure RDMA-Netzwerk unterstützt, können Sie Cluster RDMA-fähig Linux VMs erstellen. Weitere [Einrichten eines Clusters Linux RDMA MPI Betriebssystemservices](virtual-machines-linux-classic-rdma-cluster.md) Bereitstellungsoptionen und Beispiel Konfigurationsschritte.

* **Verteilung** - VMs müssen von RDMA-fähig SUSE Linux Enterprise Server (SLES) oder OpenLogic CentOS-basierten HPC Bilder in Azure Marketplace bereitstellen. Die folgenden Marketplace Bilder unterstützen Linux RDMA-Treiber:

    * SLES 12 SP1 für HPC 12 SLES SP1 für HPC (Premium)
    
    * SLES 12 für HPC SLES 12 für HPC (Premium)
    
    * 7.1 HPC CentOS basiert
    
    * 6.5 HPC CentOS basiert
    
    >[AZURE.NOTE]H-Serie VMs empfohlen einer SLES 12 SP1 für HPC Bild oder 7.1 HPC Bild CentOS basiert.
    >
    >CentOS-basierten HPC-Bilder sind in der Konfigurationsdatei **Yum** Kernel-Updates deaktiviert. Ist die Linux RDMA Treiber als ein RPM-Paket verteilt werden und Treiberupdates funktioniert möglicherweise nicht, wenn der Kernel aktualisiert wird.

* **MPI** - Intel MPI-Bibliothek 5.x

    Je nach Markt Bild Sie, separate Lizenzierung, Installation oder Konfiguration von Intel MPI erforderlich sein, wie folgt: 
    
    * **SLES 12 SP1 für HPC Bild** - Intel MPI-Pakete auf dem virtuellen Computer verteilt, durch Ausführen des folgenden Befehls installieren:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **SLES 12 Bild HPC** - Sie müssen separat herunterladen und Installieren von Intel MPI registrieren. Finden Sie im [Installationshandbuch des Intel MPI-Bibliothek](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **HPC CentOS-basierte Abbilder** - Intel MPI 5.1 ist bereits installiert.  

    Zusätzliche Konfiguration musste Aufträge MPI Cluster virtuellen Computer ausgeführt. Beispielsweise müssen in einem Cluster virtuellen Computer vertrauen zwischen den Compute-Knoten. Standardeinstellungen finden Sie unter [Einrichten eines Clusters Linux RDMA MPI-Anwendung ausgeführt](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Hinweise für HPC Pack und Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), kostenlose Microsoft HPC-Cluster und Projekt-Management-Lösung bietet eine Option für rechenintensive Instanzen mit Linux verwenden. Die neuesten Versionen von HPC Pack 2012 R2 unterstützt mehrere Linux-Distributionen auf compute-Knoten in Azure VMs Headknoten Windows Server verwaltet bereitgestellt. RDMA-fähig Linux Compute-Knoten mit Intel MPI kann Linux MPI Applikationen, die RDMA-Netzwerk zugreifen HPC Pack planen und ausführen. Zum Einstieg finden Sie unter [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Vorüberlegungen zum Netzwerk-Topologie

* RDMA-fähigen Linux VMs in Azure ist Eth1 für Netzwerkverkehr RDMA reserviert. Ändern Sie Eth1 Einstellungen oder Informationen in der Konfigurationsdatei auf diesem Netzwerk nicht. Reguläre Azure Netzwerkverkehr eth0 vorbehalten.

* In Azure wird IP über InfiniBand (IB) nicht unterstützt. Nur RDMA über IB wird unterstützt.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA Treiberupdates für SLES 12

Nach dem Erstellen einer VM basierend auf SLES 12 HPC-Image müssen Sie die Treiber RDMA VMs RDMA Netzwerkkonnektivität aktualisieren. 

>[AZURE.IMPORTANT]Dieser Schritt ist **erforderlich** für SLES 12 für HPC VM-Bereitstellung in allen Regionen Azure. 
>Dieser Schritt ist nicht erforderlich, wenn SLES 12 SP1 HPC 7.1 HPC CentOS-basierte und 6.5 HPC CentOS basierende VM bereitstellen. 

Bevor Sie die Treiber aktualisieren, beenden Sie alle **Zypper** Prozesse oder Prozesse, die die SUSE Repo-Datenbanken auf dem virtuellen Computer sperren. Andernfalls können der Treiber nicht korrekt aktualisiert.  

Um Linux RDMA Treiber auf jede VM zu aktualisieren, führen Sie eine der folgenden Azure-CLI-Befehle vom Clientcomputer.

**SLES 12 HPC VM bereitgestellt in der klassischen Bereitstellungsmodell**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**SLES 12 HPC VM bereitgestellt in der Ressourcen-Manager-Bereitstellungsmodell**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Es kann etwas dauern, den Treiber zu installieren und der Befehl ohne Ausgabe. Nach der Aktualisierung sollte Ihrer VM startet und in wenigen Minuten einsatzbereit.

### <a name="sample-script-for-driver-updates"></a>Beispielskript für Treiberupdates

Haben Sie einen Cluster von SLES 12 für HPC VMs können Sie das Treiberupdate auf allen Knoten im Cluster Skript. Das folgende Skript aktualisiert z. B. die Treiber in einem Cluster mit 8 Knoten.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Nächste Schritte

* Informationen über Verfügbarkeit und Preise für rechenintensive Größen finden Sie unter [virtuelle Computer Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Speicherkapazitäten und Datenträger finden Sie unter [Größe für virtuelle Computer](virtual-machines-linux-sizes.md).

* Zunächst bereitstellen und RDMA unter Linux mit rechenintensiven Größen finden Sie unter [Einrichten eines Clusters Linux RDMA MPI-Anwendung ausgeführt](virtual-machines-linux-classic-rdma-cluster.md).


