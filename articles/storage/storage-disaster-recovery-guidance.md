<properties
    pageTitle="Vorgehensweise bei einem Ausfall Azure Storage | Microsoft Azure"
    description="Vorgehensweise bei einem Ausfall der Azure-Speicher"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Wenn ein Ausfall Azure-Speicher

Bei Microsoft arbeiten wir hart um sicherzustellen, dass unsere Dienste immer verfügbar sind. Manchmal zwingt über unsere Steuerelement Auswirkung auf Arten, die in einem oder mehreren Bereichen ungeplanten Ausfällen führen. Als Hilfe bei der Behandlung dieser seltenen bieten wir die folgende allgemeine Richtlinien für Azure Storage Services.

## <a name="how-to-prepare"></a>Vorbereitung 

Es ist wichtig für jeden Kunden eigene Wiederherstellungsplan vorbereiten. Aufwand ein Ausfall Speicher normalerweise Wiederherstellung umfasst Betriebspersonal und automatisierte Verfahren zur Anwendung in einen funktionsfähigen Zustand zu reaktivieren. Finden Sie unter erstellen Ihre eigenen Disaster Recovery-Plan Azure-Dokumentation:

-   [Disaster Recovery und hohe Verfügbarkeit für Azure applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Technische Anleitung Azure Stabilität](../resiliency/resiliency-technical-guidance.md)

-   [Azure Site Recovery service](https://azure.microsoft.com/services/site-recovery/)

-   [Azure Speicherreplikation](storage-redundancy.md)

-   [Azure backupservice](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Ermitteln 

Das empfohlene Verfahren zum Ermitteln der Azure ist der [Azure Service Health Dashboard](https://azure.microsoft.com/status/)abonnieren.

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Wenn ein Speicher-Ausfall

Wenn mindestens Storage Services auf einem oder mehreren Bereichen verfügbar sind, stehen zwei Optionen zur Auswahl. Auf Wunsch sofortigen Zugriff auf Ihre Daten sollten Sie Option 2.

### <a name="option-1-wait-for-recovery"></a>Option 1: Warten auf Wiederherstellung

In diesem Fall ist keine Aktion Ihrerseits erforderlich. Wir arbeiten sorgfältig Azure Betrieb wiederherstellen. Sie können den Dienststatus [Azure Service Health Dashboard](https://azure.microsoft.com/status/)überwachen.

### <a name="option-2-copy-data-from-secondary"></a>Option 2: Kopieren von Daten vom sekundären Standort

Wählt [Lesezugriff Geo redundanten Speicher (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (empfohlen) für Speicherkonten haben gelesen Sie Zugriff auf die Daten aus dem sekundären. Tools können wie [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)und [Azure Datentransfer Bibliothek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) Daten aus dem zweiten Bereich in einen anderen Speicher-Konto in einem unimpacted Bereich, und zeigen Ihre Anträge auf diesem Storage-Konto sowohl lesen und Schreiben von Verfügbarkeit.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Was erwartet bei einem Speicher Failover

Wenn [Geo redundanten Speicher (GRS)](storage-redundancy.md#geo-redundant-storage) oder [Lesezugriff Geo redundanten Speicher (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (empfohlen) ausgewählt haben, erhalten Azure-Speicher der dauerhaften zwei Bereiche (Primär und sekundär) bleibt. In beiden Regionen verwaltet Azure Storage ständig mehrere Replikate Ihrer Daten.

Wenn ein regionaler Ausfall der primären Region betrifft, zunächst versuchen wir Wiederherstellung des Dienstes in diesem Bereich. Abhängig von der Art der Katastrophe und seine folgen in einigen seltenen Fällen können wir nicht die primäre Region wiederherstellen können. Nun führen wir Geo-Failover. Cross-Region Datenreplikation ist ein asynchroner Prozess mit einer Verzögerung kann so ist es möglich, dass Änderungen, die an die sekundäre Region noch nicht repliziert wurden. Sie können ["letzten Synchronisierungszeit" Ihres Speicherkontos](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) um Details auf den Replikationsstatus Abfragen.

Ein paar Punkte Speicher Geo-Failover-Erfahrung:

-   Storage Geo-Failover nur von Azure Storage-Team ausgelöst – kein Eingriff vom Kunden erforderlich ist.

-   Ihre vorhandenen Speicher-Endpunkte für Blobs, Tabellen, Warteschlangen und Dateien bleiben nach dem Failover; der DNS-Eintrag müssen aktualisiert werden, um aus dem primären in den sekundären Bereich wechseln.

-   Vor und während des Geo-Failovers Sie müssen Schreibzugriff auf das Speicherkonto aufgrund der Katastrophe, aber Sie können weiterhin aus der sekundären lesen, wenn das Speicherkonto als RA-GRS konfiguriert wurde.

-   Geo-Failover abgeschlossen wurde, die DNS-Änderungen weitergegeben werden der Lese- und Schreibzugriff auf das Speicherkonto fortgesetzt. Sie können ["Geo-Failover zuletzt" Ihres Speicherkontos](https://msdn.microsoft.com/library/azure/ee460802.aspx) Weitere Informationen Abfragen.

-   Nach dem Failover das Speicherkonto voll funktionsfähig, aber "degradiert" Status wie gehostet wird eine eigenständige Region keine Geo-Replikation möglich. Um dieses Risiko zu verringern, wir die ursprüngliche primäre Region wiederherstellen und dann Geo-Failback zum Wiederherstellen des ursprünglichen Zustands. Wenn die ursprüngliche primäre Region nicht verfügbar ist, wird anderen sekundären Region zugeordnet werden.
Weitere Informationen zur Infrastruktur Azure Storage Geo-Replikation finden Sie Artikel Storage-Teamblog [Redundanz](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)und RA-GRS.

##<a name="best-practices-for-protecting-your-data"></a>Best Practices zum Schutz Ihrer Daten

Es gibt einige empfohlenen Vorgehensweisen, Ihre Daten regelmäßig sichern.

-   VM-Laufwerke – verwenden der [Azure-Sicherungsdienst](https://azure.microsoft.com/services/backup/) von Azure VMs verwendete VM Datenträger sichern.

-   Block-blobs – erstellen einen [snapshot](https://msdn.microsoft.com/library/azure/hh488361.aspx) aller Block-Blob oder Blobs in einem anderen Ordner in einen anderen Bereich mit [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)oder [Azure Datentransfer Bibliothek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)Konto kopieren.

-   Tabellen-verwenden [AzCopy](storage-use-azcopy.md) zum Exportieren der Daten in einem anderen Speicher-Konto in einer anderen Region.

-   Dateien verwenden Sie [AzCopy](storage-use-azcopy.md) oder [Azure PowerShell](storage-powershell-guide-full.md) zum Kopieren der Dateien auf einem anderen Speicher-Konto in einer anderen Region.
