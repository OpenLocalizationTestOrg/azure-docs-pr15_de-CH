<properties
 pageTitle="Burst-Knoten zu einem Cluster HPC Pack hinzufügen | Microsoft Azure"
 description="Erfahren Sie, wie einen Cluster HPC Pack Azure bei Bedarf erweitern Worker-Instanzen in einem Clouddienst ausgeführt"
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
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>On-Demand "Ausbruch" Knoten zu einem Cluster HPC Pack in Azure hinzufügen



Wenn Sie einen [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) Cluster in Azure einrichten, empfiehlt können schnell die Kapazität der Cluster nach oben oder unten skalieren, ohne einen Satz von vorkonfigurierten Computeknoten VMs. Dieser Artikel veranschaulicht das Hinzufügen auf Anforderung "Ausbruch" Knoten (Arbeitskraft Instanzen in einem Clouddienst ausgeführt) als Head-Knoten in Azure computeressourcen. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Burst-Knoten][burst]

Die Schritte in diesem Artikel können Sie die Cloud-basierten HPC Pack Headknoten VM für einen Test oder Proof of Concept Bereitstellung Azure Knoten schnell hinzufügen. Die allgemeinen Schritte sind identisch mit den Schritten in Azure "Platzen" Cloud Computing hinzufügen Kapazität zu einem lokalen HPC Pack Cluster. Ein Lernprogramm finden Sie unter [Einrichten einer Hybrid compute Cluster mit Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ausführliche Leitfäden und Aspekte für den Praxiseinsatz finden Sie [in Azure Microsoft HPC Pack Burst](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Erforderliche Komponenten

* **HPC Pack Head-Knoten bereitgestellt in Azure-VM** - können eigenständige Headknoten VM oder Teil eines größeren Clusters ist. Eigenständigen Head-Knoten finden Sie [einen HPC Pack Head-Knoten in einer VM Azure bereitstellen](virtual-machines-windows-hpcpack-cluster-headnode.md). Automatisierte HPC Pack Cluster Bereitstellungsoptionen finden Sie [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters in Azure Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Verwenden Sie das [Bereitstellungsskript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) Cluster in Azure erstellen, ist die automatisierte Bereitstellung gehören Azure Burst-Knoten. Die Beispiele in diesem Artikel.

* **Azure-Abonnement** - Azure Knoten hinzufügen können Sie dieselbe Abonnement zum Head-Knoten VM bereitstellen oder ein anderes Abonnement (oder Abonnements).

* **Kerne Quota** - eventuell erhöhen Sie das Kontingent der Kerne, besonders wenn Sie mehrere Azure Knoten mit multicore bereitstellen. Kostenlos [Öffnen einer Anfrage](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) Kontingent zu erhöhen.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Schritt 1: Erstellen einer Cloud-Dienst und ein Speicherkonto für die Azure Knoten

Mit der Azure-Verwaltungsportal oder gleichwertige Werkzeuge Folgendes konfigurieren, die Ihre Azure-Knoten bereitstellen:

* Ein Azure-Clouddienst
* Ein neues Azure Storage-Konto

>[AZURE.NOTE] Nicht wiederverwenden eines vorhandenen Cloud-Dienstes in Ihrem Abonnement. 

**Hinweise**

* Konfigurieren Sie einen separaten Clouddienst für jede Azure-Vorlage, die Sie erstellen möchten. Allerdings können Sie das gleiche Speicherkonto für mehrere Knoten Vorlagen.

* Wir empfehlen, Cloud-Dienst und das Speicherkonto für die Bereitstellung in derselben Azure-Region suchen.




## <a name="step-2-configure-an-azure-management-certificate"></a>Schritt 2: Konfigurieren eines Zertifikats Azure management

Um Azure-Knoten als Datenverarbeitungsressourcen hinzuzufügen, benötigen Sie ein Zertifikat Management auf dem Head-Knoten und Upload entsprechende Azure Abonnement für die Bereitstellung verwendet.

In diesem Szenario können Sie das **Standardmäßige HPC Azure-Verwaltungszertifikat** , das HPC Pack installiert und konfiguriert automatisch auf dem Head-Knoten. Dieses Zertifikat ist für Zwecke und Proof-of-Concept-Installationen nützlich. Mit diesem, Hochladen der Datei C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer vom Headknoten VM Abonnements. **Klicken Sie zum Hochladen des Zertifikats im [klassischen Azure-Portal](https://manage.windowsazure.com)** > **Management-Zertifikate**.

Zusätzliche Optionen zum Konfigurieren der Zeugnisse finden Sie unter [Szenarien Azure-Verwaltungszertifikat für Azure Burst konfigurieren](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Schritt 3: Bereitstellen von Azure-Knoten zum cluster



Die Schritte hinzufügen und Azure-Knoten in diesem Szenario sind in der Regel identisch mit den Schritten mit einem lokalen Head-Knoten. Weitere Informationen finden Sie in den folgenden Abschnitten [Schritte zum Bereitstellen von Azure Knoten Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Eine Azure-Vorlage erstellen

* Windows HPC-Cluster Azure Knoten hinzufügen

* Starten Sie (Bereitstellung) der Azure-Knoten

Nach dem Hinzufügen und starten Sie die Knoten können sie zum Clusteraufträge ausführen.

Wenn Sie beim Azure-Knoten bereitstellen Probleme, finden Sie unter [Problembehandlung bei Installationen von Azure Knoten Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Nächste Schritte

* Eine rechenintensive Größe für Burst-Knoten finden Sie unter Hinweise Info [H-Serie und rechenintensiver eine Reihe VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Automatisch vergrößert oder verkleinert Azure Computerressourcen nach Arbeitslast Cluster, finden Sie unter [automatisch vergrößert und verkleinert Azure computeressourcen in einem Cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
