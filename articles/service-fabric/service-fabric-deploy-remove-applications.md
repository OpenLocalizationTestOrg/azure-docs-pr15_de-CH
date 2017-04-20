<properties
   pageTitle="Bereitstellung von Service Fabric-Anwendung | Microsoft Azure"
   description="Bereitstellen und Entfernen von Clientanwendungen in Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Bereitstellung und Anwendung mit PowerShell entfernen

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Sobald eine [Anwendung verpackt][10], in Azure Service Fabric-Cluster einsatzbereit ist. Die Bereitstellung umfasst die folgenden drei Schritte:

1. Das Anwendungspaket hochladen
2. Registrieren Sie den Anwendungstyp
3. Die Anwendungsinstanz erstellen

>[AZURE.NOTE] Verwenden Sie Visual Studio zum Bereitstellen und Debuggen in einem Cluster lokale Entwicklung, werden die folgenden Schritte automatisch ein PowerShell-Skript im Ordner "Scripts" Anwendungsprojekt behandelt. Dieser Artikel erklärt die Skripts tun, damit dieselben Operationen außerhalb von Visual Studio durchführen können.

## <a name="upload-the-application-package"></a>Das Anwendungspaket hochladen

Hochladen Anwendungspaket wird an, die von internen Service Fabric-Komponenten zugegriffen. PowerShell können Uploads durchführen. Vor dem Ausführen von PowerShell-Befehlen in diesem Artikel beginnen Sie immer mit [ServiceFabricCluster verbinden](https://msdn.microsoft.com/library/mt125938.aspx) Fabric-Dienst herstellen.

Angenommen, Sie haben einen Ordner namens *MyApplicationType* , die der erforderlichen Anwendungsmanifest Service Manifeste und Code/Konfigurationsdaten Pakete enthält. Befehl [Kopieren ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) lädt das Paket Cluster Abbildspeicher. Ist Teil des Moduls Service Fabric SDK PowerShell Cmdlet **Get-ImageStoreConnectionStringFromClusterManifest** zum Abrufen der Verbindungszeichenfolge Bild laden.  Um das SDK-Modul importieren, führen Sie Folgendes aus:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Sie können ein Anwendungspaket aus *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* , *c:\temp\MyApplicationType* (Umbenennen das Verzeichnis "Debug", "MyApplicationType"). Das folgende Beispiel lädt das Paket:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Das Anwendungspaket registrieren

Registriert das Anwendungspaket stellt die Anwendung und Version zur Verwendung im Anwendungsmanifest verfügbar deklariert. Liest das System das Paket hochgeladen im vorherigen Schritt (entspricht dem lokal ausgeführte [Test ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) ) Paket prüfen, verarbeitet den Paketinhalt und verarbeitete Paket an einen internen Speicherort kopieren.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

[Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) Befehl gibt erst das System das Anwendungspaket kopiert wurde. Wie lange dauert, hängt vom Inhalt des Anwendungspakets ab. Gegebenenfalls kann der **TimeoutSec -** Parameter angeben ein längeren Timeouts verwendet. (Das Standardtimeout ist 60 Sekunden).

Der [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) Befehl listet alle erfolgreich registrierte Anwendung geben.

## <a name="create-the-application"></a>Erstellen der Anwendung

Instanziieren Sie eine Anwendung mithilfe von die Version jeder Anwendung, die über den Befehl [Neu ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) erfolgreich registriert wurde. Jede Anwendung muss beginnen mit den *Stoff:* System und für jede Anwendungsinstanz eindeutig sein. Alle Standarddienste im Anwendungsmanifest des Zieltyps Anwendung definiert werden zu diesem Zeitpunkt erstellt.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

Der Befehl [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) Listet alle Instanzen, die erfolgreich, sowie deren Gesamtstatus erstellt wurden.

Der Befehl [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) listet Dienstinstanzen, die innerhalb einer Anwendung erstellt wurden. Standarddienste (falls vorhanden) sind hier aufgelistet.

Für jede Version von registrierten Anwendungstyp können mehrere Instanzen erstellt werden. Jede Anwendungsinstanz isoliert mit Arbeitsverzeichnis und Prozess ausgeführt wird.

## <a name="remove-an-application"></a>Entfernen einer Anwendung

Wenn eine Instanz der Anwendung nicht mehr benötigt wird, können Sie sie endgültig [Entfernen ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) Befehl entfernen. Dieser Befehl entfernt automatisch alle Dienste, die die Anwendung gehören alle Dienststatus dauerhaft entfernt. Dieser Vorgang kann nicht rückgängig gemacht und Anwendungsstatus nicht wiederhergestellt werden.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Wenn eine bestimmte Version eines Anwendungstyps nicht mehr benötigt wird, sollten Sie die Registrierung aufheben, Befehl [Unregister-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) . Aufheben der Registrierung von nicht verwendeten Typen frei Speicherplatz von Paketinhalt Anwendung dieser Art auf der Abbildspeicher. Ein Anwendungstyp kann aufgehoben werden, dagegen keine Anwendung instanziiert werden und keine ausstehenden Anwendung Upgrades sind darauf verweisen.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Problembehandlung

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopie ServiceFabricApplicationPackage fordert eine ImageStoreConnectionString

Die Service Fabric-SDK-Umgebung müssen bereits die richtigen Vorgaben. Aber gegebenenfalls ImageStoreConnectionString für alle Befehle sollte den Wert, der mit der Service Fabric-Cluster. Sie finden diese im clustermanifest mit dem Befehl [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) abgerufen:

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Nächste Schritte

[Service Fabric-Anwendung aktualisieren](service-fabric-application-upgrade.md)

[Fabric Service Health Einführung](service-fabric-health-introduction.md)

[Diagnose und Problembehandlung von Service Fabric service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Modellieren einer Anwendung in Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
