<properties
   pageTitle="PowerShell-Skript Linux HPC-Cluster bereitstellen | Microsoft Azure"
   description="Ausführen eines PowerShell-Skripts zum Bereitstellen eines Clusters Linux HPC Pack in Azure virtuellen Maschinen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Erstellen Sie eine Linux Hochleistungscomputing (HPC)-Cluster mit HPC Pack IaaS Bereitstellungsskript

Führen Sie die Bereitstellung HPC Pack IaaS PowerShell-Skript einen vollständigen HPC-Cluster für Linux-Workloads in Azure virtuelle Computer bereitstellen. Der Cluster besteht aus einer Active Directory verknüpft Head-Knoten unter Windows Server und Microsoft HPC Pack und Computeknoten, die eines HPC Pack unterstützten Linux-Distributionen. Wenn Sie einen Cluster HPC Pack Arbeitslasten für Windows Azure bereitstellen möchten, finden Sie unter [Windows HPC-Cluster mit HPC Pack IaaS Bereitstellungsskript erstellen](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Eine Azure-Ressourcen-Manager-Vorlage können Sie einen HPC Pack Cluster bereitstellen. Ein Beispiel finden Sie unter [Erstellen eines HPC-Clusters mit Linux Knoten berechnet](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Beispiel-Konfigurationsdatei

Die folgende Konfigurationsdatei erstellt einen neuen Domänencontroller und Domänengesamtstruktur und stellt einen Cluster HPC Pack 1 Headknoten mit lokalen Datenbanken und 10 Linux Compute-Knoten. Die Clouddienste werden direkt an die Ostasien erstellt. Linux Compute-Knoten werden in 2 Clouddienste und 2 Speicherkonten (d. h. _MyLnxCN-0001_ bis _MyLnxCN-0005_ in _MyLnxCNService01_ und _mylnxstorage01_) und _MyLnxCN-0006_ , _MyLnxCN-0010_ in _MyLnxCNService02_ und _mylnxstorage02_erstellt. Compute-Knoten werden aus einem OpenLogic CentOS Version 7.0 Linux Bild erstellt. 

Ersetzen durch eigene Werte für Ihr Abonnement und die Konto- und Service-Namen.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Problembehandlung

* **Fehler "VNet nicht vorhanden"** - HPC Pack IaaS Bereitstellungsskript zum Bereitstellen von mehreren Clustern gleichzeitig unter mehrere Abonnements in Azure ausgeführt möglicherweise mindestens eine Bereitstellung mit dem Fehler "VNet *VNet\_Namen* ist nicht vorhanden".
Tritt dieser Fehler erneut das Skript für die Bereitstellung fehlgeschlagen.

* **Problem Internetzugriff von Azure virtual Network** - Erstellen eines Clusters HPC Pack mit einem neuen Domänencontroller mithilfe des Bereitstellungsskripts oder manuell Headknoten VM zum Domänencontroller heraufstufen, können Sie die VMs in Azure virtuelles Netzwerk mit dem Internet Probleme auftreten. Dies kann auftreten, wenn ein Weiterleitung DNS-Server automatisch auf dem Domänencontroller konfiguriert und Weiterleitung DNS-Server nicht richtig aufgelöst.

    Dieses Problem melden der Domänencontroller und Entfernen der Weiterleitung-Einstellung oder einen gültige Weiterleitung DNS-Server konfigurieren. Klicken Sie hierzu im Server-Manager auf **Extras** >
    **DNS** DNS-Manager öffnen und dann auf **Weiterleitung**.
    
## <a name="next-steps"></a>Nächste Schritte

* Finden Sie Informationen zu unterstützten Linux-Distributionen Daten und Aufträge zu einem Cluster HPC Pack mit Linux Knoten berechnen [mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure beginnen](virtual-machines-linux-classic-hpcpack-cluster.md) .
* Lernprogramme, mit denen das Skript einen Cluster erstellen und Ausführen einer Linux HPC-Arbeitslast finden Sie unter:
    * [NAMD Microsoft HPC Pack auf Linux Computeknoten in Azure ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [OpenFOAM Microsoft HPC Pack auf Linux Computeknoten in Azure ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Ausführen von STAR-CCM + Microsoft HPC Pack unter Linux Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
