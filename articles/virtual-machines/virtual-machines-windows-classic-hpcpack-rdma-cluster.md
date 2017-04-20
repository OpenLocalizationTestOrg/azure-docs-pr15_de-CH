<properties
 pageTitle="Einrichten eines Clusters Windows RDMA MPI-Anwendung ausführen | Microsoft Azure"
 description="Informationen Sie zum Erstellen eines Clusters Windows HPC Pack mit H16r, H16mr, A8 und A9 VMs mit Azure RDMA Netzwerk MPI-apps ausführen."
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
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Einrichten eines Clusters RDMA Windows HPC Pack MPI-Anwendung ausführen

Soll ein Cluster RDMA Windows Azure [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) mit [H-Serie oder rechenintensive eine Reihe Instanzen](virtual-machines-windows-a8-a9-a10-a11-specs.md) parallel Message Passing Interface (MPI) ausgeführt. Beim Einrichten von RDMA-fähig, Windows Server-basierten Knoten in einem Cluster HPC Pack kommunizieren MPI-Applikationen effizient über niedrige Latenz hohen Durchsatz Netzwerk in Azure remote direct Memory Access (RDMA) Technologie basiert.

MPI-Arbeitslasten auf Linux VMs ausgeführt, die den Netzwerkzugriff Azure RDMA, finden Sie unter [Einrichten eines Clusters Linux RDMA MPI-Anwendung ausgeführt](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack Cluster-Bereitstellungsoptionen
Microsoft HPC Pack ist ein Tool kostenlos zu HPC lokal Clustern oder in Azure, Windows oder Linux HPC Applikationen auszuführen. HPC Pack enthält eine Runtime-Umgebung für die Microsoft-Implementierung der Nachricht übergeben Schnittstelle für Windows (MS MPI). Bei RDMA-fähig Instanzen einer unterstützten Windows Server-Betriebssystem bietet HPC Pack effiziente Möglichkeit Windows MPI-Programme ausführen, die den Azure RDMA Netzwerkzugriff. 

Dieser Artikel stellt zwei Szenarien und Links detaillierte Anleitung zum Einrichten eines Clusters Winodws RDMA Microsoft HPC Pack. 

* Szenario 1. Bereitstellen von rechenintensiven Arbeitskraft Instanzen (PaaS)

* Szenario 2. Bereitstellung von Compute-Knoten auf rechenintensive VMs (IaaS)

Allgemeine Komponenten mit rechenintensiven Instanzen mit Windows finden Sie unter [H-Serie und rechenintensiver eine Reihe VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Szenario 1. Bereitstellen von rechenintensiven Arbeitskraft Instanzen (PaaS)

Fügen Sie aus einem vorhandenen Cluster HPC Pack zusätzliche Serverressourcen in Azure Worker-Instanzen (Azure Knoten) im Cloud-Service (PaaS) ausgeführt hinzu. Diese Funktion wird auch als "Ausbruch Azure" HPC Pack unterstützt Größen für Instanzen der Arbeitskraft. Beim Hinzufügen von Azure Knoten geben Sie einfach RDMA-fähig Größen.

Es folgen Hinweise und Arbeitsschritte RDMA-fähig brach Azure Instanzen einer vorhandenen (normalerweise lokal) Cluster. Verwenden Sie ähnliche Verfahren in Azure-VM bereitgestellt wird eine HPC Pack Headknoten Arbeitskraft Instanzen hinzufügen.

>[AZURE.NOTE] Ein Azure HPC Pack Platzen finden Sie unter [Einrichten eines Clusters Hybrid mit HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Beachten Sie die Punkte in den Schritten, die speziell für RDMA-fähig gelten Azure-Knoten.

![Azure Burst][burst]

### <a name="steps"></a>Schritte

4. **Bereitstellen und Konfigurieren eines HPC Pack 2012 R2 Head-Knotens**

    Downloaden Sie das neueste HPC Pack-Installationspaket aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922). Vorschriften und Anleitung zur Vorbereitung einer Azure Burst-Bereitstellung finden Sie unter [HPC Pack Getting Started Guide](https://technet.microsoft.com/library/jj884144.aspx) und [Burst Azure Worker-Instanzen mit Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Konfigurieren eines Zertifikats Management in Azure-Abonnement**

    Konfigurieren Sie ein Zertifikat, um die Verbindung zwischen dem Head-Knoten und Azure. Optionen und Verfahren finden Sie unter [Szenarien Azure-Verwaltungszertifikat für HPC Pack konfigurieren](http://technet.microsoft.com/library/gg481759.aspx). Test Bereitstellung installiert HPC Pack standardmäßig Microsoft HPC Azure Verwaltungszertifikat schnell zu Ihrem Abonnement Azure hochladen können.

6. **Erstellen Sie einen neuen Clouddienst und ein Speicherkonto**

    Verwenden der Azure-Verwaltungsportal zu einem Clouddienst und ein Speicherkonto für die Bereitstellung in einer Region, RDMA-fähig Instanzen verfügbar sind.

7. **Eine Azure-Vorlage erstellen**

    Verwenden der Knoten Vorlagenassistenten HPC Cluster Manager erstellen. Finden Sie unter [Erstellen einer Azure-Vorlage](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) in "Schritte zum Bereitstellen von Azure Knoten mit Microsoft HPC Pack".

    Anfängliche Tests empfehlen wir Konfigurieren einer Richtlinie manuelle Verfügbarkeit in der Vorlage.

8. **Hinzufügen von Knoten zum cluster**

    Mit dem Assistenten in HPC Cluster Manager hinzufügen. Weitere Informationen finden Sie unter [Windows HPC-Cluster Azure Knoten hinzufügen](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Bei der Angabe der Größe der Knoten wählen Sie RDMA-fähig Instanz Größen.
    
    >[AZURE.NOTE]In jede Azure-Bereitstellung mit der rechenintensiven Burst bereit HPC Pack automatisch mindestens 2 RDMA-fähig Instanzen (wie A8) als Proxy Knoten neben Azure Arbeitskraft Rolleninstanzen angegebenen Die Proxy-Knoten verwenden Kerne, die das Abonnement zugeordnet und mit Azure Arbeitskraft Rolleninstanzen anfallen.

9. **Start (bereitstellen) Knoten und online-Jobs ausgeführt**

    Wählen Sie die Knoten und **Startaktion** HPC Cluster Manager. Nach Abschluss der Bereitstellung die Knoten und **Online schalten** Aktion HPC Cluster Manager verwenden. Die Knoten können Aufträge ausgeführt werden.

10. **Aufträge für den cluster**

    Verwenden Sie HPC Pack Auftrag senden Cluster Auftrag ausführen. Siehe [Microsoft HPC Pack: Job Management](http://technet.microsoft.com/library/jj899585.aspx).

11. **Beenden (Identitätsintegrationsprodukts) Knoten**

    Ausgeführte Aufträge benötigen Knoten offline und die **Beenden** -Aktion im HPC-Cluster-Manager verwenden.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Szenario 2. Bereitstellung von Compute-Knoten auf rechenintensive VMs (IaaS)

In diesem Szenario den Headknoten HPC Pack bereitstellen und Cluster compute-Knoten auf ein Active Directory-Domäne in Azure virtual Network VMs. HPC Pack bietet eine Reihe von [Bereitstellungsoptionen in Azure VMs](virtual-machines-linux-hpcpack-cluster-options.md), einschließlich automatisierter Bereitstellungsskripts und Azure Schnellstart-Vorlagen. Beispielsweise führen Aspekte und Schritte, [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) verwenden, um die meisten dieser Prozess automatisieren.

![Cluster in Azure VMs][iaas]



### <a name="steps"></a>Schritte

1. **Erstellen Sie einen Cluster-Head-Knoten und berechnen Knoten VMs mit HPC Pack IaaS Bereitstellungsskripts auf einem Clientcomputer**

    HPC Pack IaaS Bereitstellungsskript Paket im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922)herunterladen.

    Vorbereitung auf den Clientcomputer erstellen Sie die Skript-Konfigurationsdatei und führen Sie das Skript, [HPC-Cluster mit HPC Pack IaaS Bereitstellungsskript erstellen](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    RDMA-fähig Compute-Knoten bereitstellen, beachten Sie folgende zusätzlichen Informationen:
    
    * **Virtuelle Netzwerk** - Geben Sie ein neues virtuelles Netzwerk in einer Region mit die RDMA-fähig gewünschte Größe verfügbar ist.

    * **Windows Server-Betriebssystem** - RDMA-Konnektivität unterstützen, eine Windows Server 2012 R2 oder Windows Server 2012 Betriebssystem für den Computeknoten VMs.

    * **Cloud-Services** – wir empfehlen die Bereitstellung der Head-Knoten in einem Cloud-Dienst und der Compute-Knoten in einem anderen Clouddienst.

    * **Größe der Head-Knoten** - für dieses Szenario sollten Sie eine Größe von mindestens A4 (extragroß) für den Head-Knoten.

    * **HpcVmDrivers Erweiterung** - Bereitstellungsskript wird Azure VM-Agent und die Erweiterung HpcVmDrivers bei automatisch Größe A8 und A9 berechnen Knoten mit einem Windows Server-Betriebssystem bereitstellen. HpcVmDrivers installiert Treiber auf dem Computeknoten VMs, damit RDMA-Netzwerk herstellen können.

    * **Cluster-Netzwerkkonfiguration** - Bereitstellungsskript wird automatisch HPC Pack Cluster in Topologie 5 (alle Knoten im Unternehmensnetzwerk). Diese Topologie ist erforderlich für alle HPC Pack Clusterbereitstellungen VMs. Ändern Sie die Topologie des Cluster-Netzwerks nicht später.

2. **Bringen Sie die Compute-Knoten online Aufträge ausführen**

    Wählen Sie die Knoten und **Online schalten** Aktion HPC Cluster Manager. Die Knoten können Aufträge ausgeführt werden.

3. **Aufträge für den cluster**

    Verbinden mit dem Head-Knoten Aufträge senden oder einen lokalen Computer dazu eingerichtet. Informationen finden Sie unter [Senden Aufträge einem HPC-Cluster in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Die Knoten offline und (Freigeben) sie**

    Nach erfolgter ausgeführte Aufträge, Knoten offline-HPC Cluster Manager nutzen. Verwenden Sie dann Azure Managementtools diese beenden.



## <a name="run-mpi-applications-on-the-cluster"></a>MPI-Anwendung auf dem Cluster ausgeführt

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Beispiel: Führen Sie Mpipingpong in einem Cluster HPC Pack

Überprüfen eine Bereitstellung HPC Pack Instanzen RDMA-fähig, Befehl das HPC Pack **Mpipingpong** im Cluster. **Mpipingpong** sendet Pakete zwischen gepaarten Knoten wiederholt Latenz und Durchsatz Messungen berechnen und Statistiken für das Netzwerk RDMA-fähigen Anwendung. Dieses Beispiel zeigt ein normale Muster zum Ausführen eines Auftrags MPI (in diesem Fall **Mpipingpong**) mit dem Cluster **Mpiexec** -Befehl.

Angenommen, dass Sie eine "Azure" Aufteilungskonfiguration ([Szenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article)Azure-Knoten hinzugefügt Wenn Sie in einem Cluster Azure VMs HPC Pack bereitgestellt, müssen Sie die Befehlssyntax Geben Sie einen anderen Knoten und zusätzliche Umgebungsvariablen Netzwerkverkehr RDMA Netzwerk direkt ändern.


Mpipingpong auf dem Cluster ausgeführt:


1. Öffnen Sie auf dem Head-Knoten oder auf einem ordnungsgemäß konfigurierten Clientcomputer ein Eingabeaufforderungsfenster.

2. Geben Sie den folgenden Befehl zum Senden eines Auftrags zum Ausführen von Mpipingpong mit einer kleinen Paketgröße und eine große Anzahl von Iterationen Schätzung Wartezeit zwischen Knoten in einer Bereitstellung Azure Burst 4 Knoten:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Der Befehl gibt die ID des Auftrags, der übermittelt wird.

    HPC Pack Cluster bereitgestellt auf Azure VMs geben eine Knotengruppe enthält bereitgestellten compute-Knoten in einem einzelnen Clouddienst VMs bereitgestellt und **Mpiexec** -Befehl wie folgt ändern:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Bei Beendigung des Auftrags zum Anzeigen der Ausgabe (in diesem Fall die Ausgabe der Aufgabe 1 des Auftrags), geben Sie Folgendes ein

    ```
    task view <JobID>.1
    ```

    wo &lt; *JobID* &gt; ist die ID des Auftrags, der gesendet wurde.

    Die Ausgabe wird Wartezeit Ergebnisse ähnlich der folgenden enthalten.

    ![Ping-Pong-Wartezeit][pingpong1]

4. Geben Sie den folgenden Befehl zum Senden eines Auftrags zum Ausführen von **Mpipingpong** mit großen Paketgröße und eine kleine Anzahl von Iterationen Schätzung Durchsatz zwischen Azure Burst-Knoten:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Der Befehl gibt die ID des Auftrags, der übermittelt wird.

    Ein HPC Pack-Cluster auf Azure VMs bereitgestellt ändern Sie den Befehl in Schritt 2 notiert haben.

5. Bei Beendigung des Auftrags zum Anzeigen der Ausgabe (in diesem Fall die Ausgabe der Aufgabe 1 des Auftrags), geben Sie Folgendes ein:

    ```
    task view <JobID>.1
    ```

  Die Ausgabe wird Durchsatz Ergebnisse ähnlich der folgenden enthalten.

  ![Ping-Pong-Durchsatz][pingpong2]


### <a name="mpi-application-considerations"></a>MPI-Anwendung Aspekte


Folgenden werden Aspekte der MPI-Applikationen mit HPC Pack in Azure ausgeführt. Einige gelten nur für Installationen von Azure Knoten (Arbeitskraft Instanzen in einer "Azure" Aufteilungskonfiguration hinzugefügt).

* Arbeitskraft-Instanzen in einem Clouddienst werden regelmäßig ohne Vorankündigung von Azure ausgebessert (z.B. Wartung oder bei Ausfall eine Instanz). Wenn eine Instanz während der Ausführung eines Auftrags MPI ausgebessert ist, wird die Instanz Daten verloren und bei der ersten Bereitstellung wurde MPI Auftrag ein Fehler verursacht, in den Zustand zurück. Weitere Knoten, für einen einzelnen Auftrag MPI und je länger das Projekt verwenden wird, wahrscheinlicher, dass eine der Instanzen während eines Auftrags ausgebessert läuft. Dies ist auch sollten, wenn Sie einen einzelnen Knoten in der Bereitstellung als Dateiserver festlegen.


* Führen Sie MPI Aufträge in Azure müssen Sie RDMA-fähig Instanzen verwenden. Die können eine Größe von HPC Pack unterstützt wird. Allerdings sollten RDMA-fähig Instanzen für relativ große hängen von der Latenz und Bandbreite des Netzwerks, die die Knoten miteinander verbindet MPI-Jobs ausgeführt. Verwenden Sie andere Größen Latenz und Bandbreite empfindlich MPI Auftrag ausführen, empfiehlt kleine Aufträge, in denen eine Aufgabe nur wenige Knoten ausgeführt wird.

* Azure Instanzen bereitgestellt sind Gegenstand Lizenzierung mit der Anwendung verknüpft. Überprüfen Sie mit dem Kreditor kommerzielle Anwendung Lizenzierung oder andere Einschränkungen für die Ausführung in der Cloud. Nicht alle Hersteller bieten Pay-Lizenzierung.


* Azure-Instanzen benötigen weitere Setup Zugriff auf lokale Knoten, Freigaben und Lizenzserver. Um Azure Knoten auf einem lokalen Lizenzserver aktivieren, können Sie z. B. ein Azure virtuelles Standort-zu-Standort-Netzwerk konfigurieren.


* Führen Sie MPI-Anwendung in Azure Instanzen registrieren Sie jede MPI-Anwendung mit Windows-Firewall für die Instanzen durch Ausführen des Befehls **Hpcfwutil** . Dadurch MPI-Kommunikation zu einem Port, die von der Firewall dynamisch zugewiesen wird.

    >[AZURE.NOTE] Burst Azure Bereitstellungen können Sie auch konfigurieren einen Firewall Ausnahme Befehl automatisch auf alle neuen Azure-Knoten, die dem Cluster hinzugefügt werden. Führen Sie den Befehl **Hpcfwutil** und stellen Sie sicher, dass die Anwendung fügen Sie den Befehl ein Startskript Azure Knoten hinzu. Weitere Informationen finden Sie unter [Verwenden eines Skripts zum Starten für Azure-Knoten](https://technet.microsoft.com/library/jj899632.aspx).



* HPC Pack mithilfe der Umgebungsvariablen CCP_MPI_NETMASK Cluster ein Adressbereich akzeptabel für MPI-Kommunikation an. HPC Pack 2012 R2 ab, die Umgebungsvariable CCP_MPI_NETMASK Cluster wirkt sich nur auf MPI Kommunikation zwischen Domäne Compute Clusterknoten (entweder lokal oder in Azure VMs). Die Variable wird von Knoten in einem Azure Konfiguration hinzugefügt ignoriert.


* MPI-Aufträge können Azure Instanzen ausführen, die in andere Cloud-Dienste (z. B. in Burst Azure Bereitstellungen mit anderen Knoten Vorlagen oder VM Azure Compute-Knoten in mehrere Clouddienste bereitgestellt) bereitgestellt werden. Haben Sie mehrere Knoten Azure-Bereitstellung, die mit anderen Knoten Vorlagen gestartet werden, müssen MPI-Projekt auf nur einen Satz von Azure Knoten ausführen.


* Azure-Knoten zum Cluster hinzufügen und online schalten versucht der HPC-Auftragsplanungsdienst sofort Aufträge auf den Knoten starten. Nur ein Teil der Arbeitslast auf Azure ausgeführt werden kann, daß aktualisieren oder Auftrag Vorlagen zum definieren, welche Einzelvorgangstypen in Azure ausgeführt werden können. Beispielsweise um sicherzustellen, dass nur Aufträge mit einer Stellenvorlage auf Azure-Knoten ausgeführt, die Auftragsvorlage fügen Sie Knotengruppen Eigenschaft hinzu, und wählen Sie AzureNodes als der erforderliche Wert. Erstellen Sie benutzerdefinierte Gruppen für Ihre Azure-Knoten verwenden Sie das Hinzufügen HpcGroup HPC PowerShell-Cmdlet.


## <a name="next-steps"></a>Nächste Schritte

* Als Alternative zur Verwendung von HPC Pack entwickeln Sie mit dem Azure Batch-Dienst auf verwalteten Pools von Computeknoten in Azure MPI-Programme ausführen. [Verwendung mit mehreren Instanzen Message Passing Interface (MPI) Anwendung in Azure Batch auszuführenden Aufgaben](../batch/batch-mpi.md)anzeigen

* Linux MPI Programme ausführen, die den Netzwerkzugriff Azure RDMA finden Sie in der [Einrichten eines Clusters Linux RDMA MPI-Anwendung ausgeführt](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png