<properties
    pageTitle="Andockfenster Hosts in Azure mit Andockfenster erstellen | Microsoft Azure"
    description="Beschreibt Andockfenster Maschine Andockfenster Hosts in Azure erstellen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Andockfenster Computer Azure Treiber verwenden

[Andockfenster](https://www.docker.com/) ist eines der beliebtesten Ansätze bei der Speichervirtualisierung, die Linux-Container anstelle von virtuellen Maschinen zu isolieren Anwendungsdaten und Arbeiten mit freigegebenen Ressourcen verwendet. Dieses Thema beschreibt, wann und wie Sie [Andockfenster Computer](https://docs.docker.com/machine/) (die `docker-machine` Befehl) neue Linux-VMs in Azure als Host Andockfenster für Ihre Linux-Container erstellen.


## <a name="create-vms-with-docker-machine"></a>VMs mit Andockfenster erstellen

Virtuelle Computer erstellen Andockfenster Host in Azure mit der `docker-machine create` Befehl mit der `azure` Treiber-Argument für die Treiberoption (`-d`) und alle anderen Argumente. 

Das folgende Beispiel verwendet die Standardwerte jedoch öffnet Port 80 auf dem virtuellen Computer mit dem Internet mit einem Nginx-Container macht testen `ops` der angemeldete Benutzer für SSH und ruft die neue VM `machine`. 

Typ `docker-machine create --driver azure` Optionen und Standardwerte; Sie können auch die [Andockfenster Azure Treiberdokumentation](https://docs.docker.com/machine/drivers/azure/)lesen. (Beachten Sie Wenn Sie zweistufige Authentifizierung aktiviert haben, werden aufgefordert, wird für die Authentifizierung mithilfe des zweiten Faktors).

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Die Ausgabe sieht etwas, je nachdem, ob Sie zwei-Faktor-Authentifizierung in Ihrem Konto konfiguriert haben.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Konfigurieren der Andockfenster shell

Geben Sie `docker-machine env <VM name>` zu sehen, was Sie tun müssen, um der Shell konfigurieren. 

```bash
docker-machine env machine
```

Die druckt die Informationen, die etwa folgendermaßen aussieht. Beachten Sie, dass die IP-Adresse zugeordnet ist die müssen die VM testen.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Sie können den vorgeschlagenen Konfigurationsbefehl ausführen oder die Umgebungsvariablen selbst festlegen. 

## <a name="run-a-container"></a>Führen Sie einen container

Jetzt eines einfachen Webservers ausführen zu testen, ob alles ordnungsgemäß funktioniert. Hier wir standard Nginx-Bild verwenden, angeben, dass Port 80 überwachen soll und daß die VM neu startet der Container neu sowie (`--restart=always`). 

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

Und überprüfen den laufenden Container geben `docker-machine ip <VM name>` auf die IP-Adresse (wenn von vergessen der `env` Befehl):

![Ausgeführte Ngnix container](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie möchten, können Sie die Azure [VM-Erweiterung Andockfenster](virtual-machines-linux-dockerextension.md) denselben Vorgang mit der Azure-CLI oder Azure Resource Manager Vorlagen ausprobieren. 

Weitere Beispiele für die Arbeit mit Andockfenster finden Sie unter [Arbeiten mit Andockfenster](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinden [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstarts HealthClinic.biz Demo finden Sie unter [Azure Developer Tools Schnellstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

