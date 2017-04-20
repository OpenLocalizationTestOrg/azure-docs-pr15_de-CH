<properties 
   pageTitle="Accelerated für einen virtuellen Computer - Portal | Microsoft Azure"
   description="Informationen Sie zum Konfigurieren von Accelerated Netzwerk für eine Azure Virtual Machine über das Azure-Portal."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Schnellere Netzwerke für eine virtuelle Maschine

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Schnellere Netzwerke kann einzelne Stamm e/a-Virtualisierung (SR-IOV) zu einer virtuellen Maschine (VM) die Netzwerke Leistung erheblich verbessert. Dieser leistungsfähige Pfad umgeht Server reduzieren Latenz, Jitter und CPU-Auslastung mit der anspruchsvollsten Arbeitslasten Netzwerk auf unterstützten VM Datapath. Erläutert, wie mit der Azure-Verwaltungsportal schnellere Netzwerke in Azure-Ressourcen-Manager-Bereitstellungsmodell konfigurieren. Sie können auch eine VM beschleunigt Vernetzung mit Azure PowerShell erstellen. Um zu erfahren, klicken Sie auf das PowerShell am Anfang dieses Artikels.

Die folgende Abbildung zeigt die Kommunikation zwischen zwei virtuellen Maschinen (VM) mit und ohne Netzwerk beschleunigt:

![Vergleich](./media/virtual-network-accelerated-networking-portal/image1.png)

Ohne Netzwerk beschleunigt muss alle Netzwerkverkehrs und die VM Host und dem virtuellen Switch durchlaufen. Virtuelle Switch bietet alle durchsetzen, wie Netzwerk-Sicherheitsgruppen, Zugriffssteuerungslisten, Isolierung und andere virtualisiert Netzwerkdienste Netzwerkverkehr. Weitere Artikel der [Netzwerkvirtualisierung Hyper-V und virtuellen Switch](https://technet.microsoft.com/library/jj945275.aspx) .

Schnellere Netzwerke Netzwerkverkehr erreicht die Netzwerkkarte (NIC) mit der VM weitergeleitet. Alle virtuelle Switch ohne schnellere Netzwerke gilt Netzwerkrichtlinien sind ausgelagert und Hardware. Richtlinien Hardware kann NIC Netzwerk Verkehr direkt an die VM Host und dem virtuellen Switch Beibehaltung der Richtlinie im Host verwendete umgehen.

Die Vorteile von Accelerated gelten nur für den virtuellen Computer aktiviert ist. Optimale Ergebnisse bietet diese Funktion mindestens zwei VMs mit demselben VNet verbunden. Bei der Kommunikation über VNets oder lokale Verbindung hat diese Funktion einen minimalen insgesamt Latenz.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Vorteile

- **Geringere Latenz / höhere Pakete pro Sekunde (Pps):** Die Datapath virtuellen Switch entfernt Zeit Pakete den Host für die Verarbeitung der Gruppenrichtlinie entfernt und erhöht die Anzahl der Pakete, die innerhalb des virtuellen Computers verarbeitet werden können.
- **Reduziert Zittern:** Virtueller Switch Verarbeitung abhängt und die Auslastung der CPU, die die Verarbeitung der Richtlinie, die angewendet werden muss. Entlastung der Durchsetzung der Hardware entfernt diese Variabilität durch die Bereitstellung von Paketen direkt an die VM Host zu VM Kommunikation und alle Software-Interrupts Kontextwechsel entfernen.
- **Verringert CPU-Auslastung:** Umgehen des virtuellen Switches im Host führt zu weniger CPU-Auslastung für die Verarbeitung von Netzwerkverkehr.

## <a name="limitations"></a>Grenzen

Die folgenden Nachteile vorhanden sein, wenn Sie diese Funktion verwenden:
 
- **Netzwerk-Schnittstelle erstellen:** Schnellere Netzwerke kann nur für eine neue Netzwerkschnittstelle aktiviert.  Es kann nicht auf eine vorhandene Netzwerkschnittstelle aktiviert.
- **VM erstellen:** Eine Netzwerkschnittstelle aktiviert schnellere Netzwerke kann nur einer VM angefügt werden, wenn die VM erstellt wird. Die Netzwerkschnittstelle kann eine vorhandene VM angefügt werden.
- **Regionen:** Im Westen zentralen USA und West Europa Azure nur angeboten. Der Satz von Bereichen wird in Zukunft erweitert.
- **Unterstützte Betriebssystem:** Microsoft Windows Server 2012 R2 und WindowsServer 2016 Technologievorschau 5. Unterstützung für Linux und Windows Server 2012 wird in Kürze.
- **Virtueller Speicher:** Standard_D15_v2 und Standard_DS15_v2 sind die einzigen VM instanzgrößen unterstützt. Weitere Informationen finden Sie im Artikel [Windows VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md) . Der Satz von unterstützten Größen von VM-Instanz wird in Zukunft erweitert.

Ändert diese Einschränkungen werden durch die Seite [Azure Virtual Networking Updates](https://azure.microsoft.com/updates/accelerated-networking-in-preview) bekannt.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Erstellen eines Windows-VM mit schnellere Netzwerke

1. Registrieren Sie für die Vorschau per e-Mail, [Schnellere Netzwerke Abonnements](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) mit Ihrem Abonnement und Verwendungszweck. Führen Sie die verbleibenden Schritte erst nachdem Sie per E-mail darüber informiert, dass Sie in der Vorschau angenommen haben.
2. Melden Sie das Azure-Portal unter http://portal.azure.com.
3. Erstellen einer VM der Schritte im Artikel [Erstellen einer Windows-VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) die folgenden Optionen auswählen:
    - Wählen Sie ein Betriebssystem im Abschnitt Einschränkungen aufgeführt.
    - Wählen Sie einen Speicherort (Bereich) im Abschnitt Einschränkungen aufgeführt.
    - Wählen Sie eine VM-Größe im Abschnitt Einschränkungen aufgeführt. Unterstützten Größen aufgeführt, klicken Sie auf **Alle anzeigen** **Wählen Sie eine Größe** Blatt einer erweiterten Liste eine Größe aus.
    - Blatt **Einstellungen** klicken Sie auf *aktiviert* für **beschleunigte Netzwerke**, wie in der folgenden Abbildung dargestellt:

        ![Einstellungen](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] Die Option Netzwerk beschleunigt werden nur angezeigt, wenn Sie haben:
    >
    >- Angenommen in der Vorschau
    >- Betriebssystem, Standort und VM Größen gemäß Abschnitt begrenzt ausgewählt.

5. Erstellte VM [Networking beschleunigte Treiber](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84)downloaden, verbinden und VM und führen das Treiberinstallationsprogramm innerhalb des virtuellen Computers.
6. Windows rechten Maustaste, und klicken Sie auf **Geräte-Manager**. Überprüfen Sie die **Mellanox ConnectX 3 virtuellen Funktion Ethernet Adapter** unter **Netzwerk** die Option Wenn erweitert, wird angezeigt, wie in der folgenden Abbildung dargestellt:

    ![Geräte-Manager](./media/virtual-network-accelerated-networking-portal/image2.png)