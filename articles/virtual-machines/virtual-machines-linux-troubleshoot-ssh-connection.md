<properties
    pageTitle="SSH-Verbindungsprobleme einer VM | Microsoft Azure"
    description="Beheben von Problemen "SSH-Verbindung fehlgeschlagen" oder "SSH-Verbindung verweigert" für eine Azure-VM unter Linux."
    keywords="SSH Verbindung zurückgewiesen, ssh Fehler, Azure ssh SSH-Verbindung fehlgeschlagen"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Problembehandlung bei SSH Verbindung mit einer Azure Linux VM, die fehlschlägt, Fehler, oder abgelehnt
Es gibt verschiedene Gründe, Secure Shell (SSH) Fehler, SSH-Verbindungsfehler auftreten oder SSH verweigert wird, wenn Sie versuchen, einen virtuellen Linux-Maschine (VM) herstellen. Dieser Artikel hilft Ihnen suchen und beheben Sie die Probleme. Azure-Portal Azure-CLI oder Erweiterung für Linux VM können Suchen und Beheben von Verbindungsproblemen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Benötigen Sie weitere Hilfe zu diesem Artikel erhalten Sie Azure-Experten auf [MSDN Azure und Foren Stapelüberlauf](http://azure.microsoft.com/support/forums/). Alternativ können Sie eine Azure Supportfall Datei. Wechseln Sie zur [Azure support-Website](http://azure.microsoft.com/support/options/) und wählen Sie **Support erhalten**. Lesen Sie Informationen über Azure unterstützt [Microsoft Azure support FAQ](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Schritte zur Fehlerbehebung
Versuchen Sie nach jedem Schritt zur Problembehandlung VM wiederherstellen.

1. SSH-Konfiguration zurückgesetzt.
2. Die Anmeldeinformationen des Benutzers zurücksetzen.
3. Überprüfen Sie die [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) Regeln SSH-Datenverkehr zulassen.
    - Sicherstellen Sie, dass eine Netzwerk-Sicherheitsgruppe Regel ist vorhanden, um SSH-Datenverkehr (standardmäßig TCP-Port 22) zulassen.
    - Können keine Port-Umleitung / ohne Azure Lastenausgleich zuordnen.
4. Überprüfen Sie den [Zustand der VM-Ressourcen](../resource-health/resource-health-overview.md). 
    - Sicherstellen Sie, dass die VM als gesund meldet.
    - Boot-Diagnose aktiviert haben, sicher, dass die VM nicht Boot-Fehler in den Protokollen reporting.
5. Starten Sie den virtuellen Computer neu.
6. Die VM erneut.

Lesen Sie für weitere Schritte zur Problembehandlung und erläutern.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Methoden zum SSH-Verbindungsproblemen

Sie können Anmeldeinformationen oder SSH-Konfiguration mit einer der folgenden Methoden zurücksetzen:

- [Azure-Portal](#using-the-azure-portal) – Wenn Sie SSH-Konfiguration oder SSH-Schlüssel und schnell zurücksetzen müssen haben nicht Azure Tools installiert.
- [Azure-CLI-Befehle](#using-the-azure-cli) - zurücksetzen Wenn Sie bereits in der Befehlszeile schnell die SSH-Konfiguration oder Anmeldeinformationen.
- [Erweiterung der Azure-VMAccessForLinux](#using-the-vmaccess-extension) - Erstellung und Wiederverwendung von Json Dateien um die SSH-Konfiguration oder Anmeldeinformationen zurückzusetzen.

Versuchen Sie nach jedem Schritt zur Problembehandlung die VM erneut. Wenn Sie weiterhin keine Verbindung herstellen können, versuchen Sie den nächsten Schritt.


## <a name="using-the-azure-portal"></a>Mithilfe des Azure-Portals
Azure-Portal bietet eine schnelle Möglichkeit, die SSH-Konfiguration oder Anmeldeinformationen ohne Tools auf dem lokalen Computer zurückgesetzt.

Wählen Sie die VM in Azure-Portal. Im Abschnitt **Support + Problembehandlung** Blättern Sie und wählen Sie **Kennwort zurücksetzen** , wie im folgenden Beispiel:

![SSH-Konfiguration oder die Anmeldeinformationen in der Azure-portal](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Setzen Sie die SSH-Konfiguration
Wählen Sie als ersten Schritt `Reset SSH configuration only` **Modus** Dropdown-Menü wie in der vorhergehenden Abbildung klicken Sie dann auf die Schaltfläche **Zurücksetzen** . Sobald diese Aktion abgeschlossen ist, versuchen Sie die VM erneut zuzugreifen.

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-Anmeldeinformationen eines Benutzers zurücksetzen
Um die Anmeldeinformationen eines bestehenden Benutzers zurückzusetzen, wählen Sie entweder `Reset SSH public key` oder `Reset password` **Modus** Dropdown-Menü wie in der vorherigen Abbildung. Geben Sie den Benutzernamen und eine SSH-Schlüssel oder Passwort und dann auf die Schaltfläche **Zurücksetzen** .

Sie können auch Benutzer mit Sudo Privilegien für die VM aus diesem Menü erstellen. Geben Sie einen neuen Benutzernamen und Kennwort oder SSH-Schlüssel, und klicken Sie dann auf die Schaltfläche **Zurücksetzen** .


## <a name="using-the-azure-cli"></a>Verwendung der Azure-CLI
Wenn nicht bereits geschehen, [Installieren der Azure-CLI und Azure-Abonnement an](../xplat-cli-install.md). Vergessen Sie Modus Ressourcen-Manager wie folgt:

```
azure config mode arm
```

Wenn Sie hochgeladen und erstellt eine benutzerdefinierte Linux-Image sicher [Microsoft Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md) Version 2.0.5 oder höher installiert ist. Für virtuelle Computer mit Galeriebildern erstellt diese Erweiterung bereits installiert und konfiguriert.

### <a name="reset-ssh-configuration"></a>SSH-Konfiguration zurücksetzen
Die SSHD-Konfiguration fehlerhaft oder der Dienst ist ein Fehler aufgetreten. Sie zurücksetzen SSHD um sicherzustellen, dass die SSH-Konfiguration selbst gültig ist. SSHD zurücksetzen sollte der erste Schritt zur Problembehandlung ergreifen.

Das folgende Beispiel setzt SSHD auf einem virtuellen Computer mit dem Namen `myVM` in der Ressourcengruppe mit dem Namen `myResourceGroup`. Verwenden Sie eigene Gruppennamen VM und Ressourcen wie folgt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-Anmeldeinformationen eines Benutzers zurücksetzen
Wenn SSHD ordnungsgemäß angezeigt wird, können Sie das Kennwort für einen Benutzer Geber zurücksetzen. Das folgende Beispiel setzt die Anmeldeinformationen für `myUsername` den Wert in `myPassword`, auf dem virtuellen Computer mit dem Namen `myVM` in `myResourceGroup`. Verwenden Sie Ihre eigenen Werte wie folgt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Wenn SSH-Schlüssel-Authentifizierung verwenden, können Sie den SSH-Schlüssel für einen bestimmten Benutzer zurücksetzen. Das folgende Beispiel aktualisiert den SSH Schlüssel `~/.ssh/azure_id_rsa.pub` für den Benutzer `myUsername`, auf dem virtuellen Computer mit dem Namen `myVM` in `myResourceGroup`. Verwenden Sie Ihre eigenen Werte wie folgt:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>VMAccess-Erweiterung verwenden
Die Erweiterung für Linux VM liest eine Json-Datei, die Aktionen definiert durchzuführen. Dazu gehören SSHD zurücksetzen, SSH-Schlüssel zurücksetzen oder einen Benutzer hinzufügen. Azure-CLI weiterhin verwenden, um die Erweiterung VMAccess aufrufen, aber Sie können Json-Dateien über mehrere VMs wiederverwenden, falls gewünscht. Dieser Ansatz ermöglicht das Repository Json-Dateien erstellen, die dann für die angegebene Szenarien können.

### <a name="reset-sshd"></a>SSHD zurücksetzen
Erstellen Sie eine Datei namens `PrivateConf.json` mit dem folgenden Inhalt:

```bash
{  
    "reset_ssh":"True"
}
```

Verwendung der Azure-CLI, Sie rufen die `VMAccessForLinux` Erweiterung der SSHD durch Angabe der Json-Datei zurücksetzen. Das folgende Beispiel setzt SSHD auf dem virtuellen Computer mit dem Namen `myVM` in `myResourceGroup`. Verwenden Sie Ihre eigenen Werte wie folgt:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>SSH-Anmeldeinformationen eines Benutzers zurücksetzen
Wenn SSHD ordnungsgemäß angezeigt wird, können Sie die Anmeldeinformationen eines Benutzers Geber zurücksetzen. Um das Kennwort eines Benutzers zurücksetzen, erstellen Sie eine Datei namens `PrivateConf.json`. Das folgende Beispiel setzt die Anmeldeinformationen für `myUsername` den Wert in `myPassword`. Geben Sie die folgenden Zeilen in Ihre `PrivateConf.json` Datei mit Ihren eigenen Werten:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Oder zum Zurücksetzen des SSH-Schlüssels eines Benutzers zunächst eine Datei namens `PrivateConf.json`. Das folgende Beispiel setzt die Anmeldeinformationen für `myUsername` den Wert in `myPassword`, auf dem virtuellen Computer mit dem Namen `myVM` in `myResourceGroup`. Geben Sie die folgenden Zeilen in Ihre `PrivateConf.json` Datei mit Ihren eigenen Werten:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Nachdem die Json-Datei erstellen, mit der Azure-CLI rufen die `VMAccessForLinux` Erweiterung Ihrer Benutzeranmeldeinformationen SSH durch Angabe der Json-Datei zurücksetzen. Das folgende Beispiel setzt Anmeldeinformationen auf dem virtuellen Computer mit dem Namen `myVM` in `myResourceGroup`. Verwenden Sie Ihre eigenen Werte wie folgt:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Starten Sie einen virtuellen Computer
Wenn Sie die SSH-Konfiguration und Benutzeranmeldeinformationen zurücksetzen oder Fehler dabei, können Sie versuchen, Neustarten der VM an zugrunde liegenden Compute-Probleme.

### <a name="azure-portal"></a>Azure-portal
Um einen virtuellen Computer mit Azure-Portal neu VM wählen, und klicken Sie auf der * Schaltfläche**Starten** , wie im folgenden Beispiel:

![Starten Sie einen virtuellen Computer in Azure-portal](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Im folgende Beispiel startet die VM mit dem Namen `myVM` in der Ressourcengruppe mit dem Namen `myResourceGroup`. Verwenden Sie Ihre eigenen Werte wie folgt:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Eine VM erneut
Sie können einen virtuellen Computer zu einem anderen Knoten in Azure, erneut die zugrunde liegende Netzwerkprobleme behoben werden können. Informationen erneute Bereitstellung eines virtuellen Computers finden Sie unter [virtuelle Computer neue Azure Knoten erneut](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Nach Abschluss dieses Vorgangs flüchtigen Daten verloren und dynamische IP-Adressen, die mit dem virtuellen Computer aktualisiert.

### <a name="azure-portal"></a>Azure-portal
Um erneut eine Azure-Portal mit VM wählen Sie die VM und Scrollen im Abschnitt **Support + Problembehandlung aus** Klicken Sie auf die Schaltfläche **erneut** , wie im folgenden Beispiel:

![VM in Azure-Portal erneut](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Azure CLI
Im folgende Beispiel vorhandene Bezeichnung VM `myVM` in der Ressourcengruppe mit dem Namen `myResourceGroup`. Verwenden Sie Ihre eigenen Werte wie folgt:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>VMs mit klassischen Bereitstellungsmodell erstellt

Versuchen Sie, diese Schritte zur Behebung der häufigsten SSH-Verbindungsfehler für virtuelle Computer, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Versuchen Sie nach jedem Schritt die VM wiederherstellen.

- Remote-Zugriff aus dem [Azure-Portal](https://portal.azure.com)zurücksetzen. Azure-Portal VM auswählen und **Remote zurücksetzen...** klicken.

- Starten Sie den virtuellen Computer neu. [Azure-Portal](https://portal.azure.com)wählen Sie VM aus und auf die Schaltfläche **Starten** .

    -ODER-

    Wählen Sie [Azure-Verwaltungsportal](https://manage.windowsazure.com) **virtuelle Computer** > **Instanzen** > **Starten**.

- VM zu einem neuen Azure Knoten erneut. Informationen erneut eine VM finden Sie in der [Virtual Machine neuen Azure Knoten erneut](virtual-machines-windows-redeploy-to-new-node.md).

    Nach Abschluss dieses Vorgangs flüchtigen Daten verloren und dynamische IP-Adressen, die mit dem virtuellen Computer aktualisiert.

- Führen Sie die Schritte in [einem Kennwort oder einer SSH für Linux-basierte virtuelle Maschinen zurücksetzen](virtual-machines-linux-classic-reset-access.md) :
    - Kennwort oder SSH-Schlüssel zurücksetzen.
    - Erstellen Sie ein Benutzerkonto _Sudo_ .
    - SSH-Konfiguration zurückgesetzt.

- Überprüfen der VM Ressource Fragen Plattform.<br>
  Wählen Sie die VM und Bildlauf **Einstellungen** > **Gesundheit überprüfen**.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- SSH nicht die VM hingegen nach nach Schritten finden Sie unter [weitere Problembehandlungsschritte](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) zusätzliche Schritte beheben das Problem.

- Weitere Informationen zur Problembehandlung bei Zugriff finden Sie unter [Problembehandlung bei Zugriff auf eine Anwendung auf einer Azure virtuellen Computer](virtual-machines-linux-troubleshoot-app-connection.md)

- Weitere Informationen zur Problembehandlung mit dem klassischen Bereitstellungsmodell erstellten virtuellen Computer finden Sie unter [Kennwörter oder SSH für Linux-basierte virtuelle Computer zurücksetzen](virtual-machines-linux-classic-reset-access.md).
