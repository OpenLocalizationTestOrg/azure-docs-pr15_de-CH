<properties
   pageTitle="VM neu starten oder Ändern der Größe Probleme | Microsoft Azure"
   description="Die Problembehandlung Ressourcenmanager Bereitstellung mit Neustart oder das Ändern einer vorhandenen virtuellen Linux-Maschine in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Die Problembehandlung Ressourcenmanager Bereitstellung mit Neustart oder das Ändern einer vorhandenen virtuellen Linux-Maschine in Azure

Beim Starten einer angehaltenen Azure Virtual Machine (VM) verändern oder eine vorhandene Azure VM ist häufige Fehler auftreten einer Reservierungsfehler. Dieser Fehler tritt auf, wenn der Cluster Region keine Ressourcen hat oder die angeforderte VM-Größe nicht unterstützen.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Collect Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung Überwachungsprotokolle zum Problem zugeordneten Fehler zu identifizieren. Die folgenden Links enthalten Informationen über den Vorgang:

[Problembehandlung bei Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit Ressourcen-Manager](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problem: Fehlermeldung beim Starten von beendeten VM

Sie versuchen, einen angehaltenen virtuellen Computer zu starten, aber ein Reservierungsfehler.

### <a name="cause"></a>Ursache

Die Anforderung beendete VM muss am ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Cluster muss jedoch nicht der Anforderung verfügbaren Speicherplatz.

### <a name="resolution"></a>Auflösung

*   Beenden Sie alle virtuellen Computer in den Verfügbarkeit und starten Sie jede VM.

  1. Klicken Sie auf **Ressourcengruppen** > _der Ressourcengruppe_ > **Ressourcen** > _der Verfügbarkeit_ > **virtuellen Computer** > _virtuellen Computers_ > **Beenden**.

  2. Nach dem Beenden der VMs beendet VMs wählen Sie, und klicken Sie auf Start.

*   Wiederholen Sie die neustartanforderung zu einem späteren Zeitpunkt.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problem: Fehler beim Ändern der Größe einer vorhandenen VM

Sie versuchen, eine vorhandene VM Größe aber ein Reservierungsfehler erhalten.

### <a name="cause"></a>Ursache

Die Anforderung die VM Größe hat am ursprünglichen Cluster versucht, die Cloud-Dienst hostet. Cluster unterstützt jedoch nicht die angeforderte Größe VM.

### <a name="resolution"></a>Auflösung

* Wiederholen Sie die Anforderung mit einer kleineren Größe VM.

* Wenn die Größe der angeforderten VM geändert werden kann:

  1. Beenden Sie alle virtuellen Computer in den Verfügbarkeit.

    * Klicken Sie auf **Ressourcengruppen** > _der Ressourcengruppe_ > **Ressourcen** > _der Verfügbarkeit_ > **virtuellen Computer** > _virtuellen Computers_ > **Beenden**.

  2. Nach dem Beenden der VMs Größe der gewünschten VM zu vergrößern.
  3. Wählen Sie Größe VM und beendeten VMs starten Sie klicken Sie auf **Start**.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Erstellen einer neuen Linux VM in Azure Probleme, finden Sie unter [Problembehandlung Probleme bei der Bereitstellung einer neuen virtuellen Linux-Maschine in Azure erstellen](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
