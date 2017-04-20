## <a name="download-install-and-register-the-azure-backup-agent"></a>Herunterladen, installieren und Registrieren des Azure Backup-Agents

Nach dem Erstellen des Azure Backup-Tresors, sollte ein Agent auf jedem Windows-Computer (Windows Server, Windows-Client, System Center Data Protection Manager-Server oder Azure Backup-Server-Computer), das Sichern von Daten und Anwendung in Azure ermöglicht installiert werden.

1. Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com/)

2. **Recovery-Services**klicken Sie backup Tresor mit einem Server registrieren möchten. Der Schnellstart für dieses backup Depot angezeigt.

    ![Schnellstart](./media/backup-install-agent/quickstart.png)

3. Klicken Sie auf der Seite Schnellstart **für Windows Server System Center Data Protection Manager oder Windows-Client** -Option unter **Agent herunterladen**. Klicken Sie auf **Speichern** , um es auf den lokalen Computer kopieren.

    ![Agenten speichern](./media/backup-install-agent/agent.png)

4. Sobald der Agent Doppelklick MARSAgentInstaller.exe Installation von Azure Backup-Agent starten. Wählen Sie den Installationsordner Ablageordner für den Agenten erforderlich. Speicherort des angegebenen benötigen Speicherplatz mindestens 5 % der backup-Daten.

5.  Verwenden Sie einen Proxy-Server für die Verbindung mit dem Internet im Bildschirm **Proxy-Konfiguration** Geben Sie die Proxy Server. Verwenden Sie einen authentifizierten Proxy Geben Sie Benutzerinformationen und Kennwort in diesem.

6.  Azure Backup Agent installiert.NET Framework 4.5 und Windows PowerShell (falls es nicht bereits vorliegt) um die Installation abzuschließen.

7.  Sobald der Agent installiert ist, klicken Sie **zur Registrierung** der Workflow fortgesetzt.

    ![Registrieren](./media/backup-install-agent/register.png)

8. Bildschirm Anmeldeinformationen Vault zu suchen und Auswählen der Datei Depot die zuvor heruntergeladen wurde.

    ![Vault-Anmeldeinformationen](./media/backup-install-agent/vc.png)

    Die Datei Vault gilt nur für 48 Std. (nachdem das Portal heruntergeladen wurde). Wenn Sie alle in diesem Bildschirm (z.B. "Vault Datei abgelaufen") bereitgestellt, bei Azure-Portal Fehler und Vault Anmeldeinformationen erneut herunterladen.

    Sicherstellen Sie, dass die Datei Vault verfügbar an einem Ort die Setup-Anwendung zugegriffen werden kann. Sollten Sie Fehler Zugriff die Depot Datei in ein temporäres Verzeichnis auf diesem Computer kopieren und wiederholen Sie den Vorgang.

    Wenn eine ungültige Depot Anmeldeinformationen (z. B. "Ungültige Depot Anmeldeinformationen") Fehlermeldung die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen zugeordnet Recovery Service. Versuchen Sie nach dem Download eine neue Vault-Anmeldeinformationen über das Portal. Dieser Fehler tritt in der Regel die Option **Download Depot Anmeldeinformationen** in Azure-Portal hintereinander klickt der Benutzer. In diesem Fall ist nur der zweite Depot Anmeldeinformationsdatei gültig.

9. Im Bildschirm **Verschlüsselung** generieren eine Passphrase oder ein Kennwort (mindestens 16 Zeichen) angeben. Denken Sie daran, das Kennwort an einem sicheren Ort speichern.

    ![Verschlüsselung](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Wenn das Kennwort verloren oder vergessen. Hilfe kann nicht in der backup-Daten wiederherstellen. Der Benutzer besitzt die verschlüsselungspassphrase und Microsoft keinen Einblick in die Passphrase vom Endbenutzer verwendet. Speichern Sie die Datei an einem sicheren Ort wie bei einem Wiederherstellungsvorgang erforderlich ist.

10. Sobald Sie die Schaltfläche **Fertig stellen** klicken, der Computer erfolgreich Depot registriert und können nun Microsoft Azure Backup starten.

11. Beim eigenständigen Microsoft Azure Backup durch Klicken auf die Option **Ändern Eigenschaften** der Azure Backup Mmc Snap-in während der Registrierung Workflow angegebene ändern können.

    ![Eigenschaften ändern](./media/backup-install-agent/change.png)

    Bei der Verwendung von Data Protection Manager können Sie auch während der Registrierung Workflow durch Klicken die Option **Konfigurieren** auswählen **Online** unter **der Registerkarte** angegebene ändern.

    ![Konfigurieren von Azure Backup](./media/backup-install-agent/configure.png)
