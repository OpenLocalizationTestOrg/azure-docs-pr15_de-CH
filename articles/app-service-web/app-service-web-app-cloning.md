<properties
    pageTitle="Web App Klonen mit PowerShell"
    description="Erfahren Sie, wie Ihr Web Apps neue Web Apps mit PowerShell klonen."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service-App mit PowerShell Klonen#

Mit Microsoft Azure PowerShell Version 1.1.0 wurde eine neue Option neu AzureRMWebApp hinzugefügt, die dem Benutzer eine vorhandene Web App eine neu erstellte Anwendung in einer anderen Region oder in derselben Region Klonen ermöglichen würde. Dadurch können Kunden eine Anzahl von apps in verschiedenen Regionen schnell und einfach.

Cloning-App ist zurzeit nur für Premium-Tier-Anwendung Servicepläne unterstützt. Das neue Feature verwendet die gleichen Grenzen wie Web Apps Sicherungsfeature, finden Sie unter [Sichern einer Web-app in Azure App Service](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Mit Informationen Ressourcenmanager Azure Azure PowerShell Grundlage überprüfen Cmdlets zu Web-Apps [Azure-Ressourcen-Manager basiert PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Klonen einer vorhandenen Anwendung ##

Szenario: Eine vorhandene Web app südlichen zentralen USA Region der Benutzer möchte den Inhalt auf eine neue Web app US North Central Region klonen. Dies kann mithilfe der Azure-Ressourcen-Manager Version des PowerShell-Cmdlets erstellt eine neue Webanwendung mit der Option - SourceWebApp erfolgen.

Kenntnis der Gruppe Ressourcenname, der Quelle Web app enthält, können wir folgenden PowerShell-Befehl Quelle Web app Informationen (in diesem Fall Webapp Quelle bezeichnet):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Erstellen einer neuen App Service-Plan können wir neue AzureRmAppServicePlan Befehl wie im folgenden Beispiel

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Mit dem Befehl neu AzureRmWebApp können wir neue Web app im Bereich US North Central erstellen und Binden an eine bestehende Premium Tier App Service-Plan Außerdem können derselben Ressourcengruppe als Quelle Web app verwenden oder eine neue Ressourcengruppe definieren, folgende zeigt:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Klonen eine vorhandenen Web app auch alle zugehörigen Bereitstellung slots, der Benutzer mit der IncludeSourceWebAppSlots-Parameter muss folgende PowerShell-Befehl diesen Parameter mit dem Befehl neu AzureRmWebApp veranschaulicht:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Klonen eine vorhandenen Web app innerhalb derselben Region der Benutzer eine neue Ressourcengruppe und einen neuen app-Dienst muss in derselben Region planen und dann mithilfe den folgenden PowerShell-Befehl Web app Klonen

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen einer vorhandenen Anwendung einer App Service-Umgebung ##

Szenario: Eine vorhandene Web app südlichen zentralen USA Region der Benutzer möchte Inhalt zu einem neuen Web app eine vorhandene Anwendung Service Umgebung (ASE) Klonen.

Kenntnis der Gruppe Ressourcenname, der Quelle Web app enthält, können wir folgenden PowerShell-Befehl Quelle Web app Informationen (in diesem Fall Webapp Quelle bezeichnet):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Die ASE-Name und der ASE gehört Ressourcengruppenname bekannt sein, Benutzer können den Befehl neu AzureRmWebApp die neuen Web app in vorhandenen ASE erstellen: Folgendes veranschaulicht, dass

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Standortparameter legacy Grund erforderlich ist, aber beim Erstellen einer Anwendung in einer ASE wird ignoriert. 

## <a name="cloning-an-existing-app-slot"></a>Klonen einer vorhandenen App-Steckplatz ##

Szenario: Der Benutzer möchte eine vorhandene Web App Steckplatz entweder ein neues Web App oder ein neuer Slot Web App klonen. Neue Web App kann im Bereich Web App-Steckplatz oder in einer anderen Region.

Kenntnis der Gruppe Ressourcenname, der Quelle Web app enthält, können wir folgenden PowerShell-Befehl Quelle Web app Steckplatz Informationen (in diesem Fall Webappslot Quelle bezeichnet) Web App Quell-Webapp gebunden:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Das folgende Beispiel veranschaulicht, einen Klon der Quelle Web app eine neue Web-app:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Traffic Manager beim Klonen einer Anwendung konfigurieren ##

Mit mehreren webapps erstellen und Konfigurieren von Azure Traffic Manager um Datenverkehr für diese Web-apps, wird n wichtiges Szenario um sicherzustellen, dass Kunden apps sind hochverfügbare, beim Klonen einer vorhandenen Webanwendung haben Sie die Option Profil-Manager Datenverkehr oder eine vorhandene Verbindung sowohl webapps-Beachten Sie, dass nur Azure Ressourcen-Manager der Traffic Manager unterstützt wird.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Erstellen eines neuen Profils Traffic Manager beim Klonen einer Anwendung ###

Szenario: Der Benutzer möchte eine Webanwendung in eine andere Region beim Konfigurieren einer Azure Ressourcenmanager Datenverkehr-Manager-Profil, die beide webapps enthalten klonen. Erstellen eines Klons Source Web App zu einem neuen Web app beim Konfigurieren eines neuen Profils Traffic Manager veranschaulicht Folgendes:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Neue Web App zu einem bestehenden Profil Traffic Manager geklont ###

Szenario: Der Benutzer bereits ein Ressourcenmanager Azure Datenverkehr-Manager-Profil, das er sowohl webapps als Endpunkte hinzufügen möchten. Dazu müssen Sie zuerst die Traffic Manager Profil Id einbauen, benötigen wir die Abonnement-Id, den Namen und vorhandenen Traffic Manager-Profilnamen.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Traffic Manager-Id haben, wird Folgendes veranschaulicht Klon Quelle Web app zu einem neuen Web app beim Hinzufügen zu einem vorhandenen Traffic Manager-Profil erstellen:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuelle Einschränkungen ##

Diese Funktion ist zurzeit in der Vorschau, wir arbeiten daran, neue Funktionen hinzufügen, sind folgende bekannte Einschränkung in der aktuellen Version der Anwendung Klonen:

- Automatische Einstellungen werden nicht geklont.
- Sicherungszeitplan Einstellungen werden nicht geklont.
- VNET-Einstellungen werden nicht geklont.
- App Erkenntnisse werden nicht automatisch auf Ziel Web app eingerichtet
- Einfache Authentifizierung Einstellungen werden nicht geklont.
- Kudu Erweiterung werden nicht geklont.
- Tipp Regeln werden nicht geklont.
- Inhalte werden nicht geklont.


### <a name="references"></a>Referenzen ###
- [Azure Ressourcenmanager basierte PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App Klonen mit Azure-Portal](app-service-web-app-cloning-portal.md)
- [Sichern Sie eine Webanwendung in Azure App Service](web-sites-backup.md)
- [Azure Resource Manager Unterstützung für Azure Traffic Manager Vorschau](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Einführung in App Service-Umgebung](app-service-app-service-environment-intro.md)
- [Mithilfe von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md)
