<properties
   pageTitle="Problembehandlung bei Windows VM-Bereitstellung-RM | Microsoft Azure"
   description="Problembehandlung Ressourcenmanager Bereitstellung beim Erstellen eines neuen Windows virtuellen Computers in Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Ressourcenmanager Bereitstellung Problembehandlung mit einen neuen Windows-Computer in Azure erstellen

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Collect Überwachungsprotokolle

Sammeln Sie zum Starten der Problembehandlung Überwachungsprotokolle zum Problem zugeordneten Fehler zu identifizieren. Die folgenden Links enthalten Informationen über den Prozess.

[Problembehandlung bei Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md)

[Überwachen von Vorgängen mit Ressourcen-Manager](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Wenn das Betriebssystem Windows verallgemeinert ist und es hochgeladen oder mit der allgemeinen Einstellung erfasst sein nicht Fehler vorhanden. Ebenso ist das Betriebssystem Windows spezialisiert, übertragen und/oder mit der speziellen erfasst, und Fehler nicht.

**Hochzuladen Sie Fehler:**

**N<sup>1</sup>:** Wenn das Betriebssystem Windows verallgemeinert und als hochgeladen spezialisiert, erhalten Sie einen Bereitstellung Timeoutfehler mit VM OOBE Bildschirm fest.

**N<sup>2</sup>:** Ist das Betriebssystem Windows spezialisiert und ist als allgemeine hochgeladen erhalten Sie einen Bereitstellung Fehler mit der VM OOBE-Bildschirm hängen, da die neue VM mit den ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird.

**Auflösung**

Um diesen Fehler zu beheben, verwenden Sie [Add-AzureRmVhd die ursprüngliche VHD-Datei hochladen](https://msdn.microsoft.com/library/mt603554.aspx), verfügbaren lokal dieselbe Einstellung wie für das Betriebssystem (generalized/spezialisiert). Zum Hochladen verallgemeinert, denken Sie daran, Ausführen von Sysprep.

**Erfassen Sie Fehler:**

**N<sup>3</sup>:** Wenn das Betriebssystem Windows verallgemeinert und als werden spezialisiert, erhalten Sie einen Bereitstellung Timeoutfehler ist der ursprünglichen virtuellen nicht verwendbar gekennzeichnet ist als verallgemeinert.

**N<sup>4</sup>:** Ist das Betriebssystem Windows spezialisiert und werden als allgemeine erhalten Sie einen Bereitstellung Fehler, da die neue VM mit den ursprünglichen Computernamen, Benutzernamen und Kennwort ausgeführt wird. Die ursprüngliche VM ist nicht verwendbar ist gekennzeichnet spezialisierte.

**Auflösung**

Um diesen Fehler zu beheben, löschen Sie das aktuelle Bild aus dem Portal und [von der aktuellen VHDs](virtual-machines-windows-vhd-copy.md) dieselbe Einstellung wie für das Betriebssystem (generalized/spezialisiert)

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Problem: Galerie/Custom/Marketplace-Bild; Fehler beim Reservieren
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
Wenn Probleme beim Starten von beendet Windows VM oder Ändern einer vorhandenen Windows-VM in Azure, finden Sie unter [Problembehandlung Ressourcenmanager Bereitstellungsaspekte für Neustart oder das Ändern von vorhandenen Windows virtuellen Computer in Azure](virtual-machines-windows-restart-resize-error-troubleshooting.md).
