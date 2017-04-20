<properties
   pageTitle="Konfiguration eines Hosts Andockfenster mit VirtualBox | Microsoft Azure"
   description="Schritt eine Andockfenster Standardinstanz Andockfenster Computer- und VirtualBox konfigurieren"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Konfiguration eines Hosts Andockfenster mit VirtualBox

## <a name="overview"></a>Übersicht
Dieser Artikel führt Sie durch die Konfiguration einer Andockfenster Standardinstanz Andockfenster Computer- und VirtualBox. Wenn Sie die [Andockfenster für Beta](http://beta.docker.com/)verwenden, ist diese Konfiguration nicht erforderlich.

## <a name="prerequisites"></a>Erforderliche Komponenten
Die folgenden Tools installiert werden müssen.

- [Andockfenster Toolbox](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfigurieren des Clients Andockfenster mit Windows PowerShell

Konfigurieren Sie einen Client Andockfenster einfach Windows PowerShell öffnen Sie, und gehen Sie folgendermaßen vor:

1. Erstellen Sie eine Standardinstanz Andockfenster Host.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Überprüfen Sie die Standardinstanz konfiguriert und ausgeführt. (Diese sollte eine Instanz mit dem Namen 'Default' ausführen angezeigt.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![Andockfenster Computer ls Ausgabe][0]
 
1. Voreinstellung als aktuellen Host und die Shell konfigurieren.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Die aktive Andockfenster Container anzeigen Die Liste sollte leer sein.

    ```PowerShell
    docker ps
    ```

    ![Andockfenster Ps-Ausgabe][1]
 
> [AZURE.NOTE]Bei jedem Neustart auf dem Entwicklungscomputer müssen Sie Ihre lokale Andockfenster Host starten.
> Hierzu führen Sie folgenden Befehl an der Befehlszeile: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
