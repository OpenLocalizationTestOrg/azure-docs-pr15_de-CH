<properties 
    pageTitle="Auswählen von Benutzernamen für Linux | Microsoft Azure" 
    description="Erfahren Sie, wie bei einer virtuellen Linux-Maschine in Azure auswählen." 
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



#<a name="selecting-user-names-for-linux-on-azure"></a>Bei Linux auf Azure auswählen#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Bei der Bereitstellung einer virtuellen Linux-Maschine auf Azure müssen Sie den Namen der nicht-Root-Benutzer angeben, die Sie später verwenden können, den virtuellen Computer anmelden. Wählen Sie den Namen des neuen Benutzers oder wenn über klassischen Azure-Portal bereitstellen übernehmen die Standardeinstellung "Azureuser".

In den meisten Fällen dieser Benutzer nicht auf das Basisabbild vorhanden und Bereitstellungsprozess erstellt werden. Wenn der Benutzer auf das Basisabbild VM vorhanden, konfiguriert Azure Linux-Agent einfach Kennwort oder SSH-Schlüssel für den Benutzer basierend auf Informationen, die Sie beim Erstellen der VM angegeben.

**Jedoch**definiert Linux Benutzernamen nicht verwendet werden soll. Der Bereitstellungsprozess wird **fehlschlagen** beim Bereitstellen einer Linux VM mit vorhandener Systembenutzer mit UID 0 bis 99 definiert. Ein Beispiel ist die `root` Benutzer die UID 0.

 - Siehe auch: [Linux Standard Base - Benutzer-ID Bereich](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Folgendes ist eine Liste häufig integrierte Benutzer CentOS und Ubuntu, Grund sollten bei der Bereitstellung einer virtuellen Linux-Maschine in Azure. Diese Liste ist nur ein Beispiel, finden Sie in der Dokumentation Ihrer Verteilung um sicherzustellen, dass der gewählte Benutzername nicht mit einem vorhandenen System.


## <a name="centos"></a>CentOS
- abrt
- ADM
- Audio
- Lagerplatz
- CD-ROM
- cgred
- Daemon
- dbus
- Hinauswählen
- DIP
- Datenträger
- Disketten
- FTP
- FTP
- Spiele
- Gopher
- haldaemon
- Anhalten
- können
- Sperren
- LP
- e-Mail-Nachrichten
- Mann
- Mem
- nfsnobody
- niemand
- NTP
- Operator
- oprofile
- postdrop
- Postfix
- qpidd
- Stamm
- RPC
- rpcuser
- saslauth
- Herunterfahren
- slocate
- sshd
- stapdev
- stapusr
- Synchronisieren
- Sys
- Band
- Testen
- tcpdump
- TTY
- Benutzer
- utempter
- utmp
- UUCP
- vcsa
- Video
- Mausrad


## <a name="ubuntu"></a>Ubuntu
- ADM
- Admin
- Audio
- Sicherung
- Lagerplatz
- CD-ROM
- crontab
- Daemon
- Hinauswählen
- DIP
- Datenträger
- Fax
- Disketten
- Sicherung
- Spiele
- mücken
- IRC
- können
- Querformat
- libuuid3LIB
- Liste
- LP
- e-Mail-Nachrichten
- Mann
- MessageBus
- mlocate
- netdev
- Neuigkeiten
- niemand
- nogroup
- Operator
- plugdev
- Proxy
- Stamm
- SASL
- Schatten
- src
- SSH
- sshd
- Mitarbeiter
- sudo
- Synchronisieren
- Sys
- Syslog
- Band
- TTY
- Benutzer
- utmp
- UUCP
- Video
- Stimme
- whoopsie
- Www-Daten

 