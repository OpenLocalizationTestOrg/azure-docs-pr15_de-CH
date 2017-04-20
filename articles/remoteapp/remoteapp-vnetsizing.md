
<properties
    pageTitle="Informationen für VNET in Azure RemoteApp Größe | Microsoft Azure"
    description="Erfahren Sie mehr über die IP-Adresse an Azure RemoteApp mit einem VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Größenangaben für VNET in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Bei Verwendung von Azure RemoteApp virtual Network (VNET) verwendet RemoteApp IP-Adressen im Subnetz. Basierend auf der RemoteApp-Dienst müssen Sie hat Ihr Subnetz genügend IP-Adressen für RemoteApp virtuelle Maschinen. Während dieser Anleitung Größe nicht perfekt angegebenen wie läuft RemoteApp dynamisch und virtuellen Maschinen innerhalb einer Auflistung dreht, wird es der Teilnetzes schätzen helfen. Dies ist besonders wichtig, nachdem ein RemoteApp-Dienst in einem VNET platziert, Sie Subnetz vergrößern können nicht ohne RemoteApp.

Für jedes RemoteApp-Auflistung, die maximale Kapazität ausführen möchten, sollten Sie 100 IP-Adressen verfügen. Wenn Sie eine RemoteApp-Sammlung im Standard Plan und maximal 500 Benutzer sollen, sollten Sie beispielsweise 100 IP-Adressen für diese Auflistung verfügen. Ebenso benötigen Sie im Basisplan, die 800 Benutzer 100 IP-Adressen für RemoteApp-Auflistung. Möchten Sie weniger Benutzer (kleiner als das Maximum), können Sie die IP-Adressen pro Auflistung reduzieren. Die minimale Subnetz größenanforderung ist 30 IP-Adressen (von 27).

Sehen Sie sich die folgenden Informationen zu Ihrem VNET ist konfiguriert und arbeiten Propertly:

- [Migrieren von persönlichen VNET eine Azure-VNET](remoteapp-migratevnet.md)
- [Überprüfen der Azure-VNET mit Azure RemoteApp](remoteapp-vnet.md)
