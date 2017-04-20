<properties 
   pageTitle="StorSimple virtuelles Array Updates Versionsinformationen | Microsoft Azure"
   description="Beschreibt wichtige offene Probleme und Lösungsmöglichkeiten für virtuelle StorSimple Array aktualisieren 0,2 0,1 ausgeführt."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple virtuelle Array aktualisieren 0,2 0,1-Versionsinformationen

## <a name="overview"></a>Übersicht

Die folgenden Versionsinformationen stellen öffnen kritisch und behobene Probleme für Microsoft Azure StorSimple Virtual Array Updates. (Microsoft Azure StorSimple Virtual Array wird auch als StorSimple lokalen virtuellen Gerät oder StorSimple virtuelles Gerät.) 

Die Versionsinformationen werden fortlaufend aktualisiert und kritische Probleme erfordern eine Lösung gefunden werden, werden sie hinzugefügt. Vor dem Bereitstellen des StorSimple virtuellen Geräts überprüfen Sie sorgfältig der Informationen in den Release Notes.

Update 0,2 entspricht Software Version **10.0.10280.0**. Update 0,1 ist Version **10.0.10279.0**. Im folgenden Abschnitt aufgelistet für jedes Update ändert. 

> [AZURE.NOTE] Updates sind unterbrechungsfrei und das Gerät werden neu gestartet. Wenn e/a ausgeführt wird, wird das Gerät Ausfallzeiten entstehen.

## <a name="issues-fixed-in-the-update-02"></a>Probleme in Update 0,2
Update 0,2 enthält alle Änderungen von Update 0,1 neben der in der folgenden Tabelle beschrieben:

Funktion                              | Problem                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Updates                                 | In der letzten Version waren nicht Updates automatisch in der Azure-Verwaltungsportal erkannt, so der lokalen Web-Benutzeroberfläche musste, um Updates zu installieren. Dieses Problem wurde in dieser Version behoben. Nach der Installation von Update 0,2 können Sie zukünftige Updates mit klassischen Azure-Portal installieren.                       

## <a name="whats-new-in-the-update-01"></a>Was ist neu im Update 0,1

Update 0,1 enthält folgende Fehlerbehebungen und Optimierungen. 

- **Verbesserte Stabilität für Cloud-Ausfälle**: Diese Version hat mehrere Fehlerkorrekturen Wiederherstellung, Sicherung, Wiederherstellung und tiering Störungsfall Cloud-Konnektivität. 

- **Verbesserte Leistung wiederherzustellen**: dieser Version wurde die Beendigungszeit der Wiederherstellungsaufträge erheblich senken haben Fehlerkorrekturen.

- **Automatische Optimierung der Rückgewinnung**: beim Löschen von Daten auf Thin Provisioning ungenutztem Speicher-Blöcke müssen freigegeben werden. Diese Version verbessert Speicherplatz der Rückgewinnung von der Cloud, was immer schneller im Vergleich zu früheren Versionen verfügbaren Speicherplatz.

- **Neuer virtueller Datenträgerabbilder**: neue virtuelle Festplatte, VHDX und VMDK sind jetzt über die klassischen Azure-Portal. Sie können diese Bilder neue Update 0,1 Geräte bereitstellen.

- **Die Genauigkeit der Auftragsstatus im Portal**: In früheren Versionen der Software im Portal reporting Auftragsstatus war nicht präzise. Dieses Problem wird in dieser Version behoben.

- **Domänenbeitritt auftreten**: Fehlerkorrekturen Bezug auf Domäne beitreten und Umbenennen des Geräts.


## <a name="issues-fixed-in-the-update-01"></a>Probleme in Update 0,1

Die folgende Tabelle enthält eine Zusammenfassung der Probleme in dieser Version.

| Nein.  | Funktion                              | Problem                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | In einigen Versionen VMware wurde der Betriebssystem-Datenträger als geringe Datendichte verursacht Alerts und andere normale Vorgänge unterbrechen. Dies wurde in dieser Version behoben.                                                                                                                                                                                    |
| 2    | iSCSI-server                         | In der letzten Version musste der Benutzer einen Gateway für jede aktivierte Netzwerkschnittstelle des virtuellen Geräts StorSimple angeben. Dieses Verhalten ist in dieser Version geändert, so dass der Benutzer mindestens ein Gateway für die Netzwerk-Schnittstellen konfigurieren.                                                                              |
| 3    | Support-Paket                      | In früheren Versionen der Software unterstützt Paketsammlung die Größen größer als 1 GB sind fehlgeschlagen. Dieses Problem wurde in dieser Version behoben.                                                                                                                                                                               |
| 4    | Cloud-Zugriff                         |  In der letzten Version würde StorSimple Virtual Array keine Netzwerkkonnektivität und neu gestartet wurde, die lokale Benutzeroberfläche Verbindungsprobleme haben. Dieses Problem wurde in dieser Version behoben.                                                                                                                            |
| 5    | Überwachen von Diagrammen                    | In der vorherigen Version nach einem Failover Gerät angezeigt Cloud Kapazität Auslastung Diagramme inkorrekte Werte im klassischen Azure-Portal. Dies ist in der aktuellen Version behoben.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Bekannte Probleme im Update 0,1

In der folgenden Tabelle enthält eine Zusammenfassung der bekannten Probleme für StorSimple Virtual Array und enthält die Probleme aus früheren Versionen Version angegeben. **Probleme Version in dieser Version erwähnt sind mit einem Sternchen gekennzeichnet. Fast alle Probleme in dieser Liste haben von GA-Release StorSimple Virtual Array übertragen.**


| Nein. | Funktion | Problem | Abhilfe-Kommentare |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Updates | Virtuelle Geräte erstellt in der Vorabversion können auf eine unterstützte Version der allgemeinen Verfügbarkeit aktualisiert werden. | Diese virtuellen Geräte müssen für die allgemeine Verfügbarkeit Version mithilfe eines Workflows Disaster Recovery (DR) ein Failover. |
| **2.** | Bereitgestellte Datenträger | Sie haben einen Datenträger einer bestimmten angegebenen Größe bereitgestellt und erstellt das entsprechende StorSimple virtuelle Gerät, Sie müssen nicht vergrößern oder verkleinern den Datenträger. Versuch führt zu einem Verlust aller Daten auf lokalen Ebenen des Geräts. |   |
| **3.** | Gruppenrichtlinien | Wenn ein Mitglied einer Domäne ist, kann eine Gruppenrichtlinie anwenden den Gerätevorgang beeinträchtigen. | Sicherstellen Sie, dass Ihr virtuelle Array in eine eigene Organisationseinheit (OU) für Active Directory und keine Gruppenrichtlinienobjekte (GPO) angewendet.|
| **4.** | Lokale Web-UI | Verbesserte Sicherheitsfunktionen in Internet Explorer (IE ESC) aktiviert sind, können einige lokale Benutzeroberflächenseiten Problembehandlung oder Wartung nicht ordnungsgemäß. Schaltflächen auf diesen Seiten funktioniert ebenfalls nicht. | Verbesserte Sicherheitsfeatures in Internet Explorer deaktivieren.|
| **5.** | Lokale Web-UI | In einem Hyper-V virtuelle Computer Schnittstellen Netzwerkschnittstellen im Web UI werden 10 Gbit/s. | Dies ist eine Spiegelung von Hyper-V. Hyper-V zeigt immer 10 Gbit/s für virtuelle Netzwerkadapter. |
| **6.** | Mehrstufige Volumes oder Freigaben | Bytebereich für Applikationen mit StorSimple mehrstufige Volumes nicht unterstützt. Wenn Byte Bereich Sperren aktiviert ist, wird StorSimple Stufen nicht. | Empfohlene Maßnahmen umfassen: <br></br>Sperren in die Anwendungslogik Bytebereich deaktivieren.<br></br>Daten für diese Anwendung lokale Volumes mehrstufige Volumes hinzufügen möchten.<br></br>*Warnung*: Wenn lokale Volumes fixiert und Byte Bereich Sperren aktiviert ist, werden die lokalen Volumes online sein kann, bevor die Wiederherstellung abgeschlossen ist. In solchen Fällen ist eine Wiederherstellung ausgeführt wird, müssen Sie für die Wiederherstellung abgeschlossen warten. |
| **7.** | Tiered Freigaben | Arbeiten mit großen Dateien führen langsam Ebene aus. | Beim Arbeiten mit großen Dateien wird empfohlen, die größte Datei kleiner als 3 % der Freigabe ist. |
| **8.** | Kapazität für Freigaben verwendet | Möglicherweise teilen Verbrauch keine Daten auf der Freigabe. Ist die verwendete Kapazität für Freigaben Metadaten enthält. |   |
| **9.** | Disaster recovery | Sie können nur die Wiederherstellung eines Dateiservers zu derselben Domäne wie das Quell-Device durchführen. Wiederherstellung mit einem Zielgerät in einer anderen Domäne wird in dieser Version nicht unterstützt. | Dies wird in einer späteren Version implementiert. |
| **10.** | Azure PowerShell | StorSimple virtuelle Geräte können über Azure PowerShell in dieser Version verwaltet werden. | Die Verwaltung der virtuellen Geräte eines klassischen Azure-Portal und der lokale Web-Benutzeroberfläche erfolgt. |
| **11.** | Kennwort ändern | Virtuelles Array Gerät Konsole akzeptiert nur Eingabe im Format der En-US-Tastatur. |   |
| **12.** | CHAP | CHAP-Anmeldeinformationen nach dem Erstellen werden nicht entfernt. Wenn Sie die CHAP-Anmeldeinformationen ändern, müssen Sie außerdem die Volumes offline, und schalten Sie sie online die Änderung wirksam wird. | Diese werden in einer späteren Version behoben. |
| **13.** | iSCSI-server  | Die 'verwendete Speicher' für einen iSCSI-Datenträger angezeigt abweichen StorSimple Manager und iSCSI Host. | ISCSI-Server hat die Dateisystem-Ansicht.<br></br>Das Gerät erkennt Blöcke zugeordnet wird, wenn die maximale Größe war.|
| **14.** | Datei-Server *  | Wenn eine Datei in einem Ordner eine alternative Data Streams (ADS) zugeordnet ist, wird die anzeigen nicht gesichert oder wiederhergestellt über Disaster Recovery, Clone und Wiederherstellung auf Element.| |


## <a name="next-step"></a>Nächstes

[Installieren von Updates](storsimple-ova-install-update-01.md) auf Ihrem virtuellen StorSimple-Array.
