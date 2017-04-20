<properties
   pageTitle="PowerShell-Skript Windows HPC-Cluster bereitstellen | Microsoft Azure"
   description="Ausführen eines PowerShell-Skripts zum Bereitstellen eines Clusters Windows HPC Pack in Azure virtuellen Maschinen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Erstellen eines Windows cluster High Performance computing (HPC) mit HPC Pack IaaS Bereitstellungsskript

Führen Sie die Bereitstellung HPC Pack IaaS PowerShell-Skript einen vollständigen HPC-Cluster für Arbeitslasten Windows Azure virtuelle Maschinen bereitstellen. Der Cluster besteht aus einer Active Directory verknüpft Head-Knoten unter Windows Server und Microsoft HPC Pack und weitere Windows compute-Ressourcen, die Sie angeben. Bereitstellen ein Clusters HPC Pack in Azure für Linux Arbeitslasten, finden Sie unter [HPC-Linux-Cluster mit HPC Pack IaaS Bereitstellungsskript erstellen](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Eine Azure-Ressourcen-Manager-Vorlage können Sie einen HPC Pack Cluster bereitstellen. Beispiele finden Sie in der [HPC-Cluster erstellen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) und [erstellen eine HPC-Cluster mit einem Knoten berechnet](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Beispiel-Konfigurationsdateien

Ersetzen Sie in den folgenden Beispielen oder eigene Werte für Ihre Abonnement-Id und die Konto- und Service-Namen.

### <a name="example-1"></a>Beispiel 1

Die folgende Konfigurationsdatei stellt einen HPC Pack-Cluster, der Head-Knoten mit lokalen Datenbanken und fünf compute-Knoten unter dem Betriebssystem Windows Server 2012 R2. Die Clouddienste werden direkt an den Westen der USA erstellt. Der Head-Knoten fungiert als Domänencontroller für die Domänengesamtstruktur.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Beispiel 2

Die folgende Konfigurationsdatei stellt einen HPC Pack-Cluster in einer vorhandenen Domänengesamtstruktur. Der Cluster verfügt über 1 Headknoten mit lokalen Datenbanken und 12 Datenverarbeitungsknoten BGInfo VM Erweiterung angewendet.
Automatische Installation von Windows Updates ist für alle virtuellen Computer in der Domänengesamtstruktur deaktiviert. Die Clouddienste werden direkt an die Ostasien erstellt. Compute-Knoten drei Clouddienste und drei Speicherkonten erstellt: _MyHPCCN-0001_ bis _MyHPCCN-0005_ in _MyHPCCNService01_ und _mycnstorage01_; _MyHPCCN-0006_ , _MyHPCCN0010_ , _MyHPCCNService02_ und _mycnstorage02_; und _MyHPCCN-0011_ , _0012 MyHPCCN_ in _MyHPCCNService03_ und _mycnstorage03_). Compute-Knoten werden aus einem vorhandenen privaten Bild von Compute-Knoten erstellt. Automatisch vergrößern und verkleinern Service aktiviert und standardmäßig vergrößert und verkleinert Intervallen.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Beispiel 3

Die folgende Konfigurationsdatei stellt einen HPC Pack-Cluster in einer vorhandenen Domänengesamtstruktur. Der Cluster enthält eine Head-Knoten, ein Datenbankserver mit einer 500-GB-Datenträger 2 Broker Knoten das Betriebssystem Windows Server 2012 R2 und fünf Compute-Knoten unter dem Betriebssystem Windows Server 2012 R2. Cloud-Dienst MyHPCCNService in der Gruppe *MyIBAffinityGroup*, und andere Cloud-Dienste in der Gruppe Affinität *MyAffinityGroup*erstellt. HPC Job Scheduler REST API und HPC-Webportal auf dem Head-Knoten aktiviert sind.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Beispiel 4

Die folgende Konfigurationsdatei stellt einen HPC Pack-Cluster in einer vorhandenen Domänengesamtstruktur. Der Cluster verfügt über zwei Headknoten mit lokalen Datenbanken zwei Azure Knoten Vorlagen erstellt und für Azure-Vorlage _AzureTemplate1_drei Größe Mittel Azure Knoten erstellt werden. Eine Skriptdatei läuft auf dem Head-Knoten nach der Head-Knoten konfiguriert ist.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Problembehandlung


* **Fehler "VNet nicht vorhanden"** - Wenn Sie mehrere Cluster in Azure gleichzeitig unter mehrere Abonnements Bereitstellung Skript möglicherweise mindestens eine Bereitstellung mit Fehler "VNet *VNet\_Namen* ist nicht vorhanden".
Wenn dieser Fehler auftritt, führen Sie das Skript für den fehlgeschlagenen Weitergabe.

* **Problem Internetzugriff von Azure virtual Network** - erstellen Sie einen Cluster mit einem neuen Domänencontroller mithilfe des Bereitstellungsskripts oder manuell Headknoten VM zum Domänencontroller heraufstufen, können Sie die virtuellen Computer mit dem Internet Probleme auftreten. Dieses Problem kann auftreten, wenn ein Weiterleitung DNS-Server automatisch auf dem Domänencontroller konfiguriert und Weiterleitung DNS-Server nicht richtig aufgelöst.

    Dieses Problem melden der Domänencontroller und Entfernen der Weiterleitung-Einstellung oder einen gültige Weiterleitung DNS-Server konfigurieren. Klicken Sie zum Konfigurieren dieser Einstellung im Server-Manager auf **Extras** >
    **DNS** DNS-Manager öffnen und dann auf **Weiterleitung**.

* **Problem auf RDMA Netzwerk von rechenintensiven VMs** - Windows Server Compute hinzufügen oder Knoten VMs mit einer RDMA-fähig wie A8 und A9 broker, Probleme mit diesen VMs RDMA Anwendung Netzwerk auftreten. Ein möglicher Grund für dieses Problem ist die HpcVmDrivers-Erweiterung nicht ordnungsgemäß installiert ist, wenn VMs zum Cluster hinzugefügt werden. Beispielsweise kann die Erweiterung installieren Zustand fest.

    Um dieses Problem zu umgehen, zuerst den Status der Erweiterung in VMs. Wenn die Erweiterung nicht ordnungsgemäß installiert ist, entfernen Sie die Knoten aus dem HPC-Cluster und fügen Sie die Knoten wieder hinzu. Beispielsweise können Sie auf dem Head-Knoten hinzufügen HpcIaaSNode.ps1 Skript Computeknoten VMs hinzufügen.
    
## <a name="next-steps"></a>Nächste Schritte

* Führen Sie eine Test Arbeitslast im Cluster. Ein Beispiel finden Sie unter [Erste Schritte](https://technet.microsoft.com/library/jj884144)HPC Pack.

* Ein Skript für die Bereitstellung und Ausführung einer HPC-Arbeitslast finden Sie unter [fangen mit einem HPC Pack in Azure Excel und SOA-Arbeitslast ausgeführt](virtual-machines-windows-excel-cluster-hpcpack.md).

* Versuchen HPC Pack Tools zum Starten, beenden, hinzufügen und Entfernen von Compute-Knoten aus einem Cluster, den Sie erstellen. Finden Sie unter [Manage compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Einrichtung Aufträge für den Cluster von einem lokalen Computer zu senden, finden Sie unter [Senden HPC Aufträge von einem lokalen Computer zu einem Cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
