<properties
   pageTitle="Übersicht über Virtual Array StorSimple | Microsoft Azure"
   description="Beschreibt die StorSimple Virtual Array einer integrierten Lösung, die Aufgaben zwischen lokalen virtuellen Gerät und Microsoft Azure-Cloud-Speicher verwaltet."
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

# <a name="introduction-to-the-storsimple-virtual-array"></a>Einführung in die virtuelle StorSimple Array

## <a name="overview"></a>Übersicht

Willkommen Sie bei der Microsoft Azure StorSimple Virtual Array einer integrierten Lösung, die Aufgaben zwischen virtuellen lokalen-Gerät in ein Hypervisor und Microsoft Azure-Cloud-Speicher verwaltet. Virtual Array (auch bekannt als StorSimple für lokale virtuelle Gerät) ist eine effiziente, kostengünstige und leicht zu verwaltenden Dateiserver oder iSCSI-Lösung, die viele Probleme und Kosten für Speicherung und Schutz verhindert. Das Virtual Array ist besonders geeignet für Szenarios mit Zweigstellen Office (ROBO).

Diese Übersicht konzentriert sich auf die Virtual Array. 

- Übersicht der StorSimple-8000-Serie finden Sie auf [StorSimple 8000-Serie: Hybriden Cloud-Lösung](storsimple-overview.md). 

- Informationen über das Gerät StorSimple 5000/7000-Serie werden Sie [StorSimple Online-Hilfe](http://onlinehelp.storsimple.com/).

Virtual Array unterstützt iSCSI oder Protokoll (SMB = Server Message Block). Vorhandene Infrastruktur von Hypervisor ausgeführt und bietet tiering Cloud, Cloud-Backup, schnelle Wiederherstellung, Wiederherstellung auf Elementebene und Disaster Recovery-Funktionen.

In der folgenden Tabelle werden die wichtigsten Features von Virtual Array zusammengefasst.

| Funktion | Virtual Array |
| ------- | ------------- |
|Installationen | Virtualisierungs-Infrastruktur (Hyper-V oder VMware) verwendet|
| Verfügbarkeit | Einzelne Knoten |
| Gesamtkapazität (einschließlich Cloud) |Bis zu 64 TB Nutzkapazität pro virtuelles Gerät |
| Lokale Kapazität | 390 GB 6,4 TB Nutzkapazität pro virtuelles Gerät (500 GB auf 8 TB Speicherplatz bereitstellen müssen)|
| Systemeigene Protokolle | iSCSI oder SMB |
| Recovery Time Objective (RTO) | iSCSI: unabhängig von Größe weniger als 2 Minuten |
| Recovery Point Objective (RPO) | Tägliche Backups und Backups nach Bedarf |
| Speicher-tiering | Verwendet Erhitzen Zuordnung, um zu bestimmen, welche Daten oder gestuft werden soll |
| Unterstützung | Unterstützt vom Lieferanten Virtualisierungs-Infrastruktur |
| Leistung | Hängt von der zugrunde liegenden Infrastruktur |
| Datentransfer | Dasselbe Gerät wiederherstellen Sie oder auf Recovery (Dateiserver) |
| Speicher-tiers | Lokale Hypervisor Speicher- und cloud |
| Freigabe-Größe |Ebenen: bis zu 20 TB; lokal fixiert: bis zu 2 TB |
| Volume-Größe |Ebenen: bis zu 5 TB; lokal fixiert: bis zu 500 GB |
| Snapshots | Absturzkonsistent |
| Wiederherstellung auf Elementebene | Ja; Benutzer können aus wiederherstellen. |

## <a name="why-use-storsimple"></a>Warum verwenden StorSimple?

StorSimple verbindet Benutzer und Server mit Azure-Speicher in Minuten unverändert Anwendung.

Die folgende Tabelle beschreibt einige der wichtigsten Vorteile der Virtual Array bietet.

| Funktion | Vorteile |
|---------|---------|
| Transparente integration | Virtual Array unterstützt iSCSI- oder das SMB-Protokoll. Die Verlagerung von Daten zwischen der lokalen Ebene und der Cloud ist nahtlos und für den Benutzer transparent.|
| Geringere Speicherkosten | Mit StorSimple bereitgestellt ausreichende lokalen Speicher aktuellen Anforderungen für die am häufigsten verwendeten hot Daten. Wie Speicherbedarf erhöht cloud StorSimple Ebenen kalte Daten in kostengünstigen Speicher. Die Daten dedupliziert und komprimiert, bevor Sie in die Cloud an Speicherbedarf und Kosten weiter zu reduzieren.|
| Vereinfachtes Speichermanagement | StorSimple bietet zentralisierte Verwaltung in der Cloud StorSimple Manager mehrere Geräte verwalten.| 
| Verbesserte Disaster Recovery und compliance | StorSimple ermöglicht schnelleres Disaster Recovery durch die Metadaten sofort wiederherstellen und die Daten nach Bedarf wiederherstellen. Dies bedeutet, dass normale Vorgänge mit minimaler Unterbrechung fortgesetzt werden können.|
| Datentransfer | Daten in der Cloud Ebenen von anderen Standorten für Recovery und Migration möglich. Beachten Sie, dass nur für das ursprüngliche virtuelle Array Daten wiederhergestellt werden können. Disaster Recovery-Funktionen verwenden Sie dagegen die gesamte virtuelle Array in ein anderes virtuelles Array wiederherstellen.|

## <a name="storsimple-workload-summary"></a>StorSimple Arbeitslast Zusammenfassung

Tabellarisch finden Sie eine Übersicht der unterstützten StorSimple Arbeitslasten.

| Szenario                | Arbeitslast              | Unterstützt |  Einschränkung                                  | Version              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO Zusammenarbeit      | Dateifreigabe          | Ja       | [Höchstwerte für Dateiserver](storsimple-ova-limits.md)anzeigen <br>Im Abschnitt [System Requirements unterstützte SMB-Versionen](storsimple-ova-system-requirements.md).   | Alle Versionen      |


## <a name="workflows"></a>Workflows

StorSimple Virtual Array eignet sich besonders für die folgenden Arbeitsabläufe:

- [Cloud-basierte Speicher-management](#cloud-based-storage-management)
- [Standort-backup](#location-independent-backup)
- [Datenschutz und Disaster recovery](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Cloud-basierte Speicher-management

StorSimple Manager-Dienst im klassischen Azure-Portal können Sie Daten auf mehreren Geräten und mehreren Standorten verwalten. Dies ist besonders nützlich in Szenarien mit verteilten Zweigniederlassungen. Beachten Sie, dass Sie separate Instanzen des Dienstes StorSimple Manager Arrays virtuellen und physischen StorSimple Geräte zu erstellen. 

![Cloud-basierte Speicher-management](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Standort-backup

Bieten Virtual Array Cloud Snapshots ortsunabhängig, Point-in-Time-Kopie ein Volume oder eine Freigabe aus. Cloud-Snapshots sind standardmäßig aktiviert und können nicht deaktiviert werden. Alle Volumes und Freigaben werden gleichzeitig über eine tägliche Sicherung Richtlinie sichern und zusätzliche Ad-hoc-Backups bei Bedarf übernehmen.

### <a name="data-protection-and-disaster-recovery"></a>Datenschutz und Disaster recovery

Virtual Array unterstützt die folgenden Daten Datenschutz und Disaster Recovery-Szenarien:

- **Volumes oder Freigaben wiederherstellen** – mit der Wiederherstellung als neue Workflow Volume oder Freigabe wiederherstellen. Verfolgen der gesamten Volumes oder Freigaben wiederherstellen.
- **Wiederherstellung auf Elementebene** Freigaben vereinfachten Zugriff auf neue Backups. Sie können problemlos eine einzelne Datei aus einem speziellen mit Ordners in der Cloud wiederherstellen. Diese Restore-Funktion ist benutzergesteuerten und kein Verwaltungsaufwand erforderlich.
- **Disaster Recovery** – verwenden der Failover-Funktion alle Volumes oder Freigaben für ein neues virtuelles Array. Sie erstellen neue Virtual Array bei der StorSimple Manager-Dienst registrieren, und ein Failover des ursprünglichen Arrays virtuellen. Neues virtuelles Array übernimmt dann die bereitgestellten Ressourcen. 

## <a name="virtual-array-components"></a>Virtuelles Array-Komponenten

Virtual Array umfasst die folgenden Komponenten:

- [Virtual Array](#virtual-array) -Speichergerät von Hybriden Cloud basierend auf einem virtuellen Computer in der virtualisierten Umgebung oder Hypervisor bereitgestellt.  
- [StorSimple Manager-Dienst](#storsimple-manager-service) – eine Erweiterung des klassischen Azure-Portal, das Sie können verwalten ein oder mehrere StorSimple Geräte über eine einzige Web-Oberfläche, die aus verschiedenen geografischen Standorten zugreifen können. Verwenden den StorSimple Manager-Dienst erstellen Dienste verwalten, anzeigen und Verwalten von Geräten und Alarme und Volumes, Freigaben und vorhandenen Snapshots verwalten.
- [Lokale Benutzeroberfläche](#local-web-user-interface) – eine webbasierte Benutzeroberfläche, mit der das Gerät so konfigurieren, dass es kann mit dem lokalen Netzwerk und registrieren das Gerät mit der StorSimple Manager-Dienst. 
- [Befehlszeilen-Schnittstelle](#command-line-interface) – eine Windows PowerShell-Schnittstelle, mit denen Sie virtuelle Array keine Support-Sitzung starten.
In den folgenden Abschnitten werden diese Komponenten ausführlich beschrieben und erläutern, wie Lösung ordnet Daten reserviert Speicher und Speicher-Management und Schutz von Daten erleichtert.

### <a name="virtual-array"></a>Virtual Array

Virtual Array ist eine Einzelknoten-Storage-Lösung, die primären Speicher bereitstellt, verwaltet die Kommunikation mit Cloud-Speicher und trägt dazu bei, die Sicherheit und Vertraulichkeit der Daten auf dem Gerät gespeichert.

Virtual Array steht in einem Modell verfügbar ist. Speicher-Array hat eine maximale Kapazität von 6,4 TB (mit einem zugrunde liegenden Speicherbedarf von 8 TB) und 64 TB einschließlich cloud-Speicher. 

Virtual Array weist die folgenden Features:

- Kostengünstig ist. Sie verwenden Ihre vorhandene Infrastruktur von Virtualisierung und auf Ihre vorhandenen Hypervisor Hyper-V oder VMware bereitgestellt werden kann.
- Es befindet sich im Datencenter und kann als ein iSCSI-Server oder einem Dateiserver konfiguriert. 
- Es ist in der Cloud integriert.
- Backups werden in der Cloud gespeichert erleichtern Disaster Recovery und Wiederherstellung auf Elementebene (ILR) zu vereinfachen. 
- Sie können Updates auf Virtual Array anwenden, wie sie zu einem physischen Gerät anwenden möchten.

>[AZURE.NOTE] Virtuelles Array kann nicht erweitert werden. Daher unbedingt ausreichend Speicher bereitstellen, wenn das virtuelle Gerät erstellen. 

### <a name="storsimple-manager-service"></a>StorSimple-Manager-Dienst

Microsoft Azure StorSimple bietet eine webbasierte Benutzeroberfläche (der StorSimple Manager-Dienst), mit dem Rechenzentrum zentral verwalten cloud-Speicherung. Den StorSimple Manager-Dienst können Sie folgende Aufgaben ausführen:

- Management mehrerer Virtual StorSimple-Arrays von einem Dienst. 
- Konfigurieren und Verwalten von Sicherheitsrichtlinien für StorSimple Geräte. (Verschlüsselung in der Cloud ist Microsoft Azure APIs abhängig.)
- Konfigurieren Sie Speicherung von Anmeldeinformationen und Eigenschaften.
- Konfigurieren und Verwalten von Volumes oder Freigaben.
- Sichern und Wiederherstellen von Daten auf Volumes oder Freigaben.
- Überwachen der Leistung.
- Überprüfen der Systemeinstellungen und mögliche Probleme.

Mithilfe den StorSimple Manager-Dienst tägliche Verwaltung Ihres virtuellen Arrays.

Weitere Informationen finden Sie [StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Lokale Benutzeroberfläche

Das Virtual Array enthält eine webbasierte Benutzeroberfläche, die für die einmalige Konfiguration und Registrierung mit der StorSimple Manager-Dienst verwendet wird. Sie können es Herunterfahren und neu starten des Virtual Array, Diagnosetests aktualisieren, Ändern des Administratorkennworts Gerät, Systemprotokolle und wenden Sie sich an Microsoft Support Service stellen. 

Finden Sie Informationen über die webbasierte Benutzeroberfläche mit [Web-basierten Benutzeroberfläche zum Verwalten der virtuellen StorSimple-Array](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Befehlszeilen-Schnittstelle

Enthalten Windows PowerShell-Schnittstelle können Sie eine Unterstützung mit Microsoft Support-Sitzung, damit sie Ihnen helfen zu beheben Probleme, die auf dem virtuellen Gerät auftreten können.

## <a name="storage-management-technologies"></a>Speicher-Management-Technologie

Virtual Array und andere Komponenten verwendet StorSimple Lösung die folgenden Software-Technologie und bieten schnellen Zugriff auf wichtige Daten, Speicher reduzieren, Daten auf das Virtual Array:

- [Automatische Speicherkategorien](#automatic-storage-tiering) 
- [Lokalen Freigaben und volumes](#locally-pinned-shares-and-volumes)
- [Deduplizierung und Komprimierung von Daten Ebenen oder in der cloud](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Geplante und on-Demand-backups](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automatische Speicherkategorien

Virtual Array verwendet einen neuen tiering Mechanismus Virtual Array und der Cloud gespeicherte Daten verwalten. Es gibt nur zwei Ebenen: lokale Virtual Array und Azure cloud-Speicherung. StorSimple Virtual Array ordnet Daten automatisch in die Ebenen anhand einer Wärmekarte die aktuelle Nutzung, Alter und Vertrauensstellungen mit anderen Daten verfolgt. Daten, die aktivsten (heißesten), lokal ist zwar weniger aktive und inaktive Daten automatisch in die Cloud migriert werden. (Alle Backups werden in der Cloud gespeichert.) StorSimple passt und ordnet Daten und Ändern der Zuweisung von Speicher als Verwendungsmuster. Beispielsweise kann einige Informationen weniger mit der Zeit aktiv. Wie immer weniger aktiv ist, ist es in der Cloud, gestuft. Wenn dieselben Daten wieder aktiv ist, wird es zum Speicher-Array in Ebenen.

Daten für einen bestimmten tiered Freigabe oder das Volume ist eigene lokale Tier Speicherplatz garantiert. (ca. 10 % des Speicherplatzes bereitgestellte für diese Freigabe oder das Volume). Obwohl dadurch den Speicherplatz auf dem virtuellen Gerät für diese Freigabe oder das Volume wird sichergestellt, dass tiering für eine Freigabe oder das Volume nicht tiering muss von anderen Freigaben oder Volumes auswirkt. Daher kann sehr regen Arbeitslast für eine Freigabe oder ein Volume nicht alle Arbeitslasten in die Cloud erzwingen. 

![Automatische Speicherkategorien](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Sie können ein Volume lokal fixiert, in diesem Fall die Daten Virtual Array bleibt und niemals zur Cloud tiered. Weitere Informationen finden Sie [lokal Freigaben und Volumes](#locally-pinned-shares-and-volumes)fixiert.

### <a name="locally-pinned-shares-and-volumes"></a>Lokalen Freigaben und volumes

Sie können die entsprechenden Freigaben und Volumes lokal fixiert erstellen. Diese Funktion stellt sicher, dass kritische Applikationen benötigt Virtual Array bleibt und niemals zur Cloud tiered. Lokalen Freigaben und Volumes weisen die folgenden Merkmale auf: 

- Sie unterliegen Cloud Wartezeiten oder Verbindungsprobleme.
- Sie profitieren dennoch StorSimple Cloud Backup und Disaster Recovery-Funktionen.

Sie können einer lokalen Freigabe oder Volume als Ebenen oder eine mehrstufige Freigabe wiederherstellen oder Volume lokal fixiert. 

Weitere Informationen über lokale Volumes finden Sie [StorSimple Manager-Dienst zu managen](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Deduplizierung und Komprimierung von Daten Ebenen oder in der cloud

StorSimple mithilfe von Deduplizierung und Datenkompression Speicherbedarf in der Cloud weiter verringert. Deduplizierung verringert die gesamten Daten durch Vermeidung von Redundanz in den gespeicherten Daten. Ändern, StorSimple unveränderten Daten ignoriert und die Änderungen erfasst. Darüber hinaus verringert StorSimple gespeicherte Datenmenge identifizieren und entfernen doppelten Informationen. 

>[AZURE.NOTE] Virtuelles Array gespeicherte Daten nicht dedupliziert oder komprimiert wird. Alle Deduplizierung und Komprimierung tritt ein, bevor die Daten in der Cloud gesendet werden.

### <a name="scheduled-and-on-demand-backups"></a>Geplante und on-Demand-backups

StorSimple-Datenschutzfunktionen können Sie on-Demand-Backups erstellen. Ein Standardzeitplan backup wird außerdem sichergestellt, dass Daten täglich gesichert. Backups werden in Form eines inkrementellen Snapshots, die in der Cloud gespeichert werden. Snapshots, die aufzeichnen die Änderungen seit der letzten Sicherung können erstellt und schnell wiederhergestellt werden. Diese Snapshots können Disaster Recovery-Szenarien sehr wichtig, da sie sekundären Speicher (z. B. Backup auf Bandlaufwerken) ersetzen und Rechenzentrum oder alternative Sites gegebenenfalls wiederherstellen können.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [Virtual Array Portal vorbereitet](storsimple-ova-deploy1-portal-prep.md).


