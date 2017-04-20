<properties 
   pageTitle="StorSimple Snapshot-Manager Sicherungsaufträge | Microsoft Azure"
   description="Beschreibt das Anzeigen und Verwalten von geplanten, laufenden und abgeschlossenen Sicherungsaufträge StorSimple Snapshot-Manager-MMC-Snap-in verwenden."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Mit StorSimple Snapshot-Manager anzeigen und Verwalten von Sicherungsaufträgen

## <a name="overview"></a>Übersicht

**Aufträge** Knoten im **Bereich** zeigt **geplant**, **Letzte 24 Stunden**und **Ausführen** backup-Aufgaben, die Sie interaktiv oder durch eine konfigurierte Richtlinie initiiert. 

Dieses Tutorial erläutert die Verwendung Knotens **Jobs** Informationen über geplante, aktuelle und ausgeführten backup-Jobs angezeigt. (Die Liste der Aufträge und entsprechenden wird im **Ergebnisbereich** angezeigt.) Darüber hinaus können Sie aufgeführten Job rechtsklicken und ein Kontextmenü, die Listen der verfügbare Aktionen.

## <a name="view-scheduled-jobs"></a>Anzeigen von geplanten Aufträgen

Gehen Sie geplante Sicherungsaufträge anzeigen.

#### <a name="to-view-scheduled-jobs"></a>Geplante Einzelvorgänge anzeigen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Knoten Sie im **Bereichsfenster** **Aufträge** und auf **geplant**. Im **Ergebnisbereich** wird Folgendes angezeigt:

    - **Name** – der Name des geplanten snapshot

    - **Nächste Ausführung** – Datum und Uhrzeit der nächsten geplanten snapshot

    - **Letzte Ausführung** : Datum und Uhrzeit der letzten geplanten Momentaufnahme

    >[AZURE.NOTE] Für einmalige nur Snapshots wird der **Nächste** und **Letzte ausgeführt** übereinstimmen. 
 
    ![Geplante Sicherungsaufträge](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Zusätzliche Aktionen für ein bestimmtes Projekt mit der rechten Maustaste im **Ergebnisbereich** der Auftragsname und die Menüoptionen auswählen.

## <a name="view-recent-jobs"></a>Letzte Einzelvorgänge anzeigen

Gehen Sie zu Backup Wiederherstellungsaufträge, die in den letzten 24 Stunden abgeschlossen wurden.

#### <a name="to-view-recent-jobs"></a>Zuletzt verwendete Aufträge anzeigen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** Knoten Sie **Aufträge** und **Letzte 24 Stunden**auf. **Im Ergebnisbereich** werden Sicherungsaufträge für die letzten 24 Stunden (maximal 64 Aufträge). Informationen im **Ergebnisbereich** je nach **festgelegten Anzeigeoptionen** :

    - **Name** – der Name des geplanten Snapshot.
 
    - **Gestartet** – Datum und Uhrzeit der Snapshot begann.

    - **Beendet** – das Datum und die Uhrzeit der Snapshot abgeschlossen oder beendet wurde.

    - Mal **verstrichene** – die Zeitspanne zwischen **gestartet** und **beendet** .

    - **Status** – der Status des zuletzt abgeschlossenen Auftrags. **Erfolg** bedeutet, dass die Sicherung erfolgreich erstellt wurde. **Fehler** gibt an, dass der Auftrag nicht erfolgreich ausgeführt wurde.

    - **Informationen** den Grund für den Fehler.

    - **Bytes verarbeitet (MB)** – die Datenmenge in der Volume-Gruppe (in MB) verarbeitet wurde. 

    ![Aufträge, die in den letzten 24 Stunden ausgeführt](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Zusätzliche Aktionen für ein bestimmtes Projekt mit der rechten Maustaste im **Ergebnisbereich** der Auftragsname und die Menüoptionen auswählen.

    ![Löschen eines Auftrags](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Aktuell ausgeführte Aufträge anzeigen

Gehen Sie auf Jobs anzeigen, die derzeit ausgeführt werden.

#### <a name="to-view-currently-running-jobs"></a>Laufende Einzelvorgänge anzeigen

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Knoten Sie im **Bereichsfenster** **Projekte** und klicken Sie auf **Ausführen**. Je nach **festgelegten Ansichtsoptionen** im **Ergebnisbereich** die folgenden Informationen angezeigt: 

    - **Name** – der Name des geplanten Snapshot.

    - **Gestartet** – Datum und Uhrzeit der Snapshot begann.

    - **Checkpoint** -die aktuelle Aktion der Sicherung.

    - **Status** – der Vollendungsgrad.
    
    - **Vergangene** – die Zeitspanne verstrichen ist, seit die Sicherung. 

    - **Durchschnittliche Durchsatz (MB)** -Verhältnis von Gesamtanzahl Bytes Daten der Gesamtzeit für die Verarbeitung (MBs).

    - **Bytes verarbeitet (MB)** -Gesamtzahl der Bytes von Daten (MB).

    - **Bytes geschrieben (MB)** – Gesamtanzahl Bytes (in MB) geschrieben. Es enthält sowohl Daten als auch Metadaten und daher in der Regel größer als Bytes verarbeitet.

    ![Aufträgen ausgeführt](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Zusätzliche Aktionen für ein bestimmtes Projekt mit der rechten Maustaste im **Ergebnisbereich** der Auftragsname und die Menüoptionen auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).
- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager zum Verwalten des Sicherungskatalog](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

