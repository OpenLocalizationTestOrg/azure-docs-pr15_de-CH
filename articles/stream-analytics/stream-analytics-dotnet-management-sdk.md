<properties
    pageTitle=".NET SDK-Management für Stream Analytics | Microsoft Azure"
    description="Erste Schritte mit Stream Analytics Management .NET SDK. Informationen zum Einrichten und Ausführen von Analytics Projekte: Erstellen eines Projekts, Eingaben, Ausgaben und Transformationen."
    keywords=".NET SDK Analytics API"
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


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Verwaltung von .NET SDK: Einrichten und Ausführen von Analytics Aufträge mithilfe von Azure Stream Analytics-API für .NET

Einrichtung einer zur Analyse mit der Stream Analytics-API für .NET mit .NET SDK Management erfahren. Einrichten eines Projekts, Eingabe- und Quellen, Transformationen und Start und Stopp Aufträge. Für Ihre Aufträge Analytics können Sie Daten von BLOB-Speicher oder von einem Ereignis-Hub streamen.

Finden Sie die [Management-Referenzdokumentation für die Stream Analytics-API für .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics ist ein vollständig verwalteter Service mit niedriger Latenz hochverfügbaren, skalierbaren, komplexe Verarbeitung über Daten in der Cloud. Stream Analytics ermöglicht Kunden einzurichten Aufträge Datenströmen zu analysieren und sie neben Echtzeitanalysen fahren.  


## <a name="prerequisites"></a>Erforderliche Komponenten
Vor diesem Artikel benötigen Sie Folgendes:

- Installieren Sie Visual Studio 2012 oder 2013.
- Downloaden und Installieren von [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Erstellen Sie eine Ressourcengruppe Azure in Ihrem Abonnement. Das folgende ist Skript ein Beispielskript Azure PowerShell. Azure PowerShell-Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Eingabequelle als Ausgabeziel Verwendung eingerichtet. Informationen finden Sie unter Weitere [Eingaben hinzufügen](stream-analytics-add-inputs.md) eine Beispieleingabe einrichten und [Ausgaben hinzufügen](stream-analytics-add-outputs.md) eine Beispielausgabe einrichten.


## <a name="set-up-a-project"></a>Einrichten eines Projekts

Zum Erstellen ein Auftrags Analytics verwenden Stream Analytics-API für .NET zuerst das Projekt festgelegt.

1. Erstellen Sie eine Visual Studio C#.
2. In der Paket-Manager-Konsole, führen Sie die folgenden Befehle die NuGet-Pakete installiert. Die erste Azure Stream Analytics Management .NET SDK ist. Der zweite ist der Azure Active Directory-Client, der für die Authentifizierung verwendet wird.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Fügen Sie der Datei App.config **AppSettings** -Abschnitt:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Ersetzen Sie Werte für **SubscriptionId** und **ActiveDirectoryTenantId** mit Ihrem Azure-Abonnement und Mieter IDs. Das folgende Azure PowerShell-Cmdlet ausführen, können Sie diese Werte abrufen:

        Get-AzureAccount

5. Die Quelldatei (Program.cs) im Projekt **verwenden** Folgendes hinzufügen:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Fügen Sie eine Hilfsmethode Authentifizierung:

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


## <a name="create-a-stream-analytics-management-client"></a>Erstellen Sie ein Stream Analytics Management client

Ein **StreamAnalyticsManagementClient** -Objekt können Sie den Auftrag und Projekt-Komponenten, wie Eingabe-, Ausgabe- und Transformation verwalten.

Fügen Sie den folgenden Code am Anfang der **Main** -Methode:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

**ResourceGroupName** der Wert der Variablen sollte identisch mit den Namen der Ressourcengruppe erstellt oder in den erforderlichen Schritten entnommen.

Zum Automatisieren des Anmeldeinformationen Präsentation Aspekts der Schaffung von Arbeitsplätzen finden Sie unter [Authentifizierung Dienstprinzipalnamen mit Azure-Ressourcen-Manager](../resource-group-authenticate-service-principal.md).

Die verbleibenden Abschnitten dieses Artikels wird davon ausgegangen, dass dieser Code am Anfang der **Main** -Methode.

## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics-Auftrag

Der folgende Code erstellt einen Stream Analytics-Auftrag unter der Ressourcengruppe, die Sie definiert haben. Sie können ein Eingabe-, Ausgabe- und Transformation der Auftrag später hinzufügen.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Erstellen Sie eine Stream Analytics-Eingangsquelle

Der folgende Code erstellt eine Eingabequelle Stream Analytics mit BLOB-Eingangsquelle Typ CSV-Serialisierung. Erstellen Sie eine Ereignis Hub Eingabequelle **BlobStreamInputDataSource**anstelle **EventHubStreamInputDataSource** . Ebenso können Sie die Serialisierung der Eingabequelle anpassen.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Eingabequellen sind von BLOB-Speicher oder einem Ereignis-Hub auf ein bestimmtes Projekt gebunden. Um dieselbe Eingabequelle für andere Projekte verwenden, müssen Sie die Methode erneut aufrufen und geben Sie einen anderen Namen.


## <a name="test-a-stream-analytics-input-source"></a>Ein Stream Analytics Datenquelle testen

**TestConnection** -Methode überprüft, ob Stream Analytics-Auftrags die Eingabequelle sowie andere Aspekte, die auf die Datenquelle herstellen kann. Beispielsweise in die Eingabequelle Blob in einem früheren Schritt erstellt, prüft die Methode, Storage Kontoname und Schlüsselpaar verwendet werden kann, um das Speicherkonto verbinden sowie der angegebene Container vorhanden ist.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Erstellen Sie ein Stream Analytics Ausgabeziel

Erstellen ein Ausgabeziel ähnelt eine Eingabequelle Stream Analytics erstellen. Wie Eingabequellen sind Ziele an ein bestimmtes Projekt gebunden. Um dieselbe Ausgabeziel für andere Projekte verwenden, müssen die Methode erneut aufrufen und einen anderen Namen angeben.

Der folgende Code erstellt ein Ausgabeziel (Azure SQL-Datenbank). Sie können das Ausgabeziel Datentyp bzw. Serialisierungstyp anpassen.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Ein Stream Analytics Ausgabeziel testen

Ein Stream Analytics Ausgabeziel hat auch die **TestConnection** -Methode zum Testen der Verbindung.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Erstellen Sie eine Transformation Stream Analytics

Der folgende Code erstellt eine Stream Analytics Transformation mit der Abfrage "Wählen Sie * aus der Eingabe" und gibt an, dass eine Streaming-Einheit für den Stream Analytics-Auftrag reservieren. Weitere Informationen zum Anpassen von Streaming-Einheiten finden Sie unter [Skalierung Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Eingabe und Ausgabe einer Transformationen ist auch gebunden zu deaktivierende Stream Analytics, unter der Sie erstellt wurde.

## <a name="start-a-stream-analytics-job"></a>Stream Analytics-Auftrag starten
Nachdem ein Stream Analytics und seine Eingaben, Ausgaben und Transformation erstellen, können Sie den Auftrag durch Aufrufen der **Methode** starten.

Im folgenden Beispiel Code startet ein Stream Analytics-Auftrag mit einer benutzerdefinierten Ausgabe Startzeit, 12. Dezember 2012 12:12:12 festgelegt UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Stream Analytics Auftrag beenden
Die **Stop** -Methode können Sie einen ausgeführten Auftrag für Stream Analytics stoppen.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Löschen eines Auftrags Stream Analytics
Die **Delete** -Methode löscht den Auftrag sowie die zugrunde liegenden untergeordneten Ressourcen einschließlich Eingaben, Ausgaben und Umwandlung des Auftrags.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Support erhalten
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung.


## <a name="next-steps"></a>Nächste Schritte

Sie haben lernen die Grundlagen der Verwendung von .NET SDK zum Erstellen und Ausführen von Analytics Aufträge. Weitere Informationen finden Sie hier:

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
