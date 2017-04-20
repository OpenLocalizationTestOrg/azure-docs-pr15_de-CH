<properties
    pageTitle="Deaktivieren Sie SSH-Kennwörter auf Linux VM SSHD konfigurieren | Microsoft Azure"
    description="Sichern Sie Ihrer Linux VM in Azure Kennwort Benutzernamen SSH deaktivieren."
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Deaktivieren Sie SSH-Kennwörter auf Linux VM SSHD konfigurieren

Dieser Artikel befasst sich wie die Login-Sicherheit des Linux VM gesperrt.  Sobald der SSH-Port 22 an Bots Welt versuchen Erraten von Kennwörtern anmelden geöffnet wird.  Machen wir in diesem Artikel wird ist Kennwort Benutzernamen über SSH deaktivieren.  Vollständig entfernen können Kennwörter schützen wir Linux VM aus dieser Art von brute-Force-Angriff.  Der Vorteil ist wir Linux SSHD damit nur Benutzernamen über SSH öffentliche und private Schlüssel mit Abstand die sicherste Möglichkeit bei Linux konfigurieren.  Die möglichen Kombinationen müsste der private Schlüssel ist und daher rät Bots sogar belästigt versuchen, brute-Force-SSH-Schlüssel erraten.


## <a name="goals"></a>Ziele

- Konfigurieren Sie SSHD nicht:
  - Kennwort-Benutzernamen
  - Root-Benutzername
  - NTLM-Authentifizierung
- SSHD zu konfigurieren:
  - nur SSH-Schlüssel Benutzernamen
- Starten Sie SSHD noch angemeldet
- Testen der neuen SSHD-Konfiguration

## <a name="introduction"></a>Einführung

[SSH definiert](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD ist die SSH-Server, die auf Linux VM.  SSH ist ein Client, der von der Shell auf Ihrer Arbeitsstation MacBook oder Linux ausgeführt wird.  SSH ist auch das Protokoll sichern und Verschlüsseln der Kommunikation zwischen der Arbeitsstation und Linux VM.

Für diesen Artikel ist es sehr wichtig, eine Anmeldung Linux VM öffnen für die gesamte Strecke durch.  Aus diesem Grund wird es zwei Terminals und SSH Linux VM beide geöffnet.  Wir verwenden das erste Terminal zu ändern SSHDs Konfigurationsdatei SSHD-Dienst neu starten.  Wir verwenden das zweite Terminal Änderungen testen, sobald der Dienst neu gestartet wird.  Weil wir SSH Kennwörter deaktivieren und anhand ausschließlich SSH-Schlüssel, die SSH-Schlüssel nicht korrekt und schließen die Verbindung zur VM, die VM dauerhaft gesperrt und werden für die Anmeldung müssen gelöscht und neu erstellt werden können.

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Erstellen Sie SSH-Schlüssel auf Linux und Mac für Linux VMs in Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Ein Azure-Konto
  - [kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure-portal](http://portal.azure.com)
- Linux VM auf Windows Azure ausgeführte
- SSH öffentliche und private Schlüsselpaar in`~/.ssh/`
- Öffentliche SSH-Schlüssel in `~/.ssh/authorized_keys` auf Linux VM
- Sudo Rechte auf dem virtuellen Computer
- Anschluss 22 öffnen

## <a name="quick-commands"></a>Schnellzugriff

_Erfahrene Linux-Admins, die nur die Kurzfassung Version beginnen Sie hier.  Für alle anderen, die will überspringen ausführliche Erläuterung und Einführung in diesen Abschnitt._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Detaillierte Spaziergang

Bei Linux VM auf terminal 1 (T1).  Bei Linux VM auf terminal 2 (T2).

T2 werden wir die SSHD-Konfigurationsdatei bearbeiten.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Hier werden wir nur die Einstellungen Kennwörter deaktivieren und Aktivieren von SSH-Schlüssel Benutzernamen bearbeiten.  Es gibt viele Einstellungen in dieser Datei, die Sie untersuchen und ändern zu Linux & SSH so sicher, wie Sie benötigen.

#### <a name="disable-password-logins"></a>Deaktivieren Sie Kennwort-Benutzernamen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Aktivieren der Authentifizierung durch öffentliche Schlüssel

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Deaktivieren Sie die Root-Anmeldung

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Deaktivieren Sie die NTLM-Authentifizierung

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>SSHD starten

Über die Shell T1 sicher, dass Sie immer noch angemeldet sind.  Dies ist wichtig, damit Sie nicht aus der VM gesperrt erhalten werden, wenn die SSH-Schlüssel nicht korrekt sind, da Kennwörter deaktiviert werden.  Wenn eine Einstellung auf Linux VM stimmen T1 mit Sshd_config beheben Sie noch im angemeldet und SSH hält die Verbindung aktiv während des SSHD-Dienstes neu starten.

Von T2 ausführen:

##### <a name="on-the-debian-family"></a>Auf der Debian-Familie

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>RedHat-Familie

```
username@macbook$ sudo service sshd restart
```

Kennwörter sind jetzt vor brute-Force-Kennwort Anmeldeversuche VM deaktiviert.  Mit nur SSH-Schlüssel zulässig, schneller anmelden und sicherer werden.
