<properties
    pageTitle="Erstellen und Hochladen einer VM-Image mit Powershell | Microsoft Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer allgemeinen Windows Server-Abbilds (VHD) mit der klassischen Bereitstellungsmodell Azure Powershell."
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
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Erstellen Sie und Hochladen Sie der virtuellen Festplatte Server Windows Azure

Dieser Artikel beschreibt, wie eigene allgemeinen VM-Image als virtuelle Festplatte (VHD) hochladen, damit Sie es verwenden, um virtuelle Computer erstellen. Finden Sie weitere Informationen zu Festplatten und Festplatten in Microsoft Azure [zu Festplatten und Festplatten für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Sie können auch [Hochladen](virtual-machines-windows-upload-image.md) einen virtuellen Computer mit dem Ressourcen-Manager-Modell. 

## <a name="prerequisites"></a>Erforderliche Komponenten

Es wird vorausgesetzt, dass Sie haben:

- **Ein Azure-Abonnement** - haben Sie einen kann [ein Azure-Konto frei](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - Sie haben Microsoft Azure PowerShell-Modul installiert und Ihr Abonnement konfiguriert. 

- **A . VHD-Datei** - unterstützten Windows-Betriebssystem in einer VHD-Datei gespeichert und einer virtuellen Maschine zugeordnet. Überprüfen Sie, ob die Serverrollen auf der virtuellen Festplatte von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Unterstützung von Sysprep für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] Die VHDX ist nicht in Microsoft Azure unterstützt. Sie können den Datenträger mit Hyper-V-Manager oder das [Cmdlet VHD konvertieren](http://technet.microsoft.com/library/hh848454.aspx)VHD-Format konvertieren. Details finden Sie in diesem [Blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Schritt 1: Vorbereiten der virtuellen Festplatte 

Bevor Sie die virtuelle Festplatte in Azure hochladen, muss mithilfe des Tools Sysprep verallgemeinert werden. Die virtuelle Festplatte als Bild verwendet werden kann. Weitere Informationen zu Sysprep finden Sie [wie mit Sysprep: Einführung](http://technet.microsoft.com/library/bb457073.aspx). Sichern Sie die VM vor dem Ausführen von Sysprep.

Dem virtuellen Computer, der das Betriebssystem installiert wurde, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei dem Betriebssystem.

2. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator. Wechseln Sie zu **%windir%\system32\sysprep**, und führen Sie `sysprep.exe`.

    ![Öffnen Sie ein Eingabeaufforderungsfenster](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Das Dialogfeld **Des Systemvorbereitungsprogramms** wird angezeigt.

    ![Sysprep starten](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  **Das Systemvorbereitungsprogramm**aktivieren Sie **Von-Box-Experience (OOBE) für System** und sicherstellen Sie, dass **verallgemeinern** aktiviert ist.

5.  Wählen Sie unter **Optionen für Herunterfahren** **Herunterfahren**.

6.  Klicken Sie auf **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Schritt 2: Erstellen Sie ein Speicherkonto und container

Sie benötigen ein Speicherkonto in Azure Sie die VHD-Datei hochgeladen haben. Dieser Schritt zeigt, wie ein Konto erstellen oder ein vorhandenes Konto die Informationen erhalten. Ersetzen Sie die Variablen in &lsaquo; Klammern &rsaquo; durch eigene Informationen.

1. Anmeldung

        Add-AzureAccount

1. Legen Sie Ihre Azure-Abonnement.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Erstellen Sie ein neues Speicherkonto. Der Name des Speicherkontos sollte eindeutig sein, 3 24 Zeichen. Der Name kann eine beliebige Kombination aus Buchstaben und Zahlen. Sie müssen auch anders"Ost" Speicherort angeben
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Das neue Speicher-Konto als Standard festgelegt.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Erstellen Sie einen neuen Container.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Schritt 3: Laden der VHD-Datei

Die virtuelle Festplatte hochladen [AzureVhd hinzufügen](http://msdn.microsoft.com/library/dn495173.aspx) können.

Die Azure PowerShell im vorherigen Schritt verwendet Geben Sie des folgenden Befehls, und Ersetzen Sie die Variablen in &lsaquo; Klammern &rsaquo; durch eigene Informationen.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Schritt 4: Hinzufügen der Liste benutzerdefinierte Bilder Bild

Verwenden Sie das Cmdlet [Hinzufügen AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) das Bild zur Liste der benutzerdefinierten Bilder hinzufügen.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt [eine benutzerdefinierte VM erstellen](virtual-machines-windows-classic-createportal.md) Bild hochladen.

