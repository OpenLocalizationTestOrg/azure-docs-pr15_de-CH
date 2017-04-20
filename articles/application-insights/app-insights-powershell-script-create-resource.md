<properties 
    pageTitle="PowerShell-Skript zum Erstellen einer Anwendung Einblicke Ressource" 
    description="Automatische Erstellung von Application Insights-Ressourcen." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell-Skript zum Erstellen einer Anwendung Einblicke Ressource

*Anwendung Informationen ist in der Vorschau.*

Wenn Sie eine neue Anwendung- oder eine neue Version einer Anwendung - [Visual Studio](https://azure.microsoft.com/services/application-insights/)-Anwendung zum überwachen möchten, richten Sie eine neue Ressource in Microsoft Azure. Diese Ressource wird die Telemetriedaten aus Ihrer app analysiert und angezeigt werden. 

Sie können die Erstellung einer neuen Ressource mithilfe von PowerShell automatisieren.

Z. B. Wenn Sie eine Mobilgerät app entwickeln, ist es wahrscheinlich jederzeit werden mehrere veröffentlichte Versionen der Anwendung in Ihre Kunden. Sie möchten bei verschiedenen Versionen durcheinander Telemetrie Ergebnisse zu erhalten. So erhalten Sie den Buildprozess eine neue Ressource für jeden Build erstellen.

## <a name="script-to-create-an-application-insights-resource"></a>Skript zum Application Insights-Ressource erstellen

Finden Sie die entsprechenden Cmdlet-Spezifikationen:

* [Neue AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Neue AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShell-Skript*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Was mit den iKey

Jede Ressource wird durch seinen instrumentationsschlüssel (iKey) identifiziert. Der iKey ist eine Ausgabe des Erstellungsskripts Ressource. Buildskript soll der iKey Application Insights-SDK in Ihrer Anwendung eingebettet.

Gibt es zwei Arten den iKey SDK zur Verfügung stellen:
  
* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Oder im [Initialisierungscode](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Siehe auch

* [Erstellen Sie Anwendung Einblicke und Webressourcen Test aus Vorlagen](app-insights-powershell.md)
* [Einrichten der Überwachung von Azure Diagnostics mit PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Alarme mit PowerShell](app-insights-powershell-alerts.md)

 