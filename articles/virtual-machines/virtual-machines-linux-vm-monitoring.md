<properties
   pageTitle="Aktivieren oder Deaktivieren der Überwachung von Azure VM"
   description="Beschreibt das Aktivieren oder Deaktivieren der Überwachung von Azure VM"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Aktivieren oder Deaktivieren von Azure VM überwachen

Dieser Abschnitt beschreibt das Aktivieren oder Deaktivieren der Überwachung auf virtuellen Computern Azure. Standardmäßig überwachen aktiviert ist auf Azure virtuelle Maschinen Wenn von [Azure-Portal](https://portal.azure.com) bereitgestellt und Überwachung Diagramme werden standardmäßig mit 1 Minute bereitgestellt. Sie können aktivieren oder Deaktivieren der Überwachung mit dem Portal oder Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure-CLI). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Aktivieren Sie / deaktivieren Sie der Überwachung durch den Azure-Portal
 
Sie können die Überwachung der Azure-VM, das Daten über die Instanz 1 Minuten ermöglicht. (Speicher verwenden). Detaillierte Diagnosedaten ist für die VM in Diagrammen Portal oder die API verfügbar. Standardmäßig Azure-Portal ermöglicht Überwachung, aber Sie können deaktivieren sie wie unten beschrieben. Sie können die Überwachung die VM ausgeführt oder im angehaltenen Zustand.

- Öffnen Sie die Azure Portal unter ** [https://portal.azure.com](https://portal.azure.com)**

- Klicken Sie im linken Navigationsbereich auf virtuellen Maschinen.

- Wählen Sie in der Liste virtuelle Computer eine Instanz ausgeführte oder beendete. Virtuellen Blad wird geöffnet.

- Klicken Sie auf "alle".

- Klicken Sie auf "Diagnose".

- Ändern Sie Status oder deaktivieren. Sie können auch auf diesem Blatt auswählen der Überwachungsebene Details, die Sie für den virtuellen Computer aktivieren möchten.

[Azure.Note] Diagnose auf Switch ist die Standardeinstellung, wenn Sie einen neuen virtuellen Computer erstellen

![Aktivieren Sie / deaktivieren Sie der Azure-Portal überwachen.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Aktivieren / Deaktivieren von Azure CLI-Überwachung
 
Zum Aktivieren der Überwachung für eine Azure-VM.

- Erstellen Sie eine Datei mit Namen wie PrivateConfig.json mit dem folgenden Inhalt.
        {"StorageAccountName": "das Speicherkonto Daten empfangen", "StorageAccountKey": "Schlüssel des Kontos"}
- Führen Sie den folgenden Azure CLI-Befehl.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Sie können auf eine höhere Version als verfügbar ab Version 2.0 ändern. 

Weitere Informationen zum Konfigurieren der Überwachung Metriken und Beispiele finden Sie auf Dokument - **[Verwenden Linux Diagnose Erweiterung Monitor Linux VM und Diagnose](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

