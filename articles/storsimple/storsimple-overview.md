<properties 
   pageTitle="Was ist StorSimple? | Microsoft Azure" 
   description="Beschreibt StorSimple tiering, Gerät, virtuelle Geräte, Dienste und Speicher-Management und Schlüsselwörter StorSimple führt." 
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
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple-8000-Serie: einen Hybriden Cloud Storage-Lösung

## <a name="overview"></a>Übersicht

Willkommen Sie bei Microsoft Azure StorSimple einer integrierten Lösung, die Aufgaben zwischen lokalen Geräte und Microsoft Azure-Cloud-Speicher verwaltet. StorSimple ist eine effiziente, kostengünstige und leicht zu verwaltenden Speicher (SAN)-Lösung, die viele Probleme und Kosten für Speicherung und Schutz verhindert. Es nutzt proprietäre StorSimple 8000-Serie mit Cloud-Diensten integriert und bietet eine Reihe von Verwaltungstools für eine nahtlose Ansicht alle Enterprise Storage, einschließlich Cloud-Speicher. (Die StorSimple Bereitstellungsinformationen auf der Website Microsoft Azure gilt für StorSimple-8000-Serie Geräte. Wenn Sie ein Gerät StorSimple 5000/7000-Serie verwenden, wechseln Sie zu [StorSimple Hilfe](http://onlinehelp.storsimple.com/).)

StorSimple verwendet [Speicherkategorien](#automatic-storage-tiering) , um Daten über verschiedene Medien verwalten. Die aktuellen Arbeitsseiten werden gespeicherten lokalen auf solid-State-Laufwerken (SSDs), weniger häufig verwendete, Daten auf Festplattenlaufwerken (HDDs) gespeichert, und archivierte Daten in der Cloud. Darüber hinaus verwendet StorSimple Deduplizierung und Komprimierung zu Daten Speicher. Weitere Informationen zur [Deduplizierung und Komprimierung](#deduplication-and-compression). Definitionen der anderen Schlüsselbegriffe und-Konzepte im StorSimple 8000-Serie-Dokumentation finden Sie unter [StorSimple Terminologie](#storsimple-terminology) am Ende dieses Artikels.

StorSimple Update 2 erkennen Sie entsprechenden Volumes als primäre Daten lokal auf dem Gerät bleibt und nicht in die Cloud tier *lokal fixiert* . Dadurch werden Arbeitslasten, die Cloud Latenz wie SQL und virtuellen Arbeitslasten auf lokal fixiert, während weiterhin die Cloud für die Sicherung ausführen. Weitere Informationen über lokale Volumes finden Sie unter [Verwenden der StorSimple Manager-Dienst zu managen](storsimple-manage-volumes-u2.md). 

Update 2 können Sie StorSimple virtuelle Geräte erstellen, die niedrige Latenz und hohe Leistung Azure Premium-Speicher nutzen. Weitere Informationen zu StorSimple Premium virtuellen Geräten finden Sie unter [Bereitstellen und verwalten ein StorSimple virtuelles Gerät in Azure](storsimple-virtual-device-u2.md). Weitere Informationen über Azure Premium-Speicher, [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](../storage/storage-premium-storage.md).

Neben Storage StorSimple Datenschutzfunktionen können Sie bei Bedarf erstellen und geplante Backups und dann lokal oder in der Cloud speichern. Backups werden in Form eines inkrementellen Snapshots, sodass erstellt und schnell wiederhergestellt werden kann. Cloud-Snapshots können in Notfallwiederherstellungsszenarios entscheidend, da sekundären Speicher (z. B. Backup auf Bandlaufwerken) ersetzen und Rechenzentrum oder alternative Sites gegebenenfalls wiederherstellen können.

![Symbol: Video](./media/storsimple-overview/video_icon.png) Das Video für eine kurze Einführung in Microsoft Azure StorSimple.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Warum verwenden StorSimple?

In der folgenden Tabelle werden einige der wichtigsten Vorteile Microsoft Azure StorSimple bietet beschrieben.

| Funktion | Vorteile |
|---------|---------|
|Transparente integration | Microsoft Azure StorSimple verwendet das iSCSI-Protokoll unsichtbar Speichermöglichkeiten Daten verknüpfen. Dies stellt sicher, dass Daten in der Cloud im Datencenter oder auf Remoteservern wird an einem Ort gespeichert werden.|
|Geringere Speicherkosten|Microsoft Azure StorSimple reserviert ausreichend lokalen oder Cloud-Speicher für aktuelle Anforderungen und Cloud-Speicher nur bei Bedarf erweitert. Es verringert weitere Speicherung und Kosten durch Eliminierung redundante Versionen der gleichen Daten (Deduplizierung) und Komprimierung.|
|Vereinfachtes Speichermanagement|Microsoft Azure StorSimple bietet System Verwaltungstools, mit denen Sie konfigurieren und Verwalten von Daten lokal auf einem Remoteserver und in der Cloud. Darüber hinaus können Sie Backups und Wiederherstellungsfunktionen Microsoft Management Console (MMC)-Snap-in. StorSimple bietet eine separate, optionale Schnittstelle, die mit StorSimple und Schutzdienste auf SharePoint-Servern gespeicherten Inhalt zu erweitern. |
|Verbesserte Disaster Recovery und compliance|Microsoft Azure StorSimple erfordert keine erweiterte Recovery-Zeit. Stattdessen stellt Daten benötigt wird. Dies bedeutet, dass normale Vorgänge mit minimaler Unterbrechung fortgesetzt werden können. Darüber hinaus können Sie Richtlinien Sicherungszeitpläne und datenaufbewahrung konfigurieren.
|Datentransfer|Microsoft Azure Cloud Services hochgeladenen Daten von anderen Standorten für Recovery und Migration möglich. StorSimple können Sie außerdem auf virtuellen Computern (VMs) in Microsoft Azure StorSimple virtuelle Geräte konfigurieren. VMs können virtuelle Geräte für gespeicherte Daten für Test-oder Wiederherstellung.|
|Unterstützung für andere Cloud-Dienstanbieter |StorSimple-8000-Serie mit Software update 1 oder höher unterstützt Amazon S3 Ressourceneinträge, HP und OpenStack cloud-Dienste sowie Microsoft Azure. (Sie noch ein Speicherkonto Microsoft Azure für Gerät-Verwaltung benötigen.) Weitere Informationen unter [Was ist neu im Update 1.2](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Business continuity | Update 1 oder höher enthält eine neue Migrationsfunktion, StorSimple 5000-7000 Serie können ihre Daten auf einem Gerät StorSimple 8000-Serie migrieren.|
|Verfügbarkeit in Azure Government-Portal | StorSimple-Update 1 oder höher ist in Azure Regierungsportal verfügbar. Weitere Informationen finden Sie unter [Bereitstellen der lokalen StorSimple Gerät in das Portal der Behörde](storsimple-deployment-walkthrough-gov.md).|
|Datenschutz und Verfügbarkeit | StorSimple 8000-Serie mit Update 1 oder höher unterstützt Zone redundanten Speicher (ZRS), neben lokal redundanter Speicher (LRS) und georedundanten (GRS). Finden Sie in [diesem Artikel auf Azure Redundanz Speicheroptionen](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS.|
|Support für kritische Applikationen | StorSimple Update 2 erkennen Sie entsprechenden Volumes lokal fixiert. Diese Funktion stellt sicher, dass kritische Anwendung erforderliche Daten nicht in die Cloud gestuft ist. Lokale Volumes unterliegen Cloud Wartezeiten oder Verbindungsprobleme. Weitere Informationen über lokale Volumes finden Sie unter [Verwenden der StorSimple Manager-Dienst zu managen](storsimple-manage-volumes-u2.md).|
|Geringe Latenz und hohe performance | StorSimple Update 2 können Sie virtuelle Laufwerke erstellen, die hohe Performance, niedrige Latenz Azure Premium-Speicher nutzen. Weitere Informationen zu StorSimple Premium virtuellen Geräten finden Sie unter [Bereitstellen und verwalten ein StorSimple virtuelles Gerät in Azure](storsimple-virtual-device-u2.md).|

![Symbol: Video](./media/storsimple-overview/video_icon.png) sehen Sie sich [dieses Video](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) StorSimple 8000-Serie Merkmale und Vorteile im Überblick.

## <a name="storsimple-components"></a>StorSimple Komponenten

Microsoft Azure StorSimple-Lösung umfasst folgenden Komponenten:

- **Microsoft Azure StorSimple Gerät** – ein lokal Hybrid Storage Array mit SSDs und Festplatten, automatischen Failover-Funktionen sowie redundante Controller. Der Controller verwaltet Storage tiering, platzieren derzeit verwendete (aktiven) Daten auf lokalem Speicher (in dem Gerät oder lokalen-Server), während weniger häufig verwendeten Daten in die Cloud verschieben.
- **Virtuelles Gerät StorSimple** – auch die StorSimple virtuelle Appliance sieht eine Version des StorSimple-Geräts, das repliziert die Architektur und die meisten Funktionen des physischen Hybrid-Speichergerät. StorSimple virtuelle Gerät wird auf einen einzelnen Knoten in Azure virtuellen Computer ausgeführt. Premium virtuelle Geräten Azure Premium-Speicher nutzen zu können, sind in Update 2 und höher verfügbar.
- **StorSimple-Manager-Dienst** – eine Erweiterung des klassischen Azure-Portal, das einen StorSimple oder virtuelles Gerät StorSimple über eine einzige Web-Oberfläche verwalten kann. Sie können den StorSimple Manager-Dienst erstellen Dienste verwalten, anzeigen und Verwalten von Geräten, Alarme angezeigt, Volumes verwalten anzeigen und Verwalten von backup-Policies und der Sicherungskatalog.
- **Windows PowerShell für StorSimple** -eine Befehlszeilenschnittstelle, die zum Verwalten des StorSimple verwenden können. Windows PowerShell für StorSimple hat Funktionen, die ermöglichen es Ihnen, registrieren Sie Ihr StorSimple Gerät Konfigurieren der Netzwerkschnittstelle auf Ihrem Gerät installieren bestimmter Updates, Geräteproblem Support-Sitzung auf und Ändern des Geräts. Sie können Windows PowerShell für StorSimple mit der seriellen Konsole oder mithilfe von Windows PowerShell-Remoting zugreifen.
- **Azure PowerShell StorSimple-Cmdlets** – eine Sammlung von Windows PowerShell-Cmdlets, mit denen Sie Service Level und Migration Aufgaben über die Befehlszeile zu automatisieren. Weitere Informationen zu den Azure-PowerShell-Cmdlets für StorSimple finden Sie die [Cmdlet-Verweis](https://msdn.microsoft.com/library/dn920427.aspx).
- **StorSimple Snapshot-Manager** -MMC-Snap-in, das Volume-Gruppen und den Windows Volumeschattenkopie-Dienst anwendungskonsistente Backups erstellen. Darüber hinaus können Sie StorSimple Snapshot-Manager Sicherungszeitpläne und Klon erstellen oder Volumes wiederherstellen. 
- **StorSimple Adapter für SharePoint** – ein Tool, das transparent Microsoft Azure StorSimple Speicherung und Datenschutz auf SharePoint-Serverfarmen und StorSimple Speicher angezeigt und verwaltet die SharePoint-Zentraladministration Portal erweitert.

Im folgenden Diagramm bietet einen allgemeinen Überblick über die Microsoft Azure StorSimple Architektur und Komponenten.

![StorSimple-Architektur](./media/storsimple-overview/overview-big-picture.png)

In den folgenden Abschnitten werden diese Komponenten ausführlich beschrieben und erläutern, wie Lösung ordnet Daten reserviert Speicher und Speicher-Management und Schutz von Daten erleichtert. Der letzte Abschnitt enthält Definitionen für einige wichtige Begriffe und Konzepte im Zusammenhang mit StorSimple Komponenten und deren Management.

## <a name="storsimple-device"></a>StorSimple-Gerät

Microsoft Azure StorSimple-Gerät ist, die primären Speicher und iSCSI-Zugriff auf Daten auf lokalen Hybrid Storage Array. Verwaltet die Kommunikation mit Cloud-Speicher und gewährleistet die Sicherheit und Vertraulichkeit aller Daten, die auf Microsoft Azure StorSimple-Lösung gespeichert.

StorSimple-Gerät umfasst SSDs und Festplatten Festplatten sowie Unterstützung für clustering und automatisches Failover. Es enthält einen freigegebenen Prozessor, freigegebenen Speicher und zwei gespiegelte Controller. Jeder Controller bietet folgende Funktionen:

- Verbindung mit einem Host-computer
- Bis zu sechs Netzwerkanschlüsse für das lokale Netzwerk (LAN) herstellen
- Hardware-Überwachung
- Permanenter RAM (NVRAM), der Informationen enthält, auch wenn die Stromversorgung unterbrochen wird
- Mit Clusterunterstützung Aktualisierung Softwareupdates auf Servern in einem Failovercluster verwalten, so dass Updates minimale oder keine Auswirkung auf die Verfügbarkeit
- Clusterdienst, welche Funktionen wie Back-End-Cluster eine hohe Verfügbarkeit und minimiert nachteilige Effekte, die auftreten können, wenn eine Festplatte oder SSD ausfällt oder offline geschaltet

Nur ein Controller ist aktiv. Ausfall des aktiven Controllers aktiv der zweite Controller automatisch. 

Weitere Informationen zur [StorSimple Hardwarekomponenten und Status](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>Virtuelles Gerät StorSimple

StorSimple können Sie ein virtuelles Gerät erstellen, das die Architektur und die Funktionen des physischen Hybrid-Speichergerät repliziert. StorSimple virtuelle Gerät (auch bekannt als der virtuellen StorSimple-Einheit) wird auf einen einzelnen Knoten in Azure virtuellen Computer ausgeführt. (Ein virtuelles Gerät kann nur auf einem Azure virtuellen Computer erstellt werden. Sie können nicht auf einem StorSimple-Gerät oder auf dem lokalen Server erstellen.) 

Virtuelle Gerät hat folgenden Funktionen:

- Es verhält sich wie eine physische Einheit und bieten eine Schnittstelle iSCSI mit virtuellen Computern in der Cloud. 
- Sie können eine unbegrenzte Anzahl von virtuellen Geräten in der Cloud erstellen und nach Bedarf aktivieren und deaktivieren. 
- Lokale Umgebungen in Disaster Recovery, Entwicklung und Testszenarien simulieren können, und können mit auf Abruf von Backups. 

Update 2 StorSimple virtuelle Gerät lautet in zwei Modellen erhältlich: die 8010 (ehemals Modell 1100) und 8020. 8010 Gerät hat eine maximale Kapazität von 30 TB. 8020 Gerät Azure Premium Speicher nutzt, hat maximal 64 TB. (Auf lokalen Ebenen speichert Azure Premium Speicher Daten auf SSDs während Standardspeicher Daten auf Festplatten gespeichert.) Beachten Sie, dass ein Speicherkonto Azure Premium Premium Speicher verwenden müssen. Weitere Informationen zum Premium-Speicher, [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](../storage/storage-premium-storage.md).

Weitere Informationen über StorSimple virtuelles Gerät, [Bereitstellen und verwalten ein StorSimple virtuelles Gerät in Azure](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>StorSimple-Manager-Dienst

Microsoft Azure StorSimple bietet eine webbasierte Benutzeroberfläche (der StorSimple Manager-Dienst), mit dem Rechenzentrum zentral verwalten cloud-Speicherung. Den StorSimple Manager-Dienst können Sie folgende Aufgaben ausführen:

- Konfigurieren von Systemeinstellungen für StorSimple Geräte
- Konfigurieren und Verwalten von Sicherheitsrichtlinien für StorSimple Geräte.
- Cloud- Anmeldeinformationen zu konfigurieren.
- Konfigurieren und Verwalten von Datenträgern auf einem Server.
- Konfigurieren von Volumegruppen.
- Sichern und Wiederherstellen von Daten.
- Überwachen der Leistung.
- Überprüfen der Systemeinstellungen und mögliche Probleme.

Den StorSimple Manager-Dienst können Sie alle außer den Verwaltungsaufgaben, die System wie Setup und Installation des Updates erforderlich.

Weitere Informationen finden Sie [StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell für StorSimple

Windows PowerShell für StorSimple bietet eine Befehlszeilenschnittstelle, und Verwaltung des Dienstes Microsoft Azure StorSimple einrichten und Überwachen von StorSimple-Geräten. Es ist eine Windows PowerShell-basierte Befehlszeilenschnittstelle, die dedizierte Cmdlets für die Verwaltung Ihrer Geräte StorSimple enthält. Windows PowerShell für StorSimple verfügt über Funktionen, mit denen Sie auf:

- Ein Gerät zu registrieren.
- Konfigurieren der Netzwerkschnittstelle auf einem Gerät.
- Installieren Sie bestimmte Updates.
- Geräteproblem durch Zugriff auf die Support-Sitzung.
- Ändern des Geräts.

Sie können Windows PowerShell für StorSimple zugreifen, von einer seriellen Konsole (auf einem Hostcomputer direkt an das Gerät angeschlossene) oder mithilfe von Windows PowerShell-Remoting. Beachten Sie, dass einige Windows PowerShell für StorSimple Aufgaben wie das ursprüngliche geräteregistrierung an der seriellen Konsole erfolgt. 

Weitere Informationen finden Sie [Mit Windows PowerShell für StorSimple auf das Gerät verwalten](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple-cmdlets

Azure PowerShell StorSimple-Cmdlets sind eine Sammlung von Windows PowerShell-Cmdlets, mit denen Sie Service Level und Migration Aufgaben über die Befehlszeile zu automatisieren. Weitere Informationen zu den Azure-PowerShell-Cmdlets für StorSimple finden Sie die [Cmdlet-Verweis](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot-Manager

StorSimple Snapshot-Manager ist ein Snap-in der Microsoft Management Console (MMC), mit denen Sie konsistent, erstellen Sie Sicherungskopien von lokalen Point-in-Time- und clouddaten. Das Snap-In wird auf einem Windows Server-basierten Server. StorSimple Snapshot-Manager können:

- Konfigurieren, Sichern und Löschen von Volumes.
- Konfigurieren von Volume-Gruppen sicherzustellen, die gesicherten Daten Anwendung konsistent ist.
- Verwalten Sie backup-Policies, sodass Daten nach einem festgelegten Zeitplan gesichert und an einem bestimmten Ort (lokal oder in der Cloud) gespeichert.
- Wiederherstellen Sie Volumes und einzelne Dateien.

Backups werden als Snapshots erfasst nur die Änderungen seit der letzte Snapshot aufzeichnen und benötigen weniger Speicherplatz als vollständige Backups. Sie können backup-Zeitpläne erstellen oder sofortige Backups nach Bedarf. Darüber hinaus können StorSimple Snapshot-Manager Sie Aufbewahrungsrichtlinien zu Steuerelements wie viele Snapshots gespeichert werden. Möchten Sie später wiederherstellen aus einer Sicherung StorSimple Snapshot-Manager können wählen Sie aus dem Katalog von lokalen oder Cloud-Snapshots. 

Wenn ein Notfall eintritt, oder benötigen Sie Daten aus einem anderen Grund, StorSimple Snapshot-Manager inkrementell wiederhergestellt Bedarf. Wiederherstellung der Daten erfordert nicht, dass das gesamte System wiederherstellen, ersetzen oder an einen anderen Standort verschieben Sie Herunterfahren.

Weitere Informationen finden Sie unter [nach StorSimple Snapshot-Manager ist?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple Adapter für SharePoint

Microsoft Azure StorSimple enthält StorSimple Adapter für SharePoint, eine optionale Komponente, die transparent StorSimple Speicher und den Datenschutz zu SharePoint-Serverfarmen erweitert. Der Adapter funktioniert mit Remote-Blob-Speicher (RSP)-Provider und SQL Server RSP-Feature ermöglicht das Verschieben von BLOBs zu einem Server von Microsoft Azure StorSimple System gesichert. Microsoft Azure StorSimple speichert dann die BLOB-Daten lokal oder in der Cloud, je nach Verwendung.

StorSimple Adapter für SharePoint wird über die SharePoint-Zentraladministration Portal verwaltet. Folge SharePoint-Verwaltung wird zentrale, und alle Speicher in der SharePoint-Farm.

Weitere Informationen finden Sie [StorSimple Adapter für SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Speicher-Management-Technologie
 
Dedizierte StorSimple Gerät, virtuelles Gerät und anderen Komponenten verwendet Microsoft Azure StorSimple folgenden Softwaresystemen schnellen Zugriff auf Daten und Speicherbedarf reduzieren:

- [Automatische Speicherkategorien](#automatic-storage-tiering) 
- [Thin provisioning](#thin-provisioning) 
- [Deduplizierung und Komprimierung](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatische Speicherkategorien

Microsoft Azure StorSimple ordnet automatisch Daten in logische Ebenen basierend auf aktuellen Verwendung alter und Beziehung zu anderen Daten. Daten aktivsten werden lokal gespeichert, während weniger aktive und inaktive Daten automatisch in die Cloud migriert werden. Das folgende Diagramm veranschaulicht diese Speichermethode.
 
![StorSimple Speicher-tiers](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Schnellen Zugriff speichert StorSimple sehr aktive Daten (hot) auf SSDs in der StorSimple. Speichert Daten verwendet (warm Daten) auf Festplatten im Gerät oder auf Servern im Rechenzentrum. Verschiebt inaktive Daten, backup-Daten und Daten für Archivierung und Compliance-Zwecken in der Cloud. 

>[AZURE.NOTE] Update 2 oder höher, können Sie ein Volume lokal fixiert, in diesem Fall die Daten auf dem lokalen Gerät bleibt und nicht in die Cloud Ebenen. 

StorSimple passt und ordnet Daten und Ändern der Zuweisung von Speicher als Verwendungsmuster. Beispielsweise kann einige Informationen weniger mit der Zeit aktiv. Wie immer weniger aktiv ist, wird sie von SSDs auf Festplatten und dann zur Cloud migriert. Wenn dieselben Daten wieder aktiv ist, wird sie an das Speichergerät migriert.

Die Storage tiering geschieht wie folgt:

1. Ein Systemadministrator legt Microsoft Azure Cloud Storage-Konto.
2. Der Administrator verwendet die serielle Konsole und der StorSimple Manager-Dienst (mit klassischen Azure-Portal) Gerät und Datei-Server konfigurieren Erstellen von Volumes und Datenschutzrichtlinien. Lokalen Computer (z. B. Dateiserver) verwenden StorSimple-Gerät Zugriff auf das Internet Small Computer System-Interface (iSCSI).
3. StorSimple speichert Daten zunächst auf schnelle SSD-Ebene des Geräts.
4. SSD-Ebene Kapazität erreicht, StorSimple dedupliziert ältesten Datenblöcke komprimiert und auf der Festplatte verschoben.
5. HDD-Tier Ansätze Kapazität StorSimple die ältesten Datenblöcke verschlüsselt und sichere Microsoft Azure Storage-Konto über HTTPS sendet.
6. Microsoft Azure erstellt mehrere Kopien der Daten in der Datacenter und remote Datacenter sicherstellen, dass die Daten wiederhergestellt werden, wenn ein Notfall eintritt. 
7. Wenn der Dateiserver in der Cloud gespeicherte Daten anfordert, nahtlos gibt StorSimple und speichert eine Kopie auf der SSD StorSimple-Gerät.

### <a name="thin-provisioning"></a>Thin provisioning

Thin provisioning ist ein Virtualisierungstechnologie in dem verfügbaren Speicher physischen Ressourcen angezeigt wird. Statt Reservierung ausreichend Speicherplatz, verwendet StorSimple genug Speicherplatz zum aktuellen Anforderungen thin provisioning. Flexible Art der Cloud-Speicher erleichtert dies, da StorSimple erhöhen oder verringern Cloud-Speicher ändern Anforderungen. 

>[AZURE.NOTE] Lokale Volumes sind nicht dünn bereitgestellt. Nur lokale Datenträger reservierten Speicher wird in seiner Gesamtheit bereitgestellt, wenn das Volume erstellt wird.

### <a name="deduplication-and-compression"></a>Deduplizierung und Komprimierung

Microsoft Azure StorSimple verwendet Deduplizierung und Datenkompression Speicherbedarf weiter reduzieren.

Deduplizierung verringert die gesamten Daten durch Vermeidung von Redundanz in den gespeicherten Daten. Ändern, StorSimple unveränderten Daten ignoriert und die Änderungen erfasst. Darüber hinaus verringert StorSimple gespeicherte Datenmenge identifizieren und entfernen unnötigen Informationen. 

>[AZURE.NOTE] Daten auf lokalen Volumes nicht dedupliziert oder komprimiert wird. Allerdings werden Sicherungskopien der lokale Volumes dedupliziert und komprimiert.

## <a name="storsimple-workload-summary"></a>StorSimple Arbeitslast Zusammenfassung

Tabellarisch finden Sie eine Übersicht der unterstützten StorSimple Arbeitslasten.

| Szenario                  | Arbeitslast                | Unterstützt |  Einschränkung                                  | Version              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Zusammenarbeit             | Dateifreigabe            | Ja       |                                                | Alle Versionen         |
| Zusammenarbeit             | DFS-Freigabe| Ja       |                                                | Alle Versionen         |
| Zusammenarbeit             | SharePoint              | Ja *      |Unterstützt nur lokale volumes      | Update 2 und höher   |
| Archivierung                  | Einfache Archivierung   | Ja       |                                                | Alle Versionen         |
| Virtualisierung            | Virtuelle Computer        | Ja *      |Unterstützt nur lokale volumes      | Update 2 und höher   |
| Datenbank                  | SQL                     | Ja *      |Unterstützt nur lokale volumes      | Update 2 und höher   |
| Videoüberwachung        | Videoüberwachung      | Ja *       |Unterstützt StorSimple-Gerät nur für diese Arbeitslast vorgesehen ist| Update 2 und höher   |
| Sicherung                    | Primäres Ziel-backup | Ja *      |Unterstützt StorSimple-Gerät nur für diese Arbeitslast vorgesehen ist| Update 3 oder höher |
| Sicherung                    | Sekundäres Ziel-backup | Ja *      |Unterstützt StorSimple-Gerät nur für diese Arbeitslast vorgesehen ist| Update 3 oder höher |

*Ja & #42; -Lösung Richtlinien und Einschränkungen anzuwenden.*

Die folgenden Arbeitslasten nicht StorSimple 8000-Serie Geräte unterstützt. StorSimple bereitgestellt, führt diese Arbeitslasten in einer nicht unterstützten Konfiguration.

-  Medizinische Bildgebung
-  Exchange
-  VDI
-  Oracle
-  SAP
-  Große Daten
-  Content-Verteilung
-  Starten von SCSI

Es folgt eine Liste der unterstützten StorSimple-Infrastrukturkomponenten. 

| Szenario | Arbeitslast      | Unterstützt |  Einschränkung                                 | Version      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Allgemein  | Schnellstraße | Ja       |                                               |Alle Versionen |
| Allgemein  | DataCore FC   | Ja *       |Unterstützt von DataCore SANsymphony            | Alle Versionen |
| Allgemein  | DFSR          | Ja *      |Unterstützt nur lokale volumes     | Alle Versionen |
| Allgemein  | Indizierung      | Ja *       |Mehrstufige Volumes nur Metadaten Indizierung wird unterstützt (keine Daten).<br>Vollständige Indizierung wird für lokale Volumes unterstützt.| Alle Versionen |
| Allgemein  | Anti-virus    | Ja *       |Für mehrstufige Volumes wird nur Scan beim Öffnen und schließen unterstützt.<br> Scanvorgang wird für lokale Volumes unterstützt.| Alle Versionen |

*Ja & #42; -Lösung Richtlinien und Einschränkungen anzuwenden.*

## <a name="storsimple-terminology"></a>StorSimple-Terminologie 

Vor dem Bereitstellen der Projektmappe Microsoft Azure StorSimple, empfehlen wir die folgenden Begriffe und Definitionen überprüfen.

### <a name="key-terms-and-definitions"></a>Wichtige Begriffe und Definitionen

| Begriff (Akronym oder Abkürzung) | Beschreibung |
| ------------------------------ | ---------------- |
| Access Control Datensatz (ACR)    | Ein Datensatz zugeordneten Volumes auf Ihrem Gerät Microsoft Azure StorSimple, das bestimmt, welche Hosts herstellen können. Die Bestimmung basiert auf den iSCSI-qualifizierten Namen (IQN) Hosts (enthalten in der ACR), die mit dem StorSimple-Gerät verbinden.|
| AES-256                        | Ein 256-Bit Advanced Encryption Standard (AES)-Algorithmus zum Verschlüsseln von Daten in und aus der Cloud bewegt. |
| Größe der Zuordnungseinheiten (AUS)     | Die kleinste Menge an Speicherplatz kann, zum Speichern einer Datei in Windows zugewiesen werden-Dateisystemen. Ist eine Dateigröße kein gerades Vielfaches der Clustergröße, Zwischenraum muss verwendet werden, um die Datei (bis zu nächsten Vielfachen der Clustergröße) was verloren und Fragmentierung der Festplatte. <br>Empfohlene AUS Azure StorSimple Volumes beträgt 64 KB, da es gut mit Deduplizierung Algorithmen funktioniert.|
| Automatisierte Speicher-tiering      | Automatisches Verschieben von weniger aktiven Daten von SSDs auf Festplatten und dann auf eine Stufe in der Cloud und Management aller Speicher über eine zentrale Benutzeroberfläche aktivieren.|
| Backup-Katalog | Eine Auflistung von Backups in der Regel durch den Anwendungstyp wurde verknüpft. Diese Auflistung wird auf der Sicherung des Dienstes StorSimple Manager-Benutzeroberfläche angezeigt.|
| Backup-Katalog             | Eine Datei mit einer Liste der verfügbaren aus der Sicherungsdatenbank StorSimple Snapshot-Manager gespeicherten Snapshots. |
| Backup-Richtlinie                   | Eine Auswahl an Volumes Sicherungsart und einen Zeitplan, der Backups auf einem vordefinierten Zeitplan erstellen kann.|
| große Binärobjekte (Binary Large Objects)    | Eine Auflistung von binären Daten als eine Einheit in einem Datenbank-Managementsystem. BLOBs sind in der Regel Bilder, Audio und andere multimedia Objekte zwar manchmal binärer ausführbarer Code als BLOB gespeichert wird.|
| Challenge Handshake Authentication Protocol (CHAP) | Ein Protokoll den Peer einer Verbindung basierend auf der Freigabe ein Kennwort oder geheimen Peer authentifizieren. CHAP kann unidirektional oder gegenseitige. Das Ziel authentifiziert mit unidirektionaler CHAP Initiator. Gegenseitiges CHAP erfordert Authentifizierung des Initiators durch das Ziel und der Initiator das Ziel authentifiziert. | 
| Klon                          | Eine Kopie eines Datenträgers. |
|Cloud als Ebene (CaaT)          | Cloud-Speicherung integriert als Ebene innerhalb der Speicherarchitektur aller Speicher wird zu einem Unternehmens-Speichernetzwerke.|
| Cloud-Dienstanbieter (CSP)   | Ein Anbieter von Cloud-computing Services.|
| Cloud-snapshot                 | Eine Point-in-Time-Kopie der Volume-Daten in der Cloud gespeichert. Cloudmomentaufnahme entspricht eine Momentaufnahme auf einem anderen, externen Speichersystem repliziert. Cloud-Snapshots sind besonders für Disaster Recovery-Szenarien.|
| Verschlüsselungsschlüssel für Cloud-Speicher   | Ein Kennwort oder einen Schlüssel vom Gerät StorSimple Zugriff auf die verschlüsselten Daten vom Gerät in die Cloud gesendet.|
| clusterfähige aktualisieren         | Verwalten von Softwareupdates auf Servern in einem Failovercluster, so dass Updates minimale oder keine Auswirkung auf die Verfügbarkeit des Dienstes.|
| DataPath                       | Eine Auflistung von Funktionseinheiten, die miteinander verbundenen Verarbeitung ausführen.|
| Deaktivieren                     | Eine dauerhafte Aktion, die die Verbindung zwischen dem Gerät StorSimple und zugehörigen Cloud-Dienst. Cloud-Snapshots des Geräts können nach dabei bleiben und geklont oder Disaster Recovery verwendet werden.|
| Spiegelung                 | Replikation von logischen Datenträger auf separaten Festplatten Laufwerke Sicherstellung der kontinuierlichen Verfügbarkeit in Echtzeit.|
| dynamische Spiegelung         | Replikation von logischen Volumes auf dynamischen Datenträgern.|
| Dynamische Datenträger                  | Ein Datenträger-volumeformat, Logical Disk Manager (LDM) zum Speichern und Verwalten von Daten auf mehrere physische Datenträger verwendet. Dynamische Datenträger können erweitert werden, um zusätzlichen Speicherplatz bereitzustellen.|
| Erweiterte Reihe von Festplatten (EBOD)-Gehäuse | Eine sekundäre Gehäuse des Geräts Microsoft Azure StorSimple, die zusätzliche Festplatte Festplatten für zusätzlichen Speicher enthält.|
| FAT-Bereitstellung               | Ein konventioneller Speicher-provisioning in die Speicher reserviert wird basierend auf erwarteten Bedarf und ist in der Regel über aktuelle müssen. Siehe auch *thin provisioning*.|
| Festplatte (HDD)          | Ein Laufwerk, rotierende Platten zum Speichern von Daten verwendet.|
| Hybriden Cloud-Speicher           | Eine Speicherarchitektur, die lokale und externe Ressourcen einschließlich Cloud-Speicher verwendet.|
| Internet Small Computer System Interface (iSCSI) | Ein Internetprotokoll-basierten Speicher Netzwerkstandard für Speichergeräte Daten oder Funktionen verknüpft.|
| iSCSI-initiator                 | Eine Softwarekomponente, mit dem Host-Computer unter Windows mit einem externen iSCSI-basierte Netzwerk verbinden kann.|
| iSCSI qualifizierten Namen (IQN)      | Ein eindeutiger Name, der ein iSCSI-Ziel und Initiator identifiziert.|
| iSCSI-Ziel                    | Eine Softwarekomponente, die zentrale iSCSI Datenträgersubsystemen in SANs bietet.|
| Live Archivierung                  | Eine Speichermethode in der Archivdaten zugänglich ist (nicht extern auf Band, beispielsweise gespeichert). Microsoft Azure StorSimple verwendet live Archivierung.|
|Lokales volume | ein Volume, das auf dem Gerät befindet und nicht in die Cloud tiered. |
| Lokale Snapshots                  | Eine Point-in-Time-Kopie der Volume-Daten auf dem Gerät Microsoft Azure StorSimple gespeichert.|
| Microsoft Azure StorSimple      | Eine leistungsstarke Lösung aus Datacenter-Gerät und Software, IT-Organisationen Cloud-Speicher nutzen, als wären Sie Datacenter Speicher ermöglicht. StorSimple vereinfacht Datenschutz und Datenmanagement und Kosten zu senken. Die Lösung konsolidiert primären Speicher, Archiv, Backup und Disaster Recovery (DR durch nahtlose Integration in die Cloud). Durch Kombinieren von SAN Speicher- und Cloud Daten-Management auf Unternehmensebene Plattformen ermöglichen StorSimple Geräte Geschwindigkeit, Einfachheit und Zuverlässigkeit für alle speicherbezogenen.|
| Stromversorgung und Kühlung Modul (PCM)  | Hardware-Komponenten des Geräts StorSimple aus Netzteile und Lüfter, daher der Name macht und Kühlmodul. Die primäre Einheit des Geräts hat zwei 764W Kühleinheiten während EBOD-Gehäuse zwei 580 w bei Kühleinheiten.|
| primäre Einheit               | Wichtigste Gehäuse des Geräts StorSimple, die die Anwendung Plattform Controller enthält.|
| Recovery Time Objective (RTO)   | Die maximale Zeitspanne vor einem Geschäftsprozess oder System ausgegeben werden soll wird vollständig nach einem Notfall wiederhergestellt.| 
|Serial attached SCSI (SAS)       | Ein Typ des Festplattenlaufwerks (HDD).|
| Verschlüsselungsschlüssel für Dienstdaten     | Ein Schlüssel zur neuen StorSimple-Gerät, das mit der StorSimple Manager-Dienst registriert. Konfigurationsdaten zwischen der StorSimple Manager-Dienst und das Gerät mit einem öffentlichen Schlüssel verschlüsselt und dann nur auf dem Gerät mit einem privaten Schlüssel entschlüsselt werden können. Service ermöglicht Data Service zu diesen privaten Schlüssel zur Entschlüsselung.|
| Dienst-Registrierungsschlüssel        | Ein Schlüssel können, dass das Gerät StorSimple mit der StorSimple Manager-Dienst registrieren, sodass Anzeige im klassischen Azure-Portal für weitere Maßnahmen.|
| Small Computer System Interface (SCSI) | Eine Reihe von Standards für physisch Verbinden von Computern und Daten zwischen diesen übergeben.|
| Solid-State Drive (SSD)         | Ein Datenträger, der keine beweglichen Teile enthält; z. B. ein Flashlaufwerk.|
| Konto                 | Ein Satz von Anmeldeinformationen für einen bestimmten Cloud-Dienstanbieter das Speicherkonto verknüpft.| 
| StorSimple Adapter für SharePoint| Eine Microsoft Azure StorSimple-Komponente, die transparent StorSimple Speicherung und Schutz für SharePoint-Serverfarmen erweitert.|
| StorSimple-Manager-Dienst      | Eine Erweiterung des klassischen Azure-Portal, das Sie Ihre Azure StorSimple lokal und virtuelle Geräte verwalten kann.|
| StorSimple Snapshot-Manager     | Microsoft Management Console (MMC)-Snap-in zum Verwalten von Sicherung und Wiederherstellung in Microsoft Azure StorSimple.|
| Sicherung                     | Eine Funktion, den Benutzer ein interaktives Backup eines Volumes. Es ist eine Alternative mit einer manuellen Sicherung eines Datenträgers eine automatische Sicherung über eine definierte Richtlinie.|
| Thin provisioning               | Eine Methode zur Steigerung der Effizienz, der verfügbaren Speicherplatz in Speichersystemen verwendet wird. Thin provisioning wird der Speicher durch mehrere Benutzer basierend auf der minimalen Speicherplatz jeder Benutzer jederzeit zugeteilt. Siehe auch *Fat bereitstellen*.|
| Tiering | Anordnen von Daten in logische Gruppen basierend auf aktuellen Verwendung alter und Beziehung zu anderen Daten. StorSimple ordnet automatisch die Daten in Ebenen.   |
| Volume                          | Logische Speicherbereiche Laufwerke präsentiert. StorSimple-Volumes entsprechen vom Host die iSCSI-und StorSimple-Gerät einschließlich bereitgestellte Volumes.|
 | volumecontainer                | Eine Gruppierung von Volumes und die Einstellung, die sie betreffen. Volumecontainer sind alle Volumes in Ihr Gerät StorSimple gruppiert. Volume Container gehören Speicherkonten, Verschlüsselung für Daten in die cloud mit zugeordneten Schlüssel und Bandbreite für Operationen mit der Cloud.|
| Volume-Gruppe                    | StorSimple Snapshot-Manager eine Volume-Gruppe ist eine Sammlung von Volumes konfiguriert um backup-Verarbeitung zu erleichtern.|
| Volume Shadow Copy Service (VSS)| Eine Windows Server Betriebssystemdienst, der Anwendungskonsistenz mit VSS-fähigen Anwendung koordiniert die Erstellung von inkrementellen Snapshots vereinfacht. VSS wird sichergestellt, dass die Anwendung vorübergehend inaktiv sind, wenn Snapshots erstellt werden.|
| Windows PowerShell für StorSimple | Eine Windows PowerShell-basierte Befehlszeilenschnittstelle verwendet und Ihr StorSimple-Gerät verwalten. Gleichzeitig einige grundlegende Funktionen von Windows PowerShell hat diese Schnittstelle zusätzliche dedizierte Cmdlets ein StorSimple Verwaltung ausgerichtet.|


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie [StorSimple Sicherheit](storsimple-security.md).




 

 
