<properties
    pageTitle="Einführung in Azure DPM Sicherung | Microsoft Azure"
    description="Einführung in die DPM-Server mithilfe des Diensts für Azure Backup Sichern"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="System Center Data Protection Manager, DPM, Dpm-Sicherung"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Vorbereiten der Arbeitslasten in Azure mit DPM sichern

> [AZURE.SELECTOR]
- [Azure Backup-Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure Backup-Server (klassisch)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassisch)](backup-azure-dpm-introduction-classic.md)


Dieser Artikel enthält eine Einführung in Microsoft Azure Backup zum Schutz von System Center Data Protection Manager (DPM)-Server und Arbeitslasten. Lesen, werden Sie verstehen:

- Funktionsweise von Azure DPM-Server-Sicherung
- Die erforderlichen Komponenten über einen reibungslosen backup Erfahrung
- Normalerweise Fehler und wie Sie damit umgehen
- Unterstützte Szenarios

System Center DPM unterstützt Datei-und Anwendungsdaten. Daten von DPM auf Band auf Festplatte gespeichert oder in Azure mit Microsoft Azure Backup gesichert. DPM interagiert mit Azure Backup wie folgt:

- **DPM als einen physischen Server oder lokalen virtuellen Computer bereitgestellt** , wenn DPM bereitgestellt, als physischen Server oder als lokale Hyper-V virtuelle Maschine Daten, Azure Backup Tresor neben Disk und Tape sichern backup.
- **DPM als Azure VM** – von System Center 2012 R2 mit Update 3 DPM als Azure virtuellen Computer bereitgestellt werden. Wenn DPM wie Azure virtuelle Computer bereitgestellt wird, Daten in Azure Datenträger sichern können, DPM Azure Virtual Machine an oder Verschiebung der Speicherung von Daten bis zu einem Depot Azure Backup sichern.

## <a name="why-backup-your-dpm-servers"></a>Warum backup Server?

Die Vorteile der Verwendung von Azure Backup zum Sichern von DPM-Servern gehören:

- Lokalen DPM-Bereitstellung können Sie als Alternative zur langfristigen Einsatz auf Band Azure Backup.
- Für DPM-Installationen in Azure ermöglicht Azure Backup Speicher aus dem Azure offload ermöglicht die Skalierung durch ältere Daten in Azure Backup und neue Daten auf der Festplatte gespeichert.

## <a name="how-does-dpm-server-backup-work"></a>Funktionsweise von DPM-Server-Sicherung
Um einen virtuellen Computer zu sichern, wird zuerst ein Point-in-Time-Snapshot der Daten benötigt. Der Azure-Sicherungsdienst Sicherungsauftrags zum geplanten Zeitpunkt initiiert und löst die Sicherung Erweiterung Snapshots. Backup-Erweiterung ist der VSS Gast zu erreichen und ruft BLOB-Snapshot-API von Azure Storage-Diensts Konsistenz erreicht. Dies ist eine konsistente Momentaufnahme der Datenträger des virtuellen Computers ohne Herunterfahren erhalten.

Nachdem die Momentaufnahme werden die Daten von der Azure-Sicherungsdienst backup Depot übertragen. Der Dienst sorgt identifizieren und aus der letzten Sicherung Backups Speicher- und Netzwerk effizienter geänderte Blöcke. Wenn die Datenübertragung abgeschlossen ist, der Snapshot entfernt und ein Wiederherstellungspunkt erstellt. Dieser Wiederherstellungspunkt kann im klassischen Azure-Portal angezeigt werden.

>[AZURE.NOTE] Für virtuelle Linux-Computer kann nur Datei konsistente Sicherung.

## <a name="prerequisites"></a>Erforderliche Komponenten
Vorbereiten von Azure Backup Sichern von DPM-Daten wie folgt:

1. **Erstellen eines Depots Backup** – ein Depot in der Azure Backup-Konsole erstellen.
2. **Anmeldeinformationen zum Herunterladen Depot** – In Azure Backup erstellten Zeugnisse Depot hochladen.
3. **Azure Backup-Agent installieren und registrieren Sie den Server** – von Azure Backup, installieren Sie den Agent auf jedem DPM-Server und backup Depot den DPM-Server anmelden.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Vorschriften (und Grenzen)

- DPM kann als physischen Server oder einem Hyper-V-VM auf System Center 2012 SP1 oder System Center 2012 R2 installiert ausgeführt werden. Kann auch als eine Azure virtuellen Computer mit System Center 2012 R2 mit mindestens ausgeführt werden DPM 2012 R2 Updaterollup 3 oder einem Windows-Computer in System Center 2012 R2 mit mindestens unter VMWare Updaterollup 5.
- Wenn Sie DPM mit System Center 2012 SP1 ausführen, installieren Sie Update Rollup 2 für System Center Data Protection Manager SP1. Dies ist erforderlich, bevor Azure Backup-Agent installieren zu können.
- Der DPM-Server muss Windows PowerShell und .net Framework 4.5 installiert.
- DPM kann die meisten Arbeitslasten auf Azure Backup sichern. Für eine vollständige Liste der unterstützten Siehe hat unterstützen Azure Backup Elemente.
- Mit der Option "auf Band kopieren" können Daten in Azure Backup wiederhergestellt werden.
- Sie benötigen ein Azure-Konto mit Azure Backup aktiviert. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Informationen Sie zu [Preisen Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
- Mithilfe von Azure Backup erfordert Azure Backup-Agent auf den Servern installiert werden, die Sie sichern möchten. Jeder Server muss mindestens 10 % der Größe der Daten, die als lokaler Speicher gesichert wird. 100 GB an Daten sichern muss z. B. mindestens 10 GB freien Speicherplatz im Scratch Speicherort. Mindestens ist 10 %, 15 % der lokalen Speicherplatz für den Cachespeicherort verwendet werden empfohlen.
- Die Daten werden in der Azure-Tresor. Ist unbegrenzt, bis ein Azure Backup vault können, Datenmenge, aber die Größe einer Datenquelle (z. B. einen virtuellen Computer oder eine Datenbank) sollte 54.400 GB nicht überschreiten.

Diese Dateitypen werden bis Azure unterstützt:

- Verschlüsselt (vollständige Sicherung)
- Komprimiert (inkrementelle Backups unterstützt)
- Sparse (inkrementelle Backups unterstützt)
- Komprimierte und sparse (als Sparse)

Und diese werden nicht unterstützt:

- Server in Kleinschreibung Dateisysteme werden nicht unterstützt.
- Feste Links (übersprungen)
- Analysepunkte (übersprungen)
- Verschlüsselte und komprimierte (übersprungen)
- Verschlüsselten und vereinzelten (übersprungen)
- Komprimierte stream
- Sparse-stream

>[AZURE.NOTE] Vom können in System Center 2012 DPM mit SP1, Sie Arbeitslasten geschützt durch DPM Azure mit Microsoft Azure Backup sichern.
