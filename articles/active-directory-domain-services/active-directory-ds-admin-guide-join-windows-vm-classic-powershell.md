<properties
    pageTitle="Azure Active Directory Domain Services: Administration Guide | Microsoft Azure"
    description="Fügen Sie eine virtuellen Windows-Maschine einer verwalteten Domäne Azure PowerShell mit klassischen Bereitstellungsmodell."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Fügen Sie einen virtuellen Computer Windows Server einer verwalteten Domäne mithilfe von PowerShell

> [AZURE.SELECTOR]
- [Klassische Azure-Portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Fenster](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und Classic](../resource-manager-deployment-model.md). Dieser Artikel behandelt das klassische Bereitstellungsmodell verwenden. Azure Active Directory-Domänendienste unterstützt derzeit nicht das Ressourcen-Manager-Modell.

Diese Schritte zeigen, wie Azure PowerShell Befehle anpassen, die erstellen und einen Windows-basierten virtuellen Azure-Computer mithilfe einer Bausteinkonzept vorkonfigurieren. Diese Schritte können Sie das Erstellen eines Windows-basierten virtuellen Azure-Computers und einer verwalteten Azure Active Directory-Domänendienste-Domäne beitreten.

Diese Schritte eines fill-in-the-Leerzeichen Ansatzes zum Erstellen von Azure PowerShell-Befehlssätze. Dieser Ansatz kann hilfreich sein, wenn Sie PowerShell oder welche Werte für erfolgreiche Konfiguration angeben möchten. Fortgeschrittene PowerShell können Befehle und eigene Werte für die Variablen (die Zeilen mit "$") ersetzt.

Wenn Sie dies noch nicht getan haben, verwenden der Anleitung [Installieren](../powershell-install-configure.md) und Konfigurieren von Azure PowerShell Azure PowerShell auf dem lokalen Computer installieren. Öffnen Sie eine Windows PowerShell-Befehlszeile.

## <a name="step-1-add-your-account"></a>Schritt 1: Fügen Sie Ihr Konto

1. PowerShell aufgefordert werden Geben Sie **AzureAccount hinzufügen** und drücken Sie die **EINGABETASTE**.
2. Geben Sie die e-Mail-Adresse in Ihrem Azure-Abonnement, und klicken Sie auf **Weiter**.
3. Geben Sie das Kennwort für Ihr Konto ein.
4. Klicken Sie auf **Anmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Schritt 2: Legen Sie Ihr Abonnement und Konto

Festlegen der Azure-Abonnements und Speicherkonto diese Befehle in der Befehlszeile von Windows PowerShell ausgeführt. Alles innerhalb der Anführungszeichen, einschließlich der Zeichen mit den richtigen Namen < und >.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Sie können den richtigen Namen aus der SubscriptionName-Eigenschaft der Ausgabe des Befehls **Get-AzureSubscription** abrufen. Sie finden den Kontonamen Lagerung Bezeichnungseigenschaft der Ausgabe des Befehls **Get-AzureStorageAccount** nach den Befehl **Select AzureSubscription** .


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Schritt 3: Schrittweise - Bereitstellung der virtuellen Maschine und der verwalteten Domäne beitreten
Hier wird der entsprechenden Azure PowerShell Befehlssatz Leerzeilen zwischen einzelnen Lesbarkeit dieser virtuellen Computer erstellen.

Geben Sie Informationen über die virtuellen Windows-Maschine bereitgestellt werden.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

InstanceSize Werte für D-DS- oder G-Serie virtuellen Computern finden Sie unter [virtueller Computer und Azure Cloud Service Größe](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Informationen der verwalteten Domäne.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Geben Sie den Namen des Cloud-Dienst.

    $svcname="Contoso100-test"

Geben Sie den Namen des virtuellen Netzwerks, die VM verknüpft werden sollen. Dass AAD DS verwaltete Domäne in diesem virtuellen Netzwerk verfügbar ist.

    $vnetname="MyPreviewVnet"

Wählen Sie die VM mit den virtuellen Computer bereitstellen.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurieren Sie die VM - VM Namen, Größe und Bild.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Erhalten Sie lokale Administratorrechte für die VM. Wählen Sie ein sicheres lokales Administratorkennwort.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Stellen Sie Anmeldeinformationen für ein Benutzerkonto zur Gruppe 'AAD DC Administrators' VM verwalteten Domäne hinzufügen. Domänenname – in unserem Beispiel nicht zum Beispiel angeben, 'Bob' als Benutzername angegeben.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigurieren der VM - Domäne beitreten Anforderung & erforderlichen Anmeldeinformationen angeben.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Legen Sie ein Subnetz für den virtuellen Computer.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Optional: Zeigen Sie die IP-Adresse der Domäne. Festlegen von IP-Adressen der DNS-Server für das virtuelle Netzwerk verwalteten Domäne Azure Active Directory-Domänendienste ist dieser Schritt nicht erforderlich.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Jetzt bereitstellen Sie VM Windows-Domäne.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skript Bereitstellung Windows VM und automatisch zu einer AAD Domänendienste verwalteten Domäne hinzufügen
Dieser Befehlssatz PowerShell erstellt einen virtuellen Computer für eine LOB Server, die:

- Windows Server 2012 R2 Datacenter-Bild verwendet.
- Ist ein kleiner virtueller Computer.
- Hat den Namen Contoso-Test.
- Automatisch wird der verwalteten Domäne contoso100 Domäne.
- Im gleichen virtuellen Netzwerk der verwalteten Domäne hinzugefügt wird.

Hier ist das vollständige Beispielskript zu Windows Virtual Machine automatisch verwalteten Azure Active Directory-Domänendienste-Domäne beitreten.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Verwandte Inhalte
- [Azure Active Directory-Domänendienste - erste Schritte](./active-directory-ds-getting-started.md)

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](./active-directory-ds-admin-guide-administer-domain.md)
