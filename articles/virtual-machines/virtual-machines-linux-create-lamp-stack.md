<properties
    pageTitle="Bereitstellen der LEUCHTE auf einer virtuellen Linux-Maschine | Microsoft Azure"
    description="Informationen Sie zum Stapel LEUCHTE auf Linux VM installieren"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>LAMP-Stack auf Azure bereitgestellt
Dieser Artikel führt Sie durch einen Apache Webserver MySQL und PHP (LAMPE Stack) auf Azure bereitstellen. Sie benötigen ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)) und [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) , die [Ihre Azure-Konto verbunden](../xplat-cli-connect.md)ist.

Es gibt zwei Methoden zum Installieren von LAMPE in diesem Artikel behandelt:

## <a name="quick-command-summary"></a>Schnelle Befehl Zusammenfassung

1) Bereitstellen Sie auf neuen VM

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Bereitstellen Sie auf vorhandenen VM

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Bereitstellen Sie auf neue VM Exemplarische Vorgehensweise

Sie können eine neue [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) , die die VM enthält starten:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Um die VM selbst erstellen, können Sie eine bereits Azure Ressourcenmanager Vorlage [auf GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Einige weitere Eingaben aufgefordert Antwort sollte angezeigt werden:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Sie haben jetzt eine Linux VM mit bereits installiert ist. Auf Wunsch können Sie die Installation überprüfen, springen auf [überprüfen LAMPE installiert].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Bereitstellen Sie auf vorhandenen VM Exemplarische Vorgehensweise

Benötigen Sie Hilfe zum Erstellen einer Linux VM kann head [Hier erfahren, wie Linux VM erstellt (. / virtual-machines-linux-quick-create-cli.md). Als Nächstes müssen Sie SSH in Linux VM. Benötigen Sie Hilfe zum Erstellen von SSH-Schlüssel können Sie den Kopf [Hier erfahren, wie Linux-Mac SSH-Schlüssel erstellen (. / virtual-machines-linux-mac-create-ssh-keys.md).
Wenn bereits einen SSH-Schlüssel ins voraus und SSH Linux VM mit `ssh username@uniqueDNS`.

Da im Linux VM Arbeit gehen wir durch die Installation des LEUCHTE Stapels auf Debian-basierten Distributionen. Welche Befehle können für andere Linux-Distributionen unterscheiden.

#### <a name="installing-on-debianubuntu"></a>Installieren auf Debian-Ubuntu

Sie benötigen die folgenden Pakete installiert: `apache2`, `mysql-server`, `php5`, und `php5-mysql`. Sie können diese direkt greifen diese Pakete oder Tasksel verwenden. Anleitung für beide Optionen sind unten aufgeführt.
Vor der Installation müssen Sie downloaden und Aktualisieren der Paketliste.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Einzelne Pakete
Apt-Get verwenden:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel verwenden
Alternativ können Sie Tasksel Debian-Ubuntu-Tool, die mehrere verwandte Pakete als koordinierte "Aufgabe" auf Ihrem System installiert.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Nach dem Ausführen einer der oben genannten Optionen werden Sie aufgefordert, diese Pakete und zahlreiche andere installieren. Drücken Sie "y" und "Enter" und folgen alle anderen ein Administratorkennwort für MySQL fest. Mindestens erforderliche PHP Extensions MySQL PHP mit benötigt wird installiert. 

![][1]

Führen Sie den folgenden Befehl an anderen PHP-Erweiterungen, die als Pakete verfügbar sind:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php Dokument erstellen

Sie sollte jetzt überprüfen, welche Version von Apache und MySQL, PHP über die Befehlszeile durch Eingabe haben `apache2 -v`, `mysql -v`, oder `php -v`.

Wenn Sie weiter testen möchten, können Sie eine quick Info PHP-Seite in einem Browser anzeigen erstellen. Erstellen Sie eine neue Datei mit Text-Editor mit diesem Befehl:

    user@ubuntu$ sudo nano /var/www/html/info.php

Texteditor GNU Nano fügen Sie folgende Zeilen hinzu:

    <?php
    phpinfo();
    ?>

Speichern Sie und beenden Sie den Texteditor.

Starten Sie mit diesem Befehl Apache, damit alle neue Installationen wirksam werden.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Überprüfen der LEUCHTE installiert

Nachdem Sie PHP-Informationsseite gerade erstellten im Browser einchecken können zu http://youruniqueDNS/info.php, sollte wie folgt aussehen.

![][2]

Apache2 Ubuntu-Standardseite auf Sie http://youruniqueDNS/ anzeigen können Sie die Apache-Installation überprüfen. Folgendes sollte angezeigt werden.

![][3]

Herzlichen Glückwunsch, Sie haben nur einen Stapel LEUCHTE auf Azure-VM!

## <a name="next-steps"></a>Nächste Schritte

Ubuntu-Dokumentation im Stapel LAMPE Auschecken:

- [https://help.Ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
