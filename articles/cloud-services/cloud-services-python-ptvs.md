<properties
    pageTitle="Python Web- und Workerrollen Rollen mit Visual Studio | Microsoft Azure"
    description="Übersicht über die Verwendung von Python-Tools für Visual Studio Azure Cloud Services einschließlich Webrollen und Worker-Rollen erstellen."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Python Web- und Workerrollen Rollen Python-Tools für Visual Studio

Dieser Artikel enthält eine Übersicht von Python Web- und Workerrollen Rollen mit [Python-Tools für Visual Studio][]. Sie erfahren, wie Sie Visual Studio zum Erstellen und Bereitstellen einer einfachen Cloud-Dienst, Python verwendet.

## <a name="prerequisites"></a>Erforderliche Komponenten

 - Visual Studio 2013 oder 2015
 - [Python-Tools für Visual Studio][] (PTVS)
 - [Azure SDK-Tools für VS 2013][] oder [Azure SDK-Tools für VS 2015][]
 - [Python 2.7 32-Bit-][] oder [Python 3.5 32-bit][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Was werden Python Web- und Workerrollen Rollen?

Azure bietet drei Modelle für die Ausführung der Anwendung berechnen: [Web Apps-Feature in Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms], und [Azure Cloud Services][execution model-cloud services]. Alle drei Modelle unterstützen Python. Cloud-Dienste, darunter Web-und Workerrollen bieten *Plattform als Service (PaaS)*. Im Cloud-Dienst bietet eine Webrolle einen dedizierter Webserver (Internetinformationsdienste) zum Host Front-End-Applikationen während asynchrone langer oder ständige Aufgaben unabhängig von Benutzer oder Eingabe eine Worker-Rolle ausgeführt werden kann.

Weitere Informationen finden Sie unter [Was ist Cloud-Dienst?].

> [AZURE.NOTE]*Suche eine einfache Website?*
Sollten Sie Ihr Szenario nur eine einfache Website Front-End-beinhaltet, mithilfe des einfachen Web Apps in Azure App Service. Sie können einen Cloud-Dienst Upgrades auf Ihrer Website wächst und Bedarf. Finden Sie Artikel, die Entwicklung der Funktion Web Apps in Azure App Service der <a href="/develop/python/">Python Developer Center</a> .
<br />


## <a name="project-creation"></a>Erstellen eines Projekts

In Visual Studio können Sie im Dialogfeld **Neues Projekt** unter **Python** **Azure-Cloud-Dienst** auswählen.

![Dialogfeld "Neues Projekt"](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Azure Cloud Service-Assistenten können Sie neue Web- und Workerrollen Rollen erstellen.

![Dialogfeld "Azure Cloud Service"](./media/cloud-services-python-ptvs/new-service-wizard.png)

Arbeitskraft Rollenvorlage Lieferumfang Standardcode zu einem Azure Speicherkonto Azure Service Bus verbinden.

![Cloud Service-Lösung](./media/cloud-services-python-ptvs/worker.png)

Sie können einen vorhandenen Cloud-Dienst jederzeit Web-oder Arbeitskraft hinzufügen.  Sie können vorhandene Projekte in der Projektmappe hinzufügen oder neue erstellen.

![Rollenbefehl hinzufügen](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Cloud-Dienst kann in verschiedenen Sprachen implementierten Rollen enthalten.  Sie können z. B. eine Python Webrolle mit Django, Python und Worker-Rollen in C# implementiert.  Sie können einfach Ihre Rollen mit Servicebuswarteschlangen oder speicherwarteschlangen kommunizieren.

## <a name="install-python-on-the-cloud-service"></a>Installieren Sie Python auf Cloud-Dienst

>[AZURE.WARNING] Setupskripten (zum Zeitpunkt der letzten dieser Artikel Aktualisierung) mit Visual Studio installiert werden, funktionieren nicht. Dieser Abschnitt beschreibt einen Workaround.

Das Hauptproblem bei der Setup-Skripts sind sie Python nicht installieren. Definieren Sie zuerst zwei [Aufgaben](cloud-services-startup-tasks.md) in der Datei [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . Die erste Aufgabe (**PrepPython.ps1**) herunter und installiert die Python-Laufzeit. Die zweite Aufgabe (**PipInstaller.ps1**) führt Pip um Abhängigkeiten installiert haben.

Die unten angegebenen Skripts waren Python 3.5 abzielen. Möchten Sie die Version 2.x Python, festlegen die Variable **PYTHON2** -Datei **auf** zwei Startaufgaben und Runtime-Aufgabe: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

Die Variablen **PYTHON2** und **PYPATH** Arbeitskraftaufgabe Start hinzugefügt werden soll. Die Variable **PYPATH** wird nur verwendet, wenn die Variable **PYTHON2** auf **on**festgelegt ist.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Beispiel für ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Als Nächstes erstellen Sie die Dateien **PrepPython.ps1** und **PipInstaller.ps1** der **. / bin** Ordner Ihrer Rolle.

#### <a name="preppythonps1"></a>PrepPython.ps1

Diese Skripts werden Python installiert. Wenn **PYTHON2** Umgebung Variable festgelegt wird **und Python 2.7** installiert, andernfalls Python 3.5 installiert.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Dieses Skript ruft Pip und Abhängigkeit in der Datei **requirements.txt** installiert. Wenn **PYTHON2** Umgebung Variable festgelegt wird **und Python 2.7** verwendet, andernfalls Python 3.5 verwendet.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>LaunchWorker.ps1 ändern

>[AZURE.NOTE] Bei einem Projekt **Worker-Rolle** muss **LauncherWorker.ps1** Datei führen Sie die Datei. In einem Projekt **Webrolle** wird die Datei stattdessen in den Projekteigenschaften definiert.

**Bin\LaunchWorker.ps1** wurde ursprünglich Menge Vorbereitungsarbeit tun, aber eigentlich funktioniert. Ersetzen Sie den Inhalt in der Datei mit dem folgenden Skript.

Dieses Skript ruft **worker.py** -Datei aus dem Projekt Python. Wenn **PYTHON2** Umgebung Variable festgelegt wird **und Python 2.7** verwendet, andernfalls Python 3.5 verwendet.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Die Visual Studio-Vorlagen sollten haben eine **ps.cmd** in der **. / bin** Ordner. Diese Shell-Skript ruft die PowerShell Wrapperskripts oben und bietet Protokollierung basiert auf der PowerShell-Wrapper aufgerufen. Diese Datei wurde nicht erstellt, ist hier was sein sollte. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Lokal ausführen

Wenn das Cloud-Dienst-Projekt als Startprojekt festlegen, und drücken Sie F5, Cloud-Dienst im lokalen Azure-Emulator ausgeführt.

PTVS unterstützt werden (z. B. Haltepunkte) Emulator, Debuggen starten nicht funktionieren.

Zum Debuggen Ihrer Web- und Workerrollen Rollen können Rolle Projekt als Startprojekt festlegen und stattdessen Debuggen.  Sie können auch mehrere Startprojekte festlegen.  Mit der rechten Maustaste der Projektmappe, und wählen Sie dann **Startprojekte festlegen**.

![Lösung starten-Projekteigenschaften](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Azure veröffentlichen

Veröffentlichen, Maustaste Cloud Service-Projekt in der Projektmappe, und wählen Sie dann **Veröffentlichen**.

![Microsoft Azure veröffentlichen anmelden](./media/cloud-services-python-ptvs/publish-sign-in.png)

Führen Sie den Assistenten. Wenn Sie möchten, aktivieren Sie den Remotedesktop. Remotedesktop ist hilfreich, wenn Sie etwas debuggen müssen.

Beim Konfigurieren von Optionen fertig sind, klicken Sie auf **Veröffentlichen**.

Fortschritte im Ausgabefenster angezeigt wird und Microsoft Azure Activity Log-Fenster wird angezeigt.

![Microsoft Azure Aktivität Protokollfenster](./media/cloud-services-python-ptvs/publish-activity-log.png)

Bereitstellung dauert einige Minuten und Ihre Rollen Web bzw. Arbeitskraft auf Windows Azure ausgeführte!

### <a name="investigate-logs"></a>Überprüfen von Protokollen

Nach den Cloud-Dienst virtuellen Computer gestartet und Python installiert, können Sie den Protokollen Fehlermeldungen anzeigen. Diese Protokolle befinden sich in der **C:\Resources\Directory\\{Rolle} \LogFiles** Ordner. **PrepPython.err.txt** haben mindestens einen Fehler darin aus, wenn das Skript versucht zu erkennen, wenn Python installiert und **PipInstaller.err.txt** möglicherweise eine veraltete Version von Pip beschweren.

## <a name="next-steps"></a>Nächste Schritte

Ausführlichere Informationen zum Arbeiten mit Web- und Workerrollen Rollen in Python-Tools für Visual Studio finden Sie in der Dokumentation PTVS:

- [Cloud-Dienst-Projekte][]

Weitere Informationen zur Azure Services von Web- und Workerrollen Rollen wie Azure Storage oder Service Bus finden Sie in den folgenden Artikeln.

- [BLOB-Dienst][]
- [Dienst][]
- [Warteschlangendienst][]
- [Service Bus-Warteschlangen][]
- [Service Bus Topics][]


<!--Link references-->

[Was ist Cloud-Dienst?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[BLOB-Dienst]: ../storage/storage-python-how-to-use-blob-storage.md
[Warteschlangendienst]: ../storage/storage-python-how-to-use-queue-storage.md
[Dienst]: ../storage/storage-python-how-to-use-table-storage.md
[Service Bus-Warteschlangen]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Service Bus Topics]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python-Tools für Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud-Dienst-Projekte]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK-Tools für VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK-Tools für VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bit]: https://www.python.org/downloads/
[Python 3.5 32-bit]: https://www.python.org/downloads/
