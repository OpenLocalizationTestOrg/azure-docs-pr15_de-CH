<properties
 pageTitle="Planer PowerShell-Cmdlets Verweis"
 description="Planer PowerShell-Cmdlets Verweis"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Planer PowerShell-Cmdlets Verweis

In der folgenden Tabelle beschrieben und links auf der Referenzseite aller wichtigen Cmdlets in Azure Scheduler.

Azure PowerShell installieren und Ihr Azure-Abonnement zuordnen, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). 

Weitere Informationen zu [Azure-Ressourcen-Manager-Cmdlets](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).

|Cmdlets|Cmdlet-Beschreibung|
|---|---|
[AzureRmSchedulerJobCollection deaktivieren](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Deaktiviert eine Job-Auflistung. 
[AzureRmSchedulerJobCollection aktivieren](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Aktiviert einen Auftrag.
[AzureRmSchedulerJob abrufen](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Ruft Steuerprogrammaufträge.
[AzureRmSchedulerJobCollection abrufen](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Ruft Auftrag Sammlungen.
[AzureRmSchedulerJobHistory abrufen](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Ruft Auftrag Geschichte.
[Neue AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Einen HTTP-Auftrag erstellt.
[Neue AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Erstellt eine auftragsauflistung.
[Neue AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Ein Bus Warteschlangenauftrag erstellt.
[Neue AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Einen Bus Thema Auftrag erstellt.
[Neue AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Erstellt eine speichern-Warteschlangenauftrags. 
[AzureRmSchedulerJob entfernen](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Ein Planerauftrag entfernt.  
[AzureRmSchedulerJobCollection entfernen](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Job-Auflistung entfernt. 
[AzureRmSchedulerHttpJob festlegen](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Ändert einen Job Scheduler HTTP.
[AzureRmSchedulerJobCollection festlegen](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Job-Auflistung ändert. 
[AzureRmSchedulerServiceBusQueueJob festlegen](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Ein Bus Warteschlangenauftrag ändert.  
[AzureRmSchedulerServiceBusTopicJob festlegen](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Einen Bus Thema Auftrag ändert. 
[AzureRmSchedulerStorageQueueJob festlegen](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Ändert eine speichern-Warteschlangenauftrags.   

Weitere Informationen können Sie die folgenden Cmdlets ausführen: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Siehe auch


 [Was ist der Taskplaner?](scheduler-intro.md)

 [Azure Scheduler Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
