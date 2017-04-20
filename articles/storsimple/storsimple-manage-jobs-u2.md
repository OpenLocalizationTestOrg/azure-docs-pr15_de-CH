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

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>Verwenden des StorSimple Manager-Dienstes anzeigen und Verwalten von Aufträgen StorSimple (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Übersicht

Die Seite **Projekte** bietet ein zentrales Portal für anzeigen und Verwalten von Aufträgen, die auf Geräten gestartet wurden mit der StorSimple Manager-Dienst. Sie können geplante ausgeführt, abgeschlossen, abgebrochen und fehlgeschlagene Aufträge für mehrere Geräte anzeigen. Ergebnisse werden im Tabellenformat angezeigt. 

![Seite "Aufträge"](./media/storsimple-manage-jobs-u2/jobs.png)

Schnell finden die Aufträge, die Sie interessanten wie Felder filtern:

- **Status** – Aufträge können ausgeführt werden, abgeschlossen, abgebrochen, fehlgeschlagene, Abbrechen oder mit Fehlern abgeschlossen.
- **Und** – Aufträge können basierend auf den Datums- und Zeitbereich gefiltert werden.
- **Typ** der Einzelvorgangstyp kann backup, manuelle Sicherung wiederherstellen, Klonen, Device-Failover lokalen Volumes erstellen, Volume bearbeiten, aktualisieren, support-Paket oder virtuelles Gerät bereitstellen.

- **Geräte** -Aufträge auf bestimmte Geräte mit dem Dienst initiiert.
Die gefilterten Aufträge werden dann anhand der folgenden Attribute aufgelistet:

    - **Typ** – backup, manuelle Sicherung wiederherstellen, Klonen, Device-Failover lokalen Volumes erstellen, Volume bearbeiten, aktualisieren, support-Paket oder virtuelles Gerät bereitstellen.

    - **Status** – ausgeführt, abgeschlossen, abgebrochen, fehlgeschlagene, Abbrechen oder mit Fehlern abgeschlossen.

    - **Einheit** – die Aufträge können ein Volume eine Sicherungsrichtlinie oder ein Gerät zugeordnet werden. Beispielsweise ist ein Klon Einzelvorgang ein Volume zugeordnet geplanter Sicherungsauftrag einer Sicherungsrichtlinie zugeordnet ist. Ein Gerät Auftrag wird Disaster Recovery (DR) oder einer Wiederherstellung erstellt.

    - **Device** -Namen des Geräts auf dem der Auftrag gestartet wurde.

    - **Gestartet auf** – die Zeit, wann der Auftrag gestartet wurde.

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
 
    ![Job-Detailseite](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Die folgenden Schritte Abbrechen eines laufenden Auftrags.

>[AZURE.NOTE] Einige Projekte, wie ein Volume zum Volume ändern ändern oder Erweitern eines Volumes können nicht abgebrochen werden.

### <a name="to-cancel-a-job"></a>Beim Abbrechen eines Auftrags

1. Zeigen Sie auf der Seite **Projekte** ausgeführten Einzelvorgänge abbrechen durch Ausführen einer Abfrage mit entsprechenden Filter an

1. Wählen Sie den Auftrag.

1. Klicken Sie am unteren Rand der Seite auf **Abbrechen**.

1. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**.

Dieser Auftrag wird abgebrochen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie [Ihre StorSimple backup-Richtlinien](storsimple-manage-backup-policies.md)verwalten.

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
