<properties
    pageTitle="Migration von IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager-Plattform unterstützt | Microsoft Azure"
    description="Dieser Artikel führt durch Migration Plattform unterstützt Ressourcen von Classic an Azure-Ressourcen-Manager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Plattform unterstützt Migration IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager

In diesem Artikel beschreiben wir, wie wir Migration Infrastruktur als eine Ressourcen Service (IaaS) klassische Bereitstellungsmodelle für Ressourcen-Manager aktivieren. Erfahren Sie mehr über [Azure-Ressourcen-Manager-Funktionen und Vorteile](../azure-resource-manager/resource-group-overview.md). Erläutert, wie Verbindung Ressourcen zwei Modelle, die in Ihrem Abonnement Koexistenz mit virtuellen Netzwerkgateways zwischen Standorten. 

## <a name="goal-for-migration"></a>Ziel für die migration

Ressourcenmanager können komplexe Applikationen Vorlagen bereitstellen, virtuelle Computer mit VM-Erweiterungen konfiguriert und Zugriffsmanagement und Tags enthält. Azure Ressourcenmanager enthält skalierbare, parallele Bereitstellung für virtuelle Computer an Verfügbarkeit. Neues Bereitstellungsmodell bietet auch unabhängig Lifecycle Management von Computing-, Netzwerk- und Speicher. Schließlich gibt es einen Fokus auf Sicherheit standardmäßig mit der virtuellen Computer in einem virtuellen Netzwerk.

Fast alle Funktionen aus den klassischen Bereitstellungsmodell werden für Compute, Netzwerk und Speicher unter Azure-Ressourcen-Manager unterstützt. Um die neuen Funktionen in Azure-Ressourcen-Manager nutzen, können Sie vorhandene Installationen von klassischen Bereitstellungsmodell migrieren.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Ändert sich in die Automatisierung und Werkzeuge nach der migration

Als Teil der Migration Ressourcen von klassischen Bereitstellungsmodell zum Ressourcen-Manager-Bereitstellungsmodell müssen Sie Ihre vorhandene Automatisierung oder Werkzeuge, um sicherzustellen, dass sie weiterhin nach der Migration aktualisieren.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Bedeutung der Migration IaaS-Ressourcen von Classic an Ressourcen-Manager

Bevor wir die Details Drilldown, betrachten wir die Differenz zwischen Data-Plane- und Management-Ebene die IaaS-Ressourcen.

- *Verwaltungsebene* beschreibt die Aufrufe, die Managementebene oder der API für Ressourcen geändert werden. Vorgänge wie das Erstellen einer VM und Neustarten einer VM aktualisieren ein virtuelles Netzwerk mit ein neues Subnetz verwalten ausgeführten Ressourcen. Sie wirken sich nicht direkt auf Instanzen herstellen.
- *Datenebene* (Anwendung) beschreibt die Laufzeit der Anwendung und eine Interaktion mit Instanzen durchlaufen der Azure-API nicht. Zugriff auf die Website oder Daten von einer ausgeführten Instanz von SQL Server oder einem Server MongoDB wäre Daten Ebene oder Anwendung Interaktion. Einen Blob ein Speicherkonto kopieren und den Zugriff auf öffentliche IP-Adresse eines RDP oder SSH auf dem virtuellen Computer sind auch die Datenebene. Diese Vorgänge halten die Anwendung berechnen, Netzwerke und Speicher.

>[AZURE.NOTE] Die Azure-Plattform in einigen Migrationsszenarios stoppt, freigegeben und die virtuellen Computer neu gestartet. Dies verursacht eine kurze Datenebene Ausfallzeiten.

## <a name="supported-scopes-of-migration"></a>Bereiche der Migration werden unterstützt

Es gibt drei Migration Bereiche, die hauptsächlich Computing-, Netzwerk- und Speicher ausgerichtet. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migration virtueller Maschinen (nicht in einem virtuellen Netzwerk)

Sicherheit ist in der Ressourcen-Manager-Bereitstellungsmodell für die Anwendung standardmäßig durchgesetzt. Alle VMs müssen in einem virtuellen Netzwerk im Ressourcen-Manager-Modell. Neustart des Azure-Plattform (`Stop`, `Deallocate`, und `Start`) im Rahmen der Migration von VMs. Sie haben zwei Optionen für die virtuellen Netzwerke:

- Sie können die Plattform ein neues virtuelles Netzwerk erstellen und migrieren die virtuellen Computer in neues virtuelles Netzwerk anfordern.
- Sie können den virtuellen Computer in ein vorhandenes virtuelles Netzwerk Ressourcen-Manager migrieren.

>[AZURE.NOTE] In diesem Bereich Migration Operations Management-Ebene und Data-Plane-Operationen für eine Zeitspanne, während der Migration dürfen nicht.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migration virtueller Maschinen (in einem virtuellen Netzwerk)

Für die meisten VM Konfigurationen ist die Metadaten zwischen Classic und Ressourcenmanager Bereitstellungsmodelle migrieren. Die zugrunde liegende VMs werden auf derselben Hardware im gleichen Netzwerk und denselben Speicher ausgeführt. Operations Management-Ebene können für einen bestimmten Zeitraum während der Migration nicht zulässig. Jedoch weiterhin die Datenebene. D. h. fallen die Applikationen auf VMs (classic) Ausfallzeiten während der Migration.

Die folgenden Konfigurationen werden derzeit nicht unterstützt. Unterstützung in Zukunft einige VMs in hinzugefügt kann diese Konfiguration Ausfallzeiten entstehen (freigegeben durch Beenden und Neustart VM Operationen gehen).

-   Sie haben mehrere Verfügbarkeit in einem einzelnen Clouddienst festgelegt.
-   Sie haben Verfügbarkeit legt oder VMs, die eine Verfügbarkeit in einem einzelnen Clouddienst festgelegt sind.

>[AZURE.NOTE] In diesem Bereich Migration kann die Verwaltungsebene für einen Zeitraum während der Migration nicht zulässig. Bei bestimmten Konfigurationen wie zuvor beschrieben-Datenebene Ausfallzeit tritt

### <a name="storage-accounts-migration"></a>Konten Speichermigration

Um nahtlose Migration zu ermöglichen, können Sie in einem klassischen Speicherkonto Ressourcenmanager VMs bereitstellen. Mit dieser Funktion berechnen und Netzwerkressourcen können und unabhängig von Speicher-Konten migriert werden sollte. Wenn Sie virtuelle Computer und virtuelles Netzwerk migrieren, müssen Sie über Speicherkonten bis zum Abschluss der Migration migrieren. 

>[AZURE.NOTE] Das Ressourcen-Manager-Bereitstellungsmodell keinen das Konzept der klassischen Bilder und Festplatten. Wenn das Speicherkonto migrierten klassische Bilder und sind nicht sichtbar im Ressourcenmanager Stapel aber Unterstützung VHDs im Speicherkonto bleiben. 

## <a name="unsupported-features-and-configurations"></a>Nicht unterstützte Features und Konfigurationen

Wir derzeit einige Features und Konfigurationen nicht. Die folgenden Abschnitte beschreiben Unsere Empfehlung herum.

### <a name="unsupported-features"></a>Nicht unterstützte Funktionen

Die folgenden Funktionen werden derzeit nicht unterstützt. Sie können optional entfernen diese Einstellungen migrieren VMs und aktivieren die Einstellungen in der Ressourcen-Manager-Bereitstellungsmodell.

Ressourcenproviders | Funktion
---------- | ------------
Berechnen | Virtuellen Festplatten.
Berechnen | Images von virtuellen Maschinen.
Netzwerk | Endpunkt ACLs.
Netzwerk | Virtuelles Netzwerk-Gateways (Standort zu Standort Azure ExpressRoute Application Gateway zeigen auf Website).
Netzwerk | Virtuelle Netzwerke mit VNet Peering (VNet, ARM migrieren und peer) Weitere Informationen zu [VNet Peering] (... /Virtual-Network/Virtual-Network-Peering-Overview.MD).
Netzwerk | Traffic Manager Profile.

### <a name="unsupported-configurations"></a>Nicht unterstützte Konfigurationen

Die folgenden Konfigurationen werden derzeit nicht unterstützt.

Dienst | Konfiguration | Empfehlung
---------- | ------------ | ------------
Ressourcen-Manager | Rolle basiert Zugang Steuerelement (RBAC) für klassische Ressourcen | Da der URI der Ressourcen nach der Migration geändert wird, sollten RBAC Richtlinie Updates planen, die nach der Migration auftreten.
Berechnen | Mehrere Subnetze eine VM zugeordnet | Aktualisieren Sie die Subnetzkonfiguration nur Subnetze auf.
Berechnen | Virtuelle Computer, die Teil eines virtuellen Netzwerks keine explizite Subnetz zugewiesen | Sie können optional die VM.
Berechnen | Virtuelle Computer, die Warnungen, skalieren Richtlinien | Die Migration durch und diese Einstellung gelöscht. Bewerten der Umgebung vor der Migration wird dringend empfohlen. Alternativ können Sie die Einstellungen nach Abschluss der Migration neu konfigurieren.
Berechnen | XML VM Extensions (BGInfo 1. * ist, Visual Studio-Debugger Web Deploy und Remotedebuggen) | Dies wird nicht unterstützt. Es wird empfohlen, den virtuellen Computer Migration weiterhin diese Erweiterung entfernen oder sie während des Migrationsvorgangs automatisch gelöscht werden werden.
Berechnen | Boot-Diagnose mit Premium-Speicher | Deaktivieren Sie Boot-Diagnosefunktion für VMs mit der Migration fortfahren. Nachdem die Migration abgeschlossen ist, können Sie Boot Diagnose im Ressourcenmanager Stapel wieder aktivieren. Darüber hinaus sollten Screenshot und serielle Protokolle verwendeten Blobs gelöscht nicht mehr für die Blobs erhoben werden.
Berechnen | Cloud-Dienste, die Web-Worker-Rollen enthalten | Dies wird derzeit nicht unterstützt.
Netzwerk | Virtuelle Netzwerke, die virtuellen Maschinen und Web-Worker-Rollen enthalten |  Dies wird derzeit nicht unterstützt.
Azure App Service | Virtuelle Netzwerke, die App Umgebungen enthalten | Dies wird derzeit nicht unterstützt.
Azure HDInsight | Virtuelle Netzwerke, die HDInsight Dienste enthalten | Dies wird derzeit nicht unterstützt.
Microsoft Dynamics-Lebenszyklus | Virtuelle Netzwerke mit virtuellen Computern, die von Dynamics Lifecycle Services verwaltet werden | Dies wird derzeit nicht unterstützt.
Berechnen | Azure Security Center Extensions eine vnet, die eine VPN-Gateway oder ER Gateway auf Prem DNS-Server | Azure Security Center installiert Extensions automatisch auf die virtuellen Computer überwachen ihre Sicherheit und Warnungen. Diese Erweiterung in der Regel automatisch installiert wenn Azure Security Center-Richtlinie für das Abonnement aktiviert ist. Gateway-Migration wird derzeit nicht unterstützt und das Gateway muss vor die Migration Commit gelöscht wird Internetzugang zu VM Speicherkonto verloren Gateway gelöscht wird. Die Migration wird nicht fortgesetzt, geschieht dies als Gast Agent Status Blob aufgefüllt werden kann. Es wird empfohlen, Azure Security Center-Richtlinie für das Abonnement 3 Stunden vor dem Fortsetzen der Migration deaktiviert.

## <a name="the-migration-experience"></a>Migrationserfahrung

Vor der migrationserfahrung wird Folgendes empfohlen:

- Sicherstellen Sie, dass die Ressourcen, die Sie migrieren möchten nicht unterstützten Features oder Konfigurationen nicht. In der Regel die Plattform erkennt diese Probleme und generiert einen Fehler.
- Haben Sie virtuelle Computer, die nicht in einem virtuellen Netzwerk, werden sie beendet und als Teil der Vorbereitung freigegeben. Möchten Sie die öffentliche IP-Adresse verliert, untersuchen Sie reservieren die IP-Adresse vor dem Auslösen der Vorbereitung. Allerdings sind die virtuellen Computer in einem virtuellen Netzwerk, sind sie nicht beendet und freigegeben.
- Planen der Migration nicht Geschäftszeiten für unerwartete Ausfälle werden, der während der Migration auftreten.
- Downloaden Sie die aktuelle Konfiguration Ihrer virtuellen Computer mithilfe von PowerShell, Befehlszeilenschnittstelle (CLI) Befehle oder anderen APIs erleichtert für die Überprüfung nach Abschluss der Vorbereitung Schritt.
- Aktualisieren der Skripts Automation-operationalisierung Ressourcenmanager Bereitstellungsmodell behandeln, bevor Sie die Migration starten. Optional kann GET-Operationen Ressourcen Status vorbereitet sind.
- Bewerten Sie RBAC-Richtlinien, die die klassischen IaaS-Ressourcen konfiguriert sind und nach Abschluss die Migration planen.

Migrationsworkflow ist wie folgt

![Screenshot, der Migrationsworkflow anzeigt](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Die in den folgenden Abschnitten beschriebenen Vorgänge sind Idempotent. Haben Sie ein Problem nicht unterstützt oder Konfigurationsfehler Es empfiehlt wiederholen vorbereiten, Abbruch oder commit Vorgang. Die Azure-Plattform versucht es erneut.

### <a name="validate"></a>Überprüfen

Validate-Vorgang ist der erste Schritt im Migrationsprozess. Dieser Schritt soll Analysieren von Daten im Hintergrund für die Ressourcen migriert und Erfolg/Fehler zurück, wenn die Ressourcen Migration können.

Wählen Sie das virtuelle Netzwerk oder gehosteten Dienst (Wenn sie kein virtuelles Netzwerk) für die Migration überprüft werden soll.

* Wenn die Ressource nicht zur Migration ist, listet die Azure-Plattform die Gründe warum es nicht für die Migration unterstützt wird.

### <a name="prepare"></a>Vorbereiten

Die Vorbereitung ist der zweite Schritt im Migrationsprozess. Das Ziel dieses Schritts ist simulieren die Transformation IaaS-Ressourcen von Classic Ressourcenmanager Ressourcen und nebeneinander für Sie darstellen.

Wählen Sie das virtuelle Netzwerk oder gehosteten Dienst (Wenn sie kein virtuelles Netzwerk) für die Migration vorbereiten möchten.

* Ist die Ressource nicht zur Migration die Azure-Plattform den Migrationsprozess beendet und listet den Grund, warum die Vorbereitung ist fehlgeschlagen.
* Ist die Ressource zur Migration unten die ersten Azure-Plattform Sperren Operations Management-Ebene für die Ressourcen migriert. Sie können z. B. nicht VM migriert Datenträger hinzufügen.

Azure-Plattform startet dann die Migration von Metadaten von Classic Resource Manager Migrieren von Ressourcen.

Nach Abschluss die Vorbereitung Sie können Ressourcen in beiden Classic visualisieren und Ressourcen-Manager. Für jede der klassischen Bereitstellungsmodell-Clouddienst Azure-Plattform erstellt einen Ressourcennamen Gruppe, die das Muster `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtuelle Computer, die nicht in einem klassischen virtuellen Netzwerk sind in dieser Phase der Migration freigegeben beendet.

### <a name="check-manual-or-scripted"></a>Kontrollkästchen (manuell oder Skript)

Im Schritt Kontrollkästchen können optional Sie die Konfiguration, die Sie zuvor heruntergeladen haben um zu überprüfen, ob die Migration richtig ist. Alternativ können Sie das Portal und Kontrolle anmelden Eigenschaften und Ressourcen zu überprüfen, ob die Metadatenmigration gut aussieht.

Wenn Sie ein virtuelles Netzwerk migrieren, ist den meisten Konfiguration virtueller Computer nicht neu gestartet. Anwendung auf die VMs können Sie überprüfen, dass die Anwendung weiterhin ausgeführt wird.

Sie können die Überwachung/Automation und betriebsbezogenen Skripts, wenn VMs erwartungsgemäß arbeiten und aktualisierte Skripts ordnungsgemäß testen. Nur GET-Vorgänge werden unterstützt, wenn die Ressourcen Status vorbereitet sind.

Es ist kein Set Time Fenster vor dem commit der Migrations müssen. Nehmen Sie so lange in diesem Zustand werden soll. Managementebene ist jedoch für diese Ressourcen gesperrt, bis Sie Abbruch oder commit.

Wenn Probleme finden können Sie die Migration Abbrechen und zurück zur klassischen Bereitstellungsmodell. Nach zurückzukehren, öffnen die Azure-Plattform Operations Management-Ebene der Ressourcen, sodass Vorgänge auf die VMs im klassischen Bereitstellungsmodell fortgesetzt werden kann.

### <a name="abort"></a>Abbrechen

Abbruch ist optional, die die Änderung der klassischen Bereitstellungsmodell und Beenden der Migrations verwenden können.

>[AZURE.NOTE] Dieser Vorgang kann nicht ausgeführt werden, nachdem der Commitvorgang ausgelöst wurden.  

### <a name="commit"></a>Commit

Nach der Validierung können Sie die Migration übertragen. Ressourcen werden nicht mehr in Classic und stehen nur in der Ressourcen-Manager-Bereitstellungsmodell. Die migrierten Ressourcen können nur in das neue Portal verwaltet werden.

>[AZURE.NOTE] Dies ist eine idempotente Operation. Andernfalls wird empfohlen, dass Sie den Vorgang wiederholen. Wenn es fehlschlägt weiterhin, erstellen Sie Support-Ticket zu, oder erstellen Sie einen Forumsbeitrag [VM Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)mit einem ClassicIaaSMigration.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wirkt diese Migrationsplan sich meine vorhandene Dienste oder Anträge auf Azure virtuelle Maschinen?**

Nein. VMs (classic) sind im allgemeinen Verfügbarkeit vollständig unterstützt. Sie können diese Ressourcen verwenden, um die Expansion auf Microsoft Azure weiterhin.

**Was geschieht mit meiner VMs, wenn ich nicht in naher Zukunft migrieren?**

Wir sind nicht die vorhandenen klassischen APIs und Ressourcenmodell veralteter. Wir möchten erleichtern Migration, berücksichtigen die erweiterten Features, die in den Ressourcen-Manager-Bereitstellungsmodell. Wir empfehlen, [einige der Fortschritte](virtual-machines-windows-compare-deployment-models.md) , die Teil des IaaS unter Ressourcen-Manager überprüfen.

**Was bedeutet diese Migrationsplan für meine vorhandene Tools?**

Aktualisieren Ihrer Werkzeuge zum Ressourcen-Manager-Bereitstellungsmodell ist eines der wichtigsten Änderungen, die Sie in die Planung der Migration berücksichtigt.

**Wie lange werden Ausfallzeiten Management-Ebene?**

Es hängt von der Anzahl der Ressourcen, die migriert werden. Für kleinere Installationen (wenige VMs) sollte die gesamte Migration weniger als eine Stunde. Für umfangreiche Bereitstellungen (Hunderte von VMs) kann die Migration einige Stunden dauern.

**Kann ich Rollen nach Migrieren von Ressourcen in Ressourcen-Manager einsetzen?**

Als Ressourcen im Status vorbereitet sind, können Sie die Migration Abbrechen. Rollback wird nicht unterstützt, wenn Ressourcen über den Commitvorgang erfolgreich migriert wurden.

**Kann ich meine Migration einen Rollback der Commitvorgang schlägt?**

Migration kann nicht abgebrochen werden, wenn der Commitvorgang fehlschlägt. Alle Migrationsvorgänge einschließlich der Commitvorgang sind Idempotent. Wir empfehlen den Vorgang nach einer kurzen Zeit zu wiederholen. Wenn weiterhin Fehler auftreten, erstellen ein Support-Ticket oder einen Forumsbeitrag [VM Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)mit dem ClassicIaaSMigration.

**Muss ein anderes Schnellstraße Circuit kaufen IaaS unter Ressourcen-Manager verwendet werden?**

Nein. Wir aktiviert kürzlich [ExpressRoute Stromkreise in der Standardansicht auf der Ressourcen-Manager-Bereitstellungsmodell verschieben](../expressroute/expressroute-move.md). Sie müssen neue ExpressRoute-Verbindung kaufen, wenn Sie bereits haben.

**Was passiert, wenn ich Role-Based Access Control Richtlinien für meine klassischen IaaS-Ressourcen konfiguriert haben?**

Während der Migration transformiert die Ressourcen von Classic an Ressourcen-Manager. So empfiehlt RBAC Richtlinie Updates planen, die nach der Migration auftreten.

**Wenn Azure Site Recovery oder Azure Backup heute verwende ich?**

Migrieren Sie den virtuellen Computer, die für backup, siehe [aktiviert haben ich meine klassische VMs backup Depot gesichert. Jetzt möchte ich meine VMs von klassischen Ressourcenmanager Modus migrieren. Wie kann ich sichern sie in Recovery Services Tresor?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Kann ich mein Abonnement oder Ressourcen sehen, wenn sie zur Migration überprüfen?**

Ja. In die Plattform unterstützt Migrationsoption ist der erste Schritt in Vorbereitung der Migration zu überprüfen, ob die Ressourcen zur Migration sind. Bei der Validate-Vorgang fehlschlägt, erhalten Sie Nachrichten Gründen, die die Migration abgeschlossen werden kann.

**Was geschieht, wenn ich ein Kontingent Fehler ausführen, während die IaaS-Ressourcen für die Migration vorbereiten?**

Wir empfehlen die Migration Abbrechen und melden Sie eine Supportanfrage Kontingente im Bereich erhöhen, Migration von VMs. Genehmigte kontingentanforderung können Sie starten, Migrationsschritte erneut ausführen.

**Wie melde ich ein Problem?**

Buchen Sie Ihre Probleme und Fragen zur Migration zu [VM Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows)mit dem Schlüsselwort ClassicIaaSMigration. Sollten Ihre Fragen in diesem Forum veröffentlichen. Wenn Sie einen Supportvertrag haben, können Sie sich eine Support-Ticket.

**Wenn nicht die Namen der Ressourcen?, die während der Migration die Plattform ausgewählt haben**

Alle Ressourcen, die Sie explizit im klassischen Bereitstellungsmodell enthalten werden während der Migration beibehalten. In einigen Fällen werden neue Ressourcen erstellt. Beispiel: eine Netzwerkschnittstelle für jede VM erstellt. Steuern die Namen von Ressourcen während der Migration erstellt unterstützt nicht derzeit. Protokollieren Sie die stimmen für dieses Feature in [Azure Bewertungsportal](http://feedback.azure.com)

* *ich erhalte die Fehlermeldung *"VM meldet den Gesamtstatus der Agent nicht bereit. Daher kann die VM migriert werden. Stellen Sie sicher, dass VM-Agent Agent Gesamtstatus fertig meldet"* oder *"VM Erweiterung enthält, deren Status nicht von der VM gemeldet. Damit diese VM kann nicht migriert werden."***

Diese Nachricht empfangen wird, wenn die VM keinen ausgehenden Internet-Verbindung. VM-Agent verwendet ausgehende Verbindung zu Konto Azure-Speicher für den Agent-Status alle fünf Minuten aktualisiert.


## <a name="next-steps"></a>Nächste Schritte
Jetzt wissen die Migration der klassischen IaaS-Ressourcen zum Ressourcen-Manager, beginnen Sie migrieren von Ressourcen.

- [Technische detaillierte technische Informationen zur Plattform unterstützt Migration von Classic an Azure-Ressourcen-Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [Mithilfe von PowerShell IaaS-Ressourcen Classic zu Azure Resource Manager migrieren](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [Verwenden Sie CLI Migration IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Klonen einer klassischen VM an Azure-Ressourcen-Manager mithilfe Gemeinschaft PowerShell-Skripts](virtual-machines-windows-migration-scripts.md)
