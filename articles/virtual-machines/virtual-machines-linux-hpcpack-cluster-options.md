<properties
 pageTitle="Linux HPC Pack Cluster-Optionen in die Cloud | Microsoft Azure"
 description="Mit Microsoft HPC Pack erstellen und Verwalten einer Linux Hochleistungscomputing (HPC)-Cluster in der Azure-Cloud erfahren"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Mit HPC Pack erstellen und Verwalten einer HPC-Cluster in Azure für Linux-workloads

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel befasst sich Optionen mit HPC Pack Linux Arbeitslasten ausführen. Es gibt auch Optionen zum Ausführen von [Windows HPC-Arbeitslasten mit HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Ausführen eines Clusters HPC Pack in Azure VMs

### <a name="azure-templates"></a>Azure-Vorlagen


* (Markt) [HPC Pack Cluster für Linux-workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Schnellstart) [Erstellen eines HPC-Clusters mit Linux compute-Knoten](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>PowerShell Bereitstellungsskript

* [Erstellen Sie Linux HPC-Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Lernprogramme

* [Lernprogramm: Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Lernprogramm: Ausführen NAMD Microsoft HPC Pack unter Linux Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Lernprogramm: Ausführen OpenFOAM Microsoft HPC Pack auf einem Cluster Linux RDMA in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Lernprogramm: Ausführen STAR-CCM + mit Microsoft HPC Pack auf Linux RDMA cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Clusterverwaltung

* [Aufträge zu einem Cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Projektmanagement in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA-Cluster für MPI-Arbeitslasten erstellen

* [Lernprogramm: Ausführen OpenFOAM Microsoft HPC Pack auf einem Cluster Linux RDMA in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Einrichten eines Clusters Linux RDMA MPI-Anwendung ausführen](virtual-machines-linux-classic-rdma-cluster.md)

