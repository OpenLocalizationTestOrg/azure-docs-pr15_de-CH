<properties 
   pageTitle="Verwalten von DNS-Server von einem virtuellen Netzwerk (VNet)"
   description="Informationen Sie zum Hinzufügen und Entfernen von DNS-Servern in einem virtuellen Netzwerk (Vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Verwalten von DNS-Server von einem virtuellen Netzwerk (VNet)

Sie können die Liste der DNS-Server im VNet im Verwaltungsportal oder in der Konfigurationsdatei Netzwerk verwalten. Sie können bis zu 12 DNS-Server für jede VNet hinzufügen. Beim DNS-Server angeben, ist es wichtig sicherzustellen, dass die Liste der DNS-Server in der richtigen Reihenfolge für Ihre Umgebung. Round Robin funktionieren-DNS-Serverlisten nicht. Sie wird in der Reihenfolge angegeben sind. Ist der erste DNS-Server in der Liste erreichbar sein, verwendet der Client diesen DNS-Server unabhängig davon, ob der DNS-Server einwandfrei oder nicht funktioniert. Zum Ändern der DNS-Server-Reihenfolge für das virtuelle Netzwerk entfernen Sie DNS-Server, und fügen Sie sie in die gewünschte Reihenfolge.

>[AZURE.WARNING] Nach der Aktualisierung der DNS-Liste müssen Sie die virtuellen Computer im virtuellen Netzwerk befindet, damit die neue Einstellung der DNS-Server aufnehmen starten. Virtuelle Computer weiterhin ihre aktuelle Konfiguration verwenden, bis sie neu gestartet.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Bearbeiten einer DNS-Serverliste für ein virtuelles Netzwerk mit den Management-Portal

1. Melden Sie sich im **Verwaltungsportal**.

1. Klicken Sie im Navigationsbereich auf **Netzwerke**und klicken Sie dann auf den Namen des virtuellen Netzwerkes in der Spalte **Name** .

1. Klicken Sie auf **Konfigurieren**.

1. **DNS-Server**können Sie Folgendes konfigurieren:

    - **(Hinzufügen) ein neues DNS Server registrieren** Geben Sie einfach den Namen und die IP-Adresse in die Felder. Das virtuelle Netzwerk Liste DNS-Server einen DNS-Server hinzugefügt und Azure auch den DNS-Server registriert.

    - **Einen DNS-Server hinzufügen, der bereits registriert wurde,** Wenn Sie einen DNS-Server mit Azure registriert, können Sie es von vorkonfigurierter Liste auswählen.

    - **Entfernen ein DNS-Servers aus dem virtuellen Netzwerk –** Klicken Sie auf das X neben dem Server, den Sie entfernen möchten. Beachten Sie, dass dies nur den Server aus diesem virtuellen Netzwerk entfernt. Der DNS-Server bleibt im Azure für die virtuelle Netzwerke verwenden. Um einen DNS-Server aus dem Abonnement zu löschen, wechseln Sie zur Seite **Netzwerke -> DNS-Server** .

    - **DNS-Server – neu anordnen** Entfernen Sie alle DNS-Server, die aufgeführt sind, und fügen Sie diese in der Reihenfolge wieder, die Sie möchten. Beachten Sie, dass dies keine Round Robin DNS-Liste ist.

    - **Einen DNS-Server – umbenennen** Markieren Sie den DNS-Server in der Liste und geben Sie den neuen Namen. Dies wird registriert einen neuen DNS-Server in Azure sowie die Liste der DNS-Server für das virtuelle Netzwerk hinzufügen. Die alten DNS-Server und die IP-Adresse bleibt für Azure registriert. Sie können auf der Seite **DNS-Server** löschen, wenn Sie es nicht für virtuelle Netzwerke verwenden.

1. Klicken Sie am unteren Rand der Seite **Speichern** , um die neue Konfiguration ein DNS-Server speichern.

1. Starten Sie die virtuellen Computer im virtuellen Netzwerk können zu neuen DNS-Clienteinstellungen befindet.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Bearbeiten einer DNS-Server mit einer Netzwerk-Konfigurationsdatei

Um eine Liste der DNS-Server mithilfe einer Netzwerk-Konfigurationsdatei bearbeiten, exportieren Sie zunächst die Einstellungen aus dem Verwaltungsportal. Sie dann Netzwerk-Konfigurationsdatei bearbeiten und über das Verwaltungsportal importieren. Es folgt eine allgemeine Liste der Schritte dieses Vorgangs.

1. Exportieren Sie virtuelle Netzwerkkonfiguration in einer Netzwerk-Konfigurationsdatei. Weitere Informationen und Schritte zum Exportieren der Netzwerk-Konfigurationen finden Sie unter [Exportieren von Virtual Network Settings in eine Netzwerk-Konfigurationsdatei](virtual-networks-using-network-configuration-file.md).

1. Geben Sie die DNS-Serverinformationen für das virtuelle Netzwerk. Weitere Informationen zum Festlegen eines DNS-Servers finden Sie unter [einen DNS-Server in einem virtuellen Netzwerk Konfigurationsdatei angeben](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Weitere Informationen zu Netzwerk-Konfigurationsdateien finden Sie unter [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) und [einem virtuellen Netzwerk mithilfe einer Netzwerk-Konfigurationsdatei konfigurieren](virtual-networks-using-network-configuration-file.md).

1. Importieren Sie Netzwerk-Konfigurationsdatei. Weitere Informationen und Schritte zum Importieren Ihrer Netzwerk-Konfigurationsdatei finden Sie unter [Importieren einer Konfigurationsdatei Netzwerk](virtual-networks-using-network-configuration-file.md).

1. Starten Sie die virtuellen Computer im virtuellen Netzwerk können zu neuen DNS-Clienteinstellungen befindet.
