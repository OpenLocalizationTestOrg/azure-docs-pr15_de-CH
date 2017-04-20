<properties 
    pageTitle="Preismodell Logik Apps | Microsoft Azure" 
    description="Details zur Funktionsweise der Preise in Logik-Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logik Apps Preismodell

Azure Logik Apps können Sie skalieren und Integration Workflows in der Cloud ausgeführt.  Im folgenden sind die Abrechnung und Preisplänen für Logik-Apps.

## <a name="consumption-pricing"></a>Preise für die Nutzung

Neu erstellte verwenden Logik Apps Verbrauch planen. Mit der Logik Apps Verbrauch Preismodell Zahlen Sie nur für Sie verwenden.  Logik-Apps werden nicht eingeschränkt, wenn mit einem Verbrauch.
Alle Aktionen, die in einem Testlauf einer Logik app-Instanz ausgeführt werden gemessen.

### <a name="what-are-action-executions"></a>Was sind Aktionen wie?

Jeder Schritt in der Definition einer Logik-app ist eine Aktion.  Dazu gehören Trigger-Control Flow Schritte wie Umständen Bereiche for each-Schleife bis Schleifen Aufrufe Connectors und Aufrufe von systemeigenen Aktionen.
Trigger sind nur spezielle Aktionen, die eine neue Instanz einer Anwendung Logik beim Eintreten eines bestimmten Ereignisses instanziiert werden.  Es gibt zahlreiche unterschiedliche Verhaltensweisen für Trigger, die wie die Anwendung Logik gemessene beeinflussen könnte.

-   **Trigger polling** -Trigger ständig fragt einen Endpunkt, bis er eine Nachricht empfängt, die das Erstellen einer neuen Instanz einer Anwendung Logik erfüllt.  Das Abrufintervall kann im Trigger im Designer Logik Apps konfiguriert werden.  Jede Anforderung abrufen zählen, wenn eine neue Instanz einer Anwendung Logik erstellt nicht als Aktion Ausführung.

-   **Webhook Trigger** -dieser Trigger wartet ein Client eine Anforderung an einen bestimmten Endpunkt senden.  Jede Anforderung Webhook Endpunkt zählt als eine Aktion ausführen. Die Anforderung und die HTTP-Webhook-Trigger stellen beide Webhook.

-   **Serie Trigger** – dieser Trigger erstellt eine neue Instanz der Anwendung Logik basierend auf das Serienintervall Trigger konfiguriert.  Beispielsweise kann Trigger Serie konfiguriert werden alle 3 Tage oder sogar jede Minute.

Logik Apps Ressource Blatt in der Geschichte der Trigger können Trigger wie angezeigt werden.

Alle Aktionen, die ausgeführt wurden, als die Ausführung einer Aktion gemessen werden, ob sie erfolgreich waren oder fehlgeschlagen sind.  Aktionen, die durch eine Bedingung nicht erfüllt übersprungen wurden oder Aktionen nicht ausgeführt werden, da die Logik vorzeitig beendet werden als Aktion wie nicht gezählt.

Aktionen in Schleifen zählen pro Iteration der Schleife.  Angenommen, eine Aktion in einem für jede Schleife durchlaufen eine Liste der 10 Elemente als die Anzahl der Elemente in der Liste (10) multipliziert mit der Anzahl der Aktionen in der Schleife (1) gezählt werden und für die Initiierung der Schleife, die in diesem Beispiel (10 * 1) + 1 = 11 Aktion wie.

Logik deaktiviert sind Apps keinen neue Instanzen instanziiert und daher während der Zeit, die sie deaktiviert nicht erhalten berechnet.  Achten Sie darauf, dass nach dem Deaktivieren einer Anwendung Logik etwas Zeit für die Instanzen zu stoppen vor vollständig deaktiviert dauert.

## <a name="app-service-plans"></a>App Service-Pläne

App Service-Pläne müssen mehr Logik-App erstellen.  Sie können auch eine App Service-Plan mit einer vorhandenen Anwendung Logik verweisen.  Logik apps mit App-Dienst zuvor erstellte weiterhin Verhalten vor, je nach Plan ausgewählt, wird erhalten gedrosselt täglich eine Zahl überschritten werden und nicht belastet mit der Aktion ausführen.

App Service-Pläne und ihre tägliche Ausführung zulässige Aktion:

| |Frei/Shared/Basic|Standard|Premium|
|---|---|---|---|
|Aktion wie pro Tag| 200|10.000|50.000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Konvertieren Sie aus der App Service-Plan Preise

Eine App Service-Plan für den Verzehr Logik Anwendung verweisen möchten, können Sie einfach [führen die folgenden PowerShell-Skript](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Überprüfen Sie zuerst die [Azure PowerShell-Tools](https://github.com/Azure/azure-powershell) installiert sind.

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Konvertieren Sie aus App Service-Plan, Verbrauch

Ändern Entfernen einer Logik App, die eine App Service-Plan, Verbrauch Modell zugeordnet auf App Service-Plan in der Definition der Logik-App.  Dies kann durch einen Aufruf von PowerShell-Cmdlets erfolgen:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Preisgestaltung

Preisinformationen finden Sie [Logik Apps Preise](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Nächste Schritte

- [Eine Übersicht über Logik Apps][whatis]
- [Erstellen Sie Ihre erste Anwendung Logik][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

