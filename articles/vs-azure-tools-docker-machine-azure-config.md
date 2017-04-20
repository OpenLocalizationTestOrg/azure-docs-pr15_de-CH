<properties
   pageTitle="Andockfenster Hosts in Azure mit Andockfenster erstellen | Microsoft Azure"
   description="Beschreibt Andockfenster Maschine Andockfenster Hosts in Azure erstellen."
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

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Erstellen der Andockfenster Hosts in Azure Andockfenster-Maschine

[Andockfenster](https://www.docker.com/) Container ausführen muss einen Host VM Andockfenster Daemon ausgeführt.
Dieses Thema beschreibt, wie mit den Befehl [Andockfenster Computer](https://docs.docker.com/machine/) neue Linux-VMs konfiguriert Andockfenster Daemon in Azure ausgeführt. 

**Hinweis:** 
- *Dieser Artikel hängt Andockfenster Computer Version 0.7.0 oder höher*
- *Windows-Container werden durch Andockfenster Computer in Zukunft unterstützt*

## <a name="create-vms-with-docker-machine"></a>VMs mit Andockfenster erstellen

Virtuelle Computer erstellen Andockfenster Host in Azure mit den `docker-machine create` Befehl mit der `azure` Treiber. 

Azure Treiber benötigen Ihre Abonnement-ID [Azure-CLI](xplat-cli-install.md) oder [Azure-Portal](https://portal.azure.com) können Sie Ihre Azure-Abonnement abrufen. 

**Mithilfe des Azure-Portals**
- Wählen Sie auf der Navigationsleiste Abonnements und Abonnement-ID kopieren.

**Verwendung der Azure-CLI**
- Typ ```azure account list``` und die Abonnement-Id kopieren.

Typ `docker-machine create --driver azure` Optionen und Standardwerte.
Finden Sie unter [Andockfenster Azure Treiber Dokumentation](https://docs.docker.com/machine/drivers/azure/) Weitere Informationen. 

Das folgende Beispiel verwendet die Standardwerte jedoch optional Port 80 auf dem virtuellen Computer für den Internetzugang öffnen. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Wählen Sie einen Host Andockfenster Andockfenster-Maschine
Haben Sie einen Eintrag im Andockfenster Maschine für den Host, können Sie den Standardhost Ausführung Andockfenster Befehle festlegen.
##<a name="using-powershell"></a>Mithilfe von PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Bash verwenden

```bash
eval $(docker-machine env MyDockerHost)
```

Sie können jetzt Andockfenster Befehle für den angegebenen Host ausführen.

```
docker ps
docker info
```

## <a name="run-a-container"></a>Führen Sie einen container

Mit einem konfigurierten Hosts können jetzt ausführen ein einfachen Webservers zu testen, ob Ihr Server ordnungsgemäß konfiguriert wurde.
Hier wir standard Nginx-Bild verwenden, angeben, dass Port 80 überwachen soll und wenn Host VM neu gestartet wird, wird der Container neu sowie (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Die Ausgabe sollte wie folgt aussehen:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Der Testcontainer

Laufende Containern überprüfen `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Und überprüfen den laufenden Container geben `docker-machine ip <VM name>` auf die IP-Adresse in den Browser eingeben:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Ausgeführte Ngnix container](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Zusammenfassung
Andockfenster-Maschine können Sie problemlos Andockfenster Hosts in Azure für die einzelnen Andockfenster Host-Validierung bereitstellen.
Produktion von Containern hosting finden Sie unter [Azure Container Service](http://aka.ms/AzureContainerService)

Entwicklung von .NET Core Anwendung mit Visual Studio finden Sie unter [Andockfenster Tools für Visual Studio](http://aka.ms/DockerToolsForVS)
