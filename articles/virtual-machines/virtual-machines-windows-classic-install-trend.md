<properties
    pageTitle="Installieren Sie Trend Micro Deep Security auf einer VM | Microsoft Azure"
    description="Dieser Artikel beschreibt das Installieren und Konfigurieren von Trend Micro Security auf einem virtuellen Computer mit dem klassischen Bereitstellungsmodell in Azure erstellt."
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


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Zum Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

In diesem Artikel wird das Installieren und Konfigurieren von Trend Micro Deep Security als Dienst auf einem neuen oder vorhandenen virtuellen Computer (VM) unter Windows Server veranschaulicht. Tiefe Sicherheit als Service enthält Malware-Schutz, Firewall ein Intrusion Prevention-System und Integrität zu überwachen.

Der Client wird als sicherheitserweiterung über den VM-Agenten installiert. Auf einem neuen virtuellen Computer installieren Sie VM-Agent mit der Tiefe-Agent. Auf einem vorhandenen virtuellen Computer, die den VM-Agent müssen Sie downloaden und installieren Sie es zuerst. Dieser Artikel beschreibt beide Situationen.

Haben Abonnement von Trend Micro für lokale Lösung können es Sie Ihre Azure virtuellen Computer schützen. Wenn Sie ein Kunde noch nicht sind, können Sie ein Testabonnement anmelden. Weitere Informationen zu dieser Lösung finden Sie unter der Trend Micro Blogbeitrag [Microsoft Azure VM-Agent-Erweiterung für Tiefe Sicherheit](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Installieren der Tiefe-Agent auf einer neuen VM

[Azure-Verwaltungsportal](http://manage.windowsazure.com) können Sie bei Verwendung **Von Galerie** -Option zum Erstellen des virtuellen Computers VM-Agent und Trend Micro sicherheitserweiterung installieren. Erstellen eine einzige virtuellen Maschine wird über das Portal eine einfache Möglichkeit zum Schutz von Trend Micro hinzuzufügen.

Diese Option **From Gallery** öffnet einen Assistenten, den virtuellen Computer einrichten. Sie verwenden die letzte Seite des Assistenten den VM-Agent und Trend Micro sicherheitserweiterung installieren. Allgemeine Informationen finden Sie unter [Windows im klassischen Azure-Portal Erstellen eines virtuellen Computers ausgeführt](virtual-machines-windows-classic-tutorial.md). Wenn Sie zur letzten Seite des Assistenten erhalten, führen Sie folgende Schritte aus:

1.  Überprüfen Sie unter **VM-Agent** **VM-Agent installieren**.

2.  Überprüfen Sie unter **Security Extensions** **Trend Micro Deep Security Agent**.

    ![Installieren der VM-Agent und der Tiefe-Agent](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Klicken Sie zum Erstellen des virtuellen Computers.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Installieren Sie Tiefe Security Agent auf eine vorhandene VM

Um den Agenten auf eine vorhandene VM installieren, benötigen Sie Folgendes:

- Azure PowerShell-Modul, Version 0.8.2 oder höher, auf dem lokalen Computer installiert. Überprüfen Sie die Version von Azure PowerShell, die Installation mit dem **Get-Modul Azure | Tabelle formatieren Version** Befehl. Informationen und einen Link auf die neueste Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Melden Sie sich bei der Azure-Abonnement mit `Add-AzureAccount`.

- Der VM-Agent auf dem virtuellen Zielcomputer installiert.

Überprüfen Sie zunächst, dass die VM-Agent bereits installiert ist. Füllen Sie der Cloud Dienstname und Name des virtuellen Computers und führen Sie folgende Befehle ein Administratorebene Azure PowerShell-Befehlszeile. Alles innerhalb der Anführungszeichen, einschließlich der Zeichen < und >.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Wenn Sie Cloud-Dienst und Name des virtuellen Computers nicht kennen, führen Sie **Get-AzureVM** um diese Informationen für alle virtuellen Computer in Ihr aktuelles Abonnement angezeigt.

Wenn der Befehl **Write-Host** **True**zurückgibt, ist der VM-Agent installiert. Wenn **False**zurückgegeben wird, finden Sie die Anleitung und einen Link zum Download in Azure Blog [VM und Extensions - Teil2](http://go.microsoft.com/fwlink/p/?LinkId=403947)buchen.

Wenn die VM-Agent installiert ist, werden führen Sie diese Befehle aus.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

Es dauert einige Minuten für den Agent zu starten, wenn es installiert ist. Danach müssen Sie Tiefe Sicherheit auf dem virtuellen Computer aktivieren, damit es eine Tiefe Sicherheits-Manager verwaltet werden kann. Finden Sie weitere Informationen:

- Trend-Artikel zu dieser Lösung [Instant-On Cloud-Sicherheit für Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- Ein [Windows PowerShell-Beispielskript](http://go.microsoft.com/fwlink/?LinkId=404100) zum Konfigurieren der virtuellen Computer
- [Anleitung](http://go.microsoft.com/fwlink/?LinkId=404099) für das Beispiel

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Anmelden bei einem virtuellen Computer unter Windows Server]

[Azure VM-Erweiterungen und Funktionen]


<!--Link references-->
[Anmelden bei einem virtuellen Computer unter Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Azure VM-Erweiterungen und Funktionen]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
