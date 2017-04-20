<!--author=alkohli last changed: 12/15/15-->

| Limit-Bezeichner | Grenzwert | Kommentare |
|----------------- | ------|--------- |
| Maximale Anzahl von Anmeldeinformationen speichern | 64 | |
| Maximale Anzahl der volumecontainer | 64 | |
| Maximale Anzahl von volumes | 255 | |
| Maximale Anzahl von Zeitplänen pro bandbreitenvorlage | 168 | Einen Zeitplan für jede Stunde jeden Tag der Woche (24 * 7). |
| Maximale Größe eines gestuften Volumes auf Geräten | 64 TB 8100 und 8600 | 8100 und 8600 sind physische Geräte. |
| Maximale Größe eines gestuften Volumes auf virtuellen Geräten in Azure | 30 TB für 8010 <br></br> 64 TB für 8020 | 8010 und 8020 sind virtuelle Geräte in Azure Standard und Premium-Speicher verwenden. |
| Maximale Größe des lokalen Volumes auf Geräten | 9 TB für 8100 <br></br> 24 TB für 8600 | 8100 und 8600 sind physische Geräte. |
| Maximale Anzahl von iSCSI-Verbindung | 512 | |
| Maximale Anzahl von Verbindungen iSCSI-Initiatoren | 512 | |
| Maximale Anzahl der Access Control Datensätze pro Gerät | 64 | |
| Maximale Anzahl der Volumes pro backup-Richtlinie | 24 | |
| Maximale Anzahl von Sicherungen pro Sicherungsrichtlinie beibehalten | 64 | |
| Maximale Anzahl von Zeitplänen pro backup-Richtlinie | 10 | |
| Maximale Anzahl Snapshots eines beliebigen Typs, die pro Volume aufbewahrt werden | 256 | Dazu gehören lokale Snapshots und Snapshots. |
| Maximale Anzahl von Snapshots, die in jedem Gerät vorhanden sein können | 10.000 | |
| Maximale Anzahl von Volumes, die für backup und Wiederherstellung parallel verarbeitet werden können, oder Klonen | 16 |<ul><li>Sind mehr als 16 Volumes, werden sie nacheinander verarbeitet Verarbeitung Steckplätze verfügbar sind.</li><li>Neue Backups von einem geklonten oder wiederhergestellten tiered-Datenträgers kann nicht auftreten, bis der Vorgang abgeschlossen ist. Für ein lokales Volume Backups dürfen jedoch nach der Datenträger online ist.</li></ul>|
| Wiederherstellen Sie und Klonen Sie Wiederherstellungszeit für mehrstufige volumes | < 2 Minuten | <ul><li>Das Volume wird innerhalb von zwei Minuten wiederherstellen oder Clone-Vorgänge unabhängig von der Datenträgergröße zur Verfügung gestellt.</li><li>Volume-Performance zunächst möglicherweise langsamer als normal, da die meisten Daten und Metadaten in der Cloud befindet. Leistungssteigerung kann als Datenflüsse aus der Cloud StorSimple-Gerät.</li><li>Die Gesamtzeit für Metadatendownload hängt von der Volumegröße zugeordneten. Metadaten werden automatisch in das Gerät im Hintergrund mit einer Geschwindigkeit von 5 Minuten pro TB Daten zugeordneten Volumes gebracht. Diese Rate kann Internetbandbreite zur Cloud betroffen sein.</li><li>Wiederherstellung oder Klonvorgang ist abgeschlossen, wenn alle Metadaten auf dem Gerät ist.</li><li>Backup-Vorgänge können nicht ausgeführt werden, bis die Wiederherstellung oder Klon vollständig abgeschlossen ist.|
| Zeit für lokale Volumes wiederherstellen | < 2 Minuten | <ul><li>Das Volume wird innerhalb von 2 Minuten des Wiederherstellungsvorgangs unabhängig von der Datenträgergröße zur Verfügung gestellt.</li><li>Volume-Performance zunächst möglicherweise langsamer als normal, da die meisten Daten und Metadaten in der Cloud befindet. Leistungssteigerung kann als Datenflüsse aus der Cloud StorSimple-Gerät.</li><li>Die Gesamtzeit für Metadatendownload hängt von der Volumegröße zugeordneten. Metadaten werden automatisch in das Gerät im Hintergrund mit einer Geschwindigkeit von 5 Minuten pro TB Daten zugeordneten Volumes gebracht. Diese Rate kann Internetbandbreite zur Cloud betroffen sein.</li><li>Im Gegensatz zu mehrstufige Volumes für lokale Volumes Volume-Daten lokal auf dem Gerät herunterzuladen. Die Wiederherstellung ist abgeschlossen, wenn alle Daten des Datenträgers wurde auf dem Gerät.</li><li>Die Wiederherstellung möglicherweise lange und die Gesamtzeit zum Abschließen der Wiederherstellung hängt die Größe des bereitgestellten lokalen Datenträger, die Internetbandbreite und die vorhandenen Daten auf dem Gerät. Backups auf lokalen Volumes dürfen während die Wiederherstellung ausgeführt wird.|
| Thin-Restore-Verfügbarkeit | Letzte failover | |
| Maximale Client schreibgeschützt Durchsatz (wenn von SSD-Ebene bereitgestellt) * | 920/720 MB/s mit einer einzigen 10GbE Netzwerkschnittstelle | Bis zu 2 X MPIO mit zwei Netzwerkschnittstellen. |
| Maximale Client schreibgeschützt Durchsatz (wenn von der Festplatte Ebene bereitgestellt) * | 120-250 MB/s |
| Maximale Client schreibgeschützt Durchsatz (wenn von der Cloud-Ebene bereitgestellt) * | 11-41 MB/s | Lese-Durchsatz hängt Kunden generieren und die Aufrechterhaltung ausreichende e/a-Warteschlangenlänge. |

& #42; Maximaler Durchsatz pro e/a-Typ wurde mit 100 Prozent Lese- und 100 % für Schreibvorgänge gemessen. Durchsatz geringer und hängt an mischen und network Conditions.
