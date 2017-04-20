<properties 
   pageTitle="Systemgrenzen StorSimple | Microsoft Azure"
   description="Beschreibt die Systemgrenzen und empfohlene Größen für Verbindungen und Komponenten der StorSimple 8000-Serie."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>StorSimple Systemgrenzen

## <a name="overview"></a>Übersicht

StorSimple bietet skalierbare und flexible Speicher für Ihr Rechenzentrum. Es gibt jedoch Grenzen, die Sie bedenken sollten wie Planung, Bereitstellung und Betrieb die Lösung StorSimple. In der folgenden Tabelle werden diese Grenzwerte beschrieben und einige Leitfaden, damit Sie Ihre StorSimple-Lösung optimal können.

| Limit-Bezeichner | Grenzwert | Kommentare |
|----------------- | ------|--------- |
| Maximale Anzahl von Anmeldeinformationen speichern | 64 | |
| Maximale Anzahl der volumecontainer | 64 | |
| Maximale Anzahl von volumes | 255 | |
| Maximale Anzahl von lokalen volumes | 32 | |
| Maximale Anzahl von Zeitplänen pro bandbreitenvorlage | 168 | Einen Zeitplan für jede Stunde jeden Tag der Woche (24 * 7). |
| Maximale Größe eines gestuften Volumes auf Geräten | 64 TB 8100 und 8600 | 8100 und 8600 sind physische Geräte. |
| Maximale Größe eines gestuften Volumes auf virtuellen Geräten in Azure | 30 TB für 8010 <br></br> 64 TB für 8020 | 8010 und 8020 sind virtuelle Geräte in Azure Standard und Premium-Speicher verwenden. |
| Maximale Größe des lokalen Volumes auf Geräten | 8,5 TB für 8100 <br></br> 22,5 TB für 8600 | 8100 und 8600 sind physische Geräte. |
| Maximale Anzahl von iSCSI-Verbindung | 512 | |
| Maximale Anzahl von Verbindungen iSCSI-Initiatoren | 512 | |
| Maximale Anzahl der Access Control Datensätze pro Gerät | 64 | |
| Maximale Anzahl der Volumes pro backup-Richtlinie | 24 | |
| Maximale Anzahl der Sicherung nach Zeitplan (in einer Sicherungsrichtlinie) beibehalten | 64 | |
| Maximale Anzahl von Zeitplänen pro backup-Richtlinie | 10 | |
| Maximale Anzahl Snapshots eines beliebigen Typs, die pro Volume aufbewahrt werden | 256 | Hierzu zählen auch lokale Snapshots und Snapshots. |
| Maximale Anzahl von Snapshots, die in jedem Gerät vorhanden sein können | 10.000 | |
| Maximale Anzahl von Volumes, die für backup und Wiederherstellung parallel verarbeitet werden können, oder Klonen | 16 |<ul><li>Sind mehr als 16 Volumes, werden sie sequenziell verarbeitet Verarbeitung Steckplätze verfügbar sind.</li><li>Neue Backups von einem geklonten oder wiederhergestellten tiered-Datenträgers kann nicht auftreten, bis der Vorgang abgeschlossen ist. Für ein lokales Volume Backups dürfen jedoch nach der Datenträger online ist.</li></ul>|
| Wiederherstellen Sie und Klonen Sie Wiederherstellungszeit für mehrstufige volumes | < 2 Minuten | <ul><li>Das Volume wird innerhalb von zwei Minuten wiederherstellen oder Clone-Vorgänge unabhängig von der Datenträgergröße zur Verfügung gestellt.</li><li>Volume-Performance zunächst möglicherweise langsamer als normal, da die meisten Daten und Metadaten in der Cloud befindet. Leistungssteigerung kann als Datenflüsse aus der Cloud StorSimple-Gerät.</li><li>Die Gesamtzeit für Metadatendownload hängt von der Volumegröße zugeordneten. Metadaten werden automatisch in das Gerät im Hintergrund mit einer Geschwindigkeit von 5 Minuten pro TB Daten zugeordneten Volumes gebracht. Diese Rate kann Internetbandbreite zur Cloud betroffen sein.</li><li>Wiederherstellung oder Klonvorgang ist abgeschlossen, wenn alle Metadaten auf dem Gerät ist.</li><li>Backup-Vorgänge können nicht ausgeführt werden, bis die Wiederherstellung oder Klon vollständig abgeschlossen ist.|
| Zeit für lokale Volumes wiederherstellen | < 2 Minuten | <ul><li>Das Volume wird innerhalb von 2 Minuten des Wiederherstellungsvorgangs unabhängig von der Datenträgergröße zur Verfügung gestellt.</li><li>Volume-Performance zunächst möglicherweise langsamer als normal, da die meisten Daten und Metadaten in der Cloud befindet. Leistungssteigerung kann als Datenflüsse aus der Cloud StorSimple-Gerät.</li><li>Die Gesamtzeit für Metadatendownload hängt von der Volumegröße zugeordneten. Metadaten werden automatisch in das Gerät im Hintergrund mit einer Geschwindigkeit von 5 Minuten pro TB Daten zugeordneten Volumes gebracht. Diese Rate kann Internetbandbreite zur Cloud betroffen sein.</li><li>Im Gegensatz zu tiered Volumes für lokale Volumes Volume-Daten lokal auf dem Gerät herunterzuladen. Die Wiederherstellung ist abgeschlossen, wenn alle Daten des Datenträgers wurde auf dem Gerät.</li><li>Die Wiederherstellung können long sein. Die Gesamtzeit zum Abschließen der Wiederherstellung hängt die Größe des bereitgestellten lokalen Datenträger, die Internetbandbreite und die vorhandenen Daten auf dem Gerät. Backups auf lokalen Volumes dürfen während die Wiederherstellung ausgeführt wird.|
|Die Verarbeitungsrate für Cloud-snapshots| 15 Minuten-TB | <ul><li>Minimale Zeit für die Cloud snapshot bereit für den Upload pro TB zugewiesenen Datenträger Daten Sicherung. </li><li> Insgesamt Cloud Snapshotzeit berechnet, indem Sie dieses Mal Snapshot Upload Zeit Internetbandbreite Cloud betroffen ist.
| Maximale Client schreibgeschützt Durchsatz (wenn von SSD-Ebene bereitgestellt) * | 920/720 MB/s mit einer einzigen 10 GbE-Netzwerkschnittstelle | Bis zu 2 X MPIO mit zwei Netzwerkschnittstellen. |
| Maximale Client schreibgeschützt Durchsatz (wenn von der Festplatte Ebene bereitgestellt) * | 120-250 MB/s |
| Maximale Client schreibgeschützt (wenn von der Cloud-Ebene bereitgestellt)* Update 3 und höher** | 40-60 MB/s für tiered-Volumes<br><br>60-80 MB/s für tiered Volumes mit Archivierung Option während der Erstellung des Volumes | Lese-Durchsatz hängt Kunden generieren und die Aufrechterhaltung ausreichende e/a-Warteschlangenlänge. <br><br>Geschwindigkeit hängt von der Geschwindigkeit des zugrunde liegenden Speicherkontos verwendet. | 

& #42; Maximaler Durchsatz pro e/a-Typ wurde mit 100 Prozent Lese- und 100 % für Schreibvorgänge gemessen. Durchsatz geringer und hängt an mischen und network Conditions.

& #42; & #42; Performance-Zahlen vor Update 3 können geringer sein.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die [StorSimple System Requirements](storsimple-system-requirements.md). 
