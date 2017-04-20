
<properties
    pageTitle="Azure RemoteApp Bild Anforderungen | Microsoft Azure"
    description="Erfahren Sie mehr über die Vorschriften für Bilder mit Azure RemoteApp"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Vorschriften für Azure RemoteApp Bilder

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp verwendet Windows Server 2012 R2 Bild alle Programme hosten, die Sie für die Benutzer freigeben möchten. Um ein benutzerdefiniertes Bild zu erstellen, beginnen Sie mit ein vorhandenes Abbild oder [eine neue erstellen](remoteapp-create-custom-image.md).

> [AZURE.TIP] Wussten Sie, dass Ihr Abonnement Azure RemoteApp eine Windows Server 2012 R2-Bild in der Galerie Azure VM enthält, mit denen Sie eigene Vorlage Bild [Überprüfen Sie sie](remoteapp-image-on-azurevm.md).  


Sind die Vorschriften für das Bild, das für die Verwendung mit Azure RemoteApp hochgeladen werden können:


- Benutzerdefinierte Anwendung Datenspeicher nicht lokal auf dem Bild. Diese Bilder sollten sind statusfrei und nur Programme.
- Das Bild enthält keine Daten, die verloren gehen können.
- Die Größe des Bildes sollte ein Vielfaches von MBs. Wenn Sie versuchen, ein Bild, das kein Vielfaches ist, schlägt der Upload fehl.
- Die Bildgröße muss 127 GB oder kleiner.
- Muss auf einer VHD-Datei (VHDX Dateien werden derzeit nicht unterstützt).
- Die virtuelle Festplatte darf einem Generation 2 virtuelle Computer.
- Die virtuelle Festplatte kann fester Größe oder Dyn. Eine dynamisch erweiterbare virtuelle Festplatte wird empfohlen, da weniger Zeit als ein fester Größe VHD-Datei in Azure hochladen.
- Der Datenträger muss mit der Master Boot Record (MBR) Partitionstyp initialisiert werden. Partitionsstil der GUID-Partitionstabelle (GPT) wird nicht unterstützt.
- Die virtuelle Festplatte muss eine einzige Installation von Windows Server 2012 R2 enthalten. Sie können mehrere Volumes, aber nur ein, die einer Installation von Windows enthalten.
- Die Funktion Remote Desktop Session Host (RDSH) und die Desktop Experience-Funktion müssen installiert sein.
- Die Remotedesktop-Verbindungsbroker-Rolle muss *nicht* installiert werden.
- Das Verschlüsseln File System (EFS) muss deaktiviert sein.
- Das Bild mithilfe der Parameter SYSPREPed muss **/oobe / generalize/shutdown** ( **/mode:vm** -Parameter nicht verwenden).
- Hochladen der virtuellen Festplatte aus einem Snapshot wird nicht unterstützt.

Finden Sie weitere Informationen zum Erstellen von Bildern für Azure RemoteApp [ein Azure RemoteApp-Abbild erstellen](remoteapp-imageoptions.md) .
