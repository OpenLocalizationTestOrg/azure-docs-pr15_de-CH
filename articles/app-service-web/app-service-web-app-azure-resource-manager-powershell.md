<properties
    pageTitle="Azure Ressourcen-Manager-basierten PowerShell Befehle Azure Web App | Microsoft Azure"
    description="Erfahren Sie mehr über das neue Azure-Ressourcen-Manager-basierten PowerShell Befehle verwenden, um Ihre Azure Web Apps verwalten."
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
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Mithilfe von Azure Resource Manager-basierten PowerShell Azure webapps verwalten#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Mit Microsoft Azure PowerShell Version 1.0.0 haben neue Befehle wurden hinzugefügt, die Benutzer mit Azure-Ressourcen-Manager-basierten PowerShell-Befehlen Web Apps verwalten können.

Zum Verwalten von Ressourcen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md). 

Zur vollständigen Liste von Parametern und Optionen für die PowerShell-Cmdlets finden Sie in der [Vollständigen Cmdlet Verweis von PowerShell-Cmdlets Web App Azure-Ressourcen-Manager basiert](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Verwalten von App Service-Pläne ##

### <a name="create-an-app-service-plan"></a>Erstellen eines App Service-Plans ###
Verwenden Sie zum Erstellen eines app Service-Plans **Neu-AzureRmAppServicePlan** -Cmdlet.

Folgenden werden die verschiedenen Parameter:

-   **Name**: Name der app Service-Plan.
-   **Ort**: Plan-Ort-service.
-   **ResourceGroupName**: Ressourcengruppe, die neu erstellte app Service-Plan enthält.
-   **Tier**: gewünschten Tarif (Standard ist kostenlos, andere Optionen sind freigegeben, Basic, Standard und Premium)
-   **WorkerSize**: die Größe (Standard ist small Basic, Standard oder Premium Tier-Parameter angegeben wurde. Weitere Optionen sind Medium und Large.)
-   **NumberofWorkers**: die Anzahl der Arbeitskräfte in der app service-Plan (Standardwert ist 1). 

Dieses Cmdlet verwendet wird:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Erstellen eines App Service-Plans in einer App Service-Umgebung ###
Verwenden Sie zum Erstellen eines app Service-Plans in einer app Service-Umgebung Befehls **Neu AzureRmAppServicePlan** Befehl mit zusätzlichen Parametern der ASE und ASE Ressourcengruppennamen angeben.

Dieses Cmdlet verwendet wird:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Um weitere Informationen zu app Service-Umgebung überprüfen Sie [Einführung App Service-Umgebung](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Liste vorhandenen App Service-Pläne ###

Verwenden Sie zum Auflisten der vorhandenen Anwendung Servicepläne Cmdlet " **Get-AzureRmAppServicePlan** ".

Zum Auflisten aller app Servicepläne unter Abonnements verwenden: 

    Get-AzureRmAppServicePlan

Zum Auflisten aller app Servicepläne unter einer bestimmten Ressourcengruppe zu verwenden:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Zu einem bestimmten app Service-Plan verwenden:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurieren einer vorhandenen App Service-Plan ###

Verwenden Sie zum Ändern einer vorhandenen app Service-Plan für das Cmdlet " **Set-AzureRmAppServicePlan** ". Sie können die Ebene Arbeitskraft Größe und Anzahl von Arbeitskräften 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Skalieren eines App Service-Plans ####

Um einen vorhandenen App Service-Plan zu skalieren, verwenden:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Ändern der Größe der Arbeitskraft eine App Service-Plan ####

Zum Ändern der Größe der Arbeitnehmer in einer vorhandenen App Service-Plan verwenden:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Ändern die Ebene der App Service-Plan ####

Ändern Sie die Ebene einer vorhandenen App Service-Plan verwenden:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Löschen einer vorhandenen App Service-Plan ###

Zum Löschen eines vorhandenen app Service-Plans müssen alle zugeordneten webapps verschoben oder zuerst gelöscht werden. Löschen den app Service-Plan **Entfernen-AzureRmAppServicePlan** -Cmdlet anschließend verwenden.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Verwalten von App Service webapps ##

### <a name="create-a-web-app"></a>Erstellen Sie eine Webanwendung ###

Verwenden Sie **New-AzureRmWebApp** -Cmdlet Web app erstellen.

Folgenden werden die verschiedenen Parameter:

- **Name**: Name für die Web-app.
- **AppServicePlan**: für Serviceplan Web app gehostet.
- **ResourceGroupName**: Ressourcengruppe, die App Service-Plan hostet.
- **Ort**: app-Website.

Dieses Cmdlet verwendet wird:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Erstellen Sie eine Webanwendung in einer App Service-Umgebung ###

Eine Webanwendung in eine App Service-Umgebung (ASE) erstellen. Befehl gleiche **Neu AzureRmWebApp** mit zusätzlichen Parametern an die ASE und Ressource-Gruppennamen, dem ASE gehört.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Um weitere Informationen zu app Service-Umgebung überprüfen Sie [Einführung App Service-Umgebung](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Löschen Sie ein vorhandenes Web App ###

So löschen Sie eine vorhandenes Web app **Entfernen-AzureRmWebApp** -Cmdlet verwenden, müssen Sie Web app und die Ressourcengruppennamen angeben.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Liste vorhandene Web Apps ###

Verwenden Sie zum Auflisten der vorhandenen Web-apps das Cmdlet " **Get-AzureRmWebApp** ".

Zum Auflisten aller Web-apps unter Abonnements verwenden:

    Get-AzureRmWebApp

Zum Auflisten aller Web-apps unter einer bestimmten Ressourcengruppe zu verwenden:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Zu einem bestimmten Web app verwenden:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurieren Sie ein vorhandenes Web App ###

Um Einstellungen und Konfigurationen für eine vorhandene Web app zu ändern, verwenden Sie das Cmdlet " **Set-AzureRmWebApp** ". Eine vollständige Liste der Parameter überprüfen Sie die [Cmdlet-Verweis](https://msdn.microsoft.com/library/mt652487.aspx)

Beispiel (1): Verwenden Sie dieses Cmdlet Verbindungszeichenfolgen ändern

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

(2) Beispiel: Fügen Sie hinzu oder ändern Sie der app

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


(3) Beispiel: set Web app im 64-Bit-Modus ausführen

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Ändern Sie den Zustand einer vorhandenen Web App ###

#### <a name="restart-a-web-app"></a>Eine Webanwendung starten ####

Neustart eine Webanwendung müssen Sie die Gruppe und den Ressourcenschlüssel Web App angeben.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Eine Webanwendung beenden ####

Um eine Webanwendung zu beenden, müssen Sie die Gruppe und den Ressourcenschlüssel Web App angeben.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Eine Webanwendung starten ####

Um eine Webanwendung zu starten, geben Sie die Gruppe und den Ressourcenschlüssel Web App.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Webpublishing App Profile verwalten ###

Jede WebApp Profil veröffentlichen, mit der Ihre apps veröffentlichen, hat mehrere Operationen auf Profile veröffentlichen ausgeführt werden können.

#### <a name="get-publishing-profile"></a>Die Möglichkeit, Profile ####

Zum Abrufen der Präsentationsprofil für Web app verwenden:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Dieser Befehl gibt Präsentationsprofil an die Befehlszeile als auch Ausgabe Präsentationsprofil in eine Textdatei.

#### <a name="reset-publishing-profile"></a>Profil veröffentlichen zurücksetzen ####

Um beide zurücksetzen publishing Kennwort für FTP und Web bereitstellen für Web app verwenden:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Web App Zertifikate verwalten ###

Weitere Informationen zu Web app Zertifikate finden Sie unter [Zertifikate Binden mit PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Nächste Schritte ###
- Zur Unterstützung von Azure Ressourcenmanager PowerShell finden Sie unter [mithilfe von Azure PowerShell Azure Ressourcenmanager.](../powershell-azure-resource-manager.md)
- Über App Service finden Sie unter [Einführung in App Service-Umgebung.](app-service-app-service-environment-intro.md)
- Informationen zum Verwalten von App Service-Zertifikate mit PowerShell finden Sie unter [Zertifikate Bindung mit PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Zur vollständigen Liste der Azure-Ressourcen-Manager-basierten PowerShell-Cmdlets für Azure Web Apps finden Sie unter [Azure Cmdlet Verweis von Web Apps Azure Ressourcenmanager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Informationen zum Verwalten von App-Service über die Befehlszeilenschnittstelle finden Sie unter [Using Azure Resource Manager-Based Befehlsschnittstelleninstanz CLI für Azure Web App](app-service-web-app-azure-resource-manager-xplat-cli.md)
