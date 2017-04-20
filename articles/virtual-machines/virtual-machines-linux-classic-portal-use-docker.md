<properties
    pageTitle="Mit Andockfenster VM-Erweiterung für Linux | Microsoft Azure"
    description="Beschreibt Andockfenster und Azure Virtual Machines Extensions und Azure Virtual Machines erstellen Andockfenster Hosts mit der Azure-CLI im klassischen Bereitstellungsmodell."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Andockfenster VM-Erweiterung verwenden mit klassischen Azure-portal

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Andockfenster](https://www.docker.com/) ist eines der beliebtesten Ansätze bei der Speichervirtualisierung, die [Linux-Container](http://en.wikipedia.org/wiki/LXC) anstelle von virtuellen Maschinen zu isolieren von Daten und Arbeiten mit freigegebenen Ressourcen verwendet. Verwenden von [Azure Linux-Agent] verwaltet Andockfenster VM-Erweiterung Andockfenster VM erstellen, die eine beliebige Anzahl von Containern für die Anwendung in Azure gehostet.

> [AZURE.NOTE] Dieses Thema beschreibt die Andockfenster VM von klassischen Azure-Portal erstellen. Andockfenster VM in der Befehlszeile erstellen, finden Sie unter [Verwendung der Andockfenster VM Erweiterung von der Azure-Befehlszeilenschnittstelle (CLI Azure)]. Eine allgemeine Erläuterung der Container und deren Vorteile finden Sie unter [Andockfenster hoher Ebene Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Erstellen einer neuen VM aus der Bildergalerie
Zunächst muss ein Azure VM ein Linux-Image, das die Andockfenster VM-Erweiterung Ubuntu 14.04 LTS Bild verwenden die Bildergalerie Server Beispielbild und Ubuntu 14.04 Desktop als Client unterstützt. Klicken Sie im Portal **+ Neuer** in der unteren linken Ecke eine neue VM-Instanz erstellen und wählen Sie ein Ubuntu 14.04 LTS Bild von den verfügbaren Auswahlmöglichkeiten oder vollständige Bildergalerie wie unten dargestellt.

> [AZURE.NOTE] Derzeit unterstützt nur Ubuntu 14.04 LTS Bilder als Juli 2014 Andockfenster VM-Erweiterung.

![Neues Ubuntu-Image erstellen](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Andockfenster Zertifikate erstellen

Nach dem Erstellen die VM daß Andockfenster auf dem Clientcomputer installiert ist. (Weitere Informationen siehe [Installationshinweise Andockfenster](https://docs.docker.com/installation/#installation).)

Andockfenster Kommunikation nach [Ausführung Andockfenster mit Https] Zertifikat und Schlüssel-Dateien erstellen und in der **`~/.docker`** Verzeichnis auf dem Clientcomputer.

> [AZURE.NOTE] Andockfenster VM-Erweiterung im Portal benötigt gegenwärtig Anmeldeinformationen base64-codiert.

Verwenden Sie in der Befehlszeile **`base64`** oder einer anderen bevorzugten Codierung base64-codierte Themen erstellen. Mit einem einfachen Satz von Zertifikat und Schlüssel Dateien könnte wie folgt aussehen:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Andockfenster VM-Erweiterung hinzufügen
Um die Andockfenster VM Erweiterung VM-Instanz erstellten Scrollen Sie **Extensions** und auf es VM-Erweiterungen bringen wie unten dargestellt.
> [AZURE.NOTE] Diese Funktionalität wird im vorschauportal: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Hinzufügen einer Erweiterung
Klicken Sie auf das **+ Hinzufügen** möglich VM-Erweiterungen Anzeigen dieser VM hinzugefügt werden können.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Wählen Sie die Andockfenster VM-Erweiterung
Wählen Sie die Andockfenster Beschreibung und wichtige bringt Andockfenster-Erweiterung VM aus und dann auf **Erstellen** unten, um die Installation zu beginnen.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Fügen Sie das Zertifikat und die Schlüsseldateien:

Geben Sie in die Formularfelder base64-codierte Versionen von CA-Zertifikat, das Serverzertifikat und Ihre Serverschlüssel wie in der folgenden Abbildung dargestellt.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Beachten Sie, dass (wie in der vorhergehenden Abbildung) das 2376 standardmäßig ausgefüllt. Jeder Endpunkt eingeben, aber im nächste Schritt werden, um den entsprechenden Endpunkt zu öffnen. Wenn Sie die Standardeinstellung ändern, müssen Sie den entsprechenden Endpunkt im nächsten Schritt öffnen.

## <a name="add-the-docker-communication-endpoint"></a>Andockfenster Kommunikationsendpunkt hinzufügen
Beim Anzeigen der Ressourcengruppe erstellten wählen Sie VM zugeordneten Netzwerk-Sicherheitsgruppe aus und auf **Sicherheitsregeln eingehende** Regeln anzeigen, wie hier gezeigt.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Im Standardfall, geben Sie einen Namen für den Endpunkt (in diesem Beispiel **Andockfenster**) und 2376 "Ziel-Port-Bereich" auf **+ Hinzufügen** weitere Regel hinzugefügt werden Eingestellt Protokoll **TCP**anzeigen, und klicken Sie auf **OK** , um die Regel zu erstellen.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Testen Sie Ihre Andockfenster Client und Azure Andockfenster Host
Suchen und kopieren die VM Domäne und in der Befehlszeile des Clientcomputers, Typ `docker --tls -H tcp://` *Dockerextension* `.cloudapp.net:2376 info` (wobei *Dockerextension* wird ersetzt durch die Domäne für die VM).

Das Ergebnis sollte wie folgt aussehen:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Die oben aufgeführten Schritte haben Sie jetzt einen voll funktionsfähigen Andockfenster Host auf eine Azure VM konfiguriert Remote von anderen Clients verbunden werden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Sie können [Andockfenster Benutzerhandbuch] und die Andockfenster VM verwenden. Wenn Sie erstellen Andockfenster Hosts Azure VMs über Befehlszeilenschnittstelle automatisieren möchten, finden Sie unter [der Andockfenster VM-Erweiterung von der Azure-Befehlszeilenschnittstelle (CLI Azure)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Verwendung der Andockfenster VM Erweiterung von der Azure-Befehlszeilenschnittstelle (CLI Azure)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux-Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Andockfenster mit Https ausgeführt]: http://docs.docker.com/articles/https/
[Andockfenster Benutzerhandbuch]: https://docs.docker.com/userguide/
