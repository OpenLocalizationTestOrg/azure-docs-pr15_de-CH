<properties
   pageTitle="Disaster Recovery und Gerät Failover für virtuelle StorSimple Array"
   description="Weitere Informationen zum Failover StorSimple Virtual Array."
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
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Disaster Recovery und Gerät Failover für virtuelle StorSimple Array


## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt das Disaster Recovery für Ihre Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple für lokale virtuelle Gerät) einschließlich detaillierte Schritte zum anderen virtuellen Geräten im Katastrophenfall Failover. Ein Failover ermöglicht das Migrieren der Daten von einem Gerät *Quelle* im Datencenter zu *einem anderen Zielgerät im selben oder einem anderen Standort befindet* . Geräte-Failover ist für das Gerät. Während eines Failovers ändert die clouddaten für das Quell-Device Besitz, das Zielgerät.

Geräte-Failover über Disaster Recovery (DR) Funktion koordiniert und auf **Geräten** initiiert. Diese Seite verknüpfen alle StorSimple Geräte mit der StorSimple-Manager-Dienst verbunden. Für jedes Gerät angezeigter Name, Status, bereitgestellt und maximale Kapazität, Typ und Modell angezeigt.

![](./media/storsimple-ova-failover-dr/image15.png)

Dieser Artikel ist nur für virtuelle StorSimple-Arrays. Failover ein Gerät 8000-Serie zur [Failover und Wiederherstellung des StorSimple Geräts](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Was ist Wiederherstellung?

In einem Szenario Disaster Recovery (DR) funktioniert das primäre Gerät. In diesem Fall können Sie Gerät mit einem anderen Gerät zugeordneten, indem das primäre Gerät als *Quelle* und einem anderen Gerät als *Ziel*clouddaten verschieben. Dieser Vorgang wird als *Failover*bezeichnet. Während eines Failovers alle Volumes oder Freigaben vom Quellgerät Eigentümerschaft ändern und an das Gerät übertragen. Keine Filterung der Daten ist zulässig.

DR wird als Wiederherstellung Gerät mit Heat Map-basierten tiering und tracking modelliert. Eine Wärmekarte wird definiert durch Zuweisen eines Werts Wärme, die Daten lesen und Schreiben von Mustern. Wärme zuordnen dann Ebenen niedrigsten Wärme Datenblöcke Cloud zunächst Beibehaltung Datenblöcke Hitze (am häufigsten verwendet) auf der lokalen Ebene. Während einer DR wird die heatmap zum Wiederherstellen und die Daten aus der Cloud aktivieren. Das Gerät Ruft alle Volumes/Freigaben in der letzten Sicherungskopie (wie intern festgelegt) und führt eine Wiederherstellung aus dieser Sicherung. Gesamte DR-Vorgang wird vom Gerät koordiniert.


## <a name="prerequisites-for-device-failover"></a>Erforderliche Komponenten für Device-failover


### <a name="prerequisites"></a>Erforderliche Komponenten

Für alle Geräte-Failover sollte folgende Voraussetzung erfüllt sein:

- Das Quell-Device muss **deaktiviert** sein.

- Das Zielgerät muss im klassischen Azure-Portal als **aktiv** angezeigt. Sie müssen ein virtuelles Zielgerät die gleiche oder eine höhere Kapazität bereitstellen. Sie sollten verwenden die lokale Web-Benutzeroberfläche konfigurieren und virtuelle Gerät erfolgreich registriert.

    > [AZURE.IMPORTANT] Führen Sie registrierte virtuelle Gerät über den Dienst konfigurieren, indem Sie auf **Gerät abgeschlossen**. Keine Konfiguration sollte über den Dienst ausgeführt werden.

- Quell- und Geräte müssen vom gleichen Typ sein. Sie können nur ein virtuelles Gerät konfiguriert als Dateiserver zu einem anderen Dateiserver Failover. Dies gilt auch für iSCSI-Server.

- Für einen Dateiserver DR empfiehlt das Zielgerät zu derselben Domäne wie der Quelle verknüpfen sind die Freigabeberechtigungen aufgelöst. Das Failover auf ein Zielgerät in der gleichen Domäne ist in dieser Version unterstützt.

### <a name="other-considerations"></a>Weitere Aspekte

- Wir empfehlen Ihnen die Volumes oder Freigaben auf dem Quell-Device.

- Geplantes Failover ist sollten Sie eine Sicherungskopie des Geräts und dann mit dem Failover Datenverluste zu minimieren. Ist dies ein ungeplantes Failovers wird die aktuellste Sicherung verwendet das Gerät wiederherstellen.

- Die verfügbaren Zielgeräte für Disaster Recovery sind Geräte, die die gleiche oder größere im Vergleich zu dem Quellgerät Kapazität. Geräte, die mit dem Dienst verbunden sind aber nicht die Kriterien der Platz verfügbar als Zielgeräte nicht.

### <a name="dr-prechecks"></a>Dr. vorüberprüfungen

Vor Beginn des DR werden vorüberprüfungen auf dem Gerät ausgeführt. Diese Tests sicherstellen, dass keine Fehler auftreten, wenn Dr. beginnt. Die vorüberprüfungen umfassen:

- Das Speicherkonto überprüfen

- Überprüfen der Konnektivität Cloud zu Azure

- Prüfen des verfügbaren Speicherplatzes auf dem Zielgerät

- Überprüfen, ob ein iSCSI-Server-Quellgerät gültige ACR Namen hat, die Datenträger IQN (nicht mehr als 220 Zeichen) und CHAP-Kennwort (12 und 16 Zeichen lang) zugeordnet

Wenn einer der oben genannten vorüberprüfungen fehlschlägt, kann nicht die DR fortgesetzt. Sie müssen diese Probleme und DR wiederholen.

Nach der erfolgreichen DR wird an die clouddaten des Quellgeräts an das Gerät übertragen. Das Quellgerät ist dann nicht mehr im Portal. Zugriff auf alle Volumes/Freigaben des Quellgeräts blockiert und das Zielgerät wird aktiv.

> [AZURE.IMPORTANT]
> 
> Wenn das Gerät nicht mehr verfügbar ist, ist der virtuelle Computer, die auf dem Host-System bereitgestellt noch Ressourcen belegt. Wenn die DR erfolgreich abgeschlossen ist, können Sie diesen virtuellen Computer aus dem Hostsystem löschen.

## <a name="fail-over-to-a-virtual-array"></a>Virtuelles Array Failover

Wir empfehlen eine andere StorSimple Virtual Array bereitgestellt, über die lokalen Web-Benutzeroberfläche konfiguriert und StorSimple Manager-Dienst vor dem Ausführen dieser Prozedur registriert haben.


> [AZURE.IMPORTANT]
> 
> - Sie dürfen nicht von einem Gerät StorSimple 8000-Serie zu einem virtuellen Gerät 1200 Failover.
> - Sie können von einem virtuellen FIPS Federal Information Processing Standard () aktiviert Gerät regierungsportal zu einem virtuellen Gerät im klassischen Azure-Portal bereitgestellt Failover. Das Gegenteil trifft ebenfalls zu.

Führen Sie die folgenden Schritte aus, um das Gerät zu einem virtuellen StorSimple Zielgerät wiederherzustellen.

1. Der Host nehmen Sie Volumes Anteile offline. Siehe Anleitung betriebssystemspezifische auf dem Host Anteile der Datenträger offline geschaltet werden. Wenn nicht bereits offline, müssen sich alle Volumes/Freigaben offline auf dem Gerät zu **Geräte > Aktien** (oder **Gerät > Volumes**). Wählen Sie eine Freigabe-Volume und **offline schalten** , am Ende der Seite auf. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**. Wiederholen Sie diesen Vorgang für alle Freigaben/Volumes auf dem Gerät.

2. Wählen Sie auf der Seite **Geräte** Quellgerät für Failover, und klicken Sie auf **Deaktivieren**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Sie werden zur Bestätigung aufgefordert. Gerät Deaktivierung ist ein permanenter Prozess kann nicht rückgängig gemacht werden. Sie werden auch daran erinnert zu den Freigaben/Volumes offline auf dem Host.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Nach der Bestätigung wird die Deaktivierung gestartet. Nachdem die Deaktivierung erfolgreich abgeschlossen ist, werden Sie benachrichtigt.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Auf der Seite **Geräte** ändert den Gerätestatus jetzt in **deaktiviert**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Wählen Sie das deaktivierte Gerät und am unteren Rand der Seite auf **Failover**.

6. Assistenten, der öffnet Failover bestätigen folgendermaßen Sie vor:

    1. Wählen Sie aus der Dropdown-Liste der verfügbaren Geräte einen **Zielgerät.** Geräte, die Kapazitäten werden in der Dropdown-Liste angezeigt.

    2. Überprüfen Sie die Details des Quellgeräts Gerätenamen, Kapazität und die Namen der Freigaben Failover werden zugeordnet.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Überprüfen Sie **ich stimme Failover wird rückgängig gemacht werden, nachdem das Failover erfolgreich abgeschlossen ist, wird das Quell-Device gelöscht**.

8. Klicken Sie auf Check ![](./media/storsimple-ova-failover-dr/image1.png).


9. Ein Auftrag Failover initiiert, und werden Sie benachrichtigt. Klicken Sie auf **Auftrag anzeigen** , um das Failover zu überwachen.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. Auf der Seite **Projekte** sehen Sie einen Failover-Auftrag für das Quell-Device erstellt. Dieser Auftrag führt Dr. vorüberprüfungen.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Nachdem DR vorüberprüfungen erfolgreich sind, erscheinen Failover Auftrag Aufträge für jede Freigabe-Volume auf Ihrem.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Nach Abschluss des Failovers zur **Geräte** -Seite.

    ein. Wählen Sie StorSimple virtuelle Gerät als Zielgerät für den Failoverprozess verwendet wurde.

    b. Wechseln Sie zur Seite **Freigaben** (oder **Volumes** Wenn iSCSI-Server). Alle Freigaben (Volumes) des alten Geräts sollten nun angezeigt werden.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Video verfügbar**

Diesem Video wird gezeigt, wie ein StorSimple lokalen virtuellen Gerät an ein anderes virtuelles Gerät ausgeführt werden kann.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Business Continuity-Wiederherstellung (BCDR)

Ein Business Continuity (BCDR) Systemausfall tritt auf, wenn das gesamte Azure-Rechenzentrum ausfällt. Dies kann der StorSimple Manager-Dienst und die zugeordneten StorSimple Geräte auswirken.

Wenn StorSimple Geräte, die vor einem Notfall registriert wurden, müssen diese Geräte StorSimple gelöscht werden. Nach dem Notfall können Sie neu erstellen und Geräte konfigurieren.

## <a name="errors-during-dr"></a>Fehler bei Dr.

**Cloud-Konnektivität Ausfälle während DR**

Cloud-Konnektivität unterbrochen, nachdem DR gestartet wurde und bevor die Gerät die Wiederherstellung abgeschlossen ist, DR fehl, und Sie werden benachrichtigt. Das Zielgerät für Disaster Recovery verwendet wird als markiert *unbrauchbar.* Für zukünftige DRs kann dasselbe Zielgerät verwendet werden.

**Keine kompatiblen Zielgeräte**

Wenn der verfügbaren Zielgeräte nicht genügend Platz vorhanden ist, sehen Sie Fehler zu gibt es keine kompatiblen Zielgeräte.

**Precheck Fehler**

Wenn eines der vorüberprüfungen nicht erfüllt ist, werden Sie precheck Fehler angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Verwalten der virtuellen StorSimple-Array mit der lokalen Webbenutzeroberfläche](storsimple-ova-web-ui-admin.md).
