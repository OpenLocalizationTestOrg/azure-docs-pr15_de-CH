<properties
    pageTitle="Häufig gestellte Fragen für Azure | Microsoft Azure"
    description="Azure Stapel gestellte häufig Fragen."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Häufig gestellte Fragen für Azure

## <a name="deployment"></a>Bereitstellung

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Muss ich meine Datenträger formatieren, bevor Sie starten oder Neustarten einer Installations?

Datenträger sollte im raw-Format. Wenn Sie das Betriebssystem installieren, müssen Sie prüfen, ob alte Speicherpool noch vorhanden ist und löschen Sie die folgenden Schritte aus:

1. Öffnen Sie Server-Manager.
2. Wählen Sie Speicher-Pools.
3. Angezeigt, wenn ein Speicherpool aufgeführt ist.
4. Klicken Sie auf **Speicher-Pool** , wenn aufgeführten Lesen aktivieren und schreiben.
5. **Virtuelle Festplatte** Maustaste (unten links) und wählen Sie löschen.
6. **Speicher-Pools** und löschen.
7. Starten Sie Azure-Stack-Skript erneut und dass Datenträger Verifizierung erfolgreich.

Optional kann das folgende Skript verwendet werden:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Kann ich alle SSD-Datenträger für den Speicherpool POC-Installation verwenden?

Diese Konfiguration wird in dieser Version nicht unterstützt.  Weitere Informationen finden Sie im [Leitfaden](azure-stack-deploy.md) für Weitere Informationen.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Kann ich NVMe Datenträger für Microsoft Azure Stapel POC verwenden?

Storage Spaces direkte NVMe Laufwerke unterstützt, unterstützt Azure nur eine Teilmenge der möglichen Laufwerkstypen und möglichen Kombinationen für Storage Spaces direkt. 

### <a name="how-can-i-reinstall-azure-stack"></a>Wie kann ich Azure Stack neu zu installieren?
Sie können die Schritte im [Handbuch für die erneute Bereitstellung](azure-stack-redeploy.md)folgen.  

## <a name="tenant"></a>Mieter

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Kann ich meine eigenen Bilder als Mieter bereitstellen?

Ja, kann wie in Azure Mieter Bilder im Stapel Azure hochladen neben Bilder Dienstadministrator. Eine Übersicht finden Sie unter [Hinzufügen einer VM-Image](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testen

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Können ich geschachtelt Virtualisierung Microsoft Azure Stapel POC testen?

Geschachtelte Virtualisierung wird nicht unterstützt oder Azure Stapel Technical Preview 2 getestet.

## <a name="virtual-machines"></a>Virtuelle Computer

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Unterstützt Azure Stapel dynamische Datenträger für virtuelle Maschinen?

Azure Stapel unterstützt keine dynamische Festplatten.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung](azure-stack-troubleshooting.md)
