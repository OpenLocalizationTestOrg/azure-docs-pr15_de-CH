<properties
        pageTitle="Hinzufügen eines Benutzers zu einer Linux-VM auf Azure | Microsoft Azure"
        description="Hinzufügen eines Benutzers zu einer Linux-VM in Azure."
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
        ms.date="08/18/2016"
        ms.author="v-livech"
/>

# <a name="add-a-user-to-an-azure-vm"></a>Hinzufügen eines Benutzers zu einer Azure VM

Eine der ersten Aufgaben für alle neuen Linux VM ist ein neuer Benutzer erstellt.  In diesem Artikel wir gehen durch Erstellen eines Benutzerkontos Sudo Festlegen des Kennworts öffentlichen SSH-Schlüssel hinzufügen, und schließlich `visudo` Sudo ohne ein Kennwort zu.

Komponenten sind: [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), [öffentlichen und privaten Schlüsseln SSH](virtual-machines-linux-mac-create-ssh-keys.md)Azure Ressourcengruppe und Azure-CLI installiert und in Azure-Ressourcen-Manager-Modus `azure config mode arm`.

## <a name="quick-commands"></a>Schnellzugriff

```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Ausführliche Anleitung

### <a name="introduction"></a>Einführung

Die erste und häufigste Aufgabe mit einem neuen Server soll ein Benutzerkonto hinzuzufügen.  Root Logins sollte deaktiviert und das Root-Konto selbst sollte nicht mit dem Linux-Server nur Sudo verwendet werden.  Zuordnen der Benutzer Root Eskalation Berechtigungen mit Sudo es richtig verwalten und Linux verwenden.

Mit dem Befehl `useradd` wir Linux VM Benutzerkonten hinzufügen.  Ausführen `useradd` ändert `/etc/passwd`, `/etc/shadow`, `/etc/group`, und `/etc/gshadow`.  Fügen wir eine Befehlszeile Kennzeichnung der `useradd` Befehl entsprechenden Sudo Gruppe unter Linux auch neue Benutzer hinzu.  Auch Du `useradd` erstellt einen Eintrag in `/etc/passwd` es gibt dem neue Benutzerkonto ein.  Wir erstellen ein Kennwort für den neuen Benutzer mithilfe den einfachen `passwd` Befehl.  Der letzte Schritt ist die Sudo zulassen, dass Benutzer Befehle mit Sudo Berechtigungen ausführen, ohne ein Kennwort für jeden Befehl ändern.  Anmelden mit dem privaten Schlüssel wir dieses Benutzerkonto vorausgesetzt werden Verbrecher und werden Sudo ohne Kennwort zugreifen.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Ein Azure-VM hinzufügen einen einzelnes Sudo Benutzer

Melden Sie sich bei Azure VM SSH-Schlüssel verwenden.  Haben Sie nicht Setup SSH öffentlichen Schlüssel zugreifen, abgeschlossen dieser Artikel erste [Authentifizierung durch öffentliche Schlüssel mit Azure verwenden](http://link.to/article).  

Die `useradd` Befehl führt folgende Aufgaben aus:

- Erstellen eines neuen Benutzerkontos
- Erstellen Sie eine neue Benutzergruppe mit demselben Namen
- einen leeren Eintrag hinzufügen`/etc/passwd`
- einen leeren Eintrag hinzufügen`/etc/gpasswd`

Die `-G` Befehlszeilen-Flag geben das neue Benutzerkonto Eskalation rechten der richtigen Linux-Gruppe das neue Benutzerkonto hinzugefügt.

#### <a name="add-the-user"></a>Fügen Sie den Benutzer

```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Festlegen eines Kennworts

Die `useradd` Befehl erstellt den Benutzer und fügt einen Eintrag auf `/etc/passwd` und `/etc/gpasswd` , aber nicht tatsächlich das Kennwort festgelegt.  Das Kennwort wird mit dem Eintrag der `passwd` Befehl.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Wir haben jetzt einen Benutzer mit Sudo Berechtigungen auf dem Server.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>Das neue Benutzerkonto der öffentliche SSH-Schlüssel hinzufügen

Von Ihrem Computer verwenden den `ssh-copy-id` Befehl mit dem neuen Kennwort.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Mithilfe von Visudo zu Sudo Verwendung ohne Kennwort

Mit `visudo` zum Bearbeiten der `/etc/sudoers` einige Schutzschichten gegen eine fehlerhafte Änderung dieses wichtige Datei hinzugefügt.  Beim Ausführen von `visudo`, `/etc/sudoers` Datei ist gesperrt, um sicherzustellen, kein anderer Benutzer kann ändern, während sie aktiv bearbeitet wird.  Die `/etc/sudoers` Datei wird auch Fehler überprüft `visudo` Wenn Sie versuchen, speichern oder schließen, damit Sie fehlerhafte Sudoers-Datei speichern können.

Wir haben bereits Benutzer in die richtige Gruppe Sudo auf.  Wir werden nun die Gruppen Sudo ohne Kennwort aktivieren.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Überprüfen des Benutzers ssh Schlüssel und sudo

```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
