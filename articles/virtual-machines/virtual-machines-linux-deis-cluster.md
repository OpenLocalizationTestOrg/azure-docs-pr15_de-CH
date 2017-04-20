<properties
   pageTitle="Bereitstellen einen 3-Knoten-Cluster Deis | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie einen 3-Knoten erstellt Deis Cluster Azure mit einer Azure-Ressourcen-Manager-Vorlage"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Bereitstellen einen 3-Knoten-Cluster Deis

Dieser Artikel führt Sie durch die Bereitstellung einer [Deis](http://deis.io/) Cluster auf Azure. Es umfasst alle erforderlichen Zertifikate bereitstellen und Skalierung eine Beispiel **gehen** Anwendung neu bereitgestellten Cluster erstellen.

Das folgende Diagramm zeigt die Architektur des Systems bereitgestellt. Ein Systemadministrator verwaltet die Cluster mit Tools wie **deis** und **Deisctl**Deis. Über ein Azure Lastenausgleich Verbindungsaufbau Anschlüsse auf einem der Knoten im Cluster weitergeleitet. Clients Zugriff bereitgestellt, mit der Load-Balancer. In diesem Fall Lastenausgleich leitet den Datenverkehr an einen Deis Router-Netz, das weitere Datenverkehr mit entsprechenden Andockfenster Containern im Cluster gehostet routs.

  ![Architekturdiagramm des bereitgestellten Desis cluster](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Die folgenden Schritte ausführen, müssen Sie:

 * Ein aktives Azure-Abonnement. Wenn Sie haben, erhalten Sie einen kostenlosen Trail auf [azure.com](https://azure.microsoft.com/).
 * Eine Arbeits- oder Schulcomputer Id für Azure Ressourcengruppen. Wenn Sie ein Konto und melden Sie sich mit einem Microsoft-Id, müssen Sie [aus Ihrem persönlichen Id Arbeit erstellen](virtual-machines-windows-create-aad-work-id.md).
 * Entweder - Betriebssystemen Client - [Azure PowerShell](../powershell-install-configure.md) oder [CLI für Mac, Linux und Windows Azure](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL wird verwendet, um die erforderlichen Zertifikate generieren.
 * Ein Git Client wie [Git Bash](https://git-scm.com/).
 * Zum Testen der beispielanwendung benötigen Sie auch einen DNS-Server. Sie können alle DNS-Server oder Dienste, die Platzhalterzeichen ein Datensätzen unterstützen.
 * Computer ausgeführt Deis-Clienttools. Sie können einen lokalen oder einen virtuellen Computer verwenden. Führen Sie diese Tools auf fast jeder Distribution, aber gehen Ubuntu verwenden.

## <a name="provision-the-cluster"></a>Bestimmung des Clusters

In diesem Abschnitt verwenden Sie eine Vorlage [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) open-Source-Repository [Azure-Schnellstart-Vorlagen](https://github.com/Azure/azure-quickstart-templates). Zuerst kopieren Sie unten die Vorlage. Dann erstellen Sie ein neues SSH-Schlüsselpaar für die Authentifizierung. Und konfigurieren Sie dann eine neue ID für Sie Cluster. Und schließlich Sie verwenden das Shell-Skript oder das PowerShell-Skript Cluster bereitstellen.

1. Repository kopieren: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Wechseln Sie zu dem Vorlagenordner:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Erstellen Sie ein neues SSH-Schlüsselpaar ssh-Keygen verwenden:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Erstellen Sie ein Zertifikat mit dem privaten Schlüssel oben:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Gehen Sie zu [https://discovery.etcd.io/new](https://discovery.etcd.io/new) zu neuen Cluster Token sieht so aus:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Jeder CoreOS Cluster muss ein eindeutiges Token aus diesem kostenlosen Service. Weitere Informationen finden Sie in der [Dokumentation zu CoreOS](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Ändern Sie die **Cloud-config.yaml** Datei ersetzen vorhandene **Discovery** -Token mit dem neuen Token:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Ändern Sie die **Azuredeploy-parameters.json** -Datei: Öffnen Sie das Zertifikat in Schritt 4 in einem Text-Editor erstellt. Kopieren Sie Text zwischen `----BEGIN CERTIFICATE-----` und `-----END CERTIFICATE-----` in den **SshKeyData** -Parameter (Sie müssen alle Zeilenumbruchzeichen entfernen).

8. Ändern Sie den Parameter **NewStorageAccountName** . Dies ist das Speicherkonto für Betriebssystem virtuellen Festplatten. Dieser Kontoname muss eindeutig sein.

9. Ändern Sie den Parameter **PublicDomainName** . Dieser werden DNS-Name der Load-Balancer öffentliche IP-Adresse zugeordnet. Der endgültige FQDN haben das Format _[Wert dieses Parameters]_. _[Region]_. cloudapp.azure.com. Beispielsweise geben Sie den Namen wie deishbai32 und Ressourcengruppe in der Region West USA bereitgestellt wird, werden dann der letzte FQDN der Lastenausgleich deishbai32.westus.cloudapp.azure.com.

10. Speichern Sie die Datei. Und dann können Sie den Cluster mithilfe von Azure PowerShell bereitstellen:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  oder Azure CLI:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Nach der Bereitstellung der Ressourcengruppe sehen Sie alle Ressourcen in der Gruppe auf Azure-Verwaltungsportal. Wie im folgenden Screenshot gezeigt, enthält die Ressourcengruppe ein virtuelles Netzwerk mit drei VMs auf dieselben Verfügbarkeit verbunden sind. Die Gruppe enthält auch ein Lastenausgleich zugeordnete öffentliche IP-Adresse hat.

  ![Der bereitgestellte Ressourcengruppe Azure-Verwaltungsportal](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Installieren des Clients

Sie benötigen **Deisctl** Steuerelement Ihrem Cluster Deis. Obwohl Deisctl in allen Clusterknoten installiert ist, empfiehlt es eine Deisctl für einen administrativen Computer verwendet. Da alle Knoten mit privaten IP-Adressen konfiguriert sind, müssen Sie außerdem verwenden SSH tunneling über den Lastenausgleich über eine öffentliche IP-Adresse zum Knoten Computer herstellen. Es folgen die Schritte des Deisctl auf einem separaten Ubuntu physischen oder virtuellen Computer einrichten.

1. Installieren Deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Fügen Sie Ihr privater Schlüssel ssh Agent:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Konfigurieren Sie Deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Die Vorlage definiert eingehende NAT-Regeln, die 2223 zu 1 2224 Instanz 2 und 2225 Instanz 3 zugeordnet. Dies bietet Redundanz für Werkzeug Deisctl. Sie können diese Regeln auf Azure-Verwaltungsportal überprüfen:

![NAT-Regeln auf dem Lastenausgleich](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Die Vorlage unterstützt derzeit nur 3-Knoten-Cluster. Dies ist aufgrund einer Beschränkung in Azure Ressourcenmanager Vorlage NAT-Regel-Definition, die Schleife Syntax unterstützt.

## <a name="install-and-start-the-deis-platform"></a>Installieren und starten Sie die Plattform Deis

Sie können jetzt Deisctl installieren und Starten der Deis Plattform:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Starten der Plattform dauert (mehr als 10 Minuten). Starten des Generator-Dienstes kann insbesondere eine lange dauern. Manchmal dauert einige Versuche erfolgreich: Wenn der Vorgang zu hängen scheint, versuchen Sie, `ctrl+c` Ausführung des Befehls und wiederholen.

Verwenden Sie `deisctl list` überprüfen, ob alle Dienste ausgeführt werden:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Herzlichen Glückwunsch! Jetzt haben Sie eine Deis Clsuter auf Azure! Als Nächstes sehen ein Beispiel gehen Anwendung Cluster in Aktion bereitstellen.

## <a name="deploy-and-scale-a-hello-world-application"></a>Bereitstellen und eine Hello World-Anwendung skalieren

Die folgenden Schritte zeigen zum Bereitstellen von "Hello World" Anwendung auf dem Cluster wechseln. Die Schritte basieren auf [Deis Dokumentation](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Für das routing Netz ordnungsgemäß funktioniert müssen Sie einen Platzhalter ein Datensatz für die Domäne auf die öffentliche IP-Adresse des Lastenausgleich. Der folgende Screenshot zeigt ein Datensatz für einen Beispiel-Domain-Registrierung auf GoDaddy:

    ![GoDaddy-A-Datensatz](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Installieren deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Erstellen Sie einen neuen SSH-Schlüssel, und fügen Sie den öffentlichen Schlüssel auf GitHub (natürlich, Sie können auch wiederverwenden vorhandener Schlüssel). Erstellen Sie ein neues SSH-Schlüsselpaar verwenden:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Fügen Sie GitHub id_rsa.pub oder den öffentlichen Schlüssel Ihrer Wahl zu hinzu. Sie können dazu die hinzufügen SSH-Taste im Bildschirm Konfiguration SSH-Schlüssel:

  ![Github Schlüssel](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Registrieren eines neuen Benutzers:

        deis register http://deis.[your domain]
<p />
6. Fügen Sie SSH-Schlüssel:

        deis keys:add [path to your SSH public key]
  <p />      
7. Erstellen einer Anwendung.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Git Push löst Andockfenster Bilder erstellt und bereitgestellt, die einige Minuten dauern. Aus meiner Erfahrung reagiert, Schritt 10 (Push Bild privaten Repository). Geschieht dies, können Sie den Prozess beenden, Entfernen der Anwendung "apps deis: destroy – <application name> ` to remove the application and try again. You can use `deis Apps:list" auf den Namen der Anwendung. Wenn alles funktioniert, sollte am Ende des Befehl Folgendes angezeigt werden:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Überprüfen Sie, ob die Anwendung funktioniert:

        curl -S http://[your application name].[your domain]
  Sollte angezeigt werden:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Skalieren der Anwendung 3 Instanzen:

        deis scale cmd=3
<p />
11. Optional können Sie deis Informationen, um Ihre Anwendung zu überprüfen. Ausgänge aus der Bereitstellung einer Anwendung:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden die Schritte zum Bereitstellen einer neuen Deis Cluster Azure mit einer Azure-Ressourcen-Manager-Vorlage. Die Vorlage unterstützt Redundanz Werkzeuge sowie Lastenausgleich für bereitgestellte Anwendung. Die Vorlage vermeidet auch öffentliche IP-Adressen auf Memberknoten spart wertvolle öffentliche IP-Ressourcen und mehr sichere Umgebung Host-Anwendung. Weitere finden Sie in folgenden Artikeln:

[Azure-Ressourcen-Manager (Übersicht)] [resource-group-overview]  
[Verwendung der Azure-CLI] [azure-command-line-tools]  
[Mithilfe von Azure PowerShell mit Azure Resource Manager] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
