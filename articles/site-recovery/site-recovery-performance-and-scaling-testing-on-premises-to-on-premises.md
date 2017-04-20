<properties
    pageTitle="Leistungstests und Skalierung führt lokale lokalen Hyper-V-Replikation mit Site Recovery | Microsoft Azure"
    description="Dieser Artikel enthält Informationen zu Leistungstests für lokal lokale Replikation mit Azure Site Recovery."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Leistung testen und Ergebnisse für lokal Lokale Hyper-V-Replikation mit Site Recovery skalieren

Sie können Microsoft Azure Site Recovery koordinieren und Verwalten der Replikation von virtuellen Computern und Servern Azure oder sekundären Datencenter. Dieser Artikel enthält die Ergebnisse der Leistungstests haben wir beim Replizieren von Hyper-V virtuelle Computer zwischen zwei Rechenzentren lokal.



## <a name="overview"></a>Übersicht

Das Ziel des Tests wurde zu Azure Site Recovery wie bei stationären Replikation durchführt. Steady State Replikation, wenn virtuelle Computer erste Replikation abgeschlossen und Delta-Synchronisierung werden. Es ist wichtig, das Messen der Leistung mit stabilen Zustand ist der Zustand in dem meisten virtuellen Computer bleiben, wenn unerwartete Ausfälle auftreten.


Die Testkonfiguration Bestand aus zwei lokalen Sites mit VMM-Server an jedem Standort. Diese Testkonfiguration ist ein Kopf Niederlassung Office-Bereitstellung mit als primärer Standort und der Zweigstelle als sekundäre oder Recovery-Standort.

### <a name="what-we-did"></a>Wir haben

Hier ist, was wir in den Test bestanden haben:

1. Virtuelle Maschinen mit VMM Vorlagen erstellt.

1. Virtuelle Maschinen und Aufzeichnung geplante Leistungsindikatoren über 12 Stunden gestartet.

1. Erstellte Wolken auf primären und Recovery VMM-Server.

1. Konfigurierte Cloud Schutz in Azure Site Recovery, einschließlich der Zuordnung von Quelle und Recovery Wolken.

1. Schutz für virtuelle Maschinen aktiviert und sie anfängliche Replikation abgeschlossen.

1. Ein paar Stunden zur Stabilisierung System gewartet.

1. Erfasst Leistungsdaten über 12 Stunden sicherstellen, dass alle virtuellen Computer in einer erwarteten Replikationsstatus für die 12 Stunden blieb.

1. Messen Sie das Delta zwischen geplanten Leistungsmetriken und Replikation Leistungsindikatoren.

## <a name="test-deployment-results"></a>Ergebnisse der Bereitstellung

### <a name="primary-server-performance"></a>Primäre Server-Leistung

- Hyper-V Replica verfolgt asynchron ändert sich in eine Protokolldatei mit mindestens Speicherplatz auf dem primären Server.

- Hyper-V Replica verwendet sich selbst Speichercache minimieren IOPS Aufwand für die Überwachung. Speichert VHDX im Speicher schreibt und leert sie in der Protokolldatei vor dem Protokoll zum Recovery-Standort gesendet wird. Ein leeren Datenträger geschieht auch, wenn die Schreibvorgänge festgelegten Grenzwert.

- Das Diagramm unten zeigt den stationären IOPS Aufwand für die Replikation. Wir sehen, dass die IOPS overhead durch Replikation um 5 % ist gering.

![Primäre Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V Replica nutzt Speicher auf dem primären Server datenträgerleistung optimieren. Wie im folgenden Diagramm dargestellt, ist Speicher Aufwand auf allen Servern im primären Cluster marginal. Aufwand dargestellt Speicher ist der Prozentsatz des Arbeitsspeichers von der Replikation im Vergleich zur gesamten installierten Speicher auf dem Hyper-V Server verwendet.

![Primäre Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V Replica hat minimale CPU-Auslastung. Wie im Diagramm dargestellt ist Replikations-Overhead im Bereich von 2 bis 3 %.

![Primäre Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Sekundäre (Wiederherstellung) Leistung

Hyper-V Replica verwendet wenig Arbeitsspeicher auf dem Wiederherstellungsserver, um die Anzahl der Speichervorgänge optimieren. Das Diagramm fasst die Speicherverwendung auf dem Wiederherstellungsserver. Aufwand dargestellt Speicher ist der Prozentsatz des Arbeitsspeichers von der Replikation im Vergleich zur gesamten installierten Speicher auf dem Hyper-V Server verwendet.

![Sekundäre Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

Der Betrag Ausgabeoperationen Recovery-Standort ist eine Funktion die Anzahl der Schreibvorgänge auf dem primären Standort. Wir betrachten die insgesamt Ausgabeoperationen Recovery-Standort im gesamten e/a-Vorgänge und Schreibvorgänge auf dem primären Standort. Das Diagramm zeigt, daß die IOPS Recovery-Standort

- Um das 1,5-fache der Schreib-IOPS an.

- 37 % gesamt IOPS am primären Standort.

![Sekundäre Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Sekundäre Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>Auswirkung der Replikation Netzwerklast

Durchschnittlich 275 MB pro Sekunde Netzwerkbandbreite wurde zwischen dem primären und Recovery (mit aktivierter Komprimierung) für eine vorhandene Bandbreite von 5 GB pro Sekunde verwendet.

![Netzwerklast Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Auswirkung der Replikation auf Leistung virtueller Computer

Ein wichtiger Aspekt ist die Auswirkung der Replikation auf Produktions-Workloads in virtuellen Maschinen ausgeführt. Wenn der primäre Standort für die Replikation ausreichend bereitgestellt wird, sollte nicht auf die Arbeitslasten beeinträchtigt sein. Hyper-V Replica leicht nachverfolgen Mechanismus wird sichergestellt, dass Arbeitslasten in virtuellen Maschinen mit stationären Replikation nicht betroffen sind. Dies wird im folgenden Diagramm dargestellt.

Dieses Diagramm zeigt IOPS ausgeführten virtuellen Computern unterschiedliche Arbeitslasten bevor und nachdem die Replikation aktiviert wurde. Sie sehen, kein Unterschied zwischen den beiden besteht.

![Replikat Effekt Ergebnisse](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

Das folgende Diagramm zeigt den Durchsatz von virtuellen Computern unterschiedliche Arbeitslasten vor und nach der Replikation aktiviert wurde. Sie können beobachten, dass die Replikation nicht erheblich beeinträchtigt wird.

![Ergebnisse Replikat Effekte](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Abschluss

Die Ergebnisse zeigen, dass Azure Site Recovery zusammen mit Hyper-V Replica mit minimalen Aufwand für große Cluster skaliert.  Azure Site Recovery bietet einfache Bereitstellung, Replikation, Management und Überwachung. Hyper-V Replica bietet die notwendige Infrastruktur zum erfolgreichen Replikation skalieren. Für eine optimale Bereitstellung planen sollten [Hyper-V Replica Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057)herunterladen.

## <a name="test-environment-details"></a>Details zur Umgebung testen

### <a name="primary-site"></a>Primärer Standort

- Der primäre Standort wurde einen Cluster mit fünf Hyper-V Server 470 virtuellen Maschinen.

- Die virtuellen Computer unterschiedliche Arbeitslasten und alle Azure Site Recovery-Schutz aktiviert.

- Ein iSCSI SAN bietet Speicher für den Clusterknoten. Modell – Hitachi HUS130.

- Jeder Clusterserver hat vier Netzwerkkarten (NICs) eine Gbps.

- Zwei Netzwerkkarten mit einem privaten iSCSI-Netzwerk verbunden sind, und zwei externe Unternehmensnetzwerk verbunden sind. Die externen Netzwerke ist für Cluster-Kommunikation reserviert.

![Primäre die Hardware](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Server|RAM|Modell|Prozessor|Anzahl der Prozessoren|NETZWERKKARTE|Software|
|---|---|---|---|---|---|---|
|Hyper-V Server Clusters: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25|128ESTLAB HOST25 verfügt über 256 MB|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz|4|Ich Gbit/s x 4|Windows Server Datacenter 2012 R2 (x64) + Hyper-V-Rolle|
|VMM-Server|2|||2|1 Gbit/s|Windows Server 2012-Datenbank R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Sekundäre (Wiederherstellung) Website

- Der sekundäre Standort hat sechs Failovercluster.

- Ein iSCSI SAN bietet Speicher für den Clusterknoten. Modell – Hitachi HUS130.

![Primäre Hardwarespezifikation](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Server|RAM|Modell|Prozessor|Anzahl der Prozessoren|NETZWERKKARTE|Software|
|---|---|---|---|---|---|---|
|Hyper-V Server Clusters: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10|96|Dell™ PowerEdge™ R720|Intel(R) Xeon(R) CPU E5-2630 0 @ 2,30 GHz|2|Ich Gbit/s x 4|Windows Server Datacenter 2012 R2 (x64) + Hyper-V-Rolle|
|ESTLAB HOST17|128|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz|4||Windows Server Datacenter 2012 R2 (x64) + Hyper-V-Rolle|
|ESTLAB HOST24|256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz|2||Windows Server Datacenter 2012 R2 (x64) + Hyper-V-Rolle|
|VMM-Server|2|||2|1 Gbit/s|Windows Server 2012-Datenbank R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>Serverarbeitslasten

- Für Testzwecke wurde Arbeitslasten Enterprise Kundenszenarien verwendet.

- [IOMeter](http://www.iometer.org) verwenden wir mit der Arbeit in der Tabelle für die Simulation zusammengefasst.

- Alle Profile von IOMeter sollen zufällige Bytes ungünstigsten simulieren Muster für Arbeitslasten schreiben schreiben.

|Arbeitslast|E/a-Größe (KB)|% Zugriff|% Lesen|Ausstehende e/a|E/a-Muster|
|---|---|---|---|---|---|
|Datei-Server|48163264|60 % 20 %5 %5 % 10 %|80 % 80 % 80 %-80 %-80 %|88888|Alle 100 % zufällig|
|SQL Server (Volume 1) SQL Server (Band 2)|864|100 % 100 %|70 %0 %|88|100 % random100 % sequenzielle|
|Exchange|32|100 %|67 %|8|100 % zufällig|
|Workstation-VDI|464|66 % 34 %|70-95 %|11|Beide 100 % zufällig|
|Webserver-Datei|4864|33 % 34 % 33 %|95 % 95 % 95 %|888|Zufällige 75|

### <a name="virtual-machine-configuration"></a>Konfiguration des virtuellen Computers

- 470 VMs auf dem primären Cluster.

- Alle virtuellen Computer mit VHDX Datenträger.

- Virtuelle Maschinen Arbeitslasten in der Tabelle zusammengefasst. Alle Waren mit VMM Vorlagen erstellt.

|Arbeitslast|# VMs|Minimaler Arbeitsspeicher (GB)|Maximale RAM (GB)|Logischen Größe (GB) pro VM|Maximale IOPS|
|---|---|---|---|---|---|
|SQL Server|51|1|4|167|10|
|Exchange Server|71|1|4|552|10|
|Datei-Server|50|1|2|552|22|
|VDI|149|.5|1|80|6|
|Webserver|149|.5|1|80|6|
|INSGESAMT|470|||96,83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure Site Recovery-Optionen

- Azure Site Recovery wurde für lokalen Schutz lokal konfiguriert

- VMM-Server verfügt über vier Wolken konfiguriert, mit Hyper-V-Cluster-Server und virtuelle Computer.

|Primäre VMM-cloud|Geschützte virtuelle Maschinen in der cloud|Replikationsrate|Zusätzliche Wiederherstellungspunkte|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 Minuten|Keine|
|PrimaryCloudRpo30s|47|30 Sekunden|Keine|
|PrimaryCloudRpo30sArp1|47|30 Sekunden|1|
|PrimaryCloudRpo5m|235|5 Min.|Keine|

### <a name="performance-metrics"></a>Performance-Kennzahlen

Die Tabelle zeigt die Leistungskennzahlen und Indikatoren, die in der Bereitstellung gemessen wurden.

|Metrik|Zähler|
|---|---|
|CPU|\Processor(_Total)\% Prozessorzeit|
|Arbeitsspeicher|\Memory\Available MB|
|IOPS|\PhysicalDisk (_Total) \Disk/s|
|VM (IOPS) Operationen/Sek.|\Hyper-V virtuelles Gerät (<VHD>) \Read pro Sekunde|
|VM schreiben (IOPS) pro Sekunde|\Hyper-V virtuelles Gerät (<VHD>) \Write Operationen/S|
|VM lesen Durchsatz|\Hyper-V virtuelles Gerät (<VHD>) \Read Bytes/s|
|VM-Schreib-Durchsatz|\Hyper-V virtuelles Gerät (<VHD>) \Write Bytes/s|


## <a name="next-steps"></a>Nächste Schritte

- [Zwischen zwei lokalen VMM Standorte einrichten](site-recovery-vmm-to-vmm.md)
