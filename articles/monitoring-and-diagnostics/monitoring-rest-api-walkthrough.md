<properties
    pageTitle="Azure REST API Walkthrough überwachen | Microsoft Azure"
    description="Wie authentifizieren Anfragen und Azure Überwachung REST API."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure REST API Walkthrough überwachen
Dieser Artikel veranschaulicht die Authentifizierung durchführen, damit Code den [REST-API-Referenz zu Microsoft Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx)verwenden kann.         

Der Azure-Monitor-API ermöglicht programmgesteuert verfügbaren metrische Standarddefinitionen (Typ Metrik wie CPU-Zeit, Anfragen usw.), Genauigkeit und Metrikwerte abzurufen. Nachdem abgerufen, können die Daten in einem separaten Datenspeicher wie Azure SQL-Datenbank, DocumentDB oder Azure Data Lake gespeichert. Dort zusätzliche Analyse erfolgt nach Bedarf.

Neben der Arbeit mit verschiedenen Daten, wie dieser Artikel zeigt, ermöglicht die Monitor-API Warnregeln, Aktivitätsprotokollen, und vieles mehr. Eine vollständige Liste der verfügbaren Operationen finden Sie unter [Microsoft Azure Monitor REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Authentifizieren von Azure Monitor Anfragen

Der erste Schritt ist die Anforderung authentifiziert.

Alle Aufgaben in der Azure-Monitor-API ausgeführt verwenden Authentifizierungsmodell Azure-Ressourcen-Manager. Daher müssen alle Anfragen mit Azure Active Directory (Azure AD) authentifiziert werden. Azure AD Dienstprinzipalnamen erstellen und Abrufen Authentifizierungstoken (JWT) ist ein Ansatz für die Clientanwendung zu authentifizieren. Das folgende Beispielskript zeigt Azure AD Service principal über PowerShell anlegen. Eine ausführlichere Anleitung finden Sie in der Dokumentation über [Azure PowerShell Dienstprinzipal Zugriff auf Ressourcen erstellen](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). Es kann auch [ein Service-Prinzip Azure Portal](../resource-group-create-service-principal-portal.md)erstellen.

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Zum Abfragen der Azure-Monitor-API verwenden die Client-Anwendung zuvor erstellten Dienstprinzipalnamen authentifizieren. PowerShell-Skript das folgende Beispiel zeigt einen Ansatz mit [Active Directory-Authentifizierung-Bibliothek](../active-directory/active-directory-authentication-libraries.md) (ADAL) JWT Authentication token helfen. Das JWT-Token wird als Teil einer HTTP-Identifizierungsparameter Anfragen Azure Monitor REST API übergeben.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Nach Abschluss der Installation Authentifizierungsschritt können Abfragen gegen die REST-API von Azure Monitor ausgeführt werden. Es gibt zwei hilfreiche Abfragen:

1. Listendefinitionen Sie metrische für eine Ressource

2. Die metrischen Werte abrufen


## <a name="retrieve-metric-definitions"></a>Metrische Definitionen abrufen
>[AZURE.NOTE] Rufen Sie metrische Definitionen mit Azure Monitor REST API verwenden Sie "2016-03-01" als API-Version.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Einer App Azure Logik würde metrischen Definitionen wie im folgenden Screenshot angezeigt:

![ALT "JSON metrischen Definition Antwort" angezeigt.](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Weitere Informationen finden Sie in der [Liste Metrik Definitionen für eine Ressource in Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/mt743621.aspx) -Dokumentation.

## <a name="retrieve-metric-values"></a>Metrische Werte abrufen
Metrischen verfügbaren Definitionen bekannt, kann dann verknüpfte metrischen Werte abzurufen. Die Metrik namens "Wert" (nicht "LocalizedValue" genannt) für Anfragen Filtern verwenden (z. B. 'CpuTime' und 'Angefordert' Metrik Datenpunkte abrufen). Keine Filter angegeben, wird die Standardmetrik zurückgegeben.

>[AZURE.NOTE] Rufen Sie metrische Werte mit Azure Monitor REST API verwenden Sie "2016-06-01" als API-Version.

**Methode**: Abrufen

**Request-URI**: https://management.azure.com/subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/_{Ressource-Anbieter-Namespace}_/_{Ressourcentyp}_/& API-Version_{Ressourcenname}_/providers/microsoft.insights/metrics?$filter=_{Filter}_=_{Anforderungsparameter}_

Zum Abrufen der RunsSucceeded metrische Datenpunkte für den angegebenen Zeitraum und eine Zeit Körnung von 1 Stunde würde die Anforderung z. B. folgendermaßen aussehen:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Das Ergebnis würde im folgenden Screenshot Beispiel ähneln:

![ALT "Durchschnittliche Antwortzeit metrischen Wert JSON-Antwort"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Rufen Sie mehrere Daten oder Aggregation Punkte fügen Sie metrische Definitionsnamen und Aggregationstypen Filter wie im folgenden Beispiel dargestellt hinzu:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>ARMClient verwenden
Eine Alternative zur Verwendung von PowerShell (siehe oben) ist [ARMClient](https://github.com/projectkudu/ARMClient) auf Ihrem Windows-Computer verwenden. ARMClient verarbeitet automatisch Azure AD-Authentifizierung (und der resultierende JWT-Token). Die folgenden Schritte beschreiben die Verwendung von ARMClient zum Abrufen von Daten:

1. Installieren Sie [Schokoladig](https://chocolatey.org/) und [ARMClient](https://github.com/projectkudu/ARMClient).

2. Geben Sie in einem Terminalfenster _armclient.exe Anmeldung_. So fordert Sie Azure anmelden.

3. Typ _Armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Typ _Armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Mit ARMClient Azure Überwachung REST-API arbeiten"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Die Ressourcen-ID abrufen
Die REST-API kann wirklich Hilfe verfügbaren metrische Definitionen, Granularität und zugehörige Werte zu verstehen. Diese Informationen ist hilfreich, wenn die [Azure Management-Bibliothek](https://msdn.microsoft.com/library/azure/mt417623.aspx)verwendet.

Für das vorherige Codebeispiel ist die Ressourcen-ID mithilfe der vollständige Pfad zu der gewünschten Azure Ressource. Z. B. zum Abfragen einer Azure Web App wäre die Ressourcennummer:

*/Subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/Providers/Microsoft.Web/Sites/{Site-Name}/*

Die folgende Liste enthält einige Beispiele für Resource ID-Formate für verschiedene Azure-Ressourcen:

* **IoT Hub** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.Devices/IotHubs/_{Iot Hub Name}_

* **Elastische SQL-Pool** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.Sql/servers/_{Pool-Db}_/elasticpools/_{Sql-Pool-Name}_

* **SQL-Datenbank (v12)** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.Sql/servers/_{Servername}_/databases/_{Eigenschaftsname}_

* **Service Bus** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.ServiceBus/_{Namespace}_/_{Servicebus-Name}_

* **VM Maßstab legt** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{Vm-Name}_

* **VMs** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.Compute/virtualMachines/_{Vm-Name}_

* **Event Hubs** - /subscriptions/_{Abonnement-Id}_/resourceGroups/_{Ressource-Gruppenname}_/providers/Microsoft.EventHub/namespaces/_{Eventhub-Namespace}_


Alternative Ansätze zum Abrufen von Ressourcen-ID, einschließlich der Verwendung von Azure Ressourcen-Explorer die gewünschte Ressource im Azure-Portal und über PowerShell oder CLI Azure anzeigen gibt.

### <a name="azure-resource-explorer"></a>Azure-Ressourcen-Explorer
Die Ressourcen-ID für eine gewünschte Ressource zu finden ist hilfreich [Azure-Ressourcen-Explorer](https://resources.azure.com) -Tool verwenden. Navigieren Sie zu der gewünschten Ressource und sehen Sie sich die ID wie im folgenden Screenshot gezeigt:

![ALT "Azure Resource Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure-portal
Die Ressourcen-ID kann auch von Azure-Portal abgerufen werden. Hierzu navigieren Sie zu der gewünschten Ressource, und wählen Sie dann Eigenschaften. Die Ressourcen-ID ist Blatt Eigenschaften angezeigt, wie im folgenden Screenshot gezeigt:

![ALT "Ressourcen-ID angezeigt, in der Eigenschaften-Blade in Azure-Portal"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Die Ressourcen-ID kann mit Azure PowerShell-Cmdlets und abgerufen werden. Beispielsweise erhalten die Ressourcen-ID für eine Azure Web App führen Sie das Cmdlet Get-AzureRmWebApp wie in der folgenden Abbildung:

![ALT "Ressourcen-ID über PowerShell erhalten"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Um die Ressourcen-ID mithilfe der Azure-CLI abzurufen, führen Sie den Anzeigebefehl "Azure Webapp angeben die"--Json "-Option wie im folgenden Screenshot gezeigt:

![ALT "Ressourcen-ID über PowerShell erhalten"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Aktivität protokollieren Daten abrufen
Neben metrischen Definitionen und zugehörigen Werte kann auch weitere interessante Einblicke Zusammenhang mit Azure Ressourcen abrufen. Beispielsweise kann, [Aktivitätsprotokoll](https://msdn.microsoft.com/library/azure/dn931934.aspx) Daten. Das folgende Beispiel veranschaulicht die Verwendung von Azure Monitor REST API zur Datenabfrage Aktivität Protokoll innerhalb eines bestimmten Datumsbereichs für Azure-Abonnement:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Überwachung](monitoring-overview.md).
* [Unterstützte Metriken mit Azure](monitoring-supported-metrics.md)anzeigen
* Überprüfen Sie die [Microsoft Azure Monitor REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Überprüfen Sie die [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).
