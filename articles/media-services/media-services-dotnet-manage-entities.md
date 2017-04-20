
<properties 
    pageTitle="Verwalten von Ressourcen und verknüpften Entitäten mit Media Services .NET SDK" 
    description="Informationen Sie zum Verwalten von Ressourcen und verknüpften Entitäten mit Media Services SDK für .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Verwalten von Ressourcen und verknüpften Entitäten mit Media Services .NET SDK


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [REST](media-services-rest-manage-entities.md)


Dieses Thema zeigt, wie die folgenden Media Services durchführen:

- Eine Anlage Verweis 
- Bringen Sie einen Auftrag 
- Liste aller Anlagen 
- Liste Projekte und Ressourcen 
- Listen Sie alle Richtlinien 
- Liste aller Locators
- Auflisten von großen Sammlungen von Entitäten
- Löschen von Anlagen 
- Löschen eines Auftrags 
- Löschen einer Richtlinie 

##<a name="prerequisites"></a>Erforderliche Komponenten 

Einrichten [der Umgebung](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Eine Anlage Verweis

Häufig wird ein Verweis auf eine vorhandene Ressource Media Services zu. Im folgenden Codebeispiel wird veranschaulicht, wie Sie eine Anlage Verweis aus der Auflistung Elemente auf dem Server Kontextobjekt basierend auf einer Anlage ID abrufen
Im folgenden Codebeispiel wird eine Linq-Abfrage einen Verweis auf ein vorhandenes Objekt IAsset verwendet.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Bringen Sie einen Auftrag

Beim Arbeiten mit Aufgaben in Media Services Code müssen Sie oft einen Verweis auf einen vorhandenen Auftrag auf der Grundlage einer Id. Im folgenden Codebeispiel wird veranschaulicht, wie ein Verweis auf ein Objekt IJob aus der Jobs-Auflistung.
Codierungsauftrags möglicherweise eine Auftrag beim Starten eines Einzelvorgangs langer codieren und müssen den Status für einen Thread. In solchen Fällen gibt die Methode von einem Thread müssen Sie aktualisiert auf einen Auftrag abrufen.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Liste aller Anlagen

Mit steigender Anzahl der Elemente im Speicher ist es hilfreich, Ihre Liste. Im folgenden Codebeispiel wird das Durchlaufen der Auflistung Elemente des Kontextobjekts Server veranschaulicht. Jede Anlage im Codebeispiel auch einige Eigenschaftswerte in die Konsole geschrieben. Jede Anlage kann z. B. viele Dateien enthalten. Im Codebeispiel schreibt alle Dateien für jede Anlage.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Liste Projekte und Ressourcen

Wichtige gilt verwandte auf Liste mit ihrer zugeordneten Arbeit in Media Services. Im folgenden Codebeispiel wird veranschaulicht, wie jedes Objekt IJob für jeden Einzelvorgang Eigenschaften zum Einzelvorgang angezeigt, alle Aufgaben und alle Vermögenswerte und Gesamtvermögens Ausgabe eingeben. Der Code in diesem Beispiel kann für viele andere Aufgaben hilfreich sein. Beispielsweise möchten Sie Ausgabe Anlagen aus mindestens ein Kodierung Aufträge auflisten, die Sie zuvor ausgeführt haben, veranschaulicht diesen Code Ausgabe Ressourcen zugreifen. Wenn Sie einen Verweis auf eine Ausgabe haben, können Sie herunterladen oder und URLs dann Inhalt für andere Benutzer oder die Anwendung bereitstellen. 

Weitere Informationen zu Optionen für die Bereitstellung von Ressourcen finden Sie unter [Ressourcen mit Media Services SDK für .NET liefern](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Listen Sie alle Richtlinien

Media Services definieren Sie eine Zugriffsrichtlinie Vermögenswert oder Dateien. Eine Zugriffsrichtlinie definiert die Berechtigungen für eine Datei oder eine Anlage (welche Art von Zugriff und die Dauer). In Ihrem Code Media Services definieren Sie normalerweise eine Zugriffsrichtlinie ein IAccessPolicy-Objekt erstellen und es mit einer vorhandenen Anlage zuordnen. Erstellen Sie ein ILocator-Objekt, das Sie direkten Zugriff auf Ressourcen in Media Services kann. Visual Studio-Projekt, das vorliegenden Dokumentation begleitet enthält mehrere Code-Beispiele, die zeigen, wie erstellen und Zuweisen von Richtlinien und Locators Anlagen.

Im folgenden Codebeispiel wird veranschaulicht, wie alle Richtlinien auf dem Server und zeigt den jeweils zugeordneten Berechtigungen. Nützliche können anzeigen auf alle ILocator Objekte auf dem Server ist und Sie Sie für jede Locator die zugeordnete Richtlinie, über die AccessPolicy-Eigenschaft auflisten.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Liste aller Locators

Ein Locator ist ein URL, einen direkten Weg Vermögenswert mit Berechtigungen für die Ressource Zugriff auf gemäß der Locator zugeordnete Richtlinie enthält. Jede Anlage kann eine Auflistung von ILocator-Objekten auf seine Locators-Eigenschaft zugeordnet haben. Der Serverkontext hat auch eine Locator-Auflistung, die alle Locators enthält.

Das folgende Codebeispiel listet alle Locators auf dem Server. Für jede Locator wird die Id für die zugehörigen Anlagen und Zugriff. Welche Berechtigungen, das Ablaufdatum und den vollständigen Pfad zeigt auch der Anlage an.

Beachten Sie, dass ein Locator-Pfad zu einer Anlage nur eine Basis-URL für die Ressource. Um einen direkten Pfad auf einzelne Dateien erstellen, die ein Benutzer oder eine Anwendung zu navigieren kann, muss Code bestimmte Dateipfad Locator Pfad hinzufügen. Weitere Informationen hierzu finden Sie unter [Ressourcen mit Media Services SDK für .NET liefern](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Auflisten von großen Sammlungen von Entitäten

Beim Abfragen von Entitäten ist maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST v2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. Skip und Take verwendet beim großen Sammlungen von Elementen durchlaufen werden müssen. 
    
Die folgende Funktion durchläuft alle Aufträge in der bereitgestellten Media Services-Konto. Media Services gibt 1000 Jobs Jobs-Auflistung zurück. Die Funktion nutzt überspringen und nehmen Sie sicherstellen, dass alle Aufträge aufgelistet (bei mehr als 1000 Projekte in Ihrem Konto).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Löschen von Anlagen

Das folgende Beispiel löscht eine Anlage.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Löschen eines Auftrags

Um einen Auftrag zu löschen, müssen Sie den Status des Auftrags überprüfen die State-Eigenschaft angegeben. Einzelvorgänge, die abgeschlossen oder storniert werden können gelöscht werden, Aufträge in anderen Staaten, wie in der Warteschlange, geplante oder Verarbeitung, müssen zuerst storniert werden und dann sie können gelöscht werden.
Das folgende Codebeispiel zeigt eine Methode zum Löschen eines Auftrags Auftragsstatusangaben und dann löschen, wenn der Status abgeschlossen oder abgebrochen wird. Dieser Code hängt von dem vorherigen Abschnitt in diesem Thema zum Abrufen eines Verweises auf ein Projekt: ein Projekt Verweis.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Löschen einer Richtlinie

Im folgenden Codebeispiel wird veranschaulicht, wie ein Verweis auf eine Zugriffsrichtlinie basierend auf Richtlinien-Id und die Richtlinie löschen.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
