<properties
    pageTitle="Linux-Gäste Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie Linux-basierten virtuellen Maschinen Azure Stapel erstellen."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Maschinen Sie Linux virtuellen Azure Stapel

Hinzufügen eines Linux-basierten Abbilds in Azure Stapel Marketplace können Sie virtuelle Linux-Computer in Azure Stapel POC bereitstellen. Mehrere Linux-Anbietern bereitgestellt haben Bilder ein Azure Stapel POC hinzugefügt werden können oder eigene erstellen.

## <a name="download-an-image"></a>Ein Bild herunterladen

 1. Downloaden und Extrahieren von Bildbereichen Azure Stack-kompatiblen unter den folgenden Links oder eigene vorbereiten:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Extrahieren Sie das Bild VHD ggf. und [Fügen Sie das Abbild auf dem Markt](azure-stack-add-vm-image.md). Stellen Sie sicher, dass die `OSType` Parametersatz auf `Linux`.
 
 3. Nach dem Hinzufügen des Bilds auf dem Markt Markt Element erstellt und eine virtuellen Linux-Maschine bereitgestellt.
  
## <a name="prepare-your-own-image"></a>Bereiten Sie Ihr eigenes Bild

1. Bereiten Sie eigenes Linux Bild mithilfe einer der folgenden Anweisungen vor:
 - [Verteilung von centOS](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Red Hat Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Herunterladen Sie und installieren Sie der [Azure Linux-Agent](https://github.com/Azure/WALinuxAgent/)

    Azure Linux-Agent Version 2.1.3 oder höher Linux VM Azure Stapel bereit. Viele bereits aufgeführten Distributionen beinhalten diese Version des Agenten oder höher als Paket in ihre Repositories (üblicherweise `WALinuxAgent` oder `walinuxagent`). Allerdings ist die Version von Azure Agentenpaket unter 2.1.3 (2.0.18 oder unten), dann den Agent manuell installieren müssen. Die installierte Version ermittelt werden, aus der Paketname oder mit `/usr/sbin/waagent -version` auf dem virtuellen Computer.

    Gehen Sie Azure Linux-Agent manuell installieren-

 - Erstens [Github](https://github.com/Azure/WALinuxAgent/releases)herunterladen den neuesten Azure Linux-Agent wird:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Entpacken des Azure-Agents:

            # tar -vzxf v2.2.0.tar.gz

 - Installieren Sie Python-setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Installieren des Azure-Agents:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Systeme mit Python 2.x und Python 3.x installiert Side-by-Side müssen den folgenden Befehl ausführen:

        # sudo python3 setup.py install --register-service

    Weitere Informationen finden Sie unter Azure Linux-Agent [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Hinzufügen das Bild auf dem Markt](azure-stack-add-vm-image.md). Stellen Sie sicher, dass die `OSType` Parametersatz auf `Linux`.

4. Nach dem Hinzufügen des Bilds auf dem Markt Markt Element erstellt und eine virtuellen Linux-Maschine bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

[Häufig gestellte Fragen für Azure](azure-stack-faq.md)