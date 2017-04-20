<properties
 pageTitle="Windows HPC Pack Cluster Optionen in die Cloud | Microsoft Azure"
 description="Mit Microsoft HPC Pack erstellen und Verwalten einer Windows Hochleistungscomputing (HPC)-Cluster in der Azure-Cloud erfahren"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Mit HPC Pack erstellen und Verwalten eines Windows HPC-Clusters in Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel befasst sich Optionen für HPC Pack Cluster Windows Arbeitslasten ausführen. Es gibt auch Optionen zum Erstellen von Clustern [HPC-Linux-Arbeitslasten mit HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)ausgeführt.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Ausführen eines Clusters HPC Pack in Azure VMs

### <a name="azure-templates"></a>Azure-Vorlagen

* (Markt) [HPC Pack Cluster für Windows-Arbeitslasten](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Markt) [HPC Pack Cluster für Excel Arbeitslasten](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Schnellstart) [Erstellen einer HPC-cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Schnellstart) [Erstellen einer HPC-Cluster mit benutzerdefinierten Compute-Knoten](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM-images

* [HPC Pack für Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [HPC Pack Compute-Knoten auf Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack compute-Knoten mit Excel für Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>PowerShell Bereitstellungsskript

* [Erstellen Sie HPC-Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Lernprogramme

* [Lernprogramm: Erste Schritte mit eines Clusters HPC Pack in Azure Excel und SOA Arbeitslasten ausführen](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Manuelle Bereitstellung mit Azure-portal

* [Der Head-Knoten eines Clusters HPC Pack in Azure-VM einrichten](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Clusterverwaltung

* [Verwalten Sie Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Vergrößern und Verkleinern von Azure computeressourcen in einem Cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Aufträge zu einem Cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Projektmanagement in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Arbeitskraft Rolle Knoten zu einem Cluster HPC Pack hinzufügen


* [Azure Worker-Instanzen mit HPC Pack Burst](https://technet.microsoft.com/library/gg481749.aspx)

* [Anleitung: Einrichten eines Clusters Hybrid mit HPC Pack in Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Ein HPC Pack Headknoten in Azure Azure "Ausbruch" Knoten hinzufügen](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integration mit Azure 

* [Azure Batch mit HPC Pack Burst](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA-Cluster für MPI-Arbeitslasten erstellen

* [Einrichten eines Clusters RDMA Windows HPC Pack MPI-Anwendung ausführen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
