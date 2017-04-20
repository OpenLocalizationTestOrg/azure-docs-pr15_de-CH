<properties 
   pageTitle="Verwendung öffentliche IP-Adressen in einem virtuellen Netzwerk"
   description="Informationen Sie zum Konfigurieren eines virtuellen Netzwerks Verwendung öffentliche IP-Adressen"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Öffentliche IP-Adressraum in einem virtuellen Netzwerk (VNet)

Virtuelle Netzwerke (VNets) können sowohl öffentliche als auch private (RFC 1918-Adressblöcke) IP-Adresse Leerzeichen enthalten. Beim Hinzufügen eines öffentlichen IP-Adressbereich wird diese als Teil des privaten VNet IP-Adressraum behandelt, der nur innerhalb der VNet miteinander verbundenen VNets und lokalen erreichbar ist.

Die folgende Abbildung zeigt eine VNet, die öffentliche und private IP-Adresse Leerzeichen enthält.

![Grundlegende öffentliche IP-Adresse](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Wie fügen Sie einen öffentlichen IP-Adressbereich

Hinzufügen einen öffentlichen IP-Adressbereich einen privaten IP-Adressbereich hinzufügen würden genauso; mit einer Datei *Netcfg* oder die Konfiguration in der [Azure-Portal](http://portal.azure.com)hinzufügen. Beim Erstellen des Vnets oder zurückgehen und danach hinzufügen, können Sie einen öffentlichen IP-Adressbereich hinzufügen. Das folgende Beispiel zeigt sowohl öffentliche als auch private IP-Adresse Leerzeichen in derselben VNet konfiguriert.

![Öffentliche IP-Adresse im Portal](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Gibt es Einschränkungen?

Es gibt einige IP-Adressbereiche, die nicht zulässig sind:

- 224.0.0.0/4 (Multicast)

- 255.255.255.255/32 (Broadcast)

- 127.0.0.0/8 (Schleife)

- 169.254.0.0/16 (Link-Local)

- 168.63.129.16/32 (interne DNS)

## <a name="next-steps"></a>Nächste Schritte

[Ein VNet verwendeten DNS-Server verwalten](virtual-networks-manage-dns-in-vnet.md)