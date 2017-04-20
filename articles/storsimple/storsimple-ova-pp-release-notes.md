<properties 
   pageTitle="Versionshinweise Virtual Array StorSimple | Microsoft Azure"
   description="Beschreibt wichtige offene Probleme und Lösungsmöglichkeiten für virtuelle StorSimple-Array."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Virtuelle StorSimple Array-Versionsinformationen

## <a name="overview"></a>Übersicht

Die folgenden Versionsinformationen stellen öffnen kritisch März 2016 allgemein verfügbar (GA) Version von Microsoft Azure StorSimple Virtual Array (auch bekannt als StorSimple lokalen virtuellen Gerät oder StorSimple virtuelles Gerät). Diese Version entspricht Softwareversion 10.0.10271.0.

Die Versionsinformationen werden fortlaufend aktualisiert und kritische Probleme erfordern eine Lösung gefunden werden, werden sie hinzugefügt. Vor dem Bereitstellen des StorSimple virtuellen Geräts überprüfen Sie sorgfältig der Informationen in den Release Notes. 

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.


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
