<properties
    pageTitle="Bereitstellen eine Anwendung auf virtuellen Skalierung | Microsoft Azure"
    description="Bereitstellen einer Anwendung auf virtuellen skalieren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Bereitstellen einer Anwendung auf virtuellen skalieren

Eine Anwendung auf einem VM Skalierung ist typischerweise auf drei Arten:

- Neue Software installieren auf einem Plattform-Image zum Zeitpunkt der Bereitstellung. Plattform-Images in diesem Kontext ist ein Betriebssystemabbild Azure Marketplace Ubuntu 16.04, Windows Server 2012 R2 usw..

Sie können neuen Software auf Plattform-Images der [VM-Erweiterung](../virtual-machines/virtual-machines-windows-extensions-features.md). VM-Erweiterung ist Software, die bei der Bereitstellung einer VM. Sie können Code wie zum Zeitpunkt der Bereitstellung mit der Erweiterung benutzerdefiniertes Skript ausführen. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) ist ein Beispiel für Azure Ressourcenmanager Vorlage mit zwei VM: eine Linux benutzerdefinierte Skript Erweiterung Apache und PHP installieren und eine Diagnose Erweiterung von Azure Autoscale verwendeten Daten ausgeben.

Ein Vorteil dieses Ansatzes ist eine Trennung zwischen dem Anwendungscode und dem Betriebssystem haben und die Anwendung separat verwalten können. Natürlich bedeutet auch mehr bewegliche Teile und VM Bereitstellungszeit sind möglicherweise länger ist wesentlich für das Skript herunterladen und konfigurieren.

**Übergeben Sie vertrauliche Informationen in Ihrem Skript Extension benutzerdefinierte Befehl (z. B. ein Kennwort), müssen Sie angeben der `commandToExecute` in der `protectedSettings` Attribut benutzerdefiniertes Skript der anstelle der `settings` Attribut.**

- Erstellen Sie eine benutzerdefinierte VM Image, das Betriebssystem und die Anwendung in einer VHD enthält. Hier besteht aus skalieren einer Gruppe von VMs kopiert aus einem Bild erstellt haben, die Sie verwalten. Dieser Ansatz erfordert keine zusätzliche Konfiguration zum Zeitpunkt der VM-Bereitstellung. In der `2016-03-30` Version der VM Maßstab legt und frühere Versionen sind Betriebssystemlaufwerke für die virtuellen Computer in den Maßstab ein einzelnes Speicherkonto auf. So können Sie haben maximal 40 VMs in einer Skala, im Gegensatz zu 100 VM pro Skalierung festlegen mit Plattform-Images. Weitere Informationen finden Sie in der [Skalierung festgelegt Entwurfsübersicht](./virtual-machine-scale-sets-design-overview.md) .

- Bereitstellen Sie einer Plattform oder ein benutzerdefiniertes Bild im Grunde ein containerhost, und installieren Sie die Anwendung als Container, mit dem ein Verwaltungstool Orchestrator oder Konfiguration verwalten. Das Schöne an diesem Ansatz ist, die Cloud-Infrastruktur von der Anwendungsschicht abstrahiert haben und separat verwalten können.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Was geschieht, wenn ein VM Skalierung festlegen, skaliert?

Ein Maßstab durch Erhöhen der Kapazität ein oder mehrere VMs hinzufügen – manuell oder durch Skalieren – wird die Anwendung automatisch installiert. Für Beispiel Skalierung festlegen Extensions definiert ist, führen sie auf einer neuen VM jedes Mal erstellt wird. Wenn Skalierung auf ein benutzerdefiniertes Abbild basiert, ist jede neue VM eine benutzerdefinierte Quellbild. Sollten Skalierung festlegen kann VMs sind Container Hosts und möglicherweise Startcode Container in benutzerdefinierte Skript Erweiterung geladen oder eine Erweiterung einen Agent, der mit einem Cluster Orchestrator (wie Azure Container) registriert.

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Wie verwalten Sie Anwendungsupdates VM Maßstab legt?

Anwendungsupdates VM Dezimalstellen Mengen folgen aus den drei vorhergehenden Anwendung Bereitstellungsmethoden drei Ansätze:

* Mit VM-Erweiterungen aktualisiert. Jede VM-Erweiterungen für VM Skalierung festlegen erfolgt jedes Mal ein neuer virtuellen Computer bereitgestellt wird, eine vorhandene VM definiert sind, verhindert oder Erweiterung VM aktualisiert. Möchten Sie die Anwendung aktualisieren, direkt aktualisieren einer Anwendung durch Extensions ist ein geeigneter Ansatz – einfach die Definition aktualisieren. Eine einfache Möglichkeit dazu ist die neue Software auf FileUris.

* Benutzerdefiniertes Bild unveränderlich Ansatz. Wenn Sie die Anwendung (oder app-Komponenten) in einer VM-Image Backen können Sie auf eine verlässliche Pipeline zum Automatisieren von Builds, Tests und Bereitstellung Bilder konzentrieren. Sie können Ihre Architektur zu schnellen Austausch bereitgestellte Skalierungsgruppe Produktionsphase entwerfen. Ein gutes Beispiel hierfür ist der [Azure Spinnaker Treiber](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Packer und Terraform auch Ressourcenmanager Azure unterstützen, auch Bilder "als Code" definieren und in Azure erstellen, verwenden die virtuelle Festplatte in der Skala. Jedoch würde dies so Marketplace Bilder problematisch, Extensions-benutzerdefinierte Skripts wichtiger geworden, da Sie Bits von Marketplace direkt bearbeiten nicht.

* Container zu aktualisieren. Zusammenfassung der Anwendungslebenszyklus-Verwaltung über die Cloud-Infrastruktur auf beispielsweise kapselnden Applikationen und app-Komponenten in Containern und Verwalten dieser Container Container berufliche und Konfigurations-Manager wie Chef-Puppe.

Skalierung festlegen VMs werden stabile Unterlage für die Container und nur gelegentlich Sicherheits- und Betriebssystem-bezogene Updates erforderlich. Wie bereits erwähnt, ist Azure Container Service ein gutes Beispiel dieses Ansatzes und einen Dienst um es herum.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Wie Rollen Sie ein Betriebssystem-Update Update domänenübergreifende?

Angenommen Sie, Sie möchten das Betriebssystemabbild aktualisieren und Ausführung virtueller Computer festzulegen. Eine Möglichkeit hierzu ist die VM eine VM gleichzeitig Bilder. So führen Sie mit PowerShell oder Azure CLI. Es gibt separate Befehle zum Aktualisieren des Modells VM festzulegen (wie die Konfiguration definiert ist) und "manuelle Aktualisierung" Aufrufe auf einzelnen VMs.

[Hier](https://github.com/gbowerman/vmsstools) ist ein Beispiel für Python-Skript, das den Prozess der Aktualisierung einer Domäne VM festlegen ein Update gleichzeitig automatisiert. (Einschränkung: ist eine Machbarkeitsstudie als einer verstärkten Produktion Lösung – einige Fehler überprüfen usw. hinzufügen möchten.).
