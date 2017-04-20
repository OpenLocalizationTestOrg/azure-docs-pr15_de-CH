<properties
 pageTitle="Linux RDMA Cluster MPI-Programme ausführen | Microsoft Azure"
 description="Erstellen Sie einen Linux-Cluster Größe H16r, H16mr, A8 und A9 VMs mit Azure RDMA Netzwerk MPI-apps ausführen"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Einrichten eines Clusters Linux RDMA MPI-Anwendung ausführen


Informationen Sie zum Einrichten eines Clusters Linux RDMA in Azure mit [H-Serie oder rechenintensive eine Reihe VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md) parallel Message Passing Interface (MPI) ausgeführt. Dieser Artikel enthält eine schrittweise ein Linux HPC Bild Intel MPI auf einem Cluster ausgeführt. Anschließend stellen Sie einen Cluster von VMs mit diesem Bild und eines RDMA-fähig Azure VM-Größen (derzeit H16r, H16mr, A8 und A9). Mit dem Cluster der MPI-Programme ausführen, die über eine niedrige Latenz kommunizieren, hohen Durchsatz netzwerkbasierte remote direct Memory Access (RDMA)-Technologie.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Cluster-Bereitstellungsoptionen

Methoden zum Erstellen eines Clusters Linux RDMA mit oder ohne eine Objektaufrufplaner können folgen.


* **Azure-CLI-Skripte** - wie später in diesem Artikel verwenden der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) (CLI), die Bereitstellung eines Clusters RDMA-fähig VMs Skript. Die CLI im Service-Modus erstellt Clusterknoten nacheinander im klassischen Bereitstellungsmodell, so viele Compute-Knoten bereitstellen kann einige Minuten dauern. Um RDMA-Verbindung aktivieren bei Verwendung das klassischen Bereitstellungsmodell Bereitstellen virtueller Computer im selben Cloud-Dienst.

* **Azure-Ressourcen-Manager Vorlagen** – Sie können auch Ressourcen-Manager-Bereitstellungsmodell Cluster RDMA-fähig VMs bereitstellen, RDMA-Netzwerk verbindet. Sie können [eine eigene Vorlage erstellen](../resource-group-authoring-templates.md), oder die [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/) für Vorlagen von Microsoft oder der Gemeinschaft die Lösung bereitstellen soll beigetragen. Ressourcen-Manager Vorlagen bieten eine schnelle und zuverlässige Möglichkeit, einen Linux-Cluster bereitstellen. Um RDMA-Verbindung aktivieren bei Verwendung das Ressourcen-Manager-Bereitstellungsmodell, Bereitstellen virtueller Computer in der gleichen Verfügbarkeit.

* **HPC Pack** - erstellen ein Clusters Microsoft HPC Pack in Azure und fügen RDMA-fähig Compute-Knoten, die eine unterstützte Linux-Distribution für den Netzwerkzugriff RDMA ausgeführt. Finden Sie unter [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Beispiel Bereitstellungsschritte in klassisch

Die folgenden Schritte zeigen zum Verwenden der Azure-CLI erstellt ein benutzerdefiniertes VM-Image Anpassen und eine VM SUSE Linux Enterprise Server (SLES) 12 SP1 HPC Azure Marketplace bereitgestellt. Die Bereitstellung eines Clusters RDMA-fähig VMs Skript verwenden Sie das Bild dann. 

>[AZURE.TIP]Verwenden Sie ähnliche Schritte ein Clusters RDMA-fähig VMs basierend auf anderen unterstützten HPC Azure Marketplace bereitstellen. Einige Schritte können wie leicht abweichen. Beispielsweise wird Intel MPI enthalten und in diese Bilder konfiguriert. Und wenn eine SLES 12 HPC VM statt einer SLES 12 SP1 HPC VM bereitstellen, müssen RDMA Treiber aktualisieren. Näheres [über die A8 und A9, A10, A11 rechenintensive Instanzen](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Erforderliche Komponenten

*   **Client-Computer** - Sie benötigen einen Mac, Linux oder Windows-basierten Client Computer mit Azure kommunizieren. Diese Schritte setzen voraus, dass Sie einen Linux-Client verwenden.

*   **Azure-Abonnement** - Wenn Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) in wenigen Minuten erstellen. Größere Cluster sollten Sie Bedarfsbasis Abonnement oder andere Optionen. 

*   **VM Größe Verfügbarkeit** – derzeit die folgenden Instanz sind RDMA-fähig: H16r, H16mr, A8 und A9. Verfügbarkeit in Azure Regionen suchen Sie [Produkte nach Region](https://azure.microsoft.com/regions/services/) . 

*   **Kerne Quota** - eventuell erhöhen Sie das Kontingent der Kerne Cluster rechenintensive VMs bereitstellen. Beispielsweise benötigen Sie mindestens 128 Kerne 8 A9 VMs bereitstellen, wie in diesem Artikel. Ihr Abonnement kann auch die Anzahl der Kerne beschränken in bestimmten VM Größe Familien, einschließlich der H-Serie bereitgestellten. Anfordern einer Kontingent zu erhöhen, [Öffnen einer Anfrage](../azure-supportability/how-to-create-azure-support-request.md) , kostenlos. 

*   **Azure CLI** - der Azure-CLI[Installieren](../xplat-cli-install.md) und vom Client-Computer [Verbinden mit Azure-Abonnement](../xplat-cli-connect.md) .


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Schritt 1. Bereitstellen einer SLES 12 SP1 HPC-VM

Führen Sie nach der Anmeldung in Azure mit der Azure-CLI `azure config list` zu bestätigen, dass die Ausgabe Service-Modus zeigt. Ist es nicht, legen Sie den Modus durch Ausführen dieses Befehls:


    azure config mode asm


Geben Sie Folgendes ein, um alle Abonnements aufgelistet, die Sie verwenden dürfen:


    azure account list

Aktive Abonnements zugeordnet `Current` soll `true`. Dieses Abonnement die nicht Cluster erstellen möchten, legen Sie die entsprechende Abonnement-Id als aktives Abonnement:

    azure account set <subscription-Id>

Öffentlich verfügbaren SLES 12 SP1 HPC Bilder in Azure finden einen Befehl ähnlich dem folgenden ausführen, vorausgesetzt die Shell-Umgebung unterstützt **Grep**:


    azure vm image list | grep "suse.*hpc"

Jetzt bereitstellen Sie RDMA-fähig VM SLES 12 SP1 HPC-Bild mit einem Befehl ähnlich dem folgenden:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

wo

* Die Größe (in diesem Beispiel A9) ist der RDMA-fähig VM-Größen.

* Externe SSH-Portnummer (in diesem Beispiel SSH Standardeinstellung 22) ist eine beliebige gültige Anschlussnummer. 22 interne SSH-Portnummer fest.

* Neue Cloud-Dienst wird in Azure Region gemäß den Speicherort erstellt. Geben Sie einen Speicherort in dem VM-Größe wählen Sie verfügbar ist.

* SLES 12 SP1 Bildnamen derzeit kann `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` oder `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` für SUSE Priority-Support (zusätzliche Gebühr).

### <a name="step-2-customize-the-vm"></a>Schritt 2. Anpassen der VM

Die VM schließt Bereitstellung SSH VM der VMs externe IP-Adresse (oder DNS-Namen) und externe Port konfigurierten Nummer anpassen. Verbindungsdetails finden Sie unter [Anmelden bei einer virtuellen Linux ausgeführt](virtual-machines-linux-mac-create-ssh-keys.md). Befehle als Benutzer auf dem virtuellen Computer konfiguriert, wenn ein Schritt Root-Zugriff erforderlich ist.

>[AZURE.IMPORTANT]Microsoft Azure bietet keine Root-Zugriff auf Linux VMs. Administrative Verbindung als Benutzer der VM Zugriff führen Sie Befehle mit `sudo`.

* **Updates** - **Zypper**mit Updates installieren. Sie möchten auch NFS Dienstprogramme installieren. 

    >[AZURE.IMPORTANT]In einer SLES 12 SP1 HPC-VM empfiehlt Kernel-Updates anwenden nicht Probleme mit Linux RDMA Treiber verursachen können.

* **Intel MPI** - schließen Sie die Installation von Intel MPI auf SLES 12 SP1 HPC VM durch Ausführen des folgenden Befehls:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Sperren von Speicher** - Codes für MPI Sperren den verfügbaren Speicher für RDMA, hinzufügen oder ändern Folgendes in die Datei /etc/security/limits.conf. (Sie benötigen Root-Zugriff auf diese Datei.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Zu Testzwecken können Sie auch Memlock auf unbegrenzte festlegen. Beispiel: `<User or group name>    hard    memlock unlimited`. Weitere Informationen finden Sie unter [Best bekannte Methoden zur Einstellung gesperrt Speichergröße](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **SSH-Schlüssel für SLES VMs** - generieren SSH-Schlüssel zu vertrauen für Ihr Benutzerkonto unter den Compute-Knoten im Cluster SLES bei MPI-Jobs ausgeführt. (Wenn Sie VM HPC CentOS-basierte bereitgestellt, gehen Sie nicht. Siehe später in diesem Artikel ohne Passwort SSH vertrauen Clusterknoten nach Aufnahme und des Clusters bereitstellen eingerichtet.) 

    Führen Sie den folgenden Befehl SSH-Schlüssel erstellen. Wenn Sie zur Eingabe aufgefordert werden, drücken Sie die EINGABETASTE zum Generieren der Schlüssel am Standardspeicherort ohne ein Kennwort.

        ssh-keygen

    Den öffentlichen Schlüssel in der Datei Authorized_keys bekannten öffentlichen Schlüsseln anfügen.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    Bearbeiten Sie im Verzeichnis ~/.ssh oder erstellen Sie die Datei "Config". Geben Sie den IP-Adressbereich des privaten Netzwerks in Azure (in diesem Beispiel 10.32.0.0/16 soll):

        host 10.32.0.*
        StrictHostKeyChecking no

    Listen Sie private IP-Netzwerkadresse der jeder VM im Cluster wie folgt:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfiguration `StrictHostKeyChecking no` kann ein potenzielles Sicherheitsrisiko erstellen, wenn eine bestimmte IP-Adresse den Bereich oder.

* **Applications** - Programme auf diesem virtuellen Computer benötigen oder andere Anpassung durchzuführen, bevor die Aufnahme installieren.

### <a name="step-3-capture-the-image"></a>Schritt 3. Aufnahme

Um das Bild zu erfassen, führen Sie den folgenden Befehl zuerst im Linux VM. Dieser Befehl die VM Verwaltungsagents aber Benutzerkonten und SSH-Schlüssel, die Sie haben eingerichtet.

```
sudo waagent -deprovision
```

Führen Sie dann Ihre Client-Computer die folgenden Azure-CLI-Befehle auf. Einzelheiten finden Sie in [einem klassischen virtuellen Linux-Maschine als Bild erfassen](virtual-machines-linux-classic-capture-image.md) .  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nach dem Ausführen dieser Befehle VM-Image zu Ihrer Verwendung erfasst und die VM gelöscht. Haben Sie das benutzerdefinierte Abbild einen Cluster bereitstellen.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Schritt 4. Bereitstellen eines Clusters mit dem Bild

Folgende Skript mit entsprechenden Werten für Ihre Umgebung ändern und Client-Computer ausführen. Da Azure VMs seriell im klassischen Bereitstellungsmodell bereitstellt, dauert einige Minuten 8 A9 VMs vorgeschlagen in diesem Skript bereitstellen.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Aspekte einer CentOS HPC-cluster

Wenn Sie einen anhand eines HPC CentOS-basierte Bilder in Azure Marketplace SLES 12 HPC Cluster einrichten möchten, führen Sie allgemeine Schritte im vorherigen Abschnitt. Beachten Sie die folgenden Unterschiede beim Bereitstellen und die VM konfigurieren:

1. Intel MPI wird auf einem virtuellen Computer bereitgestellt von HPC CentOS-basierte Abbild installiert. 

2. Die VM /etc/security/limits.conf Datei sind bereits speichereinstellungen hinzugefügt.

2. Generieren Sie SSH-Schlüssel auf dem virtuellen Computer Sie bereitstellen für die Aufnahme nicht. Stattdessen empfehlen wir nach der Bereitstellung der Cluser Benutzer-Authentifizierung einrichten. Abschnitt.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Einrichten des Clusters ohne Passwort SSH-Vertrauensstellung

Auf einem CentOS-basierten HPC-Cluster gibt es zwei Methoden zum Einrichten von Vertrauensstellungen zwischen den Compute-Knoten: serverbasierte und benutzerbasierte Authentifizierung. Host-basierte Authentifizierung ist außerhalb des Bereichs dieses Artikels und im Allgemeinen erfolgt durch eine Erweiterung während der Bereitstellung. Benutzer-Authentifizierung ist für Vertrauensstellungen nach der Bereitstellung und erfordert die Erstellung und Freigabe von SSH-Schlüssel unter den Compute-Knoten im Cluster. Diese Methode wird ohne Passwort SSH Login genannt und ist erforderlich, wenn MPI-Jobs ausgeführt. 

Ein Beispielskript aus der Gemeinschaft beigetragen wird auf [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) einfache Authentifizierung CentOS-basierten HPC-Cluster aktivieren. Downloaden Sie und verwenden Sie dieses Skript mit den folgenden Schritten. Sie können auch dieses Skript ändern oder eine andere Methode ohne Passwort SSH-Authentifizierung zwischen den Compute-Knoten des Clusters zu verwenden.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Um das Skript auszuführen, müssen Sie das Präfix für die Subnetz-IP-Adressen zu kennen. Mit dem folgenden Befehl auf einem Clusterknoten erhalten Sie das Präfix. Die Ausgabe sollte in etwa 10.1.3.5 und das Präfix ist 10.1.3 Teil.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Nun führen Sie das Skript mit drei Parameter: den gemeinsamen Benutzernamen auf den Compute-Knoten, gemeinsames Kennwort für diesen Benutzer auf den Compute-Knoten und das Subnetzpräfix des vorherigen Befehls zurückgegeben wurde.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Dieses Skript führt Folgendes aus:

* Erstellt ein Verzeichnis auf dem Hostknoten mit dem Namen .ssh, die für die Anmeldung ohne Passwort erforderlich ist. 
* Erstellt eine Konfigurationsdatei im Verzeichnis .ssh, das ohne Passwort anmelden Anmeldung auf allen Knoten im Cluster zu weist. 
* Erstellt mit dem Knotennamen Knoten IP-Adressen für alle Knoten im Cluster. Diese Dateien verbleiben nach der Ausführung des Skripts für einen späteren. 
* Erstellt eine private und öffentliche Schlüsselpaar für jeden Clusterknoten einschließlich Hostknoten sowie Einträge in der Datei Authorized_keys.

>[AZURE.WARNING]Ausführen dieses Skripts können ein potenzielles Sicherheitsrisiko dar. Sicherstellen Sie, dass die Informationen des öffentliche Schlüssels in ~/.ssh nicht verteilt.


## <a name="configure-intel-mpi"></a>Intel MPI konfigurieren

Um auf Azure Linux RDMA MPI-Programme ausführen, müssen Sie bestimmte Umgebungsvariablen für Intel MPI. Hier ist eine Beispielskript Bash die Variablen zur Ausführung einer Anwendung konfigurieren. Ändern Sie den Pfad zum mpivars.sh für die Installation von Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Das Format der Host-Datei lautet wie folgt. Fügen Sie eine Zeile für jeden Knoten im Cluster. Geben Sie private IP-Adressen aus den VNet, DNS-Namen nicht definiert. Damit zwei Hosts IP-Adressen 10.32.0.1 und 10.32.0.2 enthält die Datei z. B. Folgendes:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI auf eine grundlegende Cluster mit zwei Knoten ausgeführt.

Wenn Sie dies nicht bereits getan haben zunächst Einrichten der Umgebung für Intel MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Führen Sie einen einfachen MPI-Befehl

Führen Sie einen einfachen MPI-Befehl auf einem Datenverarbeitungsknoten MPI ordnungsgemäß installiert und kann kommunizieren zwischen mindestens zwei Knoten berechnen aus. Der folgenden **Mpirun** -Befehl führt den Befehl **Hostname** auf beiden Knoten.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Die Ausgabe sollte Namensliste aller Knoten, die als Eingabe für übergeben `-hosts`. Beispielsweise gibt ein **Mpirun** -Befehl mit zwei Knoten Ausgabe ähnelt der folgenden:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Führen Sie eine MPI-benchmark

Intel MPI-Folgendes führt Pingpong Benchmark-Cluster-Konfiguration und Verbindung mit dem Netzwerk RDMA überprüfen.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Auf einem Cluster mit zwei Knoten sollte eine Ausgabe ähnlich der folgenden angezeigt werden. Im Netzwerk Azure RDMA erwarten Sie Wartezeit auf oder unter 3 Mikrosekunden Nachricht bis zu 512 Byte Größe.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Nächste Schritte

* Versuchen Sie bereitstellen und der MPI Linux Applikationen auf Linux Cluster ausgeführt.

* [Dokumentation der Intel MPI](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Anleitung Intel MPI finden.

* [Schnellstart-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) um ein Intel Glanz Cluster erstellen mit einer CentOS-basierten HPC versuchen. Details finden Sie in diesem [Blogbeitrag](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
