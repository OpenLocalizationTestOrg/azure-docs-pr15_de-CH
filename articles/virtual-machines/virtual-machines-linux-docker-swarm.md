<properties
   pageTitle="Erste Schritte mit Schwarm Azure Andockfenster"
   description="Beschreibt das Erstellen einer Gruppe von VMs Andockfenster VM-Erweiterung und Schwarm Andockfenster Cluster erstellen."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Verwendung von Andockfenster mit Schwarm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Dieses Thema zeigt eine einfache Methode [Andockfenster](https://www.docker.com/) [Schwarm](https://github.com/docker/swarm) mit Azure Schwarm verwalteten Cluster erstellen. Vier virtuelle Computer erstellt in Azure eine fungiert als Schwarm Manager und drei als Teil des Clusters Andockfenster Hosts. Wenn Sie fertig sind, können Sie Schwarm Cluster und dann Andockfenster verwenden. Darüber hinaus verwenden Sie Azure CLI Aufrufe in diesem Thema Service Management (Asm)-Modus. 

> [AZURE.NOTE] Dieses Thema verwendet Andockfenster mit Schwarm der Azure-CLI *ohne* **Andockfenster Computer** um unabhängig zu zeigen, wie die verschiedenen Tools zusammenarbeiten. **Andockfenster** besitzt **-Schwarm** Switches, die Sie **Andockfenster Computer** verwenden einen Knoten direkt hinzu. Ein Beispiel finden Sie [Andockfenster Computer](https://github.com/docker/machine) . Wenn verpasst **Andockfenster Computer** gegen Azure VMs finden Sie unter [Andockfenster Computer mit Azure verwenden](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Andockfenster Hosts mit Azure Virtual Machines erstellen

In diesem Thema erstellt vier virtuelle Computer, jedoch können Sie gewünschte Anzahl. Rufen Sie mit * &lt;Kennwort&gt; * das gewählte Kennwort ersetzt.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Danach sollten Sie **Azure Vm-Liste** verwenden, um Ihre Azure-VMs angezeigt werden:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Schwarm Schwarm Master VM installieren

Dieses Thema verwendet [containermodell Installation Andockfenster Schwarm Dokumentation](https://github.com/docker/swarm#1---docker-image) – Sie könnte aber auch SSH **Schwarm-Master**. In diesem Modell wird **Schwarm** als Andockfenster Container unter Schwarm heruntergeladen. Führen wir diesen Schritt *von unserem Laptop mit Andockfenster* **Schwarm Master** VM und zu der Cluster-Id erstellen Befehl **Schwarm erstellen**. Cluster-Id liegt wie die Mitglieder der Gruppe Schwarm erkennt **Schwarm** . (Auch Klonen Repository und erstellen Sie die volle Kontrolle und debugging aktivieren.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Die letzte Zeile ist die Cluster-Id; Kopieren Sie sie irgendwo da es wieder beim Verknüpfen von Knoten VMs Schwarm Master verwendet wird "Schwarm" erstellen. In diesem Beispiel ist die Cluster-Id **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Einfach zu löschen, verwenden wir unsere lokalen Andockfenster Installation Anschließen der **Schwarm Master** VM in Azure und Anweisung **Schwarm Master** herunterladen, installieren und führen Sie den Befehl **Erstellen** , der unsere Cluster-Id zurückgibt, die wir später Discovery Zwecken verwenden.
<!-- -->
> Führen Sie zur Bestätigung `docker -H tcp://` * &lt;Hostnamen&gt; * ` images` Container Prozesse **Schwarm Master** -Computer und auf einem anderen Knoten Vergleich (weil wir haben Schwarm Kommando mit der **Rm -** Container entfernt wurde, nachdem der Importvorgang abgeschlossen, mit **Andockfenster Ps - ein** nichts zurückgeben wird nicht) auflisten.:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Wenn Sie **Andockfenster**kennen, wissen Sie, dass die Knoten keine Einträge verfügen, da keine Bilder heruntergeladen und noch ausgeführt wurden.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Knoten VMs zu unserem Andockfenster Cluster beitreten

Für jeden Knoten mithilfe der Azure-CLI Endpunktinformationen auflisten Im folgenden werden noch für Host Andockfenster **Schwarm Knoten 1** um den Knoten Andockfenster Port erhalten.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Mit **Andockfenster** und `-H` Option Andockfenster-Client auf dem Knoten VM zeigen join Knotens Schwarm Cluster-Id und den Knoten Andockfenster Port erstellen (letztere mit **- Adresse**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Das sieht nicht schlecht. Bestätigen Sie, dass dieser **Schwarm** auf **Schwarm Knoten 1** ausgeführt werden, wir geben:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Wiederholen Sie für alle anderen Knoten im Cluster. In diesem Fall werden noch **Schwarm Knoten 2** und **Knoten 3 Schwarm**.

## <a name="begin-managing-the-swarm-cluster"></a>Verwalten des Clusters Schwarm

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

und dann Sie können die Knoten im Cluster:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Laufen Sie Dinge auf Ihrem Schwarm. Inspiration suchen, [https://github.com/docker/swarm/](https://github.com/docker/swarm/)oder vielleicht ein [video](https://www.youtube.com/watch?v=EC25ARhZ5bI)angezeigt.

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
