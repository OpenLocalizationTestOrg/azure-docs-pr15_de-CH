<properties
   pageTitle="Andockfenster und verfassen auf einem virtuellen Computer | Microsoft Azure"
   description="Kurze Einführung in das Arbeiten mit verfassen und Andockfenster auf virtuelle Linux-Computer in Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Erste Schritte mit Andockfenster verfassen definieren und Ausführen einer Anwendung mehrere Container auf einem virtuellen Computer Azure

Erste Schritte mit Andockfenster [Verfassen](http://github.com/docker/compose) definieren und eine komplexe Anwendung auf einer virtuellen Linux-Maschine in Azure ausgeführt. Bei erstellen verwenden Sie eine einfache Textdatei definiert eine Anwendung mehrere Andockfenster Container aus. Sie dreht sich die Anwendung in einem einzigen Befehl, der alle definierten Umgebung bereitstellen. 

Beispielsweise zeigt dieser Artikel WordPress-Blog mit Backend-MariaDB SQL-Datenbank auf einem Ubuntu VM schnell einrichten. Verfassen können Sie komplexere Anwendung einrichten.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Schritt 1: Einrichten einer Linux VM als Host Andockfenster

Können verschiedene Azure Verfahren und Bilder oder Ressourcen-Manager Vorlagen in Azure Marketplace zu Linux VM als Andockfenster Host eingerichtet. Beispielsweise finden Sie schnell eine Ubuntu VM Azure Andockfenster VM-Erweiterung mithilfe von [Schnellstart-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)erstellen [der Andockfenster VM Erweiterung Ihrer Umgebung bereitstellen](virtual-machines-linux-dockerextension.md) . 

Bei Verwendung die Andockfenster VM-Erweiterung VM wird automatisch als Andockfenster Host eingerichtet und verfassen ist bereits installiert. Das Beispiel in diesem Artikel veranschaulicht der [für Mac, Linux und Windows Azure Befehlszeilenschnittstelle](../xplat-cli-install.md) (CLI Azure) im Ressourcen-Manager-Modus die VM erstellt.

Grundkenntnisse des vorherigen Dokuments erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `West US` Speicherort und eine VM Azure Andockfenster VM-Erweiterung installiert:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Schritt 2: Überprüfen Sie, ob verfassen installiert ist

Nach Abschluss die Bereitstellung SSH auf den neuen Andockfenster mit dem DNS-Namen während der Bereitstellung bereitgestellt. Sie können `azure vm show -g myDockerResourceGroup -n myDockerVM` Details Ihrer VM, den DNS-Namen an.

Überprüfen verfassen auf dem virtuellen Computer installiert ist, führen Sie den folgenden Befehl ein:

```bash
docker-compose --version
```

Sie sehen eine Ausgabe ähnlich `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Bei Verwendung einer anderen Methode zu einem Host Andockfenster erstellen selbst installieren müssen, finden Sie in der [Dokumentation verfassen](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Schritt 3: Erstellen einer Konfigurationsdatei Andockfenster compose.yml

Als Nächstes erstellen Sie ein `docker-compose.yml` -Datei nur eine Konfiguration Textdatei, definieren die Andockfenster Container auf dem virtuellen Computer ausgeführt. Die Datei gibt das Bild auf jeder Container (oder möglicherweise ein Build von einem Dockerfile) erforderlichen Umgebungsvariablen und Abhängigkeiten, Ports und die Links zwischen Containern. Ausführliche Syntax von Yml finden Sie unter [Referenz erstellen](http://docs.docker.com/compose/yml/).

Erstellen der `docker-compose.yml` folgendermaßen:

```bash
touch docker-compose.yml
```

Verwenden Sie einen Texteditor, einige Daten zu der Datei hinzufügen. Im folgenden Beispiel wird die `vi` Editor:

```bash
vi docker-compose.yml
```

Im folgenden Beispiel wird in der Textdatei einfügen. Diese Konfiguration verwendet Bilder aus der [Registrierung DockerHub](https://registry.hub.docker.com/_/wordpress/) WordPress (open-Source Blogs und Content Managementsystem) und verknüpften Backend-MariaDB SQL-Datenbank installieren. Geben Sie Ihre eigenen `MYSQL_ROOT_PASSWORD` wie folgt:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Schritt 4: Starten Sie die Container mit verfassen

In demselben Verzeichnis wie Ihre `docker-compose.yml` Datei, führen Sie folgenden Befehl (je nach Ihrer Umgebung müssen Sie möglicherweise ausführen `docker-compose` mit `sudo`.):

```bash
docker-compose up -d

```

Dieser Befehl startet die Andockfenster Container gemäß `docker-compose.yml`. Es dauert ein oder zwei Minuten für diesen Schritt abschließen. Ausgabe wie im folgenden Beispiel angezeigt:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Werden Sie einschalten die Option **-d** verwenden, damit die Container kontinuierlich im Hintergrund ausgeführt.

Um sicherzustellen, dass die Container sind, geben Sie `docker-compose ps`. Sie sollten Folgendes sehen:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Sie können jetzt WordPress direkt auf die VM auf Port 80 herstellen. Öffnen Sie einen Webbrowser und geben Sie den DNS-Namen Ihrer VM (z. B. `http://myresourcegroup.westus.cloudapp.azure.com`). WordPress sollte nun angezeigt werden, führen Sie die Installation und erste Schritte mit der Anwendung Startbildschirm.

![WordPress-Startseite][wordpress_start]


## <a name="next-steps"></a>Nächste Schritte

* [Andockfenster VM Erweiterung Benutzerhandbuch](https://github.com/Azure/azure-docker-extension/blob/master/README.md) Weitere Optionen Andockfenster und Erstellen der Andockfenster VM konfiguriert werden. Beispielsweise ist eine Option der verfassen Yml Datei (umgerechnet in JSON) direkt in der Konfiguration der Andockfenster VM-Erweiterung.
* Im [Benutzerhandbuch](http://docs.docker.com/compose/) Weitere Beispiele für das Erstellen und Bereitstellen von Multi-Container apps [Befehlszeilenreferenz verfassen](http://docs.docker.com/compose/reference/) und Auschecken.
* Verwenden einer Vorlage Azure-Ressourcen-Manager entweder Ihre eigenen oder eine dazu aus der [Gemeinschaft](https://azure.microsoft.com/documentation/templates/)eine Azure VM mit Andockfenster und einer Anwendung mit verfassen bereitstellen. Beispielsweise die Vorlage [Bereitstellen WordPress-Blog mit Andockfenster](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) Andockfenster und verfassen WordPress mit MySQL Backend auf ein Ubuntu VM schnell bereitstellen.
* Versuchen Sie, einen [Schwarm Andockfenster](virtual-machines-linux-docker-swarm.md) Cluster integrieren Andockfenster erstellen. Szenarien finden Sie unter [Verwenden mit Schwarm verfassen](https://docs.docker.com/compose/swarm/) .

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
