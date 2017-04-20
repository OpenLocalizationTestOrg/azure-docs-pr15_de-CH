<properties
    pageTitle="Hyper-V-Replikation mit Azure Site Recovery | Microsoft Azure"
    description="Verwenden Sie in diesem Artikel verstehen die technischen Konzepte, mit denen Sie erfolgreich installieren, konfigurieren und Verwalten von Azure Site Recovery."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Hyper-V-Replikation mit Azure Site Recovery

Dieser Artikel beschreibt die technischen Konzepte, mit denen Sie erfolgreich konfigurieren und Verwalten einer Hyper-V oder einem System Center Virtual Machine Manager (VMM) Standort Azure Schutz mithilfe von Azure Site Recovery.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Einrichten der quellumgebung für die Replikation zwischen lokalen und Azure

Als Bestandteil der Wiederherstellung zwischen lokalen und Azure müssen Sie downloaden und installieren Sie Azure Site Recovery auf dem VMM-Server. Installieren Sie Azure Recovery Services Agent auf jedem Hyper-V-Host.

![VMM Site Deployment für die Replikation zwischen lokalen und Azure](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Einrichten der Quell-Umgebung in einer Hyper-V verwalteten Infrastruktur ähnelt der Quelle Umgebung VMM verwalteten Infrastruktur. Der einzige Unterschied ist, dass der Anbieter und der Agent auf dem Hyper-V-Host selbst installiert sind. In der Umgebung VMM Anbieter auf dem VMM-Server installiert ist und der Agent auf den Hyper-V-Hosts (bei Azure Replikation).

## <a name="workflows"></a>Workflows

### <a name="enable-protection"></a>Schutz aktivieren
Nachdem Sie einen virtuellen Computer aus dem Azure-Portal oder lokalen schützen, startet Site Recovery Auftrag **Schutz aktivieren** . Sie können auf der Registerkarte **Aufträge** überwachen.

![Auftrag in der Liste der Aufträge "Schutz aktivieren"](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

Der **Schutz** Auftrag überprüft für die erforderlichen Komponenten vor dem Aufrufen der Methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) . Diese Methode erstellt Azure-Replikation mit Eingaben während Schutz konfiguriert werden.

Der **Schutz** Einzelvorgang der ersten Replikations lokal durch Aufrufen der [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) -Methode. Diese Methode sendet Laufwerke des virtuellen Computers in Azure.

![Details für den Auftrag "Schutz aktivieren"](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Auf dem virtuellen Computer abschließen
[Hyper-V-VM-Snapshot](https://technet.microsoft.com/library/dd560637.aspx) wird ausgeführt, wenn die erste Replikation ausgelöst wird. Virtueller Festplatten werden verarbeitet, bis alle Datenträger in Azure hochgeladen werden. Dieser Vorgang dauert normalerweise eine Weile, basierend auf die Größe und die Bandbreite. Um die Nutzung zu optimieren, finden Sie unter [lokal zu Azure Schutz Netzwerkbandbreite zu verwalten](https://support.microsoft.com/kb/3056159).

Nach Abschluss die anfängliche Replikation konfiguriert die **Finalize auf dem virtuellen Computer** Schutzauftrag Einstellungen Netzwerk- und nach der Replikation. Während die erste Replikation ausgeführt wird:

- Alle werden auf dem Datenträger Überarbeitungen. 
- Zusätzlichen Speicherplatz verbraucht der Snapshot und Hyper-V Replica Log (HRL) Dateien.

Nach Abschluss der anfänglichen Replikation wird der Hyper-V-VM-Snapshot gelöscht. Dieser Löschvorgang führt Daten nach der anfänglichen Replikation der übergeordneten Festplatte zusammenführen.

!["Auf dem virtuellen Computer abschließen" Auftrag in der Liste der Aufträge](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta-Replikation
Hyper-V Replica Replikation Tracker ist Teil der Hyper-V Replica-Replikationsmodul, verfolgt der ändert sich in eine virtuelle Festplatte als Hyper-V Replica (*.hrl) Dateien. HRL-Dateien in demselben Verzeichnis wie die zugeordnete Laufwerke.

Jedem Datenträger für die Replikation konfiguriert ist eine HRL-Datei zugeordnet. Dieses Protokoll wird nach Abschluss der anfänglichen Replikation Debitorenkonto Speicher gesendet. Wird ein Protokoll auf dem Weg in Azure, sind die in den in einer anderen Datei im selben Verzeichnis Überarbeitungen.

Während der ersten Replikation oder Deltareplikation können Sie VM Replikation an VM überwachen wie [Monitor-Replikationsstatus für virtuelle Computer](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine).  

### <a name="resynchronization"></a>Erneute Synchronisierung
Ein virtuellen Computer ist markiert, für die erneute Synchronisierung sowohl Deltareplikation fehlschlägt und vollständige Replikation Netzwerkbandbreite oder Zeit beansprucht. Beispielsweise wenn HRL Größe bis zu 50 Prozent der gesamten Datenträgergröße Stapel, den virtuellen Computer für eine erneute Synchronisierung markiert wird. Resynchronisation minimiert Kontrollsummen Quell- und virtuellen Datenträger und die Differenz senden über das Netzwerk gesendeten Datenmenge.

Nach Abschluss der erneuten Synchronisierung sollte normal Deltareplikation fortsetzen. Erneute Synchronisierung kann fortgesetzt werden, wenn ein Netzwerkausfall oder einem anderen Ausfall auftritt.

Standardmäßig konfiguriert automatisch geplanten Resynchronisation wird außerhalb der Arbeitszeit auftreten. Wenn der virtuelle Computer manuell neu synchronisiert werden muss, wählen Sie den virtuellen Computer aus dem Portal und auf **Synchronisieren**.

![Manuelle erneute Synchronisierung](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Resynchronisation verwendet einen festen Block chunking Algorithmus, Quell-und Zieldateien in festen Blöcke unterteilt. Prüfsummen für jeden Datenblock generiert und anschließend verglichen, um zu bestimmen, welche Blöcke von der Quelle auf das Ziel angewendet werden müssen.

### <a name="retry-logic"></a>Wiederholungslogik
Es gibt integrierte Wiederholungslogik Replikationsfehler. Diese Logik kann in zwei Kategorien unterteilt werden:

| Kategorie                  | Szenarien                                    |
|---------------------------|----------------------------------------------|
| Nicht behebbarer Fehler     | Kein erneuter Versuch durchgeführt. Virtual Machine-Replikationsstatus ist **kritisch**und Administratoreingriff erforderlich ist. Beispiele: <ul><li>VHD Kette</li><li>Ungültiger Status für das Replikat VM</li><li>Netzwerk-Authentifizierungsfehler</li><li>Authentifizierungsfehler</li><li>VM, eigenständige Hyper-V-Server nicht gefunden</li></ul>|
| Behebbarer Fehler         | Versuche auftreten jedes Replikationsintervall verwenden eine exponentielle Backoff-, der das Wiederholungsintervall ab dem ersten Versuch (1, 2, 4, 8, 10 Minuten) erhöht. Wenn ein Fehler auftritt, wiederholen Sie alle 30 Minuten. Beispiele: <ul><li>Netzwerkfehler</li><li>Wenig Speicherplatz</li><li>Nicht genügend Speicher verfügbar</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Lebenszyklus von Hyper-V-Computer Schutz und recovery

![Lebenszyklus von Hyper-V-Computer Schutz und recovery](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Weitere Verweise

- [Überwachung und Schutz für VMware VMM Hyper-V und physischen Fehlerbehebung](./site-recovery-monitoring-and-troubleshooting.md)
- [Microsoft Support erreichen](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Azure Site Recovery-Fehlermeldungen und deren Lösung](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
