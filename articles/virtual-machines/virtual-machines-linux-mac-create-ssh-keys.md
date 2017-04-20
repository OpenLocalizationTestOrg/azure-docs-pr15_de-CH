<properties
    pageTitle="SSH-Schlüssel unter Linux und Mac erstellen | Microsoft Azure"
    description="Generieren Sie und SSH-Schlüssel unter Linux und Mac für Ressourcenmanager und klassischen Bereitstellungsmodelle Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Erstellen Sie SSH-Schlüssel auf Linux und Mac für Linux VMs in Azure

Mit einem SSH-Schlüsselpaar können Sie virtuelle Computer erstellen, Azure, die standardmäßig SSH-Schlüssel für die Authentifizierung Kennwörter Anmelden überflüssig.  Kennwörter erraten werden können, und öffnen Ihre VMs bis enorme brute-Force-versucht, Ihr Kennwort zu erraten. VMs mit Azure-Vorlagen erstellt oder `azure-cli` gehören den öffentlichen SSH-Schlüssel als Teil der Bereitstellung eine Bereitstellungskonfiguration Post entfernen.  Verbinden einer Linux VM von Windows finden Sie unter [diesem.](virtual-machines-linux-ssh-from-windows.md)

Der Artikel sind erforderlich:

- ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)).

- angemeldet bei [Azure-CLI](../xplat-cli-install.md)`azure login`

- Azure-CLI _muss_ Azure-Ressourcen-Manager-Modus`azure config mode arm`

## <a name="quick-commands"></a>Schnellzugriff

Ersetzen Sie in den folgenden Befehlen Beispiele mit eigenen Optionen.

SSH Schlüssel sind standardmäßig gespeichert der `.ssh` Verzeichnis.  

```bash
cd ~/.ssh/
```

Wenn Sie kein `~/.ssh` Verzeichnis der `ssh-keygen` Befehl erstellt für Sie mit den richtigen Berechtigungen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Geben Sie den Namen der Datei, die in gespeichert ist die `~/.ssh/` Verzeichnis:

```bash
id_rsa
```

Geben Sie Paßphrase für Id_rsa ein:

```bash
correct horse battery staple
```

Es ist jetzt eine `id_rsa` und `id_rsa.pub` SSH-Schlüsselpaar in der `~/.ssh` Verzeichnis.

```bash
ls -al ~/.ssh
```

Fügen Sie den neu erstellten Schlüssel, `ssh-agent` unter Linux und Mac (auch OSX Schlüsselbund hinzugefügt):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopieren Sie den öffentlichen SSH-Schlüssel mit dem Linux-Server:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Testen Sie die Tasten anstelle eines Kennworts anmelden:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Ausführliche Anleitung

SSH öffentliche und private Schlüssel ist die einfachste Möglichkeit, Ihre Linux-Server anmelden. [Kryptografie mit öffentlichem Schlüssel](https://en.wikipedia.org/wiki/Public-key_cryptography) ermöglicht sicherer Linux oder BSD VM in Azure als Kennwörter Anmelden die Brute gezwungen weitaus. Ihren öffentliche Schlüssel kann beliebig weitergegeben werden; aber nur Sie (oder der lokalen Sicherheitsinfrastruktur) Ihres privaten Schlüssels.  Der private SSH-Schlüssel muss ein [sehr sicheres Kennwort](https://www.xkcd.com/936/) (Quelle:[xkcd.com](https://xkcd.com)) zu schützen.  Dieses Kennwort wird nur Zugriff auf die privaten SSH-Schlüssel und das Kennwort des Benutzerkontos **ist** .  Beim Hinzufügen eines Kennworts zu SSH-Schlüssel verschlüsselt, ohne das Kennwort zum Entsperren der private Schlüssel ist den privaten Schlüssel.  Wenn ein Angreifer Ihren privaten Schlüssel gestohlen und diesen Schlüssel keinen Kennwort wären mithilfe dieser private Schlüssel auf allen Servern anmelden, die über den entsprechenden öffentlichen Schlüssel verfügen.  Wenn ein privater Schlüssel ist kennwortgeschützt es von dieser Angreifer eine zusätzliche Sicherheitsebene für Ihre Infrastruktur in Azure verwendet werden kann.

In diesem Artikel erstellt *ssh Rsa* formatiert Schlüsseldateien für die Bereitstellung der Ressourcen-Manager empfohlen.  *SSH-Rsa* -Schlüssel sind im [Portal](https://portal.azure.com) für Classic und Ressourcenmanager Installationen erforderlich.

## <a name="create-the-ssh-keys"></a>SSH-Schlüssel erstellen

Azure erfordert mindestens 2048-Bit ssh Rsa formatieren öffentliche und private Schlüsseln. Zum Erstellen der Schlüssel `ssh-keygen`, die eine Reihe von Fragen und schreibt einen privaten Schlüssel und einen passenden öffentlichen Schlüssel. Beim Erstellen einer Azure VM in öffentliche Schlüssel kopiert `~/.ssh/authorized_keys`.  SSH-Schlüssel in `~/.ssh/authorized_keys` zu den Client mit dem entsprechenden privaten Schlüssel auf eine SSH-Verbindung anmelden verwendet.

## <a name="using-ssh-keygen"></a>Ssh-Keygen verwenden

Dieser Befehl erstellt ein Kennwort (verschlüsselt) SSH-Schlüsselpaar mit 2048-Bit RSA gesichert und einfache Identifizierung auskommentiert.  

Beginnen Verzeichnisse ändern, damit alle Ihre ssh Schlüssel werden in diesem Verzeichnis erstellt.

```bash
cd ~/.ssh
```

Wenn Sie kein `~/.ssh` Verzeichnis der `ssh-keygen` Befehl erstellt für Sie mit den richtigen Berechtigungen.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Befehl erläutert_

`ssh-keygen`das Programm zum Erstellen der Schlüssel =

`-t rsa`= Schlüsseltyp zu erstellen, die die [RSA Format](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= Bits des Schlüssels

`-C "myusername@myserver"`= einen Kommentar angefügt an das Ende der öffentlichen Schlüsseldatei so leicht identifizieren.  Eine e-Mail wird als Kommentar verwendet normalerweise können, was am besten für Ihre Infrastruktur funktioniert.

### <a name="using-pem-keys"></a>PEM-Tasten

Verwenden Sie das klassische Modell bereitstellen (Azure-Verwaltungsportal oder Azure Service Management CLI `asm`), müssen PEM verwenden SSH-Schlüssel Zugriff auf die VMs Linux formatiert.  So erstellen eines PEM aus einem vorhandenen SSH öffentlichen Schlüssel und einer vorhandenen X509 Zertifikat.

Erstellen Sie ein PEM formatiert Schlüssel aus einem vorhandenen öffentlichen SSH-Schlüssel:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Beispiel für ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Dateien gespeichert:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Das Schlüsselpaar Name für diesen Artikel.  Ein Schlüsselpaar mit dem Namen **Id_rsa** ist die Standardeinstellung und einige Tools erwarten **Id_rsa** Privatschlüsseldatei Namen so eine gute Idee ist. Das Verzeichnis `~/.ssh/` ist der Standardspeicherort für SSH-Schlüsselpaare und SSH-Konfigurationsdatei.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Eine Liste der `~/.ssh` Verzeichnis. `ssh-keygen`erstellt die `~/.ssh` Verzeichnis ist nicht vorhanden und wird korrekt Besitz und Datei-Modus.

Kennwort:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`bezieht sich auf ein als "eine Passphrase".  Es wird *dringend* empfohlen, die Schlüsselpaare ein Kennwort hinzufügen. Ohne Kennwort schützen das Schlüsselpaar können jeder mit einer privaten Schlüsseldatei an jedem Server anmelden mit dem entsprechenden öffentlichen Schlüssel. Hinzufügen ein Kennworts bietet mehr Schutz bei jemand Ihre private Schlüsseldatei zugreifen, erhalten Sie Schlüssel geändert Authentifizierung verwendet.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Verwenden von ssh-Agent Ihr Kennwort für private Schlüssel speichern

Um zu vermeiden, der privaten Schlüsseldatei Kennwort bei jeder Anmeldung SSH, können `ssh-agent` Passwort Privatschlüsseldatei zwischengespeichert. Bei Verwendung einen Mac OSX Schlüsselbund sicher speichert die privaten Schlüssel Kennwörter beim Aufruf `ssh-agent`.

Überprüfen Sie zunächst, die `ssh-agent` wird ausgeführt

```bash
eval "$(ssh-agent -s)"
```

Nun fügen Sie den privaten Schlüssel `ssh-agent` mit dem Befehl `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Das Kennwort für der private Schlüssel werden jetzt in `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Erstellen Sie und konfigurieren Sie einer SSH-Konfigurationsdatei

Wird empfohlen, erstellen und Konfigurieren einer `~/.ssh/config` Datei beschleunigt anzumelden und optimieren die SSH-Clientverhalten.

Das folgende Beispiel zeigt eine Standardkonfiguration.

### <a name="create-the-file"></a>Erstellen Sie die Datei

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Bearbeiten Sie die Datei zum neue SSH-Konfiguration hinzufügen:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Beispiel `~/.ssh/config` Datei:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Diese SSH-Konfiguration können Sie Abschnitte für die einzelnen Server für jeweils eine eigene dedizierte Schlüsselpaar. Die Standardeinstellungen (`Host *`) sind für alle Hosts, die nicht in der Konfigurationsdatei höher bestimmte Hosts übereinstimmen.


### <a name="config-file-explained"></a>Konfigurationsdatei beschrieben

`Host`= der Name des Hosts, auf dem Terminal aufgerufen wird.  `ssh fedora22`weist `SSH` mit den Werten in der blockiert mit der Bezeichnung `Host fedora22` Hinweis: kann Beschriftung, die für die Verwendung und nicht der tatsächliche Hostname eines Servers darstellen.

`Hostname 102.160.203.241`= IP-Adresse oder DNS-Namen für den Server zugegriffen wird.

`User myusername`= das Remotebenutzerkonto verwendet beim Server anmelden.

`PubKeyAuthentication yes`= teilt SSH mit einem SSH-Schlüssel anmelden möchten.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= private SSH-Schlüssel und zugehörigen öffentlichen Schlüssel zur Authentifizierung verwendet.


## <a name="ssh-into-linux-without-a-password"></a>SSH in Linux ohne Kennwort

Jetzt haben Sie ein SSH-Schlüsselpaar und konfigurierten SSH-Konfigurationsdatei können Sie Ihre Linux VM schnell und sicher anmelden. Das erste Mal auf einem Server mit einem SSH-Schlüssel die fordert Sie das Kennwort für diese Schlüsseldatei Anmeldung.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Befehl erläutert

Wenn `ssh fedora22` erfolgt SSH zuerst sucht und lädt alle Einstellungen der `Host fedora22` blockieren und lädt alle verbleibenden Einstellungen der letzte Block `Host *`.

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes wird Azure Linux VMs mit den neuen öffentlichen SSH-Schlüssel erstellen.  Azure VMs mit einem öffentlichen SSH-Schlüssel als Login erstellt sind besser als VMs erstellt die Standardmethode des Benutzernamen, Kennwörter gesichert.  Azure VMs mit SSH-Schlüssel erstellt werden standardmäßig konfiguriert mit Kennwörtern deaktiviert, vermeiden erraten versucht Brute gezwungen.

- [Erstellen einer sicheren Linux VM mit einer Azure Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Erstellen einer sicheren Linux VM mit Azure-portal](virtual-machines-linux-quick-create-portal.md)
- [Erstellen einer sicheren Linux VM mit der Azure-CLI](virtual-machines-linux-quick-create-cli.md)
