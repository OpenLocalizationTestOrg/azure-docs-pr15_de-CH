<properties
    pageTitle="Installiert MySQL auf einer VM OpenSUSE | Microsoft Azure"
    description="Informationen Sie zum MySQL auf einem Computer OpenSUSE Linux VMirtual in Azure installieren."
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
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Installieren von MySQL auf einem virtuellen Computer ausgeführt OpenSUSE Linux in Azure

[MySQL] [ MySQL] ist eine beliebte, Open-Source-SQL-Datenbank. Dieses Lernprogramm zeigt wie eine virtuelle Maschine mit OpenSUSE Linux und Installieren von MySQL.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Erstellen Sie einen virtuellen Computer mit OpenSUSE Linux

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Installieren und Ausführen von MySQL auf dem virtuellen Computer

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Nächste Schritte
Informationen über MySQL finden Sie in der [MySQL-Dokumentation][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

