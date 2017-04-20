<properties
        pageTitle="Zurücksetzen von Linux VM Kennwort und SSH-Schlüssel von der CLI | Microsoft Azure"
        description="Verwendung die VMAccess-Erweiterung von Azure-Befehlszeilenschnittstelle (CLI) ein Linux VM Kennwort oder SSH-Schlüssel zurücksetzen, SSH-Konfiguration beheben und Überprüfen der Konsistenz der Festplatte"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Das Zurücksetzen einer Linux VM Kennwort oder SSH-Schlüssel, korrigieren Sie die SSH-Konfiguration und Überprüfen der Konsistenz der Datenträger mit der Erweiterung VMAccess


Wenn aufgrund eines vergessenen Kennworts ein falscher Schlüssel Secure Shell (SSH) keine zu einem virtuellen Linux-Maschine auf Azure Verbindung oder ein Problem mit SSH-Konfiguration mithilfe der Erweiterung VMAccessForLinux mit der Azure-CLI das Kennwort oder die SSH-Schlüssel zurücksetzen, beheben Sie SSH-Konfiguration und überprüfen Sie der Konsistenz der Festplatte. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Mit der CLI Azure verwenden Sie den Befehl **Azure Vm Erweiterung** über die Befehlszeilenschnittstelle (Bash Terminal Befehlszeile) Befehle. **Vm-Erweiterung Azure Hilfe festgelegt** für die detaillierte Verwendung ausführen.

Mit der CLI Azure können Sie folgenden Aufgaben ausführen:

+ [Zurücksetzen des Kennworts](#pwresetcli)
+ [SSH-Schlüssel zurücksetzen](#sshkeyresetcli)
+ [Zurücksetzen des Kennworts und SSH-Schlüssel](#resetbothcli)
+ [Erstellen eines neuen Benutzerkontos sudo](#createnewsudocli)
+ [Setzen Sie die SSH-Konfiguration](#sshconfigresetcli)
+ [Benutzer löschen](#deletecli)
+ [Der Status der VMAccess-Erweiterung](#statuscli)
+ [Überprüfen der Konsistenz der Datenträger](#checkdisk)
+ [Datenträger auf Linux VM reparieren](#repairdisk)


## <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes:

- [Azure-CLI installieren](../xplat-cli-install.md) und [Ihr Abonnement](../xplat-cli-connect.md) müssen Azure Ressourcen mit Ihrem Konto verknüpft ist.
- Festlegen des richtigen Modus für das klassische Bereitstellungsmodell Folgendes an der Befehlszeile eingeben:
        
        azure config mode asm
        
- Haben Sie ein neues Kennwort oder SSH-Schlüssel einem zurücksetzen möchten. Sie brauchen diese, wenn Sie SSH-Konfiguration zurücksetzen möchten.


## <a name="pwresetcli"></a>Zurücksetzen des Kennworts

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesen Zeilen. Ersetzen Sie die Klammern und & #60; Platzhalter & #62; Werte durch eigene Informationen.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Führen Sie diesen Befehl dem Namen für den virtuellen Computer & #60; Vm-Name & #62;

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>SSH-Schlüssel zurücksetzen

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesen Inhalten. Ersetzen Sie die Klammern und & #60; Platzhalter & #62; Werte durch eigene Informationen.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Führen Sie diesen Befehl dem Namen für den virtuellen Computer & #60; Vm-Name & #62;

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Zurücksetzen des Kennworts und SSH-Schlüssel

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesen Inhalten. Ersetzen Sie die Klammern und & #60; Platzhalter & #62; Werte durch eigene Informationen.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Führen Sie diesen Befehl dem Namen für den virtuellen Computer & #60; Vm-Name & #62;

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Erstellen eines neuen Benutzerkontos sudo

Wenn Sie Ihren Benutzernamen vergessen, können VMAccess Sie eine Behörde Sudo erstellen. In diesem Fall werden der vorhandene Benutzername und Kennwort nicht geändert werden.

Zum Erstellen eines neuen Benutzers Sudo mit Passwort das Skript in [das Passwort](#pwresetcli) und geben Sie den neuen Benutzernamen.

Zum Erstellen eines neuen Sudo Benutzers mit den SSH-Zugriff das Skript in [der SSH-Schlüssel zurücksetzen](#sshkeyresetcli) und geben Sie den neuen Benutzernamen.

[Zurücksetzen des Kennworts und SSH-Schlüssel](#resetbothcli) können auch neuen Benutzer mit Kennwort und den SSH-Zugriff erstellen.

## <a name="sshconfigresetcli"></a>Setzen Sie die SSH-Konfiguration

Wenn SSH-Konfiguration in einen unerwünschten Zustand befindet, können Sie auch für die VM zugreifen. Die VMAccess-Erweiterung können Sie die Konfiguration auf die Standardeinstellungen zurückzusetzen. Hierzu müssen Sie den Schlüssel "Reset_ssh" auf "True" festgelegt. Die Erweiterung den SSH-Server neu starten, öffnen Sie SSH-Port auf der VM und SSH-Konfiguration auf Standardwerte zurücksetzen. Benutzerkonto (Name, Kennwort oder SSH-Schlüssel) wird nicht geändert.

> [AZURE.NOTE] SSH-Konfigurationsdatei, die zurückgesetzt wird, befindet sich unter /etc/ssh/sshd_config.

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesem Inhaltstyp.

        {
        "reset_ssh":"True"
        }

2. Führen Sie diesen Befehl dem Namen für den virtuellen Computer & #60; Vm-Name & #62; 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Benutzer löschen

Wenn Sie ein Benutzerkonto ohne Anmeldung der VM direkt löschen möchten, können Sie dieses Skript.

1. Erstellen Sie eine Datei namens PrivateConf.json mit diesem Inhalt ersetzen den Benutzernamen entfernen & #60; Usernametoremove & #62; 

        {
        "remove_user":"<usernametoremove>"
        }

2. Führen Sie diesen Befehl dem Namen für den virtuellen Computer & #60; Vm-Name & #62; 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Der Status der VMAccess-Erweiterung

Führen Sie diesen Befehl, um den Status des VMAccess-Erweiterung anzeigen.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< a Name = 'Checkdisk' <</a>Konsistenz Datenträger überprüfen

Um Fsck auf allen Datenträgern der virtuellen Linux-Maschine ausgeführt wird, müssen Sie Folgendes tun:

1. Erstellen Sie eine Datei namens PublicConf.json mit diesem Inhaltstyp. Check Disk nimmt einen booleschen Wert, ob Datenträgern mit dem virtuellen Computer oder nicht überprüfen. 

        {   
        "check_disk": "true"
        }

2. Führen Sie diesen Befehl ausführen, ersetzen den Namen des virtuellen Computers für & #60; Vm-Name & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Notfalldisketten 

Verwenden Sie, um Datenträger zu reparieren, die nicht bereitgestellt oder Mount-Konfigurationsfehler VMAccess-Erweiterung Mount-Konfiguration auf Ihrem virtuellen Linux-Maschine zurückgesetzt. Ersetzen den Namen des Datenträgers #60; Yourdisk & #62;

1. Erstellen Sie eine Datei namens PublicConf.json mit diesem Inhaltstyp. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Führen Sie diesen Befehl ausführen, ersetzen den Namen des virtuellen Computers für & #60; Vm-Name & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Nächste Schritte

* Wenn Sie Azure PowerShell-Cmdlets oder Azure Resource Manager Vorlagen verwenden, um das Kennwort oder die SSH-Schlüssel zurücksetzen möchten, SSH-Konfiguration beheben Datenträger Konsistenz überprüfen und finden Sie die [VMAccess-Erweiterung Dokumentation auf GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Sie können auch den [Azure-Portal](https://portal.azure.com) , das Kennwort zurückzusetzen oder SSH-Schlüssel des Linux VM im klassischen Bereitstellungsmodell bereitgestellt. Derzeit können keine Portal führen Sie für Linux VM in Ressourcen-Manager-Bereitstellungsmodell bereitgestellt.

* Weitere [Features zu virtuellen Extensions](virtual-machines-linux-extensions-features.md) VM-Erweiterungen Azure virtuelle Computer mit.
