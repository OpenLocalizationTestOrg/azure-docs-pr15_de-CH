<properties
    pageTitle="Replizieren von Hyper-V virtuelle Computer in VMM-Clouds Azure Site Recovery mit PowerShell | Microsoft Azure"
    description="Informationen Sie zur Automatisierung der Replikation von Hyper-V virtuelle Computer in VMM-Clouds Site Recovery mit PowerShell."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Replizieren von Hyper-V virtuelle Computer in VMM-Clouds in Azure mithilfe von Powershell - klassisch

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - Ressourcen-Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Verwaltungsportal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassisch](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Übersicht

Azure Site Recovery trägt Ihre Business Continuity und Disaster Recovery (BCDR)-Strategie durch Replikation, Failover und Recovery von virtuellen Computern in einer Reihe von Bereitstellungsszenarien orchestriert. Eine vollständige Liste der Bereitstellungsszenarien finden Sie unter [Übersicht über Azure Site Recovery](site-recovery-overview.md).

Dieser Artikel beschreibt wie Sie PowerShell Automatisierung häufiger Aufgaben müssen Sie ausführen, wenn Sie Azure Site Recovery für Hyper-V virtuelle Computer in System Center VMM-Clouds in Azure-Speicher replizieren einrichten.

Artikel enthält Komponenten für das Szenario und zeigt, wie Sie Site Recovery Depot einrichten installieren Sie das Azure Site Recovery auf der VMM Registrieren des Servers im Tresor, Azure Storage-Konto hinzufügen, Azure Recovery Services Agent auf Hyper-V-Hostservern installieren, Schutz für alle geschützten virtuellen Maschinen angewendet werden VMM-Clouds konfigurieren , und aktivieren Sie Schutz für diese virtuellen Computer. Testen Sie das Failover sicherzustellen, dass alles erwartungsgemäß beenden.

Wenn Sie dieses Szenario einrichten Probleme auftreten, stellen Sie Ihre Fragen in [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt die Bereitstellung Klassisch verwenden.



## <a name="before-you-start"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie diese erforderlichen Komponenten verfügen:

### <a name="azure-prerequisites"></a>Azure-Komponenten

- Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten.
- Sie benötigen ein Konto Azure-Speicher zum Speichern von replizierter Daten. Das Konto benötigt Geo-Replikation aktiviert. Es sollte im Bereich Azure Site Recovery Depot und dieselbe Abonnement zugeordnet. [Erfahren Sie mehr über Azure-Speicher](../storage/storage-introduction.md).
- Sie müssen sicherstellen, dass virtuelle Computer schützen möchten [Azure virtuellen Komponenten](site-recovery-best-practices.md#virtual-machines)entsprechen.

### <a name="vmm-prerequisites"></a>VMM-Komponenten
- VMM-Server auf System Center 2012 R2 benötigen.
- Sie benötigen mindestens eine Cloud auf dem VMM-Server zu schützen. Die Cloud sollte enthalten:
    - Eine oder mehrere VMM-Hostgruppen.
    - Hyper-V-Hostserver oder Cluster in jede Hostgruppe.
    - Ein oder mehrere virtuelle Computer auf der Hyper-V

### <a name="hyper-v-prerequisites"></a>Hyper-V-Komponenten

- Hyper-V-Hostservern installiert werden mindestens **Windows Server 2012** **Microsoft Hyper-V Server 2012** und Hyper-V-Rolle und die neuesten Updates installiert haben.
- Wenn Sie Hyper-V in einer Notiz Cluster, Cluster-Broker arbeiten ist nicht automatisch erstellt, wenn einen statischen IP-Adresse-basierten Cluster haben. Sie müssen den Cluster-Broker manuell konfigurieren. Im Server-Manager dazu > Failovercluster-Manager, mit dem Cluster herstellen **Rolle konfigurieren** und auf **Hyper-V Replica Broker** **Rolle wählen Sie** Bildschirm des Assistenten für hohe Verfügbarkeit.
- Hyper-V-Hostserver oder Cluster zu Schutz werden soll eine VMM-Cloud beizufügen.

### <a name="network-mapping-prerequisites"></a>Netzwerk-Zuordnung erforderliche Komponenten
Wenn Sie virtuelle Computer in Azure Zuordnung Netzwerkkarten zwischen VM auf dem Quellserver VMM schützen und Azure Netzwerke, um Folgendes zu ermöglichen:

- Alle im gleichen Netzwerk Failover Maschinen können, unabhängig von der, denen Wiederherstellungsplan in sind.
- Ist ein Netzwerk-Gateway auf dem Ziel Azure-Netzwerk, können virtuelle Computer mit anderen lokalen virtuellen Computer verbinden.
- Wenn Failover gleichen Wiederherstellungsplan Zuordnung nur virtuellen Maschinen im Netzwerk konfiguriert werden nicht werden nach Failover auf Azure miteinander verbinden können.

Wenn Sie Netzwerkzuordnung bereitstellen möchten benötigen Sie Folgendes:

- Die virtuellen Computer auf dem Quellserver VMM schützen möchten sollten mit einem VM-Netzwerk verbunden. Das Netzwerk sollte ein logisches Netzwerk verbunden, das der Cloud zugeordnet ist.
- Ein Azure-Netzwerk der replizierten virtuellen Computer nach einem Failover herstellen können. Sie wählen dieses Netzwerk bei Failover. Das Netzwerk sollte in der gleichen Region wie Ihr Abonnement Azure Site Recovery.
- [Erfahren Sie mehr](site-recovery-network-mapping.md) über Netzwerkzuordnung:

###<a name="powershell-prerequisites"></a>PowerShell-Komponenten
Achten Sie Azure PowerShell bereit. Wenn Sie PowerShell bereits verwenden, müssen Sie auf Version 0.8.10 oder höher. Informationen zum Einrichten von PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Nach der Einrichtung und Konfiguration PowerShell sehen Sie alle verfügbaren Cmdlets für den Dienst [hier](https://msdn.microsoft.com/library/dn850420.aspx).

Tipps helfen, verwenden Sie die Cmdlets wie Parameterwerte, Eingaben und Ausgaben in der Regel in Azure PowerShell Behandlung finden Sie in [Erste Schritte mit Azure-Cmdlets](https://msdn.microsoft.com/library/azure/jj554332.aspx)

## <a name="step-1-set-the-subscription"></a>Schritt 1: Festlegen des Abonnements

In PowerShell diese Cmdlets ausführen:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Elemente in "< >" mit bestimmten Informationen ersetzt.

## <a name="step-2-create-a-site-recovery-vault"></a>Schritt 2: Erstellen eines Depots Site Recovery

PowerShell Ihre spezifischen Informationen Elemente in "< >" ersetzen und diese Befehle ausführen:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Schritt 3: Erstellen Sie einen Vault-Registrierungsschlüssel

Erstellen Sie einen Registrierungsschlüssel im Tresor. Azure Site Recovery Anbieter herunterladen und installieren auf dem VMM-Server verwenden Sie diesen Schlüssel, das Depot VMM-Server anmelden.

1.  Vault-Einstellungsdatei abzurufen und den Kontext festlegen:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Legen Sie den Vault-Kontext durch Ausführen der folgenden Befehle:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Schritt 4: Installieren Sie das Azure Site Recovery

1.  Erstellen Sie auf dem VMM ein Verzeichnis mit dem folgenden Befehl:

    ```

    pushd C:\ASR\

    ```

2.  Extrahieren Sie die Dateien mit den gedownloadeten Anbieter durch Ausführen des folgenden Befehls

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Installieren Sie den Anbieter mit den folgenden Befehlen:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Warten Sie, bis die Installation abgeschlossen.

4.  Registrieren Sie den Server in das Depot mit dem folgenden Befehl:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Schritt 5: Erstellen Sie ein Konto Azure-Speicher

Wenn ein Azure Storage-Konto besitzen, erstellen Sie ein Konto Geo-Replikation aktiviert den folgenden Befehl ausführen:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Beachten Sie, dass das Speicherkonto muss im selben Bereich wie der Azure Site Recovery-Dienst und dem gleichen Abonnement zugeordnet werden.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Schritt 6: Installieren Sie den Agent Azure Recovery Services

Installieren Sie von Azure-Portal den Azure Recovery Services Agent auf jeden Server Hyper-V-Host in VMM-Clouds zu schützen.

Führen Sie den folgenden Befehl auf allen VMM-Hosts:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Schritt 7: Konfigurieren von Cloud Einstellungen

1.  Erstellen Sie ein schutzprofil Azure Cloud durch Ausführen des folgenden Befehls:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Erhalten Sie Schutz Container durch Ausführen der folgenden Befehle:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Beginnen Sie die Zuordnung des Containers Schutz mit der Cloud:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Wenn das Projekt abgeschlossen ist, führen Sie den folgenden Befehl ein:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Nachdem der Auftrag verarbeitet wurde, führen Sie den folgenden Befehl ein:


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



Zum Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte im [Überwachen der Aktivität](#monitor).

## <a name="step-8-configure-network-mapping"></a>Schritt 8: Konfigurieren von Netzwerkzuordnung
Bevor Sie beginnen Netzwerkübersicht überprüfen Sie, ob virtuelle Maschinen auf dem Quellserver VMM mit einem VM-Netzwerk verbunden sind. Darüber hinaus erstellen Sie Azure virtuelle Netzwerke. Beachten Sie, dass ein Azure Netzwerk mehrere VM-Netzwerke zugeordnet werden können.

Beachten Sie, dass wenn das Zielnetzwerk mehrere Subnetze und von Subnetzen hat denselben Namen wie Subnetz auf dem virtuellen Quellcomputers befindet und VM Replikat an Ziel Subnetz nach einem Failover verbunden. Kein Ziel Subnetz mit einem übereinstimmenden Namen vorhanden ist, wird der virtuelle Computer auf das erste Subnetz im Netzwerk erfolgen.

Der erste Befehl ruft Servern für den aktuellen Azure Site Recovery Tresor. Der Befehl speichert Microsoft Azure Site Recovery-Server in der Arrayvariablen $Servers.

    $Servers = Get-AzureSiteRecoveryServer


Der zweite Befehl wird das Site Recovery Netzwerk für den ersten Server im $Servers-Array. Der Befehl Netzwerke in der Variablen $Networks gespeichert.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Dritte Befehl ruft Ihre Azure-Abonnements mit dem Cmdlet Get-AzureSubscription und dann in der Variablen $Subscriptions gespeichert.

    $Subscriptions = Get-AzureSubscription



Der vierte Befehl ruft Azure virtuelle Netzwerke verwenden das Cmdlet "Get-AzureVNetSite" und dann den Wert in der Variablen $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



Letzte Cmdlet erstellt eine Zuordnung zwischen der primären und Azure virtuellen Netzwerk. Das Cmdlet gibt das primäre Netzwerk als das erste Element der $Networks. Das Cmdlet gibt ein Netzwerk als erstes Element des $AzureVmNetworks mit der ID Der Befehl enthält Ihre Azure-Abonnement-ID.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Schritt 9: Aktivieren des Schutzes für virtuelle Maschinen

Nach Servern, Wolken und Netzwerke richtig konfiguriert sind, können Sie Schutz für virtuelle Maschinen in der Cloud. Beachten Sie Folgendes:

Virtuelle Maschinen müssen [Azure Virtual Machine Vorkenntnisse](site-recovery-best-practices.md#virtual-machines)mitbringen.

Zum Schutz der Betriebssysteme und aktivieren müssen Eigenschaften für den virtuellen Computer festgelegt werden. Beim Erstellen eines virtuellen Computers in VMM anhand einer Vorlage für virtuelle Computer können Sie die Eigenschaft festlegen. Diese Eigenschaften für vorhandene virtuelle Computer legen Sie auf den Registerkarten **Allgemein** und **Hardwarekonfiguration** Eigenschaften virtueller Computer. Wenn Sie diese Eigenschaften in VMM festlegen werden Sie in Azure Site Recovery-Portal konfiguriert.



1.  Um den Schutz zu aktivieren, führen Sie den folgenden Befehl Schutz Container zu:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Erhalten Sie Schutz Entität (VM) durch Ausführen des folgenden Befehls:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Aktivieren des DR für den virtuellen Computer durch Ausführen des folgenden Befehls:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Testen der Bereitstellung

Zum Testen der Bereitstellung führen Sie ein testfailover für einen einzelnen virtuellen Computer oder erstellen Sie einen Wiederherstellungsplan mehrere virtuelle Computer aus und führen Sie einen testfailover für den Plan. Testfailover simuliert Failover- und Mechanismus in einem isolierten Netzwerk. Beachten Sie Folgendes:

- Wenn Sie den virtuellen Computer in Azure mithilfe von Remotedesktop nach dem Failover verbinden möchten, aktivieren Sie Remotedesktopverbindung auf dem virtuellen Computer vor dem Ausführen von testfailover.
- Eine öffentliche IP-Adresse verwenden Sie nach dem Failover der virtuellen Computer in Azure mithilfe von Remotedesktop herstellen. Wenn Sie dies tun möchten, sicherzustellen Sie, dass Sie Domänenrichtlinien nicht, die Verbindung mit einem virtuellen Computer eine öffentliche Adresse verhindern.

Zum Abschluss des Vorgangs zu überprüfen, führen Sie die Schritte im [Überwachen der Aktivität](#monitor).

### <a name="create-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

1. Erstellen Sie eine XML-Datei als Vorlage für Ihren Wiederherstellungsplan mit folgenden Daten und speichern Sie sie als "C:\RPTemplatePath.xml".
2. Ändern der RecoveryPlan Knoten-Id, Name, PrimaryServerId und SecondaryServerId.
3. Ändern Sie den Knoten ProtectionEntity PrimaryProtectionEntityId (Vmid in VMM).
4. Sie können mehrere VMs hinzufügen ProtectionEntity Knoten hinzufügen.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Geben Sie die Daten in der Vorlage:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Erstellen der RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Führen Sie ein testfailover

1.  Abrufen des RecoveryPlan-Objekts den folgenden Befehl ausführen:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Starten Sie das testfailover durch Ausführen des folgenden Befehls:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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

[Mehr](https://msdn.microsoft.com/library/dn850420.aspx) über Azure Site Recovery PowerShell-Cmdlets. </a>.
