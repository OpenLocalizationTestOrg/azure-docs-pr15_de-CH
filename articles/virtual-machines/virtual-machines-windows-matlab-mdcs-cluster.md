<properties
   pageTitle="MATLAB Cluster virtuellen Maschinen | Microsoft Azure"
   description="Verwenden Sie Microsoft Azure virtuelle Computer MATLAB Distributed Computing-Servercluster führen Ihre rechenintensiven parallelen MATLAB Arbeitslasten erstellen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Erstellen Sie MATLAB Distributed Computing Servercluster auf Azure VMs 

Mit Microsoft Azure Virtual Machines können mindestens MATLAB Distributed Computing Servercluster führen Ihre rechenintensiven parallelen MATLAB Arbeitslasten erstellen. Installieren Sie MATLAB Distributed Computing Serversoftware auf einer VM als Basis-Image und einer Azure Schnellstart Vorlage verwenden oder Azure PowerShell-Skript (verfügbar auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) bereitstellen und Verwalten des Clusters. Verbinden Sie nach der Bereitstellung des Clusters Workloads ausgeführt. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>MATLAB Distributed Computing Server zu MATLAB 

Für technische und wissenschaftliche Probleme ist die [MATLAB](http://www.mathworks.com/products/matlab/) -Plattform optimiert. MathWorks Parallel computing Produkte können MATLAB Benutzer Großräumige Simulationen mit Daten Verarbeitungsaufgaben rechenintensive Arbeitslasten beschleunigen, Compute-Clustern und Grid-Dienste nutzen. [Parallel Computing Toolbox](http://www.mathworks.com/products/parallel-computing/) können MATLAB Benutzer parallelisieren Applikationen nutzen Multi-Kern-Prozessoren GPUs und Computeclustern. [MATLAB Distributed Computing Server](http://www.mathworks.com/products/distriben/) ermöglicht MATLAB viele Computer in einem Computecluster verwenden. 


Mithilfe von Azure VMs können Sie MATLAB Distributed Computing Servercluster erstellen, die dieselben Mechanismen zur parallelen Arbeit lokal Clustern wie interaktive Aufträge, Stapelverarbeitungen, unabhängige Aufgaben und Kommunikation Aufgaben übermitteln. Mit Azure in Verbindung mit der MATLAB-Plattform hat viele Vorteile im Vergleich zu Bereitstellung und mit herkömmlichen lokalen Hardware: ein Bereich des virtuellen Computers Größen, Erstellung von Clustern bei Bedarf so Sie nur für die Serverressourcen Sie Zahlen, und die Ebene testen.  

## <a name="prerequisites"></a>Erforderliche Komponenten

* **Client-Computer** – Sie benötigen ein Windows-basierter Client zur Kommunikation mit Azure und MATLAB Distributed Computing Servercluster nach der Bereitstellung. 

* **Azure PowerShell** – Siehe [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) auf dem Clientcomputer installieren. 

* **Azure-Abonnement** - Wenn Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) in wenigen Minuten erstellen. Größere Cluster sollten Sie Bedarfsbasis Abonnement oder andere Optionen. 

* **Kerne Quota** - müssen Core Kontingent Bereitstellung großer Cluster oder mehrere MATLAB Distributed Computing Servercluster zu erhöhen. Kostenlos [Öffnen einer Anfrage](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) Kontingent zu erhöhen. 

* **MATLAB Parallel Computing Toolbox und MATLAB Distributed Computing Serverlizenzen** - Skripts davon aus, dass [MathWorks gehostet License Manager](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) alle Lizenzen verwendet wird.  

* **MATLAB Distributed Computing Serversoftware** - installiert werden auf einer VM als Basis VM-Image für den Cluster virtuellen Computern verwendet werden. 


## <a name="high-level-steps"></a>High-Level-Schritte

Verwendung von Azure virtuelle Computer für die MATLAB Distributed Computing-Servercluster sind die folgenden allgemeinen Schritte erforderlich. Einzelheiten sind in den Begleitpapieren Schnellstart Vorlagen und Skripts auf [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Erstellen eines Basisabbilds VM**  
    * Downloaden Sie und installieren Sie MATLAB Distributed Computing Serversoftware auf diesem virtuellen Computer. 

    >[AZURE.NOTE]Dieser Vorgang kann ein paar Stunden dauern, aber nur einmal für jede Version von MATLAB führen müssen.   
    
2. **Ein oder mehrere Cluster erstellen**  
    * Verwenden Sie angegebene PowerShell-Skript oder die Schnellstart-Vorlage zum Erstellen eines Clusters aus dem Basisabbild VM.   
    * Die Verwaltung der Cluster mit dem angegebenen PowerShell-Skript ermöglicht Sie auflisten, anhalten, fortsetzen und Cluster löschen. 
 
## <a name="cluster-configurations"></a>Cluster-Konfigurationen 

Derzeit können Cluster Skripts und Vorlage eine einzelne MATLAB Distributed Computing Servertopologie erstellen. Wenn Sie möchten, erstellen Sie weitere Cluster mit jeder Cluster eine andere Anzahl von Arbeitskraft VMs mit VM Größen usw. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB und die Clusterverwaltung in Azure 

MATLAB Clientknoten, MATLAB Objektaufrufplaner Knoten und MATLAB Distributed Computing Server "Arbeitskraft" Knoten werden alle als Azure VMs in einem virtuellen Netzwerk konfiguriert wie in der folgenden Abbildung dargestellt. 

![Cluster-Topologie](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Verwendung des Clusters werden mit Remotedesktop auf den Clientknoten. Clientknoten wird den MATLAB-Client. 

* Clientknoten hat eine Dateifreigabe alle Arbeitskräfte zugreifen kann.

* MathWorks gehostet License Manager wird für die Lizenz prüft alle MATLAB-Software verwendet. 

* Standardmäßig eine MATLAB Distributed Computing Server Arbeitskraft pro Kern auf die Arbeitskraft VMs erstellt, aber Sie können eine beliebige Anzahl. 


## <a name="use-an-azure-based-cluster"></a>Einen Azure-basierten Cluster verwenden 

Wie müssen mit anderen MATLAB Distributed Computing Serverclustern mit der Cluster-Profil-Manager in der MATLAB-Client (auf dem Client VM) Profil Cluster MATLAB Objektaufrufplaner.

![Cluster-Profil-Manager](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Nächste Schritte

* Detaillierte Informationen zum Bereitstellen und Verwalten von MATLAB Distributed Computing Serverclustern in Azure finden Sie unter [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) Repository mit Vorlagen und Skripts. 

* Rufen Sie die [MathWorks Website](http://www.mathworks.com/) ausführliche Dokumentation für MATLAB und MATLAB Distributed Computing Server.
