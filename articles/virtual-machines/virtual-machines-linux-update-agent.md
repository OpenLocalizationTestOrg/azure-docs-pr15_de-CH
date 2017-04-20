<properties
    pageTitle="Aktualisieren Sie den Azure Linux-Agent von GitHub | Microsoft Azure"
    description="Erfahren Sie, wie das Update Azure Linux-Agent für Ihre Linux VM in Azure die Lateset Version von Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Aktualisieren von Azure Linux-Agent auf einem virtuellen Computer auf die neueste Version von GitHub

Aktualisieren Sie Ihre [Azure Linux-Agent](https://github.com/Azure/WALinuxAgent) auf einem Linux VM in Azure müssen Sie:

1. Eine laufende Linux VM in Azure.
2. Eine Verbindung zu diesem Linux VM über SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Wenn Sie diese Aufgabe von einem Windows-Computer ausführen, können Sie in Ihrer linuxmaschine kitten SSH. Weitere Informationen finden Sie unter [Anmelden bei einer virtuellen Linux ausgeführt](virtual-machines-linux-mac-create-ssh-keys.md).

Azure unterstützt Linux-Distributionen haben Azure Linux-Agent-Paket in ihre Repositorys so überprüfen und Installieren der neuesten Version von Distribution Repository zuerst nach Möglichkeit.  

Für Ubuntu geben:

    #sudo apt-get install walinuxagent

Und CentOS, geben:

    #sudo yum install waagent


Für Oracle Linux sicher, dass die `Addons` Repository aktiviert ist. Wählen Sie die Datei `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) oder `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), und ändern Sie die Zeile `enabled=0` , `enabled=1` **[ol6_addons]** oder **[ol7_addons]** in der Datei.

Um die neueste Version von Azure Linux-Agent installieren, geben Sie:


    #sudo yum install WALinuxAgent

Wenn Sie das Add-on-Repository finden können Sie einfach diese Zeilen am Ende der Datei .repo nach der Oracle Linux Version hinzufügen:

Für Oracle Linux 6 virtuelle Computer:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Für Oracle Linux 7 virtuelle Computer:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Geben Sie:

    #sudo yum update WALinuxAgent

Normalerweise ist dies erforderlich, aber gehen Sie folgendermaßen vor, wenn Sie aus irgendeinem Grund direkt von https://github.com installieren müssen.


## <a name="install-wget"></a>Wget installieren

Anmeldung mit SSH VM.

Installieren von Wget (es gibt einige Distributionen, die nicht standardmäßig wie Redhat CentOS und Oracle Linux-Versionen 6.4 und 6.5 installieren) eingeben `#sudo yum install wget` in der Befehlszeile.


## <a name="download-the-latest-version"></a>Die aktuelle Version herunterladen

[Version von Azure Linux-Agent im GitHub](https://github.com/Azure/WALinuxAgent/releases) in eine Webseite zu öffnen und die letzte Versionsnummer erfahren. (Suchen Sie Ihre aktuelle Version eingeben `#waagent --version`.)

### <a name="for-version-20x-type"></a>Für Version 2.0.x, Typ:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Verwendet Version 2.0.14 als Beispiel die folgende Zeile:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Für Version 2.1.x oder später eingeben:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Verwendet Version 2.1.0 als Beispiel die folgende Zeile:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Installieren Sie den Azure Linux-Agent

### <a name="for-version-20x-use"></a>Für Version 2.0.x verwenden:

 Machen Sie Waagent ausführbar:

    #chmod +x waagent

 Kopieren der ausführbaren Datei/usr/Sbin / neu.

  Für die meisten Linux verwenden:

    #sudo cp waagent /usr/sbin

  CoreOS verwenden:

    #sudo cp waagent /usr/share/oem/bin/

  Ist dies eine neue Installation von Azure Linux-Agent ausführen:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Für Version 2.1.x verwenden:

Sie müssen das Paket installieren `setuptools` zuerst finden Sie [hier](https://pypi.python.org/pypi/setuptools). Führen Sie

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Starten Sie den Dienst waagent

Für die meisten Linux-Distributionen:

    #sudo service waagent restart

Für Ubuntu verwenden:

    #sudo service walinuxagent restart

CoreOS verwenden:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Bestätigen Sie die Azure Linux-Agent-version

    #waagent -version

Für CoreOS funktioniert der obige Befehl nicht.

Sie sehen, dass Azure Linux-Agent-Version auf die neue Version aktualisiert wurde.

Weitere Informationen zu Azure Linux-Agent finden Sie unter [Azure Linux Agent README](https://github.com/Azure/WALinuxAgent).
