<properties
   pageTitle="Einführung in FreeBSD auf Azure | Microsoft Azure"
   description="Weitere Informationen Sie zur Verwendung von FreeBSD virtueller Maschinen auf Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Einführung in FreeBSD auf Azure
In diesem Thema Überblick eine FreeBSD-VM in Azure ausgeführt.

## <a name="overview"></a>Übersicht
FreeBSD Microsoft Azure ist eine fortgeschrittene zur Stromversorgung moderne Server, Desktops und embedded-Plattformen. FreeBSD 10.3 Bild von Microsoft Corporation bereitgestellt und steht in Azure. Es basiert auf FreeBSD 10.3 Release und Azure VM-Gast-Agent [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) installiert ist. Der Agent ist verantwortlich für die Kommunikation zwischen den FreeBSD VM und Azure Fabric für Operationen wie Bereitstellung virtueller Computer bei der ersten Verwendung (Benutzername, Kennwort, Hostname, etc.) und Funktionalität für selektive VM Extensions.
Für zukünftige Versionen von FreeBSD ist die Strategie zu bleiben und die neuesten Versionen Nachdem sie FreeBSD Release engineering Team freigegeben werden. Die bevorstehende Veröffentlichung ist [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Bereitstellung einer virtuellen Maschine FreeBSD
Bereitstellung einer virtuellen Maschine FreeBSD ist ein einfacher Prozess mit einem Bild von [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>VM-Erweiterungen für FreeBSD
Unterstützte VM Extensions FreeBSD folgen.

### <a name="vmaccess"></a>VMAccess

Die [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) -Erweiterung kann:

- Setzen Sie das Kennwort des ursprünglichen Benutzers Sudo.
- Erstellen eines neuen Benutzers Sudo mit dem angegebenen Kennwort.
- Legen Sie öffentliche Hosttaste mit dem angegebenen Schlüssel.
- Setzen der öffentlichen Host während VM-Bereitstellung nicht sofern die Hosttaste.
- SSH-Port (22) öffnen und Wiederherstellen der Sshd_config setzen Reset_ssh auf True.
- Entfernen Sie vorhandenen Benutzer.
- Prüfen Sie Datenträger
- Reparieren einer zusätzlichen Festplatte.

### <a name="customscript"></a>CustomScript

[CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) -Erweiterung kann:

- Sofern herunterladen Sie benutzerdefinierte Skripts Azure Storage oder externen öffentlichen Speicher (z. B. GitHub).
- Das Skript Eintrag zeigen.
- Inline-Befehle unterstützt.
- Konvertieren Sie Windows-ähnliche Zeilenumbruch in der Schale und Python-Skripts automatisch.
- Entfernen Sie Stückliste automatisch in der Schale und Python-Skripten.
- Schützen Sie vertrauliche Daten im CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel
Wenn Sie einen virtuellen Computer FreeBSD mithilfe des Azure-Portals erstellen, müssen Sie einen Benutzernamen, Kennwort oder öffentlichen SSH-Schlüssel angeben.
Bei Bereitstellung einer virtuellen Maschine FreeBSD auf Azure muss keine Namen von Systemkonten (UID < 100) bereits auf dem virtuellen Computer ("Root," beispielsweise).
Derzeit wird nur RSA SSH-Schlüssel unterstützt. Mehrzeiliger SSH-Schlüssel muss mit "---BEGIN SSH2 öffentlichen Schlüssel--" beginnen und enden mit "--Ende SSH2 öffentlicher Schlüssel--".

## <a name="obtaining-superuser-privileges"></a>Superuser-Berechtigungen erhalten
Die während der Bereitstellung von virtuellen Instanz auf Azure angegebenen Benutzerkontos ist ein privilegiertes Konto. Das Paket Sudo wurde im veröffentlichten FreeBSD-Abbild installiert.
Nachdem Sie sich über dieses Konto angemeldet sind, können Sie Befehle mit der Syntax als Root ausführen.

    # sudo <COMMAND>

Eine Root-Shell erhalten optional mit Sudo -s.

## <a name="next-steps"></a>Nächste Schritte
- [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) FreeBSD VM erstellt werden.
- Ggf. Azure eigene FreeBSD zu [Erstellen](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md)und Hochladen einer VHD FreeBSD in Azure verweisen.
