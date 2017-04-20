<properties
    pageTitle="Verwalten von Azure Service Bus Azure Automatisierung | Microsoft Azure"
    description="Informationen Sie zum Azure Automation Service Azure Service Bus zu verwenden."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Verwalten von Azure Service Bus Azure Automatisierung

Dieses Handbuch wird Ihnen Azure Automation Service und wie sie zur Vereinfachung des Managements von Azure Service Bus verwendet werden kann.

## <a name="what-is-azure-automation"></a>Was ist Automatisierung Azure?

[Azure-Automatisierung](../automation/automation-intro.md) ist eine Azure für vereinfachtes Management durch Automatisierung und Konfiguration des gewünschten Cloud. Azure Automatisierung manueller, können wiederholt, langlebige und fehleranfällige Aufgaben automatisiert werden um Zuverlässigkeit, Effizienz und Wert für Ihr Unternehmen Zeit erhöhen.

Azure Automation bietet eine hochgradig zuverlässige und hochverfügbare Ausführung Workflowmodul, das an Ihre Bedürfnisse. In Azure Automation können Prozesse manuell 3rd Party Systeme oder in bestimmten Zeitabständen gestartet so dass Aufgaben genau bei Bedarf.

Betriebskosten reduzieren und frei IT und DevOps Arbeit konzentrieren, die Unternehmen Mehrwert durch Verschieben der Cloud Verwaltungsaufgaben von Azure Automation automatisch ausgeführt werden.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Helfen Azure Automatisierung Azure Service Bus verwalten?

Sie können Service Bus mit Azure Automation mit [Service Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx)verwalten. Azure Automation können Sie PowerShell-Skripts so viele Service Bus Aufgaben mithilfe der REST-API ausführen. Sie können auch die REST API-Aufrufe in Azure Automation mit den Cmdlets für andere Dienste Azure Azure Services und 3. Systeme komplexe Aufgaben automatisieren koppeln.

Einige Beispiele der Verwendung von PowerShell zu Azure Service Bus:

* [Benutzerdefinierte PowerShell-Cmdlets zur Verwaltung von Azure Service Bus-Warteschlangen](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Service Bus-Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skripts erstellen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Erstellen Sie Azure Service Bus Namespaces mit PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

PowerShell-Modul für die Arbeit mit Azure Servicebus Automatisierung Runbooks kann [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0)heruntergeladen werden.


## <a name="next-steps"></a>Nächste Schritte

Nun, Sie die Grundlagen von Azure Automation und wie es beherrschen zu Azure Service Bus verwendet werden kann, folgen Sie diesen Links erfahren Sie mehr über Azure Automation.

* Finden Sie unter [Erste Schritte abschließen](../automation/automation-first-runbook-graphical.md) Azure Automatisierung
* Siehe [Service Bus mit PowerShell](service-bus-powershell-how-to-provision.md) verwalten
