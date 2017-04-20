<properties
 pageTitle="Führen Sie OpenFOAM mit HPC Pack auf Linux VMs | Microsoft Azure"
 description="Einen Cluster Microsoft HPC Pack Azure bereitstellen und Ausführen eines Auftrags OpenFOAM mehrere Linux Compute-Knoten über ein Netzwerk RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>OpenFoam mit Microsoft HPC Pack auf einem Cluster Linux RDMA in Azure ausgeführt

Dieser Artikel zeigt eine Möglichkeit, OpenFoam in Azure virtuellen Maschinen ausführen. Hier stellen Sie einen Microsoft HPC Pack Cluster mit Linux Compute-Knoten auf Azure und Ausführen einer [OpenFoam](http://openfoam.com/) mit Intel MPI. RDMA-fähig Azure VMs können für den Compute-Knoten Netzwerk RDMA Azure Compute-Knoten kommunizieren. OpenFoam in Azure ausgeführt sind vollständig konfigurierten kommerziellen Bilder auf dem Markt wie UberClouds [OpenFoam 2,3 CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)und auf [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/)ausgeführt. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (für Feld-Vorgang öffnen und Bearbeitung) ist ein Open Source-computational Fluid Dynamics (CFD), die häufig in Wissenschaft kommerzielle und akademische Organisationen verwendet wird. Es enthält Tools zum vernetzen, insbesondere SnappyHexMesh, parallelisierte Netzgenerator komplexe CAD-Geometrien und vor- und Nachbearbeitung. Fast alle Prozesse laufen parallel Anwender zu vollständigen Computer-Hardware zur Verfügung.  

Microsoft HPC Pack bietet umfangreiche HPC und parallele Anwendungen MPI, in Clustern von Microsoft Azure virtuellen Maschinen ausführen. HPC Pack unterstützt auch laufende Linux HPC auf Linux Knoten berechnen VMs in einem Cluster HPC Pack bereitgestellt. Eine Einführung in Linux Compute-Knoten mit HPC Pack finden Sie unter [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) .

>[AZURE.NOTE] Dieser Artikel veranschaulicht, wie Linux MPI-Arbeitslast mit HPC Pack ausgeführt. Angenommen, dass Sie Linux Systemadministration und mit MPI Arbeitslasten in Linux-Clustern vertraut sein. Wenn MPI und von den in diesem Artikel gezeigten OpenFOAM Versionen verwenden, müssen Sie möglicherweise einige Schritte Installation und Konfiguration ändern. 

## <a name="prerequisites"></a>Erforderliche Komponenten

*   **HPC Pack RDMA-fähig Linux Cluster compute-Knoten** - Pack HPC-Cluster mit Größe A8, A9, H16r oder H16rm Linux Compute-Knoten mit einer [Azure PowerShell-Skript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)oder ein [Ressourcenmanager Azure Vorlage](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) bereitstellen. Weitere [Erste Schritte mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md) Komponenten und Schritte für beide Optionen. Bei Auswahl die Bereitstellungsoption PowerShell-Skript finden Sie unter Beispiel-Konfigurationsdatei in den Beispieldateien am Ende dieses Artikels. Verwenden Sie diese Konfiguration einen HPC Pack Azure-basierten Cluster mit Größe A8 Windows Server 2012 R2 Headknoten und 2 Größe A8 SUSE Linux Enterprise Server 12 Compute-Knoten bereitgestellt. Ersetzen durch die entsprechenden Werte für Ihre Abonnements und Service. 

    **Zusätzliche wichtige Informationen**

    *   Linux RDMA Netzwerk erforderliche Komponenten in Azure finden Sie unter [H-Serie und rechenintensiver eine Reihe VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Verwenden Sie die Powershell-Skript Bereitstellungsoption bereitstellen Sie aller Linux Computeknoten in einem Clouddienst RDMA-Verbindung verwenden.

    *   Verbinden Sie nach der Bereitstellung von Linux-Knoten SSH Weitere administrativen Aufgaben ausführen. Suchen Sie SSH-Verbindungsdetails für jeden Linux VM in Azure-Portal.  
        
*   Computeknoten in Azure **Intel MPI** - OpenFOAM auf SLES 12 HPC ausführen, müssen Sie die Intel MPI-Bibliothek 5 Laufzeit von [Intel.com Website](https://software.intel.com/en-us/intel-mpi-library/)installieren. (Intel MPI 5 CentOS-basierten HPC Bilder vorinstalliert ist.)  Installieren Sie in einem späteren Schritt ggf. Intel MPI auf Ihrem Linux Compute-Knoten. Zur Vorbereitung dieser Schritt Nachdem Sie bei Intel registriert den Link in der e-Mail auf der zugehörigen Webseite. Kopieren Sie den Downloadlink für .tgz-Datei für die entsprechende Version von Intel MPI. Dieser Artikel basiert auf Intel MPI Version 5.0.3.048.

*   **OpenFOAM Quelle Pack** - Download OpenFOAM Quelle Pack-Software für Linux [OpenFOAM Foundation-Website](http://openfoam.org/download/2-3-1-source/). Dieser Artikel basiert auf Quelle Pack Version 2.3.1 herunterladen als OpenFOAM 2.3.1.tgz. Führen Sie die Schritte in diesem Artikel entpacken und Kompilieren OpenFOAM Linux Compute-Knoten.

*   **EnSight** (optional) – die Ergebnisse der Simulation OpenFOAM, downloaden und installieren Sie das Programm [EnSight](https://www.ceisoftware.com/download/) , Visualisierung und Analyse. Lizenzierung und Downloads sind auf der Website EnSight.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Einrichten von Vertrauen zwischen Compute-Knoten

Ausführen eines Auftrags knotenübergreifende auf mehrere Linux-Nodes erfordert die Knoten (nach **Rsh** oder **ssh**) vertrauen. Beim Erstellen des Clusters HPC Pack mit Microsoft HPC Pack IaaS Bereitstellungsskript legt das Skript automatisch einrichten dauerhafte vertrauen für das Administratorkonto, das Sie angeben. Für nicht-Administratoren, die in die Domäne des Clusters erstellen, temporäre vertrauen zwischen den Knoten eingerichtet, wenn ein Auftrag zugewiesen wird und nach Abschluss des Auftrags Beziehung zerstört. Vertrauensstellungen für jeden Benutzer einrichten, geben Sie ein RSA-Schlüsselpaar zum Cluster HPC Pack für die Vertrauensstellung verwendet.

### <a name="generate-an-rsa-key-pair"></a>Ein RSA-Schlüsselpaar generieren

Es ist einfach, einen öffentlichen Schlüssel und einen privaten Schlüssel enthält, mit dem Befehl **ssh-Keygen** Linux ein RSA-Schlüsselpaar generieren.

1.  Melden Sie sich beim Linux-Computer.

2.  Führen Sie den folgenden Befehl ein:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Drücken Sie die **EINGABETASTE** , die Standardeinstellungen zu verwenden, bis der Befehl ausgeführt wird. Geben Sie ein Kennwort nicht; Drücken Sie nach einem Kennwort gefragt die **EINGABETASTE**.

    ![Ein RSA-Schlüsselpaar generieren][keygen]

3.  Wechseln Sie in das Verzeichnis ~/.ssh. Der private Schlüssel wird in Id_rsa und der öffentliche Schlüssel in id_rsa.pub gespeichert.

    ![Private und öffentliche Schlüssel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Das Schlüsselpaar mit dem HPC Pack Cluster hinzufügen
1.  Stellen Sie eine Remotedesktopverbindung zu dem Headknoten mit Ihrem Administratorkonto HPC Pack (das Administratorkonto eingerichtet werden, wenn Sie das Bereitstellungsskript ausgeführt haben).

2. Verwenden Sie Windows Server Standardverfahren zum Erstellen eines Domänenbenutzerkontos in Active Directory-Domäne des Clusters. Z. B. das Tool Active Directory-Benutzer und-Computer auf dem Head-Knoten. Die Beispiele in diesem Artikel angenommen, die Benutzer einer Domäne mit dem Namen Hpclab\hpcuser erstellen.

3.  Erstellen Sie eine Datei namens C:\cred.xml und kopieren Sie RSA Key Daten hinein. Eine Beispieldatei cred.xml wird am Ende dieses Artikels.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie den folgenden Befehl der Anmeldeinformationen für das Konto Hpclab\hpcuser festgelegt. Mithilfe den **Extendeddata** -Parameter übergeben den Namen der C:\cred.xml erstellten Datei für die Schlüsseldaten.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Dieser Befehl wird ohne Ausgabe erfolgreich abgeschlossen. Nach dem Festlegen der Anmeldeinformationen für die Benutzerkonten müssen Sie Aufträge ausgeführt werden, die cred.xml-Datei an einem sicheren Ort speichern oder löschen.

5.  Wird das RSA-Schlüsselpaar auf einem Linux-Knoten erstellt wird, sollten Sie Schlüssel zu löschen, klicken Sie nach der Verwendung. HPC Pack eine vorhandene Id_rsa oder id_rsa.pub findet, wird Sie vertrauen nicht festgelegt.

>[AZURE.IMPORTANT] Wir empfehlen nicht ausführen eines Auftrags Linux als Clusteradministrator auf einem freigegebenen Cluster da ein Auftrag übermittelt werden unter dem Root-Konto auf Linux-Knoten ausgeführt wird. Allerdings führt einen Auftrag nicht Administrator-Benutzer übermittelten unter einem lokalen Linux Benutzerkonto mit demselben Namen wie der Auftrag Benutzer. In diesem Fall richtet HPC Pack vertrauen für den Linux-Benutzer auf dem Projektbudget Knoten. Linuxbenutzer manuell auf den Linux-Knoten eingerichtet, Preis oder HPC Pack erstellt den Benutzer automatisch, wenn der Auftrag gesendet wird. HPC Pack erstellt den Benutzer löscht HPC Pack nach Abschluss des Auftrags. Um Sicherheitsrisiken zu verringern, entfernt HPC Pack Schlüssel nach Auftragsabschluss

## <a name="set-up-a-file-share-for-linux-nodes"></a>Richten Sie eine Dateifreigabe für Linux-Knoten

Nun richten Sie eine standardmäßige SMB-Freigabe auf einen Ordner auf dem Head-Knoten. Um Anwendungsdateien mit einem gemeinsamen Zugriff auf Linux-Knoten ermöglichen, Bereitstellen des freigegebenen Ordners auf Linux-Knoten. Wenn Sie möchten, können Sie eine andere Option wie ein Azure-Dateien - empfohlen für viele Szenarien- oder einer NFS-Freigabe für die Dateifreigabe. Finden Sie Informationen und detaillierte Schritte [beginnen mit Linux Compute-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Erstellen Sie einen Ordner auf dem Head-Knoten und mit Lese-und Schreibberechtigungen für alle Benutzer freigeben. Beispielsweise nutzen C:\OpenFOAM auf dem Head-Knoten als \\ \\SUSE12RDMA HN\OpenFOAM. Hier ist *SUSE12RDMA HN* Hostname des Head-Knotens.

2.  Öffnen Sie ein Windows PowerShell-Fenster und führen Sie die folgenden Befehle:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Der erste Befehl erstellt einen Ordner namens /openfoam auf allen Knoten in der LinuxNodes-Gruppe. Der zweite Befehl kann die freigegebenen Ordner //SUSE12RDMA-HN/OpenFOAM Linux-Nodes mit Dir_mode und File_mode-Bits auf 777 gesetzt. *Benutzername* und *Kennwort* im Befehl sollte die Anmeldeinformationen eines Benutzers auf dem Head-Knoten.

>[AZURE.NOTE]Die "\`" Symbol im zweiten Befehl ist ein Escapezeichen für PowerShell. "\`," bedeutet "," (Komma) ist ein Teil des Befehls.

## <a name="install-mpi-and-openfoam"></a>MPI und OpenFOAM installieren

Führen Sie OpenFOAM als MPI Job RDMA-Netzwerk müssen Sie OpenFOAM mit Intel MPI-Bibliotheken kompilieren. 

Führen Sie zunächst einige Befehle **Clusrun** Intel MPI-Bibliotheken installiert (sofern nicht bereits installiert) und OpenFOAM auf Linux-Knoten. Verwenden Sie Head-Knoten konfiguriert zuvor die Installation Dateien unter Linux-Knoten.

>[AZURE.IMPORTANT]Beispiele für diese Installation und Kompilieren Schritte. Sie benötigen Kenntnisse Linux Systemadministration von Compiler und Bibliotheken korrekt installiert sind. Sie müssen bestimmte Umgebungsvariablen oder andere Einstellungen für Ihre Versionen von Intel MPI und OpenFOAM ändern. Details finden Sie im [Intel MPI-Bibliothek für Linux-Installationshandbuch](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) und [OpenFOAM Quelle Pack-Installation](http://openfoam.org/download/2-3-1-source/) für Ihre Umgebung.


### <a name="install-intel-mpi"></a>Installieren von Intel MPI

Speichern des heruntergeladenen Installationspakets für Intel MPI (in diesem Beispiel l_mpi_p_5.0.3.048.tgz) in C:\OpenFoam auf dem Head-Knoten, Linux-Knoten /openfoam diese Datei zugreifen können. Führen Sie dann **Clusrun** Intel MPI-Bibliothek für die Linux-Knoten installieren.

1.  Die folgenden Befehle das Installationspaket kopieren und extrahieren Sie die Datei /opt/intel auf jedem Knoten.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Um die automatische Installation von Intel MPI-Bibliothek verwenden Sie eine silent.cfg. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels. Speichern Sie diese Datei im freigegebenen Ordner /openfoam. Details zur silent.cfg-Datei finden Sie unter [Intel MPI-Bibliothek für Linux-Installationshandbuch - Hintergrundinstallation](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Stellen Sie sicher, dass Sie Ihre silent.cfg speichern, als Textdatei mit Linux Zeilenenden (nur LF, nicht CR-LF). Dadurch wird sichergestellt, dass es korrekt auf der Linux-Knoten ausgeführt wird.

3.  Installieren Sie Intel MPI-Bibliothek im unbeaufsichtigten Modus.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>MPI konfigurieren

Zum Testen, sollten Sie /etc/security/limits.conf auf jedem Linux-Knoten die folgenden Zeilen hinzufügen:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Nach Aktualisierung die Datei limits.conf Linux-Knoten neu. Z. B. **Clusrun** Folgendes:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Nach dem Neustart sicher, dass der freigegebene Ordner als /openfoam bereitgestellt.

### <a name="compile-and-install-openfoam"></a>Kompilieren und installieren OpenFOAM

Speichern des heruntergeladenen Installationspakets OpenFOAM Quelle Pack (OpenFOAM-2.3.1.tgz in diesem Beispiel), C:\OpenFoam auf dem Head-Knoten Knoten Linux /openfoam diese Datei zugreifen können. Dann führen Sie **Clusrun** -Befehle OpenFOAM auf den Linux-Knoten kompiliert aus.


1.  Ordner /opt/OpenFOAM auf jedem Linux-Knoten erstellen, das Quellpaket in diesen Ordner kopieren und es extrahieren.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Kompilieren Sie OpenFOAM mit Intel MPI-Bibliothek zuerst richten Sie einige Umgebungsvariablen für Intel MPI und OpenFOAM. Verwenden Sie die Variablen eine Bash-Skript namens settings.sh. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels. Setzen Sie diese Datei (mit Linux Zeilenenden) im freigegebenen Ordner /openfoam. Diese Datei enthält auch Einstellungen für MPI und OpenFOAM Laufzeiten, die Sie später zum Ausführen eines Auftrags OpenFOAM.

3. Installieren von Paketen zum Kompilieren von OpenFOAM. Je nach Linux-Distribution müssen Sie zuerst ein Repository hinzufügen. **Clusrun** Befehle wie die folgenden ausführen:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Falls erforderlich, SSH auf jedem Linux-Knoten auszuführende Befehle zu bestätigen, dass sie ordnungsgemäß ausgeführt.

4.  Führen Sie den folgenden Befehl OpenFOAM kompiliert. Die Kompilierung dauert einige Zeit und erzeugt eine große Menge von Informationen an die Standardausgabe, so mithilfe der Option **/ Überlappend** überlappend ausgeben.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]Die "\`" Symbol im Befehl ist ein Escapezeichen für PowerShell. "\`&" bedeutet das "&" ist ein Teil des Befehls.

## <a name="prepare-to-run-an-openfoam-job"></a>Vorbereiten der Ausführung eines Auftrags OpenFOAM

Jetzt einen MPI-Job namens sloshingTank3D ein OpenFoam ist auf zwei Linux-Knoten ausgeführt. 

### <a name="set-up-the-runtime-environment"></a>Die Runtime-Umgebung einrichten

Um die Common Language Runtime-Umgebung für MPI und OpenFOAM auf Linux-Knoten einzurichten, führen Sie den folgenden Befehl in einem Windows PowerShell-Fenster auf dem Head-Knoten. (Dieser Befehl ist für SUSE Linux nur gültig.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten

Verwenden Sie Head-Knoten, die bereits Dateien unter Linux Knoten (als /openfoam geladen) konfiguriert.

1.  SSH zu Ihrem Linux compute-Knoten.

2.  Führen Sie den folgenden Befehl OpenFOAM Runtime-Umgebung einrichten, wenn Sie dies bereits getan haben.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Kopieren Sie die sloshingTank3D Probe auf den freigegebenen Ordner und navigieren.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Bei Verwendung der Standardparameter dieses Beispiels dauert es 10 Minuten, so Sie einige Parameter zu beschleunigen möchten vielleicht. Eine einfache Auswahl ist Zeit Schritt Variablen DeltaT und WriteInterval in der Datei System-ControlDict ändern. Diese Datei speichert alle Eingabedaten für das Steuerelement lesen und Schreiben von Lösungsdaten. Sie können beispielsweise den Wert des DeltaT von 0,05 0,5 und der Wert des WriteInterval von 0,05 0,5 ändern.

    ![Schritt ändern][step_variables]

5.  Gewünschten Werte für die Variablen in der System-DecomposeParDict-Datei angeben. Dabei wird jeder zwei Linux-Knoten mit 8 Kernen, so NumberOfSubdomains auf 16 und n HierarchicalCoeffs auf (1 1 16), d. h. OpenFOAM mit 16 parallel ausgeführt. Weitere Informationen finden Sie unter [OpenFOAM Benutzerhandbuch: 3.4 ausgeführt Applications parallele](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Zerlegen Sie Prozesse][decompose]

6.  Führen Sie die folgenden Befehle aus dem Verzeichnis sloshingTank3D vorbereiten die Beispieldaten.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Auf dem Head-Knoten sollten Sie sehen, dass die Beispieldateien in C:\OpenFoam\sloshingTank3D kopiert werden. (C:\OpenFoam ist der freigegebene Ordner auf dem Head-Knoten).

    ![Datendateien auf dem Head-Knoten][data_files]

### <a name="host-file-for-mpirun"></a>Hostdatei mpirun

In diesem Schritt erstellen Sie eine Hostdatei (eine Liste von Compute-Knoten) der **Mpirun** -Befehl verwendet.

1.  Erstellen Sie auf einem Linux-Knoten Datei Domänenamen unter /openfoam, damit diese Datei am /openfoam/hostfile auf Linux-Knoten erreicht werden kann.

2.  Schreiben Sie die Linux-Knotennamen in diese Datei. In diesem Beispiel enthält die Datei die folgenden Namen:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Sie können diese Datei auch an C:\OpenFoam\hostfile auf dem Head-Knoten erstellen. Wenn Sie diese Option auswählen, speichern Sie sie als Textdatei mit Linux Zeilenenden Sie (nur LF, nicht CR-LF). Dies gewährleistet, dass es korrekt auf der Linux-Knoten ausgeführt wird.

    **Skriptwrapper bash**

    Wenn mehrere Linux-Knoten und Ihr Auftrag nur einige ausgeführt werden soll, empfiehlt es keine feste Host-Datei, da Sie nicht wissen, welche Knoten Ihrer Arbeit zugeteilt werden. In diesem Fall schreiben Sie einen Bash Skriptwrapper für **Mpirun** Host-Datei automatisch erstellt. Sie können einen Beispiel Bash Skriptwrapper aufgerufen hpcimpirun.sh am Ende dieses Artikels finden und als /openfoam/hpcimpirun.sh speichern. Dieses Beispiel führt Folgendes aus:

    1.  Richtet die Umgebungsvariablen **Mpirun**und einige Parameter hinzufügen zum Ausführen des Auftrags MPI RDMA-Netzwerk. In diesem Fall wird die folgenden Variablen:

        *   I_MPI_FABRICS = Shm:dapl
        *   I_MPI_DAPL_PROVIDER = Ofa-v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Erstellt eine Hostdatei nach der Umgebung Variable $CCP_NODES_CORES, die von der HPC-Head-Knoten festgelegt wird, wenn der aktiviert ist.

        Das Format der $CCP_NODES_CORES folgt diesem Muster:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        wo

        * `<Number of nodes>`-die Anzahl der Knoten, die diesem Auftrag zugewiesen.  
        
        * `<Name of node_n_...>`-der Name der einzelnen Knoten, die diesem Auftrag zugewiesen.
        
        * `<Cores of node_n_...>`-die Anzahl der Kerne auf dem Knoten, die diesem Auftrag zugewiesen.

        Beispielsweise sollte das Projekt zwei Knoten ausführen, ähnelt $CCP_NODES_CORES
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Ruft den **Mpirun** -Befehl und zwei Parameter an die Befehlszeile angefügt.

        * `--hostfile <hostfilepath>: <hostfilepath>`-der Pfad der Datei Hosts das Skript erstellt

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-Festlegen der HPC Pack Headknoten, die die Anzahl der insgesamt Kerne, die diesem Auftrag zugeordnet ist. In diesem Fall gibt die Anzahl der Prozesse **Mpirun**.


## <a name="submit-an-openfoam-job"></a>Senden eines Auftrags OpenFOAM

Jetzt können Sie einen Auftrag im HPC Cluster Manager senden. Sie müssen das Skript hpcimpirun.sh in den Befehlszeilen für einige der Aufgaben übergeben.

1. Verbindung mit der Cluster-Head-Knoten und HPC Cluster Manager starten.

2. **Im Ressourcenmanagement**sind Linux Compute-Knoten in den **Online** -Zustand. Nicht wählen sie und klicken Sie auf **Online schalten**.

3.  **Projektmanagement**klicken Sie auf **Neues Projekt**.

4.  Geben Sie z. B. _sloshingTank3D_.

    ![Job-details][job_details]

5.  Wählen Sie die Ressource als "Knoten" **Projekt Ressourcen**und mindestens auf 2 festgelegt. Diese Konfiguration führt den Auftrag auf zwei Linux-Knoten, die jeweils acht Kernen in diesem Beispiel hat.

    ![Job-Ressourcen][job_resources]

6. Klicken Sie im linken Navigationsbereich auf **Aufgaben bearbeiten** , und klicken Sie dann auf **Hinzufügen** , um dem Projekt eine Aufgabe hinzuzufügen. Der Auftrag mit den folgenden Befehlszeilen und vier Aufgaben hinzufügen.

    >[AZURE.NOTE]Unter `source /openfoam/settings.sh` Laufzeitumgebungen OpenFOAM und MPI richtet, die folgenden Aufgaben vor dem OpenFOAM-Befehl aufgerufen.

    *   **Aufgabe 1**. Führen Sie **DecomposePar** zum Generieren von Daten für **InterDyMFoam** parallel ausgeführt.
    
        *   Zuordnen eines Knotens

        *   **Befehlszeile** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Arbeitsverzeichnis** - sloshingTank3D/openfoam
        
        Abbildung. Die verbleibenden Vorgänge werden entsprechend konfigurieren.

        ![Aufgabe 1-details][task_details1]

    *   **Aufgabe 2**. **InterDyMFoam** im Beispiel berechnet parallel ausgeführt.

        *   Zwei Knoten zuordnen

        *   **Befehlszeile** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Arbeitsverzeichnis** - sloshingTank3D/openfoam

    *   **Aufgabe 3**. Führen Sie **ReconstructPar** um die Zeit Verzeichnisse von jedem Verzeichnis Processor_N_ in einem Satz zusammenzuführen.

        *   Zuordnen eines Knotens

        *   **Befehlszeile** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Arbeitsverzeichnis** - sloshingTank3D/openfoam

    *   **Aufgabe 4**. **FoamToEnsight** OpenFOAM Ergebnisdateien in EnSight Format konvertieren und EnSight-Dateien in ein Verzeichnis namens Ensight im Fall Verzeichnis parallel ausgeführt.

        *   Zwei Knoten zuordnen

        *   **Befehlszeile** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Arbeitsverzeichnis** - sloshingTank3D/openfoam

6.  Diese Aufgaben in aufsteigender Reihenfolge der Vorgänge fügen Sie Abhängigkeiten hinzu.

    ![Verzögert][task_dependencies]

7.  Klicken Sie auf **Senden** , um diesen Auftrag ausführen.

    Standardmäßig sendet HPC Pack den Auftrag als das aktuelle angemeldete Benutzerkonto. Nachdem Sie auf **Absenden**klicken, sehen Sie ein Dialogfeld aufgefordert, den Benutzernamen und das Kennwort einzugeben.

    ![Job-Anmeldeinformationen][creds]

    Unter gewissen Umständen speichert HPC Pack Benutzerinformationen vor Eingabe und dieses Dialogfeld nicht anzeigen. Um HPC Pack wieder anzuzeigen, geben Sie folgenden Befehl an der Befehlszeile und senden Sie den Auftrag.

    ```
    hpccred delcreds
    ```

8.  Der Auftrag dauert 10 Minuten bis mehrere Stunden das Beispiel festgelegten Parametern. In der heatmap auf den Linux-Knoten ausgeführte Auftrag angezeigt. 

    ![Wärmekarte][heat_map]

    Auf jedem Knoten werden acht Prozesse gestartet.

    ![Linux-Prozessen][linux_processes]

9.  Wenn der Auftrag abgeschlossen ist, finden Sie die Auftragsergebnisse im Ordner C:\OpenFoam\sloshingTank3D und Protokolldateien auf C:\OpenFoam.


## <a name="view-results-in-ensight"></a>Anzeigen der Ergebnisse in EnSight

Optional mit der [EnSight](https://www.ceisoftware.com/) visualisieren und analysieren Sie die Ergebnisse des Auftrags OpenFOAM. Weitere Informationen Visualisierung und Animation im EnSight finden Sie in diesem [video Guide](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Nach der Installation von EnSight auf dem Head-Knoten starten.

2.  Öffnen Sie C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Einen Tank im Viewer angezeigt.

    ![Tank EnSight][tank]

3.  Erstellen Sie ein **Isofläche** aus **InternalMesh**und dann die Variable **Alpha_water**.

    ![Isofläche erstellen][isosurface]

4.  Legen Sie die Farbe für **Isosurface_part** im vorherigen Schritt erstellt haben. Beispielsweise legen sie auf Blau.

    ![Isofläche Farbe bearbeiten][isosurface_color]

5.  Erstellen Sie einer **Iso-Volume** **aus** **Wände** im Bedienfeld " **Komponenten** " auswählen, und klicken Sie auf der Symbolleiste **Isoflächen** .

6.  Klicken Sie im Dialogfeld **Wählen Sie als **Isovolume** ** und Min **Isovolume Bereich** auf 0,5 festgelegt. Um die Isovolume zu erstellen, klicken Sie auf **mit ausgewählten Teilen erstellen**.

7.  Legen Sie die Farbe für **Iso_volume_part** im vorherigen Schritt erstellt haben. Beispielsweise soll es Wasser Blau.

8.  Legen Sie die Farbe für **Wände**. Beispielsweise auf transparent weiß festgelegt.

9. Klicken Sie nun auf **Wiedergeben** , um die Ergebnisse der Simulation.

    ![Tank-Ergebnis][tank_result]

## <a name="sample-files"></a>Beispieldateien

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>XML-Beispielkonfigurationsdatei für Bereitstellung von PowerShell-Skript

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Cred.xml-Beispieldatei

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Silent.cfg-Beispieldatei MPI installieren

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Beispielskript für settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Beispielskript für hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
