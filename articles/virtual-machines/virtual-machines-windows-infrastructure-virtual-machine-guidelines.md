<properties
    pageTitle="Windows virtuelle Maschinen Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien für die Bereitstellung von virtuellen Windows-Maschinen in Azure"
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Virtuelle Computer Richtlinien

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dieser Artikel konzentriert sich auf Planung erforderlichen Schritte zum Erstellen und Verwalten von virtuellen Maschinen (VMs) innerhalb Ihrer Azure-Umgebung.

## <a name="implementation-guidelines-for-vms"></a>Implementierungsrichtlinien für VMs
Entscheidung:

- Wie viele VMs benötigen Sie für die verschiedenen Ebenen der Anwendung und Komponenten der Infrastruktur?
- Muss CPU- und Ressourcen jede VM, und welche erforderlichen Speicher?

Aufgaben:

- Definieren der Arbeitslast der Anwendung und die Ressourcen, die VMs benötigen.
- Ressource erfordert für jeden virtuellen Computer mit dem entsprechenden VM Größen- und ausrichten.
- Definieren Sie Ressourcengruppen für die verschiedenen Ebenen und Komponenten Ihrer Infrastruktur.
- Definieren Sie die VM-Namenskonvention.
- Die VMs mit Azure PowerShell Web Portal oder Ressourcen-Manager Vorlagen erstellen.

## <a name="virtual-machines"></a>Virtuelle Computer

Die Hauptkomponenten innerhalb Ihrer Azure-Umgebung gehört wahrscheinlich VMs. Ist dem ausgeführt Applications, Datenbanken, Authentifizierung usw.

Es ist wichtig zu verstehen, die [VM-Größen](virtual-machines-windows-sizes.md) korrekt Größe Ihrer Umgebung Performance und Kosten. Haben Ihre VMs nicht genügend CPU-Kerne oder Speicher, leidet die Performance der Anwendung unabhängig davon, wie gut es entworfen und entwickelt wird. Überprüfen der vorgeschlagenen Arbeitslasten für jede VM-Serie als Ausgangspunkt wie Sie entscheiden, welche Größe VM für jede Komponente in Ihrer Infrastruktur verwenden. Sie können [die Größe eines virtuellen Computers ändern](https://azure.microsoft.com/blog/resize-virtual-machines/) nach der Bereitstellung.

Speicher spielt eine wichtige Rolle in Leistung virtueller Computer. Können Standardspeicher, die reguläre rotierende Festplatten verwendet oder Premium-Speicher für hohe i/o-Arbeitslasten und Spitzenleistung, die SSD-Laufwerke verwendet. Als Größe VM vorhanden sind Kostenfaktoren Speichermedium auswählen. Lesen Sie die [Speicher-Infrastruktur Richtlinien Artikel](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) Informationen entsprechenden Speicher für eine optimale Leistung der VMs entwerfen.


## <a name="resource-groups"></a>Ressourcengruppen
Komponenten wie VMs werden Management und Wartung mit Hilfe von [Azure-Ressourcengruppen](../azure-resource-manager/resource-group-overview.md)logisch gruppiert. Mithilfe von Ressourcengruppen können Sie erstellen, verwalten und überwachen alle Ressourcen, die einer bestimmten Anwendung bilden. Außerdem implementieren Sie [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-what-is.md) für andere innerhalb Ihres Teams auf die Ressourcen gewähren, die sie benötigen. Dauern Sie, Planen Sie die Ressourcengruppen und Rollen zugewiesen werden. Es gibt verschiedene Ansätze, tatsächlich entwerfen und Implementieren von Ressourcengruppen werden die [Ressource Gruppen Richtlinien Artikel](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) um zu verstehen, wie Ihre VMs erstellen.


## <a name="templates"></a>Vorlagen 
Deklarative JSON-Dateien zu Ihrer virtuellen Computer definierte Vorlagen erstellen. Vorlagen erstellen in der Regel auch die benötigte Speicher, Netzwerke, Netzwerkschnittstellen, IP-Adressierung usw. mit VMs selbst. Sie verwenden Vorlagen zum Erstellen von konsistente, reproduzierbare Umgebung für die Entwicklung und Produktion leicht repliziert Zwecke testen und umgekehrt. Erfahren Sie mehr zum [Erstellen und Verwenden von Vorlagen](../azure-resource-manager/resource-group-overview.md#template-deployment) zu verstehen, deren Verwendung zum Erstellen und Bereitstellen der VMs.


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 