<properties
    pageTitle="Servicebus mit PowerShell verwalten | Microsoft Azure"
    description="Service Bus mit PowerShell-Skripts verwalten"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Servicebus mit PowerShell verwalten

## <a name="overview"></a>Übersicht

Microsoft Azure PowerShell ist eine Skriptingumgebung, mit denen Sie steuern und Automatisieren der Bereitstellung und Verwaltung der Arbeitslasten in Azure. Dieser Artikel beschreibt wie Sie PowerShell zum Bereitstellen und Verwalten von Service Bus Entitäten wie Namespaces, Warteschlangen und Ereignis Hubs mit lokalen Azure PowerShell-Konsole.

## <a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel müssen Sie die folgenden Komponenten:

- Ein Azure-Abonnement. Azure ist ein Abonnement-basierten Plattform. Weitere Informationen über den Erwerb eines Abonnements finden Sie unter [Optionen][] [Bietet Member][]oder [Testversion][].

- Ein Computer mit Azure PowerShell. Eine Anleitung finden Sie [Installieren und Konfigurieren von Azure PowerShell][].

- Allgemeine Kenntnisse von PowerShell-Skripts, NuGet-Pakete und.NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Ein Verweis auf den Service Bus

Gibt eine begrenzte Anzahl von PowerShell-Cmdlets zur Verwaltung von Service Bus. Entitäten bereitstellen, die nicht durch vorhandene Cmdlets verfügbar sind, können Sie .NET Client Service Bus, [Service Bus NuGet-Paket][].

Vergewissern Sie sich, das Skript **Microsoft.ServiceBus.dll** Assembly finden kann mit NuGet-Paket installiert ist. Um flexibel zu sein, führt das Skript folgende Schritte aus:

1. Bestimmt den Pfad, an dem er aufgerufen wurde.
2. Durchläuft den Pfad bis zu einen Ordner mit dem Namen `packages`. Dieser Ordner wird beim Installieren von NuGet-Paketen erstellt.
3. Rekursiv sucht die `packages` Ordner für eine Assembly mit dem Namen **Microsoft.ServiceBus.dll**.
4. Auf die Assembly verweist, sind die Typen zur späteren Verwendung.

Sieht aus wie in einem Powershellskript folgendermaßen implementiert werden:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Bereitstellung von Service Bus-namespace

Zwei PowerShell-Cmdlets unterstützen Service Bus-Namespace. Statt die APIs der .NET SDK können Sie [Get-AzureSBNamespace][] und [Neu AzureSBNamespace][].

In diesem Beispiel werden einige lokale Variablen im Skript erstellt. `$Namespace` and `$Location`.

- `$Namespace`ist der Name des Service Bus-Namespace mit dem wir arbeiten möchten.
- `$Location`Gibt das Skript den Namespace Bestimmungen Rechenzentrum.
- `$CurrentNamespace`Speichert den Verweis-Namespace, den das Skript ruft (oder erstellt).

In einem tatsächlichen Skript `$Namespace` und `$Location` als Parameter übergeben werden kann.

Dieser Teil des Skripts führt folgende Aufgaben aus:

1. Versucht, einen Service Bus-Namespace mit dem angegebenen Namen abzurufen.
2. Wenn der Namespace gefunden wird, meldet es fanden.
3. Wenn der Namespace nicht finden, den Namespace erstellt und ruft dann den neu erstellten Namespace.

    ```
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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Bereitstellen anderer Entitäten Service Bus erstellen Sie eine Instanz der Klasse [NamespaceManager][] aus dem SDK.
Das Cmdlet " [Get-AzureSBAuthorizationRule][] " können Sie eine Autorisierungsregel zu einer Verbindungszeichenfolge abzurufen. Wir speichern einen Verweis auf die `NamespaceManager` -Instanz in der `$NamespaceManager` Variable. Wir verwenden `$NamespaceManager` später im Skript auf anderen Entitäten bereitzustellen.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Bereitstellung von anderen Entitäten Service Bus

Um anderen Personen bereitstellen wie Warteschlangen, Themen und Ereignis Hubs [.NET API für Service Bus][]. Dieser Artikel konzentriert sich nur auf Ereignis Hubs, aber die Schritte für andere Entitäten sind ähnlich. Darüber hinaus werden weitere Beispiele anderer Entitäten am Ende dieses Artikels verwiesen.

Dieser Teil des Skripts erstellt vier weitere lokalen Variablen. Diese Variablen werden zum Instanziieren einer `EventHubDescription` Objekt. Das Skript führt die folgenden Aufgaben:

1. Mit der `NamespaceManager` Objekt, überprüfen Sie, ob Ereignis Hub identifizierte `$Path` vorhanden ist.
2. Wenn nicht vorhanden ist, erstellen Sie ein `EventHubDescription` und, die `NamespaceManager` Klasse `CreateEventHubIfNotExists` Methode.
3. Nachdem festgestellt wurde, dass Ereignis verfügbar ist, Erstellen eines Consumers Gruppe `ConsumerGroupDescription` und `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
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

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migrieren eines Namespaces zum anderen Azure-Abonnement

Die folgende Sequenz von Befehlen verschiebt Namespace aus einem Azure-Abonnement auf einen anderen. Zum Ausführen dieses Vorgangs der Namespace bereits aktiv sein muss muss, und des Benutzers PowerShell Befehle ausführen ein Administrator auf Quell- und Ziel-Abonnements.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel bereitgestellt grundlegende Gliederung für die Bereitstellung von Service Bus Entitäten mit PowerShell. Alles, was Sie .NET Clientbibliotheken verwenden können, auch in einem Powershellskript möglich.

Gibt Ausführlichere Beispiele auf diesen Blog-Einträge:

- [Service Bus-Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skripts erstellen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Erstellen einer Service Bus-Namespace und ein Ereignis Hub mithilfe eines PowerShell-Skripts](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Einige vordefinierte Skripts sind auch zum Download zur Verfügung:
- [Service Bus PowerShell-Skripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Kaufoptionen]: http://azure.microsoft.com/pricing/purchase-options/
[Angebote für Mitglieder]: http://azure.microsoft.com/pricing/member-offers/
[Kostenlose Testversion]: http://azure.microsoft.com/pricing/free-trial/
[Installieren und Konfigurieren von Azure PowerShell]: ../powershell-install-configure.md
[Service Bus NuGet-Paket]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[AzureSBNamespace abrufen]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Neue AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[AzureSBAuthorizationRule abrufen]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API für Servicebus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
