<properties
    pageTitle="Planen der VM-backup-Infrastruktur in Azure | Microsoft Azure"
    description="Wichtig Wenn Sie virtuelle Computer in Azure sichern"
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""
    keywords="Backup Vms von virtuellen Computern"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="trinadhk; jimpark; markgal;"/>

# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Die VM-backup-Infrastruktur in Azure planen
Dieser Artikel bietet Leistung und Ressource Vorschläge für die VM-backup-Infrastruktur planen. Außerdem werden wichtige Aspekte der Sicherungsdienst definiert. Diese Aspekte ist entscheidend dafür, Ihrer Architektur Kapazität planen. Sie [die Umgebung vorbereitet](backup-azure-vms-prepare.md)haben, ist der nächste Schritt [auf backup VMs](backup-azure-vms.md)vor. Benötigen Sie weitere Informationen zu Azure virtuelle Computer, [virtuelle Computer Dokumentation](https://azure.microsoft.com/documentation/services/virtual-machines/)anzeigen

## <a name="how-does-azure-back-up-virtual-machines"></a>Wie Azure wird von virtuellen Computern?
Azure Backup Service einen Sicherungsauftrag zum geplanten Zeitpunkt initiiert, löst die Sicherung Erweiterung Point-in-Time-Snapshots. Diese Momentaufnahme erfolgt in Koordination mit Volume Shadow Copy Service (VSS), einen konsistenten Snapshot der Laufwerke auf dem virtuellen Computer ohne Herunterfahren.

Nachdem die Momentaufnahme werden die Daten von Azure Backup Service backup Depot übertragen. Den Sicherungsvorgang effizienter, der Dienst identifiziert und überträgt nur die Datenblöcke, die seit der letzten Sicherung geändert wurden.

![Azure Virtual Machine backup-Architektur](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Wenn die Datenübertragung abgeschlossen ist, der Snapshot entfernt und ein Wiederherstellungspunkt erstellt.

### <a name="data-consistency"></a>Datenkonsistenz
Sichern und Wiederherstellen von Unternehmen wichtige Daten durch die Tatsache kompliziert, die geschäftskritische Daten während der Anwendung gesichert werden müssen, die die Daten ausführen. Dieses Problem umgehen, bietet Azure Backup anwendungskonsistente Backups zum Microsoft-Workloads mit VSS sicherstellen, dass die Daten ordnungsgemäß in den Speicher geschrieben werden.

>[AZURE.NOTE] Für virtuelle Linux-Computer können nur Datei konsistente Backups, Linux weist keine entsprechende Plattform VSS

Azure Backup nimmt VSS vollständige Backups Windows VMs (mehr über [VSS-Sicherung](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). VSS-Kopie-Sicherung aktivieren die folgenden Registry Key muss auf dem virtuellen Computer.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```


Diese Tabelle erläutert die Typen von Konsistenz und die Umständen, die sie unter Azure VM auftreten sichern und wiederherstellen.

| Konsistenz | VSS-basierte | Erklärung und details |
|-------------|-----------|---------|
| Anwendungskonsistenz | Ja | Dies ist der ideale Konsistenz Typ für Microsoft-Arbeitslasten, sicherstellt:<ol><li> Die VM aus *startet*. <li>Es ist *keine Beschädigung*. <li>Es ist *kein Datenverlust*.<li> Die Daten entspricht der Anwendung, die Daten verwendet, die zum Zeitpunkt der Sicherung mit VSS mit</ol> Die meisten Microsoft-Arbeitslasten haben VSS Writer, die Arbeitslast Maßnahmen, die Datenkonsistenz zugeordnet sind. Beispielsweise hat Microsoft SQL Server einen VSS Writer, der sicherstellt, dass Schreibvorgänge in die Transaktionsprotokolldatei und die Datenbank ordnungsgemäß durchgeführt werden.<br><br> Azure VM heißt für Backups ein anwendungskonsistente Recovery Punkt backup Erweiterung konnte VSS Workflow aufrufen und es *ordnungsgemäß* abgeschlossen, bevor der VM-Snapshot erstellt wurde. Natürlich bedeutet dies, dass die VSS Writer in Azure VM Anwendung sowie aufgerufen wurde.<br><br>Erlernen Sie die [Grundlagen von VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) und tauchen Sie tief in die Details der [Funktionsweise](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx). |
| Dateisystem-Konsistenz | Ja – für Windows-basierte Computer | Es gibt zwei Szenarien kann der Wiederherstellungspunkt *Dateisystem konsistent*sein:<ul><li>Backups von Linux VMs in Azure Linux weist keine entsprechende Plattform VSS<li>VSS-Fehler bei der Sicherung für Windows VMs in Azure.</li></ul> In beiden Fällen ist das beste, das was möglich ist, um sicherzustellen, dass: <ol><li> Die VM aus *startet*. <li>Es ist *keine Beschädigung*.<li>Es ist *kein Datenverlust*.</ol> Clientanwendungen müssen einen eigenen Mechanismus "Fixup" auf die wiederhergestellten Daten implementieren.|
| Crash-Konsistenz | Nein | Dies entspricht einer virtuellen Maschine auf einen "Absturz" (harte oder weiche zurücksetzen). Dies geschieht normalerweise, wenn zum Zeitpunkt der Sicherung der Azure virtuelle Computer heruntergefahren wird. Azure Virtual Machine Backups gibt immer einen Punkt konsistente Recovery Azure Sicherung keine Garantien um die Konsistenz der Daten auf dem Speichermedium – aus der Perspektive des Betriebssystems oder aus der Perspektive der Anwendung. Nur Daten, die zum Zeitpunkt der Sicherung bereits auf dem Datenträger vorhanden ist, wird erfasst und gesichert. <br/> <br/> Es gibt keine Garantien in den meisten Fällen wird das Betriebssystem gestartet. In der Regel von einer Prozedur Überprüfung wie Chkdsk, danach alle Fehler beheben. Daten im Speicher oder vollständig nicht auf den Datenträger geleert wurden schreibt gehen verloren. Die Anwendung folgt in der Regel mit eigenen Überprüfung bei Rollback von Daten ausgeführt werden soll. <br><br>Beispielsweise hat das Transaktionslog Einträge, die nicht in der Datenbank vorhanden sind führt die Datenbanksoftware einen Rollback bis die Daten konsistent sind. Beim Zuweisen von Daten über mehrere virtuelle Laufwerke (wie übergreifende Volumes) bietet eine konsistente Recovery keine Garantie für die Richtigkeit der Daten.|


## <a name="performance-and-resource-utilization"></a>Leistung und Ressource
Wie backup-Software, die bereitgestellten lokalen sollten Sie Kapazität und Auslastung von Speicherressourcen Bedürfnisse beim Sichern von VMs in Azure. [Azure Speichergrenzwerte](azure-subscription-service-limits.md#storage-limits) definieren VM-Bereitstellung maximale Leistung bei minimaler Beeinträchtigung ausgeführte Arbeitslasten zu strukturieren.

Beachten Sie die folgenden Azure Speichergrenzwerte Planung backup-Performance:

- Max-Ausgang pro Konto
- Gesamtbetrag anfordern pro Konto

### <a name="storage-account-limits"></a>Speichergrenzwerte Konto
Wenn backup-Daten aus einem Speicherkonto kopiert werden, wird er e/a-Operationen pro Sekunde (IOPS) und ausgehenden (Durchsatz) Metriken des Speicherkontos. Zur gleichen Zeit werden die virtuellen Computer ausgeführt und IOPS und Durchsatz. Das Ziel besteht darin, des gesamten Datenverkehrs - backup und virtual Machine - Konto Speichergrenzwerte nicht überschreiten.

### <a name="number-of-disks"></a>Anzahl der Festplatten
Backup-Prozess versucht, einen Sicherungsauftrag so schnell wie möglich abgeschlossen. Damit nutzt so viele Ressourcen wie möglich. Alle e/a-Vorgänge sind jedoch beschränkt *Zieldurchsatz für einzelnes Blob*hat die maximal 60 MB pro Sekunde. Der Sicherungsvorgang versucht versucht seine Geschwindigkeit maximieren jede VM Festplatten *parallel*sichern. Also wenn ein virtueller Computer vier Festplatten verfügt, versucht Azure Backup Sichern alle vier Festplatten parallel. Deshalb ist wichtigste Faktor Sicherungsdatenverkehr beenden ein Speicherkonto Kunden die **Anzahl der Datenträger** aus dem Speicherkonto gesichert.

### <a name="backup-schedule"></a>Sicherungszeitplan
Ein weiterer Faktor, der Systemleistung ist **Sicherungszeitplan**. Wenn Sie Richtlinien konfigurieren, damit alle VMs gleichzeitig gesichert werden, haben einen Stau geplant. Der Sicherungsvorgang versucht, Sichern Sie alle Datenträger gleichzeitig. Eine Möglichkeit, den Sicherungsdatenverkehr über ein Speicherkonto reduzieren - sicherzustellen, dass andere VMs zu unterschiedlichen Zeiten mit Überlappung gesichert werden.

## <a name="capacity-planning"></a>Planen der Kapazität
Alle diese Faktoren Zusammenstellung bedeutet, dass Speicherverbrauch Konto ordnungsgemäß geplant werden. Downloaden Sie [VM backup-Kapazität Planung Excel-Arbeitsblatt](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) um die Auswirkung Ihrer Festplatte und Sicherungszeitplan Auswahlmöglichkeiten anzuzeigen.

### <a name="backup-throughput"></a>Backup-Durchsatz
Für jedes Laufwerk gesichert Azure Backup Blöcke auf der Festplatte liest und speichert nur die geänderten Daten (inkrementelle Sicherung). Diese Tabelle zeigt die Werte Durchschnittsdurchsatz von Azure Backup erwarten können. Dadurch können Sie die Zeitspanne schätzen, die dauert, um einen Datenträger einer bestimmten Größe zu sichern.

| Sicherung | Besten Durchsatz |
| ---------------- | ---------- |
| Erste backup | 160 MB/s |
| Inkrementelle Sicherung (DR) | 640 MB/s <br><br> Dieser Durchsatz kann deutlich ist viel verteilten Abwanderung auf dem Datenträger, der gesichert werden soll. |

## <a name="total-vm-backup-time"></a>VM backup Gesamtzeit
Während ein Großteil der Sicherung lesen und Kopieren von Daten aufgewendet wird, werden andere Vorgänge, die die Gesamtzeit zum Sichern einer VM benötigt beitragen:

- Zeitaufwand für die [Installation oder Aktualisierung die backup-Erweiterung](backup-azure-vms.md#offline-vms).
- Snapshot Zeit die Zeit einen Snapshot auszulösen. Snapshots werden pünktlich backup ausgelöst.
- Wartezeit der Warteschlange. Da der Sicherungsdienst Backups mehrere Kunden verarbeitet, möglicherweise Sicherungsdaten von Snapshot Backup oder Recovery Services Depot kopieren nicht sofort gestartet. In Zeiten von Peak laden, warten kann bis zu 8 Stunden durch die Anzahl der verarbeiteten Backups Strecken. Der gesamten Sicherungsdauer VM werden jedoch weniger als 24 Stunden täglich backup-Policies.

## <a name="best-practices"></a>Best Practices
Wir empfehlen folgende Vorgehensweisen beim Konfigurieren von Backups für virtuelle Maschinen:

- Nicht mehr als vier klassische VMs aus demselben Clouddienst zurück zur gleichen Zeit planen. Wir empfehlen staffeln backup Startzeiten von Stunde mehrere VMs aus demselben Clouddienst sichern möchten.
- Planen Sie nicht mehr als 40 Ressourcenmanager bereitgestellte virtuelle Computer gleichzeitig sichern.
- Planen Sie VM-Backups so der Sicherungsdienst IOPS für Datenübertragung vom Debitorenkonto Speicher zur Sicherung oder Recovery Services vault Spitzenzeiten nicht.
- Stellen Sie sicher, dass eine Richtlinie auf verschiedene Konten verteilen VMs behandelt. Wir empfehlen nicht mehr als 20 insgesamt Datenträgern ein einzelnes Speicherkonto durch eine Richtlinie geschützt werden. Wenn Sie ein Speicherkonto größer als 20 Festplatten haben, verteilt die VMs mehrere Richtlinien zu den erforderlichen IOPS Phase Übertragung des Sicherungsvorgangs.
- Einen virtueller Computer mit demselben Speicherkonto Premium-Speicher nicht wiederhergestellt werden. Fällt der Wiederherstellungsvorgang Vorgang der Sicherungsvorgang reduziert verfügbaren IOPS Backup.
- Wir empfehlen, jede VM Premium auf ein Speicherkonto entscheidende Resultate optimale backup-Performance sicherstellen.

## <a name="data-encryption"></a>Verschlüsselung

Azure Backup verschlüsselt keine Daten als Teil des Sicherungsvorgangs. Jedoch verschlüsselt Daten innerhalb der virtuellen Maschine und geschützten Daten problemlos sichern (mehr zur [Sicherung der verschlüsselten Daten](backup-azure-vms-encryption.md)).


## <a name="how-are-protected-instances-calculated"></a>Berechnung von geschützte Instanzen
Azure virtuelle Computer, die über Azure Backup gesichert unterliegen [Azure Backup Preise](https://azure.microsoft.com/pricing/details/backup/). Geschützte Instanzen Berechnung basiert auf die *tatsächliche* Größe des virtuellen Computers die Summe aller Daten auf dem virtuellen Computer - ausgenommen die "Festplatte".

Sie sind *nicht* belastet die maximale Größe, die für jeden virtuellen Computer angefügten Datenträger unterstützt wird, sondern auf die tatsächlichen Daten in den Datenträger. Ebenso basiert die Stückliste backup-Speicher auf die Datenmenge, die mit Azure Backup gespeichert werden, das die Summe der tatsächlichen Daten in jedem Wiederherstellungspunkt.

Beispielsweise nehmen Sie einen A2 Standard Größe virtuellen Computer mit zwei zusätzliche Datenträger mit maximal 1 TB. Die folgende Tabelle enthält die eigentlichen Daten auf diesen Laufwerken gespeichert:

|Datenträgertyp|Max. Größe|Aktuelle Daten|
|---------|--------|------|
| Betriebssystem-CD | 1023 GB | 17 GB |
| Festplatte / Festplatte | 135 GB | 5 GB (nicht für die Sicherung enthalten) |
| Datenträger 1 | 1023 GB | 30 GB |
| Datenträger 2 | 1023 GB | 0 GB |

Die *tatsächliche* Größe des virtuellen Computers ist in diesem Fall 17 GB + 30 GB + 0 GB = 47 GB. Dadurch wird die Größe der geschützten Instanz monatliche Rechnung auf der Grundlage. Mit zunehmender Datenmenge auf dem virtuellen Computer wird die Größe der geschützten Instanz zur Abrechnung auch entsprechend geändert.

Rechnung wird nicht gestartet, bis die erste erfolgreiche Sicherung abgeschlossen ist. Zu diesem Zeitpunkt beginnt die Abrechnung für Speicher und Instanzen geschützt. Abrechnung weiterhin als *backup mit Azure Sicherung gespeicherten Daten* für den virtuellen Computer Daten. Schutz beenden die Operation stoppt nicht die Abrechnung, wenn backup-Daten beibehalten werden.

Die Abrechnung für einen bestimmten virtuellen Computer wird nur der Schutz ist *und* alle Sicherungsdaten gelöscht eingestellt. Wenn keine aktiven Sicherungsaufträge (Wenn Schutz beendet wurde), wird die Größe des virtuellen Computers zum Zeitpunkt der letzten erfolgreichen Sicherung der Größe der geschützten Instanz monatliche Rechnung auf der Grundlage.

## <a name="questions"></a>Haben Sie Fragen?
Wenn Sie Fragen haben oder gibt es Funktion enthalten, angezeigt werden soll [uns Feedback senden](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte

- [Sichern von virtuellen Maschinen](backup-azure-vms.md)
- [Verwalten von Backups virtueller Maschinen](backup-azure-manage-vms.md)
- [Virtuelle Computer wiederherstellen](backup-azure-restore-vms.md)
- [VM-backup-Probleme beheben](backup-azure-vms-troubleshoot.md)
