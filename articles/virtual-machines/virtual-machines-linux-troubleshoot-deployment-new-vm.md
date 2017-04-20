<properties
   pageTitle="Problembehandlung bei Linux VM-Bereitstellung-RM | Microsoft Azure"
   description="Problembehandlung Ressourcenmanager Bereitstellung beim Erstellen einer neuen virtuellen Linux-Maschine in Azure"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Die Problembehandlung Ressourcenmanager Bereitstellung erstellen Sie eine neue virtuellen Linux-Maschine in Azure

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Collect Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung Überwachungsprotokolle zum Problem zugeordneten Fehler zu identifizieren. Die folgenden Links enthalten Informationen über den Prozess.

[Problembehandlung bei Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit Ressourcen-Manager](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Wenn das Betriebssystem Linux verallgemeinert und es hochgeladen oder mit der allgemeinen Einstellung erfasst, werden keine Fehler vorhanden sein. Ebenso ist das Betriebssystem Linux spezialisiert, sie hochgeladen oder mit speziellen Einstellung erfasst, und Fehler nicht.

**Hochzuladen Sie Fehler:**

**N<sup>1</sup>:** Wenn das Betriebssystem Linux verallgemeinert und wird als hochgeladen spezialisiert, erhalten Sie einen Bereitstellung Timeoutfehler denn bei der Bereitstellung die VM ist.

**N<sup>2</sup>:** Ist das Betriebssystem Linux spezialisiert und hochgeladen wie generalized erhalten Sie Bereitstellung Fehler, da die neue VM mit den ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird.

**Auflösung:**

Um diese Fehler zu beheben, laden Sie ursprüngliche VHD verfügbar auf-Prem, dieselbe Einstellung wie für das Betriebssystem (generalized/spezialisiert). Hochladen als generalized run - müssen zunächst entziehen.

**Erfassen Sie Fehler:**

**N<sup>3</sup>:** Wenn das Betriebssystem Linux verallgemeinert und als werden spezialisiert, erhalten Sie einen Bereitstellung Timeoutfehler ist der ursprünglichen virtuellen nicht verwendbar gekennzeichnet ist als verallgemeinert.

**N<sup>4</sup>:** Ist das Betriebssystem Linux spezialisiert und werden als allgemeine erhalten Sie einen Bereitstellung Fehler, da die neue VM mit den ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird. Die ursprüngliche VM ist nicht verwendbar ist gekennzeichnet spezialisierte.

**Auflösung:**

Um diesen Fehler zu beheben, löschen Sie das aktuelle Bild aus dem Portal und [von der aktuellen VHDs](virtual-machines-linux-capture-image.md) dieselbe Einstellung wie für das Betriebssystem (generalized/spezialisiert)

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Problem: Benutzerdefinierte / Gallery / Marketplace-Bild; Fehler beim Reservieren
Dieser Fehler tritt in Situationen, wenn neue VM-Anforderung mit einem Cluster, die keine Unterstützung für die VM-Größe angefordert wird oder keinen verfügbaren Speicherplatz verknüpft für die Anfrage.

**Ursache 1:** Cluster kann die angeforderte VM-Größe nicht unterstützen.

**Lösung 1:**

- Wiederholen Sie die Anforderung mit einer kleineren Größe VM.
- Wenn die Größe der angeforderten VM geändert werden kann:
  - Beenden Sie alle virtuellen Computer in den Verfügbarkeit.
  Klicken Sie auf **Ressourcengruppen** > *der Ressourcengruppe* > **Ressourcen** > *der Verfügbarkeit* > **virtuellen Computer** > *virtuellen Computers* > **Beenden**.
  - Nach dem Beenden der VMs erstellen Sie neuen VM in der gewünschten Größe.
  - Zuerst die neue VM, wählen Sie die beendeten VMs und klicken Sie auf **Start**.

**Ursache 2:** Der Cluster hat keine Ressourcen freizugeben.

**Lösung 2:**

- Wiederholen Sie die Anforderung zu einem späteren Zeitpunkt.
- Wenn die neue VM Teil einer anderen Verfügbarkeit ist
  - Erstellen einer neuen VM eine unterschiedliche Verfügbarkeit (im selben Bereich) festgelegt.
  - Im gleichen virtuellen Netzwerk neue VM hinzufügen.

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme beim Starten von beendet Linux VM oder ändern eine vorhandene Linux VM in Azure, finden Sie unter [Problembehandlung Ressourcenmanager Bereitstellungsaspekte für Neustart oder das Ändern einer vorhandenen virtuellen Linux-Maschine in Azure](virtual-machines-linux-restart-resize-error-troubleshooting.md).
