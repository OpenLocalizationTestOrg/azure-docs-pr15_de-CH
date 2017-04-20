<properties
   pageTitle="Öffnen Sie Ports einer VM mit Azure-Portal | Microsoft Azure"
   description="So öffnen Sie einen Port / Endpunkt Ihrer Windows-VM mit der Ressourcen-Manager-Bereitstellungsmodell in Azure-Portal erstellen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Öffnen von Ports einer VM in Azure mithilfe von Azure-portal
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Schnellzugriff
Sie können [diese Schritte mit Azure PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

Erstellen Sie zunächst die Netzwerk-Sicherheitsgruppe. Wählen Sie im Portal, klicken Sie auf **Hinzufügen**, suchen Sie und wählen Sie "Netzwerk-Sicherheitsgruppe":

![Eine Netzwerk-Sicherheitsgruppe hinzufügen](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Geben Sie einen Namen für die Sicherheitsgruppe Netzwerk wählen Sie oder erstellen Sie eine Ressourcengruppe und wählen Sie einen Speicherort. Klicken Sie auf **Erstellen** , wenn beendet:

![Erstellen Sie eine Netzwerk-Sicherheitsgruppe](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Wählen Sie die Netzwerk-Sicherheitsgruppe. "Eingehende Sicherheitsregeln, klicken Sie auf **Hinzufügen** , um eine Regel zu erstellen:

![Eine eingehende Regel hinzufügen](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Geben Sie einen Namen für die neue Regel. Port 80 ist standardmäßig bereits eingegeben. Dieses Blatt ist wo Sie Quelle, Protokoll und Ziel würde beim Netzwerk-Sicherheitsgruppe zusätzliche Regeln hinzufügen. Klicken Sie auf **OK** , um die Regel zu erstellen:

![Eine eingehende Regel erstellen](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Der letzte Schritt ist die Netzwerk-Sicherheitsgruppe ein Subnetz oder eine bestimmte Netzwerkschnittstelle zugeordnet. Ordnen Sie der Sicherheitsgruppe Netzwerk ein Subnetz. Subnetze", klicken Sie auf **zuordnen**:

![Ordnen Sie eine Sicherheitsgruppe Netzwerk ein Subnetz](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Wählen Sie das virtuelle Netzwerk, und wählen Sie die entsprechende Subnetz:

![Virtuelle Netzwerke Zuordnen einer Netzwerk-Sicherheitsgruppe](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Sie haben jetzt eine Netzwerk-Sicherheitsgruppe erstellt eine eingehende Regel, die Datenverkehr über Port 80 und einem Subnetz zugeordnet. Anschließen an das Subnetz VMs sind auf Port 80 erreichbar.


## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zu Netzwerk-Sicherheitsgruppen
Die schnelle Befehle ermöglichen mit der VM-Datenverkehr zu kommen. Netzwerk-Sicherheitsgruppen bieten viele hervorragende Funktionen und Granularität zum Steuern des Zugriffs auf Ressourcen. Erfahren Sie mehr über das [Erstellen einer Sicherheitsgruppe Netzwerk und ACL Regeln](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Als Teil der Azure-Ressourcen-Manager-Vorlagen können Sie Netzwerk-Sicherheitsgruppen und ACL-Regeln definieren. Mehr über das [Erstellen von Sicherheitsgruppen mit Vorlagen Netzwerk](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Verwenden Sie müssen verwenden Port Weiterleitung zu einem internen Anschluss VM einen eindeutigen externen Anschluss zuordnen, ein Lastenausgleich und Network Address Translation (NAT) Regeln. Beispielsweise möchten Sie TCP-Port 8080 extern verfügbar machen und Datenverkehr an TCP-Port 80 auf einer VM geleitet. Sie erfahren [erstellen ein Lastenausgleich Internetzugriff](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel wird eine einfache Regel HTTP-Datenverkehr erstellt. Finden Sie Informationen zum Erstellen von detaillierte Umgebung in den folgenden Artikeln:

- [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md)
- [Was ist Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure Ressourcenmanager Übersicht für Lastenausgleich](../load-balancer/load-balancer-arm.md)
