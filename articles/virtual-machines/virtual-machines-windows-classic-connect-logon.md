<properties
    pageTitle="Melden einer klassischen Azure VM | Microsoft Azure"
    description="Verwenden des klassischen Azure-Portals Anmelden bei einem Windows-Computer mit dem klassischen Bereitstellungsmodell erstellt."
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
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-classic-portal"></a>Melden Sie sich bei einem Windows-Computer mit klassischen Azure-portal

In klassischen Azure-Portal verwenden Sie die Schaltfläche **Verbinden** zu einer Remotedesktopsitzung einer VM Windows anmelden.

Möchten Sie die Verbindung zu einem Linux VM? Informationen Sie [zu einer virtuellen Maschine unter Linux anmelden](virtual-machines-linux-mac-create-ssh-keys.md).

Erfahren Sie, wie [diese Schritte mit neuen Azure-Portal](virtual-machines-windows-connect-logon.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] 

## <a name="video-walkthrough"></a>Video-Anleitung

Hier ist eine video-Anleitung die Schritte in diesem Lernprogramm. Es behandelt auch Endpunkte und öffentliche und private Ports für die Verbindung mit einem Windows-VM in Azure verwendet.

[AZURE.VIDEO logging-on-to-vm-running-windows-server-on-azure]


## <a name="connect-to-the-virtual-machine"></a>Verbinden Sie mit dem virtuellen Computer

1. Mit klassischen Azure-Portal anmelden.

2. Wählen Sie den virtuellen Computer auf **virtuellen Computern**

3. Klicken Sie auf der Befehlsleiste am unteren Rand der Seite **Verbinden**.

    ![Melden Sie sich am virtuellen Computer](./media/virtual-machines-windows-classic-connect-logon/connectwindows.png)
    
> [AZURE.TIP] Wenn die Taste **Connect** verfügbar ist, finden Sie Tipps zur Problembehandlung am Ende dieses Artikels.

## <a name="log-on-to-the-virtual-machine"></a>Melden Sie sich am virtuellen Computer

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Nächste Schritte

-   Wenn die Taste **Connect** inaktiv oder andere mit der Remotedesktopverbindung Probleme, versuchen Sie die Konfiguration. Dashboard virtuellen **Kurzer Blick**, klicken Sie auf **remote-Konfiguration zurückgesetzt**.
-   Versuchen Sie bei Problemen mit Ihrem Kennwort zurücksetzen. Vom Dashboard virtuellen **Kurzer Blick**, klicken Sie auf **Kennwort zurücksetzen**.

Diese Tipps nicht funktionsfähig sind, benötigen Sie nicht, finden Sie unter [Problembehandlung bei Remotedesktopverbindungen zu einem Windows-basierten Azure Virtual Machine](virtual-machines-windows-troubleshoot-rdp-connection.md). Dieser Artikel führt Sie durch die Diagnose und Behebung von Problemen.


