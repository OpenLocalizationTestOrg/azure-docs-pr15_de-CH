<properties 
   pageTitle="DNS-Einstellung in einer Konfigurationsdatei Service angeben | Microsoft Azure"
   description="die Standardeinstellungen des DNS-Dienst für virtuelle Netzwerk mit angeben"
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
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-Einstellung in einer Dienstkonfigurationsdatei angeben

## <a name="dns-elements"></a>DNS-Elemente

Eine Dienstkonfigurationsdatei enthalten eine DnsServers Element mit einer Liste von IPv4-Adressen der Server (DNS = Domain Name System), die den Dienst verwenden wird. Bei Dienstkonfigurationsdatei Vorrang vor Einstellungen in der Netzwerk-Konfigurationsdatei. Weitere Informationen finden Sie unter [Azure Service Configuration Schema (.cscfg Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Das **Name** -Attribut im Element **DnsServer** dient nur als ein Verweis. Es stellt nicht den Hostnamen des DNS-Servers. Jeder Attributwert **DnsServer** muss über das gesamte Microsoft Azure-Abonnement eindeutig sein.

## <a name="see-also"></a>Siehe auch

[Azure Service Configuration Schema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network Configuration Schema](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurieren eines virtuellen Netzwerks mithilfe von Netzwerk-Konfigurationsdateien](http://go.microsoft.com/fwlink/?LinkId=248094)

[Virtuelles Netzwerk Einstellungen im Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkId=248092)

