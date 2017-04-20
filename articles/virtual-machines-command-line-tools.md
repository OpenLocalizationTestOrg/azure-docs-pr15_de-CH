<properties
    pageTitle="Azure CLI-Befehle im Modus Service Management | Microsoft Azure"
    description="Azure-Befehlszeilenschnittstelle (CLI) Befehle im Service-Modus zum Verwalten von Bereitstellungen im klassischen Bereitstellungsmodell"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure CLI-Befehle in Azure Service Management (Asm) Modus

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Sie können auch [alle Ressourcen-Manager-Modell Befehle erfahren](virtual-machines/azure-cli-arm-commands.md), und die CLI [Ressourcen](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) Migrieren von Classic Ressourcen-Manager-Modell.

Dieser Artikel enthält die Syntax und Optionen für Azure-CLI-Befehle, die Sie häufig zum Erstellen und Verwalten von Azure Ressourcen im klassischen Bereitstellungsmodell verwenden. Sie können auf diese Befehle zugreifen, von der CLI in Azure Service Management (Asm) Modus ausgeführt. Dies ist keine vollständige Referenz und CLI-Version kann leicht unterschiedliche Befehle oder Parameter angezeigt. 

Um zu beginnen, [der Azure-CLI installieren](xplat-cli-install.md) und [Ihr Azure-Abonnement an](xplat-cli-connect.md).

Geben Sie aktuelle Befehlssyntax und Optionen in der Befehlszeile `azure help` oder Hilfe für einen bestimmten Befehl an `azure help [command]`. Finden Sie auch CLI Beispiele in der Dokumentation zum Erstellen und Verwalten von bestimmten Azure Services.

Optionale Parameter werden in Klammern angezeigt (z. B. `[parameter]`). Alle Parameter sind erforderlich.

Neben Befehl Optionaler Parameter dokumentiert sind drei optionale Parameter Anforderung Optionen wie Statuscodes ausführliche Ausgabe verwendet werden können. Die `-v` Parameter enthält ausführliche Ausgabe und `-vv` Parameter bietet noch ausführlichen Ausgabe. Die `--json` Option gibt das Ergebnis im raw Json-Format.

## <a name="setting-asm-mode"></a>Asm Modus

Verwenden Sie den folgenden Befehl Befehle Azure CLI Service Management-Modus aktivieren.

    azure config mode asm

>[AZURE.NOTE] Der CLI Azure Ressourcenmanager und Azure Service Management-Modus schließen sich gegenseitig aus. Ressourcen in einem Modus können also von anderen Modus verwaltet werden.

## <a name="manage-your-account-information-and-publish-settings"></a>Kontoinformationen verwalten und Veröffentlichen von Standardeinstellungen
Eine Möglichkeit die CLI eine Verbindung zu Ihrem Konto ist in Azure Abonnementinformationen. (Siehe [mit Azure-Abonnement von Azure-CLI](xplat-cli-connect.md) für weitere Optionen.) Diese Informationen können von klassischen Azure-Portal in einer Initialisierungsdatei veröffentlichen, wie hier beschrieben. Datei veröffentlichen können eine persistente lokale Konfiguration festlegen, dass die CLI für nachfolgende Operationen verwendet werden. Sie importieren müssen die Einstellungen einmal zu veröffentlichen.

**Konto herunterladen [Optionen]**

Dieser Befehl startet einen Browser, um die Datei "publishsettings" klassischen Azure-Portal herunterladen.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**Konto importieren [Optionen] &lt;Datei >**


Dieser Befehl importiert eine Publishsettings-Datei oder ein Zertifikat, dass sie mit dem Tool in zukünftigen Sitzungen verwendet werden kann.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Publishsettings-Datei kann Informationen (Namen und ID) mehrere Abonnements enthalten. Wenn Sie Publishsettings-Datei importieren, wird das erste Abonnement als die Beschreibung verwendet. Um ein anderes Abonnement verwenden, führen Sie den folgenden Befehl:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**Konto deaktivieren [Optionen]**

Dieser Befehl entfernt die gespeicherte Publishsettings, die importiert wurden. Verwenden Sie diesen Befehl, wenn Sie das Tool auf diesem Computer verwenden und sicherstellen möchten Sie, dass das Tool mit Ihrem Konto in Zukunft Sessions verwendet werden kann.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**Liste [Optionen]**

Liste der importierten Abonnements

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**Konto festlegen [Optionen] &lt;Abonnement&gt;**

Legen Sie das aktuelle Abonnement

###<a name="commands-to-manage-your-affinity-groups"></a>Befehle zum Verwalten der Gruppen

**Gruppe Liste [Optionen]**

Dieser Befehl zeigt die Azure Gruppen.

Gruppen können festgelegt werden, wenn eine Gruppe von virtuellen Computern mehrere physische Computer umfasst. Die Gruppe gibt an, dass die physischen Computer wie nahe wie möglich an die Netzwerkwartezeit zu reduzieren sollten.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**Affinität Kontogruppe erstellen [Optionen] &lt;Namen&gt;**

Dieser Befehl erstellt eine Gruppe

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**Affinität Kontogruppe anzeigen [Optionen] &lt;Namen&gt;**

Dieser Befehl zeigt die Details der Gruppe

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**Konto-Gruppe löschen [Optionen] &lt;Namen&gt;**

Dieser Befehl löscht eine Affinität Gruppe

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Befehle zur Verwaltung der Umgebung Konto

**Env Liste [Optionen]**

Liste der Konto-Umgebung

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**Konto Env anzeigen [Optionen] [Umgebung]**

Anzeigen von Kontodetails Umgebung

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**Konto Env hinzufügen [Optionen] [Umgebung]**

Dieser Befehl fügt eine Umgebung für das Konto

**Konto Env festlegen [Optionen] [Umgebung]**

Dieser Befehl legt die Konto-Umgebung

**Konto Env löschen [Optionen] [Umgebung]**

Dieser Befehl löscht die angegebene Umgebung des Kontos

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Befehle zum Verwalten der klassischen virtueller Computer
Das folgende Diagramm zeigt, wie klassische Azure virtuellen Computer in dieser bereitstellungsumgebung der Azure-Cloud-Dienst gehostet werden.

![Technisches Diagramm zu Azure](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Erstellen neuer** erstellt das Laufwerk im BLOB-Speicher (d. h. e:\ im Diagramm); **Anfügen** Fügt einen Datenträger bereits erstellten, aber zu einer virtuellen Maschine.

**Erstellen virtueller Computer [Optionen] &lt;DNS-Name > &lt;Bild > &lt;UserName > [Kennwort]**

Dieser Befehl erstellt eine Azure Virtual Machine. Standardmäßig wird jeder virtuellen Maschine (Vm) im Clouddienst erstellt. Sie können angeben, dass ein virtueller Computer einen vorhandenen Cloud-Dienst mithilfe der Option - C hinzugefügt werden soll, wie hier beschrieben.

Die Vm Befehl klassischen Azure-Portal erstellen, erstellt nur virtuelle Computer in dieser bereitstellungsumgebung. Es gibt keine Option zum Erstellen eines virtuellen Computers in die Stagingumgebung Bereitstellung eines Cloud-Diensts. Wenn Ihr Abonnement kein Azure Storage-Konto haben, erstellt der Befehl eine.

Geben Sie einen Speicherort-Speicherort-Parameter oder eine Gruppe durch-Parameter Affinity Group. Wenn keines angegeben ist, werden Sie aufgefordert, aus einer Liste der gültigen Speicherorte angeben.

Das Kennwort muss 8 123 Zeichen und erfüllt die Anforderung des Betriebssystems für diesen virtuellen Computer verwenden.

Wenn Sie SSH zu einem bereitgestellten virtuellen Linux-Maschine (wie üblich) verwenden möchten, müssen Sie SSH über die Option-e aktivieren, beim Erstellen des virtuellen Computers. Es ist nicht möglich, SSH aktivieren, nachdem der virtuelle Computer erstellt wurde.

Virtuelle Windows-Maschinen können später aktivieren RDP Port 3389 als Endpunkt hinzufügen.

Für diesen Befehl werden die folgenden optionalen Parameter unterstützt:

**- C-Verbindung** virtuellen Computer in einer bereits erstellten Bereitstellung in einem Hostingdienst erstellen. Vmname - diese Option nicht verwendet wird, wird der Name des neuen virtuellen Computers automatisch generiert.<br />
**-n, -Vm-name** Geben Sie den Namen des virtuellen Computers. Dieser Parameter nimmt standardmäßig hosting Dienstname. -Vmname nicht angegeben ist, wird der Name für den neuen virtuellen Computer als generiert &lt;Dienstname >&lt;Id >, wobei &lt;Id > ist die Anzahl der vorhandenen virtuellen Computer den Dienst plus 1. Beispielsweise, wenn Sie diesen Befehl verwenden, um einen virtuellen Computer zu einem Hostingdienst MyService hinzufügen, die einen vorhandenen virtuellen Computer, den neuen virtuellen Computer MyService2 heißt.<br />
**-u, -Blob-url** Geben Sie Ziel-BLOB-Speicher-URL mit virtuellen System erstellen. <br />
**-Z, Vm-Größe** Geben Sie die Größe des virtuellen Computers. Gültige Werte sind: "ExtraSmall", "Klein", "Medium", "Groß", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Der Standardwert ist "Klein". <br />
**-r** RDP-Verbindung hinzugefügt zu einem Windows-Computer. <br />
**-e,-ssh** SSH-Verbindung hinzugefügt zu einem Windows-Computer. <br />
**-t, ssh-Zertifikat** Gibt das SSH-Zertifikat. <br />
**-s** Das Abonnement <br />
**-o-Gemeinschaft** Das angegebene Bild ist ein Community-Bild <br />
**-w** Namen des virtuellen Netzwerks <br/>
**-l, -Position** gibt die Position (z. B. "US North Central"). <br />
**-a, Gruppe Affinität** gibt die Gruppe an.<br />
**-w, virtuelle Netzwerknamen** Geben Sie das virtuelle Netzwerk, fügen Sie den neuen virtuellen Computer. Virtuelle Netzwerke können einrichten und von Azure-Verwaltungsportal verwaltet werden.<br />
**-b, Subnetz-Namen** Gibt die Subnetznamen der virtuellen Maschine zuweisen.

In diesem Beispiel ist MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB ein Bild der Plattform. Weitere Informationen zum Betriebssystem-Images finden Sie unter Vm-Bildliste.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**VM Erstellen von &lt;DNS-Name > &lt;Rolle Datei >**

Dieser Befehl erstellt eine Azure VM aus einer JSON-Rolle-Datei.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**VM-Liste [Optionen]**

Dieser Befehl listet Azure virtuelle Computer. Die Json - Option gibt an, dass die Ergebnisse im raw JSON-Format zurückgegeben werden.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**VM Liste [Optionen]**

Dieser Befehl listet alle verfügbaren Azure-Konto.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**VM anzeigen [Optionen] &lt;Name >**

Dieser Befehl zeigt die Details einer Azure Virtual Machine. Die Json - Option gibt an, dass die Ergebnisse im raw JSON-Format zurückgegeben werden.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**VM löschen [Optionen] &lt;Name >**

Dieser Befehl löscht ein Azure Virtual Machine. Standardmäßig ist dieser Befehl nicht Azure Blob löschen aus dem Betriebssystem-CD und der Datenträger erstellt werden Um löschen BLOBs und den virtuellen Computer, auf dem es basiert, geben Sie die Option -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**VM starten [Optionen] &lt;Name >**

Dieser Befehl startet ein Azure Virtual Machine.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**VM starten [Optionen] &lt;Name >**

Dieser Befehl startet ein Azure Virtual Machine.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**VM Herunterfahren [Optionen] &lt;Name >**

Dieser Befehl beendet eine Azure Virtual Machine. Die Option-p können Sie angeben, dass Compute-Ressourcen werden beim Herunterfahren nicht freigegeben.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**VM-Aufnahme &lt;Vm-Name > &lt;Ziel-Image-Name >**

Dieser Befehl zeichnet ein Bild Azure Virtual Machine.

Abbild eines virtuellen Computers kann nur erfasst werden, wenn der Zustand des virtuellen Computers **beendet**ist. Der virtuelle Computer zunächst Herunterfahren.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**VM-Export [Optionen] &lt;Vm-Name > &lt;-Pfad >**

Dieser Befehl exportiert eine Azure VM Image in eine Datei

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Befehle zum Verwalten von Azure Virtual Machine Endpunkte
Die folgende Abbildung zeigt die Architektur typischerweise mehrere Instanzen eines klassischen virtuellen Computers. In diesem Beispiel ist Port 3389 geöffnet auf jedem virtuellen Computer (für RDP-Zugriff). Auf jedem virtuellen Computer, mit dem Lastenausgleich Datenverkehr virtuellen Computer ist auch eine interne IP-Adresse (beispielsweise 168.55.11.1). Diese interne IP-Adresse kann auch für die Kommunikation zwischen virtuellen Computern verwendet werden.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Externe Anfragen zu virtuellen Maschinen durchlaufen einen Lastenausgleich. Aus diesem Grund können Anfragen für einen bestimmten virtuellen Computer mehrere virtuelle Computer auf Bereitstellung angegeben werden. Für Installationen mit mehreren virtuellen Computern muss die Zuordnung zwischen virtuellen Maschinen (Vm-Port) und Lastenausgleich (lb-Port) konfiguriert werden.

**Erstellen Sie VM &lt;Vm-Name > &lt;lb-Port > [Vm-Anschluss]**

Dieser Befehl erstellt einen virtuellen Endpunkt. Sie können auch -u oder – aktivieren Direct-Server zurück an, ob ermöglichen direkte Server auf diesen Endpunkt standardmäßig zurück.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**Vm Endpunkt erstellen mehrere [Optionen] &lt;Vm-Name > &lt;lb-Port > [:&lt;Vm-Port > [:&lt;Protokoll > [:&lt;aktivieren Direct-Server zurück > [:&lt;lb SetName > [:&lt;Prüfpunkt-Protokoll > [:&lt;Prüfpunkt Port > [:&lt;Prüfpunkt-Pfad > [:&lt;interne lb-Name >]]] {1 -*}**

Erstellen Sie mehrere Vm-Endpunkte.

**VM-Endpunkt löschen [Optionen] &lt;Vm-Name > &lt;Endpunkt-Name >**

Dieser Befehl löscht einen virtuellen Endpunkt.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**VM-Endpunktliste &lt;Vm-Name >**

Dieser Befehl listet alle virtuellen Endpunkte. Die Json - Option gibt an, dass die Ergebnisse im raw JSON-Format zurückgegeben werden.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**VM-Endpunkt Update [Optionen] &lt;Vm-Name > &lt;Endpunkt-Name >**

Dieser Befehl aktualisiert einen Vm-Endpunkt auf neue Werte mit diesen Optionen.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**VM-Endpunkt anzeigen [Optionen] &lt;Vm-Name >**

Dieser Befehl zeigt die Details der Endpunkte auf einem virtuellen Computer

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Befehle zum Verwalten von Azure Virtual Machine Bilder

Images von virtuellen Maschinen sind erfasst bereits konfigurierte virtuelle Computer, die repliziert werden können wie erforderlich.

**VM-Bildliste [Optionen]**

Dieser Befehl ruft eine Liste von Images virtueller Maschinen. Es gibt drei Arten von Bildern: Bilder von Microsoft, der mit "MSFT" vorangestellt sind, Bilder von Dritten, der Name des Kreditors vorangestellt, Sie Bilder erstellen. Erstellen Sie Bilder vorhandenen virtuellen Computer aufzeichnen oder erstellen Sie ein Bild aus einer benutzerdefinierten VHD-BLOB-Speicher hochgeladen. Weitere Informationen über benutzerdefinierte VHD finden Sie unter Vm-Image erstellen.
Die Json - Option gibt an, dass die Ergebnisse im raw JSON-Format zurückgegeben werden.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**VM Bild anzeigen [Optionen] &lt;Name >**

Dieser Befehl zeigt die Details der Abbild eines virtuellen Computers.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**VM Image löschen [Optionen] &lt;Name >**

Dieser Befehl löscht Abbild eines virtuellen Computers.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**VM-Image Erstellen &lt;Name > [Quellpfad]**

Dieser Befehl erstellt Abbild eines virtuellen Computers. Benutzerdefinierte VHD-Dateien BLOB-Speicher hochgeladen werden, und die VM-Image wird dort erstellt. Dann verwenden Sie Abbild dieses virtuellen Computers auf einen virtuellen Computer erstellen. Speicherort und OS-Parameter sind erforderlich.

>[AZURE.NOTE]Dieser Befehl unterstützt derzeit nur Upload festen VHD-Dateien. Dynamische VHD-Datei hochladen, verwenden Sie die [Azure VHD Dienstprogramme gehen](https://github.com/Microsoft/azure-vhd-utils-for-go).

Einige Systeme beschränkt pro Datei Deskriptor. Wenn diese Grenze überschritten wird, zeigt das Tool einen Deskriptor Grenzwert Fehler. Führen Sie den Befehl erneut mit-p &lt;Zahl > Parameter für die maximale Anzahl von parallelen Uploads verringern. Die standardmäßige maximale Anzahl parallele Uploads ist 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Befehle zum Verwalten der Azure virtuellen Datenträger

Datenträger werden VHD-Dateien im Blob-Speicher von einem virtuellen Computer. Weitere Informationen zur Bereitstellung von Datenträger BLOB-Speicher finden Sie unter Azure technische Diagramm gezeigt.

Die Befehle für Datenträger anfügen (Azure Vm Festplatte und Azure Vm Datenträger anhängen-) angeschlossenen Datenträger gemäß des SCSI-Protokolls eine logische Gerätenummer (LUN) zuweisen. Der erste Datenträger einer virtuellen Maschine zugeordnet zugeordnet ist LUN 0, die nächste zugewiesene LUN 1 und So weiter.

Wenn Sie einen Datenträger mit dem Datenträger Azure Vm trennen Befehl trennen, die &lt;Lun&gt; Parameter an, welche Diskette trennen.

>[AZURE.NOTE] Sie sollten immer Datenträger in umgekehrter Reihenfolge trennen, beginnend mit der höchsten Nummer LUN, die zugewiesen wurde. Ebene Linux SCSI-unterstützt LUN niedrigeren Nummern trennen, während noch eine höhere Nummer LUN wird nicht. Beispielsweise sollten Sie nicht trennen LUN 0 Wenn LUN 1 noch angeschlossen ist.

**VM Datenträger anzeigen [Optionen] &lt;Name >**

Dieser Befehl zeigt Details zu Azure Datenträger.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**VM Datenträgerliste [Optionen] [Vm-Name]**

Dieser befehlslisten Azure Datenträger oder Datenträgern mit einem angegebenen virtuellen Computer. Wenn mit einem virtuellen Namensparameter ausgeführt wird, wird allen Datenträgern der virtuellen Maschine. LUN 1 mit dem virtuellen Computer erstellt und aufgeführten Datenträger separat angefügt.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Dieser Befehl ohne Parameter virtuellen Namen gibt alle Festplatten.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**VM Datenträger löschen [Optionen] &lt;Name >**

Dieser Befehl löscht einen Azure Datenträger aus einem persönlichen Repository. Der Datenträger muss vom virtuellen Computer getrennt werden, bevor es gelöscht wird.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**VM erstellen &lt;Name > [Quellpfad]**

Dieser Befehl lädt und Azure Datenträger registriert. -Blob-Url, Speicherort oder - Gruppe Affinität muss angegeben werden. Verwenden Sie diesen Befehl mit [Quellpfad] angegebene VHD-Datei hochgeladen und ein Abbild erstellt. Sie können dieses Bild dann einer virtuellen Maschine mit Vm anfügen anfügen.

Einige Systeme beschränkt pro Datei Deskriptor. Wenn diese Grenze überschritten wird, zeigt das Tool einen Deskriptor Grenzwert Fehler. Führen Sie den Befehl erneut mit-p &lt;Zahl > Parameter für die maximale Anzahl von parallelen Uploads verringern. Die standardmäßige maximale Anzahl parallele Uploads ist 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**VM Festplatte hochladen [Optionen] &lt;Source-Pfad > &lt;Blob-Url > &lt;Konto Speicherschlüssel >**

Dieser Befehl ermöglicht es eine Vm-Festplatte hochladen

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**VM Festplatte &lt;Vm-Name > &lt;Disk-Image-Name >**

Dieser Befehl fügt eine vorhandene Festplatte im BLOB-Speicher an vorhandenen virtuellen Computer im Cloud-Dienst bereitgestellt.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**VM Datenträger anhängen neue &lt;Vm-Name > &lt;Größe in gb > [Blob-Url]**

Dieser Befehl fügt einen Datenträger Azure virtuellen Computer. In diesem Beispiel ist 20 die Größe der neuen Festplatte in Gigabyte angefügt werden soll. Optional können eine BLOB-URL als letztes Argument Sie das Ziel-Blob erstellen explizit angeben. Wenn Sie keine BLOB-URL angeben, wird automatisch ein Blob-Objekt generiert.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**VM-Laufwerk trennen &lt;Vm-Name > &lt;Lun >**

Dieser Befehl löst einen Datenträger mit einer Azure Virtual Machine verbunden. &lt;LUN > bezeichnet die Datenträger getrennt werden. Verwenden, um eine Liste der Datenträger einen Datenträger zugeordnet, bevor Sie getrennt, Vm Datenträgerliste &lt;Vm-Name >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Befehle zum Verwalten der Azure-Cloud-Dienste

Azure-Cloud-Dienste sind Programme und Dienste auf Webrollen und Arbeitskraft. Die folgenden Befehle dienen zum Verwalten von Azure Cloud Services.

**Dienst erstellen [Optionen] &lt;Dienstname >**

Dieser Befehl erstellt einen Clouddienst

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**Dienst anzeigen [Optionen] &lt;Dienstname >**

Dieser Befehl zeigt die Details der Azure-Cloud-Dienst

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**Dienstliste [Optionen]**

Dieser Befehl listet Azure Cloud Services.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**Service löschen [Optionen] &lt;Name >**

Dieser Befehl löscht einen Azure-Cloud-Dienst.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Den Löschvorgang erzwingen, die `-q` Parameter.


## <a name="commands-to-manage-your-azure-certificates"></a>Befehle zum Verwalten von Azure-Zertifikaten

Azure Service sind SSL-Zertifikate Ihre Azure-Konto verbunden. Weitere Informationen zu Azure Zertifikaten finden Sie unter [Verwalten von Zertifikaten](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**Cert Serviceliste [Optionen]**

Dieser Befehl listet Azure Zertifikate.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Service-Zertifikat erstellen &lt;Dns-Präfix > &lt;Datei > [Kennwort]**

Dieser Befehl lädt ein Zertifikat. Lassen Sie die Aufforderung zur Kennworteingabe für Zertifikate, die nicht kennwortgeschützt

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Service-Zertifikat löschen [Optionen] &lt;Fingerabdruck >**

Dieser Befehl löscht ein Zertifikat.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Befehle zum Verwalten von Web-apps

Eine Azure Web app ist eine Webkonfiguration zugreifen URI. Webapps auf virtuellen Computern gehostet, jedoch keine Informationen zum Erstellen und Bereitstellen der virtuellen Maschine selbst denken müssen. Diese Details werden Sie von Azure behandelt.

**Liste [Optionen]**

Dieser Befehl zeigt die Web-apps.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**Website festlegen [Optionen] [Name]**

Dieser Befehl legt Konfigurationsoptionen für Ihre Web [Name]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**Website-Deploymentscript [Optionen]**

Dieser Befehl generiert einen benutzerdefinierten Bereitstellungsskripts

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**Website erstellen [Optionen] [Name]**

Dieser Befehl erstellt eine WebApp und lokalen Verzeichnis.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Der Sitename muss eindeutig sein. Sie können keine Website mit demselben DNS-Namen wie eine vorhandene Site erstellen.

**Site durchsuchen [Optionen] [Name]**

Dieser Befehl wird Ihrer Anwendung in einem Browser geöffnet.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**Website anzeigen [Optionen] [Name]**

Dieser Befehl zeigt die Details für eine Webanwendung.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**Website löschen [Optionen] [Name]**

Dieser Befehl löscht eine Web app.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **Site austauschen [Optionen] [Name]**

Mit diesem Befehl werden zwei Web app Steckplätze vertauscht.

Dieser Befehl unterstützt die folgende zusätzliche Option:

**- Q oder **--Quiet **: keine Aufforderung zur Bestätigung. Verwenden Sie diese Option in automatisierten Skripten.


**Site starten [Optionen] [Name]**

Dieser Befehl startet eine Webanwendung.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**Website beenden [Optionen] [Name]**

Dieser Befehl beendet eine Webanwendung.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**Website-Neustart [Optionen] [Name]**

Dieser Befehl stoppt und startet eine angegebene Web app.

Dieser Befehl unterstützt die folgende zusätzliche Option:

**-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.


**Speicherort der Siteliste [Optionen]**

Dieser Befehl zeigt die app Webadressen.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Befehle zum Verwalten von Einstellungen Anwendung Web app

**Appsetting Liste [Optionen] [Name]**

Dieser Befehl zeigt die app-Einstellung Web app hinzugefügt.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**Website Appsetting hinzufügen [Optionen] &lt;Keyvaluepair > [Name]**

Dieser Befehl fügt eine app-Einstellung zu Ihrer Anwendung als Schlüssel-Wert-Paar.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**Website Appsetting löschen [Optionen] &lt;Schlüssel > [Name]**

Dieser Befehl löscht die Einstellung angegebene Anwendung aus dem Web app.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**Website Appsetting anzeigen [Optionen] &lt;Schlüssel > [Name]**

Dieser Befehl zeigt Details der angegebenen app-Einstellung

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Befehle zum Verwalten Ihrer Web app Zertifikate

**Cert Liste [Optionen] [Name]**

Dieser Befehl zeigt eine Liste der Web app Zertifikate.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**Website-Zertifikat hinzufügen [Optionen] &lt;Zertifikat Pfad > [Name]**

**Website-Zertifikat löschen [Optionen] &lt;Fingerabdruck > [Name]**

**Site-Zertifikat anzeigen [Optionen] &lt;Fingerabdruck > [Name]**

Dieser Befehl zeigt die Zertifikat-details

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Befehle zum Verwalten von Web app Verbindungszeichenfolgen

**Connectionstring Liste [Optionen] [Name]**

**Site Connectionstring hinzufügen [Optionen] &lt;Verbindungsname > &lt;Wert > &lt;Type > [Name]**

**Site Connectionstring löschen [Optionen] &lt;Verbindungsname > [Name]**

**Site Connectionstring anzeigen [Optionen] &lt;Verbindungsname > [Name]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Befehle zum Verwalten Ihrer Web app Standard-Dokumente

**Defaultdocument Liste [Optionen] [Name]**

**Website Defaultdocument hinzufügen [Optionen] &lt;Dokument > [Name]**

**Website Defaultdocument löschen [Optionen] &lt;Dokument > [Name]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Befehle zum Verwalten der Bereitstellung Ihrer Web app

**Bereitstellung Siteliste [Optionen] [Name]**

**websitebereitstellung anzeigen [Optionen] &lt;CommitId > [Name]**

**Website-Bereitstellung bereitstellen [Optionen] &lt;CommitId > [Name]**

**Website-Bereitstellung Github [Optionen] [Name]**

**Website-Bereitstellung Benutzersatz [Optionen] [Benutzername] [Kennwort]**

###<a name="commands-to-manage-your-web-app-domains"></a>Befehle zum Verwalten der Anwendungsdomänen web

**Website-Domänenliste [Optionen] [Name]**

**Website-Domäne hinzufügen [Optionen] &lt;dn > [Name]**

**Website-Domäne löschen [Optionen] &lt;dn > [Name]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Befehle zum Verwalten von Ihrem Web app Handlerzuordnungen

**Handler Liste [Optionen] [Name]**

**Website-Handler hinzufügen [Optionen] &lt;Erweiterung > &lt;Prozessor > [Name]**

**Website-Handler löschen [Optionen] &lt;Erweiterung > [Name]**

###<a name="commands-to-manage-your-web-jobs"></a>Befehle zum Verwalten von Web-Jobs

**Siteliste [Optionen] [Name]**

Dieser Befehl zeigt alle Webaufträge unter Web app.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Job-Typ** &lt;Beschäftigungsart >: Optional. Der Typ der Webauftrag. Gültige Wert ist "Trigger" oder "fortlaufend". Standardmäßig Webaufträge aller Typen zurück.
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

**Site-Auftrag anzeigen [Optionen] &lt;JobName > &lt;im JobType > [Name]**

Dieser Befehl zeigt die Details einer bestimmten Webauftrag.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen >: erforderlich. Der Name der Webauftrag.
+ **-Job-Typ** &lt;Beschäftigungsart >: erforderlich. Der Typ der Webauftrag. Gültige Wert ist "Trigger" oder "fortlaufend".
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

**Website löschen [Optionen] &lt;JobName > &lt;im JobType > [Name]**

Dieser Befehl löscht den angegebenen Webauftrag.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen > erforderlich. Der Name der Webauftrag.
+ **-Job-Typ** &lt;Beschäftigungsart > erforderlich. Der Typ der Webauftrag. Gültige Wert ist "Trigger" oder "fortlaufend".
+ **-q** oder **--Stiller**: keine Aufforderung zur Bestätigung. Verwenden Sie diese Option in automatisierten Skripten.
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

**Site Auftrag Upload [Optionen] &lt;JobName > &lt;im JobType > <jobFile> [Name]**

Dieser Befehl löscht den angegebenen Webauftrag.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen >: erforderlich. Der Name der Webauftrag.
+ **-Job-Typ** &lt;Beschäftigungsart >: erforderlich. Der Typ der Webauftrag. Gültige Wert ist "Trigger" oder "fortlaufend".
+ **-Job-Datei** &lt;Job-Datei >: erforderlich. Die jobdatei.
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

**Site-Job Start [Optionen] &lt;JobName > &lt;im JobType > [Name]**

Dieser Befehl startet den angegebenen Webauftrag.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen >: erforderlich. Der Name der Webauftrag.
+ **-Job-Typ** &lt;Beschäftigungsart >: erforderlich. Der Typ der Webauftrag. Gültige Wert ist "Trigger" oder "fortlaufend".
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

**Website-Auftragsende [Optionen] &lt;JobName > &lt;im JobType > [Name]**

Dieser Befehl beendet den angegebenen Webauftrag. Kontinuierliche Aufträge können beendet werden.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen >: erforderlich. Der Name der Webauftrag.
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

###<a name="commands-to-manage-your-web-jobs-history"></a>Befehle zum Verwalten der Einzelvorgänge Webprotokoll

**Site Verlaufsliste [Optionen] [JobName] [Name]**

Dieser Befehl zeigt eine Geschichte führt den angegebenen Webauftrag.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen >: erforderlich. Der Name der Webauftrag.
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

**Site Auftragsverlauf anzeigen [Optionen] [JobName] [RunId] [Name]**

Dieser Befehl ruft die Details des Auftrags für den angegebenen Webauftrag ausgeführt.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Namen** &lt;-Namen >: erforderlich. Der Name der Webauftrag.
+ **-Ausführen-Id** &lt;ausführen-Id >: Optional. Die Id des zur Historie. Wenn nicht angegeben, Anzeigen der neuesten ausführen
+ **-Steckplatz** &lt;Steckplatz >: der Name des Feldes neu starten.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Befehle zum Verwalten von Dell Diagnostics Web app

**Standortdatenbank herunterladen [Optionen] [Name]**

Downloaden Sie eine ZIP-Datei mit Ihrer Anwendung die Diagnose.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**Website-Protokoll Ende [Optionen] [Name]**

Dieser Befehl verbindet das Terminal Protokoll-streaming-Service.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**Website-Protokollsatz [Optionen] [Name]**

Dieser Befehl konfiguriert diagnostischen Optionen für Ihr Web app.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Befehle zum Verwalten von Web app repositories

**Website-Zweig [Optionen] &lt;Zweig > [Name]**

**Website-Repository löschen [Optionen] [Name]**

**Website-Repository synchronisieren [Optionen] [Name]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Befehle zum Verwalten der Web app skalieren

**Skalierungsmodus [Optionen] &lt;Modus > [Name]**

**Site skalieren Instanzen [Optionen] &lt;Instanzen > [Name]**


## <a name="commands-to-manage-azure-mobile-services"></a>Befehle zum Verwalten von Azure Mobile Services

Azure Mobile Services vereint eine Reihe von Azure Services, die Back-End-Funktionen für Ihre apps ermöglichen. Mobile Befehle sind in die folgenden Kategorien unterteilt:

+ [Befehle zum Verwalten von mobilen Dienstinstanzen](#Mobile_Services)
+ [Befehle zum Verwalten von mobilen Konfiguration](#Mobile_Configuration)
+ [Befehle zum Verwalten von mobilen Servicetabellen](#Mobile_Tables)
+ [Befehle zum Verwalten von mobilen Skripts](#Mobile_Scripts)
+ [Befehle zum Verwalten geplanter Aufträge](#Mobile_Jobs)
+ [Befehle zum mobilen Dienst skalieren](#Mobile_Scale)

Die folgenden Optionen gelten für die meisten Befehle für Mobile Dienste:

+ **-h** oder **-Hilfe**: Anzeige Informationen ausgegeben.
+ **-s `<id>` ** oder **-Abonnement `<id>` **: verwenden ein bestimmtes Dauerauftrags als `<id>`.
+ **-v** oder **– ausführliche**: ausführliche Ausgabe.
+ **Json-**: Schreiben JSON-Ausgabe.

### <a name="Mobile_Services"></a>Befehle zum Verwalten von mobilen Dienstinstanzen

**Mobile Speicherorte [Optionen]**

Dieser Befehl listet Standorten von Mobile Dienste unterstützt.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Mobile erstellen [Optionen] [Dienstname] [SqlAdminUsername] [SqlAdminPassword]**

Dieser Befehl erstellt einen mobilen Dienst und eine Datenbank von SQL Server.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **- R `<sqlServer>` ** oder **- SqlServer `<sqlServer>` **: Verwenden Sie eine vorhandene SQL Database Server als `<sqlServer>`.
+ **-d `<sqlDb>` ** oder **- SqlDb `<sqlDb>` **: Verwenden Sie vorhandenen SQL-Datenbank als `<sqlDb>`.
+ **-l `<location>` ** oder **-Speicherort `<location>` **: Erstellen den Dienst an einem bestimmten Speicherort als `<location>`. Führen Sie Azure mobile Speicherorte verfügbaren Standorte aus.
+ **- SqlLocation `<location>` **: Erstellen von SQL Server in einem `<location>`; Standardmäßig an den Mobilfunkanbieter.

**Mobile löschen [Optionen] [Dienstname]**

Dieser Befehl löscht einen mobilen Dienst SQL-Datenbank und Server.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-d** oder **--DeleteData**: Löschen aller Daten aus diesen mobilen Dienst aus der Datenbank.
+ **-** oder **- DeleteAll**: SQL-Datenbank und den Server zu löschen.
+ **-q** oder **--Stiller**: keine Aufforderung zur Bestätigung. Verwenden Sie diese Option in automatisierten Skripten.

**Mobile Listen [Optionen]**

Dieser Befehl zeigt die mobile-Dienste.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**Mobile anzeigen [Optionen] [Dienstname]**

Dieser Befehl zeigt Details zu einem mobilen Service.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**Mobile Neustart [Optionen] [Dienstname]**

Dieser Befehl startet eine mobilen Dienstinstanz.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**Mobile Protokoll [Optionen] [Dienstname]**

Dieser Befehl gibt Mobilfunkanbieter Protokolle alle Protokolltypen herausfiltern, aber `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **- R `<query>` ** oder **-Abfrage `<query>` **: führt die Abfrage angegebene Protokoll.
+ **t - `<type>` ** oder **-Typ `<type>` **: Filter zurückgegebenen Protokolle mit `<type>`, die `information`, `warning`, oder `error`.
+ **-k `<skip>` ** oder **-fahren Sie `<skip>` **: überspringt die Anzahl der Zeilen von angegebenen `<skip>`.
+ **-p `<top>` ** oder **-oben `<top>` **: Gibt eine bestimmte Anzahl von Zeilen von angegebenen `<top>`.

> [AZURE.NOTE] Die **-Abfrage** Parameter hat Vorrang vor **-Typ**, **-fahren**, und **-oben**.

**Mobile Recover [Optionen] [Unhealthyservicename] [Healthyservicename]**

Dieser Befehl stellt fehlerhafte Mobilfunkanbieter durch fehlerfrei mobilen Dienst in einen anderen Bereich verschieben.

Dieser Befehl unterstützt die folgende zusätzliche Option:

**-q** oder **--Stiller**: unterdrücken die Aufforderung zur Bestätigung der Wiederherstellung.

**Mobile Schlüssel regeneriert [Optionen] [Dienstname] [Typ]**

Dieser Befehl generiert die ANWENDUNGSTASTE Mobilfunkanbieter.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Wichtige Typen sind `master` und `application`.

> [AZURE.NOTE] Beim Regenerieren Schlüssel möglicherweise Clients, die den alten Schlüssel nicht auf Ihren mobilen Dienst zugreifen. Beim Regenerieren Anwendungstaste sollten Sie Ihre Anwendung mit der neue Schlüsselwert aktualisieren.

**Mobile Schlüsselsatz [Optionen] [Dienstname] [Typ] [Wert]**

Dieser Befehl stellt den mobile Schlüssel auf einen bestimmten Wert.


### <a name="Mobile_Configuration"></a>Befehle zum Verwalten von mobilen Konfiguration

**Mobile Config Liste [Optionen] [Dienstname]**

Dieser Befehl zeigt die Konfigurationsoptionen für einen mobilen Dienst.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**Mobile-Konfiguration erhalten [Optionen] [Dienstname] [Schlüssel]**

Dieser Befehl ruft eine bestimmte Konfigurationsoption für mobilen Service bei dynamischen Schema.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**Mobile-Konfiguration festlegen [Optionen] [Dienstname] [Key] [Wert]**

Mit diesem Befehl wird eine bestimmte Konfigurationsoption für mobilen Service bei dynamischen Schema.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Befehle zum Verwalten von mobilen Servicetabellen

**Mobile Tabellenliste [Optionen] [Dienstname]**

Dieser Befehl listet alle Tabellen in Ihrem Mobilfunkanbieter.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**Mobile Tabelle anzeigen [Optionen] [Dienstname] [Tabellenname]**

Dieser Befehl zeigt die Details zu einer bestimmten Tabelle zurück.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**Mobile erstellen [Dienstname] [Tabellenname] [Optionen]**

Dieser Befehl erstellt eine Tabelle.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

Dieser Befehl unterstützt die folgende zusätzliche Option:

+ **-p `&lt;permissions>` ** oder **-Berechtigungen `&lt;permissions>` **: durch Trennzeichen getrennte Liste von `<operation>` = `<permission>` -Paare, `<operation>` ist `insert`, `read`, `update`, oder `delete` und `&lt;permissions>` ist `public`, `application` (Standard), `user`, oder `admin`.

**Mobile Daten lesen [Optionen] [Dienstname] [Tabellenname] [Abfrage]**

Dieser Befehl liest Daten aus einer Tabelle.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-k `<skip>` ** oder **-fahren Sie `<skip>` **: überspringt die Anzahl der Zeilen von angegebenen `<skip>`.
+ **t - `<top>` ** oder **-oben `<top>` **: Gibt eine bestimmte Anzahl von Zeilen von angegebenen `<top>`.
+ **-l** oder **-Liste**: Gibt die Daten in einer Liste zurück.

**Mobile Aktualisierung [Optionen] [Dienstname] [Tabellenname]**

Dieser Befehl ändert nur Administratoren Berechtigungen für eine Tabelle löschen.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-p `&lt;permissions>` ** oder **-Berechtigungen `&lt;permissions>` **: durch Trennzeichen getrennte Liste von `<operation>` = `<permission>` -Paare, `<operation>` ist `insert`, `read`, `update`, oder `delete` und `&lt;permissions>` ist `public`, `application` (Standard), `user`, oder `admin`.
+ **- DeleteColumn `<columns>` **: durch Trennzeichen getrennte Liste von Spalten löschen, als `<columns>`.
+ **-q** oder **--Stiller**: Löscht Spalten ohne zur Bestätigung aufzufordern.
+ **- AddIndex `<columns>` **: durch Kommas getrennte Liste der Spalten im Index.
+ **- DeleteIndex `<columns>` **: durch Trennzeichen getrennte Liste von Spalten aus dem Index ausgeschlossen.

**Mobile Tabelle löschen [Optionen] [Dienstname] [Tabellenname]**

Dieser Befehl löscht eine Tabelle.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Festlegen des Parameters die Tabelle ohne Bestätigung löschen. Dies verhindert des Automatisierungsskripts.

**Mobile Daten abgeschnitten [Optionen] [Dienstname] [Tabellenname]**

Dieser Befehl entfernt alle Datenzeilen aus der Tabelle.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Befehle zum Verwalten von Skripts

Befehle in diesem Abschnitt wird die Serverskripts Verwalten mobiler Service. Weitere Informationen finden Sie unter [Arbeiten mit Serverskripts in Mobile Dienste](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**Mobile Skriptliste [Optionen] [Dienstname]**

Dieser Befehl listet registrierte Skripts einschließlich Tabelle und Planer Skripts.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**Mobile Skriptdownload [Optionen] [Dienstname] [Skriptname]**

Dieser Befehl Einfügeskript aus der TodoItem Tabelle downloads in einer Datei namens `todoitem.insert.js` in der `table` Unterordner.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-p `<path>` ** oder **-Pfad `<path>` **: die Position in der Datei, speichern Sie das Skript, das aktuelle Verzeichnis die Standardeinstellung ist.
+ **-f `<file>` ** oder **-Datei `<file>` **: der Name der Datei, das Skript zu speichern.
+ **-o** oder **--Überschreiben**: Überschreiben einer vorhandenen Datei.
+ **-c** oder **-Konsole**: das Skript in der Konsole nicht in eine Datei schreiben.

**Mobile Skript hochladen [Optionen] [Dienstname] [Skriptname]**

Dieser Befehl lädt ein Skript namens `todoitem.insert.js` aus der `table` Unterordner.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Der Name der Datei muss von Tabellen-und Vorgang bestehen. Es muss im Unterverzeichnis Tabelle relativ zum Speicherort befinden, in dem der Befehl ausgeführt wird. Sie können auch die **-f `<file>` ** oder **-Datei `<file>` ** Parameter einen anderen Dateinamen und Pfad der Datei mit dem Skript registriert.


**Mobile Skript löschen [Optionen] [Dienstname] [Skriptname]**

Dieser Befehl entfernt das vorhandene Einfügeskript aus der TodoItem-Tabelle.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Befehle zum Verwalten geplanter Aufträge

Befehle in diesem Abschnitt dienen zum geplanten Aufträge verwalten mobiler Service. Weitere Informationen finden Sie unter [Planen von Projekten](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**Liste der Mobile [Optionen] [Dienstname]**

Dieser Befehl listet geplante Aufträge.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**Mobile Auftrag erstellen [Optionen] [Dienstname] [Jobname]**

Dieser Befehl erstellt den Auftrag `getUpdates` stündlich ausführen soll.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-i `<number>` ** oder **-Intervall `<number>` **: Intervall Auftrag als ganze Zahl. Der Standardwert ist `15`.
+ **-u `<unit>` ** oder **- IntervalUnit `<unit>` **: die Einheit für das _Zeitintervall_, einen der folgenden Werte:
    + **Minute** (Standard)
    + **Stunde**
    + **Tag**
    + **Monat**
    + **keine** (bei Bedarf Aufträge)
+ **-t `<time>`** **- StartTime `<time>` ** Die Startzeit des ersten Skripts im ISO-Format. Der Standardwert ist `now`.

> [AZURE.NOTE] Arbeitsplätze entstehen deaktiviert, da ein Skript hochgeladen werden muss. Befehl **mobile Skript hochladen** , ein Skript und Befehls **mobile Projekt aktualisiert werden,** um den Auftrag zu aktivieren.

**Mobile Job aktualisieren [Optionen] [Dienstname] [Jobname]**

Der folgende Befehl aktiviert die deaktiviert `getUpdates` Projekt.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-i `<number>` ** oder **-Intervall `<number>` **: Intervall Auftrag als ganze Zahl. Der Standardwert ist `15`.
+ **-u `<unit>` ** oder **- IntervalUnit `<unit>` **: die Einheit für das _Zeitintervall_, einen der folgenden Werte:
    + **Minute** (Standard)
    + **Stunde**
    + **Tag**
    + **Monat**
    + **keine** (bei Bedarf Aufträge)
+ **-t `<time>`** **- StartTime `<time>` ** Die Startzeit des ersten Skripts im ISO-Format. Der Standardwert ist `now`.
+ **- `<status>` ** oder **-Status `<status>` **: der Status, die entweder `enabled` oder `disabled`.

**Mobile löschen [Optionen] [Dienstname] [Jobname]**

Dieser Befehl entfernt GetUpdates geplanten Auftrag vom Server Aufgabenliste.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Löschen Löscht auch das Upload-Skript.

### <a name="Mobile_Scale"></a>Befehle zum mobilen Dienst skalieren

Befehle in diesem Abschnitt wird ein Mobilfunkanbieter skalieren. Weitere Informationen finden Sie unter [Skalierung einen Mobilfunkanbieter](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**Skala anzeigen [Optionen] [Dienstname]**

Dieser Befehl zeigt Informationen, einschließlich Compute-Modus und Anzahl der Instanzen.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**Skala ändern [Optionen] [Dienstname]**

Dieser Befehl ändert die Skalierung des mobilen Diensts Premium-Modus aus.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **- C `<mode>` ** oder **- ComputeMode `<mode>` **: Compute-Modus muss entweder `Free` oder `Reserved`.
+ **-i `<count>` ** oder **- NumberOfInstances `<count>` **: die Anzahl der Instanzen im reservierten Modus verwendet.

> [AZURE.NOTE] Beim Festlegen von Compute-Modus `Reserved`, Ihre mobile Dienste in derselben Region im Premium-Modus ausführen.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Befehle zur Vorschau Funktionen für Mobile Service

**Mobile Vorschauliste [Optionen] [Dienstname]**

Dieser Befehl zeigt die vorschaufeatures angegebenen Dienst und ob sie aktiviert sind.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**Mobile Vorschau aktivieren [Optionen] [Dienstname] [Funktionsname]**

Dieser Befehl ermöglicht die angegebenen Vorschaufunktion für einen mobilen Dienst. Nach der Aktivierung können Vorschaufunktionen für einen mobilen Dienst deaktiviert werden.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Befehle zum Verwalten des mobilen Service-APIs

**Mobile api Liste [Optionen] [Dienstname]**

Dieser Befehl zeigt eine Liste mobile service benutzerdefinierte APIs für den mobilen Dienst erstellt haben.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**Mobile-api erstellen [Optionen] [Dienstname] [Apiname]**

Erstellt eine benutzerdefinierte mobile Service API

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

Dieser Befehl unterstützt die folgende zusätzliche Option:

**-p** oder **-Berechtigungen** &lt;Berechtigungen >: eine durch Kommas getrennte Liste der &lt;Methode > =&lt;Berechtigung > Paare.

**Mobile api Update [Optionen] [Dienstname] [Apiname]**

Dieser Befehl aktualisiert die angegebene mobile benutzerdefinierte-API.

Dieser Befehl unterstützt die folgende zusätzliche Option:

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-p** oder **-Berechtigungen** &lt;Berechtigungen >: eine durch Kommas getrennte Liste der &lt;Methode > =&lt;Berechtigung > Paare.
+ **-f** oder **-erzwingen**: benutzerdefinierten Änderungen Metadatendatei Berechtigungen überschrieben.

**Mobile-api löschen [Optionen] [Dienstname] [Apiname]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

Dieser Befehl löscht die angegebene mobile benutzerdefinierte-API.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Befehle zum Verwalten von Einstellungen der mobilen Anwendung

**Mobile Appsetting Liste [Optionen] [Dienstname]**

Dieser Befehl zeigt die mobile Anwendung app-Einstellungen für den angegebenen Dienst.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**Mobile Appsetting hinzufügen [Optionen] [Dienstname] [Name] [Wert]**

Dieser Befehl fügt eine benutzerdefinierte anwendungseinstellung für den mobilen Dienst.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**Mobile Appsetting löschen [Optionen] [Dienstname] [Name]**

Dieser Befehl entfernt die angegebene anwendungseinstellung für den mobilen Dienst.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**Mobile Appsetting anzeigen [Optionen] [Dienstname] [Name]**

Dieser Befehl entfernt die angegebene anwendungseinstellung für den mobilen Dienst.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Lokale Einstellungen verwalten

Lokale Einstellung sind Ihre Abonnement-ID und Storage-Kontonamen Standard.

**Config Liste [Optionen]**

Dieser Befehl zeigt Config Settings.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**Konfiguration festlegen [Optionen] &lt;Name&gt;,&lt;Wert&gt;**

Dieser Befehl ändert eine Einstellung.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Befehle zum Verwalten von Service Bus

Verwenden Sie diese Befehle zur Verwaltung Ihres Kontos Service Bus

**SB Namespace Kontrollkästchen [Optionen] &lt;Name >**

Überprüfen Sie, ob ein Service Bus-Namespace rechtliche und verfügbar.

**SB Namespace erstellen &lt;Name > &lt;Position >**

Erstellt einen Service Bus-Namespace.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**SB Namespace löschen &lt;Name >**

Entfernen eines Namespaces.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**SB Namespaceliste**

Listen Sie alle Namespaces, die für Ihr Konto erstellt.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**SB Namespaceliste**

Zeigt eine Liste der verfügbaren Namespace Positionen.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**SB Namespace anzeigen &lt;Name >**

Anzeigen von Details zu einem bestimmten Namespace.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**SB Namespace überprüfen &lt;Name >**

Überprüfen Sie, ob der Namespace verfügbar ist.

## <a name="commands-to-manage-your-storage-objects"></a>Befehle zum Verwalten der Speicherobjekte

###<a name="commands-to-manage-your-storage-accounts"></a>Befehle zum Verwalten von Speicherkonten

**Speicher-Kontoliste [Optionen]**

Dieser Befehl zeigt die Speicherkonten Ihres Abonnements.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**Storage-Konto anzeigen [Optionen]<name>**

Dieser Befehl zeigt Informationen einschließlich der URI und Konto Eigenschaften angegebenen Speicherkonto.

**Konto erstellen [Optionen]<name>**

Dieser Befehl erstellt ein Speicherkonto basierend auf den angegebenen Optionen.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-e** oder **-Bezeichnung** &lt;Bezeichnung >: die Bezeichnung für das Speicherkonto.
+ **-d** oder **-Beschreibung** &lt;Beschreibung >: Speicherkonto Beschreibung.
+ **-l** oder **-Speicherort** &lt;Name >: das geografische Gebiet, das Speicherkonto erstellen.
+ **-** oder **-Gruppe** &lt;Name >: der Gruppe Affinität, Storage-Konto zuzuordnen. 
+ **-Typ**: Gibt den Typ des Kontos zu erstellen: entweder Standardspeicher Redundanz Option (LRS/ZRS/g/RAGRS) oder Premium-Speicher (ÖREB).

**Konto festlegen [Optionen]<name>**

Dieser Befehl aktualisiert das angegebenen Speicherkonto.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-e** oder **-Bezeichnung** &lt;Bezeichnung >: die Bezeichnung für das Speicherkonto.
+ **-d** oder **-Beschreibung** &lt;Beschreibung >: Speicherkonto Beschreibung.
+ **-l** oder **-Speicherort** &lt;Name >: das geografische Gebiet, das Speicherkonto erstellen.
+ **-Typ**: Gibt den neuen Konto: entweder Standardspeicher Redundanz Option (LRS/ZRS/g/RAGRS) oder Premium-Speicher (ÖREB).

**Storage-Konto löschen [Optionen]<name>**

Dieser Befehl löscht das angegebenen Speicherkonto.

Dieser Befehl unterstützt die folgende zusätzliche Option:

**-q** oder **--Stiller**: keine Aufforderung zur Bestätigung. Verwenden Sie diese Option in automatisierten Skripten.

###<a name="commands-to-manage-your-storage-account-keys"></a>Befehle zum Verwalten der speicherkontoschlüssel

**speicherkontoschlüssel Liste [Optionen]<name>**

Dieser Befehl zeigt die primären und sekundären Schlüssel für das angegebene Speicherkonto.

**Storage-kontoschlüssel erneuern [Optionen]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Befehle zum Verwalten der Behälter

**Container Speicherliste [Optionen] [Prefix]**

Dieser Befehl zeigt der Speicherliste Container für einen angegebenen Speicherkonto. Das Speicherkonto wird durch die Verbindungszeichenfolge oder Konto und Speicherschlüssel angegeben.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-p** oder **-Präfix** &lt;Präfix >: das Präfix Storage Container.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: Speicher-Befehl im Debugmodus ausgeführt.

**Storage Container anzeigen [Optionen] [Container-]**
**Behälter erstellen [Optionen] [Container-]**

Dieser Befehl erstellt einen Speichercontainer für die angegebene Speicherkonto. Das Speicherkonto wird durch die Verbindungszeichenfolge oder Konto und Speicherschlüssel angegeben.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **-p** oder **-Präfix** &lt;Präfix >: das Präfix Storage Container.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge
+ **-Debuggen**: Speicher-Befehl im Debugmodus ausgeführt.

**Storage Container löschen [Optionen] [Container-]**

Dieser Befehl löscht den angegebenen Speichercontainer. Das Speicherkonto wird durch die Verbindungszeichenfolge oder Konto und Speicherschlüssel angegeben.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **-p** oder **-Präfix** &lt;Präfix >: das Präfix Storage Container.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: Speicher-Befehl im Debugmodus ausgeführt.

**Behälter festlegen [Optionen] [Container-]**

Mit diesem Befehl wird die Zugriffssteuerungsliste für die Behälter. Das Speicherkonto wird durch die Verbindungszeichenfolge oder Konto und Speicherschlüssel angegeben.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **-p** oder **-Präfix** &lt;Präfix >: das Präfix Storage Container.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: Speicher-Befehl im Debugmodus ausgeführt.

###<a name="commands-to-manage-your-storage-blob"></a>Befehle zum Verwalten des Speicher-BLOBs

**Blob Speicherliste [Optionen] [Container-] [Prefix]**

Dieser Befehl gibt eine Liste der Speicher-Blobs in der angegebenen Speichercontainer.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **-p** oder **-Präfix** &lt;Präfix >: das Präfix Storage Container.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: Speicher-Befehl im Debugmodus ausgeführt.

**Speicher-Blob anzeigen [Optionen] [Container-] [Blob]**

Dieser Befehl zeigt die Details des angegebenen Speicher-Blob.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **-p** oder **-Präfix** &lt;Präfix >: das Präfix Storage Container.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: führt den Befehl Speicher Debuggen.

**Speicher-Blob löschen [Optionen] [Container-] [Blob]**

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **b -** oder **-Blob** &lt;BlobName >: der Name des Blob Speicher löschen.
+ **-q** oder **– stillen**: das angegebene Blob Speicher ohne Bestätigung entfernen.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: führt den Befehl Speicher Debuggen.

**Speicher-Blob hochladen [Optionen] [Datei] [Container-] [Blob]**

Dieser Befehl hochladen die angegebene Datei angegebene Speicher-BLOB.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **b -** oder **-Blob** &lt;BlobName >: der Name des Blob Speicher hochladen.
+ **-t** oder **-Blobtype** &lt;Blobtype >: Blob Speichertyp: fest.
+ **-p** oder **-Eigenschaften** &lt;Eigenschaften >: Blob Speichereigenschaften für hochgeladene Datei. Eigenschaften sind Schlüssel = Wert-Paar s und Semikolon (;) getrennt. Verfügbare Eigenschaften sind ContentType, ContentEncoding, ContentLanguage und CacheControl.
+ **-m** oder **Metadaten -** &lt;Metadaten >: Blob Speichermetadaten für hochgeladene Datei. Metadaten sind Schlüssel = Wert-Paaren ein Semikolon (;) getrennt.
+ **--concurrenttaskcount** &lt;Concurrenttaskcount >: die maximale Anzahl von gleichzeitigen Upload-Anfragen.
+ **-q** oder **– stillen**: das angegebene Blob Speicher ohne Bestätigung überschreiben.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: führt den Befehl Speicher Debuggen.

**Speicher-Blob herunterladen [Optionen] [Container-] [Blob] [Ziel]**

Dieser Befehl lädt angegebenen Speicher-Blob.

Dieser Befehl unterstützt die folgenden zusätzlichen Optionen:

+ **-Container** &lt;Container >: der Name des Containers Speicher erstellen.
+ **b -** oder **-Blob** &lt;BlobName >: Namen BLOB-Speicher.
+ **-d** oder **-Ziel** [Ziel]: der Download Ziel Datei- oder Verzeichnispfad.
+ **-m** oder **-checkmd5**: md5sum Kontrollkästchen für die Datei.
+ **--concurrenttaskcount** &lt;Concurrenttaskcount > die maximale Anzahl von gleichzeitigen Upload-Anfragen
+ **-q** oder **--Stiller**: die Datei ohne Bestätigung überschreiben.
+ **-** oder **-Namen-** &lt;Kontoname >: Speicher-Kontoname.
+ **-k** oder **-Konto Schlüssel** &lt;AccountKey >: Speicherschlüssel Konto.
+ **-c** oder **-Verbindungszeichenfolge** &lt;ConnectionString >: Speicher-Verbindungszeichenfolge.
+ **-Debuggen**: führt den Befehl Speicher Debuggen.

## <a name="commands-to-manage-sql-databases"></a>Befehle zum Verwalten von SQL-Datenbanken

Verwenden Sie diese Befehle zur Verwaltung von Datenbanken SQL Azure

###<a name="commands-to-manage-sql-servers"></a>Befehle zum Verwalten von SQL Server.

Mit diesen Befehlen können Sie Ihre SQL Server verwalten

**Erstellen von SQLServer &lt;AdministratorLogin > &lt;AdministratorPassword > &lt;Position >**

Erstellen Sie einen Datenbankserver

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**Anzeigen von SQL Server &lt;Name >**

Serverdetails anzeigen

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**SQL Server-Liste**

Ruft die Liste der Server.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**SQL Server löschen &lt;Name >**

Löscht einen server

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Befehle zum Verwalten von SQL-Datenbanken

Verwenden Sie diese Befehle zum Verwalten der SQL-Datenbanken.

**SQL-Datenbank erstellen [Optionen] &lt;ServerName > &lt;DatabaseName > &lt;AdministratorPassword >**

Erstellt eine Datenbankinstanz

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**SQL-Datenbank anzeigen [Optionen] &lt;ServerName > &lt;DatabaseName > &lt;AdministratorPassword >**

Details der Datenbank anzuzeigen.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**Liste der SQL-Db [Optionen] &lt;ServerName > &lt;AdministratorPassword >**

Liste der Datenbanken.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**SQL-Datenbank löschen [Optionen] &lt;ServerName > &lt;DatabaseName > &lt;AdministratorPassword >**

Löscht eine Datenbank.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Befehle zum Verwalten der SQL Server-Firewall-Regeln

Mit diesen Befehlen können Sie die SQL Server-Firewall-Regeln verwalten

**SQL Firewallrule erstellen [Optionen] &lt;ServerName > &lt;Regelname > &lt;Start > &lt;EndIPAddress >**

Erstellen Sie eine Firewallregel für SQL Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**SQL Firewallrule anzeigen [Optionen] &lt;ServerName > &lt;Regelname >**

Firewall-Regeldetails anzeigen.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**SQL Firewallrule Liste [Optionen] &lt;ServerName >**

Liste der Firewall-Regeln.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**SQL Firewallrule löschen [Optionen] &lt;ServerName > &lt;Regelname >**

Dieser Befehl löscht eine Firewall-Regel.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Befehle zum Verwalten von virtuellen Netzwerken

Mit diesen Befehlen können Sie virtuelle Netzwerke verwalten

**Netzwerk-Vnet erstellen [Optionen] &lt;Position >**

Erstellen Sie ein virtuelles Netzwerk.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**Vnet anzeigen Netz &lt;Name >**

Zeigt Details ein virtuelles Netzwerk.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**Vnet Liste**

Listen Sie alle vorhandenen virtuellen Netzwerke.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**Löschen Sie Netzwerk-Vnet &lt;Name >**

Löscht die angegebene virtuelle Netzwerk.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**Netzwerk-Export [Dateipfad]**

Für erweiterte Netzwerkkonfiguration können Sie Ihre Netzwerkkonfiguration lokal exportieren. Die exportierte Konfiguration enthält DNS-Server konfigurieren, virtual Network Settings Standortparameter LAN und anderen Einstellungen.

**Netzwerk-Import [Dateipfad]**

Importieren einer lokalen Netzwerk-Konfiguration.

**Netzwerk Dnsserver registrieren [Optionen] &lt;DnsIP >**

Registrieren Sie einen DNS-Server, den für die Auflösung von Namen in der Netzwerkkonfiguration verwenden möchten.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**Dnsserver Liste**

Listen Sie alle DNS-Server registriert in der Netzwerkkonfiguration.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**Netzwerk Dnsserver Registrierung [Optionen] &lt;DnsIP >**

Einen DNS-Server-Eintrag entfernt aus der Netzwerkkonfiguration.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

