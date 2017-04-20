<properties 
   pageTitle="Best Practices für Virtual Array StorSimple | Microsoft Azure"
   description="Beschreibt die empfohlenen Vorgehensweisen für die Bereitstellung und Verwaltung der virtuellen StorSimple-Array."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Bewährte StorSimple Virtual Array

## <a name="overview"></a>Übersicht

Microsoft Azure StorSimple Virtual Array ist eine integrierte, die Aufgaben zwischen virtuellen lokalen-Gerät in ein Hypervisor und Microsoft Azure-Cloud-Speicher verwaltet. StorSimple Virtual Array ist eine effiziente, kostengünstige Alternative zur physischen Array 8000-Serie. Virtuelle Array auf Ihrer vorhandenen Infrastruktur Hypervisor ausgeführt werden kann, unterstützt die iSCSI und SMB-Protokolle und eignet sich für Szenarios mit Zweigstellen Office. Weitere Informationen über StorSimple Solutions finden Sie im [Microsoft Azure StorSimple Übersicht](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Dieser Artikel erläutert die best Practices während der anfänglichen Installation, Bereitstellung und Verwaltung von StorSimple Virtual Array implementiert. Diese best Practices bereitzustellen überprüfte Richtlinien für die Einrichtung und Verwaltung von virtual Array. Dieser Artikel richtet sich an IT-Administratoren, die Bereitstellung und Verwaltung der virtuellen Arrays in ihren Rechenzentren.

Wir empfehlen eine regelmäßige Überprüfung der best Practices sicherstellen, dass Ihr Gerät im Compliance wird Setup oder Operation Fluss geändert werden. Sie sollten Probleme auftreten, während der Implementierung der empfohlenen Vorgehensweisen auf Ihrem virtual Array [Microsoft Support wenden](storsimple-contact-microsoft-support.md) , um Hilfe.

## <a name="configuration-best-practices"></a>Best Practices für Konfiguration 

Diese Vorgehensweisen umfassen die Richtlinien, die bei der Erstinstallation und Bereitstellung der virtuellen Arrays müssen. Diese Vorgehensweisen gehören die Bereitstellung des virtuellen Computers Gruppenrichtlinien anpassen, Netzwerk eingerichtet, Speicherkonten konfigurieren und Erstellen von Freigaben und Volumes für das virtuelle Array. 

### <a name="provisioning"></a>Bereitstellung 

StorSimple Virtual Array ist des Hostservers einer virtuellen Maschine (VM) auf dem Hypervisor (Hyper-V oder VMware) bereitgestellt. Beim virtuellen Computer bereitstellen, damit der Host ausreichende Ressourcen aufwenden kann. Weitere Informationen finden Sie auf [minimale Ressourcen](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) ein Array bereitstellen. 

Implementieren Sie die folgenden best Practices beim Bereitstellen des virtual Arrays:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Virtuelle Maschine**   | **Generation 2** VM für die Verwendung mit Windows Server 2012 und ein *.vhdx* Bild. <br></br> **Generation 1** VM mit Windows Server 2008 oder höher und ein *VHD-* Abbild.                                                                                                              | Verwenden Sie virtueller Computer Version 8 bis 11 *.vmdk* -Image.                                                                      |
| **Speichertyp**            | Konfigurieren Sie als **statischen Speicher**. <br></br> Verwenden Sie nicht die Option **dynamic Memory** .            |                                                    |
| **Datenträger-Datentyp**         | Bereitstellen Sie als **dynamisch erweiterbar**.<br></br> **Feste Größe** dauert sehr lange. <br></br> Verwenden Sie nicht die Option **Vergleichen** .                                                                                                                   | Verwenden Sie die Option **dünne bereitstellen** .                                                                                      |
| **Änderung von Daten Datenträger** | Erweiterung oder Verkleinerung ist nicht zulässig. Ein Versuch führt zum Verlust aller lokalen Daten auf Gerät.                       | Erweiterung oder Verkleinerung ist nicht zulässig. Ein Versuch führt zum Verlust aller lokalen Daten auf Gerät. |

### <a name="sizing"></a>Größe

Beim Virtual Array StorSimple, Faktoren Sie die folgenden:

- Lokale Reservierung für Volumes oder Freigaben. Etwa 12 % des Speicherplatzes wird auf der lokalen Ebene für jedes bereitgestellte tiered Volumes oder Freigaben reserviert. 10 % des Speicherplatzes ist etwa auch für ein lokales Volume Dateisystem reserviert.
- Snapshot-overhead. 15 % Speicherplatz auf der lokalen Ebene ist ungefähr für Snapshots reserviert.
- Müssen für die Wiederherstellung. Wiederherstellen als eine neue Operation ausführen sollten Größe für den Speicherplatz für die Wiederherstellung berücksichtigt werden. Wiederherstellung ist für eine Freigabe oder das Volume gleich groß oder größer.
- Bei unerwartetem anwachsen sollten Puffer reserviert werden.

Anhand der obigen Faktoren können erforderlichen Größe durch folgende Gleichung dargestellt werden:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Die folgenden Beispiele veranschaulichen, wie Sie Ihre Bedürfnisse virtuelles Array Größe.

#### <a name="example-1"></a>Beispiel 1:
Ihr virtuelles Array möchten Sie können 

- Bereitstellung von 2 TB tiered Volumes oder Freigaben.
- Bereitstellung von 1 TB tiered Volumes oder Freigaben.
- 300 GB lokalen Volumes bereitstellen oder nutzen.


Berechnen Sie für die vorangehenden Volumes oder Freigaben wir Speicherplatz auf der lokalen Ebene. 

Zunächst würde für jeden gestuften Volume/Freigabe lokaler Reservierung 12 % der Volume-Freigabe entsprechen. Lokale Reservierung ist für lokales Volume/Freigabe 10 % der lokalen Volumenanteil (zusätzlich bereitgestellte Größe). In diesem Beispiel müssen Sie

- Lokale Reservierung 240 GB (2 TB gestuft Volume-Freigabe)
- Lokale Reservierung 120 GB (1 TB gestuft Volume-Freigabe)
- 330 GB für lokalen Volumes oder Freigaben (300 GB bereitgestellt Größe 10 % der lokalen Reservierung hinzufügen)

Ist der gesamte Speicherplatz auf der lokalen Ebene bisher: 240 GB + 120 GB + 330 GB = 690 GB.

Zweitens benötigen wir mindestens so viel Speicherplatz auf der lokalen Ebene als die größte einzelne Reservierung. Dieser zusätzliche Betrag wird verwendet von cloudmomentaufnahme wiederherstellen müssen. In diesem Beispiel ist die größte lokale Reservierung 330 GB (einschließlich Reservierung für File System), 690 GB: 690 GB + 330 GB = 1020 GB hinzufügen möchten.
Wenn wir weitere nachfolgende Wiederherstellung durchgeführt, können wir immer die aus der vorherigen Wiederherstellungsoperation Speicherplatz.

Schließlich brauchen wir 15 % des gesamten lokalen Space, lokale Snapshots gespeichert, damit nur 85 % verfügbar ist. In diesem Beispiel wäre um 1020 GB = 0,85&ast;bereitgestellte Daten Datenträger TB. So wäre der bereitgestellte Datenträger (1020&ast;(1/0,85)) = 1200 GB = 1,20 TB ~ 1,25 TB (zum nächsten Quartil runden)

Finanzierung unerwarteter Wachstum und neue Wiederherstellung sollten Sie bereitstellen, um einen lokalen Datenträger 1,25-1,5 TB.

> [AZURE.NOTE] Wir empfehlen auch die Festplatte dünn bereitgestellt wird. Diese Empfehlung ist Platz Wiederherstellung nur benötigt wird, um Daten, die älter als 5 Tage. Wiederherstellung auf Elementebene können Sie Daten für die letzten fünf Tage ohne zusätzlichen Speicherplatz für die Wiederherstellung.

#### <a name="example-2"></a>Beispiel 2: 
Ihr virtuelles Array möchten Sie können 

- Bereitstellung von 2 TB tiered-Volume
- Bereitstellen eines Volumes lokal fixiert 300 GB

Ausgehend von 12 % des lokalen Buchung für tiered Volumes-Freigaben und 10 % für lokalen Volumes-Freigaben müssen wir

- Lokale Reservierung 240 GB (2 TB gestuft Volume-Freigabe)
- 330 GB für lokalen Volumes oder Freigaben (300 GB bereitgestellt Platz 10 % der lokalen Reservierung hinzufügen)

Insgesamt Erforderlicher Speicherplatz auf der lokalen Ebene ist: 240 GB + 330 GB = 570 GB

Der minimale lokale Speicherplatz für die Wiederherstellung ist 330 GB. 

15 % der gesamten Festplatte wird zum Speichern von Snapshots, damit nur 0,85 verfügbar ist. Daher ist die Größe (900&ast;(1/0,85)) = 1,06 TB ~ 1,25 TB (zum nächsten Quartil runden) 

Einteilung in unerwartetem anwachsen, können Sie 1,25-1,5 TB lokal bereitstellen.


### <a name="group-policy"></a>Gruppenrichtlinien

Gruppenrichtlinien sind eine Infrastruktur, bestimmte Konfigurationen für Benutzer und Computer zu implementieren. Gruppenrichtlinien innerhalb von Gruppenrichtlinienobjekten (GPOs) die folgenden Active Directory-Domänendienste (AD DS) Containern verknüpft: Standorte, Domänen oder Organisationseinheiten (OUs). 

Wenn Ihr virtuelle Array Mitglied einer Domäne ist, können Gruppenrichtlinienobjekte zugewiesen sein. Diese Gruppenrichtlinienobjekte können wie eine Antivirensoftware installieren, die den Betrieb des StorSimple Virtual Array beeinträchtigen können.

Daher sollten Sie:

-   Sicherstellen Sie, dass Ihr virtuelle Array in eine eigene Organisationseinheit (OU) für Active Directory. 

-   Stellen Sie sicher, dass Ihr virtuelles Array keine Gruppenrichtlinienobjekte (GPOs) zugewiesen werden. Sie können die Vererbung um sicherzustellen, dass virtual Array (untergeordnete Knoten) nicht automatisch alle Gruppenrichtlinienobjekte vom übergeordneten Element erbt. Weitere Informationen finden Sie auf [Vererbung](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Netzwerk

Die Konfiguration für Ihr virtuelles Array erfolgt über lokale Web-UI. Durch den Hypervisor ist eine virtuelle Netzwerkschnittstelle aktiviert mit virtual Array bereitgestellt wird. Verwenden Sie die Seite [Network Settings](storsimple-ova-deploy3-fs-setup.md) virtual Network Interface IP-Adresse, Subnetzmaske und Gateway konfigurieren.  Sie können auch primäre und sekundäre DNS-Server, Intervalle und optionale Proxyeinstellungen für Ihr Gerät konfigurieren. Die Netzwerkkonfiguration ist eine einmalige Installation. [StorSimple Netzwerk Vorschriften](storsimple-ova-system-requirements.md#networking-requirements) vor der Bereitstellung des virtual Arrays überprüfen.

Wenn Ihr virtuelle Array bereitstellen, wird empfohlen, folgende best Practices einzuhalten:

-   Sicherstellen Sie, dass das Netzwerk in dem virtual Array immer bereitgestellt wird 5 Mbit/s Internetbandbreite (oder länger) ist. 

    -   Internet Bandbreite benötigen variiert abhängig von der arbeitslastmerkmale und die Daten ändern.

    -   Die Daten ändern, die behandelt werden können ist direkt proportional zur Internet-Bandbreite. Beispielsweise wenn eine Sicherung bietet 5 Mbit/s Bandbreite eine Datenänderung von ungefähr 18 GB 8 Stunden. Mit vier Mal mehr Bandbreite (20 Mbit/s) können Sie vier Mal mehr Daten ändern (72 GB) behandeln. 

-   Sicherstellen Sie, dass der Internetkonnektivität immer verfügbar ist. Sporadisch oder unzuverlässige Internet-Verbindung der Geräte möglicherweise der Zugriff auf Daten in der Cloud und in einer nicht unterstützten Konfiguration führen.

-   Wenn Sie Ihr Gerät als iSCSI-Server bereitstellen möchten: 
    -   Wir empfehlen die Option **Get IP-Adresse automatisch** (DHCP) zu deaktivieren. 
    -   Konfigurieren Sie statische IP-Adressen. Sie müssen einen primären und einen sekundären DNS-Server konfigurieren.

    -   Wenn mehrere Netzwerkschnittstellen virtuelles definieren array nur die erste Schnittstelle (Standardmäßig ist diese Schnittstelle **Ethernet**) erreichen die Cloud. Zum Steuern der Art des Datenverkehrs können mehrere virtuelle Netzwerkschnittstellen auf Ihrem virtual Array (konfiguriert als iSCSI-Server) erstellen und verschiedene Subnetze Schnittstellen an.

-   Um die Cloud-Bandbreite einschränken (verwendet von virtual Array), Beschränkung auf dem Router oder die Firewall konfigurieren. Wenn Sie Ihre Hypervisor Drosselung definieren, wird es alle Protokolle sowie iSCSI SMB statt nur die Cloud Bandbreite einschränken. 

-   Sicherstellen Sie, dass Synchronisierung für Hypervisoren aktiviert ist. Wenn mit Hyper-V virtuelle Array im Hyper-V-Manager auswählen, gehen Sie zu **Settings &gt; Integration Services**, und stellen Sie sicher, dass die **Synchronisierung** aktiviert ist.

### <a name="storage-accounts"></a>Speicherkonten

StorSimple Virtual Array kann ein einzelnes Speicherkonto zugeordnet werden. Dieses Speicherkonto kann eine automatisch generierte Speicherkonto ein Konto im gleichen Abonnement Service oder ein Speicherkonto Zusammenhang mit einem anderen Abonnement. Weitere Informationen finden Sie unter Verwalten von [Speicherkonten für Ihr virtuelles Array](storsimple-ova-manage-storage-accounts.md).

Verwenden Sie Folgendes für Speicherkonten virtual Array zugeordnet.

-   Bei mehreren virtuellen Arrays ein einzelnes Speicherkonto Faktor in die maximale Kapazität (64 TB) für virtuelles Array und die maximale Größe (500 TB) ein Speicherkonto. Dies begrenzt die Anzahl voller Größe virtuellen Arrays, die diesem Storage-Konto über 7 zugeordnet werden können.

-   Beim Erstellen eines neuen Speicherkontos
    -   Wir empfehlen in der Region am nächsten remote Office/Branch Office erstellen, wo das virtuelle StorSimple Array zu Wartezeiten bereitgestellt wird.

    -   Denken Sie daran, dass ein Speicherkonto in verschiedenen Regionen verschoben werden kann. Einen Dienst kann nicht auch zwischen Abonnements verschoben werden.

    -   Verwenden Sie ein Speicherkonto, die Redundanz zwischen Rechenzentren implementiert. Geo-Redundant Storage (GRS) Zone redundanten Speicher (ZRS) und lokal redundanter Speicher (LRS) werden alle mit virtual Array unterstützt. Weitere Informationen zu den verschiedenen Speicherkonten zum Wechseln Sie [Azure Storage-Replikation](../storage/storage-redundancy.md)


### <a name="shares-and-volumes"></a>Freigaben und volumes

Für das StorSimple Virtual Array können Sie Freigaben bereitstellen, bei der Konfiguration eines Dateiservers und Volumes als iSCSI-Server konfiguriert. Die optimalen Methoden zum Erstellen von Freigaben und Volumes beziehen sich auf die Größe und den Typ konfiguriert.

#### <a name="volumeshare-size"></a>Größe des Datenträger-Freigabe

Ihr virtuelles Array können Sie Freigaben bereitstellen, bei der Konfiguration eines Dateiservers und Volumes als iSCSI-Server konfiguriert. Die optimalen Methoden zum Erstellen von Freigaben und Volumes beziehen sich auf die Größe und den Typ konfiguriert. 

Beachten Sie die folgenden best Practices beim Freigaben oder Volumes auf das virtuelle Gerät bereitstellen.

-   Dateigrößen Verhältnis bereitgestellte tiered Freigabe können tiering Leistung auswirken. Arbeiten mit großen Dateien führen langsam Ebene aus. Beim Arbeiten mit großen Dateien wird empfohlen, die größte Datei kleiner als 3 % der Freigabe ist.

-   Maximal 16 Volumes-Freigaben kann virtuelle Array erstellt. Lokal fixiert, kann der Datenträger Anteile zwischen 50 GB bis 2 TB. Wenn gestuft, muss die Volumes Anteile 500 GB bis 20 TB. 

-   Beim Erstellen eines Volumes Faktor in die erwarteten Daten sowie zukünftiges Wachstum. Wenn das Volume später erweitert werden kann, können Sie immer auf einen größeren Datenträger wiederherstellen.

-   Nachdem das Volume erstellt wurde, kann nicht die Größe des Volumes auf StorSimple verkleinert.
   
-   Beim Schreiben von StorSimple, tiered Volume, Volume-Daten einen bestimmten Schwellenwert (bezogen auf die lokalen reserviert für das Volume) erreicht, ist der e/a gedrosselt. Weiterhin auf dieses Volume schreiben verlangsamt IO deutlich. Sie tiered Volume über die bereitgestellte Kapazität schreiben (wir sind aktive Benutzer schreiben bereitgestellte Fähigkeit nicht), sehen Sie eine Benachrichtigung für den Effekt Sie überzeichnet. Wenn Sie die Warnung sehen, unbedingt Sie Maßnahmen wie Löschen Daten des Datenträgers oder den Datenträger auf einen größeren Datenträger wiederherstellen (Volume-Erweiterung wird derzeit nicht unterstützt).

-   Für Disaster Recovery Fällen die Anzahl der zulässigen Aktien-Volumes ist 16 und die maximale Anzahl von Freigaben/Volumes, die parallele Verarbeitung auch 16, die Anzahl der Aktien-Volumes auf Ihrem RPO und RTO keinen. 

#### <a name="volumeshare-type"></a>Volume-Freigabe-Typ

StorSimple unterstützt zwei Datenträger-Freigabe anhand ihrer Verwendung: lokal fixiert und Ebenen. Lokalen Volumes-Freigaben werden dicht bereitgestellt, während der gestuften Volumes Anteile dünn bereitgestellt werden. 

Wir empfehlen folgenden Vorgehensweisen beim Konfigurieren von StorSimple-Volumes Anteile implementieren:

-   Identifizieren Sie den Datenträgertyp basierend auf Arbeitslast, die Sie bereitstellen, bevor Sie ein Volume erstellen möchten. Verwenden Sie lokale Volumes für Arbeitslasten erfordern lokale Garantien von Daten (auch bei einem Ausfall Cloud) und die niedrigen Cloud Wartezeiten erfordern. Nach dem Erstellen eines Volumes auf Ihrem virtual Array kann nicht, ändern Sie den Volumetyp von fixiert Sie lokal zu tiered oder *umgekehrt*. Erstellen Sie beispielsweise lokale Volumes, wenn SQL Arbeitslasten oder Arbeitslasten hosting virtuelle Maschinen (VMs) bereitstellen Verwenden Sie mehrstufige Volumes für Datei Share Arbeitslasten.

-   Aktivieren Sie die Option für weniger häufig verwendeten Archivdaten Umgang mit großen Dateien. Deduplizierung Chunk größer 512 k wird verwendet, wenn diese Option aktiviert ist, um die Datenübertragung in die Cloud zu beschleunigen.

#### <a name="volume-format"></a>Datenträger formatieren

Nach dem Erstellen StorSimple-Volumes auf dem iSCSI-Server müssen Sie initialisieren, bereitstellen und die Datenträger formatieren. Dieser Vorgang ist auf dem Host verbunden StorSimple ausgeführt. Folgende Vorgehensweisen sind bei Montage und Formatierung von Volumes auf dem Host StorSimple empfehlen.

-   Durchführen Sie Formatierung mit QuickFormat, auf alle StorSimple.

-   Verwenden Sie beim Formatieren eines Volumes StorSimple Größe der Zuordnungseinheiten (AUS) von 64 KB (Standardwert ist 4 KB). 64 KB AUS basiert auf Tests für allgemeine StorSimple Workloads und anderen Arbeitslasten durchgeführt.

-   Verwendung des Virtual StorSimple-Array als ein iSCSI-Server übergreifende Datenträger oder dynamischen Datenträger nicht als diese Volumes verwenden oder Datenträger werden vom StorSimple nicht unterstützt.

#### <a name="share-access"></a>Freigeben des Zugriffs

Beim Erstellen von Freigaben auf dem Dateiserver virtuelles Array gelten Sie folgende Richtlinien:

-   Beim Erstellen einer Freigabe weisen Sie eine Benutzergruppe als Administrator Freigabe anstelle eines einzelnen Benutzers.

-   Nach dem Erstellen die Freigabe Freigaben über Windows Explorer bearbeiten, können Sie die NTFS-Berechtigungen verwalten.

#### <a name="volume-access"></a>Volume-Zugriff

Beim Konfigurieren von iSCSI-Volumes auf virtuellen StorSimple Array unbedingt gegebenenfalls den Zugriff steuern. Um zu bestimmen, welche Hostserver Volumes zugreifen können, erstellen und StorSimple-Volumes Zugriff Überprüfungsprotokolle (ACRs) zuordnen.

Verwenden Sie die folgenden best Practices beim Konfigurieren von ACRs für StorSimple-Volumes:

-   Ein Volume immer mindestens ACR zuordnen.

-   Definieren Sie mehrere ACRs in einer Clusterumgebung.

-   Wenn ein Volume mehrere ACR zuweisen, sicherzustellen Sie, dass das Volume nicht so verfügbar gemacht wird, wo es gleichzeitig von mehr als einem nicht gruppierten Server zugegriffen werden kann. Wenn Sie ein Volume mehrere ACRs zugewiesen haben, erscheint eine Warnung für Ihre Konfiguration zu überprüfen.

### <a name="data-security-and-encryption"></a>Sicherheit und Verschlüsselung

Virtuelle StorSimple Array bietet Daten Sicherheit und Verschlüsselung, die die Vertraulichkeit und Integrität Ihrer Daten zu gewährleisten. Wenn Sie diese Features verwenden, sollten folgende best Practices einzuhalten: 

-   Definieren Sie eine Cloud Storage Chiffrierschlüssel AES-256-Verschlüsselung generieren, bevor die Daten in die Cloud aus virtual Array gesendet werden. Dieser Schlüssel ist nicht erforderlich, wenn die Daten zunächst verschlüsselt werden. Der Schlüssel kann generiert und sicher über ein Schlüsselverwaltungssystem wie [Azure-Tresor](../key-vault/key-vault-whatis.md)aufbewahrt werden.

-   Wenn das Speicherkonto über StorSimple Manager-Dienst zu konfigurieren, unbedingt aktivieren SSL-Modus, um einen sicheren Kanal für die Kommunikation zwischen dem StorSimple-Gerät und der Cloud erstellen.

-   Regenerieren Sie Schlüssel für Speicherkonten (durch den Zugriff auf den Azure-Speicher-Dienst) regelmäßig auf Änderungen auf anhand der geänderten Liste der Administratoren.

-   Daten auf das virtual Array komprimiert und dedupliziert, bevor sie in Azure gesendet wird. Empfehlen wir keine Datendeduplizierung Rollendienst auf dem Windows Server-Host.


## <a name="operational-best-practices"></a>Bewährte Betriebsmethoden

Bewährte Betriebsmethoden sind Richtlinien während der Geschäftsführung oder Betrieb des Arrays virtuellen befolgt. Vorgehensweisen umfassen bestimmte Verwaltungsaufgaben wie Backups aus einem Sicherungssatz wiederherstellen einen Failover ausführen, deaktivieren und löschen das Array Systemverwendung und Health monitoring und Viren scannt Ihr virtuelles Array.

### <a name="backups"></a>Backups

Die Daten auf Ihrem virtuellen Array in die Cloud auf zwei Arten gesichert, Standard automatische tägliche Sicherung des gesamten Geräts ab 22:30 oder über eine manuelle Sicherung auf Anforderung. Standardmäßig erstellt das Gerät automatisch tägliche Cloud Snapshots aller auf diese Daten. Weitere Informationen finden Sie [StorSimple Virtual Array](storsimple-ova-backup.md)zurück.

Die Häufigkeit und Aufbewahrungsfristen Backups Standard zugeordnet nicht geändert werden, aber die Zeit mit täglichen Backups täglich initiiert werden können. Wenn die Startzeit für die automatische Sicherung zu konfigurieren, empfehlen wir:

-   Planen Sie Backups für Zeiten. Backup Startzeit notwendig mit zahlreichen IO nicht.

-   Initiieren einer manuellen Sicherung auf Anforderung beim Planen einer Device-Failover oder zum Schutz der Daten auf Ihrem virtuellen Array im Wartungsfenster vor.

### <a name="restore"></a>Wiederherstellen

Sie können von einem Sicherungssatz auf zwei Arten wiederherstellen: auf anderen Volumes oder Freigaben wiederherstellen, oder führen Sie eine Wiederherstellung auf Elementebene (verfügbar nur für virtuelles Array als Dateiserver konfiguriert). Wiederherstellung auf Elementebene können Sie detaillierte Recovery von Dateien und Ordnern aus einer Cloud-Sicherung aller Freigaben auf dem Gerät StorSimple. Weitere Informationen zum wechseln [von einer Sicherung wiederherstellen](storsimple-ova-restore.md)

Beim Durchführen einer Wiederherstellung sollten Sie folgenden Richtlinien beachten:

-   Virtuelle StorSimple Array unterstützt keine in-Place-Wiederherstellung. Dies kann jedoch leicht durch zwei Schritte erreicht: Platz auf dem virtuellen Arrays und anschließend auf einem anderen Volume-Freigabe.

-   Auf einem lokalen Volume wiederherstellen dabei werden die Wiederherstellung eine umfangreiche Operation. Obwohl die Lautstärke schnell online geschaltet werden kann, weiterhin die Daten im Hintergrund hydratisiert.

-   Der Datenträgertyp bleibt während des Wiederherstellungsvorgangs. Tiered-Volume auf ein anderes tiered Volume wiederhergestellt und ein lokales Volume zu einem anderen lokalen Datenträger fixiert.

-   Wenn Sie versuchen, ein Volume oder eine Freigabe aus einer Sicherung wiederherstellen festgelegt, schlägt die Auftrag ein Ziel-Volume oder Freigabe im Portal erstellt werden. Es ist wichtig, dass diese nicht verwendeten Zielvolume löschen oder im Portal freigeben zu zukünftige Probleme aus diesem Element.

### <a name="failover-and-disaster-recovery"></a>Failover und Disaster recovery

Device-Failover ermöglicht das Migrieren der Daten von einem Gerät *Quelle* im Datencenter zu *einem anderen Zielgerät im selben oder einem anderen Standort befindet* . Geräte-Failover ist für das Gerät. Während eines Failovers ändert die clouddaten für das Quell-Device Besitz, das Zielgerät.

Für das StorSimple Virtual Array können Sie nur in ein anderes virtuelles Array vom gleichen StorSimple Manager-Dienst verwaltet Failover. Ein Failover auf ein Gerät 8000-Serie oder ein Array von einem anderen StorSimple Manager-Dienst (als für das Quell-Device) verwaltet ist nicht zulässig. Erfahren Sie mehr über die Failover-Hinweise zum Wechseln Sie [Voraussetzung für Device-failover](storsimple-ova-failover-dr.md)

Beachten Sie bei Fail über virtual Array Folgendes:

-   Bei einem geplanten Failover ist es empfiehlt es sich zu alle Volumes/Freigaben offline vor das Failover initiiert. Anweisungen Sie die betriebssystemspezifischen zuerst die Volumes Anteile offline auf dem Host und dann die offline auf Ihrem virtuellen Gerät.

-   Datei Server Disaster Recovery (DR) sollten Sie das Zielgerät zu derselben Domäne wie die Quelle verknüpfen, sind die Freigabeberechtigungen aufgelöst. Das Failover auf ein Zielgerät in der gleichen Domäne ist in dieser Version unterstützt.

-   Sobald die DR erfolgreich abgeschlossen ist, wird das Quellgerät automatisch gelöscht. Wenn das Gerät nicht mehr verfügbar ist, ist der virtuelle Computer, die auf dem Host-System bereitgestellt noch Ressourcen belegt. Wir empfehlen Sie diesen virtuellen Computer Host anfallenden Gebühren verhindern löschen.

-   Beachten Sie, das ist das Failover nicht erfolgreich, **die Daten werden immer in der Cloud**. Berücksichtigen Sie die folgenden drei Szenarien, in denen das Failover nicht erfolgreich abgeschlossen wurde:

    -   Fehler in der Anfangsphase des Failovers wie bei Dr. Vorabprüfungen durchgeführt werden. In diesem Fall kann das Zielgerät weiterhin verwendet werden. Sie können das Failover auf dasselbe Zielgerät wiederholen.

    -   Fehler bei der tatsächlichen Failover. In diesem Fall ist das Zielgerät nicht verwendbar gekennzeichnet. Sie müssen bereitstellen und Konfigurieren einer anderen virtuellen Zielarrays und für Failover verwenden.

    -   Das Failover wurde vollständig nach dem Quellgerät gelöscht wurde, aber das Zielgerät hat Probleme und auf Daten zugreifen. Daten in der Cloud ist noch sicher und einfach durch ein anderes virtuelles Array erstellt und dann als ein Gerät für die Disaster Recovery mit abgerufen werden.

### <a name="deactivate"></a>Deaktivieren

Beim Deaktivieren von StorSimple virtuelles Array trennt die Verbindung zwischen dem Gerät und der entsprechenden StorSimple Manager-Dienst. Deaktivierung ist ein **endgültiger** Vorgang und kann nicht rückgängig gemacht werden. Ein deaktiviertes Gerät kann wieder mit der StorSimple Manager-Dienst registriert werden. Weitere Informationen zum [deaktivieren und StorSimple Virtual Array löschen](storsimple-deactivate-and-delete-device.md).

Beachten Sie die folgenden best Practices, wenn Ihr virtuelle Array deaktivieren:

-   Eine Momentaufnahme Cloud Die Daten vor ein virtuelles Gerät deaktivieren. Virtuelles Array deaktivieren, wird die lokale Gerätedaten verloren. Cloud Momentaufnahme können Sie Daten zu einem späteren Zeitpunkt wiederherzustellen.

-   Deaktivieren von StorSimple virtuelles Array Vergewissern Sie zu beenden oder Löschen von Clients und Hosts, die das Gerät abhängig.

-   Löschen Sie ein deaktiviertes Gerät, wenn Sie nicht mehr verwenden, damit Gebühren anfallen nicht.

### <a name="monitoring"></a>Überwachung

Damit virtuelle StorSimple Array kontinuierlichen fehlerfrei ist, müssen Sie Arrays überwachen und sicherstellen, dass aus dem System Informationen, einschließlich Alerts. Zur Überwachung des Gesamtzustands des virtual Array implementiert die folgenden best Practices:

- Konfigurieren Sie um die Datenträgerverwendung der Arrays virtuellen Datenträger sowie die Betriebssystem-Datenträger überwachen Überwachung. Wenn Hyper-V ausführen, können Sie aus System Center Virtual Machine Manager (SCVMM) und System Center Operations Manager (SCOM), Ihre Virtualisierungshosts überwachen.   

- Konfigurieren Sie e-Mail-Benachrichtigung auf Ihrem virtual Array Benachrichtigungen auf bestimmte Verwendung.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Gesucht und Virus scan Applikationen

StorSimple virtuelles Array können automatisch von der lokalen Ebene Microsoft Azure Cloud Datenebene. Bei eine Anwendung gesucht oder einen VirusScan StorSimple gespeicherten Daten scannen müssen Sie darauf achten, dass die clouddaten nicht zugegriffen und auf der lokalen Ebene zurückgezogen.

Wir empfehlen, die folgenden best Practices implementieren, wenn Index suchen oder Virus-Scan auf Ihrem virtual Array konfigurieren:

-   Automatisch konfigurierte vollständige Scannen zu deaktivieren.

-   Konfigurieren Sie für mehrstufige Volumes die Index suchen oder Virus Scan-Anwendung eine inkrementelle Suche durchführen. Dies wäre nur die neuen Daten wahrscheinlich wohnen auf der lokalen Ebene scannen. Die Daten in der Cloud Ebenen erfolgt nicht während einer inkrementelle.

-   Sicherstellen der richtigen Filter und Einstellungen konfiguriert, damit nur die gewünschten Dateitypen gescannt. Z. B. Bilddateien (JPEG-, GIF- und TIFF) und Konstruktionszeichnungen nicht gescannt werden sollen während der inkrementellen oder vollständigen Index neu erstellen.

Wenn Windows Indizierungsvorgang, gelten Sie folgende Richtlinien:

-   Verwenden Sie Windows-Indexer für mehrstufige Volumes wie sie große Datenmengen (TBs) aus der Cloud Index muss häufig neu erstellt werden. Erneutes Erstellen des Indexes werden alle Dateitypen, um ihren Inhalt indiziert abgerufen.

-   Verwenden Sie die Windows Indizierungsvorgang für lokale Volumes, wie dies nur Daten auf lokalen Ebenen zum Erstellen des Index (clouddaten ist nicht möglich).

### <a name="byte-range-locking"></a>Byte Bereich Sperren

Applikationen Sperren ein angegebenen Bereichs von Bytes in den Dateien. Wenn Byte Bereich Sperren für die Anwendung, die in Ihrem StorSimple schreiben aktiviert ist, funktioniert dann tiering auf Ihrem virtual Array nicht. Zur Stufung arbeiten, sollten alle Bereiche des aufgerufenen Dateien entsperrt werden. Byte Bereich Sperren wird mit tiered virtual Array nicht unterstützt.

Empfohlene Maßnahmen vermieden Byte Bereich Sperren umfassen:

-   Sperren in die Anwendungslogik Bytebereich deaktivieren.

-   Lokal verwendet fixiert Volumes (anstelle von Ebenen) für die Daten dieser Anwendung zugeordnet. Lokale Volumes nicht in die Cloud tier.

-   Wenn Volumes mit lokal mit Byte Bereich aktiviert fixiert werden, kann das Volume online geschaltet, bevor die Wiederherstellung abgeschlossen ist. In diesen Fällen müssen Sie die Wiederherstellung vollständig warten.

## <a name="multiple-arrays"></a>Mehrere arrays

Mehrere virtuelle Arrays müssen in der Cloud so beeinflussen die Leistung des Geräts verschütten konnte Konto für wachsende Workingset Daten bereitgestellt werden. In diesen Fällen empfiehlt es sich, Maßstab Geräte zunehmender Workingsets. Dies erfordert ein oder mehrere Geräte im Rechenzentrum lokalen hinzugefügt werden. Wenn Sie Geräte hinzufügen, können Sie:

-   Teilen Sie den aktuellen Satz von Daten.
-   Bereitstellen Sie neue Arbeitslasten auf neue Geräte.
-   Wenn mehrere virtuelle Arrays bereitstellen, empfehlen wir aus den Lastenausgleich im Hinblick auf das Array auf anderen Hypervisorhosts verteilen.

-  Mehrere virtuelle Arrays (wenn ein iSCSI-Server sowie einen Dateiserver konfiguriert) können in einer verteilten Datei System-Namespace bereitgestellt werden. Ausführliche Schritte finden Sie unter [Distributed File System Namespace Solution mit hybriden Cloud Storage-Bereitstellungshandbuch](https://www.microsoft.com/download/details.aspx?id=45507). Distributed File System Replication wird derzeit nicht für die Verwendung mit virtual Array empfohlen. 


## <a name="see-also"></a>Siehe auch
Informationen über den Dienst StorSimple Manager [virtuelle StorSimple Array verwalten](storsimple-ova-manager-service-administration.md) .
