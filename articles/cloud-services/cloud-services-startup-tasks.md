<properties 
pageTitle="Ausführen von Aufgaben in Azure Cloud Services | Microsoft Azure" 
description="Aufgaben bei der Vorbereitung Ihrer Cloudumgebung-Dienst für Ihre Anwendung. Dies zeigt Ihnen, wie Aufgaben und machen" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Konfigurieren und Ausführen von Aufgaben für einen Clouddienst

Aufgaben können Sie Operationen ausführen, bevor einer gestartet wird. Vorgänge, die Sie ausführen möchten enthalten eine Komponente registrieren von COM-Komponenten, Registrierungsschlüssel festlegen oder einen langer Prozess starten.

>[AZURE.NOTE] Aufgaben gelten nicht für virtuelle Maschinen, nur Cloud Service Web- und Workerrollen.

## <a name="how-startup-tasks-work"></a>Funktionsweise von Aufgaben

Aufgaben sind Aktionen, die ausgeführt werden, bevor Ihre Rollen beginnen und mit [das Aufgabenelement in der [Startup] -Element] in der Datei [ServiceDefinition.csdef] definiert sind. Aufgaben sind häufig Batchdateien, aber sie können auch Fenster und Batchdateien, die PowerShell-Skripts gestartet werden.

Umgebungsvariablen übergeben Informationen an eine Startaufgabe und lokaler Speicher kann zum Übergeben von Informationen aus einer Startaufgabe. Z. B. eine Umgebungsvariable Geben Sie den Pfad zu einem Programm zu installieren und Dateien geschrieben werden können, in den lokalen Speicher durch Ihre Rollen dann später gelesen werden kann.

Die Startaufgabe kann Informationen und Fehler durch die Umgebungsvariable **TEMP** angegebenen Verzeichnis protokollieren. Während die Startaufgabe löst die Umgebungsvariable **TEMP** auf *C:\\Ressourcen\\Temp\\[Guid]. [ Rollenname]\\RoleTemp* Verzeichnis unter der Cloud.

Aufgaben können auch mehrmals zwischen Neustarts ausgeführt werden. Beispielsweise der starttask wird ausgeführt, wenn Sie jedes Mal, wenn die Rolle recycelt und Rolle recycelt immer einen Neustart enthalten. Aufgaben sollten so geschrieben werden, die mehrmals ohne Probleme ausgeführt werden können.

Aufgaben müssen ein **Errorlevel** oder die Exitcode 0 für den Startvorgang abgeschlossen enden. Eine Startaufgabe NULL **Errorlevel**endet, wird die Rolle nicht gestartet.


## <a name="role-startup-order"></a>Startreihenfolge Rolle

Die folgende Liste enthält die Startprozedur Rolle in Azure:

1. Die Instanz als **Starten** gekennzeichnet ist und Datenverkehr nicht empfangen.

2. Alle Startaufgaben werden nach ihrer **TaskType** Attribut ausgeführt.
    - **Einfache** Aufgaben erfolgt synchron nacheinander.
    - Die ** **Vorder-** und** Aufgaben werden asynchron, parallel Start-Vorgang gestartet.  
       
    > [AZURE.WARNING] IIS kann nicht vollständig konfiguriert Phase Task Starten des Startvorgangs rollenspezifische Daten möglicherweise nicht verfügbar. Aufgaben, die rollenspezifische Daten sollten [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)verwenden.

3. Der Hostprozess Rolle gestartet und die Website in IIS erstellt.

4. Der [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) wird aufgerufen.

5. **Die Instanz wird markiert** und Verkehr der Instanz weitergeleitet.

6. Der [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) wird aufgerufen.


## <a name="example-of-a-startup-task"></a>Beispiel für eine Startaufgabe

Aufgaben werden in der Datei [ServiceDefinition.csdef] im **Task** -Element definiert. **CommandLine** -Attribut gibt den Namen und die Parameter der Batchdatei starten oder Konsole Befehl **ExecutionContext** -Attribut gibt die Berechtigungsebene der Startaufgabe und **TaskType** -Attribut gibt an, wie die Aufgabe ausgeführt wird.

In diesem Beispiel eine Umgebungsvariable **MyVersionNumber**für die Startaufgabe erstellt und dem Wert "**1.0.0.0**" festgelegt.

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

Im folgenden Beispiel schreibt die Batchdatei **Startup.cmd** der Zeile "die aktuelle Version ist 1.0.0.0" in der Datei StartupLog.txt im Verzeichnis TEMP-Umgebungsvariable angegeben. Die `EXIT /B 0` Zeile stellt sicher, dass die Startaufgabe ein **Errorlevel** 0 endet.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] In Visual Studio die Eigenschaft **In Ausgabeverzeichnis kopieren** für die Batchdatei starten sollten festgelegt werden immer **Kopieren** um sicherzustellen, dass die Batchdatei starten ordnungsgemäß auf Azure bereitgestellt (**Approot\\Bin** Webrollen und **Approot** für Workerthreads).

## <a name="description-of-task-attributes"></a>Beschreibung der Aufgabenattribute

Folgender Abschnitt beschreibt die Attribute des **Task** -Elements in der Datei [ServiceDefinition.csdef] :

**CommandLine** - gibt die Befehlszeile für den Start-Vorgang:

- Der Befehl mit optionalen Befehlszeilenparametern Startaufgabe beginnt.
- Häufig ist dies der Name einer Batchdatei cmd oder bat.
- Der Task bezieht den AppRoot\\Bin-Ordner für die Bereitstellung. Umgebungsvariablen werden nicht erweitert, bestimmen den Pfad und Dateinamen der Aufgabe. Wenn Erweiterung erforderlich ist, können Sie eine kleine CMD Skript erstellen, das die Startaufgabe aufruft.
- Ist eine oder eine Batchdatei, die [PowerShell-Skript](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)beginnt.

**ExecutionContext** - gibt die Berechtigungsstufe für den Vorgang starten. Die Berechtigungsstufe kann beschränkt oder erweitert:

- **beschränkt**  
Die Startaufgabe wird mit den gleichen Berechtigungen wie die Rolle. Wenn das **ExecutionContext** -Attribut für das Element [Runtime] auch **beschränkt**ist, werden Benutzerrechte verwendet.

- **Erhöhte**  
Die Startaufgabe mit Administratorrechten ausgeführt wird. Dadurch Start Programme installieren IIS-Konfiguration ändern, führen Sie Registrierungseinträge und anderen Administrator auf Aufgaben ohne die Berechtigungsebene der Rolle.  

> [AZURE.NOTE] Die Privilegstufe des Start-Vorgang muss nicht die Rolle identisch sein.

**TaskType** - gibt an, wie eine Startaufgabe ausgeführt wird.

- **einfache**  
Aufgaben werden einzeln nacheinander in der Reihenfolge angegeben, in der [ServiceDefinition.csdef] -Datei synchron ausgeführt. Beim **Start einfache** ein **Errorlevel** null endet, ist die **einfache** Startaufgabe ausgeführt. Es gibt keine **einfache** Startaufgaben mehr ausführen, wird die Rolle gestartet.   

    > [AZURE.NOTE] **Einfache** Aufgabe mit NULL **Errorlevel**endet, wird die Instanz blockiert. Nachfolgenden **einfachen** Aufgaben und die Rolle werden nicht gestartet.

    Um sicherzustellen, dass die Batchdatei ein **Errorlevel** null endet, Befehl `EXIT /B 0` am Ende der Batch-Datei.

- **Hintergrund**  
Aufgaben werden mit den Start der Rolle asynchron ausgeführt.

- **Vordergrund**  
Aufgaben werden mit den Start der Rolle asynchron ausgeführt. Der Hauptunterschied zwischen dem **Vordergrund** und **Hintergrund** werden **Vordergrund** Aufgabe verhindert die Rolle von recycling oder heruntergefahren wird, bis der Vorgang beendet ist. **Hintergrundaufgaben** müssen diese Einschränkung nicht.

## <a name="environment-variables"></a>Umgebungsvariablen

Umgebungsvariablen sind eine Möglichkeit, Informationen zu einem Start-Vorgang übergeben. Beispielsweise setzen Sie den Pfad ein Blob mit einem Programm zu installieren, Ihre Rolle, Portnummern oder Einstellungen auf die Funktionen der Startaufgabe.

Es gibt zwei Arten von Umgebungsvariablen für Aufgaben. statische Variablen und Variablen basierend auf Member der [RoleEnvironment] -Klasse. Sind im Abschnitt [Umgebung] der Datei [ServiceDefinition.csdef] , und beide [Variablen] Element und **Name** -Attribut verwenden.

Statische Variablen verwendet das **Value** -Attribut des [Variable] -Elements. Im obigen Beispiel erstellt die Umgebungsvariable **MyVersionNumber** hat einen statischen Wert**1.0.0.0"**. Ein weiteres Beispiel wäre eine Umgebungsvariable **StagingOrProduction** erstellen, die Sie zu "**staging**" oder "**Produktion**" Werte manuell können Aktionen andere Startdatei basierend auf dem Wert der Umgebungsvariable **StagingOrProduction** .

Basierend auf Member der RoleEnvironment-Klasse Umgebungsvariablen verwenden nicht das **Value** -Attribut des [Variable] -Elements. Stattdessen wird [RoleInstanceValue] untergeordnetes Element mit dem entsprechenden **XPath** -Attributwert eine Umgebungsvariable basierend auf einen bestimmten Member der [RoleEnvironment] -Klasse erstellen Für das **XPath** -Attribut auf verschiedene [RoleEnvironment] Werte finden Sie [hier](cloud-services-role-config-xpath.md).



Um eine Umgebungsvariable zu erstellen, die "**true**" ist, wenn die Instanz Serveremulator und "**false**" ausgeführt wird, wenn in der Cloud ausgeführt, z. B. [Variablen] und [RoleInstanceValue] Folgendes:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie einige [Startaufgaben](cloud-services-startup-tasks-common.md) mit dem Cloud-Dienst.

[Paket](cloud-services-model-and-package.md) dem Cloud-Dienst.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Aufgabe]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Start]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Laufzeit]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Umgebung]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variable]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx