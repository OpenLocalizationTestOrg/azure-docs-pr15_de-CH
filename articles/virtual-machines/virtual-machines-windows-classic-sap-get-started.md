<properties
   pageTitle="Virtuelle Windows-Maschinen mit SAP | Microsoft Azure"
   description="Zur Verwendung von SAP auf Windows virtuelle Maschinen (VMs) in Microsoft Azure löschen"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Verwenden von SAP auf virtuelle Windows-Maschinen in Azure

Cloud Computing ist ein weit verbreiteter Begriff, der von kleinen Unternehmen bis zu großen multinationalen Unternehmen immer mehr Bedeutung der IT-Branche gewinnt. Microsoft Azure ist der Cloud-Services-Plattform von Microsoft bietet ein breites Spektrum an neue. Jetzt können Kunden schnell bereitstellen und deaktivieren Applikationen als Cloud-Dienste nicht auf technische oder Budgetierung eingeschränkt sind. Statt Zeit und Geld in Hardware-Infrastruktur, können Unternehmen die Anwendung Geschäftsprozesse und Vorteile für Kunden und Benutzer konzentrieren.

Mit Microsoft Azure virtuellen Maschinen bietet Microsoft eine umfassende Infrastruktur als Service (IaaS) Plattform. SAP NetWeaver-basierte Programme werden auf Azure Virtual Machines (IaaS) unterstützt. Die folgenden Whitepapers beschrieben planen und Implementieren von SAP NetWeaver-basierte Programme auf Windows-VMs in Azure. SAP NetWeaver-basierte Anwendung implementieren Sie auf [virtuelle Linux-Computer](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>Sapnetweaver Azure - HA

Titel: Sapnetweaver auf Azure - Clustering SAP ASC-SCS-Instanzen mit Windows-Failovercluster auf Azure mit SIOS DataKeeper

Zusammenfassung: "dieses Dokument beschreibt die SIOS DataKeeper verwenden, um eine Konfiguration mit hoher Verfügbarkeit SAP ASC/SCS Azure einzurichten. SAP schützt ihre zentrale Komponenten Fehler wie SAP ASC/SCS oder Enqueue Replikationsdienste Windows Server Failover-Cluster-Konfigurationen, die freigegebene Datenträger erfordern. Diese SAP-Komponenten sind für die Funktionalität des SAP-Systems. Daher muß Hochverfügbarkeits-Funktion soll sicherstellen, dass diese Komponenten ein Ausfall eines Servers oder einer VM wie Windows-Cluster-Konfigurationen für bare Metal und Hyper-V-Umgebungen bewältigen. Ab August 2015 kann nicht Azure selbst bieten freigegebene Datenträgern für Windows erforderlich wäre hochverfügbare Konfigurationen für diese wichtigen SAP-Komponenten erforderlich. Mit Hilfe der DataKeeper von SIOS, können jedoch Windows Server Failover-Cluster-Konfigurationen Bedarf für SAP ASC/SCS Azure IaaS-Plattform erstellt werden. Dieses Dokument beschreibt eine Schritt-für-Schritt-Ansatz Windows Server Failover-Clusterkonfiguration mit SIOS Datakeeper in Azure bereitgestellten freigegebenen Datenträger installieren. Das Papier erläutert Details in Konfigurationen auf Azure, Windows und SAP macht Hochverfügbarkeitskonfiguration optimal zu funktionieren. Das Papier ergänzt die SAP-Installationsdokumentation und Hinweise die primären Ressourcen für Installation und Bereitstellung von SAP-Software auf angegebenen Plattformen darstellen.

Aktualisiert: August 2015

[Dieses Handbuch herunterladen](http://go.microsoft.com/fwlink/?LinkId=613056)
