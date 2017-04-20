<properties
   pageTitle="VM neu starten oder Ändern der Größe Probleme | Microsoft Azure"
   description="Problembehandlung für Neustart oder das Ändern einer vorhandenen virtuellen Linux-Maschine in Azure klassische Bereitstellung"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Problembehandlung für Neustart oder das Ändern einer vorhandenen virtuellen Linux-Maschine in Azure klassische Bereitstellung

> [AZURE.SELECTOR]
- [Classic](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Ressourcen-Manager](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Beim Starten einer angehaltenen Azure Virtual Machine (VM) verändern oder eine vorhandene Azure VM ist häufige Fehler auftreten einer Reservierungsfehler. Dieser Fehler tritt auf, wenn der Cluster Region keine Ressourcen hat oder die angeforderte VM-Größe nicht unterstützen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Collect Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung Überwachungsprotokolle zum Problem zugeordneten Fehler zu identifizieren.

In Azure-Portal klicken Sie auf **Durchsuchen** > **virtuellen Computer** > _der virtuellen Linux-Maschine_ > **Settings** > **Audit-Protokolle**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fehlermeldung beim Starten von beendeten VM

Sie versuchen, einen angehaltenen virtuellen Computer zu starten, aber ein Reservierungsfehler.

### <a name="cause"></a>Ursache

Die Anforderung beendete VM muss am ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Cluster muss jedoch nicht der Anforderung verfügbaren Speicherplatz.

### <a name="resolution"></a>Auflösung

* Erstellen Sie neue Clouddienst und ordnen Sie sie entweder einen Bereich oder ein virtuelles Netzwerk bereichsspezifische aber keiner Gruppe Affinität.

* Löschen Sie den angehaltenen virtuellen Computer

* VM in der neuen Cloud-Dienst mithilfe der Datenträger neu.

* Starten Sie den neu erstellten virtuellen Computer.

Wenn Sie beim Erstellen eines neuen Cloud-Diensts einen Fehler erhalten, zu einem späteren Zeitpunkt wiederholen Sie oder ändern Sie den Bereich für den Clouddienst.

> [AZURE.IMPORTANT] Neue Cloud-Dienst hat einen neuen Namen und VIP daher müssen Sie diese Informationen für die Abhängigkeit ändern, diese Informationen für den Cloud-Dienst.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Fehler beim Ändern der Größe einer vorhandenen VM

Sie versuchen, eine vorhandene VM Größe aber ein Reservierungsfehler erhalten.

### <a name="cause"></a>Ursache

Die Anforderung die VM Größe hat am ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Cluster unterstützt jedoch nicht die angeforderte Größe VM.

### <a name="resolution"></a>Auflösung

Verkleinern Sie die angeforderte VM und wiederholen Sie die Anforderung Größe.

* Klicken Sie **Alle** > **virtuellen Maschinen (classic)** > _virtuellen Computers_ > **Settings** > **Größe**. Ausführliche Schritte finden Sie in der [Größe der virtuellen Computer](https://msdn.microsoft.com/library/dn168976.aspx).

Ist es nicht möglich, VM verkleinern, gehen Sie folgendermaßen vor:

  * Neue Cloud-Dienst erstellen, es ist nicht mit einer Gruppe verbunden und ein virtuelles Netzwerk, die mit einer Gruppe zugeordnet.

  * Erstellen einer neuen, größere VM.

Konsolidieren Sie Ihre virtuellen Computer im selben Cloud-Dienst. Wenn Ihre vorhandenen Cloud-Dienst bereichsspezifische virtuelles Netzwerk zugeordnet ist, können Sie neue Cloud-Dienst mit dem vorhandenen virtuellen Netzwerk verbinden.

Wenn der vorhandenen Cloud-Dienst nicht bereichsspezifische virtuelles Netzwerk zugeordnet ist, müssen Sie die VMs in der Cloud-Dienst löschen und neue Cloud Service von Datenträgern erstellen. Allerdings ist es wichtig zu bedenken, dass neue Cloud-Dienst hat einen neuen Namen und VIP, müssen Sie für alle Abhängigkeiten aktualisieren, die derzeit diese Informationen für den Cloud-Dienst.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Erstellen einer neuen Linux VM in Azure Probleme, finden Sie unter [Problembehandlung Probleme bei der Bereitstellung einer neuen virtuellen Linux-Maschine in Azure erstellen](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
