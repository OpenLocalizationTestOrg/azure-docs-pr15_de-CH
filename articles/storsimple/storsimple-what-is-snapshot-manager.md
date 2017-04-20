<properties 
   pageTitle="Was ist StorSimple Snapshot-Manager? | Microsoft Azure"
   description="StorSimple Snapshot-Manager, der Architektur und Funktionen beschrieben."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="what-is-storsimple-snapshot-manager"></a>Was ist StorSimple Snapshot-Manager?

## <a name="overview"></a>Übersicht

StorSimple Snapshot-Manager ist ein Snap-in der Microsoft Management Console (MMC), die den Datenschutz und backup-Management in einer Umgebung mit Microsoft Azure StorSimple vereinfacht. StorSimple Snapshot Manager Microsoft Azure StorSimple Daten im Rechenzentrum und in der Cloud als einzigen integrierten Storage-Lösung verwalten Sie also backup-Prozesse vereinfachen und Kosten senken.

In dieser Übersicht StorSimple Snapshot-Manager führt, beschreibt seine Funktionen und erläutert die Rolle Microsoft Azure StorSimple. 

Überblick über das gesamte Microsoft Azure StorSimple-System, einschließlich StorSimple-Gerät StorSimple Manager Service StorSimple Snapshot-Manager, und StorSimple-Adapter für SharePoint finden Sie unter [StorSimple 8000-Serie: einen Hybriden Cloud Storage-Lösung](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- StorSimple Snapshot-Manager können Sie Microsoft Azure StorSimple virtuelle Arrays verwalten (auch lokal StorSimple virtuelle Geräte).
>
>- Möchten Sie StorSimple Update 2 auf dem StorSimple-Gerät installieren, müssen Sie die neueste Version von StorSimple Snapshot-Manager und **vor der Installation von StorSimple 2**installieren. Die neueste Version von StorSimple Snapshot Manager ist abwärts kompatibel und funktioniert mit allen Versionen von Microsoft Azure StorSimple. Wenn Sie die vorherige Version von StorSimple Snapshot-Manager verwenden, müssen Sie aktualisieren (Sie müssen nicht auf die vorherige Version deinstallieren, bevor Sie die neue Version installieren).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot-Manager Zweck und Architektur

StorSimple Snapshot-Manager stellt eine zentrale Verwaltungskonsole, konsistente erstellen Sicherungskopien von lokalen Point-in-Time- und clouddaten. Beispielsweise können Sie die Konsole:

- Konfigurieren, Sichern und Löschen von Volumes.
- Konfigurieren von Volume-Gruppen sicherzustellen, die gesicherten Daten Anwendung konsistent ist.
- Verwalten Sie backup-Policies, sodass Daten nach einem festgelegten Zeitplan gesichert.
- Erstellen Sie lokale und cloud-Snapshots, die in der Cloud gespeichert und für die Wiederherstellung verwendet werden können.

StorSimple Snapshot-Manager ruft die Liste der Programme mit dem VSS-Anbieter auf dem Server registriert. Um anwendungskonsistente Backups zu erstellen, überprüft, die von einer Anwendung verwendeten Volumes und Volume-Gruppen konfigurieren vorschlägt. StorSimple Snapshot-Manager verwendet diese Volume-Gruppen zu Sicherungskopien, die Anwendung konsistent sind. (Anwendungskonsistenz gibt alle zugehörigen Dateien und Datenbanken werden synchronisiert den wahren Status der Anwendung zu einem bestimmten Zeitpunkt darstellen.) 

StorSimple Snapshot-Manager Backups nehmen die Form von inkrementellen Snapshots, die nur die seit der letzten Sicherung erfassen. Daher Backups erfordert weniger Speicherplatz erstellt und schnell wiederhergestellt. StorSimple Snapshot-Manager verwendet Windows Volume Shadow Copy Service (VSS), um sicherzustellen, dass Snapshots anwendungskonsistente Daten erfassen. (Weitere Informationen zum Wechseln der Integration mit Windows Volume Shadow Copy Service) StorSimple Snapshot Manager Sie können backup-Zeitpläne erstellen oder sofortige Backups nach Bedarf. Benötigen Sie wiederherstellen aus einer Sicherung StorSimple Snapshot-Manager können wählen Sie einen Katalog von lokalen oder Cloud-Snapshots. Azure StorSimple stellt nur die erforderlichen Daten benötigt, die Verzögerung in Daten während der Wiederherstellung verhindert.)

![StorSimple Snapshot-Manager-Architektur](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot-Manager-Architektur** 

## <a name="support-for-multiple-volume-types"></a>Unterstützung für mehrere Datenträger

StorSimple Snapshot-Manager können Sie konfigurieren, und folgende Volumes sichern: 

- **Basisvolumes** – ein Basisvolume ist eine Partition auf einem Basisdatenträger. 

- **Einfache Volumes** – ein einfaches Volume ist ein dynamisches Volume, das aus einem einzigen dynamischen Datenträger Speicherplatz enthält. Ein einfaches Volume besteht aus einem einzelnen Festplattenbereich oder aus mehreren Bereichen zusammen auf demselben Datenträger verknüpft sind. (Sie können einfache Volumes nur auf dynamischen Festplatten erstellen.) Einfache Volumes sind nicht fehlertolerant.

- **Dynamische Volumes** – ein dynamisches Volume ist ein Volume auf einem dynamischen Datenträger erstellt. Dynamische Datenträger verwenden eine Datenbank Informationen zu enthaltenen Volumes auf dynamischen Datenträgern in einem Computer verfolgen. 

- **Dynamische Volumes mit Spiegelung** – dynamische Volumes mit Spiegelung basieren auf RAID 1-Architektur. Mit RAID 1 werden identische Daten auf zwei oder mehr Datenträger Erzeugen einer gespiegelten Gruppe geschrieben. Eine Leseoperation kann dann vom Datenträger mit den angeforderten Daten behandelt werden.

- **Cluster freigegebenen Clustervolumes** – mit Cluster freigegebenen Clustervolumes (CSVs) mehrere Knoten in einem Failovercluster können gleichzeitig lesen oder Schreiben auf die Festplatte. Failover von einem Knoten auf einen anderen Knoten möglich, ohne eine Änderung im Laufwerk Eigentum oder bereitstellen, Aufheben der Bereitstellung und ein Volume entfernen. 

>[AZURE.IMPORTANT] Mischen Sie CSVs und CSVs in der gleichen Momentaufnahme nicht. Kombinieren von CSVs und CSVs in einem Snapshot wird nicht unterstützt. 
 
StorSimple Snapshot-Manager können zum gesamten Volume-Gruppen oder einzelne Volumes clone wiederherstellen einzelne Dateien.

- [Volumes und Volume-Gruppen](#volumes-and-volume-groups) 
- [Sicherungstypen und backup-policies](#backup-types-and-backup-policies) 

Weitere Informationen zu StorSimple Snapshot-Manager-Features und deren Verwendung finden Sie unter [StorSimple Snapshot-Manager-Benutzeroberfläche](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Volumes und Volume-Gruppen

StorSimple Snapshot Manager Volumes erstellen und konfigurieren sie in Volume-Gruppen. 

StorSimple Snapshot-Manager verwendet Volumegruppen Sicherungskopien erstellen, die Anwendung konsistent sind. Anwendungskonsistenz vorhanden ist, wenn alle Dateien zugehörigen und Datenbanken synchronisiert sind und den wahren Status einer Anwendung zu einem bestimmten Zeitpunkt dar. Volume-Gruppen (die auch *Konsistenzgruppen*) bildet die Grundlage einer Sicherung oder Wiederherstellungsauftrag.

Volume-Gruppen sind nicht dasselbe wie volumecontainer. Ein volumecontainer enthält ein oder mehrere Volumes, die ein Speicherkonto Cloud und andere Attribute wie Verschlüsselung und Bandbreite. Ein einzelnes volumecontainer kann bis zu 256 Thin Provisioning StorSimple-Volumes enthalten. Weitere Informationen zu volumecontainern finden Sie zu [den volumecontainern](storsimple-manage-volume-containers.md). Volume-Gruppen sind Sammlungen von Volumes, die Sie konfigurieren, um Backups zu ermöglichen. Wenn Sie zwei Volumes gehören zu verschiedenen Containern, die ein einzelnes Volume-Gruppe ablegen und erstellen Sie eine Sicherungsrichtlinie für die Volumegruppe auswählen, wird jedes Volume angemessener Container mit dem entsprechenden Konto gesichert werden.

>[AZURE.NOTE] Alle Volumes in einer Volume-Gruppe müssen aus einer einzelnen Cloud-Dienstanbieter kommen.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integration mit Windows Volumeschattenkopie-Dienst

StorSimple Snapshot-Manager verwendet Windows Volume Shadow Copy Service (VSS) anwendungskonsistente Daten erfassen. VSS ermöglicht Anwendungskonsistenz mit VSS-fähigen Anwendung Erstellung von inkrementellen Snapshots koordinieren. VSS wird sichergestellt, dass die Anwendung vorübergehend inaktiv oder Ruhezustand, wenn Snapshots erstellt werden. 

StorSimple Snapshot-Manager Implementierung von VSS zusammen mit SQL Server und generische NTFS-Volumes. Der Prozess ist wie folgt: 

1. Requestor ist normalerweise ein Datenmanagement und Lösung (wie StorSimple Snapshot Manager) oder eine Anwendung ruft VSS und fordert Informationen von Software Writer Anwendung erfasst.

2. VSS Kontakte Writer-Komponente, um eine Beschreibung der Daten abzurufen. Der Writer gibt die Beschreibung der zu sichernden Daten. 

3. VSS signalisiert den Writer, den die Anwendung für die Sicherung vorzubereiten. Der Writer bereitet die Daten für die Sicherung von offenen Transaktionen abschließen Transaktionsprotokolle usw. aktualisieren und benachrichtigt VSS

4. VSS weist den Writer zu vorübergehend Datenspeicher der Anwendung sicherstellen, dass keine Daten auf den Datenträger geschrieben werden, während die Schattenkopie erstellt wird. Dieser Schritt gewährleistet Datenkonsistenz und nicht mehr als 60 Sekunden dauert.

5. VSS wird angewiesen, um die Schattenkopie zu erstellen. Anbieter, die Software oder Hardware-basierte, Verwalten von Datenträgern, die derzeit ausgeführt und Schattenkopien von ihnen bei Bedarf erstellen. Der Anbieter die Schattenkopie erstellt und VSS benachrichtigt, wenn es abgeschlossen ist.

6. VSS kontaktiert den Writer die Anwendung zu benachrichtigen, die e/a fortgesetzt und bestätigen, dass e/a während der Shadow Copy-Erstellung erfolgreich angehalten wurde. 

7. Wenn der Kopiervorgang erfolgreich war, gibt VSS Speicherort für die Kopie an den Requestor. 

8. Wenn Daten geschrieben wurde, während die Schattenkopie erstellt wurde, wird die Sicherung inkonsistent. VSS löscht die Schattenkopie und informiert den Requestor. Der Requestor des backup-Vorgangs automatisch wiederholen oder den Administrator zu einem späteren Zeitpunkt wiederholen.

Abbildung.

![VSS-Prozess](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows Volume Shadow Copy Service-Prozess** 

## <a name="backup-types-and-backup-policies"></a>Sicherungstypen und backup-policies

Mit StorSimple Snapshot-Manager können Sie Daten sichern und lokal und in der Cloud speichern. Können Sie StorSimple Snapshot-Manager Daten sofort sichern oder können Sie eine Sicherungsrichtlinie erstellen ein Zeitplans zum automatisch Sicherungskopien erstellen. Backup-Policies können Sie angeben, wie viele Snapshots beibehalten werden. 

### <a name="backup-types"></a>Sicherungsarten

StorSimple Snapshot-Manager können die folgenden Arten von Backups erstellen:

- **Lokale Snapshots** – lokale Snapshots sind Point-in-Time-Kopien der Volume-Daten auf dem Gerät StorSimple. Normalerweise kann diese Art der Sicherung erstellt und schnell wiederhergestellt werden. Lokale Snapshots können wie eine lokale Kopie.

- **Cloud-Snapshots** – Cloud Snapshots Point-in-Time-Kopien der Volume-Daten sind, die in der Cloud gespeichert sind. Cloudmomentaufnahme entspricht eine Momentaufnahme auf einem anderen, externen Speichersystem repliziert. Cloud-Snapshots sind besonders für Disaster Recovery-Szenarien.

### <a name="on-demand-and-scheduled-backups"></a>Auf Anforderung und geplante backups

StorSimple Snapshot Manager eine einmalige Sicherung sofort erstellt initiieren oder können Sie eine Sicherungsrichtlinie planen Sie regelmäßige Backups.

Eine Sicherungsrichtlinie ist eine Reihe von automatisierten Regeln, mit denen Sie reguläre Sicherungen. Backup-Richtlinie können Sie die Häufigkeit und die Parameter für Snapshots von einem bestimmten Volume-Gruppe definieren. Können Sie Richtlinien geben Sie Start- und Enddatum, Zeiten Frequenzen und Aufbewahrungspflichten für lokalen und Snapshots. Eine Richtlinie wird angewendet, sobald Sie definieren. 

StorSimple Snapshot-Manager können Sie konfigurieren oder backup-Richtlinien bei Bedarf konfigurieren. 

Sie konfigurieren die folgende Informationen für jede backup-Richtlinie, die Sie erstellen:

- **Name** – der eindeutige Name der ausgewählten Sicherungsrichtlinie.

- **Typ** – der Typ Sicherungsrichtlinie; lokale Snapshot oder Snapshot Cloud.

- **Volume-Gruppe** – der Volume-Gruppe ausgewählte Sicherungsrichtlinie zugewiesen.

- **Archivierung** – die Anzahl der Sicherungskopien beibehalten. Wenn Sie die **Alle** Kontrollkästchen bleiben alle Sicherungskopien bis die maximale Anzahl von backup-Kopien pro Volume erreicht ist, an welcher Stelle die Richtlinie fehl und eine Fehlermeldung. Alternativ können Sie eine Anzahl von Sicherungskopien beibehalten (zwischen 1 und 64) angeben.

- **Datum** – das Datum der Erstellung die Sicherungsrichtlinie.

Informationen zum Konfigurieren von backup-Policies wechseln Sie [Mit StorSimple Snapshot-Manager erstellen und Verwalten von backup-Policies](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Sicherungsauftrag Überwachung und Verwaltung

StorSimple Snapshot-Manager können Sie Überwachung und Verwaltung der nächsten geplanten und abgeschlossenen backup-Jobs. StorSimple Snapshot-Manager bietet außerdem einen Katalog mit bis zu 64 abgeschlossenen Backups. Können Sie den Katalog suchen und Wiederherstellen von Volumes oder einzelne Dateien. 

Informationen zum Überwachen von backup-Jobs wechseln Sie [Mit StorSimple Snapshot-Manager anzeigen und Verwalten von backup-Jobs](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).

- [StorSimple Snapshot-Manager](https://www.microsoft.com/download/details.aspx?id=44220)herunterladen
