<properties 
    pageTitle="Verwenden Sie Root-Rechte für virtuelle Linux-Computer | Microsoft Azure" 
    description="Informationen Sie zum Root-Berechtigungen auf einem virtuellen Linux-Maschine in Azure verwenden." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Root-Rechte verwenden für virtuelle Linux-Computer in Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Standardmäßig die `root` Benutzer auf virtuelle Linux-Computer in Azure deaktiviert. Benutzer können mit erhöhten Befehle ausführen, mit dem `sudo` Befehl. Die Erfahrung variiert je nachdem, wie das System bereitgestellt wurde.

1. **SSH-Schlüssel und Kennwort oder nur** - virtuelle Maschine wurde mit einem Zertifikat bereitgestellt (`.CER` Datei) SSH-Schlüssel als auch ein Kennwort oder nur einen Benutzernamen und Kennwort. In diesem Fall `sudo` das Kennwort des Benutzers aufgefordert wird, bevor der Befehl ausgeführt.

2. **SSH-Schlüssel** – der virtuelle Computer mit einem Zertifikat bereitgestellt wurde (`.cer`, `.pem`, oder `.pub` Datei) oder SSH-Schlüssel, aber kein Kennwort.  In diesem Fall `sudo` **keine** Aufforderung für das Kennwort des Benutzers vor der Ausführung.


## <a name="ssh-key-and-password-or-password-only"></a>SSH Schlüssel und Kennwort oder nur Kennwort

Virtuellen Linux-Maschine über SSH-Schlüssel oder Kennwortauthentifizierung anmelden, und führen Sie Befehle mit `sudo`, beispielsweise:

    # sudo <command>
    [sudo] password for azureuser:

In diesem Fall wird der Benutzer nach einem Kennwort aufgefordert. Nach Eingabe des Kennworts `sudo` Ausführen den Befehl `root` Berechtigungen.

Sie können auch ohne Passwort Sudo durch Bearbeiten der `/etc/sudoers.d/waagent` Datei, beispielsweise:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Diese Änderung ermöglicht ohne Passwort Sudo vom Benutzer "Azureuser".

## <a name="ssh-key-only"></a>SSH nur Schlüssel

Virtuellen Linux-Maschine SSH-Schlüssel Authentifizierung anmelden, und führen Sie Befehle mit `sudo`, beispielsweise:

    # sudo <command>

In diesem Fall wird der Benutzer **nicht** nach seinem Passwort gefragt werden. Nach Drücken `<enter>`, `sudo` Ausführen den Befehl `root` Berechtigungen.

 
