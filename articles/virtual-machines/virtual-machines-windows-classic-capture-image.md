<properties
    pageTitle="Zeichnen Sie ein Abbild einer Windows Azure-VM | Microsoft Azure"
    description="Zeichnen Sie ein Abbild eines virtuellen Computers Windows Azure mit klassischen Bereitstellungsmodell erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Zeichnen Sie ein Abbild eines virtuellen Computers Windows Azure mit klassischen Bereitstellungsmodell erstellt.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcenmanager Modellinformationen finden Sie unter [Erstellen einer Kopie Windows VM in Azure ausgeführt](virtual-machines-windows-vhd-copy.md).


Dieser Artikel veranschaulicht das Erfassen einer Azure virtuellen Maschine unter Windows, als Bild Verwendung andere virtuelle Computer erstellen. Hierzu zählen die Betriebssystem-Datenträger und alle Datenträger, die den virtuellen Computer zugeordnet sind. Netzwerkkonfiguration auch so müssen Sie diese konfigurieren, wenn Sie die virtuellen Computer erstellen, das Bild.

Azure speichert das Bild unter **Eigene Bilder**. Dies ist die gleiche finden Sie alle Bilder, die Sie hochgeladen haben. Weitere Informationen über Bilder finden Sie unter [Bilder für virtuelle Computer](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Bevor Sie beginnen##

Diese Schritte setzen voraus, da Sie bereits einen Azure virtuellen Computer erstellt und konfiguriert das Betriebssystem hängen alle Datenträger. Wenn Sie dies noch nicht getan, finden Sie folgendermaßen:

- [Erstellen Sie einen virtuellen Computer aus einem Bild](virtual-machines-windows-classic-createportal.md)
- [Wie einen Datenträger mit einem virtuellen Computer](virtual-machines-windows-classic-attach-disk.md)
- Stellen Sie sicher, dass die Serverrollen mit Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Unterstützung von Sysprep für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Dieser Vorgang löscht den ursprünglichen virtuellen Computer nach erfasst wird. 

Vor Caputuring ein Bild von Azure Virtual Machine wird empfohlen, der virtuelle Zielcomputer gesichert werden. Azure virtuelle Maschinen können mit Azure Backup gesichert werden. Details finden Sie in [Azure virtuelle Computer sichern](../backup/backup-azure-vms.md). Andere Lösungen sind Partner ab. Was derzeit suchen Sie Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>Der virtuelle Computer erfassen

1. Im [klassischen Azure-Portal](http://manage.windowsazure.com)mit dem virtuellen Computer **Verbinden** . Eine Anleitung finden Sie unter [einer virtuellen Maschine unter Windows Server anmelden] [].

2.  Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.

3.  Wechseln Sie zu `%windir%\system32\sysprep`, und führen Sie sysprep.exe.

4.  Das Dialogfeld **Des Systemvorbereitungsprogramms** wird angezeigt. Folgendermaßen Sie vor:

    - **Systembereinigungsaktion**wählen Sie **System geben Sie Out-of-Box-Experience (OOBE aus)** und sicherstellen Sie, dass **verallgemeinern** aktiviert ist. Weitere Informationen zum Verwenden von Sysprep finden Sie [wie mit Sysprep: Einführung][].

    - Wählen Sie unter **Optionen für Herunterfahren** **Herunterfahren**.

    - Klicken Sie auf **OK**.

    ![Ausführen von Sysprep](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep fährt den virtuellen Computer, ändert sich den Status des virtuellen Computers im klassischen Azure-Portal **beendet**.

8.  In der Azure-Verwaltungsportal auf **virtuelle Computer** und den virtuellen Computer, die Sie erfassen möchten.

9.  Klicken Sie auf der Befehlsleiste auf **erfassen**.

    ![Virtuelle Maschine aufnehmen](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    **Erfassen Sie den virtuellen Computer** -Dialogfeld wird angezeigt.

10. **Image-Name**Namen Sie einen für das neue Bild.

11. Bevor Sie Windows Server-Abbilds zu einem Satz von benutzerdefinierten Bilder hinzufügen, müssen verallgemeinert werden durch Ausführen von Sysprep, wie in den vorherigen Schritten beschrieben. Klicken Sie, um anzugeben, dass habe **ich habe Sysprep auf dem virtuellen Computer ausgeführt** .

12. Klicken Sie auf das Häkchen, um das Abbild aufzuzeichnen. Das neue Bild wird jetzt unter **Bilder**.

    ![Erfolgreiche Aufnahme](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Nächste Schritte

Das Bild kann verwendet werden, um virtuelle Computer erstellen. Hierzu erstellen Sie einen virtuellen Computer mit dem Menüelement **Aus Galerie** und Auswählen des soeben erstellten Abbilds. Informationen finden Sie unter [Erstellen eines virtuellen Computers von einem Bild](virtual-machines-windows-classic-createportal.md).



[Anmelden bei einem virtuellen Computer unter Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Verwendung von Sysprep: Einführung]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
