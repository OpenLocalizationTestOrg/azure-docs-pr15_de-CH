<properties
   pageTitle="Navigieren und Windows VM Bilder | Microsoft Azure"
   description="Informationen Sie zum Herausgeber, Angebot und SKU für Bilder bestimmen, wann einen Windows-Computer mit dem Ressourcen-Manager-Bereitstellungsmodell erstellen."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Navigieren Sie und wählen Sie Windows virtuellen Computerimages in Azure PowerShell oder CLI

Dieses Thema beschreibt wie Sie VM Image Herausgeber, bietet Skus und Versionen für jeden Standort in die bereitstellen kann. Ein Beispiel sind einige häufig verwendete Windows-VM Bilder:

## <a name="table-of-commonly-used-windows-images"></a>Tabelle häufig verwendete Windows-Abbilder


| PublisherName                        | Angebot                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise-optimiert-für-DW      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise-optimiert-für-OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 R2 Datacenter                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 Datacenter               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows Server-Technical Preview |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
