<properties
    pageTitle="Schützen Sie Server in Azure mithilfe von Azure PowerShell Azure Ressourcenmanager | Microsoft Azure"
    description="Automatisieren Sie Schutz von Azure mit Azure Site Recovery Server mithilfe von PowerShell und Azure Resource Manager."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikation zwischen lokalen Hyper-V virtuelle Computer und Azure mithilfe von PowerShell und Azure-Ressourcen-Manager

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell - Ressourcen-Manager](site-recovery-deploy-with-powershell-resource-manager.md)
- [Verwaltungsportal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Übersicht

Azure Site Recovery trägt Ihre Business Continuity und Disaster Recovery-Strategie durch Replikation, Failover und Recovery von virtuellen Computern in einer Reihe von Bereitstellungsszenarien orchestriert. Eine vollständige Liste der Bereitstellungsszenarien finden Sie unter [Übersicht über Azure Site Recovery](site-recovery-overview.md).

Azure PowerShell ist ein Modul, die Cmdlets zum Verwalten von Azure über Windows PowerShell bereitstellt. Kann für zwei Arten von Modulen: Modul Azure Profil oder Azure-Ressourcen-Manager-Modul.

Site Recovery PowerShell-Cmdlets mit Azure PowerShell für Azure-Ressourcen-Manager können Sie schützen und Wiederherstellen der Server in Azure.

Dieser Artikel beschreibt, wie mithilfe von Windows PowerShell mit Azure-Ressourcen-Manager konfigurieren und koordinieren Serverschutz in Azure Site Recovery bereitstellen. Das Beispiel in diesem Artikel wird das schützen, Failover und Wiederherstellung virtueller Maschinen auf einem Hyper-V-Host, Azure mithilfe von Azure PowerShell Azure Ressourcenmanager veranschaulicht.

> [AZURE.NOTE] Site Recovery PowerShell-Cmdlets derzeit können Sie Folgendes konfigurieren: eine Virtual Machine Manager-Website zu einer anderen, Virtual Machine Manager-Website in Azure und Hyper-V-Website in Azure.

Sie müssen ein Experte PowerShell mithilfe dieses Artikels, aber die grundlegenden Konzepte wie Module, Cmdlets und Sessions verstehen müssen. Weitere Informationen zu Windows PowerShell finden Sie unter [Erste Schritte mit Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Sie können auch mehr [mithilfe von Azure](../powershell-azure-resource-manager.md)PowerShell mit Azure-Ressourcen-Manager.

> [AZURE.NOTE] Microsoft-Partner, die Bestandteil der Cloud Solution Provider (CSP) konfigurieren und Verwalten von Schutz ihrer Kunden Server ihrer Kunden entsprechenden CSP Abonnements (Abonnements Mieter).

## <a name="before-you-start"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie diese erforderlichen Komponenten verfügen:

- Ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)starten. Darüber hinaus können Sie [Preise Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/)nachlesen.
- Azure PowerShell 1.0. Weitere Informationen zu dieser Version und installieren [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) und [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) -Module. Sie erhalten die neuesten Versionen dieser Module von [PowerShell-Galerie](https://www.powershellgallery.com/)

Dieser Artikel veranschaulicht, wie mithilfe von Azure Powershell mit Azure-Ressourcen-Manager konfigurieren und Verwalten von Schutz Ihrer Server. In diesem Artikel verwendeten Beispiel zeigt, wie zu einer virtuellen Maschine auf einem Hyper-V-Host in Azure ausgeführt. Die folgenden Komponenten sind in diesem Beispiel. Ein umfassender Satz an verschiedenen Site Recovery-Szenarien finden Sie in der Dokumentation zu diesem Szenario.

- Ein Hyper-V-Host mit Windows Server 2012 R2 oder Microsoft Hyper-V Server 2012 R2, enthält ein oder mehrere virtuelle Computer.
- Hyper-V Server mit dem Internet verbunden, entweder direkt oder über einen Proxy.
- Die virtuellen Computer zu schützen sollte mit [virtuellen Komponenten](site-recovery-best-practices.md#virtual-machines)entsprechen.


## <a name="step-1-sign-in-to-your-azure-account"></a>Schritt 1: Azure-Konto anmelden


1. Öffnen Sie eine PowerShell-Konsole und führen Sie diesen Befehl ein Azure-Konto anmelden. Das Cmdlet wird einer Webseite, die Sie aufgefordert werden, Ihre Anmeldeinformationen einzugeben.

        Login-AzureRmAccount

    Alternativ kann auch Ihre Anmeldeinformationen als Parameter an die `Login-AzureRmAccount` Cmdlet mit dem `-Credential` Parameter.

    Sind für ein Mieter CSP-Partner Geben Sie den Debitor als Mieter mit ihren primären Domänennamen TenantID oder Mieter.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Ein Konto kann mehrere Abonnements verfügen, damit das Abonnement zuordnen sollten Sie das Konto verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Stellen Sie sicher, dass Ihr Abonnement registriert ist, um Azure Anbieter für Recovery Services und Site Recovery mithilfe der folgenden Befehle:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    In der Ausgabe dieser Befehle setzen von **RegistrationState** auf **registriert**, können Sie mit Schritt2 fortfahren. Wenn nicht, registrieren Sie den fehlenden Provider in Ihrem Abonnement.

    Um Azure Anbieter für Site Recovery und Recovery Services registrieren, müssen Sie die folgenden Befehle:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Überprüfen, ob die Anbieter registriert mithilfe der folgenden Befehle: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` und `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Schritt 2: Einrichten von Vault Recovery Services

1. Erstellen Sie eine Ressourcengruppe Azure-Ressourcen-Manager, in der Sie das Depot erstellen oder eine vorhandene Ressourcengruppe verwenden. Mit dem folgenden Befehl können Sie eine neue Ressourcengruppe erstellen:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    die $ResourceGroupName-Variable enthält den Namen der Ressourcengruppe erstellen möchten wobei Variable $Geo enthält der Azure-Region in der Ressourcengruppe (z. B. "Brasilien Süd") erstellt.

    Abrufen einer Liste von Ressourcengruppen in Ihrem Abonnement mithilfe der `Get-AzureRmResourceGroup` Cmdlet.

2. Erstellen Sie ein neues Depot Azure Recovery Services:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Abrufen einer Liste der vorhandenen Depots mithilfe der `Get-AzureRmRecoveryServicesVault` Cmdlet.

> [AZURE.NOTE] Operationen auf Site Recovery Depots mit dem Verwaltungsportal oder Azure Service Management PowerShell-Modul erstellt werden soll, können Sie eine Liste von solchen Depots mit Abrufen der `Get-AzureRmSiteRecoveryVault` Cmdlet. Erstellen Sie ein neues Depot Recovery Services für alle neuen Vorgänge. Bereits erstellte Site Recovery Depots werden unterstützt, aber die neuesten Funktionen haben.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Schritt 3: Festlegen Sie Recovery Services Vault-Kontext

1.  Legen Sie den Vault-Kontext durch Ausführen des folgenden Befehls:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Schritt 4: Erstellen einer Website Hyper-V und generiert einen neuen Schlüssel Depot für die Site.

1. Erstellen Sie eine neue Hyper-V-Website:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Dieses Cmdlet beginnt einen Wiederherstellungsauftrag Website um die Website zu erstellen und eine Site Recovery-Auftrag zurück. Warten Sie Auftrag und überprüfen Sie, ob des Auftrags erfolgreich abgeschlossen.

    Job-Objekts abgerufen und damit Überprüfen des aktuellen Status des Projekts, mit dem Cmdlet Get-AzureRmSiteRecoveryJob.
2. Erstellen Sie und downloaden Sie einen Registrierungsschlüssel für die Site wie folgt:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopieren des heruntergeladenen Schlüssels Hyper-V-Host. Sie benötigen die Taste, um Hyper-V-Host zur Website registrieren.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Schritt 5: Installieren der Azure Site Recovery-Anbieter und Azure Services Wiederherstellungsagenten auf dem Hyper-V-host

1. Das Installationsprogramm für die neueste Version des Anbieters von [Microsoft](https://aka.ms/downloaddra)herunterladen.

2. Ausführen der Installer auf dem Hyper-V-Host und am Ende der Installation weiterhin die Registrierung.

3. Aufforderung geben Sie heruntergeladenen Website Registrierungsschlüssel und Anmeldung von Hyper-V-Host zur Website an.

4. Überprüfen Sie, ob Hyper-V-Hosts auf der Website registriert ist, mithilfe des folgenden Befehls:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Schritt 6: Erstellen Sie einer Replikationsrichtlinie und Schutz Container zuordnen

1. Erstellen Sie eine Replikationsrichtlinie:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Überprüfen Sie den zurückgegebenen Auftrag, um sicherzustellen, dass das Replikation Richtlinie erstellen.

    >[AZURE.IMPORTANT] Angegebene Speicherkonto sollte in derselben Azure-Region als Vault Recovery Services und Geo-Replikation aktiviert haben.
    >
    > - Ist Recovery Storage Kontoname des Typs Azure Storage (klassisch) Failover der geschützten Computer Wiederherstellen des Computers Azure IaaS (klassisch).
    > - Wenn bestimmte Recovery Speicherkonto vom Typ Azure Storage (Azure Resource Manager) Failover der geschützte Computer ist den Computer Azure IaaS (Azure Resource Manager) wiederherstellen.

2. Erhalten Sie Schutz Container für die Website wie folgt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Beginnen Sie die Zuordnung des Containers Schutz mit der Replikation, wie folgt:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Für die Zuordnung Auftrag abgeschlossen, und erfolgreich abgeschlossen.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Schritt 7: Schutz für virtuelle Computer aktivieren

1. Erhalten Sie Schutz Entität für die VM zu schützen, wie folgt:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Starten Sie den virtuellen Computer folgendermaßen geschützt:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Angegebene Speicherkonto sollte in derselben Azure-Region als Vault Recovery Services und Geo-Replikation aktiviert haben.
    >
    > - Ist Recovery Storage Kontoname des Typs Azure Storage (klassisch) Failover der geschützten Computer Wiederherstellen des Computers Azure IaaS (klassisch).
    > - Wenn bestimmte Recovery Speicherkonto vom Typ Azure Storage (Azure Resource Manager) Failover der geschützte Computer ist den Computer Azure IaaS (Azure Resource Manager) wiederherstellen.

    > Hat VM Sie schützen mehr als eine Festplatte verknüpft, Angeben der Betriebssystem-CD mit dem *OSDiskName* -parameter

3. Die virtuellen Computer zu einem geschützten Status nach der ersten Replikation warten. Dies kann je nach Faktoren wie die Menge der zu replizierenden Daten und upstream Bandbreite in Azure dauern. Auftragsstatus und StateDescription werden wie folgt auf die VM eine geschützte Zustand aktualisiert.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Aktualisieren Sie Recovery Eigenschaften wie die Größe der VM-Rolle und Azure-Netzwerk der virtuellen Maschine Netzwerkkarten, bei einem Failover an.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Schritt 8: Führen Sie ein testfailover

1. Führen Sie ein Failover Testauftrags wie folgt aus:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Stellen Sie sicher, dass die VM in Azure erstellt wird. (Test-Failover Auftrag wird angehalten, nachdem die Test-VM in Azure erstellt. Auftragsabschluss bereinigen erstellte Artefakte auf den Job wiederaufnehmen, wie im nächsten Schritt.)

3. Führen Sie das testfailover wie folgt:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Nächste Schritte

[Mehr](https://msdn.microsoft.com/library/azure/mt637930.aspx) über Azure Site Recovery mit Azure Ressourcenmanager PowerShell-Cmdlets.
