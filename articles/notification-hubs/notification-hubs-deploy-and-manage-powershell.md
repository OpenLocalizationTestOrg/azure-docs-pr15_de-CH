<properties 
    pageTitle="Bereitstellen und Verwalten von Notification Hubs mit PowerShell" 
    description="Zum Erstellen und Verwalten von Notification Hubs mit PowerShell für die Automatisierung" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Bereitstellen und Verwalten von Notification Hubs mit PowerShell

##<a name="overview"></a>Übersicht

Dieser Artikel veranschaulicht das Erstellen und Verwalten von Azure Notification Hubs mit PowerShell verwenden. In diesem Thema werden folgende allgemeine Automatisierungsaufgaben angezeigt.

+ Erstellen eines Benachrichtigung Hubs
+ Legen Sie die Anmeldeinformationen

Benötigen Sie auch einen neuen Service Bus-Namespace für die benachrichtigungshubs erstellen, finden Sie unter [Verwalten von Service Bus mit PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Verwalten von Benachrichtigungen Hubs nicht direkt von Azure PowerShell enthaltenen Cmdlets werden. Am besten von PowerShell ist die Microsoft.Azure.NotificationHubs.dll-Assembly verweisen. Die Assembly wird mit [Microsoft Azure Notification Hubs NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)verteilt.


## <a name="prerequisites"></a>Erforderliche Komponenten

Vor diesem Artikel benötigen Sie Folgendes:

- Ein Azure-Abonnement. Azure ist ein Abonnement-basierten Plattform. Weitere Informationen über den Erwerb eines Abonnements finden Sie unter [Optionen] [Bietet Member]oder [Testversion].

- Ein Computer mit Azure PowerShell. Eine Anleitung finden Sie [Installieren und Konfigurieren von Azure PowerShell].

- Allgemeine Kenntnisse von PowerShell-Skripts, NuGet-Pakete und.NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Ein Verweis auf den Service Bus

Verwalten von Azure Notification Hubs noch nicht in die PowerShell-Cmdlets in Azure PowerShell enthalten. Benachrichtigungshubs bereitstellen, können Sie den .NET Client [Microsoft Azure Notification Hubs NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Vergewissern Sie sich, dass Ihr Skript die Assembly **Microsoft.Azure.NotificationHubs.dll** finden die als NuGet-Paket in einem Visual Studio-Projekt installiert. Um flexibel zu sein, führt das Skript folgende Schritte aus:

1. Bestimmt den Pfad, an dem er aufgerufen wurde.
2. Durchläuft den Pfad bis zu einen Ordner mit dem Namen `packages`. Dieser Ordner wird beim Installieren von NuGet-Pakete für Visual Studio-Projekte erstellt.
3. Rekursiv sucht die `packages` Ordner für eine Assembly mit dem Namen **Microsoft.Azure.NotificationHubs.dll**.
4. Auf die Assembly verweist, sind die Typen zur späteren Verwendung.

Sieht aus wie in einem Powershellskript folgendermaßen implementiert werden:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>NamespaceManager-Klasse erstellen

Benachrichtigungshubs bereitstellen, erstellen Sie eine Instanz der Klasse [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) aus dem SDK. 

Lieferumfang von Azure PowerShell [Get-AzureSBAuthorizationRule] -Cmdlet können Sie eine Autorisierungsregel zu einer Verbindungszeichenfolge abzurufen. Wir speichern einen Verweis auf die `NamespaceManager` -Instanz in der `$NamespaceManager` Variable. Wir verwenden `$NamespaceManager` benachrichtigungshub bereitstellen.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Bereitstellung einer neuen Benachrichtigungshub 

Bereitstellen einen neuen benachrichtigungshub verwenden Sie die [.NET API für Notification Hubs].

In diesem Teil des Skripts werden vier lokale Variablen eingerichtet. 

1. `$Namespace`: Legen Sie diese auf den Namen des Namespaces soll einen benachrichtigungshub erstellen.
2. `$Path`: Dieser Pfad auf den Namen des neuen Benachrichtigung Hubs festgelegt.  Beispielsweise "MyHub".    
3. `$WnsPackageSid`: Legen Sie den Paket-SID für Sie Windows App im [Windows-Entwicklungscenter](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Legen Sie den geheimen Schlüssel zur Windows-App im [Windows-Entwicklungscenter](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Diese Variablen werden verwendet, um den Namespace und eine neue Benachrichtigungshub Benachrichtigungen WNS Anmeldeinformationen für eine Windows-Anwendung (Windows Notification Services, WNS) konfiguriert. Informationen zum Abrufen des Pakets SID und geheimen Schlüssels finden Sie unter [Erste Schritte mit Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Tutorial. 

+ Der Codeausschnitt verwendet die `NamespaceManager` zu überprüfen, ob der Benachrichtigungshub identifizierte Objekt `$Path` vorhanden ist.

+ Wenn es nicht vorhanden ist, erstellt das Skript eine `NotificationHubDescription` WNS Anmeldeinformationen und übergeben, um die `NamespaceManager` Klasse `CreateNotificationHub` Methode.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Servicebus mit PowerShell verwalten](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Service Bus-Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skripts erstellen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Erstellen einer Service Bus-Namespace und ein Ereignis Hub mithilfe eines PowerShell-Skripts](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Einige vordefinierte Skripts sind auch zum Download zur Verfügung:
- [Service Bus PowerShell-Skripts](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Kaufoptionen]: http://azure.microsoft.com/pricing/purchase-options/
[Angebote für Mitglieder]: http://azure.microsoft.com/pricing/member-offers/
[Kostenlose Testversion]: http://azure.microsoft.com/pricing/free-trial/
[Installieren und Konfigurieren von Azure PowerShell]: ../powershell-install-configure.md
[.NET API für Notification Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[AzureSBAuthorizationRule abrufen]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
