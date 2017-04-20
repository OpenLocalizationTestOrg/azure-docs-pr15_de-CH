<properties 
   pageTitle="Anzeigen und Verwalten von Aufträgen StorSimple | Microsoft Azure"
   description="Beschreibt StorSimple Manager Service Aufträge Seite und zum Nachverfolgen von aktuellen, aktuelle und geplante Sicherungsaufträge verwenden."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Verwenden des StorSimple Manager-Dienstes anzeigen und Verwalten von Aufträgen StorSimple

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Übersicht

Die Seite **Projekte** bietet ein zentrales Portal für anzeigen und Verwalten von Aufträgen, die auf Geräten gestartet wurden mit der StorSimple Manager-Dienst. Sie können geplante ausgeführt, abgeschlossen und fehlgeschlagene Aufträge für mehrere Geräte anzeigen. Ergebnisse werden im Tabellenformat angezeigt. 

![Seite "Aufträge"](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Schnell finden die Aufträge, die Sie interessanten wie Felder filtern:

- **Status** – Aufträge können geplante fehlgeschlagen, abgeschlossen, abgebrochen oder stornierte ausgeführt.

- **Typ** – Arbeitsplätze können aufgrund einer geplanten oder eine Sicherung bei Bedarf (**Backup nutzen**), Klonen, Gerät wiederherstellen oder einen Aktualisierungsvorgang.

- **Geräte** -Aufträge auf bestimmte Geräte mit dem Dienst initiiert.

- **Und** – Aufträge können basierend auf den Datums- und Zeitbereich gefiltert werden.

Die gefilterten Aufträge werden dann anhand der folgenden Attribute aufgelistet:

- **Typ** – Backup, Clone, Wiederherstellung, Failover oder aktualisieren.

- **Status** – ausgeführt, geplante fehlgeschlagen, abgeschlossen, abgebrochen oder abgebrochen.

- **Einheit** – die Aufträge können ein Volume eine Sicherungsrichtlinie oder ein Gerät zugeordnet werden. Klonauftrag ist ein Volume zugeordnet, ein geplanter Sicherungsauftrag einer Sicherungsrichtlinie zugeordnet ist. Ein Gerät Auftrag wird Disaster Recovery (DR) oder einer Wiederherstellung erstellt.

- **Device** -Namen des Geräts auf dem der Auftrag gestartet wurde.

- **Schritte für** die Zeit, wann der Auftrag gestartet wurde.

- **Fortschritt** – der Prozentsatz Projektabschlusses ausgeführt. Bei abgeschlossenen Jobs sollte dies immer 100 %.

Die Liste der Aufträge wird alle 30 Sekunden aktualisiert.

Auf dieser Seite können Sie projektbezogene Folgendes ausführen:

- Job-Details anzeigen

- Abbrechen eines Auftrags

## <a name="view-job-details"></a>Job-Details anzeigen

Führen Sie die folgenden Schritte zum Anzeigen der Details eines Auftrags.

#### <a name="to-view-job-details"></a>Job-Details anzeigen

1. Zeigen Sie auf der Seite **Projekte** die Sie sich interessieren, durch Ausführen einer Abfrage mit entsprechenden Filtern Einzelvorgänge an Sie können Suchen abgeschlossen, ausgeführt oder Aufträge abgebrochen.

2. Wählen Sie einen Einzelvorgang aus.

3. Klicken Sie am unteren Rand der Seite auf **Details**.

4. Im Dialogfeld " **Backup Job Details** " können Sie Status, Details, Statistiken und Statistik anzeigen.

## <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Die folgenden Schritte Abbrechen eines laufenden Auftrags.

### <a name="to-cancel-a-job"></a>Beim Abbrechen eines Auftrags

1. Zeigen Sie auf der Seite **Projekte** ausgeführten Einzelvorgänge abbrechen durch Ausführen einer Abfrage mit entsprechenden Filter an

1. Wählen Sie den Auftrag.

1. Klicken Sie am unteren Rand der Seite auf **Abbrechen**.

1. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**.

Dieser Auftrag wird abgebrochen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie [Ihre StorSimple backup-Richtlinien](storsimple-manage-backup-policies.md)verwalten.

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
