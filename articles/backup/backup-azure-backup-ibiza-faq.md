<properties
   pageTitle="Recovery Services FAQ Depot | Microsoft Azure"
   description="Diese Version der FAQ unterstützt die Public Preview-Version von Azure Backup Service. Antworten auf häufig gestellte Fragen zu den Netzwerksicherungsdienst, Sicherung und Aufbewahrung, Recovery, Sicherheit und andere häufig gestellte Fragen zur Azure Backup-Lösung."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="Backup-Lösung; Backup-service"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Recovery Services Vault - FAQ


Dieser Artikel enthält Informationen zu Recovery Services Depot und [Azure Backup FAQ](backup-azure-backup-faq.md)ergänzt. Azure Backup FAQ enthält den vollständigen Satz von Fragen und Antworten zum Azure Backup-Dienst.  

Sie können Fragen zu Azure Backup im Abschnitt Disqus dieser Artikel oder einen Artikel. Fragen zur Azure Backup Service buchen Sie im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Recovery sind Services Ressourcen-Manager basiert. Werden Backup-Depots (klassischen Modus) werden weiterhin unterstützt? <br/>
Ja, Sicherung Depots werden weiterhin unterstützt. Erstellen Sie Backup-Depots im [klassischen Portal](https://manage.windowsazure.com). Erstellen Sie Recovery Services Depots in [Azure-Portal](https://portal.azure.com). Jedoch wir empfehlen Ihnen dringend, alle zukünftigen verbesserte Recovery Services Depot erstellen im Depot Recovery Services sind verfügbar.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kann ich ein Depot Backup Recovery Services Tresor migrieren? <br/>
Leider kann nicht Nein, zurzeit den Inhalt der Sicherung Depot in ein Depot Recovery Services migrieren. Wir arbeiten auf diese Funktionalität hinzufügen, aber ist nicht verfügbar im Rahmen der Public Preview.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Unterstützen Depots Recovery Services die klassische VMs oder basierten Ressourcenmanager VMs? <br/>
Beide Modelle unterstützen Recovery Services Depots.  Im klassischen Portal (die klassischen VMs) erstellt einen virtuellen Computer sichern oder eine VM in Azure-Portal (die Ressourcen-Manager basiert) in ein Depot Recovery Services erstellt.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Meine klassische VMs backup Depot haben gesichert werden. Jetzt möchte ich meine VMs von klassischen Ressourcenmanager Modus migrieren.  Wie kann ich sie in Recovery Services Tresor sichern?
Backups von klassischen VMs backup Depot wird nicht automatisch Recovery Services Tresor migriert, bei der Migration von VMs von classic Ressourcen-Manager-Modus. Gehen Sie zur Migration von VM-Backups:

1. In backup Vault **Geschützte Objekte** Registerkarte und wählen Sie den virtuellen Computer. Klicken Sie auf [Schutz aufheben](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Lassen Sie *zugehörige backup löschen* Option **deaktiviert**.
2. Migrieren Sie den virtuellen Computer im klassischen Modus Ressourcen-Manager-Modus. Stellen Sie sicher, dass Speicher- und entsprechenden virtuellen Computer auch Ressourcenmanager Modus migriert werden.
3. Ein Depot Recovery Services erstellen und Konfigurieren von Backup auf die migrierten virtuellen Computer mit **Backup** -Aktion auf Vault-Dashboard. Erfahren Sie mehr zum [Backup Recovery Services Depot aktivieren](backup-azure-vms-first-look-arm.md)
