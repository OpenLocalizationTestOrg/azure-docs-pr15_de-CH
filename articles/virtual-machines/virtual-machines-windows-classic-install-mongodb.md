<properties
    pageTitle="MongoDB auf Windows VM installieren | Microsoft Azure"
    description="Informationen Sie zum MongoDB auf eine Azure-VM erstellt mit dem klassischen Bereitstellungsmodell unter Windows Server installieren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>MongoDB auf Windows VM installieren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Installieren und konfigurieren Sie mit dem Ressourcen-Manager-Bereitstellungsmodell MongoDB, finden Sie [in diesem Artikel](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] ist eine beliebte Open Source, leistungsfähige NoSQL-Datenbank. Dieser Artikel führt Sie durch die Erstellung von Windows Server mit eines virtuellen Computers (VM) im [klassischen Azure-Portal][AzurePortal]. Dann erstellen und die VM vor dem Installieren und Konfigurieren von MongoDB Datenträger zuordnen. Wenn Sie eine vorhandene VM in Azure, die Sie verwenden möchten verfügen, springen Sie direkt zum [Installieren und Konfigurieren von MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Erstellen Sie einen virtuellen Computer unter Windows Server

Gehen Sie folgendermassen vor, um einen virtuellen Computer erstellen.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Fügen Sie einen Endpunkt für MongoDB beim virtuellen Computer erstellen können, und wie folgt konfiguriert: als **Mongo**benennen, verwenden **TCP** als Protokoll und öffentliche und private Ports auf **27017**festgelegt.

## <a name="attach-a-data-disk"></a>Legen Sie einen Datenträger
Speicher für den virtuellen Computer bereitstellen fügen Sie Datenträger an und dann initialisieren Sie, sodass Windows verwendet werden kann. Haben Sie bereits einen Datenträger, vorhandenen Datenträger anfügen oder einen leeren Datenträger anfügen.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Informationen auf der Festplatte finden Sie unter "wie: Initialisieren einen neuen Datenträger in WindowsServer" wie [einen Datenträger auf einem Windows-Computer an](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Installieren und Ausführen von MongoDB auf dem virtuellen Computer

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Zusammenfassung
In diesem Lernprogramm haben Sie gelernt, wie eine virtuelle Maschine mit Windows Server Remote verbinden und legen einen Datenträger.  Sie haben gelernt, wie installieren und Konfigurieren von MongoDB auf Windows basierenden virtuellen Computer. Sie können nun MongoDB auf Windows basierenden virtuellen Computer zugreifen, folgenden erweiterten Themen in der [MongoDB-Dokumentation][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
