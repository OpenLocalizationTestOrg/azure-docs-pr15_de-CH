<properties
    pageTitle="Erstellen von Azure RemoteApp Bildes anhand einer Azure VM | Microsoft Azure"
    description="Erfahren Sie, wie ein Bild mit einer Azure Virtual Machine für Azure RemoteApp zu erstellen."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Basierend auf einer virtuellen Maschine Azure Azure RemoteApp Image Erstellen

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp-Bilder (der in der Auflistung Teilen apps enthalten) erstellen Azure virtuellen Computer. Können auch Abbild eines virtuellen Computers verwenden wir Azure VM-Bildergalerie hinzugefügt, die alle die Azure RemoteApp Bild erfüllt – können diese VM-Image als Ausgangspunkt für eigene VM soll. Achten Sie einfach auf das Bild "Windows Server Remote Desktop Session Host" in der Bibliothek.

Es gibt zwei Schritte erstellen Sie Ihr eigenes Bild basierend auf einer Azure VM - Bild erstellen und dann aus der Azure-VM-Bibliothek Azure RemoteApp hochladen.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Erstellen eines benutzerdefinierten Bildes anhand einer Azure VM

Verwenden Sie Schritte zum Erstellen eines Abbildes basierend auf einer Azure VM.

1. Erstellen Sie einen Azure virtuellen Computer. "Windows Server Remote Desktop Session Host" oder "Windows Server Remote Desktop Session Host mit Microsoft Office 365 ProPlus" Image können von Azure Virtual Machine-Bildergalerie. Dieses Bild erfüllt alle Azure RemoteApp Vorlage Bild.

    Details finden Sie unter [Erstellen einer VM Windows ausgeführt](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Verbindung zur VM und installieren und Konfigurieren der apps über RemoteApp freigeben möchten. Stellen Sie sicher, Ihre apps erforderlichen zusätzlichen Windows Konfigurationen ausführen.

    Weitere Informationen finden Sie unter [Anmelden bei einem virtuellen Computer ausführen von Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Wenn Sie eine Windows Server Remotedesktop-Sitzungshost-Bilder verwenden, ist ein Validierungsskript enthalten, die sicherstellen, dass die VM RemoteApp Prä-reqs. erfüllt Um das Skript auszuführen, doppelklicken Sie auf **ValidateRemoteAppImage** auf dem Desktop. Sicherstellen Sie, dass alle vom Skript gemeldeten Fehler vor dem Fortfahren mit dem nächsten Schritt behoben werden.

4. SYSPREP generalize und Aufnahme. Weitere Informationen finden Sie [wie einem Windows-Computer als Vorlage verwenden](../virtual-machines/virtual-machines-windows-classic-capture-image.md) .



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Importieren Sie das Bild in der Bildbibliothek Azure RemoteApp

Gehen Sie folgendermaßen vor, um das neue Bild in Azure RemoteApp importieren:

1. Auf der Registerkarte **Vorlage Bilder** :
    - Wenn Sie keine vorhandene Bilder, klicken Sie auf **Hochladen oder Importieren einer Vorlage**.
    - Wenn Sie bereits mindestens ein Bild haben, klicken Sie auf **+** ein neues Bild hinzufügen.

2. Wählen Sie Bibliothek **Importieren eines Bildes, die virtuellen Computer aus** und dann auf **Weiter**.

3. Wählen Sie auf der nächsten Seite das benutzerdefinierte Abbild aus und bestätigen Sie, dass die Schritte aufgelistet, wenn Sie das Bild erstellt. Klicken Sie auf **Weiter**.
4. Geben Sie einen Namen für das neue RemoteApp-Bild und wählen Sie die Position und klicken Sie auf das Häkchen, um den Importvorgang zu starten.

> [AZURE.NOTE] Sie können Bilder von Azure überall von Azure Virtual Machines in Azure überall von Azure RemoteApp unterstützt unterstützt. Die Positionen kann der Import bis zu 25 Minuten dauern.

Jetzt können Sie die neue Sammlung ein [Cloud](remoteapp-create-cloud-deployment.md) -Sammlung oder [hybride](remoteapp-create-hybrid-deployment.md)Bedarf erstellen.
