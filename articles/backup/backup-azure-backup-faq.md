<properties
   pageTitle="Azure Backup FAQ | Microsoft Azure"
   description="Antworten auf häufig gestellte Fragen zu den Sicherungsdienst, backup-Agent Sicherheit Backup und Recovery, Archivierung und andere häufig gestellte Fragen zu Backup und Disaster Recovery."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="Backup- und Disaster Recovery; Backup-service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>

# <a name="azure-backup-service--faq"></a>Azure Backup Service-FAQ


Dieser Artikel ist eine Liste der häufig gestellten Fragen (und die entsprechenden Antworten) zum Azure Backup-Dienst. Unsere Community antwortet, und wenn eine häufig Frage wir es in diesem Artikel. Antworten auf Fragen normalerweise Verweis und Supportinformationen. Sie können Fragen zu Azure Backup im Abschnitt Disqus dieser Artikel oder einen Artikel. Fragen zur Azure Backup Service buchen Sie im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).


## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup-br"></a>Was ist die Liste der unterstützten Betriebssysteme aus denen ich in Azure mithilfe von Azure Backup gesichert werden? <br/>
Azure Backup unterstützt folgende Betriebssysteme für Ordner Backup Anwendung Sicherungskopie Azure Backup-Server und SCDPM.

| Betriebssystem        | Plattform           | SKU  |
| :------------- |-------------| :-----|
| Windows 8 und neueste SPs      | 64-bit | Pro Unternehmen |
| Windows 7 und neueste SPs      | 64-bit | Ultimate, Enterprise, Professional, Home Premium Home Basic Starter |
| Windows 8.1 und neueste SPs | 64-bit      |    Pro Unternehmen |
| Windows 10      | 64-bit | Enterprise, Pro Haus |
|Windows Server 2012 R2 und neueste SPs| 64-bit| Standard, Datacenter Foundation|
|Windows Server 2012 und neueste SPs|    64-bit| Datacenter Foundation, Standard|
|Windows Storage Server 2012 R2 und neueste SPs  |64-bit|    Workgroup-Standard|
|Windows Storage Server 2012 und neueste SPs |64-bit |Workgroup-Standard
|Windows Server 2012 R2 und neueste SPs  |64-bit|    Wichtig|
|Windows Server 2008 R2 SP1 |64-bit|    Standard, Enterprise, Datacenter Foundation|
|Windows Server 2008 SP2    |64-bit|    Standard, Enterprise, Datacenter Foundation|

Azure VM-Backup

- **Linux**: Azure Backup [eine Liste der Distributionen, die von Azure unterstützt,](../virtual-machines/virtual-machines-linux-endorsed-distros.md) Core Betriebssystem Linux unterstützt.  Andere in-Your-eigenen-Linux-Distributionen können solange der VM-Agent auf dem virtuellen Computer verfügbar ist auch Unterstützung für Python vorhanden ist.
- **Windows Server**: Windows Server 2008 R2-Versionen werden nicht unterstützt.

## <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Wo kann den neuesten Azure Backup Agent herunterladen? <br/>
Aktuellen Agent können zum Sichern von Windows Server System Center DPM oder Windows-Client [hier](http://aka.ms/azurebackup_agent). Um einen virtuellen Computer zu sichern, verwenden Sie die VM Agent (installiert automatisch die richtige Erweiterung). VM-Agent ist bereits auf virtuellen Maschinen von Azure Galerie erstellt.

## <a name="which-version-of-scdpm-server-is-supported-br"></a>Welche Version von SCDPM Server unterstützt? <br/>
Wir empfehlen die Installation des [neuesten](http://aka.ms/azurebackup_agent) Azure Backup-Agents auf das neueste Updaterollup SCDPM (UR11 August 2016)

## <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Bei der Konfiguration von Azure Backup-Agent werde ich aufgefordert, die Vault-Anmeldeinformationen eingeben. Laufen Depot Anmeldeinformationen?
Ja, Ablauf Depot Anmeldeinformationen nach 48 Stunden. Die Datei läuft ab, Azure-Portal melden Sie an und Dateien Sie das Depot Anmeldeinformationen von Ihrem Tresor.

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Gibt es irgendeine Einschränkung für die Anzahl der Depots in jede Azure-Abonnement erstellt werden kann? <br/>
Ja. Ab September 2016 erstellen 25 backup Depots pro Abonnement. Sie können bis zu 25 Recovery Services Depots pro unterstützten Regionen Azure Backup pro Abonnement erstellen. Benötigen Sie mehr Depots, erstellen Sie ein neues Abonnement.

## <a name="are-there-any-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Gibt es keine Grenzen für die Anzahl der für jedes Depot Registrierung Server/Computer? <br/>
Ja, können Sie bis zu 50 Computer pro Depot registrieren. Für Azure IaaS virtuelle Maschinen ist begrenzt 200 VMs pro Depot. Benötigen Sie weitere Computer zu registrieren, erstellen Sie ein neues Depot.

## <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Wie registriere ich meinen Server in einem anderen Rechenzentrum?<br/>
Backup-Daten werden in das Rechenzentrum das Depot gesendet es registriert. Am einfachsten Datencenter ändern wird, deinstallieren Sie den Agent und installieren Sie den Agent neu registrieren, um ein neues Depot gewünschten Datacenter gehört.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Was geschieht, wenn ich WindowsServer umbenennen, der in Azure sichern?<br/>
Beim Umbenennen eines Servers werden alle aktuell konfigurierten Backups beendet.
Sie müssen den neuen Namen des Servers mit der Sicherung zu registrieren. Beim Erstellen einer neuen Registrierung ist der erste Sicherungsvorgang eine vollständige Sicherung und keiner inkrementellen Sicherung. Möchten Sie Daten wiederherstellen, die zuvor in das Depot mit dem alten Namen gesichert wurden, können Sie mit [**einem anderen Server**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) Option im Assistenten zum **Wiederherstellen von** Daten wiederherstellen.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from-br"></a>Welche Arten von Festplatten kann ich Dateien und Ordner sichern? <br/>
Die folgenden Festplatten-Volumes kann nicht backup erhalten:

- Wechselmedien: Laufwerk melden zu festen backup Elementquelle verwendet.
- Schreibgeschützte Datenträger: der Datenträger muss für den Volumeschattenkopie-Dienst (VSS) Funktion geschrieben werden.
- Offline Volumes: Online VSS Funktion muß.
- Netzwerk: das Volume muss lokal auf dem Server mit der online-Sicherung gesichert werden.
- BitLocker geschützten Volumes: das Volume muss aufgehoben werden, bevor die Sicherung durchgeführt werden kann.
- Datei Identifizierung: NTFS ist das einzige Dateisystem für diese Version online backup-Dienst unterstützt.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Werden welche Dateien und Ordner können vom Server gesichert?<br/>
Die folgenden Typen werden unterstützt:

- Verschlüsselt
- Komprimiert
- Geringe Datendichte
- Komprimierte + Sparse
- Feste Links: Nicht unterstützt, wird übersprungen
- Analysepunkt: Nicht unterstützt, wird übersprungen
- Verschlüsselt und komprimiert: Nicht unterstützt, wird übersprungen
- Verschlüsselte + Sparse: Nicht unterstützt, wird übersprungen
- Komprimierte Stream: Nicht unterstützt, wird übersprungen
- Sparse Stream: Nicht unterstützt, wird übersprungen

## <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Was ist die Mindestgröße für den Cacheordner? <br/>
Die Größe des Cacheordners bestimmt die Menge der Daten, die Sie möchten sichern. Cache-Ordner sollte 5 % des Speicherplatzes zum Speichern von Daten.

## <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Hat das Unternehmen ein Depot können wie ich einen Server Daten von einem anderen Server isolieren, wenn Daten wiederherstellen?<br/>
Alle Server, die für dasselbe Depot registriert können andere Server *, die die gleiche Passphrase verwenden*gesicherten Daten wiederherstellen. Haben Sie Server, deren backup-Daten von anderen Servern in Ihrer Organisation isolieren möchten, verwenden Sie festgelegte Kennwort für diese Server. Personal Server können z. B. eine verschlüsselungspassphrase accounting Server und in der Speicherserver dritte.

## <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Kann ich "Meine Backup- und Vault zwischen Abonnements migrieren"? <br/>
Nein. Das Depot Ebene Abonnement erstellt und kann nicht mit einem anderen Abonnement zugewiesen werden, erstellt werden.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Funktioniert Azure Backup-Agent auf einem Server, der Windows Server 2012 Deduplizierung verwendet? <br/>
Ja. Der Agent-Dienst konvertiert deduplizierten Daten in Daten, wenn den Sicherungsvorgang vorbereitet. Optimiert die Daten für die Sicherung, verschlüsselt die Daten und sendet dann die verschlüsselten Daten online backup Service.

## <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Wenn ich nach dem Starten ein Sicherungsauftrags Abbrechen, werden übertragenen Sicherungsdaten gelöscht? <br/>
Nein. Backup Vault speichert die gesicherten Daten, die bis zum Zeitpunkt der Kündigung übertragen wurden. Azure Backup verwendet einen Mechanismus Checkpoint Prüfpunkte auf Sicherungsdaten gelegentlich während der Sicherung hinzufügen. Da Checkpoints im backup-Daten vorhanden sind, kann der nächste backup-Prozess die Integrität der Dateien überprüfen. Die nächste Sicherung ausgelöst wäre inkrementellen Daten, die zuvor gesichert haben. Eine inkrementelle Sicherung bietet eine bessere Nutzung der Bandbreite, sodass nicht immer wieder dieselben Daten übertragen werden müssen.

Bei Azure VM Backup nach der Auftrag abgebrochen, übertragene Daten ignoriert, und neue Sicherungskopie aus zuvor erfolgreich Sicherungsauftrag inkrementelle Daten übertragen.

## <a name="why-am-i-seeing-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-had-scheduled-regular-backups-previously-br"></a>Weshalb sehe ich die Warnung "Azure Backups nicht für diesen Server konfiguriert haben", obwohl ich regelmäßige Backups bereits geplant hatten? <br/>
Diese Warnung tritt auf, wenn die Sicherungszeitplan Einstellungen auf dem lokalen Server nicht Einstellungen im backup-Tresor gespeichert sind. Wenn der Server oder die Einstellungen auf einen als funktionierend bekannten Zustand wiederhergestellt wurden, können backup-Zeitpläne Synchronisation verlieren. Wenn Sie diese Warnung, [backup-Richtlinie konfigurieren](backup-azure-manage-windows-server.md) und dann **Führen Sie jetzt** zum Synchronisieren des lokalen Servers mit Azure erhalten.

## <a name="what-firewall-rules-should-be-configured-for-azure-backup-br"></a>Welche Firewall Regeln für Azure Backup konfiguriert werden sollten? <br/>
Nahtlosen Schutz auf lokal-auf Azure und Arbeitslast Azure wird empfohlen, dass Ihre Firewall die folgenden URLs zu ermöglichen:

- www.msftncsi.com
- \*. Microsoft.com
- \*. WindowsAzure.com
- \*. microsoftonline.com
- \*. Windows

##<a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Kann ich auf eine Azure VM bereits von Azure Backup Service mit der VM-Erweiterung unterstützt Azure Backup-Agent installieren? <br/>
Absolut. Azure Backup bietet Backup auf VM-Ebene für Azure VMs VM-Erweiterung verwenden. Sie können Azure Backup-Agent auf einem Windows Gastbetriebssystem zu Dateien und Ordnern, Gastbetriebssystem installieren.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Kann ich Azure Backup-Agent auf einer Azure VM Sichern von Dateien und Ordner auf temporären Speicher der Azure-VM installieren? <br/>
Sie können auf dem Gastbetriebssystem Windows Azure Backup-Agent installieren und Sichern von Dateien und Ordnern in den temporären Speicher. Beachten Sie jedoch, dass Backups fehlschlagen, wenn temporäre Daten gelöscht. Auch die temporäre Speicherung Daten gelöscht, können Sie nur in den nicht flüchtigen Speicher wiederherstellen.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Ich habe meine Dateien und Ordner schützen Azure Backup Agent installiert. Kann ich jetzt SCDPM Azure Backup Agent für lokale Anwendung-VM Arbeitslasten in Azure zu arbeiten? <br/>
Um Azure Backup mit SCDPM verwenden, ist es ratsam SCDPM zunächst nur dann installieren und Azure Backup-Agent installieren. Dies sorgt für nahtlose Integration von Azure Backup-Agent mit SCDPM und schützen Dateien und Ordner-Arbeitslasten und VMs in Azure, direkt von der Verwaltungskonsole SCDPM. Installation SCDPM nach der Installation von Azure Backup Agent für die oben genannten Zwecke nicht empfohlen oder unterstützt.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent-br"></a>Was ist der Dateipfad als Bestandteil Azure Backup mit Azure Backup Agent angegeben werden? <br/>  
Azure Backup Agent verwendet NTFS. [Filepath Längenangabe von Windows-API beschränkt](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Bei Sicherungskopien Datei Pfad länger als die angegebenen Windows-API, können Kunden den Ordner oder das Laufwerk Sicherungsdateien sichern.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Zeichen im Pfad von Azure Backup-Richtlinie Azure Backup-Agent mit dürfen? <br>  
 Azure Backup Agent verwendet NTFS. Sie können als Teil der Dateiangabe [NTFS unterstützt Zeichen](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) .  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Können ich Azure Sicherungsserver Backups Bare Metal Recovery (BMR) für einen physischen Server? <br/>
Ja.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails-br"></a>Kann ich den Sicherungsdienst zum Senden von Nachrichten ein Sicherungsauftrag schlägt konfigurieren? <br/>
Ja, hat der Sicherungsdienst mehrere Ereignis-basierte Alarme, die PowerShell-Skript verwendet werden können. Eine vollständige Beschreibung finden Sie unter [Benachrichtigung](backup-azure-manage-vms.md#alert-notifications)

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Besteht eine Begrenzung der Größe der einzelnen Datenquellen gesichert? <br/>
Vault-Ebene gibt es keine Beschränkung der Datenmenge, die Sie sichern können, Azure Backup erlegt eine Einschränkung (praktisch, diese Grenzwerte sind sehr hoch) auf maximale Größe der Datenquelle. Ab August 2015 ist die maximale Größe Datenquelle unterstützte Betriebssysteme:

|S.No | Betriebssystem |  Maximale Größe der Datenquelle |
| :-------------: |:-------------| :-----|
|1| Windows Server 2012 oder höher| 54400 GB|
|2| Windows 8 oder höher| 54400 GB|
|3| WindowsServer 2008, Windows Server 2008 R2 | 1700 GB|
|4| Windows 7 | 1700 GB|

Die folgende Tabelle erläutert, wie jede Datengröße Quell bestimmt wird.

|   Datenquelle  |   Details |
| :-------------: |:-------------|
|Volume |Die Datenmenge aus einzelnen Server oder Client-Computer gesichert|
|Hyper-V-Computer | Summe der Daten die VHDs des virtuellen Computers gesichert|
|Microsoft SQL Server-Datenbank | Größe eines einzelnen SQL-Datenbank gesichert |
|Microsoft SharePoint |Summe der Inhalts- und Konfigurationsdatenbanken innerhalb einer Sharepointfarm gesichert|
|Microsoft Exchange |Summe aller Exchange-Datenbanken auf einem Exchange-Server gesichert|
|BMR-System Status |Jede Kopie der BMR oder System Status gesichert|

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-daybr"></a>Gibt es auf der Anzahl der Sicherungsauftrag pro Tag geplant werden kann?<br/>
Ja, Sie können Sicherungsaufträge auf Ausführen Windows Server oder Windows-Client bis zu dreimal täglich. Sie können Sicherungsaufträge auf System Center DPM bis zu zweimal täglich ausführen. Sie können einen Sicherungsauftrag für IaaS VMs täglich ausführen.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-ie-on-windows-server-without-dpm-br"></a>Gibt es einen Unterschied zwischen der Planung für DPM und Windows-Server (d. h. auf Windows Server ohne DPM)? <br/>
Ja. DPM können Sie täglich, wöchentlich, monatlich und jährlich Zeitpläne angeben. Windows-Server (ohne DPM) können Sie nur tägliche oder wöchentliche Zeitpläne angeben.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-ie-on-windows-server-without-dpmbr"></a>Gibt es einen Unterschied zwischen der Aufbewahrungsrichtlinie für DPM und Windows Server-Client (d. h. auf Windows Server ohne DPM)?<br/>
Keine, DPM und Windows Server-Client haben täglich, wöchentlich, monatlich und jährlich Aufbewahrungsrichtlinien.

## <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Kann ich meine Aufbewahrung, selektiv – d. h. wöchentlich konfigurieren, und täglich nicht jährlich und monatlich konfigurieren?<br/>
Ja, Azure Backup Aufbewahrung Struktur können Sie volle Flexibilität beim Definieren der Aufbewahrungsrichtlinie nach Ihren Wünschen.

## <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Kann "Planen einer Sicherung" 6 Uhr und angeben "Aufbewahrungsrichtlinien" zu einem anderen Zeitpunkt?<br/>
Nein. Aufbewahrungsrichtlinien können nur backup Punkte angewendet werden. In der folgenden Abbildung wird die Aufbewahrungsrichtlinie für Sicherungen 12 und 6 Uhr angegeben. <br/>

![Zeitplan-Backup und Archivierung](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Werden eine inkrementelle Kopie wird für geplanten Aufbewahrungsrichtlinien übertragen? <br/>
Nein, wird die inkrementelle Kopie anhand der Zeit auf der Seite Sicherungszeitplan gesendet. Punkte, die aufbewahrt werden, werden anhand der Aufbewahrungsrichtlinie.

## <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Wenn eine Sicherung für längere Zeit beibehalten wird, dauert es mehr Zeit für einen älteren Datenpunkt wiederherstellen? <br/>
 Nein – entspricht die Zeit wieder die älteste oder der neueste Punkt. Jedem Wiederherstellungspunkt verhält sich wie eine vollständige Point.

## <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Wenn jedem Wiederherstellungspunkt wie vollständige Point, wirkt es berechenbaren backup Gesamtspeicher?<br/>
Typisch langfristige Aufbewahrung Produkte wie vollständige backup Daten gespeichert. Die vollständige Punkte sind Speicher *ineffizient* jedoch einfacher und schneller wiederherstellen. Inkrementelle Kopien sind Speicher *effizient* , jedoch benötigen Sie eine Kette von Daten wiederherstellen, die Wiederherstellung beeinträchtigt. Azure Backup-Speicherarchitektur bietet das beste aus beiden Welten optimal Speichern von Daten für schnelle Wiederherstellung und Kosten geringer. Diese Speichermethode Daten gewährleistet, dass Ihre Ingress- und Egress-Bandbreite effizient genutzt werden. Der Daten-Speicherplatz und die Zeit zum Wiederherstellen der Daten wird auf ein Minimum beschränkt. Erfahren Sie mehr wie [inkrementelle Backups](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) zu speichern sind.

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Gibt es eine maximale Anzahl der Wiederherstellungspunkte erstellt werden kann?<br/>
Nein. Wir haben Grenzwerte für Wiederherstellungspunkte entfernt. Erstellen Sie so viele Wiederherstellungspunkte wie gewünscht.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-upbr"></a>Warum ist die Datenmenge in Sicherung entspricht nicht der Datenmenge übertragen gesicherten?<br/>
 Alle Azure Backup Agent oder SCDPM Azure Backup Server gesicherten Daten komprimiert und verschlüsselt übertragen. Nach der Komprimierung und Verschlüsselung angewendet wird, sind die Daten im backup-Tresor 30-40 % kleiner.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Gibt es eine Möglichkeit, die Bandbreite der Sicherungsdienst anzupassen?<br/>
 Ja, die Option **Eigenschaften ändern** in Backup Agent Bandbreite anpassen. Passen Sie die Bandbreite und Uhrzeiten, wenn die Bandbreite verwenden. Weitere Informationen finden Sie in der [Netzwerk-Drosselung](../backup-configure-vault.md#enable-network-throttling).

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure-br"></a>Internet-Bandbreite beschränkt Datenmenge sichern müssen. Gibt es eine Möglichkeit, die Daten an einem Ort mit einem umfangreichen Netzwerk leiten und schieben Sie die Daten in Azure bewegen? <br/>
Sie können Daten sichern in Azure über die Online-backup-Prozess, oder können Azure Import/Export-Dienst zur Datenübertragung BLOB-Speicher in Azure. Es gibt keine zusätzliche Wege Sicherungsdatum Azure-Speicher. Informationen zur Verwendung von Azure Import/Export-Dienst mit Azure Backup finden Sie im Artikel [Offline Backup-Workflow](backup-azure-backup-import-export.md) .

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Wie viele Recovery kann ich Daten durchführen, die in Azure gesichert?<br/>
Es gibt keine Beschränkung für die Anzahl der Wiederherstellung von Azure Backup.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveriesbr"></a>Müssen während der Wiederherstellung für ausgehenden Datenverkehr von Azure-Rechenzentrum bezahlen?<br/>
 Nein. Die Recovery sind und Sie Zahlen für ausgehenden Datenverkehr.

## <a name="is-the-data-sent-to-azure-encrypted-br"></a>Werden die Daten werden in Azure verschlüsselt gesendet? <br/>
Ja. Daten auf dem lokalen Client/Server/SCDPM Computer AES256 verwenden und die Daten über eine sichere HTTPS-Verbindung gesendet werden.

## <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Ist die Sicherungsdaten auf Azure sowie verschlüsselt<br/>
 Ja. Azure gesendeten Daten bleibt verschlüsselt (ruhende). Microsoft die Sicherungsdaten jederzeit nicht entschlüsseln. Azure VM Backup Azure Backup beruht auf Verschlüsselung des virtuellen Computers d.h. wenn die VM Azure Datenträgerverschlüsselung oder andere Verschlüsselung verschlüsselt ist, Azure Backup verschlüsselt, um Ihre Daten zu schützen.

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Was ist die minimale Länge des Verschlüsselungsschlüssel zum Verschlüsseln von Daten verwendet? <br/>
 Der Schlüssel sollte 16 Zeichen.

## <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Was geschieht, wenn ich den Schlüssel vergessen? Können die Daten wiederherstellen (oder) kann Microsoft die Daten wiederherstellen? <br/>
Der Schlüssel zum Verschlüsseln von backup-Daten ist nur für die Kunden vorhanden. Microsoft behält keine Kopie in Azure und keinen Zugriff auf den Schlüssel. Wenn der Kunde den Schlüssel verlegt, kann Microsoft backup Daten nicht wiederherstellen.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Ändern den Cachespeicherort für Azure Backup Agent angegeben<br/>
 Durchlaufen Sie nacheinander die Aufzählung unten, um den Speicherort zu ändern.
- Den Motor Sicherung durch den folgenden Befehl in ein Eingabeaufforderungsfenster mit erhöhten Rechten ausführen:

  ```PS C:\> Net stop obengine```

- Verschieben Sie die Dateien nicht. Kopieren Sie stattdessen Cacheordner Speicherplatz auf ein anderes Laufwerk mit ausreichend Speicherplatz. Ursprüngliche Cachespeicher kann nach Bestätigung der Sicherung mit den neuen Cache arbeiten entfernt werden.

- Aktualisieren Sie die folgenden Registrierungseinträge mit den Pfad für den neuen Speicherplatz Cacheordner.<br/>

|Pfad | Registrierungsschlüssel | Wert |
| ------ | ------- | ------|
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` | ScratchLocation | *Neue Speicherort des Cacheordners* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` | ScratchLocation | *Neue Speicherort des Cacheordners* |

- Starten Sie Backup-Engine durch den folgenden Befehl in ein Eingabeaufforderungsfenster mit erhöhten Rechten ausführen:

  ```PS C:\> Net start obengine```

  Nach die backup-Erstellung in den Speicherort des neuen erfolgreich, können Sie den ursprünglichen Cacheordner entfernen.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Wo kann den Cacheordner Azure Backup Agent erwartungsgemäß setzen?<br/>
Die folgenden Speicherorte für den Cacheordner werden nicht empfohlen:

- Netzwerk, Freigabe oder Wechselmedien: Cache-Ordners muss lokal auf dem Server, die mit der online-Sicherung sichern. Netzwerkadressen oder Wechseldatenträger wie USB-Laufwerke werden nicht unterstützt.
- Offline Volumes: Cache-Ordners muss für erwartete Sicherungskopie Azure Backup Agent online sein.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Sind alle Attribute den Cacheordner, die nicht unterstützt werden?<br/>
 Die folgenden Attribute oder deren Kombinationen sind für den Cacheordner nicht unterstützt:

- Verschlüsselt
- Dedupliziert
- Komprimiert
- Geringe Datendichte
- Analysepunkt

Es wird empfohlen, den Cacheordner weder die Metadaten VHD die Attribute über Azure Backup Agent erwartet funktioniert hat.
