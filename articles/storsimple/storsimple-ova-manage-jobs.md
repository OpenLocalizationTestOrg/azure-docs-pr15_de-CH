<properties 
   pageTitle="Anzeigen und Verwalten von StorSimple Virtual Array Aufträge | Microsoft Azure"
   description="Beschreibt StorSimple Manager Service Aufträge Seite und zum Nachverfolgen von aktuellen und aktuellen Aufträge für StorSimple Virtual Array verwenden."
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
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Verwenden des StorSimple Manager-Dienstes für virtuelle StorSimple Array Aufträge anzeigen

## <a name="overview"></a>Übersicht

Seite " **Aufträge** " bietet eine einzelne zentrale Portal zum Anzeigen und Verwalten von Aufträgen, die auf virtuellen Arrays (auch bekannt als lokale virtuelle Geräte) gestartet werden, die mit dem StorSimple-Manager-Dienst verbunden sind. Sie können Aufträge ausgeführt werden, abgeschlossen und fehlgeschlagen für mehrere virtuelle Geräte anzeigen. Ergebnisse werden im Tabellenformat angezeigt. 

![Seite "Aufträge"](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Schnell finden die Aufträge, die Sie interessanten wie Felder filtern:

- **Status** – alle Projekte ausgeführt, abgeschlossen oder Fehler suchen.
- **Und** – Aufträge können basierend auf den Datums- und Zeitbereich gefiltert werden.
- **Typ** der Einzelvorgangstyp kann alle, Sicherung, wiederherstellen, Failover Updates herunterladen und installieren.
- **Geräte** -Aufträge auf bestimmte Geräte mit dem Dienst initiiert. Die gefilterten Aufträge werden dann anhand der folgenden Attribute aufgelistet:

    - **Typ** der Einzelvorgangstyp kann alle, Sicherung, wiederherstellen, Failover Updates herunterladen und installieren.

    - **Status** – Aufträge können alle ausgeführt, abgeschlossen oder fehlgeschlagen.

    - **Einheit** – die Aufträge können ein Volume freigeben oder Gerät zugeordnet werden. 

    - **Device** -Namen des Geräts auf dem der Auftrag gestartet wurde.

    - **Gestartet auf** – die Zeit, wann der Auftrag gestartet wurde.

    - **Fortschritt** – der Prozentsatz Projektabschlusses ausgeführt. Bei abgeschlossenen Jobs sollte dies immer 100 %.

Die Liste der Aufträge wird alle 30 Sekunden aktualisiert.

## <a name="view-job-details"></a>Job-Details anzeigen

Führen Sie die folgenden Schritte zum Anzeigen der Details eines Auftrags.

#### <a name="to-view-job-details"></a>Job-Details anzeigen

1. Zeigen Sie auf der Seite **Projekte** die Sie sich interessieren, durch Ausführen einer Abfrage mit entsprechenden Filtern Einzelvorgänge an Sie können für abgeschlossene oder laufende Projekte suchen.

2. Tabellarische Liste der Projekte ein Projekt auswählen.

3. Klicken Sie am unteren Rand der Seite auf **Details**.

4. Im Dialogfeld **Details** können Sie Status, Details und Statistiken anzeigen. Die folgende Abbildung zeigt ein Beispiel für das Dialogfeld **Backup Job Details** .
 
    ![Job-Detailseite](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Wenn der virtuelle Computer in der Hypervisor angehalten bei

Wenn ein Einzelvorgang wird bei Ihrem virtuellen StorSimple-Array und das Gerät (VM Hypervisor bereitgestellt) für mehr als 15 Minuten angehalten, der Auftrag fehl. Dies ist aufgrund der StorSimple Virtual Array mit Microsoft Azure Zeit ist. Ein Beispiel für eine Wiederherstellung Auftragsfehler wird im folgenden Screenshot angezeigt.

![Auftragsfehler wiederherstellen](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Dieser Fehler gilt für Backup, Wiederherstellung, Aktualisierung und Failover-Aufträge. Der virtuelle Computer in Hyper-V bereitgestellt wird, werden der Computer schließlich Zeit mit der Hypervisor synchronisiert. Danach können Sie Ihr Projekt starten. 

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie lokale Webbenutzeroberfläche StorSimple Virtual Array verwalten](storsimple-ova-web-ui-admin.md).
