<properties
    pageTitle="Das Kennwort oder die Remotedesktop-Konfiguration auf einem Windows-VM zurücksetzen | Microsoft Azure"
    description="Erfahren Sie, wie ein Kennwort oder Remote Desktop Services auf einer Windows-VM mit der Azure-Portal oder Azure PowerShell zurücksetzen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Der Remotedesktopdienst oder dessen Passwort in einer Windows-VM zurücksetzen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wenn Sie mit einem Windows-Computer (VM) verbinden können, können Sie das lokale Administratorkennwort zurücksetzen oder Remotedesktop-Dienstkonfiguration zurücksetzen. Azure-Portal oder Erweiterung des VM können in Azure PowerShell Sie das Kennwort zurücksetzen. Wenn Sie PowerShell verwenden, stellen Sie sicher, daß das neueste PowerShell installiert auf Ihrem Arbeitscomputer und Azure-Abonnement angemeldet. Ausführliche Anleitung [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] Sie können die Version von PowerShell überprüfen, die Installation mit`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windows-VMs in Ressourcen-Manager-Bereitstellungsmodell

### <a name="azure-portal"></a>Azure-Portal
VM auswählen, indem Sie auf **Durchsuchen** > **virtuellen Computer** > *der virtuellen Windows-Maschine* > **Alle** > **Passwort**. Kennwort zurücksetzen Blatt wird angezeigt:

![Passwort zurücksetzen](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Geben Sie den Benutzernamen und das neue Kennwort, und klicken Sie auf **Speichern**. Versuchen Sie die VM erneut.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess-Erweiterung und PowerShell

Sicher Azure PowerShell 1.0 oder höher installiert ist und Sie über Ihr Konto angemeldet haben die `Login-AzureRmAccount` Cmdlet.

#### <a name="reset-the-local-administrator-account-password"></a>**Das Kennwort des lokalen Administratorkontos zurücksetzen**

Sie können Administratornamen Passwort oder mit dem Befehl [Set AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell zurücksetzen.

Erstellen Sie Ihren lokalen Administrator Anmeldeinformationen mithilfe des folgenden Befehls:

    $cred=Get-Credential

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, VMAccess Erweiterung Befehl benennt das lokale Administratorkonto das Kennwort für dieses Konto zugewiesen und Remotedesktop Abmeldeereignis Probleme. Wenn das lokale Administratorkonto deaktiviert ist, kann die VMAccess-Erweiterung es.

Mit Erweiterung des VM die neuen Anmeldeinformationen wie folgt fest:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Ersetzen Sie `myRG`, `myVM`, `myVMAccess`, und Werte für das Setup.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Konfiguration des Remotedesktop zurücksetzen**

Sie können die VM mit [Set AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) bzw. [Set AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx)wie folgt RAS zurücksetzen. (Ersetzen Sie die `myRG`, `myVM`, `myVMAccess` und mit Ihren eigenen Werten.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Oder:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Beide Befehle hinzufügen einen neuen benannten VM Access Agent auf dem virtuellen Computer. Jederzeit können ein virtuellen Computer nur einen einzelnen VM Access Agent. Um VM Zugriff erfolgreich Agenteigenschaften festzulegen, legen Sie zuvor mit Zugriff Agenten entfernen `Remove-AzureRmVMAccessExtension` oder `Remove-AzureRmVMExtension`. Azure PowerShell Version 1.2.2 ab, Sie können diesen Schritt auch vermeiden Verwendung `Set-AzureRmVMExtension` mit einem `-ForceRerun` Option. Verwendung von `-ForceRerun`, den gleichen Namen für VM Access Agent wie vom vorhergehenden Befehl verwenden.

Wenn Sie weiterhin remote mit dem virtuellen Computer verbinden können, finden Sie weitere Schritte zur [Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer zu beheben](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windows-VMs im klassischen Bereitstellungsmodell

### <a name="azure-portal"></a>Azure-portal

[Azure-Portal](https://portal.azure.com) können Sie für virtuelle Maschinen mit dem klassischen Bereitstellungsmodell erstellt den Remotedesktopdienst zurücksetzen. Klicken: **Durchsuchen** > **virtuellen Maschinen (classic)** > *der virtuellen Windows-Maschine* > **Remote zurücksetzen...**. Die folgende Seite angezeigt.

![RDP-Konfigurationsseite zurücksetzen](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Sie können auch versuchen, den Namen und das Kennwort des lokalen Administratorkontos zurücksetzen. Klicken: **Durchsuchen** > **virtuellen Maschinen (classic)** > *der virtuellen Windows-Maschine* > **Alle** > **Passwort**. Die folgende Seite angezeigt.

![Passwort zurücksetzen](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Nachdem Sie den neuen Benutzernamen und das Kennwort eingeben, klicken Sie auf **Speichern**.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess-Erweiterung und PowerShell

Stellen Sie sicher, dass VM-Agent auf dem virtuellen Computer installiert ist. Die VMAccess-Erweiterung muss installiert sein, bevor Sie es verwenden können als VM-Agent verfügbar ist. Überprüfen Sie, ob die VM-Agent bereits installiert ist, mithilfe des folgenden Befehls. (Ersetzen Sie "MyCloudService" und "MyVM" den Namen der Cloud-Dienst und die VM. Lernen Sie diese Namen mit `Get-AzureVM` ohne Parameter.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Wenn **der Befehl **Write-Host** angezeigt wird,**ist der VM-Agent installiert. Wenn **False**angezeigt wird, finden Sie die Informationen und einen Link zum Download des Azure Blogbeitrags [VM und Extensions - Teil2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) .

Wenn Sie den virtuellen Computer mithilfe der Portalwebsite erstellt, überprüfen Sie, ob `$vm.GetInstance().ProvisionGuestAgent` gibt **True**zurück. Wenn nicht, können Sie es mit diesem Befehl:

    $vm.GetInstance().ProvisionGuestAgent = $true

Dieser Befehl wird verhindert, dass den folgenden Fehler beim Ausführen des Befehls **Set AzureVMExtension** in den nächsten Schritten: "Bereitstellung Gast-Agent muss aktiviert auf VM-Objekt vor IaaS VM Erweiterung."

#### <a name="reset-the-local-administrator-account-password"></a>**Das Kennwort des lokalen Administratorkontos zurücksetzen**

Anmeldeinformationen anmelden den Namen des aktuellen lokalen Administratorkontos und ein neues Kennwort zu erstellen, und führen Sie das `Set-AzureVMAccessExtension` wie folgt.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Wenn Sie einen anderen Namen als das aktuelle Konto eingeben, VMAccess-Erweiterung benennt das lokale Administratorkonto das Kennwort für dieses Konto zugewiesen und Remotedesktop Abmelden Probleme. Wenn das lokale Administratorkonto deaktiviert ist, kann die VMAccess-Erweiterung es.

Diese Befehle auch zurücksetzen Remotedesktop-Dienstkonfiguration.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Konfiguration des Remotedesktop zurücksetzen**

Um die Remotedesktop-Dienstkonfiguration zurückzusetzen, führen Sie den folgenden Befehl ein:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Die VMAccess-Erweiterung führt zwei Befehle auf dem virtuellen Computer:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Dieser Befehl ermöglicht die integrierte Windows-Firewall Gruppe, Remotedesktop Datenverkehr verwendet TCP-Port 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Dieser Befehl stellt die fDenyTSConnections Registrierungswert auf 0 Remotedesktopverbindungen aktivieren.


## <a name="next-steps"></a>Nächste Schritte

Wenn Erweiterung des Azure VM nicht reagiert und Sie nicht, das Kennwort zurückzusetzen, können Sie [offline das lokale Windows-Kennwort zurücksetzen](virtual-machines-windows-reset-local-password-without-agent.md). Diese Methode ist eine erweiterte Prozess und erfordert eine andere VM die virtuelle Festplatte der problematischen VM herstellen. Führen Sie die Schritte in diesem Artikel zuerst dokumentiert und nur dann offline Kennwort-Reset-Methode als letzten Ausweg.

[Azure VM-Erweiterungen und Funktionen](virtual-machines-windows-extensions-features.md)

[Verbinden Sie mit einer virtuellen Azure-Computer mit RDP oder SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md)
