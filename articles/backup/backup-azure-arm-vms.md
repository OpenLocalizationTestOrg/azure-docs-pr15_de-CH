<properties
    pageTitle="Sichern von Azure VMs in ein Depot Recovery Services | Microsoft Azure"
    description="Ermitteln, registrieren und Azure VMs Recovery Services Tresor mit diesen Verfahren Azure Virtual Machine Backup sichern."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""
    keywords="Backups virtueller Maschinen; Sichern Sie die virtuellen Computer. Backup- und Disaster Recovery; ARM Vm backup"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="trinadhk; jimpark; markgal;"/>


# <a name="back-up-azure-vms-to-a-recovery-services-vault"></a>Sichern von Azure VMs in ein Depot Recovery Services

> [AZURE.SELECTOR]
- [Sichern von VMs Recovery Services vault](backup-azure-arm-vms.md)
- [Sichern von VMs Backup vault](backup-azure-vms.md)

Dieser Artikel enthält das Verfahren zum Sichern von Azure VMs (Ressourcen-Manager bereitgestellt und Classic bereitgestellt) in ein Depot Recovery Services. Der Großteil der Arbeit Sichern virtueller Computer wird der Zubereitung. Vor dem Sichern oder einen virtuellen Computer zu schützen, müssen Sie [Komponenten](backup-azure-arm-vms-prepare.md) zur Vorbereitung Ihrer Umgebung zum Schutz Ihrer virtuellen Computer. Nach Abschluss die erforderlichen Komponenten können Sie Backup-Betrieb Ihrer VM Momentaufnahmen initiieren.

>[AZURE.NOTE] Azure hat zwei Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und Classic](../resource-manager-deployment-model.md). Sie können Ressourcen-Manager bereitgestellte VMs und klassischen VMs mit Recovery Services schützen. Informationen zum Arbeiten mit klassischen Bereitstellungsmodell VMs finden Sie unter [Azure virtuelle Computer sichern](backup-azure-vms.md) .

Weitere Informationen finden Sie in den Artikeln [Planen der VM-backup-Infrastruktur in Azure](backup-azure-vms-introduction.md) und [Azure virtuelle Computer](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-back-up-job"></a>Den Sicherungsauftrag auslösen

Sichern mit Vault Recovery Services definiert, wie oft und wann der Sicherungsvorgang ausgeführt wird. Standardmäßig ist die erste geplante Sicherung des ersten Backups. Bis der ersten Sicherung zeigt den Status des letzten Backups auf der **Sicherungsaufträge** als **Warnung (erste Backup ausstehend)**.

![Sicherung ausstehend](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Wenn die erste Sicherung bald beginnen soll, sollten **Jetzt sichern**ausführen. Das folgende Verfahren beginnt Vault-Dashboard. Dieses Verfahren dient zum Ausführen des ersten Sicherungsauftrags nach Abschluss aller erforderlichen Komponenten. Wenn der anfängliche Sicherungsauftrag bereits ausgeführt wurde, ist dieses Verfahren nicht verfügbar. Die zugeordnete Sicherungsrichtlinie bestimmt den nächsten Sicherungsauftrag.  

Den anfänglichen Sicherungsauftrag ausführen:

1. Klicken Sie im Schaltpult Depot auf der Kachel **Backup** auf **Azure Virtual Machines**. <br/>
    ![Symbol](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    Das **Sicherung Elemente** Blatt wird geöffnet.

2. Blade **Sicherung Elemente** Maustaste auf Vault sichern möchten, und klicken Sie auf **Jetzt sichern**.

    ![Symbol](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Der Sicherungsauftrag wird ausgelöst. <br/>

    ![Sicherungsauftrag ausgelöst](./media/backup-azure-vms-first-look-arm/backup-triggered.png)

3. Die erste Sicherung im Schaltpult Depot auf der Kachel **Sicherungsaufträge** abgeschlossen hat klicken Sie **Azure virtuelle Computer**.

    ![Sicherungsaufträge nebeneinander](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Backup-Jobs-Blatt wird geöffnet.

4. **Backup-Jobs** Blatt sehen Sie den Status aller Projekte.

    ![Sicherungsaufträge nebeneinander](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

    >[AZURE.NOTE] Als Teil des Sicherungsvorgangs Befehl Azure Backup Service ein backup Erweiterung auf jedem virtuellen Computer alle Schreibvorgänge und eine konsistente Momentaufnahme.

    Nach Abschluss des Sicherungsauftrags lautet der Status *abgeschlossen*.


## <a name="troubleshooting-errors"></a>Problembehandlung bei Fehlern
Wenn Sie Probleme beim Sichern Ihrer virtuellen Maschine ausführen, finden Sie [Artikel zur Problembehandlung VM](backup-azure-vms-troubleshoot.md) zu.

## <a name="next-steps"></a>Nächste Schritte

Jetzt geschützt VM überprüfen die folgenden Artikel zusätzliche Aufgaben und Ihre VMs VMs wiederherstellen kann.

- [Verwalten Sie und überwachen Sie die virtuellen Computer](backup-azure-manage-vms.md)
- [Virtuelle Computer wiederherstellen](backup-azure-arm-restore-vms.md)
