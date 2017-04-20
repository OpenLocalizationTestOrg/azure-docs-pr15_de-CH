<properties
   pageTitle="Bereitstellen einen ASP.NET Core Linux Andockfenster Container zu einem Remotehost Andockfenster | Microsoft Azure"
   description="Erfahren Sie, wie mit Visual Studio Tools for Andockfenster Andockfenster Container auf eine Azure Andockfenster Host Linux VM eine ASP.NET Core Web app Bereitstellung"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Bereitstellen eines ASP.NET Containers zu einem Remotehost Andockfenster

## <a name="overview"></a>Übersicht
Andockfenster ist ein einfacher Container wie in mancher Hinsicht einer virtuellen Maschine zum Host-Applikationen und Services verwenden können.
Dieses Lernprogramm führt Sie mithilfe von [Visual Studio 2015 Tools für Andockfenster](http://aka.ms/DockerToolsForVS) Erweiterung eine Anwendung ASP.NET Kern zu einem Host Andockfenster Azure mithilfe von PowerShell bereitstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten
Folgendes muss dieses Lernprogramm abgeschlossen:

- Erstellen einer Azure Andockfenster Host VM unter [Verwendung Andockfenster Computer mit Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Installieren Sie [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- Installieren Sie [Visual Studio 2015 Tools für Andockfenster - Vorschau](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. erstellen Sie 1. eine ASP.NET Core Web app
Die folgenden Schritte führt Sie durch die Erstellung einer einfachen ASP.NET Core-app, die in diesem Lernprogramm verwendet wird.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Andockfenster Unterstützung hinzufügen

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. DockerTask.ps1-PowerShell-Skript verwenden 

1.  Öffnen Sie ein PowerShell Prompt zum Stammverzeichnis des Projekts. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Überprüfen der Remote-Host ausgeführt. Müsste Status = wird ausgeführt 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Wenn die Andockfenster Beta verwenden, wird nicht der Host hier.

1.  Erstellen der Anwendung mit dem Build-Parameter

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Wenn Sie Andockfenster Beta verwenden, lassen Sie das Argument Computer
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Führen Sie die Anwendung mit dem Parameter ausführen

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Wenn Sie Andockfenster Beta verwenden, lassen Sie das Argument Computer
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Nach Abschluss der Andockfenster erhalten Sie Ergebnisse, die der folgenden ähnelt:

    ![Ihre app anzeigen][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
