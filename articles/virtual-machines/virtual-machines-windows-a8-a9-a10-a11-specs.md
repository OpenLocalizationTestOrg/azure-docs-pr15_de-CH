<properties
 pageTitle="Um rechenintensive VMs mit Windows | Microsoft Azure"
 description="Erhalten Sie Hintergrundinformationen und Hinweise für die Verwendung der Azure H-Serie und A8, A9 A10 und A11 rechenintensive Windows-VMs und cloud"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Über H-Serie und rechenintensiver eine Reihe VMs

Hier ist Hintergrundinformationen und einige für neuere Azure H-Serie mit früheren A8, A9 und A10, A11 Instanzen auch *rechenintensive* Instanzen. Dieser Artikel befasst sich mit diesen Instanzen für Windows-VMs. Dieser Artikel steht auch für [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugriff auf das Netzwerk RDMA

Sie können Cluster RDMA-fähige Windows Server-Instanzen erstellen und unterstützten MPI-Implementierung zu Azure RDMA Netzwerk bereitstellen. Dieses Netzwerk niedriger Latenz und hohem Durchsatz ist für MPI-Verkehr reserviert.

* **Betriebssystem**
    * **Virtuelle Maschinen** – Windows Server 2012 R2, Windows Server 2012
    * **Cloud-Services** – Windows Server 2012 R2, Windows Server 2012 und Windows Server 2008 R2-Gastbetriebssystem Familie

* **MPI** - Microsoft MPI (MS-MPI) 2012 R2 oder höher Intel MPI-Bibliothek 5.x

Unterstützte MPI-Implementierung verwenden Microsoft Netzwerk direkte Schnittstelle zur Kommunikation zwischen Instanzen. Finden Sie unter [Einrichten eines Clusters Windows RDMA mit HPC Pack MPI ausgeführt](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) und [verwendet mehrere Instanzen Message Passing Interface (MPI) Anwendung in Azure Batch auszuführenden Aufgaben](../batch/batch-mpi.md) Bereitstellungsoptionen und Beispiel Konfigurationsschritte.


>[AZURE.NOTE]RDMA-fähig rechenintensive VMs muss die Erweiterung HpcVmDrivers hinzugefügt werden VMs Windows Netzwerk-Gerätetreiber zu installieren, die RDMA Konnektivität erforderlich sind. HpcVmDrivers-Erweiterung wird i. d. r. automatisch hinzugefügt. Die Erweiterung hinzufügen, finden Sie unter [Verwalten von VM-Erweiterungen](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Hinweise für HPC Pack und Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx)kostenlose Microsoft HPC-Cluster und Projekt-Management-Lösung ist nicht erforderlich für rechenintensive Instanzen von Windows Server verwenden. Allerdings ist es eine Option in Azure Compute-Cluster erstellen MPI Windows-basierte Anwendung und anderen HPC-Arbeitslasten. HPC Pack 2012 R2 und spätere Versionen enthalten eine Runtime-Umgebung für MS MPI, die Azure RDMA Netzwerk bei RDMA-fähig VMs verwenden kann.

Weitere Informationen und Checklisten rechenintensive Instanzen mit HPC Pack unter Windows Server finden Sie unter [Einrichten eines Clusters RDMA Windows HPC Pack MPI-Anwendung ausgeführt](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Nächste Schritte

* Informationen über Verfügbarkeit und Preise für rechenintensive Größen finden Sie unter [virtuelle Computer Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) und [Preise von Cloud-Diensten](https://azure.microsoft.com/pricing/details/cloud-services/).

* Speicherkapazitäten und Datenträger finden Sie unter [Größe für virtuelle Computer](virtual-machines-linux-sizes.md).

* Zunächst bereitstellen und Verwenden von rechenintensiven Instanzen mit HPC Pack unter Windows finden Sie unter [Einrichten eines Clusters RDMA Windows HPC Pack MPI-Anwendung ausgeführt](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Informationen über Instanzen A8 und A9 MPI-Applikationen mit Azure ausführen finden Sie unter [verwenden mehrere Instanzen Message Passing Interface (MPI) Anwendung in Azure Batch auszuführenden Aufgaben](../batch/batch-mpi.md).
