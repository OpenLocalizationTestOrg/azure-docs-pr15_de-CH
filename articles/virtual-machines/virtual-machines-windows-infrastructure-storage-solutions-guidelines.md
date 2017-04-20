<properties
    pageTitle="Storage Solutions Richtlinien | Microsoft Azure"
    description="Erfahren Sie mehr über wichtigen Entwurf und Implementierung von Richtlinien zur Bereitstellung von Storage Solutions in Azure Infrastrukturdienste."
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

# <a name="storage-infrastructure-guidelines"></a>Speicherrichtlinien Infrastruktur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Dieser Artikel konzentriert sich auf Speicherbedarf und Vorüberlegungen für optimale Virtual Machine (VM) Leistung.


## <a name="implementation-guidelines-for-storage"></a>Implementierungsrichtlinien für Speicher

Entscheidung:

- Müssen Sie Ihre Arbeitslast mit Standard oder Premium-Speicher?
- Benötigen Sie Festplattenstriping Festplatten größer als 1023 GB erstellen?
- Benötigen Festplattenstriping optimale e/a-Leistung für Ihre Arbeit?
- Welche Speicher-Konten müssen Sie Ihre IT-Arbeitslast oder Infrastruktur?

Aufgaben:

- Überprüfen Sie i/o-Applikationen, die Sie bereitstellen und die entsprechende Anzahl und Art der Speicherkonten, an.
- Erstellen Sie die Benennungskonvention verwenden Speicherkonten. Sie können Azure PowerShell oder das Portal verwenden.


## <a name="storage"></a>Speicher

Azure-Speicher ist ein wichtiger Bestandteil bereitstellen und Verwalten von virtuellen Maschinen (VMs) und Applikationen. Azure-Speicher bietet Dienste zum Speichern von Daten, unstrukturierte Daten und Nachrichten, und ist auch Bestandteil der VMs unterstützt.

Gibt zwei Arten von Speicherkonten VMs unterstützt:

- Standardspeicher Konten können Blob-Speicher (zum Speichern von Azure VM Festplatten), Tabellenspeicher, Warteschlangenspeicher und Dateispeicher zugreifen.
- [Premium](../storage/storage-premium-storage.md) -Speicherkonten unterstützen leistungsfähige Diskette mit niedriger Latenz für e/a-intensive Arbeitslasten wie SQL Server in einem Cluster AlwaysOn. Premium-Speicher unterstützt derzeit Azure VM-Datenträger.

Azure erstellt VMs eine Betriebssystem-CD ein temporärer und NULL oder mehr optionale Datenträger. Die Betriebssystem-Datenträger und Datenträger sind Azure Seitenblobs temporäre Speicherplatz der Computer lebt lokal auf dem Knoten gespeichert. Achten Sie beim Entwerfen von Clientanwendungen temporärer Speicherplatz nur über nicht beständige Daten zwischen Hosts während eines Ereignisses Wartung die VM migriert werden kann. Alle Daten auf dem temporären Datenträger würden verloren.

Haltbarkeit und hohe Verfügbarkeit wird von der zugrunde liegenden Azure Storage Umgebung bereitgestellt, damit Ihre Daten nicht geplante Wartung oder Hardwareausfälle geschützt bleibt. Entwerfen Ihrer Azure Storage-Umgebung können Sie die Replikation von VM-Speicher:

- lokal innerhalb einer bestimmten Azure-Rechenzentrum
- für Azure Rechenzentren innerhalb eines bestimmten Bereichs
- für Azure Rechenzentren in verschiedenen Regionen

Lesen Sie [mehr über die Replikationsoptionen für hohe Verfügbarkeit](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Betriebssystem-Datenträger und Datenträger sind maximal 1023 Gigabyte (GB). Die maximale Größe eines Blob ist 1024 GB und müssen die Metadaten (Fußzeile) die VHD-Datei (ein GB ist 1024<sup>3</sup> Bytes) enthält. Speicherplätze in Windows Server 2012 können Sie dieses Limit überschreiten durch Zusammenlegung Datenträger um logische Volumes über 1023 GB VM angezeigt.

Einige Skalierbarkeitsgrenzen gibt es bei der Bereitstellung Ihrer Azure Storage - Näheres [Microsoft Azure-Abonnement und Service, Kontingente und Einschränkungen](azure-subscription-service-limits.md#storage-limits) . Siehe auch [Azure Skalierbarkeits- und Ziele](../storage/storage-scalability-targets.md).

Anwendungsspeicher können Sie unstrukturierte Daten wie Dokumente, Bilder, Backups, Konfigurationsdaten, Protokolle usw. speichern. BLOB-Speicher verwenden. Anstatt der Anwendung schreiben auf ein virtuelles Laufwerk für die VM zugeordnet kann die Anwendung direkt in Azure BLOB-Speicher schreiben. BLOB-Speicher bietet auch die Möglichkeit, [warme und kalte Speicherebenen](../storage/storage-blob-storage-tiers.md) je nach Verfügbarkeit und Kosten.


## <a name="striped-disks"></a>Festplattenarray
Nicht nur Festplatten in vielen Fällen mehr als 1023 GB erstellen optimiert mit Striping für Datenträger Leistung ermöglicht mehrere Blobs Speicher für einen einzelnen Datenträger zurück. Striping wird die erforderlich zum Schreiben und Lesen von Daten aus einer einzigen logischen e/a Parallel.

Azure legt Grenzwerte für die Anzahl der Datenträger und je VM verfügbare Bandbreite. Einzelheiten finden Sie [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

Wenn Sie Disk Striping für Azure Datenträger verwenden, beachten Sie Folgendes:

- Datenträger sollte immer die maximale Größe (1023 GB).
- Legen Sie die maximale Datenträger für die VM-Größe zulässig.
- Verwenden Sie Storage Spaces.
- Vermeiden Sie Zwischenspeicherungsoptionen Azure Datenträger (Cachingrichtlinie = None).

Weitere Informationen finden Sie unter [Storage Spaces - für Leistung](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).


## <a name="multiple-storage-accounts"></a>Mehrere Storage-Konten

Beim Entwerfen der Azure Storage-Umgebung können mehrere Storage-Konten als Anzahl der VMs erhöht bereitstellen. Dies hilft, die e/a auf zugrunde liegende Azure-Speicherinfrastruktur für optimale Leistung für Ihre VMs und verteilen. Entwerfen der Anwendung, die Sie bereitstellen können Sie die e/a-Anforderungen an jede VM und Ausgleich die VMs in Azure-Speicherkonten. Versuchen Sie zu gruppieren alle die hohe e/a nur ein oder zwei Speicherkonten VMs in will.

Weitere Informationen zu e/a-Funktionen der Azure-Speicher Optionen und sollten Sie Höchstwerte, finden Sie [Azure Skalierbarkeits- und Ziele](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 