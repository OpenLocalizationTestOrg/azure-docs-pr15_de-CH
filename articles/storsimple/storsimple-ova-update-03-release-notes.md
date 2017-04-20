<properties 
   pageTitle="StorSimple virtuelles Array Updates Versionsinformationen | Microsoft Azure"
   description="Beschreibt wichtige offene Probleme und Lösungsmöglichkeiten für virtuelle StorSimple Array Aktualisierung 0,3."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>0,3 Versionshinweise StorSimple Virtual Array aktualisieren

## <a name="overview"></a>Übersicht

Die folgenden Versionsinformationen stellen öffnen kritisch und behobene Probleme für Microsoft Azure StorSimple Virtual Array Updates.

Die Versionsinformationen werden fortlaufend aktualisiert und kritische Probleme erfordern eine Lösung gefunden werden, werden sie hinzugefügt. Bevor Sie virtuelle StorSimple Array bereitstellen, überprüfen Sie sorgfältig die Informationen in den Release Notes.

Update 0,3 entspricht Software Version **10.0.10288.0**.

> [AZURE.NOTE] Updates sind unterbrechungsfrei und das Gerät neu starten. Wenn e/a ausgeführt wird, ist das Gerät Ausfallzeiten.


## <a name="whats-new-in-the-update-03"></a>Was ist neu im Update 0,3

Update 0,3 ist hauptsächlich ein Fehler. In dieser Version wurden verschiedene wegen fehlgeschlagener Backups in der früheren Version behoben.

## <a name="issues-fixed-in-the-update-03"></a>Probleme in Update 0,3

Die folgende Tabelle enthält eine Zusammenfassung der Probleme in dieser Version.

| Nein.  | Funktion                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Backups                                |Ein Problem trat in der früheren Version der Backups fehlschlagen würde eine Dateifreigabe abgeschlossen. Wenn dieses Problem aufgetreten ist, wurde der Sicherungsauftrag fehl und eine wichtige Warnung auf der StorSimple Manager-Dienst, den Benutzer zu benachrichtigen. Daten auf Freigaben oder Zugriff auf die Daten hierdurch nicht beeinträchtigt. Die Ursache wurde erkannt und in dieser Version behoben. <br></br> Das Update gilt rückwirkend nicht für Aktien, die bereits dieses Problem auftritt. Kunden, die dieses Problem auftritt sollte wenden Update 0,3 zunächst wenden Sie sich an Microsoft Support, um eine vollständige Sicherung des Problem. Statt sich an Microsoft Support wenden, können Kunden auch von einer fehlerfreien Sicherung für die betroffenen Aktien eine neue Freigabe wiederherstellen.                                                                                                                                                                                 |
| 2    | iSCSI                         | Ein Problem trat beim Kopieren von Daten auf einen Datenträger im virtuellen StorSimple-Array, in dem die Volumes verschwinden in der früheren Version. Dieses Problem wurde in dieser Version behoben. <br></br> Updates treten nur auf neu erstellte Volumes. Updates gelten nicht rückwirkend auf Volumes, die bereits dieses Problem auftritt. Kunden sollten betroffenen Volumes online über klassischen Azure-Portal, führen Sie eine Sicherung dieser Mengen und diesen Volumes auf neue Volumes wiederherstellen.                                                               |


## <a name="known-issues-in-the-update-03"></a>Bekannte Probleme im Update 0,3

In der folgenden Tabelle enthält eine Zusammenfassung der bekannten Probleme für StorSimple Virtual Array und enthält die Probleme aus früheren Versionen Version angegeben. 


| Nein. | Funktion | Problem | Abhilfe-Kommentare |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | Virtuelle Geräte erstellt in der Vorabversion können auf eine unterstützte Version der allgemeinen Verfügbarkeit aktualisiert werden. | Diese virtuellen Geräte müssen für die allgemeine Verfügbarkeit Version mithilfe eines Workflows Disaster Recovery (DR) ein Failover. |
| **2.** | Bereitgestellte Datenträger | Sie haben einen Datenträger einer bestimmten angegebenen Größe bereitgestellt und erstellt das entsprechende StorSimple virtuelle Gerät, Sie müssen nicht vergrößern oder verkleinern den Datenträger. Versuch, Ergebnisse zu einem Verlust aller Daten auf lokalen Ebenen des Geräts. |   |
| **3.** | Gruppenrichtlinien | Wenn ein Mitglied einer Domäne ist, kann eine Gruppenrichtlinie anwenden den Gerätevorgang beeinträchtigen. | Sicherstellen Sie, dass Ihr virtuelle Array in eine eigene Organisationseinheit (OU) für Active Directory und keine Gruppenrichtlinienobjekte (GPO) angewendet.|
| **4.** | Lokale Web-UI | Verbesserte Sicherheitsfunktionen in Internet Explorer (IE ESC) aktiviert sind, können einige lokale Benutzeroberflächenseiten Problembehandlung oder Wartung nicht ordnungsgemäß. Schaltflächen auf diesen Seiten funktioniert ebenfalls nicht. | Verbesserte Sicherheitsfeatures in Internet Explorer deaktivieren.|
| **5.** | Lokale Web-UI | In einem Hyper-V virtuelle Computer Schnittstellen Netzwerkschnittstellen im Web UI werden 10 Gbit/s. | Dies ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbit/s für virtuelle Netzwerkadapter. |
| **6.** | Mehrstufige Volumes oder Freigaben | Bytebereich für Applikationen mit StorSimple mehrstufige Volumes nicht unterstützt. Wenn Byte Bereich Sperren aktiviert ist, funktioniert StorSimple Stufen nicht. | Empfohlene Maßnahmen umfassen: <br></br>Sperren in die Anwendungslogik Bytebereich deaktivieren.<br></br>Daten für diese Anwendung lokale Volumes mehrstufige Volumes hinzufügen möchten.<br></br>*Warnung*: lokale Volumes fixiert ist Byte Bereich sperren, lokalen Volumes kann online sein, bevor die Wiederherstellung abgeschlossen ist. In solchen Fällen ist eine Wiederherstellung ausgeführt wird, müssen Sie für die Wiederherstellung abgeschlossen warten. |
| **7.** | Tiered Freigaben | Arbeiten mit großen Dateien führen langsam Ebene aus. | Beim Arbeiten mit großen Dateien wird empfohlen, die größte Datei kleiner als 3 % der Freigabe ist. |
| **8.** | Kapazität für Freigaben verwendet | Möglicherweise teilen Verbrauch wird keine Daten auf der Freigabe. Ist die verwendete Kapazität für Freigaben Metadaten enthält. |   |
| **9.** | Disaster recovery | Sie können nur die Wiederherstellung eines Dateiservers zu derselben Domäne wie das Quell-Device durchführen. Wiederherstellung mit einem Zielgerät in einer anderen Domäne wird in dieser Version nicht unterstützt. | Diese wird in einer späteren Version implementiert. |
| **10.** | Azure PowerShell | StorSimple virtuelle Geräte können über Azure PowerShell in dieser Version verwaltet werden. | Die Verwaltung der virtuellen Geräte eines klassischen Azure-Portal und der lokale Web-Benutzeroberfläche erfolgt. |
| **11.** | Kennwort ändern | Virtuelles Array Gerät Konsole akzeptiert nur Eingabe im Format der En-US-Tastatur. |   |
| **12.** | CHAP | CHAP-Anmeldeinformationen nach dem Erstellen werden nicht entfernt. Außerdem, wenn Sie die CHAP-Anmeldeinformationen ändern, müssen Sie Volumes offline, und schalten Sie sie online die Änderung wirksam wird. | Dieses Problem wird in einer späteren Version behoben. |
| **13.** | iSCSI-server  | Die 'verwendete Speicher' für einen iSCSI-Datenträger angezeigt abweichen StorSimple Manager und iSCSI Host. | ISCSI-Server hat die Dateisystem-Ansicht.<br></br>Das Gerät erkennt Blöcke zugeordnet wird, wenn die maximale Größe war.|
| **14.** | Datei-server  | Wenn eine Datei in einem Ordner eine alternative Data Streams (ADS) zugeordnet ist, wird die anzeigen nicht gesichert oder wiederhergestellt über Disaster Recovery, Clone und Wiederherstellung auf Element.| |


## <a name="next-step"></a>Nächstes

[Installieren Sie Update 0,3](storsimple-ova-install-update-01.md) auf Ihrem virtuellen StorSimple-Array.

## <a name="references"></a>Referenzen

Eine ältere Version Hinweis suchen Gehe zu: 

- [StorSimple virtuelles Array Update 0,1 bis 0,2 Release Notes](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuelle Array allgemeine Verfügbarkeit – Versionsinformationen](storsimple-ova-pp-release-notes.md)
 

