<properties
    pageTitle="Erstellen eine VM im klassischen Portal | Microsoft Azure"
    description="Erstellen Sie eine virtuellen Windows-Maschine im klassischen Azure-Portal."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Eine virtuelle Maschine auf Windows Azure-Verwaltungsportal

> [AZURE.SELECTOR]
- [Azure-Verwaltungsportal](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: Bereitstellung klassisch](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie **neue Azure-Portal** [gehen mit dem Ressourcen-Manager-Bereitstellungsmodell](virtual-machines-windows-hero-tutorial.md) . 

Dieses Lernprogramm zeigt eine auf Windows Azure-Verwaltungsportal Azure Virtual Machine (VM) erstellen. Windows Server-Abbilds verwenden wir als Beispiel, aber das ist nur eines von vielen Bildern Azure bietet. Beachten Sie, dass Ihr Abonnement die Auswahlmöglichkeiten abhängen. Beispielsweise können Windows desktop-Images für MSDN-Abonnenten verfügbar.

In diesem Abschnitt wird veranschaulicht, wie mithilfe **Von Galerie** -Option im klassischen Azure-Portal Erstellen des virtuellen Computers. Diese Option bietet mehr Konfigurationsoptionen als Option **Schnell erstellen** . Wenn Sie einen virtuellen Computer zu einem virtuellen Netzwerk beitreten möchten, müssen Sie die Option **Aus Galerie** .

Sie können auch VMs mit [Ihren eigenen Bildern](virtual-machines-windows-classic-createupload-vhd.md)erstellen. Weitere Informationen zu dieser und anderen Methoden finden Sie unter [verschiedene Verfahren zum Erstellen einer virtuellen Windows-Maschine](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Video-Anleitung

Hier ist eine Anleitung in diesem Lernprogramm.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Virtuellen Computer erstellen

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie im neuen Azure-Portal [Erstellen Sie einen virtuellen Computer mit dem Ressourcen-Manager-Bereitstellungsmodell](virtual-machines-windows-hero-tutorial.md) . 

- Melden Sie sich am virtuellen Computer an. Hinweise finden Sie in [einer virtuellen Maschine unter Windows Server anmelden](virtual-machines-windows-classic-connect-logon.md).

- Legen Sie einen Datenträger zum Speichern von Daten. Sie können anfügen, leere Datenträger und Laufwerke, die Daten enthalten. Eine Anleitung finden Sie unter [Attach einen Datenträger auf einem Windows-Computer mit dem klassischen Bereitstellungsmodell erstellt](virtual-machines-windows-classic-attach-disk.md).
