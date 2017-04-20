<properties 
   pageTitle="Verwalten der StorSimple backup-Policies | Microsoft Azure"
   description="Erläutert die Verwendung des StorSimple Manager-Dienstes erstellen und Verwalten von manuellen Backups, backup-Pläne und backup Aufbewahrung."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a>Verwenden des StorSimple Manager-Dienstes zum Verwalten von backup-policies

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert, wie mit der StorSimple Manager-Service **Backup-Policies** Seite backup-Prozesse und backup Archivierung für StorSimple-Volumes. Es beschreibt auch eine manuelle Sicherung abgeschlossen.

Seite **Backup-Policies** können backup-Policies verwalten und Planen von lokalen und cloud-Snapshots. (Backup-Policies werden backup-Zeitpläne und backup Aufbewahrung für eine Auflistung von Volumes verwendet.) Backup-Policies können Sie eine Momentaufnahme der mehrere Volumes gleichzeitig erstellen. Dies bedeutet, dass durch eine Sicherung erstellten Sicherungskopien konsistente Kopien. Diese Seite listet die backup-Policies, ihre Typen zugeordneten Volumes, wie viele Backups aufbewahrt und können diese Richtlinien aktivieren.

Die Seite **Backup-Policies** können Sie vorhandene backup-Policies durch einen oder mehrere der folgenden Felder filtern:

- **Richtlinienname** den Namen der Richtlinie zugeordnet. Die verschiedenen Richtlinien gehören:

   - Geplante Richtlinien explizit vom Benutzer erstellt wurden.
   - Automatische Richtlinien erstellt werden, wenn zum Zeitpunkt der Erstellung des Volumes Sicherung dieser Volume-Option aktiviert wurde. Diese Richtlinien werden als VolumeName_Default bezeichnet den Namen des vom Benutzer im klassischen Azure-Portal konfiguriert StorSimple-Volumes, Volume-Namen auf. Automatischen Richtlinien führen tägliche Cloud Snapshots zu 22.30 Gerät.
   - Importierten Richtlinien ursprünglich StorSimple Snapshot-Manager erstellt wurden. Sie verfügen über ein Tag, der den Host StorSimple Snapshot-Manager, dem importierten Richtlinien beschreibt aus.

- **Datenträger** -Volumes, die der Richtlinie zugeordnet. Alle Volumes einer Sicherungsrichtlinie zugeordnet werden zusammengefasst, wenn Backups erstellt werden.

- **Letzte erfolgreiche Sicherung** – Datum und Uhrzeit der letzten erfolgreichen Sicherung, die mit dieser Richtlinie.

- **Nächste Sicherung** – Datum und Uhrzeit der nächsten geplanten Sicherung, die von dieser Richtlinie eingeleitet werden.

- **Zeitpläne** – Anzahl der Sicherungsrichtlinie zugeordnet.

Sind häufig verwendete Vorgänge, die Sie über diese Seite ausführen können:

- Hinzufügen einer backup-Richtlinie 
- Hinzufügen oder Ändern eines Zeitplanes 
- Löschen einer backup-Richtlinie 
- Nehmen Sie eine manuelle Sicherung 
- Erstellen Sie eine benutzerdefinierte Richtlinien mit mehreren Volumes und Zeitpläne 

## <a name="add-a-backup-policy"></a>Hinzufügen einer backup-Richtlinie

Hinzufügen einer Sicherungsrichtlinie um Backups automatisch planen. Die folgenden Schritte im klassischen Azure-Portal eine Sicherungsrichtlinie für Ihr StorSimple-Gerät hinzufügen. Nachdem Sie hinzufügen, können Sie einen Zeitplan (siehe [Hinzufügen oder Ändern eines Zeitplanes](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Video verfügbar](./media/storsimple-manage-backup-policies/Video_icon.png) **Video verfügbar**

Ein Video, das zum Erstellen eines lokales oder cloud backup-Richtlinie, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Hinzufügen oder Ändern eines Zeitplanes

Sie können hinzufügen oder ändern ein Zeitplans mit einer vorhandenen backup-Richtlinie auf dem StorSimple-Gerät verbunden ist. Die folgenden Schritte im klassischen Azure-Portal hinzufügen oder Ändern eines Zeitplanes.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Löschen einer backup-Richtlinie

Die folgenden Schritte im klassischen Azure-Portal eine Sicherungsrichtlinie auf dem StorSimple-Gerät löschen.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Nehmen Sie eine manuelle Sicherung

Die folgenden Schritte im klassischen Azure-Portal eine Sicherung bei Bedarf (manuelle) für einen einzigen Datenträger erstellen.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Erstellen Sie eine benutzerdefinierte Richtlinien mit mehreren Volumes und Zeitpläne

Die folgenden Schritte im klassischen Azure-Portal eine benutzerdefinierte Richtlinien erstellen, die mehrere Volumes und Zeitpläne.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
