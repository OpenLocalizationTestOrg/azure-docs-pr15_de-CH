<properties
    pageTitle="Andockfenster VM-Erweiterung verwenden für Linux auf Azure"
    description="Andockfenster und Azure Virtual Machines Extensions beschreibt und veranschaulicht, wie programmgesteuert auf Azure Andockfenster Hosts über die Befehlszeile mithilfe der Azure-CLI sind virtuelle Computer erstellen."
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
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Mit der Erweiterung Andockfenster VM aus der Befehlszeile Azure Schnittstelle (Azure CLI)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



Dieses Thema beschreibt das Erstellen einer VM Andockfenster VM-Erweiterung vom Modus Management (Asm) in Azure CLI auf allen Plattformen. [Andockfenster](https://www.docker.com/) ist eines der beliebtesten Ansätze bei der Speichervirtualisierung, die [Linux-Container](http://en.wikipedia.org/wiki/LXC) anstelle von virtuellen Maschinen zu isolieren von Daten und Arbeiten mit freigegebenen Ressourcen verwendet. Sie können Andockfenster VM-Erweiterung und [Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md) Andockfenster VM erstellen, die eine beliebige Anzahl von Containern für die Anwendung in Azure hostet. Eine allgemeine Erläuterung der Container und deren Vorteile finden Sie unter [Andockfenster hoher Ebene Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Verwendung der Andockfenster VM-Erweiterung mit Azure
Um die Andockfenster VM-Erweiterung mit Azure verwenden, installieren Sie eine Version der [Azure-Befehlszeilen-Schnittstelle](https://github.com/Azure/azure-sdk-tools-xplat) (CLI Azure) höher als 0.8.6 (wie dieser Artikel die aktuelle Version 0.10.0). Sie können die Azure-CLI für Mac, Linux und Windows.


Vorgang mit Andockfenster Azure ist einfach:

+ Installieren der Azure-CLI und die zugehörigen Dateien auf dem Computer Steuerelement Azure soll (unter Windows ist das Linux-Distribution als virtueller Computer ausgeführt)
+ Verwenden Sie die Befehle Azure CLI Andockfenster VM Andockfenster Host in Azure erstellen
+ Verwenden Sie lokale Andockfenster Befehle zum Verwalten der Andockfenster Container in Ihrer Andockfenster VM in Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Installieren die Azure Befehlszeilenschnittstelle (CLI Azure)

Installieren und Konfigurieren der Azure-CLI, finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md). Um die Installation zu bestätigen, geben Sie `azure` in der Befehlszeile und nach kurzer Zeit die Azure-CLI ASCII-Grafik erscheint enthält die grundlegenden Befehlen zur Verfügung. Wenn die Installation ordnungsgemäß funktioniert, Sie sollen geben `azure help vm` und sehen, dass eines der aufgeführten Befehle "Andockfenster".

> [AZURE.NOTE] Andockfenster verfügt über Tools für Windows [Andockfenster Computer](https://docs.docker.com/installation/windows/), die Sie auch verwenden, um die Erstellung eines Clients Andockfenster automatisieren, mit denen Sie Azure VMs als Andockfenster Hosts arbeiten.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Verbinden der Azure CLI Azure-Konto
Vor der Verwendung der Azure-CLI müssen Sie Ihre Azure Anmeldeinformationen Azure-CLI auf Ihrer Plattform zuordnen. Abschnitt [der Azure-Abonnement Verbindung](../xplat-cli-connect.md) erläutert herunterladen und importieren die Datei **"publishsettings"** oder der Azure-CLI eine Organisations-Id zuordnen.

> [AZURE.NOTE] Es gibt einige Unterschiede im Verhalten bei Verwendung eine oder die andere Methoden der Authentifizierung so lesen Sie das Dokument über die verschiedenen Funktionen.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Installieren Sie Andockfenster und die Andockfenster VM-Erweiterung für Azure
Befolgen Sie die [Installationshinweise Andockfenster](https://docs.docker.com/installation/#installation) Andockfenster lokal auf Ihrem Computer installieren.

Um Andockfenster mit einer Azure Virtual Machine verwenden, muss das Bild Linux für die VM [Azure Linux VM-Agent](virtual-machines-linux-agent-user-guide.md) installiert. Derzeit gibt es nur zwei Arten von Bildern, die dies ermöglichen:

+ Ubuntu-Image aus dem Azure-Bildergalerie oder

+ Eine benutzerdefinierte Linux-Abbild mit dem Azure Linux VM-Agent erstellt installiert und konfiguriert. Weitere Informationen zum Erstellen einer benutzerdefinierten Linux VM mit Azure VM-Agent finden Sie unter [Azure Linux VM-Agent](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Mithilfe der Azure-Bildergalerie

Verwenden Sie einen Bash oder Terminalserver-Sitzung folgenden Azure CLI-Befehl Suchen des letzten Ubuntu Bilds in der VM-Galerie mit eingeben

`azure vm image list | grep Ubuntu-14_04`

und wählen Sie einen Image-Namen wie `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, und verwenden Sie den folgenden Befehl, um einen neuen virtuellen Computer mit diesem Bild erstellen.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Wo:

+ * &lt;Name des Vm-Cloudservice&gt; * ist der Name der VM Andockfenster Container Hostcomputer in Azure werden

+  * &lt;Benutzername&gt; * ist der Benutzername des Standardbenutzers Stamm der VM

+ * &lt;Kennwort&gt; * das Kennwort des Kontos *Username* , die die Komplexität für Azure erfüllt

> [AZURE.NOTE] Derzeit ein Kennwort muss mindestens 8 Zeichen, einem Kleinbuchstaben und einem Großbuchstaben, eine Zahl und ein Sonderzeichen wie die folgenden Zeichen enthalten: `!@#$%^&+=`. Nein, ist der Punkt am Ende des vorhergehenden Satzes keine Sonderzeichen.

Wenn der Befehl erfolgreich ausgeführt wurde, erhalten Sie etwa folgenden präzise Argumente und Optionen, die Sie verwendet:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Erstellen eines virtuellen Computers kann einige Minuten dauern, aber wurde nach bereitgestellt wurden (der Wert ist `ReadyRole`) Andockfenster Daemon (Andockfenster Service) startet und Andockfenster containerhost herstellen.

Geben Sie zum Testen der Andockfenster VM in Azure erstellt haben

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

wo * &lt;Vm-Name--Verwendung&gt; * ist der Name des virtuellen Computers, die beim Aufruf verwendet `azure vm docker create`. Sie sollten sehen ähnlich der folgenden gibt an, dass die VM Andockfenster Host ist in Azure ausgeführt und Warten auf Ihre Befehle. 

Jetzt können Sie mit Ihrem Client Andockfenster Informationen zu versuchen (in manchen Systemen Andockfenster Client wie auf Mac kann Ihnen `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Um sicherzustellen, dass alle arbeitet, können die VM für die Erweiterung Andockfenster überprüfen:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Andockfenster Hostauthentifizierung VM

Neben der VM Andockfenster der `azure vm docker create` Befehl erstellt automatisch die erforderlichen Zertifikate zum Andockfenster Client Azure containerhost mit HTTPS herstellen kann und Zertifikate auf den Computern Client und Host gespeichert. Bei nachfolgenden Versuchen sind die vorhandenen Zertifikate wiederverwendet und gemeinsam mit dem neuen Host.

Befinden sich Zertifikate `~/.docker`, und Andockfenster auf Port **2376**konfiguriert. Wenn Sie einen anderen Anschluss oder Verzeichnis verwenden möchten, können eine der folgenden `azure vm docker create` Befehlszeilenoptionen konfigurieren Andockfenster Containers Hosten virtueller Computer mit einem anderen Port oder andere Zertifikate für Clients verbinden:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Andockfenster Daemon auf dem Host konfiguriert überwachen und Clientverbindungen auf den angegebenen Port mit dem Zertifikate authentifiziert die `azure vm docker create` Befehl. Der Clientcomputer müssen diese Zertifikate auf dem Host Andockfenster zugreifen.

> [AZURE.NOTE] Eine Ausführung ohne diese Zertifikate Netzwerkhost werden jeder mit dem Computer herstellen können. Bevor Sie die Konfiguration ändern, sicherstellen Sie, dass die Risiken für Computer und Anwendung verstehen.

## <a name="next-steps"></a>Nächste Schritte

* Sie können [Andockfenster Benutzerhandbuch] und die Andockfenster VM verwenden. Zum Erstellen einer VM Andockfenster aktiviert in das neue Portal finden Sie unter [der Andockfenster VM-Erweiterung mit dem Portal].

* Azure Andockfenster VM-Erweiterung unterstützt auch Andockfenster erstellen eine deklarative YAML-Datei eine Anwendung Entwickler modelliert in jeder Umgebung und generieren eine konsistente Bereitstellung verwendet. Siehe [Verfassen definieren und Ausführen einer Anwendung mehrere Container Azure VM mit Andockfenster beginnen].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Verwendung der Andockfenster VM-Erweiterung mit dem Portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Andockfenster Benutzerhandbuch]: https://docs.docker.com/userguide/
 
[Erste Schritte mit Andockfenster verfassen definieren und Ausführen einer Anwendung mehrere Container auf einem virtuellen Computer Azure]:virtual-machines-linux-docker-compose-quickstart.md