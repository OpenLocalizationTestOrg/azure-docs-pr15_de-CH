<properties
    pageTitle="Installieren Sie Symantec Endpoint Protection auf einer VM | Microsoft Azure"
    description="Informationen Sie zum Installieren und Konfigurieren von Symantec Endpoint Protection Security Erweiterung auf einer neuen oder vorhandenen Azure VM mit klassischen Bereitstellungsmodell erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Zum Installieren und Konfigurieren von Symantec Endpoint Protection auf einer Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In diesem Artikel wird das Installieren und konfigurieren Sie Symantec Endpoint Protection auf vorhandenen virtuellen Computer (VM) unter Windows Server veranschaulicht. Dies ist der vollständige Client Services wie Viren- und Spyware-Schutz, Firewall- und Intrusion Prevention enthält. Der Client wird als sicherheitserweiterung mithilfe der VM-Agent installiert.

Haben Sie ein Abonnement von Symantec für eine lokale Lösung, können Sie Ihre Azure virtuellen Computer schützen. Wenn Sie ein Kunde noch nicht sind, können Sie ein Testabonnement anmelden. Weitere Informationen zu dieser Lösung finden Sie [Symantec Endpoint Protection auf Microsoft Azure-Plattform][Symantec]. Diese Seite enthält auch Links zu Informationen zur Lizenzierung und Anleitung für den Client installieren, wenn Sie bereits einen Symantec-Kunden.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Endpoint Protection auf eine vorhandene VM installieren

Bevor Sie beginnen, benötigen Sie Folgendes:

- Azure PowerShell-Modul, Version 0.8.2 oder höher auf Ihrem Arbeitscomputer. Überprüfen Sie die Version von Azure PowerShell, die Installation mit dem **Get-Modul Azure | Tabelle formatieren Version** Befehl. Informationen und einen Link auf die neueste Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][PS]. Melden Sie sich bei der Azure-Abonnement mit `Add-AzureAccount`.

- Der VM-Agent die Azure Virtual Machine ausgeführt.

Überprüfen Sie zunächst, dass der VM-Agent auf dem virtuellen Computer installiert ist. Füllen Sie der Cloud Dienstname und Name des virtuellen Computers und führen Sie folgende Befehle ein Administratorebene Azure PowerShell-Befehlszeile. Alles innerhalb der Anführungszeichen, einschließlich der Zeichen < und >.

> [AZURE.TIP] Wenn Sie Cloud-Dienst und virtuellen Namen nicht kennen, führen Sie **Get-AzureVM** um die Namen für alle virtuellen Maschinen Ihr Abonnement.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Wenn **der Befehl **Write-Host** angezeigt wird,**ist der VM-Agent installiert. Wenn **False**angezeigt wird, finden Sie in der Anleitung und einen Link zum Download in Azure Blog [VM und Extensions - Teil2]buchen[Agent].

Wenn die VM-Agent installiert ist, führen Sie diese Befehle Symantec Endpoint Protection Agent installieren.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Um sicherzustellen, dass Symantec Security Erweiterung installiert wurde und auf dem neuesten Stand ist:

1.  Melden Sie sich am virtuellen Computer an. Eine Anleitung finden Sie unter [Anmelden bei einem virtuellen Computer ausführen von Windows Server][Logon].
2.  Windows Server 2008 R2 finden Sie **starten > Symantec Endpoint Protection**. Geben Sie für Windows Server 2012 oder Windows Server 2012 R2 aus dem Startbildschirm **Symantec**, und klicken Sie dann auf **Symantec Endpoint Protection**.
3.  Auf der Registerkarte **Status** im Fenster **Status Symantec Endpoint Protection** installieren oder bei Bedarf neu starten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Anmelden bei einem virtuellen Computer unter WindowsServer][Logon]

[Azure VM-Erweiterungen und Funktionen][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493