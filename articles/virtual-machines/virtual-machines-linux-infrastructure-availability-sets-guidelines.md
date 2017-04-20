<properties
    pageTitle="Verfügbarkeit legen Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Richtlinien für Entwurf und Implementierung zur Bereitstellung von Verfügbarkeit legt in Azure Infrastrukturdienste."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Verfügbarkeit setzt Richtlinien

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel konzentriert sich auf die erforderlichen Planungsschritte bei Verfügbarkeit Sicherstellen der Anwendung bleibt bei geplanten oder ungeplanten Ereignissen verfügbar.

## <a name="implementation-guidelines-for-availability-sets"></a>Richtlinien für die Implementierung bei Verfügbarkeit

Entscheidung:

- Wie viele verfügbarkeitssätze müssen Sie für die verschiedenen Rollen und Ebenen in Ihrer Anwendungsinfrastruktur?

Aufgaben:

- Definieren Sie die Anzahl der VMs in jede Anwendungsebene benötigten
- Bestimmen Sie ggf. die Anzahl der Fehler oder Aktualisieren von Domänen für die Anwendung verwendet wird.
- Definieren der erforderlichen Verfügbarkeit mit der Namenskonvention wie VMs befinden werden. Eine VM kann nur in einer verfügbarkeitsgruppe befinden. 

## <a name="availability-sets"></a>Verfügbarkeit-sets

In Azure können virtuelle Maschinen (VMs) in eine logische Gruppierung einer Verfügbarkeit genannt liegen. Beim Erstellen von VMs innerhalb einer Verfügbarkeit verteilt die Azure-Plattform die Platzierung die VMs auf der zugrunde liegenden Infrastruktur. Sollte ein Ereignis Wartungsarbeiten der Azure-Plattform oder eine zugrunde liegende Hardware / Infrastruktur Fehler, die Verwendung von Verfügbarkeit stellt sicher, dass mindestens ein VM weiterhin ausgeführt wird.

Als bewährte Methode sollten Anträge nicht auf einer einzelnen VM befinden. Schutz vor geplanten und ungeplanten Ereignissen innerhalb der Azure-Plattform gewinnen ein Satz Verfügbarkeit, der eine einzige VM enthält. Der [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) erfordert mindestens zwei VMs innerhalb einer Verfügbarkeit können die Verteilung der VMs in der zugrunde liegenden Infrastruktur.

Die zugrunde liegende Infrastruktur in Azure gliedert sich in und Fehlertoleranz Domänen zu aktualisieren. Diese Domänen werden definiert, welche Hosts eine gemeinsame Aktualisierungszyklus oder Freigabe ähnliche Infrastruktur wie Stromversorgung und Netzwerk freigeben. Azure verteilt automatisch Ihre VMs innerhalb einer Verfügbarkeit Verfügbarkeit und Fehlertoleranz in Domänen festgelegt. Je nach Größe der Anwendung und die Anzahl der VMs innerhalb einer Verfügbarkeit passen Sie die Anzahl der Domänen, die Sie verwenden möchten. Erfahren Sie mehr zum [Verwalten von Verfügbarkeit und Verwendung von Update und Fehlertoleranz Domänen](virtual-machines-linux-manage-availability.md).

Beim Entwerfen der Anwendungsinfrastruktur für Ihre sollten Sie auch, Anwendungsebenen verwenden. Gruppe-VMs, die demselben Zweck in Verfügbarkeit erfüllen, festlegt, wie eine für Ihre Front-End-VMs mit Nginx oder Apache festgelegt. Erstellen Sie eine separate Verfügbarkeit für Ihre Back-End-VMs mit MongoDB oder MySQL festgelegt. Das Ziel ist, dass jede Komponente der Anwendung durch einen Satz Verfügbarkeit geschützt und mindestens Instanz immer ausgeführt.

Lastenausgleich genutzt vor jeder Anwendungsebene zusammen mit einer Verfügbarkeit und sicherzustellen, dass Datenverkehr immer auf eine ausgeführte Instanz weitergeleitet werden kann. Ohne ein Lastenausgleich Ihrer virtuellen Computer können weiterhin ausgeführt in geplante und nicht geplante Wartungsereignisse, aber Ihre Endbenutzer möglicherweise Problembehebung ist die primäre VM nicht verfügbar.


## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 