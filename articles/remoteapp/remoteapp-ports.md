
<properties
    pageTitle="Liste der Anschlüsse und URLs Whitelist für Azure RemoteApp bereitgestellt virtuelle Netzwerk Kunden | Microsoft Azure"
    description="Erfahren Sie, welche Ports und URLs Sie für die Kommunikation über Azure RemoteApp konfigurieren müssen."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Liste der Anschlüsse und URLs bei virtuellen Netzwerk zugänglich für Azure RemoteApp bereitgestellt 

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Folgendes gilt für Azure RemoteApp Cloud oder Hybrid-Auflistung, wenn Sie in einem virtuellen Netzwerk (VNET) bereitstellen. Lesen Sie weitere virtuelle Netzwerke [Übersicht über Virtual Network](../virtual-network/virtual-networks-overview.md). Wenn Sie eine Netzwerk-Sicherheitsgruppe (NSG) Einschränkung des Datenverkehrs an virtuelle Netzwerkressourcen für Azure RemoteApp ausgesuchten erstellt haben, stellen Sie sicher sind folgende zugänglich und über die Sicherheitsrichtlinien auf dem virtuellen Netzwerk erlaubt. Weitere Informationen zu Netzwerkgruppen Sicherheit lesen Sie bitte [wie ein Netzwerk ist? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp Subnetz benötigt Zugriff auf diese Endpunkte und URLs: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Ausgehend: TCP: 443, TCP: 10101 10175 
*    Optional – UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-Clients benötigen Zugriff auf diese Endpunkte und URLs: 

Ich meine Kunden Desktops Geräte usw. Personen zum Herstellen der apps in Azure RemoteApp-Sammlung bereitgestellt.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (Optionaler UDP-Ports sind für diese Adresse) 
-  https://Login.Windows.NET  
-  https://Login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Ausgehend: TCP: 443  
-  Optional - UDP: 3391 