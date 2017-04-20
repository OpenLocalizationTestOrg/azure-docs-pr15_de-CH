<properties
   pageTitle="Mithilfe von Windows PowerShell Skripts Entwicklung und Testumgebung veröffentlichen | Microsoft Azure"
   description="Erfahren Sie, wie mit Windows PowerShell-Skripts in Visual Studio zur Entwicklung veröffentlichen und Umgebung testen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Mithilfe von Windows PowerShell Skripts Dev veröffentlichen und Testen der Umgebung

Beim Erstellen einer Web-Anwendung in Visual Studio können Sie ein Windows PowerShell-Skript generieren, die Sie später verwenden können, die Veröffentlichung Ihrer Website in Azure Web App in Azure App Service oder einen virtuellen Computer zu automatisieren. Sie können bearbeiten und Windows PowerShell-Skript in der Visual Studio-Editor Ihren Bedürfnissen entsprechend erweitern oder vorhandenen Build testen und Veröffentlichen von Skripts Skript integriert.

Diese Skripts können Sie angepasste Versionen (auch bekannt als Entwicklungs- und Umgebung) der Site für die temporäre Verwendung bereitstellen. Beispielsweise könnten Sie eine bestimmte Version Ihrer Website auf einem virtuellen Computer Azure oder stagingslot auf einer Website eine Testsammlung auszuführen, einen Fehler reproduzieren, Fehlerkorrektur Testversion eine vorgeschlagene Änderung testen oder Einrichten einer benutzerdefinierten Umgebung für eine Demo oder Präsentation einrichten. Nachdem Sie ein Skript, das das Projekt veröffentlicht erstellt haben, können identische Umgebungen erstellen, indem Sie das Skript nach Bedarf erneut ausgeführt oder Skript mit eigenen Web-Anwendung eine benutzerdefinierte Umgebung zum Testen zu erstellen.

## <a name="what-you-need"></a>Was Sie benötigen

- Azure SDK 2.3 oder höher. Weitere Informationen finden Sie in der [Visual Studio-Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) .

Sie brauchen keine Azure SDK Skripts für Webprojekte generiert. Dieses Feature ist für Webprojekte nicht Webrollen Clouddienste.

- Azure PowerShell 0.7.4 oder höher. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md) für Weitere Informationen.

- [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) oder höher.

## <a name="additional-tools"></a>Weitere tools

Zusätzliche Tools und Ressourcen für die Arbeit mit PowerShell in Visual Studio für Azure-Entwicklung sind verfügbar. Sehen Sie [PowerShell-Tools für Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Generieren von Skripts veröffentlichen

Sie können Skripts veröffentlichen für eine virtuelle Maschine, die Ihre Website, beim Erstellen eines neuen Projekts folgenden [lernen hostet](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md). Sie können auch [generieren veröffentlichen Skripts für Web-apps in Azure App Service](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Visual Studio generiert Skripts

Visual Studio generiert auf Ordner **PublishScripts** , die zwei Windows PowerShell-Dateien, für die virtuellen Computer oder die Website ein Skript veröffentlichen enthält und ein Modul mit Funktionen, die Sie in Skripts verwenden können. Visual Studio generiert eine Datei auch im JSON-Format, das die Details des Projekts angibt, die Sie bereitstellen.

### <a name="windows-powershell-publish-script"></a>Windows PowerShell-veröffentlichen Skript

Das Veröffentlichen Skript enthält bestimmte veröffentlichen Schritte zum Bereitstellen einer Website oder virtuellen Computer. Visual Studio bietet Syntaxfarbe für die Entwicklung von Windows PowerShell. Hilfe für die Funktionen steht zur Verfügung und Sie die Funktionen im Skript ändern beliebig frei können.

### <a name="windows-powershell-module"></a>Windows PowerShell-Modul

Das Windows PowerShell-Modul, das Visual Studio generiert enthält Funktionen, die das Skript Veröffentlichen verwendet. Diese Azure PowerShell-Funktionen sind und nicht geändert werden soll. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md) für Weitere Informationen.

### <a name="json-configuration-file"></a>JSON-Konfigurationsdatei

Die JSON-Datei im Ordner **Konfigurationen** erstellt und enthält Konfigurationsdaten, die genau die Ressourcen in Azure bereitstellen. Der Name der Datei, die Visual Studio generiert ist Projekt-Namen-WAWS-dev.json Wenn Sie eine Website oder ein Projekt namens-VM-dev.json erstellt, wenn ein virtueller Computer erstellt. Hier ist ein Beispiel einer JSON-Konfigurationsdatei, die beim Erstellen einer Website generiert. Die meisten Werte sind selbsterklärend. Der Name der Website erzeugt Azure, damit er möglicherweise nicht den Projektnamen übereinstimmt.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Wenn Sie einen virtuellen Computer erstellen, ähnelt die JSON-Konfigurationsdatei Folgendes. Beachten Sie, dass ein Clouddienst als Container für den virtuellen Computer erstellt wird. Virtuelle Computer enthält die üblichen Endpunkte für den Webzugriff über HTTP und HTTPS als auch Endpunkte für Web bereitstellen, können Sie von Ihrem lokalen Remotedesktop und Windows PowerShell auf der Website veröffentlicht.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Sie können die JSON-Konfiguration zum Ändern der Aktionen beim Veröffentlichen Skripte bearbeiten. Die `cloudService` und `virtualMachine` sind erforderlich, aber Sie können die `databases` Abschnitt, wenn Sie sie benötigen. In der Standardkonfigurationsdatei enthält, die Visual Studio generiert Eigenschaften sind optional. die Werte in der Standardkonfigurationsdatei sind erforderlich.

Haben Sie eine Website mit mehreren bereitstellungsumgebungen (Steckplätze genannt) statt einzelner Produktionsstandort in Azure können Steckplatzname Namen der Website in der JSON-Konfigurationsdatei aufnehmen. Beispielsweise haben Sie eine Website, die benannte **meinesite** und ein Steckplatz für namens **test** dann der URI ist meinesite test.cloudapp.net, aber der richtige Name in der Konfigurationsdatei ist mysite(test). Sie können nur diesen Website und Steckplätze bereits in Ihrem Abonnement. Falls nicht vorhanden, erstellen Sie die Website durch Ausführen des Skripts ohne den Steckplatz dann erstellen Sie Steckplatz im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)und danach führen Sie das Skript mit dem Websitenamen geänderte. Weitere Informationen zu Bereitstellung Steckplätze für Web-apps finden Sie unter [Stagingumgebungen für Web-apps in Azure App Service einrichten](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Veröffentlichen Skripts ausführen

Wenn Sie ein Windows PowerShell-Skript nie ausgeführt haben, müssen Sie die Ausführungsrichtlinie Skripts aktivieren festlegen. Dies ist eine Sicherheitsfunktion, damit Benutzer Windows PowerShell-Skripts ausführen, wenn sie sind anfällig für Malware oder Viren, die Skripts ausgeführt werden.

### <a name="run-the-script"></a>Führen Sie das Skript

1. Web Deploy-Paket für das Projekt zu erstellen. Ein Web Deploy-Paket ist ein komprimiertes Archiv (ZIP-Datei) mit Dateien, die Sie mit Ihrer Website oder virtuellen Computer kopieren möchten. Sie können Web Deploy Pakete für jede Web-Anwendung in Visual Studio erstellen.

![Erstellen Sie Paket bereitstellen](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Weitere Informationen finden Sie unter [wie: Erstellen eines Webpakets Bereitstellung in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Wie im Abschnitt **Anpassen und Erweitern von Skripts veröffentlichen** weiter unten in diesem Thema beschrieben, können Sie auch die Erstellung Ihres Pakets Web Deploy automatisieren.

1. Im **Projektmappen-Explorer**das Kontextmenü für das Skript, und wählen Sie **Öffnen mit PowerShell ISE**.

1. Ist dies das erste Mal Windows PowerShell-Skripts auf diesem Computer ausgeführt haben, öffnen Sie ein Eingabeaufforderungsfenster mit Administratorrechten, und geben Sie folgenden Befehl:

`Set-ExecutionPolicy RemoteSigned`

1. Melden Sie sich bei Azure mithilfe des folgenden Befehls.

`Add-AzureAccount`

Bei Aufforderung geben Sie Ihren Benutzernamen und Ihr Kennwort.

Beachten Sie, dass beim Automatisieren des Skripts diese Methode Azure Anmeldeinformationen funktionieren. Die Datei "publishsettings" verwenden Sie stattdessen Anmeldeinformationen. Einmal, Sie Befehl **Get-AzurePublishSettingsFile** Datei von Azure und danach mit der **Import-AzurePublishSettingsFile** die Datei importieren. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).

1. (Optional) Azure Ressourcen wie virtuelle Maschine, Datenbank und Website ohne Veröffentlichung Ihrer Web-Anwendung erstellen, verwenden Sie den Befehl **Veröffentlichen WebApplication.ps1** mit der **-Konfiguration** Argument auf JSON-Konfigurationsdatei festgelegt. Diese Befehlszeile verwendet JSON-Konfigurationsdatei um zu feststellbar, welche Ressourcen erstellen. Es verwendet die Standardeinstellungen für andere Befehlszeilenargumente Ressourcen erstellt, aber nicht die Webanwendung veröffentlichen. Verbose-Option bietet Ihnen weitere Informationen geschieht.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Befehl **Veröffentlichen WebApplication.ps1** wie im folgenden Beispiel gezeigt das Skript und veröffentlichen Sie die Webanwendung. Möchten Sie die Standardeinstellungen für alle anderen Argumente, wie den Namen überschreiben, Paketname, virtuellen Anmeldeinformationen oder Serveranmeldeinformationen veröffentlichen, können Sie diese Parameter angeben. Verwenden der **-Verbose** können Informationen über den Fortschritt der Veröffentlichungsvorgang.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Wenn Sie einen virtuellen Computer erstellen, sieht der Befehl wie folgt. Außerdem wird veranschaulicht, wie die Anmeldeinformationen für die Datenbanken angeben. Für diese Skripts erstellen virtuellen Computer ist das SSL-Zertifikat nicht von einer vertrauenswürdigen Stammzertifizierungsstelle. Daher müssen Sie die **AllowUntrusted-** Option.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Das Skript kann Datenbanken erstellen, aber Datenbankserver erstellt. Wenn Sie einen Datenbankserver erstellen möchten, können Sie **Neue AzureSqlDatabaseServer** -Funktion in Azure-Modul.

## <a name="customizing-and-extending-the-publish-scripts"></a>Anpassen und Erweitern von Skripts veröffentlichen

Sie können das Skript veröffentlichen und die JSON-Konfigurationsdatei anpassen. Funktionen in Windows PowerShell-Modul **AzureWebAppPublishModule.psm1** sollen nicht geändert werden. Wenn Sie gerade eine andere Datenbank angeben oder Ändern der Eigenschaften des virtuellen Computers möchten bearbeiten Sie JSON-Konfigurationsdatei Ggf. zum Erweitern der Funktionalität des Skripts zur Automatisierung erstellen und Testen des Projekts können Sie Funktion Stubs in **Veröffentlichen WebApplication.ps1**implementieren.

Fügen Sie zum Erstellen des Projekts automatisieren MSBuild aufrufende Code `New-WebDeployPackage` wie in diesem Beispiel gezeigt. Der Pfad des MSBuild-Befehls ist abhängig von der Version von Visual Studio installiert haben. Zu den richtigen Pfad die Funktion **Get-MSBuildCmd**können Sie wie im folgenden Beispiel gezeigt.

### <a name="to-automate-building-your-project"></a>Erstellen eines Projekts automatisieren

1. Hinzufügen der `$ProjectFile` Parameter im Abschnitt globaler Parameter.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Kopieren Sie die Funktion `Get-MSBuildCmd` in der Skriptdatei.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Ersetzen Sie `New-WebDeployPackage` mit die folgenden code, und Ersetzen Sie die Platzhalter in der Zeile erstellen `$msbuildCmd`. Dieser Code ist für Visual Studio 2015. Wenn Visual Studio 2013 verwenden, ändern Sie die **VisualStudioVersion** -Eigenschaft unter `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Rufen Sie die `New-WebDeployPackage` Funktion vor dieser Zeile: `$Config = Read-ConfigFile $Configuration` für Web-apps oder `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` für virtuelle Computer.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Rufen Sie das benutzerdefinierte Skript in Befehlszeile übergeben der `$Project` Argument, wie in der folgenden Beispiel-Befehlszeile.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Fügen Sie Code zum Automatisieren der Anwendung testen, `Test-WebApplication`. Achten Sie darauf, dass Zeilen **Veröffentlichen WebApplication.ps1** kommentieren, in denen diese Funktionen aufgerufen werden. Wenn Sie eine Implementierung angeben, können Sie manuell erstellen Sie das Projekt mit Visual Studio und Skript dann Azure veröffentlichen veröffentlichen.

## <a name="publishing-function-summary"></a>Zusammenfassung veröffentlichen Funktion

Erhalten von Hilfe zu Funktionen der Windows PowerShell-Befehlszeile mit dem Befehl `Get-Help function-name`. Die Hilfe enthält Parameterhilfe und Beispiele. Dieselbe Hilfetext ist auch die Quelle Skriptdateien, **AzureWebAppPublishModule.psm1** und **WebApplication.ps1 veröffentlichen**. Das Skript und die Hilfe sind in der Visual Studio-Sprache lokalisiert.

**AzureWebAppPublishModule**

|Funktionsname|Beschreibung|
|---|---|
|Hinzufügen AzureSQLDatabase|Erstellt eine neue Azure SQL-Datenbank.|
|Hinzufügen AzureSQLDatabases|SQL Azure-Datenbanken erstellt aus den Werten in der JSON Konfiguration, Visual Studio generiert.|
|Hinzufügen AzureVM|Azure Virtual Machine erstellt und gibt die URL der bereitgestellten virtuellen Computer. Die Funktion stellt die erforderlichen Komponenten und ruft dann die Funktion **Neu AzureVM** (Azure Modul), um einen neuen virtuellen Computer erstellen.|
|Hinzufügen AzureVMEndpoints|Fügt neue Eingabe Endpunkte einer virtuellen Maschine und gibt den virtuellen Computer mit den neuen Endpunkt.|
|Hinzufügen AzureVMStorage|Erstellt ein neues Azure-Speicher-Konto in das aktuelle Abonnement. Der Name des Kontos beginnt mit "Devtest" eine eindeutige alphanumerische Zeichenfolge gefolgt. Die Funktion gibt den Namen des neuen Speicher. Sie müssen einen Standort oder eine Gruppe für das neue Speicherkonto angeben.|
|Hinzufügen AzureWebsite|Erstellt eine Website mit dem angegebenen Namen und Speicherort. Diese Funktion ruft die Funktion **Neu AzureWebsite** in Azure-Modul. Wenn das Abonnement bereits eine Website mit dem angegebenen Namen enthält, wird diese Funktion erstellt die Website und eine Website zurück. Andernfalls wird `$null`.|
|Backup-Abonnement|Speichert das aktuelle Azure-Abonnement in der `$Script:originalSubscription` Variable im Skriptbereich. Diese Funktion speichert das Azure-Abonnement (wie durch `Get-AzureSubscription -Current`) und dessen Speicherkonto und Abonnements, die von diesem Skript geändert wird (in der Variablen gespeichert `$UserSpecifiedSubscription`) und die Speicherung, im Skriptbereich. Speichern Sie die Werte, können Sie eine Funktion wie `Restore-Subscription`, aktuellen Abonnement und Speicher Ausgangskonto Stand wiederherstellen, wenn der aktuelle Status geändert hat.|
|Suchen AzureVM|Ruft den angegebenen Azure virtuellen Computer ab.|
|Format DevTestMessageWithTime|Datum und Uhrzeit in einer Nachricht voran. Diese Funktion dient in Streams Fehler und Verbose geschriebenen Nachrichten.|
|AzureSQLDatabaseConnectionString abrufen|Baut eine Verbindungszeichenfolge für die Verbindung mit einer Azure SQL-Datenbank.|
|AzureVMStorage abrufen|Gibt den Namen des ersten Storage-Konto mit dem Namen "Devtest*" (Groß-/Kleinschreibung beachten) im angegebenen Speicherort oder Gruppe. Wenn der "Devtest*" Konto stimmt nicht überein, den Standort oder die Gruppe, die Funktion ignoriert. Sie müssen einen Standort oder eine Gruppe angeben.|
|MSDeployCmd abrufen|Gibt einen Befehl zum Ausführen des Tools MsDeploy.exe.|
|Neue AzureVMEnvironment|Sucht oder erstellt einen virtuellen Computer in der Subskription, die die Werte in der Konfigurationsdatei JSON entspricht.|
|WebPackage veröffentlichen|Verwendet MsDeploy.exe und ein Web veröffentlichen Paket. ZIP-Datei auf einer Website Ressourcen bereitstellen. Diese Funktion generiert keine Ausgabe. Schlägt der Aufruf von MSDeploy.exe löst die Funktion eine Ausnahme. Um eine detailliertere Ausgabe zu erhalten, verwenden Sie die **-ausführliche** Option.|
|Veröffentlichen WebPackageToVM|Überprüft die Parameterwerte und ruft dann die Funktion **WebPackage veröffentlichen** .|
|ConfigFile lesen|Überprüft die JSON-Konfigurationsdatei und gibt eine Hash-Tabelle der ausgewählten Werte.|
|Wiederherstellen-Abonnement|Setzt das aktuelle Abonnement auf das ursprüngliche Abonnement.|
|Test-AzureModule|Gibt `$true` Wenn die installierte Azure-Modulversion 0.7.4 oder höher. Gibt `$false` Wenn das Modul nicht installiert oder eine frühere Version. Diese Funktion hat keine Parameter.|
|Test-AzureModuleVersion|Gibt `$true` ist die Version des Moduls Azure 0.7.4 oder höher. Gibt `$false` Wenn das Modul nicht installiert oder eine frühere Version. Diese Funktion hat keine Parameter.|
|Test-HttpsUrl|Die eingegebene URL konvertiert System.Uri-Objekt. Gibt `$True` wenn URL absolut und Schema Https ist. Gibt `$false` Wenn die URL relativ ist, Schema nicht HTTPS oder die Eingabezeichenfolge nicht in einen URL umgewandelt werden.|
|Test-Member|Gibt `$true` Wenn eine Eigenschaft oder Methode ein Member des Objekts ist. Sonst `$false`.|
|Schreiben ErrorWithTime|Schreibt eine Fehlermeldung mit der aktuellen Zeit vorangestellt. Diese Funktion ruft die Funktion **Format DevTestMessageWithTime** vor dem Schreiben in den Stream Fehler vorangestellt.|
|Schreiben HostWithTime|Schreibt eine Meldung in das Hostprogramm (**Write-Host**) vorangestellt, die aktuelle Uhrzeit an. Die Auswirkung des an das Schreiben variiert. Die meisten Programme schreiben, Host Windows PowerShell diese Nachrichten an die Standardausgabe.|
|Schreiben VerboseWithTime|Schreibt eine ausführliche Meldung derzeit vorangestellt. Da sie **Write-Verbose**aufruft, wird die Meldung nur beim Ausführen des Skripts mit der **Verbose** -Parameter oder **für**die **VerbosePreference** eingestellt ist.|

**Veröffentlichen WebApplication**

|Funktionsname|Beschreibung|
|---|---|
|Neue AzureWebApplicationEnvironment|Azure Ressourcen wie eine Website oder virtuellen Computer erstellt.|
|Neue WebDeployPackage|Diese Funktion ist nicht implementiert. Sie können Befehle in dieser Funktion zu Ihrem Projekt hinzufügen.|
|Veröffentlichen AzureWebApplication|Eine Web-Anwendung in Azure veröffentlicht.|
|Veröffentlichen WebApplication|Erstellt und setzt Web Apps, virtuelle Computer, SQL-Datenbanken und Speicherkonten für ein Visual Studio-Webprojekt.|
|Test-WebApplication|Diese Funktion ist nicht implementiert. Sie können Befehle in dieser Funktion der Anwendung hinzufügen.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu PowerShell Skripts [mit Windows PowerShell-Skripts](https://technet.microsoft.com/library/bb978526.aspx) lesen und andere Azure PowerShell-Skripts im [Script Center](https://azure.microsoft.com/documentation/scripts/)finden Sie unter.
