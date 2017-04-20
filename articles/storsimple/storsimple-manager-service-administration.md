<properties 
   pageTitle="StorSimple-Manager-Verwaltung | Microsoft Azure"
   description="Erfahren Sie, wie Ihr Gerät StorSimple mit der StorSimple Manager-Dienst im klassischen Azure-Portal verwalten."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Verwenden des StorSimple Manager-Dienstes das Gerät StorSimple verwalten

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt die StorSimple Manager-Schnittstelle wie anschließen, die verschiedenen Optionen und Links, um bestimmte Workflows, die über diese Benutzeroberfläche ausgeführt werden können. Diese Anleitung gilt für beide; StorSimple physische und virtuelle Geräte.

Nach dem Lesen dieses Artikels lernen Sie:

- StorSimple-Manager-Dienst herstellen
- StorSimple Manager-Benutzeroberfläche Navigieren
- Verwalten Sie Ihr StorSimple Gerät über StorSimple Manager-Dienst


## <a name="connect-to-storsimple-manager-service"></a>StorSimple-Manager-Dienst herstellen

StorSimple Manager-Dienst in Microsoft Azure ausgeführt wird und eine Verbindung mit mehreren StorSimple. Mithilfe einer zentralen Microsoft Azure-Verwaltungsportal in einem Browser ausgeführt dieser Geräte. Zum Herstellen des StorSimple Manager-Dienstes folgendermaßen Sie vor.

#### <a name="to-connect-to-the-service"></a>Verbindung mit dem Dienst

1. Navigieren Sie zu [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Melden Sie sich mit Ihren Microsoft-Anmeldeinformationen, bei (rechts oben im Bereich am) Microsoft Azure-Verwaltungsportal.

1. Scrollen Sie im linken Navigationsbereich auf der StorSimple Manager-Dienst.


## <a name="navigate-storsimple-manager-service-ui"></a>StorSimple-Verwaltungsdienst Benutzeroberfläche Navigieren

Die Navigationshierarchie für den StorSimple-Manager-Dienst wird in der folgenden Tabelle Benutzeroberfläche angezeigt.

- **StorSimple Manager** -Startseite Öffnet die Benutzeroberfläche Servicelevel Seiten für alle Geräte innerhalb eines Dienstes.

- **Geräte** -Seite gelangen Sie zu der Geräteebene – Benutzeroberflächenseiten für ein bestimmtes Gerät.

- **Volume-Container** Seite gelangen Sie zu der Datenträger angezeigt, alle Volumes, die einem Gerät zugeordnet enthält.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Manager Service Navigationshierarchie

|Zielseite|Service Level-Seiten|Auf Seiten|Auf Seiten|
|---|---|---|---|
|StorSimple-Manager-Dienst|Service-dashboard|Gerät-dashboard||
||Geräte →|Monitor|
||Backup-Katalog|Volume containers→|Volumes|
||Konfigurieren (Dienst)|Backup-policies||
||Aufträge|Konfigurieren (Geräte)|
||Alarme|Wartung|

![Video verfügbar](./media/storsimple-manager-service-administration/Video_icon.png) **Video verfügbar**

Ein Video, das geht über die Benutzeroberfläche der StorSimple Manager-Dienst, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Verwalten Sie StorSimple Gerät StorSimple Manager-Dienst

Die folgende Tabelle zeigt eine Zusammenfassung aller häufige Verwaltungsaufgaben und Arbeitsabläufe innerhalb der StorSimple Manager-Benutzeroberfläche ausgeführt werden können. Diese Aufgaben werden basierend auf der Benutzeroberflächenseiten organisiert auf denen sie gestartet werden.

Weitere Informationen zu den einzelnen Workflows klicken Sie auf das entsprechende Verfahren in der Tabelle.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager workflows

|Wenn Sie dies tun möchten...|Gehe zu Seite Benutzeroberfläche...|Verwenden Sie dieses Verfahren.|
|---|---|---|
|Erstellen eines Dienstes</br>Löschen eines Dienstes</br>Dienst-Registrierungsschlüssel zu erhalten</br>Dienstregistrierungsschlüssel Regenerieren|StorSimple-Manager-Dienst|[Bereitstellen eines StorSimple Manager-Dienstes](storsimple-manage-service.md)
|Ändern Sie den Verschlüsselungsschlüssel für Daten</br>Anzeigen der Protokolle|StorSimple Manager Service → Dashboard|[Verwenden Sie das StorSimple Manager Service-dashboard](storsimple-service-dashboard.md)|
|Deaktivieren eines Geräts</br>Ein Gerät löschen|StorSimple Manager → Servicegeräte|[Deaktivieren oder Löschen eines Geräts](storsimple-deactivate-and-delete-device.md)|
|Erfahren Sie mehr über Disaster Recovery und Gerät failover</br>Failover auf ein physisches Gerät</br>Failover auf ein virtuelles Gerät</br>Business Continuity-Wiederherstellung (BCDR)|StorSimple Manager → Servicegeräte|[Failover- und Disaster Recovery für das StorSimple-Gerät](storsimple-device-failover-disaster-recovery.md)|
|Liste Backups für ein volume</br>Wählen Sie einen backup-Satz</br>Löschen eines Sicherungssatzes|StorSimple Manager Service → Sicherungskatalog|[Verwalten von backups](storsimple-manage-backup-catalog.md)|
|Klonen eines Volumes|StorSimple Manager Service → Sicherungskatalog|[Klonen eines Volumes](storsimple-clone-volume.md)|
|Wiederherstellen eines Sicherungssatzes|StorSimple Manager Service → Sicherungskatalog|[Wiederherstellen eines Sicherungssatzes](storsimple-restore-from-backup-set.md)|
|Über Speicherkonten</br>Ein Speicherkonto hinzufügen</br>Ein Speicherkonto bearbeiten</br>Ein Speicherkonto löschen</br>Schlüsselrotation Speicherkonten|StorSimple Manager Service → konfigurieren|[Speicher verwalten](storsimple-manage-storage-accounts.md)|
|Bandbreite Vorlagen</br>Bandbreitenvorlage hinzufügen</br>Bearbeiten einer bandbreitenvorlage</br>Löschen einer bandbreitenvorlage</br>Bandbreite verwenden</br>Erstellen Sie eine ganztägige bandbreitenvorlage, die zu einem bestimmten Zeitpunkt beginnt|StorSimple Manager Service → konfigurieren|[Bandbreitenvorlagen verwalten](storsimple-manage-bandwidth-templates.md)|
|Access Control Datensätze</br>Erstellen eines Access-Steuerelement</br>Bearbeiten eines Datensatzes Access control</br>Löschen eines Datensatzes Access control|StorSimple Manager Service → konfigurieren|[Access Control Datensätze verwalten](storsimple-manage-acrs.md)|
|Job-Details anzeigen</br>Abbrechen eines Auftrags|StorSimple Manager → stellen|[Aufträge verwalten](storsimple-manage-jobs.md)
|Benachrichtigung erhalten</br>Alerts verwalten</br>Hinweise überprüfen|StorSimple Manager Service → Alerts|[Anzeigen und Verwalten von StorSimple Alarme](storsimple-manage-alerts.md)
|Angeschlossene Initiatoren anzeigen</br>Seriennummer des Geräts gefunden</br>Das Ziel IQN|StorSimple Manager → Servicegeräte → Dashboard|[Verwenden Sie das Dashboard StorSimple-Gerät](storsimple-device-dashboard.md)|
|Überwachung Diagramme erstellen|StorSimple Manager → Servicegeräte → überwachen|[Überwachen von StorSimple-Gerät](storsimple-monitor-device.md)|
|Volumecontainer hinzufügen</br>Volumecontainer ändern</br>Löschen eines Containers volume|StorSimple Manager Service → Geräte → Volumecontainer|[Volumecontainer verwalten](storsimple-manage-volume-containers.md)|
|Hinzufügen eines Datenträgers</br>Ein Volume bearbeiten</br>Ein Volume offline schalten</br>Löschen eines Datenträgers</br>Überwachen eines Datenträgers|StorSimple Manager Service → Geräte → Volumecontainer → Volumes|[Verwalten von Datenträgern](storsimple-manage-volumes.md)|
|Gerät ändern</br>Zeit ändern</br>DNS.md ändern</br>Konfiguration der Netzwerkschnittstellen|StorSimple Manager → Servicegeräte → konfigurieren|[Ändern der Konfiguration für das StorSimple-Gerät](storsimple-modify-device-config.md)|
|Webproxyeinstellungen anzeigen|StorSimple Manager → Servicegeräte → konfigurieren|[Webproxy für Ihr Gerät konfigurieren](storsimple-configure-web-proxy.md)|
|Gerät Administratorkennwort ändern</br>StorSimple Snapshot-Manager-Kennwort ändern|StorSimple Manager → Servicegeräte → konfigurieren|[StorSimple-Kennwörter ändern](storsimple-change-passwords.md)|
|Konfigurieren der Remoteverwaltung|StorSimple Manager → Servicegeräte → konfigurieren|[Eine Remoteverbindung mit dem Gerät StorSimple](storsimple-remote-connect.md)|
|Warnung konfigurieren|StorSimple Manager → Servicegeräte → konfigurieren|[Anzeigen und Verwalten von StorSimple Alarme](storsimple-manage-alerts.md)|
|Konfigurieren von CHAP für Ihr StorSimple-Gerät|StorSimple Manager → Servicegeräte → konfigurieren|[Konfigurieren von CHAP für Ihr StorSimple-Gerät](storsimple-configure-chap.md)|
|Hinzufügen einer backup-Richtlinie</br>Hinzufügen oder Ändern eines Zeitplanes</br>Löschen einer backup-Richtlinie</br>Nehmen Sie eine manuelle Sicherung</br>Erstellen Sie eine benutzerdefinierte Richtlinien mit mehreren Volumes und Zeitpläne|StorSimple Manager → Geräte → Sicherungsrichtlinien|[Verwalten von backup-policies](storsimple-manage-backup-policies.md)|
|Gerätecontroller beenden</br>Gerätecontroller starten</br>Gerätecontroller heruntergefahren</br>Das Gerät auf Standardwerte zurücksetzen</br>(Sind vor lokalen Gerät)|StorSimple Manager → Servicegeräte → Wartung|[StorSimple Gerätecontroller verwalten](storsimple-manage-device-controller.md)|
|Erfahren Sie mehr über StorSimple Hardware-Komponenten</br>Hardware-Status überwachen</br>(Sind vor lokalen Gerät)|StorSimple Manager → Servicegeräte → Wartung|[Monitor-Hardware-Komponenten](storsimple-monitor-hardware-status.md)|
|Erstellen Sie ein Supportpaket|StorSimple Manager → Servicegeräte → Wartung|[Erstellen und Verwalten von Support-Paket](storsimple-create-manage-support-package.md)|
|Softwareupdates installieren|StorSimple Manager → Servicegeräte → Wartung|[Aktualisieren Sie Ihr Gerät](storsimple-update-device.md)|


##<a name="next-steps"></a>Nächste Schritte
Wenn Probleme mit den täglichen Betrieb des Geräts StorSimple oder Hardwarekomponenten auftreten, lesen Sie:

- [Betriebliche Geräteproblem](storsimple-troubleshoot-operational-device.md)
- [Verwenden Sie StorSimple überwachen Anzeige-LEDs](storsimple-monitoring-indicators.md)

Sie können nicht die Probleme beheben und müssen Sie eine Serviceanfrage erstellen, finden Sie [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md).
