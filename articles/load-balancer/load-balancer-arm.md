<properties
   pageTitle="Azure Resource Manager Unterstützung für Lastenausgleich | Microsoft Azure "
   description="Verwendung von Powershell für Lastenausgleich mit Azure-Ressourcen-Manager. Verwenden von Vorlagen für Lastenausgleich"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Verwenden von Azure Ressourcen-Manager-Unterstützung mit Azure Lastenausgleich

Azure Ressourcen-Manager ist das bevorzugte Management-Framework für Dienste in Azure. Azure Lastenausgleich kann mit Azure-Ressourcen-Manager-basierte APIs und Tools verwaltet werden.

## <a name="concepts"></a>Konzepte

Mit Ressourcen-Manager Azure Lastenausgleich enthält die folgenden untergeordneten Ressourcen:

- Front-End-IP-Konfiguration – ein Lastenausgleich kann eine oder mehrere front-End IP-Adressen als virtuelle IP-Adressen (VIPs) enthalten. Diese IP-Adressen dienen als Eindringen für den Datenverkehr.

- Back-End-Adresspool – sind IP-Adressen mit der virtuellen Network Interface Card (NIC), Last verteilt werden.

- Load Balance Regeln – Rule-Eigenschaft ordnet einen bestimmten front-End-IP- und Port-Kombination auf einem Back-End-IP-Adressen und Port-Kombination. Ein einzelnes System zum Lastenausgleich können mehrere Regeln für den Lastenausgleich. Jede Regel ist eine Kombination aus einer Front-End-IP- und Port und Back-End-IP und-Port VMs zugeordnet.

- Prüfpunkte-Prüfpunkte aktivieren Sie den Zustand der VM-Instanzen des. Schlägt eine Integritätstest VM-Instanz aus der Rotation automatisch erfolgt.

- Eingehende NAT Regeln NAT eingehenden Datenverkehr über die Vorderseite definieren IP enden und an den Back-End-IP verteilt.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Azure Ressourcen-Manager können Sie Ihre Anwendung mit deklarativer Vorlage bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit deren abhängigen Dateien bereitstellen. Sie verwenden die gleiche Vorlage wiederholt die Anwendung in jeder Phase des Lebenszyklus der Anwendung bereitstellen.

Vorlagen können Definitionen für virtuelle Computer, virtuelle Netzwerke Verfügbarkeit legt, Netzwerkschnittstellen (NICs), Speicherkonten, Lastenausgleich, Netzwerk-Sicherheitsgruppen und öffentlichen IP-Adressen enthalten. Mit Vorlagen können Sie alles für eine komplexe Anwendung erstellen. Die Vorlagendatei kann Content-Management-System zur Versionskontrolle und Zusammenarbeit überprüft werden.

[Weitere Informationen zu Vorlagen](http://go.microsoft.com/fwlink/?LinkId=544798)

[Weitere Informationen zu Netzwerkressourcen](../virtual-network/resource-groups-networking.md)

Schnellstart-Vorlagen mithilfe von Azure Lastenausgleich in einem [GitHub Repository](https://github.com/Azure/azure-quickstart-templates) finden hosten mehrere Communityvorlagen generiert.

Beispiele für Vorlagen:

- [2 VMs Lastenausgleich und Lastenausgleich Regeln](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 VMs in einem VNET mit einer internen Lastenausgleich und Lastenausgleich](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 VMs in einem Lastenausgleich und Konfigurieren von NAT-Regeln auf die Pfd](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Einrichten von Azure Lastenausgleich mit PowerShell oder CLI

Erste Schritte mit Azure-Ressourcen-Manager-Cmdlets Befehlszeilentools und REST-APIs

- [Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) kann verwendet werden, um einen Lastenausgleich zu erstellen.
- [Ein Lastenausgleich mit Azure Ressource-Manager erstellen](load-balancer-get-started-ilb-arm-ps.md)
- [Verwenden von Azure CLI mit Azure Ressourcenmanagement](../xplat-cli-azure-resource-manager.md)
- [Lastenausgleich REST-APIs](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Nächste Schritte

Sie können auch [ein Lastenausgleich mit Internetzugriff erstellen beginnen](load-balancer-get-started-internet-arm-ps.md) und welche [Verteilung Modus](load-balancer-distribution-mode.md) für einen bestimmten Load Balancer Netzwerk Verhalten konfigurieren.

Informationen Sie zu [im Leerlauf TCP-Timeouteinstellungen für einen Lastenausgleich](load-balancer-tcp-idle-timeout.md). Dies ist wichtig, wenn Ihre Anwendung Verbindungen für Server hinter einem Lastenausgleich beizubehalten.
