## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Mithilfe von Vault Anmeldeinformationen bei Azure Backup Service authentifizieren

Die lokale Server (Windows-Clients oder Windows Server oder Data Protection Manager-Server) muss mit einem backup authentifiziert werden, bevor es in Azure Daten sichern kann. Die Authentifizierung erfolgt über "Anmeldeinformationen vault". Das Konzept der Vault-Anmeldeinformationen ähnelt das Konzept des "Veröffentlichen Settings" eine der in Azure PowerShell verwendet wird.

### <a name="what-is-the-vault-credential-file"></a>Was ist der Anmeldeinformationsdatei Depot?

Die Datei Vault ist ein Zertifikat vom Portal für jede Sicherung Depot generiert. Das Portal lädt dann den öffentlichen Schlüssel an Access Control Service (ACS). Der private Schlüssel des Zertifikats wird dem Benutzer als Teil des Workflows zur Verfügung als Eingabe Registrierung Zustandsautomatworkflow eingeräumt. Dies authentifiziert die Maschine Sicherungsdaten identifizierten Tresor in Azure Backup Service senden.

Vault-Anmeldeinformationen ist nur während der Registrierung Workflow verwendet. Es obliegt dem Benutzer sicherstellen, dass die Datei Vault ist. Fällt in eine nicht autorisierte Benutzer, kann die Datei Vault sich andere Computer gegen dasselbe Depot verwendet werden. Backup-Daten verschlüsselt ist die Verwendung einer Passphrase Kunden gehört, können nicht jedoch vorhandene Sicherungsdaten beeinträchtigt. Um dieses Problem zu verringern, sind Vault Anmeldeinformationen in 48 Stunden Gültigkeitsdauer festgelegt. Mehrfach-Anmeldeinformationen Depot ein Depot backup herunterladen kann jedoch nur der aktuelle Depot Anmeldeinformationsdatei während des Registrierung Workflows.

### <a name="download-the-vault-credential-file"></a>Die Vault-Anmeldeinformationen herunterladen

Vault-Anmeldeinformationsdatei wird über einen sicheren Kanal von Azure-Portal heruntergeladen. Azure Backup-Dienst ist nicht der private Schlüssel des Zertifikats und des privaten Schlüssels wird in das Portal oder den Dienst. Gehen Sie Vault Anmeldeinformationsdatei auf einem lokalen Computer herunterzuladen.

1.  Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com/)
2.  Klicken Sie im linken Navigationsbereich auf **Recovery Services** und wählen Sie das Depot backup erstellt haben. Klicken Sie auf die Cloud auf Quick Start backup Vault abgerufen.

    ![Schnellansicht](./media/backup-download-credentials/quickview.png)

3.  Klicken Sie auf Schnellstart auf **Vault Anmeldeinformationen**. Generiert die Vault Anmeldeinformationsdatei zum Download zur Verfügung gestellt wird.

    ![Herunterladen](./media/backup-download-credentials/downloadvc.png)

4.  Das Portal generiert tresoranmeldeinformationen mit einer Kombination aus den Vault-Namen und das aktuelle Datum. Klicken Sie auf **Speichern** , um Vault Anmeldeinformationen in das lokale Konto Downloads Ordner herunterladen oder Menü speichern Speichern einen Speicherort für die Vault-Anmeldeinformationen an.

### <a name="note"></a>Hinweis
- Sicherstellen Sie, dass die Vault-Anmeldeinformationen an, die auf Ihrem Computer gespeichert. Wenn sie eine Datei freigeben/SMB gespeichert ist, für die Zugriffsberechtigungen überprüfen.
- Die Datei Depot wird nur während der Registrierung Workflow verwendet.
- Datei der Vault nach 48 Stunden abläuft und aus dem Portal heruntergeladen werden.
- Lesen der Azure Backup [FAQ](../articles/backup/backup-azure-backup-faq.md) Fragen im Workflow.
