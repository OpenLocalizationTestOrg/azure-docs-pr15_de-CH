<properties
    pageTitle="Mithilfe von PowerShell Service Bus und Ereignis Hubs Ressourcen | Microsoft Azure"
    description="Mithilfe von PowerShell erstellen und Service Bus und Ereignis Hubs Ressourcen"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Mithilfe von PowerShell Service Bus und Ereignis Hubs Ressourcen

Microsoft Azure PowerShell ist eine Skriptingumgebung, mit denen Sie steuern und Automatisieren der Bereitstellung und Verwaltung von Azure Services. Dieser Artikel beschreibt wie Sie PowerShell zum Bereitstellen und Verwalten von Service Bus Entitäten wie Namespaces, Warteschlangen und Ereignis Hubs mit lokalen Azure PowerShell-Konsole.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Azure ist ein Abonnement-basierten Plattform. Weitere Informationen über den Erwerb eines Abonnements finden Sie unter [Kaufoptionen][] [Member bietet][]oder [kostenloses Konto][].

- Ein Computer mit Azure PowerShell. Eine Anleitung finden Sie [Installieren und Konfigurieren von Azure PowerShell][].

- Allgemeine Kenntnisse von PowerShell-Skripts, NuGet-Pakete und.NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Ein Verweis auf den Service Bus

Gibt eine begrenzte Anzahl von PowerShell-Cmdlets zur Verwaltung von Service Bus. Entitäten bereitstellen, die nicht durch vorhandene Cmdlets verfügbar sind, können Sie .NET Client [Service Bus NuGet-Paket]verweisen für Service Bus in PowerShell verwenden.

Vergewissern Sie sich, das Skript **Microsoft.ServiceBus.dll** Assembly finden kann mit NuGet-Paket installiert ist. Um flexibel zu sein, führt das Skript folgende Schritte aus:

1. Bestimmt den Pfad, aus dem sie aufgerufen wurde.
2. Durchläuft den Pfad bis zu einen Ordner mit dem Namen `packages`. Dieser Ordner wird beim Installieren von NuGet-Paketen erstellt.
3. Rekursiv sucht die `packages` Ordner für eine Assembly mit dem Namen **Microsoft.ServiceBus.dll**.
4. Auf die Assembly verweist, sind die Typen zur späteren Verwendung.

Sieht aus wie in einem Powershellskript folgendermaßen implementiert werden:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Bereitstellung von Service Bus-namespace

Beim Arbeiten mit Service Bus Namespaces sind zwei Cmdlets .NET SDK anstelle: [Get-AzureSBNamespace][] und [Neu AzureSBNamespace][].

In diesem Beispiel werden einige lokale Variablen im Skript erstellt. `$Namespace` and `$Location`.

- `$Namespace`Der Name der des Service Bus-Namespace mit dem wir arbeiten möchten.
- `$Location`identifiziert das Rechenzentrum in das werden wir den Namespace bereitstellen.
- `$CurrentNamespace`Speichert den Verweis-Namespace, den wir abrufen (oder erstellen).

In einem tatsächlichen Skript `$Namespace` und `$Location` als Parameter übergeben werden kann.

Dieser Teil des Skripts führt Folgendes aus:

1. Versucht, einen Service Bus-Namespace mit dem angegebenen Namen abzurufen.
2. Wenn der Namespace gefunden wird, meldet es fanden.
3. Wenn der Namespace nicht finden, den Namespace erstellt und ruft dann den neu erstellten Namespace.

    ``` powershell

    $Namespace = "MyServiceBusNS"
    $Location = "West US"

    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Andere Entitäten Service Bus bereitstellen, erstellen Sie eine Instanz der `NamespaceManager` -Objekt aus dem SDK. Das Cmdlet " [Get-AzureSBAuthorizationRule][] " können Sie eine Autorisierungsregel zu einer Verbindungszeichenfolge abzurufen. In diesem Beispiel speichert einen Verweis auf die `NamespaceManager` -Instanz in der `$NamespaceManager` Variable. Das Skript hat `$NamespaceManager` anderen Personen bereitstellen.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Bereitstellung von anderen Entitäten Service Bus

Andere Entitäten wie Warteschlangen, Themen und Ereignis Hubs bereitstellen können Sie [.NET API für Service Bus][]. Detailliertere Beispiele anderer Entitäten werden am Ende dieses Artikels verwiesen.

### <a name="create-an-event-hub"></a>Erstellen Sie einen Ereignis-Hub

Dieser Teil des Skripts erstellt vier weitere lokalen Variablen. Diese Variablen werden zum Instanziieren einer `EventHubDescription` Objekt. Das Skript führt Folgendes aus:

1. Mit der `NamespaceManager` Objekt, überprüfen Sie, ob Ereignis Hub identifizierte `$Path` vorhanden ist.
2. Wenn nicht vorhanden ist, erstellen Sie ein `EventHubDescription` und, die `NamespaceManager` Klasse `CreateEventHubIfNotExists` Methode.
3. Nachdem festgestellt wurde, dass Ereignis verfügbar ist, Erstellen eines Consumers Gruppe `ConsumerGroupDescription` und `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }

    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

### <a name="create-a-queue"></a>Erstellen einer Warteschlange

Erstellen eine Warteschlange oder ein Thema Überprüfen Sie Namespace wie im vorherigen Abschnitt.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Erstellen eines Themas

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel bereitgestellte ein grundlegender Workflow für die Bereitstellung von Service Bus Entitäten PowerShell. Gibt eine begrenzte Anzahl von PowerShell-Cmdlets zur Verwaltung von Service Bus führen messaging Entitäten verweisen auf die Assembly Microsoft.ServiceBus.dll, nahezu jede Operation Sie mithilfe von .NET Clientbibliotheken können, die Sie auch in einem Powershellskript ausführen.

Gibt Ausführlichere Beispiele in dieser Blogbeiträgen:

- [Service Bus-Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skripts erstellen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Erstellen einer Service Bus-Namespace und ein Ereignis Hub mithilfe eines PowerShell-Skripts](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Einige vordefinierte Skripts sind auch zum Download zur Verfügung:

- [Service Bus PowerShell-Skripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[Kaufoptionen]: http://azure.microsoft.com/pricing/purchase-options/
[Angebote für Mitglieder]: http://azure.microsoft.com/pricing/member-offers/
[kostenloses Konto]: http://azure.microsoft.com/pricing/free-trial/
[Service Bus NuGet-Paket]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[AzureSBNamespace abrufen]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Neue AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[AzureSBAuthorizationRule abrufen]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API für Servicebus]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Installieren und Konfigurieren von Azure PowerShell]: ../powershell-install-configure.md
