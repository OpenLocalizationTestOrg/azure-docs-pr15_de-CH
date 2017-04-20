<properties
    pageTitle="Zurücksetzen auf Azure Linux VMs mit der Erweiterung VMAccess | Microsoft Azure"
    description="Zurücksetzen Sie auf Azure Linux VMs mit VMAccess-Erweiterung."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Verwalten von Benutzern, SSH und Kontrollkästchen oder Notfalldisketten in Azure Linux VMs mit VMAccess-Erweiterung

Dieser Artikel beschreibt wie Sie Azure VMAcesss Erweiterung oder einen Datenträger reparieren, Benutzerzugriff zurücksetzen, Benutzerkonten verwalten oder Zurücksetzen der SSHD-Konfigurations unter Linux. Der Artikel sind erforderlich:

- ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)).

- angemeldet bei [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure-CLI _muss_ Azure-Ressourcen-Manager-Modus `azure config mode arm`.

## <a name="quick-commands"></a>Schnellzugriff

Es gibt zwei Verwendungsmöglichkeiten VMAccess für Ihre Linux-VMs:

- Verwenden der Azure-CLI und die erforderlichen Parameter.
- Verwenden raw JSON-Dateien, die VMAccess verarbeitet und dann auf.

Für Abschnitt quick-Befehl werden wir die Azure CLI `azure vm reset-access` Methode. Ersetzen Sie in den folgenden Beispielen Befehl die Werte, die "Beispiel" mit den Werten aus Ihrer Umgebung enthalten.

## <a name="create-a-resource-group-and-linux-vm"></a>Erstellen Sie eine Ressourcengruppe und Linux VM

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Erstellen einer Debian VM

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Passwort zurücksetzen

Das Root-Passwort zurücksetzen:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH-Schlüssel zurücksetzen

SSH-Schlüssel eines Benutzers nicht Stamm zurücksetzen:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Erstellen Sie einen Benutzer

So erstellen Sie einen Benutzer

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Entfernen eines Benutzers

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD zurücksetzen

Die SSHD-Konfiguration zurücksetzen:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Ausführliche Anleitung

### <a name="vmaccess-defined"></a>VMAccess definiert:

Der Datenträger auf Linux VM zeigt Fehler. Sie irgendwie das Stammkennwort für Linux VM zurücksetzen oder den SSH-Schlüssel versehentlich gelöscht. In diesem Fall in den Tagen des Rechenzentrums müssten Sie es und KVM auf der Serverkonsole zu öffnen. Vorstellen der Azure-VMAccess-Erweiterung als KVM-Switches, mit dem Sie Zugriff auf die Konsole, um Zugriff auf Linux zurücksetzen oder Datenträger auf Wartung.

Ausführliche exemplarische Vorgehensweise werden wir für die Langform des VMAccess, der raw JSON-Dateien verwendet.  Diese VMAccess JSON-Dateien können auch von Azure Vorlagen aufgerufen.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Verwenden VMAccess oder reparieren Sie die Festplatte Linux VM

Mit VMAccess möglich, eine Fsck auf dem Datenträger unter Linux VM ausführen.  Sie können eine Überprüfung des Datenträgers und eine Reparatur mit einem VMAccess tun.

Überprüfen und reparieren Sie die Festplatte dieses VMAccess verwenden:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Mithilfe von VMAccess Zugriff auf Linux zurücksetzen

Wenn Sie Zugriff auf Linux VM Stamm verloren haben, können Sie ein Skript für das Root-Passwort zurücksetzen VMAccess starten.

Verwenden Sie dieses VMAccess, um das Root-Passwort zurückzusetzen:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Um den SSH-Schlüssel eines Benutzers nicht Root zurückzusetzen, verwenden Sie dieses VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Verwenden VMAccess zum Verwalten von Benutzerkonten auf Linux

VMAccess ist eine Python-Skript, das Linux VM Benutzer verwalten, ohne Anmeldung und Sudo oder Root-Konto verwendet werden kann.

Verwenden Sie zum Erstellen eines Benutzers dieses VMAccess:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Verwenden Sie zum Löschen eines Benutzers dieses VMAccess:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Die SSHD-Konfiguration zurücksetzen mithilfe von VMAccess

Wenn Linux VMs SSHD-Konfiguration ändern und die SSH-Verbindung vor dem Überprüfen der Änderung schließen können Sie nicht SSH'ing werden wieder.  VMAccess kann verwendet werden, um die SSHD-Konfiguration auf einen als funktionierend bekannten Konfiguration zurückgesetzt, ohne über SSH eingeloggt.

Zurücksetzen die SSHD-Konfiguration dieses VMAccess verwenden:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Führen Sie das Skript VMAccess mit:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Nächste Schritte

Aktualisieren von Linux ist mit VMAccess Extensions Azure eine Methode ausgeführten Linux VM ändern.  Wie Cloud Init und Azure-Vorlagen können Sie Ihre Linux VM beim Start ändern.

[VM-Erweiterungen zu Funktionen](virtual-machines-linux-extensions-features.md)

[Erstellen von Vorlagen mit Linux VM Azure-Ressourcen-Manager](virtual-machines-linux-extensions-authoring-templates.md)

[Mithilfe von Cloud-Init Linux VM während der Erstellung anpassen](virtual-machines-linux-using-cloud-init.md)
