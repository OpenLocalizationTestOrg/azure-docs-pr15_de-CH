<properties
   pageTitle="Installieren .NET Cloud Service Rolle | Microsoft Azure"
   description="Dieser Artikel beschreibt das Installieren von .NET Framework auf Cloud Service Web- und Workerrollen"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Installieren Sie .NET eine Cloud Service-Rolle 

Dieser Artikel beschreibt die Installation von .NET Framework auf Cloud Service Web- und Workerrollen. Diese Schritte können .NET 4.6.1 auf Azure Gast OS Familie 4 installieren. Die neuesten Informationen zu Gast-BS-Versionen finden Sie unter [Azure Gast OS und SDK-Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md).

Die Installation von .NET auf Ihrer Web- und Workerrollen Rollen umfasst einschließlich .NET Installer-Paket als Teil des Projekts Cloud und starten das Installationsprogramm im Rahmen der Aufgaben der Rolle.  

## <a name="add-the-net-installer-to-your-project"></a>Installer .NET zum Projekt hinzufügen
- Herunterladen der Webinstaller für .NET Framework installieren möchten
    - [4.6.1 .NET web Installer](http://go.microsoft.com/fwlink/?LinkId=671729)
- Für eine Webrolle
  1. Im **Projektmappen-Explorer**im Projekt für den Cloud-Dienst **Rollen** rechts klicken Sie auf Ihre Rolle und wählen Sie **Hinzufügen > Neuer Ordner**. Erstellen Sie einen Ordner mit dem Namen *bin*
  2. Klicken Sie mit der rechten Maustaste auf den Ordner **Bin** , und wählen Sie **Hinzufügen > Vorhandenes Element**. Wählen Sie den Installer für .NET und Bin-Ordner hinzufügen.
- Für eine Worker-Rolle
  1. Klicken Sie mit der rechten Maustaste auf die Rolle und wählen Sie **Hinzufügen > Vorhandenes Element**. Wählen Sie den Installer für .NET und Rolle hinzufügen. 

Dateien hinzugefügt zum Rolle wird automatisch dem Cloud Service-Paket hinzugefügt und an derselben Position auf dem virtuellen Computer bereitgestellt. Wiederholen Sie diesen Vorgang für alle Web- und Workerrollen Rollen im Cloud-Dienst, haben alle Rollen eine Kopie des Installationsprogramms.

> [AZURE.NOTE] Installieren Sie .NET 4.6.1 auf Ihre Cloud-Dienst Rollen, selbst wenn die Anwendung .NET 4.6 abzielt. Azure-Gastbetriebssystem enthält Updates [3098779](https://support.microsoft.com/kb/3098779) und [3097997](https://support.microsoft.com/kb/3097997). .NET 4.6 auf Updates installieren kann zu Problemen führen, wenn Ihren, so sollten Sie .NET 4.6.1 statt .NET 4.6 direkt installieren. Weitere Informationen finden Sie unter [KB 3118750](https://support.microsoft.com/kb/3118750).

![Inhalt der Rolle mit Installer-Dateien][1]

## <a name="define-startup-tasks-for-your-roles"></a>Aufgaben für Ihre Rollen definieren
Aufgaben können Sie Operationen ausführen, bevor einer gestartet wird. Installieren von.NET Framework als Teil der Startaufgabe wird sichergestellt, dass das Framework installiert ist, bevor die Anwendung Code ausführen. Informationen zum Systemstart Aufgaben finden Sie unter: [Startaufgaben in Azure ausgeführt](cloud-services-startup-tasks.md). 

1. Fügen Sie Folgendes in die Datei *ServiceDefinition.csdef* unter dem **WebRole** oder **WorkerRole** Knoten für alle Rollen:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Die oben genannten Konfiguration wird Konsole Befehl *install.cmd* mit Administratorrechten ausgeführt, damit sie .NET Framework installieren können. Die Konfiguration wird auch ein LocalStorage mit dem Namen *NETFXInstall*erstellt. Das Startskript wird den temporären Ordner hier lokalen Speicher verwenden, damit der Installer für .NET Framework heruntergeladen und diese Ressource installiert festgelegt. Es ist wichtig, die Größe dieser Ressource 1024 MB, um sicherzustellen, dass das Framework ordnungsgemäß installiert wird. Weitere Informationen zum Starten von Aufgaben finden Sie unter: [Startaufgaben Cloud-Dienst](cloud-services-startup-tasks-common.md) 

2. Erstellen einer Datei **install.cmd** durch Rechtsklick auf die Rolle alle Rollen hinzufügen und auswählen **Hinzufügen > Vorhandenes Element...**. Alle Rollen haben nun .NET Installer-Datei sowie die Datei "install.cmd".
    
    ![Rolle Inhalt mit allen Dateien][2]

    > [AZURE.NOTE] Verwenden Sie einen Texteditor wie Notepad zum Erstellen dieser Datei. Wenn Sie mit Visual Studio erstellt eine Textdatei und benennen Sie sie "cmd" die Datei kann weiterhin enthalten eine UTF-8-Byte-Order Mark und mit der ersten Zeile des Skripts zu einem Fehler führt. Wurden mit Visual Studio erstellt Datei belassen hinzufügen REM (Hinweis) in die erste Zeile der Datei bei der Ausführung ignoriert wird. 

3. Fügen Sie das folgende Skript, um die Datei " **install.cmd** ":

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Das Installationsskript überprüft, ob die angegebene Version von .NET Framework bereits auf dem Computer installiert ist, durch Abfragen der Registrierung. Wenn die Version .NET nicht installiert .net Web Installer gestartet. Um alle Probleme beheben protokolliert das Skript alle Aktivitäten in einer Datei namens *Startuptasklog-(Currentdatetime) .txt* *InstallLogs* lokalen Speicher gespeichert.

    > [AZURE.NOTE] Das Skript zeigt weiterhin .NET 4.5.2 oder .NET 4.6 für Continuity installieren. Es muss keine .NET 4.5.2 installieren auf Azure Gastbetriebssystem ist. Anstatt .NET 4.6 installieren Sie direkt .NET 4.6.1 durch [KB 3118750](https://support.microsoft.com/kb/3118750).
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Konfigurieren der Diagnose Start Task-Protokolle BLOB-Speicher übertragen 
Konfigurieren Sie zur Vereinfachung der Installationsprobleme Problembehandlung Azure Diagnostics aus, um alle Protokolldateien Startskript oder das Installationsprogramm .NET BLOB-Speicher übertragen. Dabei können Sie Protokolle anzeigen, indem einfach die Protokolldateien aus dem BLOB-Speicher herunterladen anstatt Remotedesktop in die Rolle.

So konfigurieren Diagnose öffnen *diagnostics.wadcfgx* und fügen Sie Folgendes unter dem Knoten **Verzeichnisse** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Hierfür wird Azure Diagnostics aus, um alle Dateien im *Verzeichnis unter der Ressource *NETFXInstall* * Diagnose Speicherkonto *Netfx installieren* BLOB-Container übertragen.

## <a name="deploying-your-service"></a>Ihr Dienst bereitstellen 
Wenn Sie Ihren Dienst bereitstellen wird die Aufgaben ausgeführt und .NET Framework installieren, wenn es nicht bereits installiert ist. Rollen werden die ausgelastet und das Framework installiert möglicherweise auch neu starten, wenn die Framework-Installation erfordert. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Installieren von.NET Framework][]
- [Gewusst wie: bestimmen, welche.NET Framework-Versionen installiert sind][]
- [Problembehandlung bei.NET Framework-Installationen][]

[Gewusst wie: bestimmen, welche.NET Framework-Versionen installiert sind]: https://msdn.microsoft.com/library/hh925568.aspx
[Installieren von.NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Problembehandlung bei.NET Framework-Installationen]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
