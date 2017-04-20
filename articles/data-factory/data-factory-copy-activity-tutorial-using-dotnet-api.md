<properties 
    pageTitle="Lernprogramm: Erstellen eine Pipeline mit .NET API mit Kopieren | Microsoft Azure" 
    description="In diesem Lernprogramm erstellen Sie mit einer Kopie eine Azure Data Factory-Pipeline mit .NET API." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Lernprogramm: Erstellen einer Pipeline mit .NET API mit Kopieren
> [AZURE.SELECTOR]
- [Übersicht und Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure-Ressourcen-Manager-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm wird das Erstellen und Überwachen einer Azure Data Factory mithilfe der .NET API veranschaulicht. Pipeline in der Data Factory verwendet eine Aktivität kopieren Daten von Azure BLOB-Speicher in Azure SQL-Datenbank kopiert.

Der Kopie Aktivität Datenübertragungen in Azure Data Factory. Die Aktivität wird durch global verfügbaren Service betrieben, die Daten zwischen verschiedenen Datenspeichern eine sichere, zuverlässige und skalierbare Weise kopieren können. Siehe [Datenaktivitäten](data-factory-data-movement-activities.md) Weitere Details zur Aktivität kopieren.   

> [AZURE.NOTE] 
> Dieser Artikel behandelt nicht alle die Factory .NET-API. Informationen zum Data Factory .NET SDK finden Sie in der [Data Factory.NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) . 

## <a name="prerequisites"></a>Erforderliche Komponenten
- [Lernprogramm und erforderlichen Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) eine Übersicht des Lernprogramms und **erforderliche** Schritte durchlaufen. 
- Visual Studio 2012 oder 2013 oder 2015
- [Azure.NET SDK](http://azure.microsoft.com/downloads/) herunterladen und installieren
- Azure PowerShell. Anleitung [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Artikel Azure PowerShell auf Ihrem Computer installieren. Azure PowerShell verwenden zum Erstellen einer Anwendung Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Erstellen einer Anwendung in Azure Active Directory
Erstellen einer Anwendung Azure Active Directory Dienstprinzipalnamen für die Anwendung erstellen und **Daten Factory** Teilnehmerrolle zuweisen.  

1. Starten Sie **PowerShell**. 
1. Führen Sie den folgenden Befehl, und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei Azure-Portal.
    
        Login-AzureRmAccount   
2. Führen Sie den folgenden Befehl an alle Abonnements für dieses Konto.

        Get-AzureRmSubscription 
3. Führen Sie den folgenden Befehl Abonnements wählen Sie arbeiten. Ersetzen Sie ** &lt;NameOfAzureSubscription** &gt; mit dem Namen der Azure-Abonnement. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Beachten Sie die Ausgabe dieses Befehls **SubscriptionId** und **TenantId** . 
4. Eine Azure-Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** durch Ausführen des folgenden Befehls in der PowerShell zu erstellen.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Wenn bereits die Ressourcengruppe angeben, ob aktualisieren (Y) oder als (N). 

    Verwenden eine anderen Ressourcengruppe müssen Sie den Namen der Ressourcengruppe anstelle ADFTutorialResourceGroup in diesem Lernprogramm verwenden.
5. Erstellen einer Active Directory Azure-Anwendung. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Wenn Sie die Fehlermeldung erhalten, geben Sie einen anderen URL und führen Sie den Befehl erneut aus. 

        Another object with the same value for property identifierUris already exists.

6. Erstellen Sie die Anzeige Dienstprinzipalnamen. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. **Data Factory** Teilnehmerrolle Dienstprinzipalnamen hinzufügen. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Die Anwendung ID anfordern

        $azureAdApplication

    Notieren Sie die ID (**ApplicationID** der Ausgabe).

Sie haben folgende vier Werte folgendermaßen aus: 

- Mandanten-ID
- Abonnement-ID
- ID der Anwendung 
- Kennwort (in den ersten Befehl)   

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Mit Visual Studio 2012/2013/2015, erstellen Sie eine C#.
    1. Starten Sie **Visual Studio** 2012/2013/2015.
    2. Klicken Sie auf **Datei**, zeigen Sie auf **neu**, und klicken Sie auf **Projekt**.
    3. **Vorlagen Sie**, und wählen Sie **Visual C#**. In dieser exemplarischen Vorgehensweise verwenden C#, aber jeder verwenden.
    4. Die Liste auf der rechten Seite der **Konsolenanwendung** auswählen.
    5. Geben Sie **DataFactoryAPITestApp** ein.
    6. Wählen Sie **C:\ADFGetStarted** für die Position.
    7. Klicken Sie auf **OK** , um das Projekt zu erstellen.
2. Klicken Sie auf **Extras**, zeigen Sie **Nuget Paket-Manager**und auf **Paket-Manager-Konsole**.
3.  **Paket-Manager Konsole**führen Sie die folgenden Schritte aus: 
    1.  Führen Sie den folgenden Befehl Data Factory-Paket zu installieren:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Führen Sie den folgenden Befehl Azure Active Directory-Paket zu installieren (Sie verwenden Active Directory-API im Code):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Fügen Sie den folgenden **AppSetttings** Abschnitt der Datei **App.config** . Dazu dienen die Hilfsmethode: **GetAuthorizationHeader**. 

    Ersetzen von Werten für ** &lt;ID der Anwendung&gt;**, ** &lt;Kennwort&gt;**, ** &lt;Abonnement-ID&gt;**, und ** &lt;Mieter ID&gt; ** mit Ihren eigenen Werten. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Die folgende Anweisung **verwenden** an (Program.cs) im Projekt hinzufügen.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Fügen Sie den folgenden Code, der eine Instanz der **DataPipelineManagementClient** -Klasse an die **Main** -Methode erstellt. Sie verwenden dieses Objekt Daten Factory, verknüpfte Dienst Eingabe- und Datasets und einer Pipeline erstellen. Sie verwenden dieses Objekt auch Segmente eines Datasets zur Laufzeit zu überwachen.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Ersetzen Sie den Wert des **ResourceGroupName** mit dem Namen der Azure-Ressourcengruppe. 
    > 
    > Aktualisieren Sie Namen Data Factory (**DataFactoryName**) eindeutig sein. Name der Data Factory muss eindeutig sein. Benennungskonventionen für Artefakte Data Factory finden Sie [Data Factory - Namenskonventionen](data-factory-naming-rules.md) . 

7. Fügen Sie den folgenden Code, der die **Main** -Methode einer **Data Factory** erstellt.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Fügen Sie den folgenden Code, der eine **Azure Storage Service verknüpft** die **Main** -Methode erstellt. 

    > [AZURE.IMPORTANT] Ersetzen Sie **Storageaccountname** und **Accountkey** mit Namen und Schlüssel Ihres Kontos Azure-Speicher. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Fügen Sie den folgenden Code, der eine **SQL Azure Service verknüpft** die **Main** -Methode erstellt.
 
    > [AZURE.IMPORTANT] Ersetzen Sie **Servername**, **Datenbankname**, **Benutzername**und **Kennwort** mit Namen der SQL Azure-Server, Datenbank, Benutzername und Kennwort.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Fügen Sie den folgenden Code, **Eingabe- und Datasets** in der **Main** -Methode erstellt. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Fügen Sie folgenden Code, **erstellt und aktiviert eine Pipeline** für die **Main** -Methode. Diese Pipeline hat eine **CopyActivity** , die **BlobSource** als Quelle verwendet und **BlobSink** als eine Senke.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Fügen Sie folgenden Code der **Main** -Methode den Status ein Datenslice das ausgabedataset. Es gibt nur Slice in diesem Beispiel erwartet.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Fügen Sie den folgenden Code, um Details für einen Datenslice der **Main** -Methode ausgeführt werden.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Fügen Sie die folgende Hilfsmethode, mit der **Main** -Methode der Klasse **Anwendung** .  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. Erweitern Sie das Projekt (**DataFactoryAPITestApp**) im Projektmappen-Explorer Maustaste **Verweise**, und klicken Sie auf **Verweis hinzufügen**. Das Kontrollkästchen "**System.Configuration**" Assembly, und klicken Sie auf **OK**. 
16. Erstellen Sie das Konsolenanwendungsprojekt. Klicken Sie im Menü **Erstellen** , und klicken Sie auf **Projektmappe erstellen**. 
16. Bestätigen Sie, dass mindestens eine Datei im Container **Adftutorial** im Azure BLOB-Speicher. Wenn nicht, **Emp.txt** -Datei in Editor mit folgendem Inhalt erstellen und Adftutorial Container hochladen.

        John, Doe
        Jane, Doe
     
17. Führen Sie das Beispiel, indem Sie auf **Debuggen** -> Sie im Menü**Debuggen starten** . Wenn **Details ein Datenslice führen immer**angezeigt wird, warten Sie einige Minuten und drücken Sie die **EINGABETASTE**. 
18. Verwenden des Azure-Portals, Data Factory **APITutorialFactory** mit folgenden Elemente erstellt: 
    - Service verknüpft: **LinkedService_AzureStorage** 
    - DataSet: **DatasetBlobSource** und **DatasetBlobDestination**.
    - Pipeline: **PipelineBlobSample** 
18. Stellen Sie sicher, dass zwei Mitarbeiterdatensätze in der Tabelle "**emp**" angegebene SQL Azure-Datenbank erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

- Bietet detaillierte Informationen zum Projektvorgang kopieren im Lernprogramm verwendeten [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel lesen.
- Informationen zum Data Factory .NET SDK finden Sie in der [Data Factory.NET API Reference](https://msdn.microsoft.com/library/mt415893.aspx) . Dieser Artikel behandelt nicht alle die Factory .NET-API. 

 
