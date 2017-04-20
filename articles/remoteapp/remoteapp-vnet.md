
<properties
    pageTitle="Überprüfen der Azure-VNET mit Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Sie sicherstellen, dass Ihr VNET Azure Azure RemoteApp verwenden kann"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Überprüfen der Azure-VNET mit Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Vor der Verwendung einer Azure VNET Azure RemoteApp möchten Sie das VNET überprüfen. Dadurch werden Probleme mit Konnektivität.

Überprüfen Sie Ihre Azure-VNET folgendermaßen Sie vor:

1. Erstellen Sie einen Azure virtuellen Computer innerhalb des Subnetzes Azure VNET Azure RemoteApp verwenden möchten.

2. Mit diesen virtuellen Computer mithilfe der Option **Verbindung** im Verwaltungsportal.
3. Fügen Sie den virtuellen Computer derselben Domäne Azure RemoteApp verwenden möchten. Wenn Hybrid-Auflistung, die mit Ihrem lokalen Netzwerk verbinden erstellen, verknüpfen Sie virtuellen Computer der lokalen Domäne.

Wenn dies erfolgreich ist, ist der Azure-VNET mit RemoteApp bereit.

Weitere Informationen zum End-to-End-Hybrid Auflistung Workflow finden Sie in folgenden Artikeln:

- [Virtuelle Netzwerk für Azure RemoteApp planen](remoteapp-planvnet.md)
- [Erstellen einer Hybrid-Auflistung](remoteapp-create-hybrid-deployment.md)
- [Bereitstellen Sie Azure RemoteApp-Sammlung für Ihre Azure Virtual Network (mit Unterstützung für ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
