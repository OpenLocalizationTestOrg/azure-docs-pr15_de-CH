<properties
    pageTitle="Erstellen von SharePoint-Serverfarmen | Microsoft Azure"
    description="Erstellen einer neuen SharePoint 2013 oder 2016 SharePoint-Farm in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>Erstellen von SharePoint-Serverfarmen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassisch.

## <a name="sharepoint-2013-farms"></a>SharePoint 2013 Farmen

Mit Microsoft Azure Portal Marketplace können Sie vorkonfigurierte SharePoint Server 2013 Farmen schnell erstellen. Dies sparen viel Zeit Sie eine SharePoint-Farm Basis- oder hoher Verfügbarkeit für eine Test-/Umgebung oder wenn Sie SharePoint Server 2013 als Collaboration-Lösung für Ihr Unternehmen evaluieren.

> [AZURE.NOTE] **SharePoint-Serverfarm** Element in Azure Marketplace der Azure-Portal wurde entfernt. Es wurde mit **SharePoint 2013 nicht-HA** und **SharePoint 2013 HA Farm** ersetzt.

Einfache SharePoint-Serverfarm besteht aus drei virtuellen Computer in dieser Konfiguration.

![Sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Diese Farmkonfiguration können Sie für ein vereinfachtes Setup für SharePoint-app-Entwicklung oder Evaluierung erstmals SharePoint 2013.

Basic (drei Server) SharePoint-Farm zu erstellen:

1. Klicken Sie [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klicken Sie auf **Bereitstellen**.
3. Klicken Sie im **SharePoint 2013 HA nichtlandwirtschaftlichen** auf **Erstellen**.
4. Geben Sie Einstellungen auf **SharePoint 2013 erstellen HA nichtlandwirtschaftlichen** Bereich an, und klicken Sie auf **Erstellen**.

Hohe Verfügbarkeit SharePoint-Farm besteht aus neun virtuelle Computer in dieser Konfiguration.

![Sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Diese Farmkonfiguration können um höhere Client lädt und hohe Verfügbarkeit der externen Website SQL Server AlwaysOn Availability Gruppen für eine SharePoint-Farm zu testen. Sie können auch diese Konfiguration für SharePoint-Anwendungsentwicklung in einer Umgebung mit hoher Verfügbarkeit.

Hohe Verfügbarkeit (neun Server) SharePoint-Farm zu erstellen:

1. Klicken Sie [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klicken Sie auf **Bereitstellen**.
3. Klicken Sie im **SharePoint 2013 HA Farm** auf **Erstellen**.
4. Geben Sie Einstellungen auf sieben Schritte im Bereich **Erstellt SharePoint 2013 HA Farm an** , und klicken Sie auf **Erstellen**.

> [AZURE.NOTE] Sie können nicht mit einer Azure-Testversion **SharePoint 2013 nicht-HA-Farm** oder **SharePoint 2013 HA Farm** erstellen.

Azure-Portal erstellt beide dieser Betriebe in einem virtuellen Netzwerk Cloud nur eine Webpräsenz Internetzugriff. Es gibt keine Standort-zu-Standort-VPN oder ExpressRoute Verbindung zum Netzwerk Ihrer Organisation.

> [AZURE.NOTE] Wenn Sie das Basic oder Hochverfügbarkeits-SharePoint-Serverfarmen mithilfe des Azure-Portals, Sie können keine vorhandene Ressourcengruppe angeben. Um diese Einschränkung zu umgehen, erstellen Sie diese Betriebe mit Azure PowerShell. Weitere Informationen finden Sie unter [Erstellen SharePoint 2013 Test-/Betriebe mit Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>2016 SharePoint-Farmen

Finden Sie [in diesem Artikel](https://technet.microsoft.com/library/mt723354.aspx) auf einem Server SharePoint Server 2016 Farm erstellen.

![Sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Verwalten von SharePoint-Serverfarmen

Sie können die Server dieser Betriebe über Remotedesktopverbindungen verwalten. Weitere Informationen finden Sie [auf dem virtuellen Computer anmelden](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Von der zentralen Verwaltung SharePoint-Website können Sie eigene SharePoint-Applikationen und andere Funktionen konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Nächste Schritte

- Entdecken Sie zusätzliche [SharePoint-Konfigurationen](https://technet.microsoft.com/library/dn635309.aspx) in Azure Infrastrukturdienste.
