<properties 
   pageTitle="Deaktivieren und löschen ein Geräts StorSimple | Microsoft Azure"
   description="Beschreibt das StorSimple-Gerät zunächst deaktivieren und löschen es aus dem Dienst entfernen."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Deaktivieren und Löschen von StorSimple-Gerät

## <a name="overview"></a>Übersicht

Sie möchten ein StorSimple Gerät außer Betrieb nehmen (z. B., wenn Sie ersetzen oder aktualisieren Ihr Gerät oder Sie StorSimple nicht mehr verwenden). Wenn dies der Fall ist, müssen Sie das Gerät deaktivieren, bevor Sie ihn löschen können. Deaktivieren von trennt die Verbindung zwischen dem Gerät und der entsprechenden StorSimple Manager-Dienst. In diesem Lernprogramm erläutert, wie ein StorSimple erste deaktivieren und löschen es aus dem Dienst entfernen. 

Wenn Sie ein Gerät deaktivieren, werden alle lokal auf dem Gerät gespeicherten Daten nicht mehr zugegriffen werden. Das Gerät, das in der Cloud gespeichert zugeordneten Daten können wiederhergestellt werden.  

>[AZURE.WARNING] Deaktivierung ist ein endgültiger Vorgang und kann nicht rückgängig gemacht werden. Ein deaktiviertes Gerät kann mit der StorSimple Manager-Dienst registriert werden, wenn es zunächst von der Factory zurückgesetzt wird. 
>
>Die Factory zurücksetzen Vorgang löscht alle Daten, die lokal auf Ihrem Gerät gespeichert wurde. Daher unbedingt eine Momentaufnahme Cloud Ihrer Daten, bevor Sie ein Gerät deaktivieren. Alle Daten zu einem späteren Zeitpunkt wiederherstellen können.

In diesem Lernprogramm wird erläutert, wie Sie:

- Deaktivieren eines Geräts und die Daten löschen
- Deaktivieren eines Geräts und die Daten beibehalten

Außerdem erfahren Sie, wie Deaktivierung und Löschung auf einem virtuellen StorSimple-Gerät funktioniert.

>[AZURE.NOTE] Deaktivieren einer StorSimple physikalisches oder virtuelles Gerät Vergewissern Sie zu beenden oder Löschen von Clients und Hosts, die das Gerät abhängig.

## <a name="deactivate-and-delete-data"></a>Deaktivieren und Löschen von Daten

Wenn Sie das Gerät vollständig löschen, und die Daten auf dem Gerät behalten möchten führen Sie die folgenden Schritte.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Das Gerät deaktivieren und Löschen von Daten  

1. Vor dem Deaktivieren eines Geräts müssen Sie das Volume löschen Container (und Volumes) mit dem Gerät verbunden. Sie können volumecontainer erst zugeordneten Backups gelöscht haben.

2. Deaktivieren Sie das Gerät wie folgt:

    1. Wählen Sie auf der Seite StorSimple Manager **Geräte** das Gerät zu deaktivieren, klicken am unteren Rand der Seite **Deaktivieren**.

    2. Eine Meldung wird angezeigt. Klicken Sie auf **Ja,** um fortzufahren. Deaktivieren-Prozess startet und ein paar Minuten dauern.

3. Nach der Deaktivierung können Sie das Gerät vollständig löschen. Löschen ein Gerät entfernt aus der Liste der Geräte an den Dienst. Der Dienst können nicht gelöschte Gerät. Gehen Sie folgendermaßen vor, um das Gerät zu löschen:

    1. Wählen Sie auf der Seite StorSimple Manager **Geräte** ein deaktiviertes Gerät, das Sie löschen möchten.

    2. Klicken Sie unten auf der Seite auf **Löschen**.

    3. Sie werden zur Bestätigung aufgefordert. Klicken Sie auf **Ja,** um fortzufahren.

    Es dauert ein paar Minuten das Gerät gelöscht.

## <a name="deactivate-and-retain-data"></a>Deaktivieren und Daten

Wenn Sie das Gerät löschen möchten aber Daten beibehalten möchten, führen Sie die folgenden Schritte aus.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Deaktivieren eines Geräts und die Daten beibehalten 

1. Deaktivieren Sie das Gerät. Die volumecontainer und Snapshots des Geräts bleiben.

    1. Wählen Sie auf der Seite StorSimple Manager **Geräte** das Gerät zu deaktivieren, klicken am unteren Rand der Seite **Deaktivieren**.

    2. Eine Meldung wird angezeigt. Klicken Sie auf **Ja,** um fortzufahren. Deaktivieren-Prozess startet und ein paar Minuten dauern.

2. Sie können jetzt volumecontainer und die zugeordneten Snapshots Failover. Verfahren zum Wechseln Sie [Failover- und Disaster Recovery für das StorSimple-Gerät](storsimple-device-failover-disaster-recovery.md)

3. Nach der Deaktivierung und Failover können Sie das Gerät vollständig löschen. Löschen ein Gerät entfernt aus der Liste der Geräte an den Dienst. Der Dienst können nicht gelöschte Gerät. Führen Sie die folgenden Schritte aus, um das Gerät zu löschen:
 
    1. Wählen Sie auf der Seite StorSimple Manager **Geräte** ein deaktiviertes Gerät, das Sie löschen möchten.

    2. Klicken Sie unten auf der Seite auf **Löschen**.

    3. Sie werden zur Bestätigung aufgefordert. Klicken Sie auf **Ja,** um fortzufahren.

    Es dauert ein paar Minuten das Gerät gelöscht.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deaktivieren und Löschen eines virtuellen Geräts

Ein virtuelles StorSimple freigegeben Deaktivierung der virtuellen Computer. Sie können dann den virtuellen Computer und Ressourcen erstellt, wenn sie bereitgestellt wurde. Nachdem das virtuelle Gerät deaktiviert, nicht um den vorherigen Zustand wiederhergestellt. 

Deaktivierung führt die folgenden Aktionen:

- StorSimple virtuelle Gerät wird entfernt.

- OSDisk und Datenträger für StorSimple virtuelles Gerät erstellt werden entfernt.

- Hosted Service und virtuelles Netzwerk während der Bereitstellung erstellt wurden beibehalten. Wenn Sie diese Elemente nicht verwenden, sollten Sie sie manuell löschen.

- Cloud-Snapshots von virtuellen StorSimple-Gerät erstellt werden beibehalten.

## <a name="next-steps"></a>Nächste Schritte
- Um das deaktivierte Gerät auf die werkseitigen Standardeinstellungen wiederherzustellen, gehen Sie zu [das Gerät auf die werkseitigen Standardeinstellungen zurückgesetzt](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Technische Unterstützung benötigen, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md).

- Weitere Informationen zum Verwenden des StorSimple Manager-Dienstes zum Wechseln Sie [den StorSimple Manager-Dienst zum Verwalten von StorSimple-Gerät verwenden](storsimple-manager-service-administration.md) 
