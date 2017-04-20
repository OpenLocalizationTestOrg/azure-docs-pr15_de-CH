<properties
    pageTitle="programmgesteuert auf Stream Analytics überwachen | Microsoft Azure"
    description="Informationen Sie zum Stream Analytics Arbeitsplätze über REST-APIs, Azure SDK oder Powershell programmgesteuert überwachen."
    keywords=".NET Monitor, Projekt überwachen Überwachung app"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Einen Stream Analytics Job Monitor programmgesteuert erstellen
 Dieser Artikel veranschaulicht das Aktivieren der Überwachung für einen Stream Analytics-Auftrag. Stream Analytics Arbeitsplätze über REST-APIs, Azure SDK oder Powershell tun haben nicht Überwachung standardmäßig aktiviert.  Sie manuell dazu im Azure-Portal des Auftrags Monitor Seite navigieren und auf die Schaltfläche aktivieren oder dieser Prozess automatisiert, die Schritte in diesem Artikel. Die Daten zeigen in der Registerkarte "Überwachen" Azure-Portal für den Stream Analytics-Auftrag.

![Job Monitor Registerkarte Aufträge](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Erforderliche Komponenten
Vor diesem Artikel benötigen Sie Folgendes:

- Visual Studio 2012 oder 2013.
- Downloaden und Installieren von [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Einen vorhandenen Stream Analytics-Auftrag, die Überwachung aktiviert.

## <a name="setup-a-project"></a>Einrichten eines Projekts

1.  Erstellen Sie eine Visual Studio C#.
2.  In der Paket-Manager-Konsole, führen Sie die folgenden Befehle die NuGet-Pakete installiert. Die erste Azure Stream Analytics Management .NET SDK ist. Der zweite ist der Azure-Monitor SDK verwendet wird, aktivieren Sie die Überwachung. Die letzte ist Azure Active Directory-Client, der für die Authentifizierung verwendet wird.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  Die Datei App.config AppSettings-Abschnitt hinzugefügt.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Ersetzen Sie Werte für *SubscriptionId* und *ActiveDirectoryTenantId* mit Ihrem Azure-Abonnement und Mieter IDs. Folgende PowerShell-Cmdlet ausgeführt, um diese Werte zu erhalten:

    ```
    Get-AzureAccount
    ```
4.  Fügen Sie die folgenden Aussagen mit der Quelldatei (Program.cs) im Projekt.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Hinzufügen einer Hilfsmethode Authentifizierung.

        public static string GetAuthorizationHeader()
            {
                AuthenticationResult result = null;
                var thread = new Thread(() =>
                {
                    try
                    {
                        var context = new AuthenticationContext(
                            ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                            ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                        result = context.AcquireToken(
                            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                            clientId: ConfigurationManager.AppSettings["AsaClientId"],
                            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                            promptBehavior: PromptBehavior.Always);
                    }
                    catch (Exception threadEx)
                    {
                        Console.WriteLine(threadEx.Message);
                    }
                });

                thread.SetApartmentState(ApartmentState.STA);
                thread.Name = "AcquireTokenThread";
                thread.Start();
                thread.Join();

                if (result != null)
                {
                    return result.AccessToken;
                }

                throw new InvalidOperationException("Failed to acquire token");
        }

## <a name="create-management-clients"></a>Kunden erstellen
Im folgende Code wird die erforderlichen Variablen und Kunden eingerichtet.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Aktivieren der Überwachung für ein Stream Analytics

Der folgende Code wird **für ein Stream Analytics** Überwachung aktivieren. Der erste Teil des Codes führt eine GET-Anforderung für den Stream Analytics-Dienst zum Abrufen von Informationen zu bestimmten Stream Analytics-Auftrag. Verwendet die Eigenschaft "Id" (GET-Anforderung entnommen) als Parameter für die Put-Methode in der zweiten Hälfte der Code sendet eine Insights-Dienst anfordern, zum Aktivieren der Überwachung für den Stream Analytics-Auftrag.

> [AZURE.WARNING]
> Wenn Sie zuvor aktiviert haben für einen anderen Stream Analytics-Auftrag Azure-Portal oder programmgesteuert über Überwachung der folgenden Code **wird empfohlen, dass Sie den gleichen speicherkontonamen bereitstellen, die Sie zuvor die Überwachung aktiviert haben.**
>
> Verknüpft das Speicherkonto Stream Analytics Druckauftrag in erstellt Region, nicht selbst.
>
> Alle Stream Analytics Auftrag (und alle anderen Azure Ressourcen) in der gleichen Region Teilen dieses Speicherkonto zum Speichern von Daten. Wenn Sie einen anderen Speicherkonto bereitstellen, möglicherweise es unerwünschte Nebeneffekte der Stream Analytics-Aufträge oder andere Azure Ressourcen überwachen.
>
> Der Speicher-Kontoname ersetzt ```“<YOUR STORAGE ACCOUNT NAME>”``` unten sollte ein Speicherkonto Stream Analytics Auftrag im selben Abonnement ist für die Überwachung aktivieren.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Support erhalten
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung.


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
