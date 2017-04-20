<properties
    pageTitle="Blick: Schützen Azure VMs mit einem backup | Microsoft Azure"
    description="Schützen Sie Azure VMs mit Sicherung. Tutorial erklärt Tresors VMs registrieren, Richtlinie erstellen und VMs in Azure zu schützen."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/15/2016"
    ms.author="markgal; jimpark"/>


# <a name="first-look-backing-up-azure-virtual-machines"></a>Zunächst: Azure virtuelle Computer sichern

> [AZURE.SELECTOR]
- [Schützen von VMs mit einem Recovery services](backup-azure-vms-first-look-arm.md)
- [Schützen von Azure VMs mit einem backup](backup-azure-vms-first-look.md)

Dieses Lernprogramm führt Sie durch die Schritte zum Sichern einer Azure Virtual Machine (VM) backup Tresor in Azure. Dieser Artikel beschreibt die Klassisch oder Service Manager-Bereitstellungsmodell für VMs sichern. Wenn Sie in einer VM in ein Depot Recovery Services sichern möchten, die zu einer Ressourcengruppe gehört, sehen [zuerst: schützen VMs mit einem Recovery Services](backup-azure-vms-first-look-arm.md). Um dieses Lernprogramm erfolgreich abzuschließen, müssen Folgendes vorhanden sein:

- Sie haben eine VM in Azure-Abonnement.
- Die VM hat Konnektivität Azure öffentlicher IP-Adressen. Weitere Informationen finden Sie in der [Netzwerkkonnektivität](./backup-azure-vms-prepare.md#network-connectivity).

Zum Sichern einer VM sind fünf Schritte:  

![Schritt 1-](./media/backup-azure-vms-first-look/step-one.png) backup Depot erstellen oder identifizieren Sie eine vorhandene Sicherung Depot. <br/>
![Schritt zwei](./media/backup-azure-vms-first-look/step-two.png) der Azure-Verwaltungsportal zu entdecken und registrieren Sie die virtuellen Computer verwenden. <br/>
![Schritt 3-](./media/backup-azure-vms-first-look/step-three.png) den VM-Agent installieren. <br/>
![Schritt vier](./media/backup-azure-vms-first-look/step-four.png) die Richtlinie zum Schutz virtueller Computer erstellen. <br/>
![Schritt 5](./media/backup-azure-vms-first-look/step-five.png) führen Sie die Sicherung.

![Überblick über VM-Sicherung](./media/backup-azure-vms-first-look/backupazurevm-classic.png)

>[AZURE.NOTE] Azure hat zwei Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und Classic](../resource-manager-deployment-model.md). Dieses Lernprogramm ist für die Verwendung mit VMs in Azure-Verwaltungsportal erstellt werden können. Azure Backup-Dienst unterstützt VMs Ressourcenmanager basiert. Informationen zum Sichern von VMs in ein Depot Recovery Services finden Sie unter [First Look: schützen VMs mit einem Recovery Services](backup-azure-vms-first-look-arm.md).



## <a name="step-1---create-a-backup-vault-for-a-vm"></a>Schritt 1 - Erstellen eines backup-Depots für eine VM

Ein backup Vault ist eine Entität, die speichert alle Backup- und Recovery-Punkte, die mit der Zeit erstellt wurden. Backup Depot enthält auch backup-Policies, die den zu sichernden virtuellen Maschinen angewendet werden.

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://manage.windowsazure.com/)an.

2. Klicken Sie unten links im Azure-Portal **neu**

    ![Klicken Sie auf Neues Menü](./media/backup-azure-vms-first-look/new-button.png)

3. Klicken Sie im Assistenten zum schnellen Erstellen auf **Datendienste** > **Recovery Services** > **Sicherung Vault** > **Schnell erstellen**.

    ![Sicherungstresor erstellen](./media/backup-azure-vms-first-look/new-vault-wizard-one-subscription.png)

    Der Assistent fordert Sie für **Name** und **Region**. Wenn Sie mehrere Abonnements verwalten, erscheint ein Dialogfeld zum Auswählen des Abonnements.

4. **Name**Geben Sie einen Anzeigenamen zu Tresor. Der Name muss eindeutig für den Azure-Abonnement.

5. Wählen Sie im **Bereich**geografische Region für das Depot. **Muss** Vault ist im Bereich der virtuellen Computer schützt.

    Sollten Sie den Bereich nicht in dem VM vorhanden ist, schließen Sie den Assistenten und klicken Sie auf **virtuellen Computern** in der Liste der Azure-Dienste. Die Spalte enthält den Namen des Bereichs. Haben Sie virtuelle Computer in mehreren Regionen erstellen Sie ein backup Depot in jeder Region.

6. Liegt kein **Abonnement** -Dialogfeld des Assistenten, fahren Sie mit dem nächsten Schritt. Bei der Arbeit mit mehreren Abonnements wählen Sie ein Abonnement mit der neuen Sicherung zuordnen.

    ![Popupbenachrichtigung Depot erstellen](./media/backup-azure-vms-first-look/backup-vaultcreate.png)

7. Klicken Sie auf **Vault**. Es dauert eine Weile backup Depot erstellt werden. Überwachen Sie Status Notifications am unteren Rand des Portals.

    ![Popupbenachrichtigung Depot erstellen](./media/backup-azure-vms-first-look/create-vault-demo.png)

    Eine Meldung bestätigt, dass das Depot erfolgreich erstellt wurde. Es wird als **aktiv**auf **Recovery Services** aufgeführt.

    ![Popupbenachrichtigung Depot erstellen](./media/backup-azure-vms-first-look/create-vault-demo-success.png)

8. Wählen Sie in der Depots auf **Recovery-** Seite das Depot erstellt, um die Seite **Quick Start** starten.

    ![Liste der backup +++](./media/backup-azure-vms-first-look/active-vault-demo.png)

9. Der **Schnellstart** klicken Sie auf **Konfigurieren** , um die Option Storage Replication öffnen.
    ![Liste der backup +++](./media/backup-azure-vms-first-look/configure-storage.png)

10. Die Option **Storage-Replikation** die Option Replikation für Vault.

    ![Liste der backup +++](./media/backup-azure-vms-first-look/backup-vault-storage-options-border.png)

    Standardmäßig hat der Tresor Geo redundanten Speicher. Wählen Sie Geo-redundanten Speicher ist dies die primäre Sicherung aus Wählen Sie lokal redundanten Speicher, wenn Sie ein billiger, die sehr langlebig. Weitere Informationen über Geo-redundant und lokal redundanten Speicheroptionen in [Azure Storage Replication Overview](../storage/storage-redundancy.md).

Nach dem Auswählen der Speicheroption für Ihr Depot, können Sie die VM Tresor zuordnen. Die Zuordnung zunächst entdecken und Azure virtuelle Computer registrieren.

## <a name="step-2---discover-and-register-azure-virtual-machines"></a>Schritt 2: ermitteln und Azure registrieren-Computer
Führen Sie vor der Registrierung der VM mit einem Erkennungsprozess um neue VMs zu identifizieren. Dies gibt eine Liste von virtuellen Maschinen im Abonnement zusätzliche Informationen wie den Namen Cloud und der Region.

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://manage.windowsazure.com/)

2. Klicken Sie in der Azure-Verwaltungsportal auf **Recovery Services** Recovery Services +++ Liste öffnen.
    ![Wählen Sie Arbeitslast](./media/backup-azure-vms-first-look/recovery-services-icon.png)

3. Wählen Sie +++ Liste Depot, um einen virtuellen Computer zu sichern.

    Wenn Sie Ihrem Tresor auswählen, wird auf **Schnellstart**

4. Klicken Sie im Depot auf **Elemente registriert**.

    ![Wählen Sie Arbeitslast](./media/backup-azure-vms-first-look/configure-registered-items.png)

5. Wählen Sie im Menü **Typ** **Azure Virtual Machine**.

    ![Wählen Sie Arbeitslast](./media/backup-azure-vms/discovery-select-workload.png)

6. Klicken Sie auf **DISCOVER** am unteren Rand der Seite.
    ![Schaltfläche Suchen](./media/backup-azure-vms/discover-button-only.png)

    Der Erkennungsvorgang kann einige Minuten dauern virtuelle Computer aufgelistet sind. Es ist eine Benachrichtigung am unteren Bildschirmrand, die Sie informiert, dass der Prozess ausgeführt wird.

    ![Ermitteln von VMs](./media/backup-azure-vms/discovering-vms.png)

    Benachrichtigung geändert wird, wenn der Prozess abgeschlossen.

    ![Suche abgeschlossen](./media/backup-azure-vms-first-look/discovery-complete.png)

7. Klicken Sie unten auf der Seite **Registrieren** .
    ![Registrieren](./media/backup-azure-vms-first-look/register-icon.png)

8. Wählen Sie im Kontextmenü **Elemente registrieren** der virtuellen Computer, die Sie registrieren möchten.

    >[AZURE.TIP] Mehrere virtuelle Maschinen können gleichzeitig registriert werden.

    Ein Auftrag wird für jeden virtuellen Computer erstellt, die Sie ausgewählt haben.

9. Klicken Sie in der Benachrichtigung der Seite **Projekte** zu **Auftrag anzeigen** .

    ![Auftrag registrieren](./media/backup-azure-vms/register-create-job.png)

    Der virtuelle Computer wird auch in der Liste der registrierten Objekte zusammen mit dem Status des Vorgangs Registrierung.

    ![Status 1 Anmeldung](./media/backup-azure-vms/register-status01.png)

    Wenn der Vorgang abgeschlossen ist, ändert sich der *registrierte* Status widerspiegelt.

    ![Registrierungsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---install-the-vm-agent-on-the-virtual-machine"></a>Schritt 3: Installieren der VM-Agent auf dem virtuellen Computer

Der Azure-VM-Agent muss auf Azure Virtual Machine für die Backup-Erweiterung zu installiert. Wenn die VM aus dem Azure-Katalog erstellt wurde, ist VM-Agent bereits auf dem virtuellen Computer vorhanden. Sie können zum [Schutz der VMs](backup-azure-vms-first-look.md#step-4-protect-azure-virtual-machines)überspringen.

Wenn die VM aus einem lokalen Rechenzentrum migriert, wird die VM VM Agent installiert wahrscheinlich nicht. Sie müssen den VM-Agent auf dem virtuellen Computer zunächst die VM zu installieren. Ausführliche Anleitung zum Installieren des Agenten VM finden Sie unter [VM-Agent Abschnitt Backup VMs](backup-azure-vms-prepare.md#vm-agent).


## <a name="step-4---create-the-backup-policy"></a>Schritt 4 - die Sicherungsrichtlinie erstellen
Vor dem Auslösen des ursprünglichen Sicherungsauftrags Festlegen der bei backup-Snapshots. Der Zeitplan Sicherungssnapshots sind, und die Dauer dieser Momentaufnahmen bleiben ist die Sicherungsrichtlinie. Die Aufbewahrung Informationen basiert auf Großvater-Vater und Sohn Sicherungsrotationsplan aus.

1. Backup Depot unter **Recovery Services** in Azure-Verwaltungsportal navigieren Sie und auf **Elemente registriert**.
2. Wählen Sie im Dropdown-Menü **Azure Virtual Machine** .

    ![Wählen Sie im Portal Arbeitslast](./media/backup-azure-vms/select-workload.png)

3. Klicken Sie auf **Schutz** am unteren Rand der Seite.
    ![Klicken Sie auf schützen](./media/backup-azure-vms-first-look/protect-icon.png)

    **Elemente schützen-Assistent** wird angezeigt und listet *nur* virtuelle Computer, die registriert sind und nicht geschützt.

    ![Schutz Ebene konfigurieren](./media/backup-azure-vms/protect-at-scale.png)

4. Wählen Sie die virtuellen Computer, die Sie schützen möchten.

    Verwenden Sie zwei oder mehrere virtuelle Computer mit demselben Namen vorhanden sind, Cloud-Dienst die virtuellen Computer unterscheiden.

5. Wählen Sie eine vorhandene Richtlinie im **Schutz konfigurieren** oder Erstellen einer neuen Richtlinie zum Schutz der virtuellen Computer, die Sie identifiziert.

    Neue Backup-Depots haben eine Standardrichtlinie Tresor zugeordnet. Diese Richtlinie wird täglich abends snapshot und der Snapshot für 30 Tage beibehalten. Jede backup-Richtlinie können mehrere virtuelle Computer zugeordnet. Jedoch kann der virtuelle Computer nur eine Richtlinie gleichzeitig zugeordnet.

    ![Neue Richtlinie schützen](./media/backup-azure-vms/policy-schedule.png)

    >[AZURE.NOTE] Backup-Richtlinie umfasst eine Aufbewahrung für geplante Backups. Bei Auswahl eine vorhandene backup-Richtlinie wird im nächsten Schritt Aufbewahrung Optionen möglich.

6. **Beibehaltungsdauer** definiert den täglichen, wöchentlichen, monatlichen und jährlichen Bereich backup Punkte.

    ![Virtuelle Maschine mit Recovery unterstützt](./media/backup-azure-vms/long-term-retention.png)

    Aufbewahrungsrichtlinie gibt die Zeitspanne für das Speichern einer Sicherung. Sie können unterschiedliche Aufbewahrungsrichtlinien basierend darauf, wann die Sicherung ausgeführt wird.

7. Klicken Sie auf **Aufträge** **Konfigurieren** Schutzaufträge anzeigen.

    ![Konfigurieren der Schutzauftrag](./media/backup-azure-vms/protect-configureprotection.png)

    Nachdem die Richtlinie festgelegt haben, zum nächsten Schritt und führen anfängliche Backup.

## <a name="step-5---initial-backup"></a>Schritt 5: erste backup

Nach mit ein virtuellen Computer geschützt ist, können Sie diese Beziehung auf der Registerkarte **Geschützte Elemente** anzeigen. Bis der ersten Sicherung zeigt den **Schutzstatus** als **Protected - (ausstehende anfänglichen Backup)**. Standardmäßig ist die erste geplante Sicherung der *ersten Sicherung*.

![Sicherung ausstehend](./media/backup-azure-vms-first-look/protection-pending-border.png)

Die erste Sicherung jetzt starten:

1. Klicken Sie auf **Geschützte Elemente** auf **Jetzt sichern** am unteren Rand der Seite.
    ![Sicherung jetzt Symbol](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Der Azure-Sicherungsdienst erstellt einen Sicherungsauftrag für die anfängliche Sicherung.

2. Klicken Sie auf der Registerkarte **Aufträge** , um die Liste der Projekte anzuzeigen.

    ![Sicherung wird durchgeführt](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Nach Abschluss der ersten Sicherung ist der Status des virtuellen Computers auf der Registerkarte **Geschützte Elemente** *geschützt*.

    ![Virtuelle Maschine mit Recovery unterstützt](./media/backup-azure-vms/protect-backedupvm.png)

    >[AZURE.NOTE] Sichern virtueller Computer ist ein lokaler Prozess. Sie können nicht virtuelle Computer aus einem backup Tresor in anderen sichern. Für jede Azure Region mit VMs, die gesichert werden müssen, muss also mindestens backup Depot in diesem Bereich erstellt werden.

## <a name="next-steps"></a>Nächste Schritte
Da ein virtueller Computer erfolgreich gesichert wurden, sind mehrere Nächstes von Interesse sein könnten. Der logische Schritt ist Wiederherstellung der Daten einer VM vertraut. Allerdings gibt es Aufgaben, mit denen Sie verstehen, wie Ihre Daten schützen und Kosten.

- [Verwalten Sie und überwachen Sie die virtuellen Computer](backup-azure-manage-vms.md)
- [Virtuelle Computer wiederherstellen](backup-azure-restore-vms.md)
- [Hinweise zur Problembehandlung](backup-azure-vms-troubleshoot.md)


## <a name="questions"></a>Haben Sie Fragen?
Wenn Sie Fragen haben oder gibt es Funktion enthalten, angezeigt werden soll [uns Feedback senden](http://aka.ms/azurebackup_feedback).
