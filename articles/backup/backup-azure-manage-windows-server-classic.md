<properties
    pageTitle="Verwalten von Azure Backup Depots und Servern mit dem klassischen Bereitstellungsmodell Azure | Microsoft Azure"
    description="Verwenden Sie dieses Lernprogramm lernen Azure Backup Depots und Server verwalten."
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jimpark;markgal"/>


# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Verwalten von Azure Backup Depots und Servern mit dem klassischen Bereitstellungsmodell

> [AZURE.SELECTOR]
- [Ressourcen-Manager](backup-azure-manage-windows-server.md)
- [Classic](backup-azure-manage-windows-server-classic.md)

In diesem Artikel finden Sie eine Übersicht über die Sicherung Verwaltungsaufgaben klassischen Azure-Portal mit Microsoft Azure Backup Agent.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Bereitstellungsmodell.

## <a name="management-portal-tasks"></a>Portal-Management-Aufgaben
1. Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com).

2. Klicken Sie unter **Recovery Services**backup Depot zum Anzeigen der Seite Schnellstart klicken.

    ![Recovery-services](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Durch Auswählen der Optionen am oberen Rand der Seite Quick Start, sehen Sie die verfügbaren Verwaltungsaufgaben.

![Registerkarten verwalten](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Dashboard
Wählen Sie **Dashboard** Verwendung Übersicht für den Server. Die **Verwendung Übersicht** enthält:

- Registrierte Windows Server Cloud
- Die Anzahl der Azure Cloud geschützte virtuelle Maschinen
- Der Gesamtspeicher in Azure verwendet
- Der Status des aktuellen Aufträge

Am unteren Rand das Dashboard können Sie folgenden Aufgaben ausführen:

- **Zertifikat verwalten** – Wenn ein Zertifikat wurde der Server und das Zertifikat aktualisieren können. Verwenden Sie bei Verwendung von Vault Anmeldeinformationen nicht **Verwalten Zertifikat**.
- **Löschen** - löscht die aktuelle Sicherung Vault. Wenn ein backup Depot nicht mehr verwendet wird, können sie Speicherplatz freigeben löschen. **Löschen** ist nur verfügbar, nachdem alle registrierte Servern aus dem Tresor gelöscht wurden.

![Backup-Dashboard-Aufgaben](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Einschreiben
Wählen Sie **Elemente registriert** die Namen der Server anzeigen, die für diesen Tresor registriert.

![Einschreiben](./media/backup-azure-manage-windows-server-classic/registered-items.png)

Der **Typ** Filter standardmäßig auf Azure Virtual Machine. Wählen Sie zum Anzeigen der Namen der Server, die für diesen Tresor registriert **WindowsServer** aus dem Dropdown-Menü.

Hier können Sie die folgenden Aufgaben ausführen:

- **Allow Re-Registrierung** - Wenn diese Option für einen Server können Sie den **Registrier-Assistenten** im lokalen Microsoft Azure Backup Agent registrieren Sie den Server mit der Sicherung erneut. Möglicherweise aufgrund eines Fehlers im Zertifikat registrieren, oder wenn ein Server wiederhergestellt werden.
- **Löschen** - Löscht einen Server aus der Sicherung Tresor. Alle Daten des Servers wird sofort gelöscht.

    ![Einschreiben Aufgaben](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Geschützte Elemente
Wählen Sie **Geschützte Objekte** , Elemente anzuzeigen, die von den Servern gesichert wurden.

![Geschützte Elemente](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurieren

Auf der Registerkarte **Konfigurieren** können Sie die entsprechende Speicheroption Redundanz auswählen. Die beste Zeit um Redundanz Speicheroption auszuwählen ist richtig, nach dem Erstellen eines Depots und Computer registriert werden.

>[AZURE.WARNING] Sobald ein Depot registriert wurde, Redundanz Speicheroption ist gesperrt und kann nicht geändert werden.

![Konfigurieren](./media/backup-azure-manage-windows-server-classic/configure.png)

Weitere Informationen über [Speicherredundanz](../storage/storage-redundancy.md)anzeigen

## <a name="microsoft-azure-backup-agent-tasks"></a>Microsoft Azure Backup Agent Aufgaben

### <a name="console"></a>Konsole

Öffnen Sie den **Microsoft Azure Backup-Agent** (Sie finden sie nach dem Computer suchen *Microsoft Azure Backup*).

![Backup-agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Von rechts Netzwerksicherungsdienst Konsole verfügbaren **Aktionen** können Sie die folgenden Verwaltungsaufgaben ausführen:

- Server registrieren
- Sicherung planen
- Jetzt sichern
- Eigenschaften ändern

![Agent-Konsolen-Aktionen](./media/backup-azure-manage-windows-server-classic/console-actions.png)

>[AZURE.NOTE] **Wiederherstellen von Daten**finden Sie unter [Dateien wiederherstellen in einem WindowsServer oder Windows-Clientcomputer](backup-azure-restore-windows-server.md).

### <a name="modify-an-existing-backup"></a>Ändern Sie eine vorhandene Sicherung

1. Klicken Sie in der Microsoft Azure Backup-Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

2. Im **Zeitplan Assistenten** lassen Sie die **Änderungen backup oder Zeiten** Option und klicken Sie auf **Weiter**.

    ![Ändern einer geplanten Sicherung](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

3. Hinzufügen oder ändern, klicken Sie auf Elemente auf dem Bildschirm **Zu sichernde Elemente wählen Sie** **Elemente hinzufügen**.

    Sie können auch **Ausschluss** von dieser Seite im Assistenten eingestellt. Auszuschließende Dateien oder Dateitypen lesen Sie das Verfahren zum [Ausschluss Einstellungen](#exclusion-settings)hinzufügen.

4. Wählen Sie die Dateien und Ordner zu sichern, und klicken Sie auf **OK**.

    ![Hinzufügen von Elementen](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)

5. Geben Sie den **Sicherungszeitplan** , und klicken Sie auf **Weiter**.

    Sie können täglich (maximal 3 Mal pro Tag) oder wöchentliche Backups planen.

    ![Der Sicherungszeitplan angeben](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

    >[AZURE.NOTE] Angeben des Sicherungszeitplans wird in diesem [Artikel](backup-azure-backup-cloud-as-tape.md)erläutert.

6. Wählen Sie die **Aufbewahrungsrichtlinie** für die Sicherungskopie, und klicken Sie auf **Weiter**.

    ![Die Aufbewahrungsrichtlinie auswählen](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)

7. **Das Bestätigungsfenster** überprüfen Sie die Informationen und klicken Sie auf **Fertig stellen**.

8. Nachdem der Assistent den **Sicherungszeitplan**erstellen, klicken Sie auf **Schließen**.

    Nach dem Ändern der Schutz können Sie bestätigen, dass Backups korrekt auf der Registerkarte **Aufträge** und bestätigt, dass die Änderungen in der Sicherungsaufträge auslösen.

### <a name="enable-network-throttling"></a>Netzwerk-Drosselung aktivieren  
Azure Backup-Agent stellt eine Registerkarte Beschränkung, die können Sie steuern, wie Netzwerkbandbreite während der Datenübertragung verwendet wird. Dieses Steuerelement ist hilfreich, möchten Sie sichern Daten während der Arbeitszeit wollen aber nicht den Sicherungsvorgang beeinträchtigen andere Internetdatenverkehr. Beschränkung der Daten gilt für Übertragung sichern und Wiederherstellen von Aktivitäten.  

Drosselung aktivieren:

1. Klicken Sie im **Backup Agent**auf **Eigenschaften ändern**.

2. Aktivieren Sie das Kontrollkästchen **Internet-Bandbreite für Backups Einschränkungen aktivieren** .

    ![Netzwerk-Drosselung](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)

3. Wenn Sie die Einschränkung aktiviert haben Geben Sie zugelassene Bandbreite für die Übertragung von Daten während der **Arbeitszeit** und **nicht - Arbeitszeiten an**

    Die Bandbreite Werte können 512 Kilobyte pro Sekunde (Kbps) beginnen und bis 1023 MB pro Sekunde (Mbit/s). Sie kennzeichnen den Anfang und Ende **Arbeitsstunden**und die Tage der Woche gelten Arbeit Tage. Die Zeit außerhalb der festgelegten Arbeitszeiten gilt nicht als Arbeitszeit.

4. Klicken Sie auf **OK**.

## <a name="exclusion-settings"></a>Ausschluss-Einstellung

1. Öffnen Sie den **Microsoft Azure Backup-Agent** (Sie finden sie nach dem Computer suchen *Microsoft Azure Backup*).

    ![Offene backup-Agenten](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

2. Klicken Sie in der Microsoft Azure Backup-Agent auf **Sicherung planen**.

    ![Planen einer Windows Server-Sicherung](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)

3. Im Zeitplan Assistenten lassen Sie die **Änderungen backup oder Zeiten** Option und klicken Sie auf **Weiter**.

    ![Ändern eines Zeitplanes](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)

4. Klicken Sie auf **Ausschlüsse**.

    ![Auszuschließende Elemente](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)

5. Klicken Sie auf **Ausschluss hinzufügen**.

    ![Ausschlüsse hinzufügen](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)

6. Wählen Sie den Speicherort, und klicken Sie dann auf **OK**.

    ![Wählen Sie einen Speicherort für den Ausschluss](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)

7. Hinzufügen der Erweiterung im Feld **Dateityp** .

    ![Nach Dateityp ausschließen](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Hinzufügen einer MP3-Erweiterung

    ![Beispiel für Typ](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Fügen Sie eine andere Erweiterung **Ausschluss hinzufügen** und geben Sie einen anderen typerweiterung (Erweiterung .jpeg hinzufügen).

    ![Ein anderes Beispiel Typ](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)

8. Wenn Sie die Erweiterung hinzugefügt haben, klicken Sie auf **OK**.

9. Zeitplan Assistenten durch Klicken auf **Weiter** bis die **Bestätigungsseite**fahren Sie und dann auf **Fertig stellen**.

    ![Ausschluss-Bestätigung](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Nächste Schritte
- [WindowsServer oder Client Windows Azure wiederherstellen](backup-azure-restore-windows-server.md)
- Mehr über Azure Backup finden Sie unter [Übersicht über die Sicherung von Azure](backup-introduction-to-azure-backup.md)
- Besuchen Sie das [Forum Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)
