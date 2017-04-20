<properties 
   pageTitle="DNS-Einstellung in einer Konfigurationsdatei des virtuellen Netzwerks angeben | Microsoft Azure"
   description="Wie DNS-Server in einem virtuellen Netzwerk über ein virtuelles Netzwerk-Konfigurationsdatei im klassischen Bereitstellungsmodell ändern"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-Einstellung in einer Konfigurationsdatei des virtuellen Netzwerks angeben

Eine Netzwerk-Konfigurationsdatei enthält zwei Elemente, mit denen Sie Einstellungen (DNS = Domain Name System): **DnsServers** und **DnsServerRef**. Zum Hinzufügen einer Liste der DNS-Server die IP-Adressen und Verweisnamen **DnsServers** Element angeben. Ein **DnsServerRef** -Element können Sie angeben, welche DNS-Servereinträge DnsServers Element für verschiedene Websites innerhalb des virtuellen Netzwerks verwendet werden.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell.

Netzwerk-Konfigurationsdatei enthalten die folgenden Elemente. Der Titel jedes Elements ist mit einer Seite verknüpft, die zusätzliche über die Einstellung des Elements Informationen.

>[AZURE.IMPORTANT] Informationen zum Netzwerk-Konfigurationsdatei konfigurieren finden Sie in [einem virtuellen Netzwerk mithilfe einer Netzwerk-Konfigurationsdatei konfigurieren](virtual-networks-using-network-configuration-file.md). Informationen über jedes Element im Netzwerk Konfigurationsdatei enthaltenen anzeigen Sie [Azure Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100.aspx)

[DNS-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Das **Name** -Attribut im Element **DnsServer** dient als Referenz für das **DnsServerRef** -Element. Es stellt nicht den Hostnamen des DNS-Servers. Jeder Attributwert **DnsServer** muss für das gesamte Microsoft Azure-Abonnement eindeutig sein.

[Virtuelle Websites Netzwerkelement](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Um diese Einstellung für virtuelle Netzwerke Element angeben, müssen sie zuvor im DNS-Element definiert werden. Ein Name Wert im Element DNS- *Namen*des muss den DnsServerRef *Name* im virtuellen Netzwerke Element verweisen.

## <a name="next-steps"></a>Nächste Schritte

- Kennen Sie [Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093).
- Kennen Sie [Azure Service Configuration Schema](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Konfigurieren eines virtuellen Netzwerks mithilfe von Netzwerk-Konfigurationsdateien](virtual-networks-using-network-configuration-file.md).
