<properties
    pageTitle="Azure Resource Manager-basierte plattformübergreifende Befehlszeilentools für Azure Web App | Microsoft Azure"
    description="Die neue Azure-Ressourcen-Manager-basierte plattformübergreifende Befehlszeilentools Azure Web Apps zu erfahren."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Verwenden von Azure Resource Manager-basierte Befehlsschnittstelleninstanz CLI für Azure WebApp#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Mit der neuen Version Microsoft Azure plattformübergreifende Befehlszeilentools 0.10.5 wurden neue Befehle hinzugefügt. Diese Befehle ermöglichen dem Benutzer Azure-Ressourcen-Manager-basierten PowerShell-Befehlen verwenden Web Apps verwalten.

Informationen zum Verwalten von Ressourcen finden Sie unter [Azure-CLI zur Verwaltung Azure Ressourcen und Ressourcengruppen verwenden](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Verwalten von App Service-Pläne ##

### <a name="create-an-app-service-plan"></a>Erstellen eines App Service-Plans ###
Erstellen Sie ein app-Serviceplan Befehl **Azure Appserviceplan erstellen** .

Folgenden werden die verschiedenen Parameter:

-   **-Ressourcengruppen**: Ressourcengruppe, die neu erstellte app Service-Plan enthält.
-   **-Name**: Name der app Service-Plan.
-   **-Speicherort**: app Plan Speicherort.
-   **-Ebene**: die gewünschten Preise Sku (Optionen: F1 (kostenlos). D1 (freigegeben). B1 (grundlegende klein) B2 (grundlegende Mittel) und B3 (Basic groß). S1 (Standard klein) S2 (Standard Mittel) und S3 (Standard groß). (Premium klein) P1, P2 (Premium Mittel) und P3 (große Premium).)
-   **-Instanzen**: die Anzahl der Arbeitskräfte in der app service-Plan (Standardwert ist 1).

Dieses Cmdlet verwendet wird:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Liste vorhandenen App Service-Pläne ###

Liste der vorhandenen Anwendung Servicepläne Befehl **Azure Appserviceplan Liste** .

Zum Auflisten aller app Servicepläne unter einer bestimmten Ressourcengruppe zu verwenden:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Zu einer bestimmten Anwendung Serviceplan Befehl **Azure Appserviceplan anzeigen** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurieren einer vorhandenen App Service-Plan ###

Ändern Sie die Einstellung für einen vorhandenen app Service-Plan verwenden Sie den Befehl **Azure Appserviceplan Config** . Sie können die Sku und die Anzahl der Arbeitskräfte 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Skalieren eines App Service-Plans ####

Um einen vorhandenen App Service-Plan zu skalieren, verwenden:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Ändern der SKUs App Service-Plan ####

Ändern Sie eine vorhandene Anwendung planen Service Sku verwenden:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Löschen einer vorhandenen App Service-Plan ###

Zum Löschen eines vorhandenen app Service-Plans müssen alle zugeordneten webapps verschoben oder zuerst gelöscht werden. Dann Befehl **Azure Webapp löschen** löschen app Service-Plan.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Verwalten von App Service webapps ##

### <a name="create-a-web-app"></a>Erstellen Sie eine Webanwendung ###

Erstellen Sie eine Webanwendung den Befehl **Azure Webapp erstellen** .

Folgenden werden die verschiedenen Parameter:

- **-Name**: für das Web app.
- **-Plan**: für Serviceplan Web app gehostet.
- **-Ressourcengruppen**: Ressourcengruppe, die App Service-Plan hostet.
- **-Speicherort**: app-Website.

Dieses Cmdlet verwendet wird:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Löschen Sie ein vorhandenes Web App ###

So löschen Sie eine vorhandenes Web app Befehl **Azure Webapp löschen** können, müssen Sie Web app und die Ressourcengruppennamen angeben.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Liste vorhandene Web Apps ###

Liste der vorhandenen Web apps verwenden Sie den Befehl **Azure Webapp Liste** .

Zum Auflisten aller Web-apps unter einer bestimmten Ressourcengruppe zu verwenden:

    azure webapp list --resource-group ContosoAzureResourceGroup

Zu einem bestimmten Web app Befehl **Azure Webapp anzeigen** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Konfigurieren Sie ein vorhandenes Web App ###

Um Einstellungen und Konfigurationen für eine vorhandene Web app zu ändern, verwenden Sie den Befehl **set Config Azure Webapp** .

Beispiel (1): Ändern der Php Version Web app 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

(2) Beispiel: Fügen Sie hinzu oder ändern Sie der app

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Wissen, was andere Konfiguration kann geändert, verwenden Sie den Befehl **set Config Azure Webapp -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Ändern Sie den Zustand einer vorhandenen Web App ###

#### <a name="restart-a-web-app"></a>Eine Webanwendung starten ####

Neustart eine Webanwendung müssen Sie die Gruppe und den Ressourcenschlüssel Web App angeben.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Eine Webanwendung beenden ####

Um eine Webanwendung zu beenden, müssen Sie die Gruppe und den Ressourcenschlüssel Web App angeben.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Eine Webanwendung starten ####

Um eine Webanwendung zu starten, geben Sie die Gruppe und den Ressourcenschlüssel Web App.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Webpublishing App Profile verwalten ###

Jede WebApp hat ein Veröffentlichungsprofil, mit der Ihre apps veröffentlichen.

#### <a name="get-publishing-profile"></a>Die Möglichkeit, Profile ####

Zum Abrufen der Präsentationsprofil für Web app verwenden:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Dieser Befehl gibt publishing Profil Benutzername und Kennwort in der Befehlszeile.

### <a name="manage-web-app-hostnames"></a>Verwalten von Hostnamen Web App ###

Hostname Bindings für Ihr Web app verwalten, verwenden Sie den Befehl **Azure Webapp Config Hostnamen**  

#### <a name="list-hostname-bindings"></a>Liste Hostname Bindungen ####

Verwenden Sie zum Abrufen der aktuellen Hostname Bindings für eine Webanwendung

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hostname Bindungen hinzufügen ####

Zum Hinzufügen von Hostname Bindings Web App verwenden:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname Bindings löschen ####

Zum Löschen von Hostname Bindungen verwenden:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Nächste Schritte ###
- Über Azure Resource Manager CLI-Unterstützung finden Sie unter [mit dem Azure-Ressourcen und Ressourcengruppen Verwalten der Azure-CLI.](../xplat-cli-azure-resource-manager.md)
- Informationen zum Verwalten von App Service mit PowerShell finden Sie unter [Using Azure Resource Manager-Based PowerShell zum Verwalten von Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
