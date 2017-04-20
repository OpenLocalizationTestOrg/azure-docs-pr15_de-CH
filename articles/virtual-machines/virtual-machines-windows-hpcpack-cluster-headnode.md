<properties
 pageTitle="Erstellen einer HPC Pack Headknoten in Azure-VM | Microsoft Azure"
 description="Informationen Sie zum Azure-Portal und Bereitstellungsmodell Ressourcen-Manager verwenden, um Microsoft HPC Pack Headknoten in Azure-VM erstellen."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Erstellen Sie den Head-Knoten eines Clusters HPC Pack in einer VM Azure Marketplace-Bild


Verwenden eines [Microsoft HPC Pack VM Image](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) von Azure Marketplace und Azure-Portal Headknoten HPC-Cluster erstellen. Diese HPC Pack VM-Image basiert auf Windows Server 2012 R2 Datacenter HPC Pack 2012 R2 Update 3 vorinstalliert. Verwenden Sie Head-Knoten einen Nachweis Konzept Bereitstellung von HPC Pack in Azure. Sie können dann Compute-Knoten zum Cluster HPC-Arbeitslasten ausführen hinzufügen.



>[AZURE.TIP]Um eine vollständige HPC Pack Cluster in Azure bereitstellen, Head-Knoten und Compute-Knoten enthält, wird empfohlen, eine automatisierte Methode verwenden. Das [Bereitstellungsskript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) und [HPC Pack für Windows Arbeitslasten Cluster](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) Resource Manager Vorlage gehören. Weitere Vorlagen finden Sie unter [HPC Pack Cluster Optionen in Azure](virtual-machines-windows-hpcpack-cluster-options.md) . 


## <a name="planning-considerations"></a>Hinweise zur Planung

Wie in der folgenden Abbildung dargestellt, stellen Sie den HPC Pack Head-Knoten in einer Active Directory-Domäne in Azure virtual Network.

![HPC Pack Head-Knoten][headnode]

* **Active Directory-Domäne** - das HPC Pack Head-Knoten Active Directory-Domäne in Azure verknüpft werden muss, bevor der HPC-Services auf dem virtuellen Computer zu starten. Wie in diesem Artikel Nachweis Konzept Bereitstellung können Sie die VM Heraufstufen der Head-Knoten als Domänencontroller für erstellen vor dem Starten der HPC-Dienste. Eine weitere Option ist einen eigenen Domänencontroller und Gesamtstruktur in Azure, den Headknoten VM beitreten, bereitstellen.

* **Azure virtual Network** - Verwendung das Ressourcen-Manager-Bereitstellungsmodell zum Bereitstellen des Head-Knotens angeben oder ein Azure virtuelles Netzwerk erstellen. Verwenden Sie das virtuelle Netzwerk, wenn den Head-Knoten Active Directory-Domäne beitreten müssen. Sie benötigen sie später für VMs Compute-Knoten zum Cluster hinzufügen.

    
## <a name="steps-to-create-the-head-node"></a>Schritte zum Erstellen des Head-Knotens

Allgemeine Schritte zur Azure-Portal eine Azure VM für HPC Pack Head-Knoten mithilfe des Ressourcen-Manager-Bereitstellungsmodells folgen. 


1. Wenn Sie eine neue Active Directory-Gesamtstruktur mit separaten Domänencontroller VMs in Azure erstellen möchten, ist eine Option ein [Ressourcen-Manager-Vorlage](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/)verwenden. Für einfache Beweis Konzept Bereitstellung ist für diesen Schritt auslassen und VM selbst den Head-Knoten als Domänencontroller konfigurieren. Diese Option wird später in diesem Artikel beschrieben.
    
2. [HPC Pack 2012 R2 Windows Server 2012 R2 auf](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Marketplace klicken Sie auf **virtuellen Computer erstellen**. 

3. Wählen Sie im Portal auf **HPC Pack 2012 R2 Windows Server 2012 R2** **Ressourcenmanager** Bereitstellungsmodell aus und dann auf **Erstellen**.

    ![HPC Pack Images][marketplace]

4. Verwenden Sie das Portal das Konfigurieren und erstellen Sie den virtuellen Computer. Wenn Sie neu in Azure, folgen Sie der Anleitung [Windows virtuellen Computer in Azure-Portal erstellen](virtual-machines-windows-hero-tutorial.md). Für Beweis Konzept Bereitstellung können in der Regel den Standardpfad oder empfohlene Einstellung.

    >[AZURE.NOTE]Ggf. an der Head-Knoten zu einer vorhandenen Active Directory-Domäne in Azure sicherstellen Sie, dass das virtuelle Netzwerk für diese Domäne angeben, wenn die VM erstellen.
       
4. Nach dem Erstellen des virtuellen Computer und die VM läuft, Remotedesktop [Verbindung zur VM](virtual-machines-windows-connect-logon.md) . 

5. Die VM zu einer vorhandenen Domänengesamtstruktur beitreten oder eine Domänengesamtstruktur auf dem virtuellen Computer selbst erstellen.

    * Erstellt die VM in ein Azure virtuelles Netzwerk mit einer vorhandenen Domäne treten Sie VM mit Server-Manager oder Windows PowerShell Standardtools für die Gesamtstruktur bei. Starten Sie neu.

    * Wenn Sie ein neues virtuelles Netzwerk (ohne eine vorhandene Domänengesamtstruktur) die VM erstellt haben, fördern Sie die VM als Domänencontroller. Gehen Sie standard zum Installieren und Konfigurieren der Active Directory-Domänendienste-Rolle auf dem Head-Knoten. Ausführliche Schritte finden Sie in [einer neuen Windows Server 2012 Active Directory-Gesamtstruktur installieren](https://technet.microsoft.com/library/jj574166.aspx).

5. Nachdem die VM und Active Directory-Gesamtstruktur angehört, starten Sie HPC Pack Dienste wie folgt:

    ein. Verbinden Sie mit Head-Knoten VM mit einem Domänenkonto, das Mitglied der lokalen Administratorengruppe ist. Verwenden Sie z. B. das Administratorkonto eingerichtet werden, wenn den Head-Knoten-VM erstellt.

    b. Starten Sie für eine Standardkonfiguration Head-Knoten Windows PowerShell als Administrator, und geben Sie Folgendes ein:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Es kann mehrere Minuten HPC Pack-Dienste werden gestartet.

    Geben Sie zusätzliche Headknoten Konfigurationsoptionen `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Nächste Schritte

* Sie können jetzt mit dem Head-Knoten des Clusters HPC Pack arbeiten. Z. B. HPC Cluster Manager starten und vollständige [Bereitstellung Aufgabenliste](https://technet.microsoft.com/library/jj884141.aspx).
* Fügen Sie möchten Sie erhöhen compute Cluster Kapazität bei Bedarf hinzu, [Azure Burst-Knoten](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) in einem Clouddienst. 

* Führen Sie eine Test Arbeitslast im Cluster. Ein Beispiel finden Sie unter [Erste Schritte](https://technet.microsoft.com/library/jj884144)HPC Pack.

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
