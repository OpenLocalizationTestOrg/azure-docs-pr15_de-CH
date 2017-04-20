<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer in VMM-Clouds zu einer sekundären VMM-Website mithilfe von PowerShell (Resource Manager) | Microsoft Azure"
    description="Beschreibt das Bereitstellen von Azure Site Recovery, Replikation, Failover und Recovery von Hyper-V virtuelle Computer in VMM-Clouds zu einer sekundären VMM-Website mithilfe von PowerShell (Resource Manager) zu koordinieren"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Replizieren von Hyper-V virtuelle Computer in VMM-Clouds zu einer sekundären VMM-Website mithilfe von PowerShell (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-vmm.md)
- [Verwaltungsportal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - Ressourcen-Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Willkommen bei Azure Site Recovery! Verwenden Sie in diesem Artikel möchten Sie lokale Hyper-V virtuelle Computer replizieren in System Center Virtual Machine Manager (VMM) Wolken an einem sekundären Standort verwaltet. 

Dieser Artikel beschreibt wie Sie PowerShell Automatisierung häufiger Aufgaben müssen Sie ausführen, wenn Sie Azure Site Recovery für Hyper-V virtuelle Computer in System Center VMM-Clouds in System Center VMM-Clouds in sekundären Standort replizieren einrichten.

Der Artikel enthält Komponenten für das Szenario und zeigt 

- Ein Depot Recovery Services einrichten
- Installieren Sie das Azure Site Recovery VMM-Quellserver und dem Zielserver VMM
- Registrieren der VMM-Server im Tresor
- Konfigurieren der Replikationsrichtlinie für die VMM-Cloud. Replikationseigenschaften in die Richtlinie gilt für alle geschützte virtuelle Maschinen 
- Aktivieren Sie Schutz für die virtuellen Computer. 
- Testen Sie das Failover von virtuellen Maschinen einzeln oder als Teil der Wiederherstellungsplan sicherstellen, dass alles wie erwartet funktioniert.
- Ausführen eines geplanten oder ungeplanten Failover von virtuellen Maschinen einzeln oder als Teil der Wiederherstellungsplan sicherstellen, dass alles wie erwartet funktioniert.

Wenn Sie dieses Szenario einrichten Probleme auftreten, stellen Sie Ihre Fragen in [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager.



## <a name="on-premises-prerequisites"></a>Lokalen Komponenten

Hier ist, was Sie in der primären und sekundären lokalen Standorten benötigen Sie dieses Szenario bereitstellen:

**Erforderliche Komponenten** | **Details** 
--- | ---
**VMM** | Wir empfehlen Sie VMM-Server am primären Standort und VMM-Server am sekundären Standort bereitstellen.<br/><br/> Sie können auch [zwischen auf einem einzelnen VMM-Server replizieren](site-recovery-single-vmm.md). Dazu benötigen Sie mindestens zwei Wolken auf dem VMM-Server konfiguriert.<br/><br/> VMM-Server sollte mindestens ausführen System Center 2012 SP1 mit den neuesten Updates.<br/><br/> Jede VMM-Server müssen in einer oder mehr Wolken konfiguriert und alle Wolken müssen des Kapazität von Hyper-V-Profils. <br/><br/>Wolken enthalten eine oder mehrere VMM-Hostgruppen.<br/><br/>Weitere Informationen zum Einrichten von VMM-Clouds in [VMM-Cloud-Struktur konfigurieren](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)und [Exemplarische Vorgehensweise: Erstellen von privaten Clouds mit System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-Server benötigen Zugriff auf das Internet. 
**Hyper-V** | Hyper-V Server laufen mindestens Windows Server 2012 mit Hyper-V-Rolle und die neuesten Updates installiert haben.<br/><br/> Hyper-V Server sollte eine oder mehrere VMs.<br/><br/>  Hyper-V-Host-Server sollte in Gruppen in der primären und sekundären VMM-Clouds befinden.<br/><br/> Wenn Sie Windows Server 2012 R2 Hyper-V in einem Cluster ausführen sollten Sie [update 2961977](https://support.microsoft.com/kb/2961977) installieren<br/><br/> Wenn Sie Hyper-V in einem Cluster auf Windows Server 2012, Cluster-Broker arbeiten ist nicht automatisch erstellt, wenn einen statischen IP-Adresse-basierten Cluster haben. Sie müssen den Cluster-Broker manuell konfigurieren. [Mehr](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Anbieter** | Während der Bereitstellung der Site Recovery installiert Azure Site Recovery Provider auf VMM-Server. Der Anbieter kommuniziert mit Site Recovery über HTTPS 443 Replikation orchestrieren. Daten-Replikation zwischen den primären und sekundären Hyper-V-Servern über das LAN oder VPN-Verbindung.<br/><br/> Der Anbieter auf dem VMM-Server benötigt Zugriff auf diese URLs: *. hypervrecoverymanager.windowsazure.com, *. AccessControl.Windows.NET; *. backup.windowsazure.com, *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Außerdem lässt Firewall Kommunikation zwischen VMM-Server und [Azure-Rechenzentrum IP-Adressbereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) und das Protokoll HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Netzwerk-Zuordnung erforderliche Komponenten
Netzwerk-Zuordnung Karten zwischen VMM VM auf die primären und sekundären VMM Server:

- Platzieren Sie optimal Replikat VMs auf sekundäre Hyper-V-Hosts nach einem Failover.
- Replikat VMs entsprechende VM-Netzwerken verbinden.
- Wenn VMs nach einem Failover mit einem Netzwerk verbunden wird nicht Netzwerk Zuordnung Replikat nicht konfigurieren.
- Möchten Sie Netzwerk ist während der Wiederherstellung Sites zuordnen Bereitstellung hier was Sie benötigen:

    - Stellen Sie sicher, dass VMs auf der Hyper-V-Host mit einem VMM VM-Netzwerk verbunden sind. Das Netzwerk sollte ein logisches Netzwerk verbunden, das der Cloud zugeordnet ist.
    - Überprüfen Sie, ob die sekundäre Cloud, die Sie für die Wiederherstellung verwenden ein entsprechende VM-Netzwerk konfiguriert wurde. Das VM-Netzwerk sollte ein logisches Netzwerk verbunden, die sekundäre Cloud zugeordnet ist.


Weitere Informationen zum Konfigurieren von Netzwerken in VMM die folgenden Artikel

- [Logische Netzwerke in VMM konfigurieren](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Konfigurieren von VM-Netzwerken und Gateways in VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Erfahren Sie mehr](site-recovery-network-mapping.md) über die Funktionsweise von Netzwerkzuordnung.

###<a name="powershell-prerequisites"></a>PowerShell-Komponenten
Achten Sie Azure PowerShell bereit. Wenn Sie PowerShell bereits verwenden, müssen Sie auf Version 0.8.10 oder höher. Informationen zum Einrichten von PowerShell finden Sie im [Handbuch zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md). Nach der Einrichtung und Konfiguration PowerShell sehen Sie alle verfügbaren Cmdlets für den Dienst [hier](https://msdn.microsoft.com/library/dn850420.aspx). 

Tipps helfen, verwenden Sie die Cmdlets wie Parameterwerte, Eingaben und Ausgaben in der Regel in Azure PowerShell Behandlung finden Sie im [Handbuch Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Schritt 1: Festlegen des Abonnements 

1. Azure PowerShell melden Sie Ihre Azure-Konto: die folgenden Cmdlets verwenden
 
        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred 
    

2. Enthält eine Liste der Abonnements. Dadurch werden auch die SubscriptionIDs für jedes Abonnement aufgeführt. Notieren Sie SubscriptionID Abonnement Recovery Services Depot erstellt werden soll    

        Get-AzureRmSubscription 

3. Legen Sie das Abonnement in der Recovery Services Depot erwähnt die Abonnement-ID erstellt werden soll

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Schritt 2: Erstellen eines Depots Recovery Services 

1. Erstellen Sie eine Azure Ressourcenmanager Ressourcengruppe, wenn Sie nicht bereits eine haben

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Erstellen Sie ein neues Depot Recovery Services und speichern Sie das erstellte ASR Vault-Objekt in einer Variablen (wird später verwendet). Sie können auch das ASR Vault Beitrag erstellen mit dem Cmdlet Get-AzureRMRecoveryServicesVault abrufen:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Festlegen des Recovery Services Vault-Kontexts

1.  Wenn ein Depot bereits erstellt haben, führen Sie den folgenden Befehl, um das Depot.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Festlegen den Vault-Kontext die folgenden Befehl.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault



## <a name="step-4-install-the-azure-site-recovery-provider"></a>Schritt 4: Installieren Sie das Azure Site Recovery

1.  Erstellen Sie auf dem VMM ein Verzeichnis mit dem folgenden Befehl:
    
        New-Item c:\ASR -type directory
        
2.  Extrahieren Sie die Dateien mit den gedownloadeten Anbieter durch Ausführen des folgenden Befehls
    
        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q

    
3.  Installieren Sie den Anbieter mit den folgenden Befehlen:
    
        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Warten Sie, bis die Installation abgeschlossen.
    
4.  Registrieren Sie den Server in das Depot mit dem folgenden Befehl:
    
        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-and-associate-a-replication-policy"></a>Schritt 5: Erstellen Sie und ordnen Sie einer Replikationsrichtlinie zu

1.  Erstellen Sie ein Hyper-V 2012 R2 Replikationsrichtlinie durch Ausführen des folgenden Befehls:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] VMM-Cloud kann Hyper-V-Hosts mit verschiedenen Versionen von Windows Server (gemäß der Hyper-V-Komponenten), aber die Replikationsrichtlinie bestimmte Betriebssystemversion ist. Haben Sie verschiedene Hosts auf verschiedene Betriebssystemversionen ausgeführt, erstellen Sie separate Replikationsrichtlinien für jede Version des Betriebssystems. Für z. B.: haben Sie fünf Server unter Windows Server 2012 und drei Windows Server 2012 R2 erstellen Sie zwei Replikations-Richtlinien – einen für jeden Betriebssystemversionen.

2.  Abrufen der primäre Schutz Container (primäre VMM-Cloud) und Recovery Protection (Recovery VMM-Cloud) mit folgenden Befehlen:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Rufen Sie in Schritt 1 mit dem Anzeigenamen der Richtlinie erstellten Richtlinien ab

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Starten Sie die Zuordnung des Containers Schutz (VMM-Cloud) mit der Replikation:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Warten auf Policy Association Auftrag abgeschlossen. Sie können überprüfen, ob der Job abgeschlossen wurde mit den folgenden PowerShell Ausschnitt.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Nachdem der Auftrag verarbeitet wurde, führen Sie den folgenden Befehl ein:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Zum Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte im [Überwachen der Aktivität](#monitor).

## <a name="step-5-configure-network-mapping"></a>Schritt 5: Konfigurieren von Netzwerkzuordnung

1. Der erste Befehl ruft Servern für den aktuellen Azure Site Recovery Tresor. Der Befehl speichert Microsoft Azure Site Recovery-Server in der Arrayvariablen $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Die folgenden Befehle Site Recovery Netzwerk für VMM Quellserver und den Zielserver VMM abrufen.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] VMM Quellserver kann den ersten oder zweiten in Server-Array. Die Namen der VMM-Server und Netzwerke entsprechend


4. Letzte Cmdlet erstellt eine Zuordnung zwischen dem primären und Recovery Netzwerk. Das Cmdlet gibt das primäre Netzwerk als erstes Element von $PrimaryNetworks und die Wiederherstellung als erstes Element des $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Schritt 6: Konfigurieren von Speicher-Zuordnung

1. Die folgenden Befehl ruft die Liste der Speicher Klassifikationen in der Variablen $storageclassifications.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Die folgenden Befehle erhalten die Quelle Einteilung in $SourceClassificaion Variable und Ziel Klassifizierung in der Variablen $TargetClassification. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Die Quell- und Klassifikationen kann jedes Element im Array. Verweisen auf die Ausgabe der folgenden Befehl an, der Index des Quell- und Einstufung im $storageclassifications-Array. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object-Eigenschaft FriendlyName, Id | Tabelle formatieren


3. Die folgenden Cmdlets erstellt eine Zuordnung zwischen Quelle Klassifizierung und die gewünschte Klassifizierung. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Schritt 7: Schutz für virtuelle Computer aktivieren

Nach Server, Wolken und Netzwerke ordnungsgemäß konfiguriert sind, können Sie Schutz für virtuelle Maschinen in der Cloud. 


  1. Um den Schutz zu aktivieren, führen Sie den folgenden Befehl Schutz Container zu:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Erhalten Sie Schutz Entität (VM) durch Ausführen des folgenden Befehls:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Aktivieren Sie die Replikation für die VM durch Ausführen des folgenden Befehls:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Testen der Bereitstellung

Zum Testen der Bereitstellung führen Sie ein testfailover für einen einzelnen virtuellen Computer oder erstellen Sie einen Wiederherstellungsplan mehrere virtuelle Computer aus und führen Sie einen testfailover für den Plan. Testfailover simuliert Failover- und Mechanismus in einem isolierten Netzwerk. 

> [AZURE.NOTE] Sie können einen Wiederherstellungsplan für die Anwendung in Azure-Portal erstellen.

Zum Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte im [Überwachen der Aktivität](#monitor).


### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

1.  Führen Sie die folgenden Cmdlets zu VM Netzwerk Sie Failover testen möchten Ihre VMs.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Führen Sie ein testfailover eines virtuellen Computers wie folgt:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Führen Sie ein testfailover für einen Wiederherstellungsplan wie folgt:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Ausführen eines geplanten Failovers

1. Durchführung eines geplanten Failover eines virtuellen Computers wie folgt:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Durchführung eines geplanten Failover eines Recovery-Plans folgendermaßen:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Führen Sie ein ungeplantes Failovers

1. Führen Sie ein ungeplantes Failovers eines virtuellen Computers wie folgt:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. Führen Sie ein ungeplantes Failovers einen Wiederherstellungsplan folgendermaßen aus:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
## <a name=monitor></a>Überwachen der Aktivität

Verwenden Sie die folgenden Befehle, um die Aktivität zu überwachen. Beachten Sie, dass zwischen Projekten für die Verarbeitung beendet warten.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Nächste Schritte

[Mehr](https://msdn.microsoft.com/library/azure/mt637930.aspx) über Azure Site Recovery mit Azure Ressourcenmanager PowerShell-Cmdlets.

