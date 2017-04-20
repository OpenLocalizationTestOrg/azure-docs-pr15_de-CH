<properties
    pageTitle="Übersicht über Windows-VMs | Microsoft Azure"
    description="Erstellen und Verwalten von Windows-VMs in Azure erfahren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Übersicht über virtuelle Windows-Maschinen in Azure

Azure virtuellen Maschinen (VM) ist einer von mehreren Typen [auf Abruf, skalierbare Computerressourcen](../app-service-web/choose-web-site-cloud-service-vm.md) , die Azure bietet. Normalerweise wählen Sie eine VM, wenn mehr Kontrolle über die Umgebung als die anderen Optionen benötigen. Dieser Artikel enthält Informationen über Sie sollten vor dem Erstellen einer VM wie erstellen und wie Sie verwalten.

Azure-VM gibt Ihnen die Flexibilität der Virtualisierung ohne zu kaufen die physische Hardware, die sie ausgeführt wird. Allerdings müssen Sie noch die VM von Aufgaben konfigurieren und Patches installieren, die auf ihm ausgeführte Software verwalten.

Azure virtuelle Computer kann auf unterschiedliche Weise verwendet werden. Beispiele sind:

- **Entwicklung und Test** – Azure VMs schnell und einfach mit spezifischen Konfigurationen erstellen Sie eine erforderliche Code und Testen einer Anwendung.
- **Anwendung in der Cloud** – da Nachfrage für Ihre Anwendung schwanken kann, es sinnvoll wirtschaftlichen auf einer VM in Azure ausgeführt. Sie Zahlen für zusätzliche VMs benötigen und fahren sie Sie herunter, wenn Sie nicht.
- **Erweiterte Datacenter** -virtuelle Computer in Azure virtual Network können problemlos Netzwerk Ihrer Organisation verbunden.

Die Anzahl der VMs, die Ihre Anwendung skalierbar und, was Ihre Bedürfnisse erfüllen.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Was muss ich denken vor dem Erstellen einer VM?

Gibt immer eine Vielzahl von [Vorüberlegungen](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) beim Erstellen einer Anwendung Infrastruktur in Azure. Diese Aspekte für eine VM sind Denken Sie vor:

- Die Namen der Ressourcen der Anwendung
- Der Speicherort die Ressourcen
- Die Größe der VM
- Die maximale Anzahl virtueller Computer erstellt werden können
- Das Betriebssystem die VM
- Die Konfiguration des virtuellen Computers nach dem Start
- Die zugehörigen Ressourcen die VM muss

### <a name="naming"></a>Benennen

Eine virtuelle Maschine verfügt über einen [Namen](virtual-machines-windows-infrastructure-naming-guidelines.md) zugewiesen und hat einen Computernamen als Teil des Betriebssystems. Der Name eines virtuellen Computers kann bis zu 15 Zeichen.

Verwenden Sie Azure Betriebssystem erstellen, stimmen der Computername und der Name des virtuellen Computers. Wenn Sie [Ihr eigenes Bild hochladen und verwenden](virtual-machines-windows-upload-image.md) , die eine zuvor konfigurierte Betriebssysteme und einen virtuellen Computer erstellen, die Namen unterscheiden können. Wir empfehlen, beim Hochladen eigener Bilddatei die Computername des Betriebssystems und der gleiche name, den virtuellen Computer.

### <a name="locations"></a>Standorte

Alle Ressourcen in Azure sind mehrere [Regionen](https://azure.microsoft.com/regions/) verteilt. Normalerweise wird die Region beim Erstellen einer VM **Speicherort** aufgerufen. Für eine VM gibt den Speicherort an, wo die virtuellen Festplatten gespeichert sind.

Dieser Tabelle können Sie eine Liste der verfügbaren Arten.

| -Methode | Beschreibung |
| ------ | ----------- |
| Azure-portal | Wählen Sie einen Speicherort aus der Liste, wenn Sie einen virtuellen Computer erstellen. |
| Azure PowerShell | Verwenden Sie den Befehl [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| REST-API | Verwenden Sie die [Standorte auflisten](https://msdn.microsoft.com/library/dn790540.aspx) Operation. |

### <a name="vm-size"></a>Virtueller Speicher

Die [Größe](virtual-machines-windows-sizes.md) des virtuellen Computers, mit dem Sie bestimmt die Arbeitslast, die Sie ausführen möchten. Sie wählen Größe ermittelt wie Leistung, Arbeitsspeicher und Speicher. Azure bietet eine Vielzahl von Größen unterstützen viele verwendet.

Azure Gebühren ein [Stundenpreis](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) basierend auf Größe und das Betriebssystem des virtuellen Computers. Teilweise Stunden Azure Gebühren für Minuten verwendet. Speicher ist preislich und separat in Rechnung gestellt.

### <a name="vm-limits"></a>VM-Grenzwerte

Ihr Abonnement ist Standard- [Kontingentgrenzen](../azure-subscription-service-limits.md) , die die Bereitstellung vieler virtueller Computer für das Projekt auswirken. Die aktuelle Begrenzung auf pro beträgt 20 VMs pro Region. Grenzwerte können durch Einreichung von Support-Ticket Anfordern einer Erhöhung ausgelöst werden.

### <a name="operating-system-disks-and-images"></a>Betriebssystem-Datenträger und Bilder

Virtuelle Maschinen verwenden [virtuelle Festplatten (VHDs)](virtual-machines-windows-about-disks-vhds.md) zum Speichern ihrer Betriebssystem (OS) und. Festplatten werden auch für die Bilder verwendet, denen Sie auswählen können, um ein Betriebssystem zu installieren. 

Azure bietet viele [Marketplace Bilder](https://azure.microsoft.com/marketplace/virtual-machines/) mit verschiedenen Versionen Windows Server-Betriebssysteme. Marketplace-Bilder identifizierten Bild Publisher, Angebot, Sku und Version (in der Regel ist Version als aktuelle angegeben). 

Diese Tabelle zeigt einige Methoden finden Sie die Informationen für ein Bild.

| -Methode | Beschreibung |
| ------ | ----------- |
| Azure-portal | Die Werte werden automatisch für Sie angegeben, wenn Sie ein Bild auswählen. |
| Azure PowerShell | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -Speicherort "Speicherort"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -Speicherort "Location"-Publisher "PublisherName"<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -Speicherort "Location"-"PublisherName" Publisher-bieten "OfferName" |
| REST-APIs | [Liste Bild Herausgeber](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Liste Bild](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Liste Bild skus](https://msdn.microsoft.com/library/mt743701.aspx) |

Sie können [Ihr eigenes Bild hochladen und verwenden](virtual-machines-windows-upload-image.md) und dabei den Verlegernamen Angebot und Sku nicht verwendet.

### <a name="extensions"></a>Extensions

VM- [Erweiterungen](virtual-machines-windows-extensions-features.md) geben die VM-Zusatzfunktionen Post Bereitstellungskonfiguration und automatisierte Aufgaben.

Diese gemeinsamen Aufgaben möglich Erweiterungen:

- **Benutzerdefinierte Skripts ausführen** – [Benutzerdefinierte Skript-Erweiterung](virtual-machines-windows-extensions-customscript.md) können Sie die Arbeitslasten auf dem virtuellen Computer konfigurieren, indem Sie das Skript ausführen, wenn die VM bereitgestellt wird.
- **Bereitstellen und Verwalten von Konfigurationen** - [PowerShell gewünschten Zustand Konfiguration (DSC) Erweiterung](virtual-machines-windows-extensions-dsc-overview.md) hilft Ihnen, DSC auf einer VM-Konfigurationen und Umgebung verwalten.
- **Sammeln von Diagnosedaten** – [Azure Diagnostics Erweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) können Sie die VM Diagnosedaten sammeln, die mit der Überwachung der Anwendung konfigurieren.

### <a name="related-resources"></a>Verwandte Ressourcen

Die Ressourcen in dieser Tabelle von der VM verwendet und müssen vorhanden sein oder erstellt werden, wenn die VM erstellt wird.

| Ressource | Erforderlich | Beschreibung |
| -------- | -------- | ----------- |
| [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) | Ja | Die VM muss in einer Ressourcengruppe enthalten sein. |
| [Konto](../storage/storage-create-storage-account.md) | Ja | Die VM benötigt das Speicherkonto den virtuellen Festplatten speichern. |
| [Virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) | Ja | Die VM muss ein virtuelles Netzwerk angehören. |
| [Öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Nein | Die VM haben eine öffentliche IP-Adresse zugewiesen, Remote zugreifen. |
| [Netzwerkschnittstelle](../virtual-network/virtual-network-network-interface-overview.md) | Ja | Die VM benötigt die Schnittstelle im Netzwerk kommunizieren. |
| [Datenträger](virtual-machines-windows-attach-disk-portal.md) | Nein | Die VM kann Datenträger erweitern Speicherfunktionen enthalten. |

## <a name="how-do-i-create-my-first-vm"></a>Erstellen meiner ersten VM

Sie haben mehrere Optionen zum Erstellen der virtuellen Computer. Die Auswahl hängt von der Umgebung sind in. 

Diese Tabelle enthält Informationen zu begann VM.

| -Methode | Artikel |
| ------ | ------- |
| Azure-portal | [Erstellen einer virtuellen Maschine unter Windows über das portal](virtual-machines-windows-hero-tutorial.md) |
| Vorlagen | [Erstellen einer virtuellen Windows-Maschine mit einer Ressourcen-Manager-Vorlage](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Erstellen einer Windows-VM mit PowerShell](virtual-machines-windows-ps-create.md) |
| Client-SDKs | [Ressourcen Sie Azure mit C#](virtual-machines-windows-csharp.md) |
| REST-APIs | [Erstellen oder Aktualisieren einer VM](https://msdn.microsoft.com/library/mt163591.aspx) |

Sie hoffen, dass es nie dazu kommt jedoch manchmal etwas schief geht. Wenn dies passiert, betrachten Sie die Informationen im [Ressourcenmanager beheben Bereitstellungsprobleme mit einem Windows-Computer in Azure erstellen](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Wie verwalte ich die VM, die ich erstellt?

VMs können ein browserbasiertes Portal mit Befehlszeilentools mit Unterstützung für Skripting oder direkt über APIs verwaltet werden. Normale Verwaltungsaufgaben ausführen können abrufen Informationen einer VM Protokollierung auf einer VM verwalten und Sicherungskopien.

### <a name="get-information-about-a-vm"></a>Informationen Sie über einen virtuellen Computer

Diese Tabelle zeigt einige der Methoden, die Informationen über einen virtuellen Computer zu erhalten.

| -Methode | Beschreibung |
| ------ | ----------- |
| Azure-portal | Klicken Sie auf **virtuellen Maschinen** im Hub und wählen Sie den virtuellen Computer aus. Auf dem Blatt für die VM haben Sie Zugriff auf Informationen, Festlegen von Werten und Metriken überwachen. |
| Azure PowerShell | Informationen über PowerShell VMs verwalten finden Sie unter [Verwalten von Azure Virtual Machines Ressourcenmanager und PowerShell verwenden](virtual-machines-windows-ps-manage.md). |
| REST-API | Verwenden Sie die Operation [VM erhalten Informationen](https://msdn.microsoft.com/library/mt163682.aspx) um über einen virtuellen Computer zu informieren. |
| Client-SDKs | Informationen über C# VMs verwalten finden Sie unter [Verwalten von Azure Virtual Machines verwenden Azure Resource Manager und C#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Melden Sie die VM

Verwenden Sie die Schaltfläche Verbinden in Azure-Portal an [Desktop (RDP)](virtual-machines-windows-connect-logon.md). Manchmal kann schief gehen beim Versuch eine Verbindung verwenden. Wenn dies passiert, checken Sie die Hilfeinformationen in [Remotedesktopverbindungen mit einer Azure virtuellen Maschine unter Windows zu beheben](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Verwalten der Verfügbarkeit

Es ist wichtig zu verstehen, wie [hohe Verfügbarkeit](virtual-machines-windows-manage-availability.md) für Ihre Anwendung. Diese Konfiguration umfasst das Erstellen mehrere virtuelle Computer, um sicherzustellen, dass mindestens ausgeführt wird.

Reihenfolge für die Bereitstellung unserer 99,95 VM Service Level Agreement qualifizieren müssen Sie zwei oder mehr VMs mit der Arbeitslast innerhalb einer [Verfügbarkeit](virtual-machines-windows-infrastructure-availability-sets-guidelines.md)bereitstellen. Diese Konfiguration gewährleistet die VMs mehrere Fehlerdomänen verteilt und auf Servern mit unterschiedlichen Wartungsfenster bereitgestellt werden. Die vollständige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) erläutert die garantierte Verfügbarkeit von Azure als Ganzes.
 
### <a name="back-up-the-vm"></a>Sichern Sie die VM
 
Ein [Depot Recovery Services](../backup/backup-introduction-to-azure-backup.md) zum Schutz von Daten und Ressourcen in Azure Backup und Azure Site Recovery. Ein Depot Recovery Services [Bereitstellen](../backup/backup-azure-vms-automation.md)und Backups für Ressourcen-Manager bereitgestellte VMs mit PowerShell verwalten können. 

## <a name="next-steps"></a>Nächste Schritte

- Wenn Ihre Absicht Linux VMs ist, betrachten Sie [Azure und Linux](virtual-machines-linux-azure-overview.md).
- Erfahren Sie mehr über die Richtlinien in der Infrastruktur im [Beispiel Azure Infrastruktur Exemplarische Vorgehensweise](virtual-machines-windows-infrastructure-example.md).
- Stellen Sie sicher, führen Sie die [Best Practices für eine VM Windows Azure ausgeführt](virtual-machines-windows-guidance-compute-single-vm.md).


