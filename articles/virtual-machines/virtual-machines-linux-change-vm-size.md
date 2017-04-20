<properties
   pageTitle="Ändern der Größe einer Linux VM | Microsoft Azure"
   description="Zum Vergrößern oder Verkleinern einer virtuellen Linux-Maschine durch VM ändern."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Ändern der Größe einer Linux VM

## <a name="overview"></a>Übersicht 

Nach der Bereitstellung einer virtuellen Maschine (VM) können skalieren die VM oben oder unten durch Ändern der [VM-Größe][vm-sizes]. In einigen Fällen müssen Sie zunächst die VM freigeben. Dies ist die neue Größe nicht auf der Hardware-Cluster, der die VM befindet.

Dieser Artikel zeigt, wie ein Linux VM mit der [Azure-CLI]Größe[azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.


## <a name="resize-a-linux-vm"></a>Ändern der Größe einer Linux VM 

Gehen Sie zum Ändern der Größe einer VM.

1. Führen Sie den folgenden CLI-Befehl. Dieser Befehl listet die VM-Größen, die auf Hardware-Clusters, der virtuellen Computer gehostet wird.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Wenn die gewünschte Größe aufgeführt ist, führen Sie den folgenden Befehl die VM Größe.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Dabei wird die VM neu gestartet. Nach dem Neustart werden Ihr vorhandenes Betriebssystem und Datenträger zugeordnet. Auf dem temporären Datenträger verloren.

    Verwenden der `--enable-boot-diagnostics` ermöglicht [Diagnose Boot][boot-diagnostics], sich alle Fehler zu starten.

3. Wenn die gewünschte Größe nicht aufgeführt ist führen Sie folgende Befehle auf den virtuellen Computer freigeben, Größe, und starten Sie die VM.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Aufheben der VM gibt auch der VM zugewiesenen dynamischen IP-Adressen frei. Betriebssystem und Daten-Festplatten sind nicht betroffen.
   
## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Skalierbarkeit mehrere VM-Instanzen ausführen und skalieren. Weitere Informationen finden Sie unter [Linux-Computern in einer virtuellen Maschine Skalierung automatisch skalieren][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md