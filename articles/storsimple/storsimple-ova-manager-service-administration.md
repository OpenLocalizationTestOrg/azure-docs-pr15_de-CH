<properties 
   pageTitle="StorSimple Manager virtuelle Array Verwaltung | Microsoft Azure"
   description="Informationen Sie zum StorSimple für lokale Virtual Array mithilfe des StorSimple Manager-Dienstes im klassischen Azure-Portal verwalten."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>Verwenden des StorSimple Manager-Dienstes StorSimple Virtual Array verwalten

![Setup-Prozessablauf](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt die StorSimple Manager-Schnittstelle wie herstellen und die verschiedenen Optionen und Links zu bestimmten Workflows, die über diese Benutzeroberfläche ausgeführt werden können. 

Nach dem Lesen dieses Artikels sehen wie:

- Verbinden Sie mit der StorSimple Manager-Dienst
- StorSimple Manager-Benutzeroberfläche Navigieren
- Verwalten Sie virtuelle StorSimple Array über StorSimple Manager-Dienst

> [AZURE.NOTE] Finden Sie Verwaltungsoptionen für das Gerät StorSimple 8000-Serie mit [der StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Verbinden Sie mit der StorSimple Manager-Dienst

Der StorSimple Manager-Dienst in Microsoft Azure ausgeführt wird und mehrere StorSimple Arrays virtuellen Verbindung. Mithilfe einer zentralen Microsoft Azure-Verwaltungsportal in einem Browser ausgeführt dieser Geräte. Zum Herstellen des StorSimple Manager-Dienstes folgendermaßen Sie vor.

#### <a name="to-connect-to-the-service"></a>Verbindung mit dem Dienst

1. Gehen Sie zu [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Melden Sie sich mit Ihren Microsoft-Anmeldeinformationen, bei (rechts oben im Bereich am) Microsoft Azure-Verwaltungsportal.

3. Scrollen Sie im linken Navigationsbereich auf der StorSimple Manager-Dienst.

    ![Führen Sie einen Bildlauf zum Dienst](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Den Dienst StorSimple Manager Benutzeroberfläche Navigieren

Die Navigationshierarchie für den StorSimple-Manager-Dienst wird in der folgenden Tabelle Benutzeroberfläche angezeigt.

- **StorSimple Manager** Zielseite gelangen Sie zur Benutzeroberfläche Servicelevel Seiten für alle virtuellen Arrays innerhalb eines Dienstes.

- **Geräte** -Seite gelangen Sie zu der Geräteebene – Benutzeroberflächenseiten für ein bestimmtes virtuelles Array.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Manager Service Navigationshierarchie

|Zielseite|Service Level-Seiten|Auf Seiten|
|---|---|---|
|StorSimple-Manager-Dienst|Dashboard (Dienst)|Dashboard (Geräte)|
||Geräte →|Monitor|
||Backup-Katalog|Aktien (Dateiserver) oder </br>Volumes (iSCSI-Server)|
||Konfigurieren (Dienst)|Konfigurieren (Geräte)|
||Aufträge|Wartung|
||Alarme|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Mit der Durchführung der StorSimple Manager-Dienst

Die folgende Tabelle zeigt eine Zusammenfassung aller häufige Verwaltungsaufgaben und Arbeitsabläufe innerhalb der StorSimple Manager-Benutzeroberfläche ausgeführt werden können. Diese Aufgaben werden basierend auf der Benutzeroberflächenseiten organisiert auf denen sie gestartet werden.

Weitere Informationen zu den einzelnen Workflows klicken Sie auf das entsprechende Verfahren in der Tabelle.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager workflows

|Wenn Sie dies tun möchten...|Gehe zu Seite Benutzeroberfläche...|Gehen Sie folgendermaßen vor|
|---|---|---|
|Erstellen eines Dienstes</br>Löschen eines Dienstes</br>Den Dienst-Registrierungsschlüssel zu erhalten</br>Regenerieren Sie den Dienst-Registrierungsschlüssel|StorSimple-Manager-Dienst|[Bereitstellen des StorSimple Manager-Dienstes](storsimple-ova-manage-service.md)|
|Ändern Sie den Verschlüsselungsschlüssel für Daten</br>Das Log anzeigen|StorSimple Manager Service → Dashboard|[Verwenden Sie das StorSimple Service-dashboard](storsimple-ova-service-dashboard.md)|
|Virtuelles Array deaktivieren</br>Virtuelles Array löschen|StorSimple Manager → Servicegeräte|[Deaktivieren Sie oder löschen Sie ein virtuelles Array](storsimple-ova-deactivate-and-delete-device.md)|
|Disaster Recovery und Gerät failover</br>Failover-Komponenten</br>Failover auf ein virtuelles Gerät</br>Business Continuity-Wiederherstellung (BCDR)</br>Fehler während der Wiederherstellung|StorSimple Manager → Servicegeräte|[Disaster Recovery und Gerät Failover für virtuelle StorSimple Array](storsimple-ova-failover-dr.md)|
|Sichern von Freigaben und volumes</br>Nehmen Sie eine manuelle Sicherung</br>Ändern Sie den Sicherungszeitplan</br>Vorhandene Sicherungskopien anzeigen|StorSimple Manager Service → Sicherungskatalog|[Sichern Sie Ihre Virtual Array StorSimple](storsimple-ova-backup.md)|
|Aktien aus einem Sicherungssatz wiederherstellen</br>Volumes von einem Sicherungssatz wiederherstellen</br>Wiederherstellung auf Elementebene (nur Dateiserver)|StorSimple Manager Service → Sicherungskatalog|[Wiederherstellung von einer Sicherung Ihres virtuellen StorSimple-Arrays](storsimple-ova-restore.md)|
|Über Speicherkonten</br>Ein Speicherkonto hinzufügen</br>Ein Speicherkonto bearbeiten</br>Ein Speicherkonto löschen|StorSimple Manager Service → konfigurieren|[Speicherkonten für StorSimple Virtual Array verwalten](storsimple-ova-manage-storage-accounts.md)|
|Access Control Datensätze</br>Hinzufügen oder Ändern eines Datensatzes Access control </br>Löschen eines Datensatzes Access control|StorSimple Manager Service → konfigurieren|[Verwalten Sie Access Control-Datensätze für StorSimple Virtual Array](storsimple-ova-manage-acrs.md)|
|Job-Details anzeigen|StorSimple Manager → stellen| [StorSimple Virtual Array Aufträge verwalten](storsimple-ova-manage-jobs.md)|
|Warnung konfigurieren</br>Benachrichtigung erhalten</br>Alerts verwalten</br>Hinweise überprüfen|StorSimple Manager Service → Alerts|[Anzeigen und Verwalten von Alerts für StorSimple Virtual Array](storsimple-ova-manage-alerts.md)|
|Ändern des Administratorkennworts Gerät|StorSimple Manager → Servicegeräte → konfigurieren|[Ändern des Administratorkennworts für Virtual Array StorSimple-Gerät](storsimple-ova-change-device-admin-password.md)|
|Softwareupdates installieren|StorSimple Manager → Servicegeräte → Wartung|[Aktualisieren Sie Ihre Virtual Array](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] Sie müssen [lokale Webbenutzeroberfläche](storsimple-ova-web-ui-admin.md) für folgenden Aufgaben verwenden:
>
>- [Den Verschlüsselungsschlüssel Daten abrufen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Erstellen Sie ein Supportpaket](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Beenden und Starten von Virtual Array](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Nächste Schritte
Informationen der Web-Benutzeroberfläche und Ihre Verwendung finden Sie unter [verwenden StorSimple-Webbenutzeroberfläche StorSimple Virtual Array verwalten](storsimple-ova-web-ui-admin.md).
