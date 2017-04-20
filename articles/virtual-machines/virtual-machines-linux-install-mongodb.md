<properties
   pageTitle="MongoDB auf Linux VM installieren | Microsoft Azure"
   description="Informationen Sie zum Installieren und Konfigurieren von MongoDB auf einer virtuellen Linux-Maschine in Azure mit dem Ressourcen-Manager-Bereitstellungsmodell."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Installieren und Konfigurieren von MongoDB auf Linux VM in Azure
[MongoDB](http://www.mongodb.org) ist eine beliebte Open Source, hochleistungsfähigen NoSQL-Datenbank. Dieser Artikel beschreibt, wie installieren und Konfigurieren von MongoDB auf Linux VM in Azure mit dem Ressourcen-Manager-Bereitstellungsmodell. Beispiele, Details wie an:

- [Manuell installieren Sie und konfigurieren Sie eine grundlegende MongoDB-Instanz](#manually-install-and-configure-mongodb-on-a-vm)
- [Erstellen Sie eine grundlegende MongoDB Instanz Ressourcenmanager Vorlage](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Erstellen einer komplexen MongoDB Sharding Cluster mit Replikat wird mithilfe einer Vorlage Ressourcenmanager](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Erforderliche Komponenten
Dieser Artikel ist Folgendes erforderlich:

- ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)).
- angemeldet bei [Azure-CLI](../xplat-cli-install.md)`azure login`
- der Azure-CLI *muss* in Azure-Ressourcen-Manager-Modus`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Manuell installieren und Konfigurieren von MongoDB auf einem virtuellen Computer
MongoDB [bieten Installationshinweise](https://docs.mongodb.com/manual/administration/install-on-linux/) für Linux-Distributionen, einschließlich Red Hat / CentOS, SUSE, Ubuntu und Debian. Im folgenden Beispiel wird ein `CoreOS` VM mit einem SSH-Schlüssel gespeichert unter `.ssh/azure_id_rsa.pub`. Beantworten Sie die Aufforderung für Speicher-Kontonamen, DNS-Namen und Administratoranmeldeinformationen:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Melden Sie sich an die öffentliche IP-Adresse am Ende der vorhergehenden VM Erstellungsschritt VM:

```bash
ssh ops@40.78.23.145
```

Die Installationsquellen für MongoDB hinzufügen, Erstellen einer `yum` Repository-Datei wie folgt:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Die MongoDB Repo-Datei zur Bearbeitung zu öffnen. Fügen Sie die folgenden Zeilen hinzu:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Installation mit MongoDB `yum` wie folgt:

```bash
sudo yum install -y mongodb-org
```

Standardmäßig ist SELinux CentOS-Bilder, die Zugriff auf MongoDB verhindert durchgesetzt. Installieren Sie Policy-Management-Tools und konfigurieren Sie SELinux um MongoDB auf der standardmäßigen TCP-Anschluss 27017 wie folgt angewendet werden können. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Starten Sie den Dienst MongoDB wie folgt:

```bash
sudo service mongod start
```

Prüfen von Verbindung mit dem lokalen MongoDB `mongo` Client:

```bash
mongo
```

Einige Daten hinzufügen und dann testen Sie die MongoDB-Instanz jetzt:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Konfigurieren Sie ggf. MongoDB bei einem Systemneustart automatisch:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Erstellen Sie grundlegende MongoDB Instanz auf CentOS mithilfe einer Vorlage
Sie können eine grundlegende MongoDB-Instanz auf einer einzigen CentOS VM mit der folgenden Azure Schnellstart-Vorlage von Github erstellen. Diese Vorlage verwendet die benutzerdefinierte Skript-Erweiterung für Linux Hinzufügen einer `yum` Repository zu der neu erstellten CentOS VM MongoDB installieren.

- [Grundlegende MongoDB Instanz CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Region. Geben Sie Ihre eigenen Werte wie folgt:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure-CLI Sie zurück auf eine Aufforderung in Sekundenschnelle Bereitstellung erstellen, aber die Installation und Konfiguration dauert einige Minuten. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`, den Namen der Ressourcengruppe entsprechend eingeben. Warten Sie, bis die `ProvisioningState` zeigt "Erfolgreich" vor dem SSH für die VM.

Nach Abschluss der Bereitstellung, SSH für die VM. Die IP-Adresse des VM mit der `azure vm show` Befehl wie im folgenden Beispiel:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Am Ende der Ausgabe der `Public IP address` wird angezeigt. SSH für die VM mit der IP-Adresse der VM:

```bash
ssh ops@138.91.149.74
```

Prüfen von Verbindung mit dem lokalen MongoDB `mongo` Client wie folgt:

```bash
mongo
```

Jetzt testen der Instanz durch einige Daten hinzufügen und wie folgt suchen:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Erstellen eines komplexen MongoDB Sharding Clusters auf CentOS mithilfe einer Vorlage
Sie können einen komplexen MongoDB Sharding Cluster mit der folgenden Azure Schnellstart-Vorlage von Github erstellen. Diese Vorlage folgt die [MongoDB Sharding bewährten Verfahren](https://docs.mongodb.com/manual/core/sharded-cluster-components/) zur Redundanz und hohe Verfügbarkeit. Die Vorlage erstellt zwei Splitter mit drei Knoten in jeder Replikatgruppe. Eine Config Server Replikatsatz mit drei Knoten entsteht auch, plus zwei `mongos` Routerservern zur Konsistenz Applikationen über den Splitter.

- [MongoDB Sharding Cluster CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Bereitstellen von diesem komplexen MongoDB Sharding Cluster erfordert mehr als 20 Kerne normalerweise Standard Core-Anzahl pro Region für ein Abonnement. Öffnen Sie Azure Anfrage die Core-Anzahl erhöht.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUS` Region. Geben Sie Ihre eigenen Werte wie folgt:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure-CLI Sie zurück auf eine Aufforderung in Sekundenschnelle Bereitstellung erstellen, aber die Installation und Konfiguration übernehmen einer Stunde abgeschlossen. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`, den Namen der Ressourcengruppe entsprechend anpassen. Warten Sie, bis die `ProvisioningState` mit VMs zeigt "Erfolgreich".


## <a name="next-steps"></a>Nächste Schritte
In diesen Beispielen lokal Sie MongoDB Instanz von der VM. Wenn Sie MongoDB Instanz von einem anderen virtuellen Computer oder Netzwerk herstellen möchten, stellen Sie sicher, das entsprechende [Netzwerk-Sicherheitsgruppe Regeln erstellt](virtual-machines-linux-nsg-quickstart.md).

Weitere Informationen zur Erstellung von Vorlagen finden Sie unter [Übersicht über Azure Ressource-Manager](../azure-resource-manager/resource-group-overview.md).

Azure-Ressourcen-Manager Vorlagen verwenden benutzerdefinierte Skript Erweiterung herunterladen und Ausführen von Skripts auf die VMs. Weitere Informationen finden Sie unter [Verwenden von Azure benutzerdefinierte Skript Erweiterung mit virtuelle Linux-Computer](virtual-machines-linux-extensions-customscript.md).